---
title: ".NET-alkalmazások Application Insights pillanatkép hibakereső aaaAzure |} Microsoft Docs"
description: "Debug pillanatképek vannak automatikusan gyűjti a program kivételek éles .NET-alkalmazásokban"
services: application-insights
documentationcenter: 
author: qubitron
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: bwren
ms.openlocfilehash: f0173a752b5795d934fbab1bd53eb077433edc90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a><span data-ttu-id="9c2a7-103">A .NET-alkalmazásokban kivételek pillanatképek hibakeresése</span><span class="sxs-lookup"><span data-stu-id="9c2a7-103">Debug snapshots on exceptions in .NET apps</span></span>

<span data-ttu-id="9c2a7-104">Ha kivétel lép, a működés közbeni webes alkalmazás automatikusan begyűjtheti a hibakeresési pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-104">When an exception occurs, you can automatically collect a debug snapshot from your live web application.</span></span> <span data-ttu-id="9c2a7-105">hello pillanatkép forráskód hello állapotát jeleníti meg, és változók: hello jelenleg hello kivétel lépett fel.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-105">hello snapshot shows hello state of source code and variables at hello moment hello exception was thrown.</span></span> <span data-ttu-id="9c2a7-106">hello pillanatkép hibakereső (előzetes verzió) a [Azure Application Insights](app-insights-overview.md) kivételtelemetria a webes alkalmazás figyeli.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-106">hello Snapshot Debugger (preview) in [Azure Application Insights](app-insights-overview.md) monitors exception telemetry from your web app.</span></span> <span data-ttu-id="9c2a7-107">Pillanatképek a felső értesítő kivételek a összegyűjti az, hogy éles környezetben toodiagnose problémák szükséges hello információkat.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-107">It collects snapshots on your top-throwing exceptions so that you have hello information you need toodiagnose issues in production.</span></span> <span data-ttu-id="9c2a7-108">Hello tartalmaznak [pillanatkép adatgyűjtő NuGet-csomag](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) az alkalmazásban, és szükség esetén adja meg a gyűjtemény paramétereit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). A pillanatképek jelennek meg [kivételek](app-insights-asp-net-exceptions.md) hello Application Insights portálon.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-108">Include hello [Snapshot collector NuGet package](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in your application, and optionally configure collection parameters in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snapshots appear on [exceptions](app-insights-asp-net-exceptions.md) in hello Application Insights portal.</span></span>

<span data-ttu-id="9c2a7-109">A portál toosee hello hívásveremben hello hibakeresési pillanatképek tekintheti meg és vizsgálja meg az egyes Hívásiverem-keret változókat.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-109">You can view debug snapshots in hello portal toosee hello call stack and inspect variables at each call stack frame.</span></span> <span data-ttu-id="9c2a7-110">a forráskódot, a Visual Studio 2017 vállalati által megnyitott pillanatképek hatékonyabb hibakeresési élmény tooget [hello pillanatkép hibakereső bővítmény letöltése a Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="9c2a7-110">tooget a more powerful debugging experience with source code, open snapshots with Visual Studio 2017 Enterprise by [downloading hello Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

<span data-ttu-id="9c2a7-111">Pillanatkép gyűjtemény érhető el:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-111">Snapshot collection is available for:</span></span>
* <span data-ttu-id="9c2a7-112">.NET-keretrendszer és az ASP.NET alkalmazások futó .NET-keretrendszer 4.5-ös vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-112">.NET Framework and ASP.NET applications running .NET Framework 4.5 or later.</span></span>
* <span data-ttu-id="9c2a7-113">Windows rendszeren futó alkalmazások .NET core 2.0 és 2.0-s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-113">.NET Core 2.0 and ASP.NET Core 2.0 applications running on Windows.</span></span>

### <a name="configure-snapshot-collection-for-aspnet-applications"></a><span data-ttu-id="9c2a7-114">Pillanatkép gyűjtemény az ASP.NET-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c2a7-114">Configure snapshot collection for ASP.NET applications</span></span>

1. <span data-ttu-id="9c2a7-115">[Az Application Insights engedélyezéséhez a web app alkalmazásban](app-insights-asp-net.md), ha még nincs kész.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-115">[Enable Application Insights in your web app](app-insights-asp-net.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="9c2a7-116">Hello tartalmaznak [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-116">Include hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span> 

3. <span data-ttu-id="9c2a7-117">Tekintse át a csomag túl hozzáadott hello hello alapértelmezett beállításokat[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="9c2a7-117">Review hello default options that hello package added too[ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- hello default is true, but you can disable Snapshot Debugging by setting it toofalse -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this tootrue. -->
        <!-- DeveloperMode is a property on hello active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need toosee an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- hello maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- hello maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often tooreset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- hello maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- hello maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. <span data-ttu-id="9c2a7-118">A pillanatképek összegyűjtése csak a kivételek, amelyek jelentett tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-118">Snapshots are collected only on exceptions that are reported tooApplication Insights.</span></span> <span data-ttu-id="9c2a7-119">Bizonyos esetekben (például hello .NET platform régebbi verzióknál), akkor módosítania kell túl[kivétel gyűjtemény konfigurálása](app-insights-asp-net-exceptions.md#exceptions) toosee kivételek hello portálon pillanatfelvételekkel.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-119">In some cases (for example, older versions of hello .NET platform), you might need too[configure exception collection](app-insights-asp-net-exceptions.md#exceptions) toosee exceptions with snapshots in hello portal.</span></span>


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a><span data-ttu-id="9c2a7-120">Az ASP.NET Core 2.0 alkalmazások pillanatkép-gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c2a7-120">Configure snapshot collection for ASP.NET Core 2.0 applications</span></span>

1. <span data-ttu-id="9c2a7-121">[Az Application Insights engedélyezése az ASP.NET Core web app alkalmazásban](app-insights-asp-net-core.md), ha még nincs kész.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-121">[Enable Application Insights in your ASP.NET Core web app](app-insights-asp-net-core.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="9c2a7-122">Hello tartalmaznak [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-122">Include hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="9c2a7-123">Módosítsa a hello `ConfigureServices` módszer az alkalmazás `Startup` tooadd hello pillanatkép adatgyűjtő telemetriai processzor osztály.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-123">Modify hello `ConfigureServices` method in your application's `Startup` class tooadd hello Snapshot Collector's telemetry processor.</span></span> <span data-ttu-id="9c2a7-124">hello kódot, hozzá kell adnia a hello hivatkozott hello Microsoft.ApplicationInsights.ASPNETCore NuGet-csomag verziójának függ.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-124">hello code you should add depends on hello referenced version of hello Microsoft.ApplicationInsights.ASPNETCore NuGet package.</span></span>

   <span data-ttu-id="9c2a7-125">Microsoft.ApplicationInsights.AspNetCore 2.1.0 hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-125">For Microsoft.ApplicationInsights.AspNetCore 2.1.0 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   <span data-ttu-id="9c2a7-126">Microsoft.ApplicationInsights.AspNetCore 2.1.1 hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-126">For Microsoft.ApplicationInsights.AspNetCore 2.1.1 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           public ITelemetryProcessor Create(ITelemetryProcessor next) =>
               new SnapshotCollectorTelemetryProcessor(next);
       }

       // This method is called by hello runtime. Use it tooadd services toohello container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a><span data-ttu-id="9c2a7-127">Pillanatkép gyűjtemény más .NET-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c2a7-127">Configure snapshot collection for other .NET applications</span></span>

1. <span data-ttu-id="9c2a7-128">Ha az alkalmazás már nem tagolva az Application insights szolgáltatással, Kezdésként [engedélyezése az Application Insights és beállítási hello instrumentation kulcs](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="9c2a7-128">If your application is not already instrumented with Application Insights, get started by [enabling Application Insights and setting hello instrumentation key](app-insights-windows-desktop.md).</span></span>

2. <span data-ttu-id="9c2a7-129">Adja hozzá a hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-129">Add hello [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="9c2a7-130">A pillanatképek összegyűjtése csak a kivételek, amelyek jelentett tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-130">Snapshots are collected only on exceptions that are reported tooApplication Insights.</span></span> <span data-ttu-id="9c2a7-131">Szükség lehet toomodify a kód tooreport őket.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-131">You may need toomodify your code tooreport them.</span></span> <span data-ttu-id="9c2a7-132">hello kivételkezelő kód hello struktúra az alkalmazás függ, de például nem éri el:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-132">hello exception handling code depends on hello structure of your application, but an example is below:</span></span>
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle hello request.
        }
        catch (Exception ex)
        {
            // Report hello exception tooApplication Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow hello exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a><span data-ttu-id="9c2a7-133">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="9c2a7-133">Grant permissions</span></span>

<span data-ttu-id="9c2a7-134">Azure-előfizetés hello tulajdonosainak pillanatképek vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-134">Owners of hello Azure subscription can inspect snapshots.</span></span> <span data-ttu-id="9c2a7-135">Más felhasználók tulajdonos engedéllyel kell rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-135">Other users must be granted permission by an owner.</span></span>

<span data-ttu-id="9c2a7-136">toogrant engedély hozzárendelése hello `Application Insights Snapshot Debugger` szerepkör toousers, akik vizsgálja a pillanatképeket.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-136">toogrant permission, assign hello `Application Insights Snapshot Debugger` role toousers who will inspect snapshots.</span></span> <span data-ttu-id="9c2a7-137">Ehhez a szerepkörhöz is hozzárendelhető, tooindividual felhasználók vagy csoportok által előfizetésnél tulajdonos hello cél Application Insights-erőforrást, vagy az erőforráscsoportba vagy előfizetésbe.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-137">This role can be assigned tooindividual users or groups by subscription owners for hello target Application Insights resource or its resource group or subscription.</span></span>

1. <span data-ttu-id="9c2a7-138">Hello hozzáférés-vezérlés (IAM) panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-138">Open hello Access Control (IAM) blade.</span></span>
1. <span data-ttu-id="9c2a7-139">Kattintson a hello + Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-139">Click hello +Add button.</span></span>
1. <span data-ttu-id="9c2a7-140">Válassza ki az Application Insights pillanatkép hibakereső hello szerepkörök legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-140">Select Application Insights Snapshot Debugger from hello Roles drop-down list.</span></span>
1. <span data-ttu-id="9c2a7-141">Keresse meg, és adjon meg egy nevet hello felhasználói tooadd.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-141">Search for and enter a name for hello user tooadd.</span></span>
1. <span data-ttu-id="9c2a7-142">Kattintson a hello Mentés gombra tooadd hello toohello szerepkörben.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-142">Click hello Save button tooadd hello user toohello role.</span></span>


[!IMPORTANT]
    <span data-ttu-id="9c2a7-143">A pillanatképek potenciálisan tartalmazó változó és a paraméter értékét a személyes és más bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-143">Snapshots can potentially contain personal and other sensitive information in variable and parameter values.</span></span>

## <a name="debug-snapshots-in-hello-application-insights-portal"></a><span data-ttu-id="9c2a7-144">Pillanatképek hello Application Insights portál hibakeresési</span><span class="sxs-lookup"><span data-stu-id="9c2a7-144">Debug snapshots in hello Application Insights portal</span></span>

<span data-ttu-id="9c2a7-145">Pillanatkép érhető el egy adott kivétel vagy probléma azonosítója, ha egy **Debug pillanatfelvétel megnyitása** hello gomb [kivétel](app-insights-asp-net-exceptions.md) hello Application Insights portálon.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-145">If a snapshot is available for a given exception or a problem ID, an **Open Debug Snapshot** button appears on hello [exception](app-insights-asp-net-exceptions.md) in hello Application Insights portal.</span></span>

![Nyissa meg Debug pillanatkép gombra a kivételei](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

<span data-ttu-id="9c2a7-147">Hello pillanatkép hibakeresési nézet láthatja, a hívási verem és a változók ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-147">In hello Debug Snapshot view, you see a call stack and a variables pane.</span></span> <span data-ttu-id="9c2a7-148">Keretek hello hívás a verem hello hívási verem ablaktáblán, megtekintheti a helyi változókat és paraméterek függvény hívása hello változók ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-148">When you select frames of hello call stack in hello call stack pane, you can view local variables and parameters for that function call in hello variables pane.</span></span>

![Nézet Debug pillanatkép hello portálon](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

<span data-ttu-id="9c2a7-150">A pillanatképek kényes információt is tartalmazhat, és alapértelmezés szerint nincsenek megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-150">Snapshots might contain sensitive information, and by default they are not viewable.</span></span> <span data-ttu-id="9c2a7-151">tooview pillanatképeket, rendelkeznie kell hello `Application Insights Snapshot Debugger` szerepkörrel tooyou.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-151">tooview snapshots, you must have hello `Application Insights Snapshot Debugger` role assigned tooyou.</span></span>

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a><span data-ttu-id="9c2a7-152">A Visual Studio 2017 vállalati pillanatképek hibakeresése</span><span class="sxs-lookup"><span data-stu-id="9c2a7-152">Debug snapshots with Visual Studio 2017 Enterprise</span></span>
1. <span data-ttu-id="9c2a7-153">Hello kattintson **letöltése pillanatkép** gomb toodownload egy `.diagsession` fájlt, amely a Visual Studio 2017 Enterprise nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-153">Click hello **Download Snapshot** button toodownload a `.diagsession` file, which can be opened by Visual Studio 2017 Enterprise.</span></span> 

2. <span data-ttu-id="9c2a7-154">tooopen hello `.diagsession` fájl, először [töltse le és telepítse a hello pillanatkép hibakereső bővítményt a Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="9c2a7-154">tooopen hello `.diagsession` file, you must first [download and install hello Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

3. <span data-ttu-id="9c2a7-155">Hello pillanatfelvétel-fájl megnyitása után hello kis memóriakép Hibakeresés lap a Visual Studio jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-155">After you open hello snapshot file, hello Minidump Debugging page in Visual Studio appears.</span></span> <span data-ttu-id="9c2a7-156">Kattintson a **felügyelt kód Debug** toostart hello pillanatkép hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-156">Click **Debug Managed Code** toostart debugging hello snapshot.</span></span> <span data-ttu-id="9c2a7-157">hello pillanatkép megnyitja toohello kódsort ahol hello kivétel lépett fel, hogy a hibakeresést tud hello folyamat hello aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-157">hello snapshot opens toohello line of code where hello exception was thrown so that you can debug hello current state of hello process.</span></span>

    ![A Visual Studio nézet hibakeresési pillanatkép](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

<span data-ttu-id="9c2a7-159">hello letöltött pillanatkép bármely alkalmazás webkiszolgálón található szimbólum fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-159">hello downloaded snapshot contains any symbol files that were found on your web application server.</span></span> <span data-ttu-id="9c2a7-160">A szimbólum fájlok szükséges tooassociate pillanatkép-adatok a forráskódot.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-160">These symbol files are required tooassociate snapshot data with source code.</span></span> <span data-ttu-id="9c2a7-161">App Service-alkalmazásokhoz győződjön meg arról, hogy tooenable szimbólum központi telepítés, a webalkalmazások közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-161">For App Service apps, make sure tooenable symbol deployment when you publish your web apps.</span></span>

## <a name="how-snapshots-work"></a><span data-ttu-id="9c2a7-162">A pillanatképek működése</span><span class="sxs-lookup"><span data-stu-id="9c2a7-162">How snapshots work</span></span>

<span data-ttu-id="9c2a7-163">Az alkalmazás indításakor egy külön pillanatkép feltöltése folyamat jön létre, amely az alkalmazás a pillanatkép-kéréseket figyeli.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-163">When your application starts, a separate snapshot uploader process is created that monitors your application for snapshot requests.</span></span> <span data-ttu-id="9c2a7-164">Ha pillanatkép van szükség, folyamatának futtatása hello árnyékmásolatát too20 körülbelül 10 perc múlva történik.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-164">When a snapshot is requested, a shadow copy of hello running process is made in about 10 too20 minutes.</span></span> <span data-ttu-id="9c2a7-165">hello árnyékmásolat folyamat majd elemzésnek, és egy pillanatkép jön létre, miközben hello fő folyamat tovább toorun és ki tudja szolgálni forgalom toousers.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-165">hello shadow process is then analyzed, and a snapshot is created while hello main process continues toorun and serve traffic toousers.</span></span> <span data-ttu-id="9c2a7-166">hello pillanatkép nem feltöltött tooApplication Insights, valamint bármely megfelelő szimbólum (.pdb) fájlok, amelyek szükséges tooview hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-166">hello snapshot is then uploaded tooApplication Insights along with any relevant symbol (.pdb) files that are needed tooview hello snapshot.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="9c2a7-167">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="9c2a7-167">Current limitations</span></span>

### <a name="publish-symbols"></a><span data-ttu-id="9c2a7-168">Szimbólumok közzététele</span><span class="sxs-lookup"><span data-stu-id="9c2a7-168">Publish symbols</span></span>
<span data-ttu-id="9c2a7-169">hello pillanatkép hibakereső szimbólumfájlok hello üzemi kiszolgáló toodecode változók és a Visual Studio hibakeresési élményt tooprovide igényel.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-169">hello Snapshot Debugger requires symbol files on hello production server toodecode variables and tooprovide a debugging experience in Visual Studio.</span></span> <span data-ttu-id="9c2a7-170">Visual Studio 2017 hello 15.2 kiadásának kibocsátási buildek szimbólumait alapértelmezés szerint közzéteszi tooApp szolgáltatás teszi közzé.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-170">hello 15.2 release of Visual Studio 2017 publishes symbols for release builds by default when it publishes tooApp Service.</span></span> <span data-ttu-id="9c2a7-171">A korábbi verziókban tooadd hello következőkre lesz szüksége a közzétételi profil sor tooyour `.pubxml` fájlt úgy, hogy a szimbólumok közzétett kiadási módban:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-171">In prior versions, you need tooadd hello following line tooyour publish profile `.pubxml` file so that symbols are published in release mode:</span></span>

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

<span data-ttu-id="9c2a7-172">Az Azure számítási és egyéb, győződjön meg arról, hogy hello szimbólumfájlok hello hello fő alkalmazás .dll fájl ugyanabba a mappába (általában `wwwroot/bin`) vagy hello aktuális útvonalon érhetők el.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-172">For Azure Compute and other types, ensure that hello symbol files are in hello same folder of hello main application .dll (typically, `wwwroot/bin`) or are available on hello current path.</span></span>

### <a name="optimized-builds"></a><span data-ttu-id="9c2a7-173">Optimalizált buildek</span><span class="sxs-lookup"><span data-stu-id="9c2a7-173">Optimized builds</span></span>
<span data-ttu-id="9c2a7-174">Bizonyos esetekben helyi változók nem lehet megtekinteni a kiadási buildek hello felépítési folyamat során alkalmazott optimalizálásokat miatt.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-174">In some cases, local variables cannot be viewed in release builds because of optimizations that are applied during hello build process.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9c2a7-175">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9c2a7-175">Troubleshooting</span></span>

<span data-ttu-id="9c2a7-176">Ezek a tippek hello pillanatkép hibakereső kapcsolatos problémák hibaelhárítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-176">These tips help you troubleshoot problems with hello Snapshot Debugger.</span></span>

### <a name="verify-hello-instrumentation-key"></a><span data-ttu-id="9c2a7-177">Hello instrumentation kulcs ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9c2a7-177">Verify hello instrumentation key</span></span>

<span data-ttu-id="9c2a7-178">Győződjön meg arról, hogy a közzétett alkalmazás hello megfelelő instrumentation kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-178">Make sure that you're using hello correct instrumentation key in your published application.</span></span> <span data-ttu-id="9c2a7-179">Általában az Application Insights olvassa be az hello instrumentation kulcs hello ApplicationInsights.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-179">Usually, Application Insights reads hello instrumentation key from hello ApplicationInsights.config file.</span></span> <span data-ttu-id="9c2a7-180">Győződjön meg arról, hogy hello érték van hello azonos kulcsaként hello instrumentation hello Application Insights-erőforrást, hogy megjelenik-e hello portálon.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-180">Verify that hello value is hello same as hello instrumentation key for hello Application Insights resource that you see in hello portal.</span></span>

### <a name="check-hello-uploader-logs"></a><span data-ttu-id="9c2a7-181">Hello feltöltése naplófájlokban</span><span class="sxs-lookup"><span data-stu-id="9c2a7-181">Check hello uploader logs</span></span>

<span data-ttu-id="9c2a7-182">Pillanatkép létrehozása után egy kis memóriakép fájl (.dmp) jön létre a lemezen.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-182">After a snapshot is created, a minidump file (.dmp) is created on disk.</span></span> <span data-ttu-id="9c2a7-183">Egy külön feltöltése folyamat veszi, hogy kis memóriakép fájl, és feltölti azt, és minden társított PDB,-tooApplication Insights pillanatkép hibakereső tárolási fájlok.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-183">A separate uploader process takes that minidump file and uploads it, along with any associated PDBs, tooApplication Insights Snapshot Debugger storage.</span></span> <span data-ttu-id="9c2a7-184">Után hello kis memóriakép sikeresen fel van töltve, hanem törli a lemezen.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-184">After hello minidump has uploaded successfully, it is deleted from disk.</span></span> <span data-ttu-id="9c2a7-185">hello naplófájlok hello kis memóriakép feltöltése megőrzi a lemezen.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-185">hello log files for hello minidump uploader are retained on disk.</span></span> <span data-ttu-id="9c2a7-186">Egy App Service environment-környezetben található a naplók a `D:\Home\LogFiles\Uploader_*.log`.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-186">In an App Service environment, you can find these logs in `D:\Home\LogFiles\Uploader_*.log`.</span></span> <span data-ttu-id="9c2a7-187">Használjon hello Kudu felügyeleti webhely az App Service toofind ezek naplófájlok.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-187">Use hello Kudu management site for App Service toofind these log files.</span></span>

1. <span data-ttu-id="9c2a7-188">Nyissa meg az App Service alkalmazás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-188">Open your App Service application in hello Azure portal.</span></span>

2. <span data-ttu-id="9c2a7-189">Jelölje be hello **speciális eszközök** panelen, vagy keressen a **Kudu**.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-189">Select hello **Advanced Tools** blade, or search for **Kudu**.</span></span>
3. <span data-ttu-id="9c2a7-190">Kattintson a **Ugrás**.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-190">Click **Go**.</span></span>
4. <span data-ttu-id="9c2a7-191">A hello **hibakereső konzol** legördülő listáján jelölje ki **CMD**.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-191">In hello **Debug console** drop-down list box, select **CMD**.</span></span>
5. <span data-ttu-id="9c2a7-192">Kattintson a **naplófájlok**.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-192">Click **LogFiles**.</span></span>

<span data-ttu-id="9c2a7-193">Legalább egy fájlt egy nevű kezdődő kell megjelennie `Uploader_` és egy `.log` bővítmény.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-193">You should see at least one file with a name that begins with `Uploader_` and a `.log` extension.</span></span> <span data-ttu-id="9c2a7-194">Kattintson a megfelelő ikonra toodownload hello minden naplófájl, vagy egy böngészőben nyissa meg ezeket.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-194">Click hello appropriate icon toodownload any log files or open them in a browser.</span></span>
<span data-ttu-id="9c2a7-195">hello fájlnév hello gép nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-195">hello file name includes hello machine name.</span></span> <span data-ttu-id="9c2a7-196">Az App Service-példány egynél több számítógép üzemelteti, ha nincsenek az egyes gépek külön naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-196">If your App Service instance is hosted on more than one machine, there are separate log files for each machine.</span></span> <span data-ttu-id="9c2a7-197">Ha hello feltöltése egy új kis memóriakép fájlt észlel, azt hello naplófájl rögzíti.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-197">When hello uploader detects a new minidump file, it is recorded in hello log file.</span></span> <span data-ttu-id="9c2a7-198">Íme egy példa a hibátlan feltöltés:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-198">Here's an example of a successful upload:</span></span>

```
MinidumpUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:08.0349846Z
MinidumpUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 329.12 MB
    DateTime=2017-05-25T14:25:16.0145444Z
MinidumpUploader.exe Information: 0 : Upload successful.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2017-05-25T14:25:42.9164120Z
MinidumpUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2017-05-25T14:25:44.2310982Z
MinidumpUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2017-05-25T14:25:44.5435948Z
MinidumpUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2017-05-25T14:25:44.6095821Z
```

<span data-ttu-id="9c2a7-199">Hello előző példában hello instrumentation kulcsa `c12a605e73c44346a984e00000000000`.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-199">In hello previous example, hello instrumentation key is `c12a605e73c44346a984e00000000000`.</span></span> <span data-ttu-id="9c2a7-200">Ezt az értéket meg kell felelnie hello instrumentation kulcs az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-200">This value should match hello instrumentation key for your application.</span></span>
<span data-ttu-id="9c2a7-201">hello kis memóriakép rendelve egy pillanatkép hello azonosítójú `139e411a23934dc0b9ea08a626db16c5`.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-201">hello minidump is associated with a snapshot with hello ID `139e411a23934dc0b9ea08a626db16c5`.</span></span> <span data-ttu-id="9c2a7-202">Ezt az Azonosítót használható újabb toolocate hello kapcsolódó Application Insights Analytics kivételtelemetria.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-202">You can use this ID later toolocate hello associated exception telemetry in Application Insights Analytics.</span></span>

<span data-ttu-id="9c2a7-203">hello feltöltése 15 percenként egyszer kapcsolatos új PDB-fájlok keres.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-203">hello uploader scans for new PDBs about once every 15 minutes.</span></span> <span data-ttu-id="9c2a7-204">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-204">Here's an example:</span></span>

```
MinidumpUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\home\site\wwwroot\ for local PDBs.
    DateTime=2017-05-25T15:11:38.8003886Z
MinidumpUploader.exe Information: 0 : Scanning D:\local\Temporary ASP.NET Files\root\a6554c94\e3ad6f22\assembly\dl3\81d5008b\00b93cc8_dec5d201 for local PDBs.
    DateTime=2017-05-25T15:11:38.8160276Z
MinidumpUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2017-05-25T15:11:38.8316450Z
MinidumpUploader.exe Information: 0 : Deleted PDB scan marker D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\.pdbscan.
    DateTime=2017-05-25T15:11:38.8316450Z
```

<span data-ttu-id="9c2a7-205">Az alkalmazások, amelyek _nem_ hello feltöltése naplók vannak az App Service-ben üzemel hello mappában, amelyben a tömörített memóriaképek hello: `%TEMP%\Dumps\<ikey>` (ahol `<ikey>` a rendszerállapot-kulcs).</span><span class="sxs-lookup"><span data-stu-id="9c2a7-205">For applications that are _not_ hosted in App Service, hello uploader logs are in hello same folder as hello minidumps: `%TEMP%\Dumps\<ikey>` (where `<ikey>` is your instrumentation key).</span></span>

### <a name="use-application-insights-search-toofind-exceptions-with-snapshots"></a><span data-ttu-id="9c2a7-206">Használja az Application Insights toofind kivételek pillanatképekkel keresése</span><span class="sxs-lookup"><span data-stu-id="9c2a7-206">Use Application Insights search toofind exceptions with snapshots</span></span>

<span data-ttu-id="9c2a7-207">Egy pillanatkép jön létre, ha Kivétel kiváltása hello címkéje egy pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-207">When a snapshot is created, hello throwing exception is tagged with a snapshot ID.</span></span> <span data-ttu-id="9c2a7-208">Jelentett tooApplication Insights – kivételtelemetria hello esetén, hogy a pillanatkép-azonosító szerepel egy egyéni tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-208">When hello exception telemetry is reported tooApplication Insights, that snapshot ID is included as a custom property.</span></span> <span data-ttu-id="9c2a7-209">Hello az összes telemetriai adat található Application Insights hello Search paneljét használva `ai.snapshot.id` egyéni tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-209">Using hello Search blade in Application Insights, you can find all telemetry with hello `ai.snapshot.id` custom property.</span></span>

1. <span data-ttu-id="9c2a7-210">Keresse meg az Azure-portálon hello tooyour Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-210">Browse tooyour Application Insights resource in hello Azure portal.</span></span>
2. <span data-ttu-id="9c2a7-211">Kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-211">Click **Search**.</span></span>
3. <span data-ttu-id="9c2a7-212">Típus `ai.snapshot.id` a keresőmezőben hello, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-212">Type `ai.snapshot.id` in hello Search text box and press Enter.</span></span>

![Keresse meg a telemetriai adatok hello portálon egy pillanatkép-azonosító](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

<span data-ttu-id="9c2a7-214">Ha ez a keresés eredménytelen, majd pillanatképek nem volt jelentett tooApplication Insights az alkalmazáshoz kijelölt hello időtartományba esik.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-214">If this search returns no results, then no snapshots were reported tooApplication Insights for your application in hello selected time range.</span></span>

<span data-ttu-id="9c2a7-215">hello feltöltése naplókból adott pillanatkép azonosító toosearch azonosító hello keresőmezőbe írja be.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-215">toosearch for a specific snapshot ID from hello Uploader logs, type that ID in hello Search box.</span></span> <span data-ttu-id="9c2a7-216">Ha olyan pillanatképet, amely töltött fel tudja telemetriai adatok nem találhatók, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9c2a7-216">If you can't find telemetry for a snapshot that you know was uploaded, follow these steps:</span></span>

1. <span data-ttu-id="9c2a7-217">Ellenőrizze, hogy a hello instrumentation kulcs ellenőrzésével hello jobb Application Insights-erőforrás tartózkodik.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-217">Double-check that you're looking at hello right Application Insights resource by verifying hello instrumentation key.</span></span>

2. <span data-ttu-id="9c2a7-218">Hello időbélyeg hello feltöltése naplóból használ, állítsa be úgy hello időtartomány szűrő hello keresési toocover az adott időtartományt.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-218">Using hello timestamp from hello Uploader log, adjust hello Time Range filter of hello search toocover that time range.</span></span>

<span data-ttu-id="9c2a7-219">Ha még nem látja a pillanatkép Azonosítóval rendelkező kivételek, hello kivételtelemetria jelentett tooApplication Insights nem volt.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-219">If you still don't see an exception with that snapshot ID, then hello exception telemetry wasn't reported tooApplication Insights.</span></span> <span data-ttu-id="9c2a7-220">Ez a helyzet akkor fordulhat elő, ha az alkalmazás összeomlott után hello pillanatkép tartott, de előtt hello kivételtelemetria jelentette.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-220">This situation can happen if your application crashed after it took hello snapshot but before it reported hello exception telemetry.</span></span> <span data-ttu-id="9c2a7-221">Ebben az esetben ellenőrizze az App Service naplózza a hello `Diagnose and solve problems` toosee, ha váratlan volt újraindul, vagy nem kezelt kivételeket.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-221">In this case, check hello App Service logs under `Diagnose and solve problems` toosee if there were unexpected restarts or unhandled exceptions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c2a7-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9c2a7-222">Next steps</span></span>

* <span data-ttu-id="9c2a7-223">[Állítsa be a snappoints a kódban](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget pillanatképek kivétel várakozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-223">[Set snappoints in your code](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) tooget snapshots without waiting for an exception.</span></span>
* <span data-ttu-id="9c2a7-224">[Kivételek a webalkalmazások diagnosztizálásához](app-insights-asp-net-exceptions.md) ismerteti, hogyan toomake tooApplication Insights további kivételek látható.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-224">[Diagnose exceptions in your web apps](app-insights-asp-net-exceptions.md) explains how toomake more exceptions visible tooApplication Insights.</span></span> 
* <span data-ttu-id="9c2a7-225">[Észlelési intelligens](app-insights-proactive-diagnostics.md) automatikusan észleli a teljesítményanomáliákat.</span><span class="sxs-lookup"><span data-stu-id="9c2a7-225">[Smart Detection](app-insights-proactive-diagnostics.md) automatically discovers performance anomalies.</span></span>

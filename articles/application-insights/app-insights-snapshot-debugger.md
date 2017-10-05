---
title: "Az Azure Application Insights pillanatkép-hibakereső .NET-alkalmazások |} Microsoft Docs"
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
ms.openlocfilehash: 56eba2ff7af228b3c44354ad43b384288b4e1972
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="debug-snapshots-on-exceptions-in-net-apps"></a><span data-ttu-id="2c199-103">A .NET-alkalmazásokban kivételek pillanatképek hibakeresése</span><span class="sxs-lookup"><span data-stu-id="2c199-103">Debug snapshots on exceptions in .NET apps</span></span>

<span data-ttu-id="2c199-104">Ha kivétel lép, a működés közbeni webes alkalmazás automatikusan begyűjtheti a hibakeresési pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="2c199-104">When an exception occurs, you can automatically collect a debug snapshot from your live web application.</span></span> <span data-ttu-id="2c199-105">A pillanatkép a kivétel időpontjában forráskódját és a változók állapotát mutatja.</span><span class="sxs-lookup"><span data-stu-id="2c199-105">The snapshot shows the state of source code and variables at the moment the exception was thrown.</span></span> <span data-ttu-id="2c199-106">A pillanatkép-hibakereső (előzetes verzió) a [Azure Application Insights](app-insights-overview.md) kivételtelemetria a webes alkalmazás figyeli.</span><span class="sxs-lookup"><span data-stu-id="2c199-106">The Snapshot Debugger (preview) in [Azure Application Insights](app-insights-overview.md) monitors exception telemetry from your web app.</span></span> <span data-ttu-id="2c199-107">A pillanatképek a felső értesítő kivételek a összegyűjti az, hogy éles környezetben problémák felderítéséhez szükséges információk.</span><span class="sxs-lookup"><span data-stu-id="2c199-107">It collects snapshots on your top-throwing exceptions so that you have the information you need to diagnose issues in production.</span></span> <span data-ttu-id="2c199-108">Tartalmazza a [pillanatkép adatgyűjtő NuGet-csomag](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) az alkalmazásban, és szükség esetén adja meg a gyűjtemény paramétereit [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). A pillanatképek jelennek meg [kivételek](app-insights-asp-net-exceptions.md) az Application Insights portálon.</span><span class="sxs-lookup"><span data-stu-id="2c199-108">Include the [Snapshot collector NuGet package](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) in your application, and optionally configure collection parameters in [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md). Snapshots appear on [exceptions](app-insights-asp-net-exceptions.md) in the Application Insights portal.</span></span>

<span data-ttu-id="2c199-109">A portálon, hogy a hívási verem, és vizsgálja meg a változókat, minden egyes Hívásiverem-keret hibakeresési pillanatképek tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="2c199-109">You can view debug snapshots in the portal to see the call stack and inspect variables at each call stack frame.</span></span> <span data-ttu-id="2c199-110">Ahhoz, hogy egy sokkal hatékonyabb hibakeresési élmény a forráskódot, nyissa meg a pillanatképek a Visual Studio 2017 vállalati által [a pillanatkép-hibakereső bővítmény letöltése a Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="2c199-110">To get a more powerful debugging experience with source code, open snapshots with Visual Studio 2017 Enterprise by [downloading the Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

<span data-ttu-id="2c199-111">Pillanatkép gyűjtemény érhető el:</span><span class="sxs-lookup"><span data-stu-id="2c199-111">Snapshot collection is available for:</span></span>
* <span data-ttu-id="2c199-112">.NET-keretrendszer és az ASP.NET alkalmazások futó .NET-keretrendszer 4.5-ös vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2c199-112">.NET Framework and ASP.NET applications running .NET Framework 4.5 or later.</span></span>
* <span data-ttu-id="2c199-113">Windows rendszeren futó alkalmazások .NET core 2.0 és 2.0-s ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c199-113">.NET Core 2.0 and ASP.NET Core 2.0 applications running on Windows.</span></span>

### <a name="configure-snapshot-collection-for-aspnet-applications"></a><span data-ttu-id="2c199-114">Pillanatkép gyűjtemény az ASP.NET-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2c199-114">Configure snapshot collection for ASP.NET applications</span></span>

1. <span data-ttu-id="2c199-115">[Az Application Insights engedélyezéséhez a web app alkalmazásban](app-insights-asp-net.md), ha még nincs kész.</span><span class="sxs-lookup"><span data-stu-id="2c199-115">[Enable Application Insights in your web app](app-insights-asp-net.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="2c199-116">Tartalmazza a [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2c199-116">Include the [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span> 

3. <span data-ttu-id="2c199-117">Tekintse át az alapértelmezett beállításokat, a csomag hozzáadott [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="2c199-117">Review the default options that the package added to [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span>

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>5</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>06:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in one minute. -->
        <SnapshotsPerMinuteLimit>2</SnapshotsPerMinuteLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>50</SnapshotsPerDayLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. <span data-ttu-id="2c199-118">A pillanatképek összegyűjtése csak a kivételek az Application Insights jelentett.</span><span class="sxs-lookup"><span data-stu-id="2c199-118">Snapshots are collected only on exceptions that are reported to Application Insights.</span></span> <span data-ttu-id="2c199-119">Bizonyos esetekben (például a .NET platformon régebbi verzióknál), akkor módosítania [kivétel gyűjtemény konfigurálása](app-insights-asp-net-exceptions.md#exceptions) kivételeket pillanatfelvételekkel a portálon.</span><span class="sxs-lookup"><span data-stu-id="2c199-119">In some cases (for example, older versions of the .NET platform), you might need to [configure exception collection](app-insights-asp-net-exceptions.md#exceptions) to see exceptions with snapshots in the portal.</span></span>


### <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a><span data-ttu-id="2c199-120">Az ASP.NET Core 2.0 alkalmazások pillanatkép-gyűjtésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2c199-120">Configure snapshot collection for ASP.NET Core 2.0 applications</span></span>

1. <span data-ttu-id="2c199-121">[Az Application Insights engedélyezése az ASP.NET Core web app alkalmazásban](app-insights-asp-net-core.md), ha még nincs kész.</span><span class="sxs-lookup"><span data-stu-id="2c199-121">[Enable Application Insights in your ASP.NET Core web app](app-insights-asp-net-core.md), if you haven't done it yet.</span></span>

2. <span data-ttu-id="2c199-122">Tartalmazza a [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2c199-122">Include the [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="2c199-123">Módosítsa a `ConfigureServices` módszer az alkalmazás `Startup` osztály hozzáadása a pillanatkép-gyűjtő telemetriai processzor.</span><span class="sxs-lookup"><span data-stu-id="2c199-123">Modify the `ConfigureServices` method in your application's `Startup` class to add the Snapshot Collector's telemetry processor.</span></span> <span data-ttu-id="2c199-124">A kódot, hozzá kell adnia a Microsoft.ApplicationInsights.ASPNETCore NuGet csomag hivatkozott verziójától függ.</span><span class="sxs-lookup"><span data-stu-id="2c199-124">The code you should add depends on the referenced version of the Microsoft.ApplicationInsights.ASPNETCore NuGet package.</span></span>

   <span data-ttu-id="2c199-125">Microsoft.ApplicationInsights.AspNetCore 2.1.0 hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="2c199-125">For Microsoft.ApplicationInsights.AspNetCore 2.1.0 add:</span></span>
   ```C#
   using Microsoft.ApplicationInsights.SnapshotCollector;
   ...
   class Startup
   {
       // This method is called by the runtime. Use it to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddSingleton<Func<ITelemetryProcessor, ITelemetryProcessor>>(next => new SnapshotCollectorTelemetryProcessor(next));
           // TODO: Add any other services your application needs here.
       }
   }
   ```

   <span data-ttu-id="2c199-126">Microsoft.ApplicationInsights.AspNetCore 2.1.1 hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="2c199-126">For Microsoft.ApplicationInsights.AspNetCore 2.1.1 add:</span></span>
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

       // This method is called by the runtime. Use it to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
            services.AddSingleton<ITelemetryProcessorFactory>(new SnapshotCollectorTelemetryProcessorFactory());
           // TODO: Add any other services your application needs here.
       }
   }
   ```

### <a name="configure-snapshot-collection-for-other-net-applications"></a><span data-ttu-id="2c199-127">Pillanatkép gyűjtemény más .NET-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2c199-127">Configure snapshot collection for other .NET applications</span></span>

1. <span data-ttu-id="2c199-128">Ha az alkalmazás már nem tagolva az Application insights szolgáltatással, Kezdésként [Application Insights engedélyezése és a rendszerállapot-kulcs](app-insights-windows-desktop.md).</span><span class="sxs-lookup"><span data-stu-id="2c199-128">If your application is not already instrumented with Application Insights, get started by [enabling Application Insights and setting the instrumentation key](app-insights-windows-desktop.md).</span></span>

2. <span data-ttu-id="2c199-129">Adja hozzá a [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet-csomag az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2c199-129">Add the [Microsoft.ApplicationInsights.SnapshotCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet package in your app.</span></span>

3. <span data-ttu-id="2c199-130">A pillanatképek összegyűjtése csak a kivételek az Application Insights jelentett.</span><span class="sxs-lookup"><span data-stu-id="2c199-130">Snapshots are collected only on exceptions that are reported to Application Insights.</span></span> <span data-ttu-id="2c199-131">Szükség lehet a jelentés azokat a kód módosítása.</span><span class="sxs-lookup"><span data-stu-id="2c199-131">You may need to modify your code to report them.</span></span> <span data-ttu-id="2c199-132">A kivételkezelő kód függ az alkalmazás szerkezete, de például nem éri el:</span><span class="sxs-lookup"><span data-stu-id="2c199-132">The exception handling code depends on the structure of your application, but an example is below:</span></span>
    ```C#
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```
    
## <a name="grant-permissions"></a><span data-ttu-id="2c199-133">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="2c199-133">Grant permissions</span></span>

<span data-ttu-id="2c199-134">Az Azure-előfizetés tulajdonosainak pillanatképek vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="2c199-134">Owners of the Azure subscription can inspect snapshots.</span></span> <span data-ttu-id="2c199-135">Más felhasználók tulajdonos engedéllyel kell rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="2c199-135">Other users must be granted permission by an owner.</span></span>

<span data-ttu-id="2c199-136">Adja meg az engedélyt, rendelje hozzá a `Application Insights Snapshot Debugger` a felhasználók számára, akik vizsgálja a pillanatképek szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2c199-136">To grant permission, assign the `Application Insights Snapshot Debugger` role to users who will inspect snapshots.</span></span> <span data-ttu-id="2c199-137">Egyes felhasználók vagy csoportok által a cél Application Insights-erőforrás előfizetésnél tulajdonos vagy a erőforráscsoportba vagy előfizetésbe ehhez a szerepkörhöz is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="2c199-137">This role can be assigned to individual users or groups by subscription owners for the target Application Insights resource or its resource group or subscription.</span></span>

1. <span data-ttu-id="2c199-138">A hozzáférés-vezérlés (IAM) panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2c199-138">Open the Access Control (IAM) blade.</span></span>
1. <span data-ttu-id="2c199-139">Kattintson a + Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="2c199-139">Click the +Add button.</span></span>
1. <span data-ttu-id="2c199-140">Application Insights pillanatkép hibakereső kiválasztása a szerepkörök legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="2c199-140">Select Application Insights Snapshot Debugger from the Roles drop-down list.</span></span>
1. <span data-ttu-id="2c199-141">Keresse meg és írja be a hozzáadni kívánt felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="2c199-141">Search for and enter a name for the user to add.</span></span>
1. <span data-ttu-id="2c199-142">A Mentés gombra kattintva adja hozzá a felhasználót a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="2c199-142">Click the Save button to add the user to the role.</span></span>


[!IMPORTANT]
    <span data-ttu-id="2c199-143">A pillanatképek potenciálisan tartalmazó változó és a paraméter értékét a személyes és más bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="2c199-143">Snapshots can potentially contain personal and other sensitive information in variable and parameter values.</span></span>

## <a name="debug-snapshots-in-the-application-insights-portal"></a><span data-ttu-id="2c199-144">Az Application Insights portálon pillanatképek hibakeresése</span><span class="sxs-lookup"><span data-stu-id="2c199-144">Debug snapshots in the Application Insights portal</span></span>

<span data-ttu-id="2c199-145">Pillanatkép érhető el egy adott kivétel vagy probléma azonosítója, ha egy **Debug pillanatfelvétel megnyitása** gomb megjelenik a [kivétel](app-insights-asp-net-exceptions.md) az Application Insights portálon.</span><span class="sxs-lookup"><span data-stu-id="2c199-145">If a snapshot is available for a given exception or a problem ID, an **Open Debug Snapshot** button appears on the [exception](app-insights-asp-net-exceptions.md) in the Application Insights portal.</span></span>

![Nyissa meg Debug pillanatkép gombra a kivételei](./media/app-insights-snapshot-debugger/snapshot-on-exception.png)

<span data-ttu-id="2c199-147">A hibakeresési pillanatkép nézetben láthatja a hívási verem és a változók ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="2c199-147">In the Debug Snapshot view, you see a call stack and a variables pane.</span></span> <span data-ttu-id="2c199-148">A hívási verem keretek a hívási verem ablaktáblán, megtekintheti a helyi változókat és paraméterek függvény hívása a Változók panelen.</span><span class="sxs-lookup"><span data-stu-id="2c199-148">When you select frames of the call stack in the call stack pane, you can view local variables and parameters for that function call in the variables pane.</span></span>

![Nézet Debug pillanatkép a portálon](./media/app-insights-snapshot-debugger/open-snapshot-portal.png)

<span data-ttu-id="2c199-150">A pillanatképek kényes információt is tartalmazhat, és alapértelmezés szerint nincsenek megtekinthető.</span><span class="sxs-lookup"><span data-stu-id="2c199-150">Snapshots might contain sensitive information, and by default they are not viewable.</span></span> <span data-ttu-id="2c199-151">A pillanatképek megtekintéséhez rendelkeznie kell a `Application Insights Snapshot Debugger` Önhöz rendelt szerepkör.</span><span class="sxs-lookup"><span data-stu-id="2c199-151">To view snapshots, you must have the `Application Insights Snapshot Debugger` role assigned to you.</span></span>

## <a name="debug-snapshots-with-visual-studio-2017-enterprise"></a><span data-ttu-id="2c199-152">A Visual Studio 2017 vállalati pillanatképek hibakeresése</span><span class="sxs-lookup"><span data-stu-id="2c199-152">Debug snapshots with Visual Studio 2017 Enterprise</span></span>
1. <span data-ttu-id="2c199-153">Kattintson a **letöltése pillanatkép** gombra kattintva töltse le a `.diagsession` fájlhoz, amely a Visual Studio 2017 Enterprise nyithatja meg.</span><span class="sxs-lookup"><span data-stu-id="2c199-153">Click the **Download Snapshot** button to download a `.diagsession` file, which can be opened by Visual Studio 2017 Enterprise.</span></span> 

2. <span data-ttu-id="2c199-154">Lehetőségre a `.diagsession` fájl, először [töltse le és telepítse a pillanatkép-hibakereső bővítményt a Visual Studio](https://aka.ms/snapshotdebugger).</span><span class="sxs-lookup"><span data-stu-id="2c199-154">To open the `.diagsession` file, you must first [download and install the Snapshot Debugger extension for Visual Studio](https://aka.ms/snapshotdebugger).</span></span>

3. <span data-ttu-id="2c199-155">A pillanatfelvétel-fájl megnyitása után a Visual Studio kis memóriakép Hibakeresés lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2c199-155">After you open the snapshot file, the Minidump Debugging page in Visual Studio appears.</span></span> <span data-ttu-id="2c199-156">Kattintson a **felügyelt kód Debug** a hibakeresés a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="2c199-156">Click **Debug Managed Code** to start debugging the snapshot.</span></span> <span data-ttu-id="2c199-157">Ha a kivétel lépett fel, hogy a folyamat az aktuális állapotát is hibakeresése kódsort a pillanatkép nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2c199-157">The snapshot opens to the line of code where the exception was thrown so that you can debug the current state of the process.</span></span>

    ![A Visual Studio nézet hibakeresési pillanatkép](./media/app-insights-snapshot-debugger/open-snapshot-visualstudio.png)

<span data-ttu-id="2c199-159">A letöltött pillanatkép bármely alkalmazás webkiszolgálón található szimbólum fájlokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2c199-159">The downloaded snapshot contains any symbol files that were found on your web application server.</span></span> <span data-ttu-id="2c199-160">A szimbólum fájlok rendelje hozzá a pillanatkép-adatok forráskód szükségesek.</span><span class="sxs-lookup"><span data-stu-id="2c199-160">These symbol files are required to associate snapshot data with source code.</span></span> <span data-ttu-id="2c199-161">App Service-alkalmazásokhoz végezze el a szimbólum központi telepítés engedélyezése a webalkalmazások közzétételekor.</span><span class="sxs-lookup"><span data-stu-id="2c199-161">For App Service apps, make sure to enable symbol deployment when you publish your web apps.</span></span>

## <a name="how-snapshots-work"></a><span data-ttu-id="2c199-162">A pillanatképek működése</span><span class="sxs-lookup"><span data-stu-id="2c199-162">How snapshots work</span></span>

<span data-ttu-id="2c199-163">Az alkalmazás indításakor egy külön pillanatkép feltöltése folyamat jön létre, amely az alkalmazás a pillanatkép-kéréseket figyeli.</span><span class="sxs-lookup"><span data-stu-id="2c199-163">When your application starts, a separate snapshot uploader process is created that monitors your application for snapshot requests.</span></span> <span data-ttu-id="2c199-164">Pillanatkép van szükség, ha a futó folyamattal árnyékmásolatát körülbelül 10-20 perc múlva történik.</span><span class="sxs-lookup"><span data-stu-id="2c199-164">When a snapshot is requested, a shadow copy of the running process is made in about 10 to 20 minutes.</span></span> <span data-ttu-id="2c199-165">Az árnyékmásolat-folyamat majd elemzésnek, és egy pillanatkép jön létre, amíg a fő folyamat tovább fut és forgalmat szolgálja ki a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="2c199-165">The shadow process is then analyzed, and a snapshot is created while the main process continues to run and serve traffic to users.</span></span> <span data-ttu-id="2c199-166">A pillanatkép majd tölt fel az Application insights szolgáltatással együtt megfelelő szimbólum (.pdb) fájlok, amelyek szükségesek ahhoz, hogy az adatok megtekintését.</span><span class="sxs-lookup"><span data-stu-id="2c199-166">The snapshot is then uploaded to Application Insights along with any relevant symbol (.pdb) files that are needed to view the snapshot.</span></span>

## <a name="current-limitations"></a><span data-ttu-id="2c199-167">Aktuális korlátozások</span><span class="sxs-lookup"><span data-stu-id="2c199-167">Current limitations</span></span>

### <a name="publish-symbols"></a><span data-ttu-id="2c199-168">Szimbólumok közzététele</span><span class="sxs-lookup"><span data-stu-id="2c199-168">Publish symbols</span></span>
<span data-ttu-id="2c199-169">A pillanatkép-hibakereső van szükség, szimbólumfájlok dekódolni a változók és a Visual Studio hibakeresési érdekében az üzemi kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2c199-169">The Snapshot Debugger requires symbol files on the production server to decode variables and to provide a debugging experience in Visual Studio.</span></span> <span data-ttu-id="2c199-170">A Visual Studio 2017 15.2 kiadása kiadás buildek szimbólumait alapértelmezés szerint közzéteszi az App Service teszi közzé.</span><span class="sxs-lookup"><span data-stu-id="2c199-170">The 15.2 release of Visual Studio 2017 publishes symbols for release builds by default when it publishes to App Service.</span></span> <span data-ttu-id="2c199-171">A korábbi verziók, adja hozzá az alábbi sort a közzétételi profilt kell `.pubxml` fájlt úgy, hogy a szimbólumok közzétett kiadási módban:</span><span class="sxs-lookup"><span data-stu-id="2c199-171">In prior versions, you need to add the following line to your publish profile `.pubxml` file so that symbols are published in release mode:</span></span>

```xml
    <ExcludeGeneratedDebugSymbol>False</ExcludeGeneratedDebugSymbol>
```

<span data-ttu-id="2c199-172">Azure számítási és más típusú, győződjön meg arról, hogy a szimbólum a fő alkalmazás .dll fájl ugyanabban a mappában (általában `wwwroot/bin`) vagy az aktuális útvonalon érhetők el.</span><span class="sxs-lookup"><span data-stu-id="2c199-172">For Azure Compute and other types, ensure that the symbol files are in the same folder of the main application .dll (typically, `wwwroot/bin`) or are available on the current path.</span></span>

### <a name="optimized-builds"></a><span data-ttu-id="2c199-173">Optimalizált buildek</span><span class="sxs-lookup"><span data-stu-id="2c199-173">Optimized builds</span></span>
<span data-ttu-id="2c199-174">Bizonyos esetekben helyi változók nem lehet megtekinteni kiadás buildekben létrehozása során alkalmazott optimalizálás miatt.</span><span class="sxs-lookup"><span data-stu-id="2c199-174">In some cases, local variables cannot be viewed in release builds because of optimizations that are applied during the build process.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="2c199-175">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="2c199-175">Troubleshooting</span></span>

<span data-ttu-id="2c199-176">Ezek a tippek a pillanatkép-hibakereső kapcsolatos problémák hibaelhárítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="2c199-176">These tips help you troubleshoot problems with the Snapshot Debugger.</span></span>

### <a name="verify-the-instrumentation-key"></a><span data-ttu-id="2c199-177">Ellenőrizze a rendszerállapot-kulcsot</span><span class="sxs-lookup"><span data-stu-id="2c199-177">Verify the instrumentation key</span></span>

<span data-ttu-id="2c199-178">Győződjön meg arról, hogy a közzétett alkalmazást a megfelelő instrumentation kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="2c199-178">Make sure that you're using the correct instrumentation key in your published application.</span></span> <span data-ttu-id="2c199-179">Az Application Insights általában a instrumentation kulcs beolvassa az ApplicationInsights.config fájl.</span><span class="sxs-lookup"><span data-stu-id="2c199-179">Usually, Application Insights reads the instrumentation key from the ApplicationInsights.config file.</span></span> <span data-ttu-id="2c199-180">Győződjön meg arról, hogy az érték azonos a instrumentation kulcs az Application Insights-erőforrás, melyek megjelennek a portálon.</span><span class="sxs-lookup"><span data-stu-id="2c199-180">Verify that the value is the same as the instrumentation key for the Application Insights resource that you see in the portal.</span></span>

### <a name="check-the-uploader-logs"></a><span data-ttu-id="2c199-181">Ellenőrizze a feltöltése naplókat</span><span class="sxs-lookup"><span data-stu-id="2c199-181">Check the uploader logs</span></span>

<span data-ttu-id="2c199-182">Pillanatkép létrehozása után egy kis memóriakép fájl (.dmp) jön létre a lemezen.</span><span class="sxs-lookup"><span data-stu-id="2c199-182">After a snapshot is created, a minidump file (.dmp) is created on disk.</span></span> <span data-ttu-id="2c199-183">Egy külön feltöltése folyamat veszi, hogy kis memóriakép fájl, és feltölti azt, és minden társított PDB-fájlok, az Application Insights pillanatkép hibakereső tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="2c199-183">A separate uploader process takes that minidump file and uploads it, along with any associated PDBs, to Application Insights Snapshot Debugger storage.</span></span> <span data-ttu-id="2c199-184">Után a kis memóriakép sikeresen fel van töltve, hanem törli a lemezen.</span><span class="sxs-lookup"><span data-stu-id="2c199-184">After the minidump has uploaded successfully, it is deleted from disk.</span></span> <span data-ttu-id="2c199-185">A naplófájlokat a kis memóriakép feltöltése megőrzi a lemezen.</span><span class="sxs-lookup"><span data-stu-id="2c199-185">The log files for the minidump uploader are retained on disk.</span></span> <span data-ttu-id="2c199-186">Egy App Service environment-környezetben található a naplók a `D:\Home\LogFiles\Uploader_*.log`.</span><span class="sxs-lookup"><span data-stu-id="2c199-186">In an App Service environment, you can find these logs in `D:\Home\LogFiles\Uploader_*.log`.</span></span> <span data-ttu-id="2c199-187">A Kudu felügyeleti webhely az App Service segítségével ezekben a naplófájlokban található.</span><span class="sxs-lookup"><span data-stu-id="2c199-187">Use the Kudu management site for App Service to find these log files.</span></span>

1. <span data-ttu-id="2c199-188">Nyissa meg az App Service-alkalmazás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="2c199-188">Open your App Service application in the Azure portal.</span></span>

2. <span data-ttu-id="2c199-189">Válassza ki a **speciális eszközök** panelen, vagy keressen a **Kudu**.</span><span class="sxs-lookup"><span data-stu-id="2c199-189">Select the **Advanced Tools** blade, or search for **Kudu**.</span></span>
3. <span data-ttu-id="2c199-190">Kattintson a **Ugrás**.</span><span class="sxs-lookup"><span data-stu-id="2c199-190">Click **Go**.</span></span>
4. <span data-ttu-id="2c199-191">Az a **hibakereső konzol** legördülő listáján jelölje ki **CMD**.</span><span class="sxs-lookup"><span data-stu-id="2c199-191">In the **Debug console** drop-down list box, select **CMD**.</span></span>
5. <span data-ttu-id="2c199-192">Kattintson a **naplófájlok**.</span><span class="sxs-lookup"><span data-stu-id="2c199-192">Click **LogFiles**.</span></span>

<span data-ttu-id="2c199-193">Legalább egy fájlt egy nevű kezdődő kell megjelennie `Uploader_` és egy `.log` bővítmény.</span><span class="sxs-lookup"><span data-stu-id="2c199-193">You should see at least one file with a name that begins with `Uploader_` and a `.log` extension.</span></span> <span data-ttu-id="2c199-194">Kattintson a megfelelő ikonra, töltse le a naplófájlokat, illetve egy böngészőben nyissa meg ezeket.</span><span class="sxs-lookup"><span data-stu-id="2c199-194">Click the appropriate icon to download any log files or open them in a browser.</span></span>
<span data-ttu-id="2c199-195">A fájl nevét a gép nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2c199-195">The file name includes the machine name.</span></span> <span data-ttu-id="2c199-196">Az App Service-példány egynél több számítógép üzemelteti, ha nincsenek az egyes gépek külön naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="2c199-196">If your App Service instance is hosted on more than one machine, there are separate log files for each machine.</span></span> <span data-ttu-id="2c199-197">Ha a feltöltése egy új kis memóriakép fájlt észlel, azt a naplófájl rögzíti.</span><span class="sxs-lookup"><span data-stu-id="2c199-197">When the uploader detects a new minidump file, it is recorded in the log file.</span></span> <span data-ttu-id="2c199-198">Íme egy példa a hibátlan feltöltés:</span><span class="sxs-lookup"><span data-stu-id="2c199-198">Here's an example of a successful upload:</span></span>

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

<span data-ttu-id="2c199-199">Az előző példában a instrumentation kulcsa `c12a605e73c44346a984e00000000000`.</span><span class="sxs-lookup"><span data-stu-id="2c199-199">In the previous example, the instrumentation key is `c12a605e73c44346a984e00000000000`.</span></span> <span data-ttu-id="2c199-200">Ezt az értéket meg kell felelnie a instrumentation billentyűt az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2c199-200">This value should match the instrumentation key for your application.</span></span>
<span data-ttu-id="2c199-201">A kis memóriakép társítva a azonosítójú pillanatkép `139e411a23934dc0b9ea08a626db16c5`.</span><span class="sxs-lookup"><span data-stu-id="2c199-201">The minidump is associated with a snapshot with the ID `139e411a23934dc0b9ea08a626db16c5`.</span></span> <span data-ttu-id="2c199-202">Ezt az Azonosítót később segítségével keresse meg a társított kivételtelemetria Application Insights Analytics.</span><span class="sxs-lookup"><span data-stu-id="2c199-202">You can use this ID later to locate the associated exception telemetry in Application Insights Analytics.</span></span>

<span data-ttu-id="2c199-203">A feltöltése 15 percenként egyszer kapcsolatos új PDB-fájlok keres.</span><span class="sxs-lookup"><span data-stu-id="2c199-203">The uploader scans for new PDBs about once every 15 minutes.</span></span> <span data-ttu-id="2c199-204">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="2c199-204">Here's an example:</span></span>

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

<span data-ttu-id="2c199-205">Az alkalmazások, amelyek _nem_ üzemelteti az App Service-ben, a feltöltése feldolgozásra a tömörített memóriaképek ugyanabban a mappában: `%TEMP%\Dumps\<ikey>` (ahol `<ikey>` a rendszerállapot-kulcs).</span><span class="sxs-lookup"><span data-stu-id="2c199-205">For applications that are _not_ hosted in App Service, the uploader logs are in the same folder as the minidumps: `%TEMP%\Dumps\<ikey>` (where `<ikey>` is your instrumentation key).</span></span>

### <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a><span data-ttu-id="2c199-206">Használja az Application Insights keresési pillanatképekkel kivételek kereséséhez</span><span class="sxs-lookup"><span data-stu-id="2c199-206">Use Application Insights search to find exceptions with snapshots</span></span>

<span data-ttu-id="2c199-207">Egy pillanatkép jön létre, amikor a rtesítő kivétel címkéje egy pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="2c199-207">When a snapshot is created, the throwing exception is tagged with a snapshot ID.</span></span> <span data-ttu-id="2c199-208">Ha a kivételtelemetria bejelentések Application Insights, hogy a pillanatkép azonosítója megtalálható egyéni tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2c199-208">When the exception telemetry is reported to Application Insights, that snapshot ID is included as a custom property.</span></span> <span data-ttu-id="2c199-209">Az összes telemetriai adat található Application Insights a Search paneljét használ, a `ai.snapshot.id` egyéni tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2c199-209">Using the Search blade in Application Insights, you can find all telemetry with the `ai.snapshot.id` custom property.</span></span>

1. <span data-ttu-id="2c199-210">Keresse meg az Application Insights-erőforrást az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="2c199-210">Browse to your Application Insights resource in the Azure portal.</span></span>
2. <span data-ttu-id="2c199-211">Kattintson a **keresési**.</span><span class="sxs-lookup"><span data-stu-id="2c199-211">Click **Search**.</span></span>
3. <span data-ttu-id="2c199-212">Típus `ai.snapshot.id` a keresőmezőben, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="2c199-212">Type `ai.snapshot.id` in the Search text box and press Enter.</span></span>

![Keresse meg a telemetriai adatok egy pillanatkép-azonosító a portálon](./media/app-insights-snapshot-debugger/search-snapshot-portal.png)

<span data-ttu-id="2c199-214">Ha ez a keresés eredménytelen, majd pillanatképek nem jelentette az Application Insights az alkalmazáshoz, a kijelölt időtartományban.</span><span class="sxs-lookup"><span data-stu-id="2c199-214">If this search returns no results, then no snapshots were reported to Application Insights for your application in the selected time range.</span></span>

<span data-ttu-id="2c199-215">Keresse meg a feltöltése naplókból egy adott pillanatkép-azonosító, a keresési mezőbe írja be azonosító.</span><span class="sxs-lookup"><span data-stu-id="2c199-215">To search for a specific snapshot ID from the Uploader logs, type that ID in the Search box.</span></span> <span data-ttu-id="2c199-216">Ha olyan pillanatképet, amely töltött fel tudja telemetriai adatok nem találhatók, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2c199-216">If you can't find telemetry for a snapshot that you know was uploaded, follow these steps:</span></span>

1. <span data-ttu-id="2c199-217">Ellenőrizze, hogy van szüksége, a jobb oldali Application Insights-erőforrás a instrumentation kulcs ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="2c199-217">Double-check that you're looking at the right Application Insights resource by verifying the instrumentation key.</span></span>

2. <span data-ttu-id="2c199-218">A feltöltése naplóból időbélyeg használatával, állítsa be úgy a fedik le az adott időtartományt a keresés időtartományát szűrő.</span><span class="sxs-lookup"><span data-stu-id="2c199-218">Using the timestamp from the Uploader log, adjust the Time Range filter of the search to cover that time range.</span></span>

<span data-ttu-id="2c199-219">Ha még nem látja a pillanatkép Azonosítóval rendelkező kivételek, a kivételtelemetria Application Insights nem jelzett.</span><span class="sxs-lookup"><span data-stu-id="2c199-219">If you still don't see an exception with that snapshot ID, then the exception telemetry wasn't reported to Application Insights.</span></span> <span data-ttu-id="2c199-220">Ez a helyzet akkor fordulhat elő, ha az alkalmazás összeomlott követően a pillanatkép által, de a kivételtelemetria jelentette előtt.</span><span class="sxs-lookup"><span data-stu-id="2c199-220">This situation can happen if your application crashed after it took the snapshot but before it reported the exception telemetry.</span></span> <span data-ttu-id="2c199-221">Ebben az esetben a naplófájlokban App Service alatt `Diagnose and solve problems` megtekintéséhez, hogy voltak-e váratlan újraindítása vagy a nem kezelt kivételeket.</span><span class="sxs-lookup"><span data-stu-id="2c199-221">In this case, check the App Service logs under `Diagnose and solve problems` to see if there were unexpected restarts or unhandled exceptions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c199-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c199-222">Next steps</span></span>

* <span data-ttu-id="2c199-223">[Állítsa be a snappoints a kódban](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) kivétel várakozás nélkül pillanatképek eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2c199-223">[Set snappoints in your code](https://azure.microsoft.com/blog/snapshot-debugger-for-azure/) to get snapshots without waiting for an exception.</span></span>
* <span data-ttu-id="2c199-224">[Kivételek a webalkalmazások diagnosztizálásához](app-insights-asp-net-exceptions.md) ismerteti, hogyan további kivételek láthatóvá az Application Insights részére.</span><span class="sxs-lookup"><span data-stu-id="2c199-224">[Diagnose exceptions in your web apps](app-insights-asp-net-exceptions.md) explains how to make more exceptions visible to Application Insights.</span></span> 
* <span data-ttu-id="2c199-225">[Észlelési intelligens](app-insights-proactive-diagnostics.md) automatikusan észleli a teljesítményanomáliákat.</span><span class="sxs-lookup"><span data-stu-id="2c199-225">[Smart Detection](app-insights-proactive-diagnostics.md) automatically discovers performance anomalies.</span></span>

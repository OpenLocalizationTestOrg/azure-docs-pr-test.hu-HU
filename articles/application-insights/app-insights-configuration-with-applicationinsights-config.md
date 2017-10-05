---
title: ApplicationInsights.config referencia - Azure |} Microsoft Docs
description: "Engedélyezze vagy tiltsa le az adatok gyűjtése modulok, és adja hozzá a teljesítményszámlálók és más paramétereket."
services: application-insights
documentationcenter: 
author: OlegAnaniev-MSFT
editor: alancameronwills
manager: carmonm
ms.assetid: 6e397752-c086-46e9-8648-a1196e8078c2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 7737f47d4181b5e920434f3a5372991efb58f63e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configuring-the-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a><span data-ttu-id="09470-103">Az Application Insights SDK konfigurálása az ApplicationInsights.config vagy .xml használatával</span><span class="sxs-lookup"><span data-stu-id="09470-103">Configuring the Application Insights SDK with ApplicationInsights.config or .xml</span></span>
<span data-ttu-id="09470-104">Az Application Insights .NET SDK NuGet-csomagok számos áll.</span><span class="sxs-lookup"><span data-stu-id="09470-104">The Application Insights .NET SDK consists of a number of NuGet packages.</span></span> <span data-ttu-id="09470-105">A [core csomag](http://www.nuget.org/packages/Microsoft.ApplicationInsights) telemetriai adatok küldése az Application Insights az API-t biztosít.</span><span class="sxs-lookup"><span data-stu-id="09470-105">The [core package](http://www.nuget.org/packages/Microsoft.ApplicationInsights) provides the API for sending telemetry to the Application Insights.</span></span> <span data-ttu-id="09470-106">[További csomagok](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) adja meg a telemetriai adatok *modulok* és *inicializálók* automatikusan nyomon követése a telemetriai adatok az alkalmazás és a környezetben.</span><span class="sxs-lookup"><span data-stu-id="09470-106">[Additional packages](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) provide telemetry *modules* and *initializers* for automatically tracking telemetry from your application and its context.</span></span> <span data-ttu-id="09470-107">A konfigurációs fájl módosításával engedélyezze vagy tiltsa le a telemetria-modulokat és az inicializálók, és némelyikük paramétereinek megadása.</span><span class="sxs-lookup"><span data-stu-id="09470-107">By adjusting the configuration file, you can enable or disable telemetry modules and initializers, and set parameters for some of them.</span></span>

<span data-ttu-id="09470-108">A konfigurációs fájl neve `ApplicationInsights.config` vagy `ApplicationInsights.xml`, az alkalmazás típusától függően.</span><span class="sxs-lookup"><span data-stu-id="09470-108">The configuration file is named `ApplicationInsights.config` or `ApplicationInsights.xml`, depending on the type of your application.</span></span> <span data-ttu-id="09470-109">Az automatikusan hozzáadódik a projekt amikor Ön [telepítse az SDK legtöbb verzióit][start].</span><span class="sxs-lookup"><span data-stu-id="09470-109">It is automatically added to your project when you [install most versions of the SDK][start].</span></span> <span data-ttu-id="09470-110">Is hozzáadódik a webes alkalmazások [állapotfigyelő egy IIS-kiszolgálón][redfield], vagy, ha bejelöli a Appplication Insights [az Azure webhelyén vagy a virtuális gép bővítmény](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="09470-110">It is also added to a web app by [Status Monitor on an IIS server][redfield], or when you select the Appplication Insights [extension for an Azure website or VM](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="09470-111">Nincs a vezérlőhöz egy egyenértékű fájlt a [SDK-t egy weblap][client].</span><span class="sxs-lookup"><span data-stu-id="09470-111">There isn't an equivalent file to control the [SDK in a web page][client].</span></span>

<span data-ttu-id="09470-112">Ez a dokumentum ismerteti a szakaszok látható, a konfigurációs fájlban, hogyan azok szabályozza, hogy az SDK összetevői és mely NuGet-csomagok betölteni az összetevőket.</span><span class="sxs-lookup"><span data-stu-id="09470-112">This document describes the sections you see in the configuration file, how they control the components of the SDK, and which NuGet packages load those components.</span></span>

## <a name="telemetry-modules-aspnet"></a><span data-ttu-id="09470-113">Telemetria modulok (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="09470-113">Telemetry Modules (ASP.NET)</span></span>
<span data-ttu-id="09470-114">Minden telemetriai modul egy adott típusú adatokat gyűjt, és a core API segítségével küldheti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="09470-114">Each telemetry module collects a specific type of data and uses the core API to send the data.</span></span> <span data-ttu-id="09470-115">A modulok különböző NuGet-csomagok, amelyek is vegye fel a szükséges sorok .config fájl telepíti.</span><span class="sxs-lookup"><span data-stu-id="09470-115">The modules are installed by different NuGet packages, which also add the required lines to the .config file.</span></span>

<span data-ttu-id="09470-116">Nincs a csomópont minden modul a konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="09470-116">There's a node in the configuration file for each module.</span></span> <span data-ttu-id="09470-117">Modul letiltásához törölje a csomópont, vagy hozzászólási ki.</span><span class="sxs-lookup"><span data-stu-id="09470-117">To disable a module, delete the node or comment it out.</span></span>

### <a name="dependency-tracking"></a><span data-ttu-id="09470-118">A függőségi nyomon követése</span><span class="sxs-lookup"><span data-stu-id="09470-118">Dependency Tracking</span></span>
<span data-ttu-id="09470-119">[Követés függőségi](app-insights-asp-net-dependencies.md) telemetriai adatainak az alkalmazás hajt végre, és külső szolgáltatások és adatbázisait hívások gyűjti.</span><span class="sxs-lookup"><span data-stu-id="09470-119">[Dependency tracking](app-insights-asp-net-dependencies.md) collects telemetry about calls your app makes to databases and external services and databases.</span></span> <span data-ttu-id="09470-120">Ez a modul fog működni az IIS-kiszolgáló engedélyezéséhez kell [Állapotmonitor telepítése][redfield].</span><span class="sxs-lookup"><span data-stu-id="09470-120">To allow this module to work in an IIS server, you need to [install Status Monitor][redfield].</span></span> <span data-ttu-id="09470-121">A használatára az Azure-webalkalmazásokban vagy a virtuális gépek, [jelölje ki az Application Insights bővítményt](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="09470-121">To use it in Azure web apps or VMs, [select the Application Insights extension](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="09470-122">A saját függőségi nyomkövetési kód használatával is írhat a [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="09470-122">You can also write your own dependency tracking code using the [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* <span data-ttu-id="09470-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="09470-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet package.</span></span>

### <a name="performance-collector"></a><span data-ttu-id="09470-124">Teljesítmény-gyűjtő</span><span class="sxs-lookup"><span data-stu-id="09470-124">Performance collector</span></span>
<span data-ttu-id="09470-125">[Gyűjti a rendszerteljesítmény-számlálók](app-insights-performance-counters.md) például CPU és memória- és hálózati betöltése az IIS telepítése.</span><span class="sxs-lookup"><span data-stu-id="09470-125">[Collects system performance counters](app-insights-performance-counters.md) such as CPU, memory and network load from IIS installations.</span></span> <span data-ttu-id="09470-126">Megadhat számlálók gyűjthet, többek között a saját kezűleg beállított teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="09470-126">You can specify which counters to collect, including performance counters you have set up yourself.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* <span data-ttu-id="09470-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="09470-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet package.</span></span>

### <a name="application-insights-diagnostics-telemetry"></a><span data-ttu-id="09470-128">Application Insights diagnosztika Telemetria</span><span class="sxs-lookup"><span data-stu-id="09470-128">Application Insights Diagnostics Telemetry</span></span>
<span data-ttu-id="09470-129">A `DiagnosticsTelemetryModule` magát az Application Insights instrumentation kódban hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="09470-129">The `DiagnosticsTelemetryModule` reports errors in the Application Insights instrumentation code itself.</span></span> <span data-ttu-id="09470-130">Például ha a kód nem fér hozzá a teljesítményszámlálókat, vagy ha egy `ITelemetryInitializer` kivételt jelez.</span><span class="sxs-lookup"><span data-stu-id="09470-130">For example, if the code cannot access performance counters or if an `ITelemetryInitializer` throws an exception.</span></span> <span data-ttu-id="09470-131">Ez a modul követik – nyomkövetési telemetria megjelenik a [diagnosztikai keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="09470-131">Trace telemetry tracked by this module appears in the [Diagnostic Search][diagnostic].</span></span> <span data-ttu-id="09470-132">Diagnosztikai adatokat küld a dc.services.vsallin.net.</span><span class="sxs-lookup"><span data-stu-id="09470-132">Sends diagnostic data to dc.services.vsallin.net.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* <span data-ttu-id="09470-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="09470-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="09470-134">Ha csak telepíteni ezt a csomagot, az ApplicationInsights.config fájl nem automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="09470-134">If you only install this package, the ApplicationInsights.config file is not automatically created.</span></span>

### <a name="developer-mode"></a><span data-ttu-id="09470-135">Fejlesztői mód</span><span class="sxs-lookup"><span data-stu-id="09470-135">Developer Mode</span></span>
<span data-ttu-id="09470-136">`DeveloperModeWithDebuggerAttachedTelemetryModule`arra kényszeríti az Application Insights `TelemetryChannel` küldendő adatok azonnal, több telemetriai tétel egyszerre, ha van csatolva hibakereső az alkalmazás folyamatának.</span><span class="sxs-lookup"><span data-stu-id="09470-136">`DeveloperModeWithDebuggerAttachedTelemetryModule` forces the Application Insights `TelemetryChannel` to send data immediately, one telemetry item at a time, when a debugger is attached to the application process.</span></span> <span data-ttu-id="09470-137">Ez csökkenti a közötti idő, amikor az alkalmazás telemetriai nyomon követi, és úgy tűnik, az Application Insights portál.</span><span class="sxs-lookup"><span data-stu-id="09470-137">This reduces the amount of time between the moment when your application tracks telemetry and when it appears on the Application Insights portal.</span></span> <span data-ttu-id="09470-138">A Processzor- és hálózati sávszélesség jelentős terhelést okoz.</span><span class="sxs-lookup"><span data-stu-id="09470-138">It causes significant overhead in CPU and network bandwidth.</span></span>

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* <span data-ttu-id="09470-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="09470-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package</span></span>

### <a name="web-request-tracking"></a><span data-ttu-id="09470-140">Webes kérelem nyomon követése</span><span class="sxs-lookup"><span data-stu-id="09470-140">Web Request Tracking</span></span>
<span data-ttu-id="09470-141">Jelentések a [időt és az eredmény válaszkód](app-insights-asp-net.md) a HTTP-kérések.</span><span class="sxs-lookup"><span data-stu-id="09470-141">Reports the [response time and result code](app-insights-asp-net.md) of HTTP requests.</span></span>

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* <span data-ttu-id="09470-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="09470-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>

### <a name="exception-tracking"></a><span data-ttu-id="09470-143">Kivétel követése</span><span class="sxs-lookup"><span data-stu-id="09470-143">Exception tracking</span></span>
<span data-ttu-id="09470-144">`ExceptionTrackingTelemetryModule`a webalkalmazás kezeletlen kivételek nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="09470-144">`ExceptionTrackingTelemetryModule` tracks unhandled exceptions in your web app.</span></span> <span data-ttu-id="09470-145">Lásd: [hibákat és kivételeket][exceptions].</span><span class="sxs-lookup"><span data-stu-id="09470-145">See [Failures and exceptions][exceptions].</span></span>

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* <span data-ttu-id="09470-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="09470-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>
* <span data-ttu-id="09470-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-számok [feladat kivételek észrevétlen](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span><span class="sxs-lookup"><span data-stu-id="09470-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` - tracks [unobserved task exceptions](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span></span>
* <span data-ttu-id="09470-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-a feldolgozói szerepköröket, a központi windows-szolgáltatások és a konzol alkalmazások nem kezelt kivételek nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="09470-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` - tracks unhandled exceptions for worker roles, windows services, and console applications.</span></span>
* <span data-ttu-id="09470-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="09470-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package.</span></span>

### <a name="eventsource-tracking"></a><span data-ttu-id="09470-150">EventSource nyomon követése</span><span class="sxs-lookup"><span data-stu-id="09470-150">EventSource Tracking</span></span>
<span data-ttu-id="09470-151">`EventSourceTelemetryModule`az Application Insights nyomkövetési adatokat, küldendő EventSource események konfigurálását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="09470-151">`EventSourceTelemetryModule` allows you to configure EventSource events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="09470-152">Információ az EventSource nyomon követés: [használatával EventSource események](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span><span class="sxs-lookup"><span data-stu-id="09470-152">For information on tracking EventSource events, see [Using EventSource Events](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span></span>

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [<span data-ttu-id="09470-153">Microsoft.ApplicationInsights.EventSourceListener</span><span class="sxs-lookup"><span data-stu-id="09470-153">Microsoft.ApplicationInsights.EventSourceListener</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a><span data-ttu-id="09470-154">ETW-események követése</span><span class="sxs-lookup"><span data-stu-id="09470-154">ETW Event Tracking</span></span>
<span data-ttu-id="09470-155">`EtwCollectorTelemetryModule`az Application Insights nyomkövetési adatokat, küldendő ETW-szolgáltatóktól származó események konfigurálását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="09470-155">`EtwCollectorTelemetryModule` allows you to configure events from ETW providers to be sent to Application Insights as traces.</span></span> <span data-ttu-id="09470-156">Információ az ETW-események nyomon követése: [ETW-esemény használatával](app-insights-asp-net-trace-logs.md#using-etw-events).</span><span class="sxs-lookup"><span data-stu-id="09470-156">For information on tracking ETW events, see [Using ETW Events](app-insights-asp-net-trace-logs.md#using-etw-events).</span></span>

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [<span data-ttu-id="09470-157">Microsoft.ApplicationInsights.EtwCollector</span><span class="sxs-lookup"><span data-stu-id="09470-157">Microsoft.ApplicationInsights.EtwCollector</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a><span data-ttu-id="09470-158">Microsoft.ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="09470-158">Microsoft.ApplicationInsights</span></span>
<span data-ttu-id="09470-159">A Microsoft.ApplicationInsights csomag biztosítja a [API alapvető](https://msdn.microsoft.com/library/mt420197.aspx) SDK.</span><span class="sxs-lookup"><span data-stu-id="09470-159">The Microsoft.ApplicationInsights package provides the [core API](https://msdn.microsoft.com/library/mt420197.aspx) of the SDK.</span></span> <span data-ttu-id="09470-160">Ezt az egyéb telemetriai modulok használják, és is [segítségével határozza meg a saját telemetriai](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="09470-160">The other telemetry modules use this, and you can also [use it to define your own telemetry](app-insights-api-custom-events-metrics.md).</span></span>

* <span data-ttu-id="09470-161">Nem találhatók bejegyzések applicationinsights.config.</span><span class="sxs-lookup"><span data-stu-id="09470-161">No entry in ApplicationInsights.config.</span></span>
* <span data-ttu-id="09470-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="09470-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="09470-163">Ha most telepíti a NuGet, nem .config fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="09470-163">If you just install this NuGet, no .config file is generated.</span></span>

## <a name="telemetry-channel"></a><span data-ttu-id="09470-164">Telemetria csatorna</span><span class="sxs-lookup"><span data-stu-id="09470-164">Telemetry Channel</span></span>
<span data-ttu-id="09470-165">A telemetria-csatorna a pufferelés és az Application Insights szolgáltatáshoz telemetriai adatok továbbítása kezeli.</span><span class="sxs-lookup"><span data-stu-id="09470-165">The telemetry channel manages buffering and transmission of telemetry to the Application Insights service.</span></span>

* <span data-ttu-id="09470-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`az alapértelmezett csatornán szolgáltatások van.</span><span class="sxs-lookup"><span data-stu-id="09470-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` is the default channel for services.</span></span> <span data-ttu-id="09470-167">Adatok a memóriában puffereli azt.</span><span class="sxs-lookup"><span data-stu-id="09470-167">It buffers data in memory.</span></span>
* <span data-ttu-id="09470-168">`Microsoft.ApplicationInsights.PersistenceChannel`a konzol alkalmazások alternatív van.</span><span class="sxs-lookup"><span data-stu-id="09470-168">`Microsoft.ApplicationInsights.PersistenceChannel` is an alternative for console applications.</span></span> <span data-ttu-id="09470-169">Azt is unflushed adatok mentése az állandó tároló az alkalmazás bezárása után, és elküldi azt az alkalmazás indításakor újra.</span><span class="sxs-lookup"><span data-stu-id="09470-169">It can save any unflushed data to persistent storage when your app closes down, and will send it when the app starts again.</span></span>

## <a name="telemetry-initializers-aspnet"></a><span data-ttu-id="09470-170">Telemetria inicializálók (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="09470-170">Telemetry Initializers (ASP.NET)</span></span>
<span data-ttu-id="09470-171">Telemetria inicializálók tulajdonságainak környezetben küldött telemetriai minden elem mellett.</span><span class="sxs-lookup"><span data-stu-id="09470-171">Telemetry initializers set context properties that are sent along with every item of telemetry.</span></span>

<span data-ttu-id="09470-172">Is [saját inicializálók írási](app-insights-api-filtering-sampling.md#add-properties) környezet tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="09470-172">You can [write your own initializers](app-insights-api-filtering-sampling.md#add-properties) to set context properties.</span></span>

<span data-ttu-id="09470-173">A szabványos inicializálók be vannak állítva vagy a Web vagy WindowsServer NuGet-csomagot:</span><span class="sxs-lookup"><span data-stu-id="09470-173">The standard initializers are all set either by the Web or WindowsServer NuGet packages:</span></span>

* <span data-ttu-id="09470-174">`AccountIdTelemetryInitializer`Beállítja a AccountId tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="09470-174">`AccountIdTelemetryInitializer` sets the AccountId property.</span></span>
* <span data-ttu-id="09470-175">`AuthenticatedUserIdTelemetryInitializer`a AuthenticatedUserId tulajdonság beállítása a JavaScript SDK által beállított.</span><span class="sxs-lookup"><span data-stu-id="09470-175">`AuthenticatedUserIdTelemetryInitializer` sets the AuthenticatedUserId property as set by the JavaScript SDK.</span></span>
* <span data-ttu-id="09470-176">`AzureRoleEnvironmentTelemetryInitializer`frissítések a `RoleName` és `RoleInstance` tulajdonságainak a `Device` környezet az Azure futtatókörnyezetben kinyert adatokkal az összes telemetriai adat.</span><span class="sxs-lookup"><span data-stu-id="09470-176">`AzureRoleEnvironmentTelemetryInitializer` updates the `RoleName` and `RoleInstance` properties of the `Device` context for all telemetry items with information extracted from the Azure runtime environment.</span></span>
* <span data-ttu-id="09470-177">`BuildInfoConfigComponentVersionTelemetryInitializer`frissítések a `Version` tulajdonsága a `Component` kinyert értékét az összes telemetriai környezetben a `BuildInfo.config` MS Build által.</span><span class="sxs-lookup"><span data-stu-id="09470-177">`BuildInfoConfigComponentVersionTelemetryInitializer` updates the `Version` property of the `Component` context for all telemetry items with the value extracted from the `BuildInfo.config` file produced by MS Build.</span></span>
* <span data-ttu-id="09470-178">`ClientIpHeaderTelemetryInitializer`frissítések `Ip` tulajdonsága a `Location` környezetben az összes telemetriai elem alapján a `X-Forwarded-For` a kérelem HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="09470-178">`ClientIpHeaderTelemetryInitializer` updates `Ip` property of the `Location` context of all telemetry items based on the `X-Forwarded-For` HTTP header of the request.</span></span>
* <span data-ttu-id="09470-179">`DeviceTelemetryInitializer`következő tulajdonságait a `Device` környezetben az összes telemetriai adat.</span><span class="sxs-lookup"><span data-stu-id="09470-179">`DeviceTelemetryInitializer` updates the following properties of the `Device` context for all telemetry items.</span></span>
  * <span data-ttu-id="09470-180">`Type`"PC" értékre van állítva</span><span class="sxs-lookup"><span data-stu-id="09470-180">`Type` is set to "PC"</span></span>
  * <span data-ttu-id="09470-181">`Id`értéke a számítógép tartománynevét, amelyen fut a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="09470-181">`Id` is set to the domain name of the computer where the web application is running.</span></span>
  * <span data-ttu-id="09470-182">`OemName`kinyert értékre van állítva a `Win32_ComputerSystem.Manufacturer` mezőben a WMI segítségével.</span><span class="sxs-lookup"><span data-stu-id="09470-182">`OemName` is set to the value extracted from the `Win32_ComputerSystem.Manufacturer` field using WMI.</span></span>
  * <span data-ttu-id="09470-183">`Model`kinyert értékre van állítva a `Win32_ComputerSystem.Model` mezőben a WMI segítségével.</span><span class="sxs-lookup"><span data-stu-id="09470-183">`Model` is set to the value extracted from the `Win32_ComputerSystem.Model` field using WMI.</span></span>
  * <span data-ttu-id="09470-184">`NetworkType`kinyert értékre van állítva a `NetworkInterface`.</span><span class="sxs-lookup"><span data-stu-id="09470-184">`NetworkType` is set to the value extracted from the `NetworkInterface`.</span></span>
  * <span data-ttu-id="09470-185">`Language`a névre van beállítva a `CurrentCulture`.</span><span class="sxs-lookup"><span data-stu-id="09470-185">`Language` is set to the name of the `CurrentCulture`.</span></span>
* <span data-ttu-id="09470-186">`DomainNameRoleInstanceTelemetryInitializer`frissítések a `RoleInstance` tulajdonsága a `Device` az összes telemetriai adatokat a tartomány nevét a számítógép, amelyen a webalkalmazás fut a környezetben.</span><span class="sxs-lookup"><span data-stu-id="09470-186">`DomainNameRoleInstanceTelemetryInitializer` updates the `RoleInstance` property of the `Device` context for all telemetry items with the domain name of the computer where the web application is running.</span></span>
* <span data-ttu-id="09470-187">`OperationNameTelemetryInitializer`frissítések a `Name` tulajdonsága a `RequestTelemetry` és a `Name` tulajdonsága a `Operation` összes telemetriai elem kontextusában a HTTP-metódus, valamint a ASP.NET MVC-vezérlő és feldolgozni a kérelmet meghívott művelet neve alapján.</span><span class="sxs-lookup"><span data-stu-id="09470-187">`OperationNameTelemetryInitializer` updates the `Name` property of the `RequestTelemetry` and the `Name` property of the `Operation` context of all telemetry items based on the HTTP method, as well as names of ASP.NET MVC controller and action invoked to process the request.</span></span>
* <span data-ttu-id="09470-188">`OperationIdTelemetryInitializer`vagy `OperationCorrelationTelemetryInitializer` frissítések a `Operation.Id` context tulajdonság az összes telemetriai elem nyomon követheti az automatikusan létrehozott kérelem kezelése közben `RequestTelemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="09470-188">`OperationIdTelemetryInitializer` or `OperationCorrelationTelemetryInitializer` updates the `Operation.Id` context property of all telemetry items tracked while handling a request with the automatically generated `RequestTelemetry.Id`.</span></span>
* <span data-ttu-id="09470-189">`SessionTelemetryInitializer`frissítések a `Id` tulajdonsága a `Session` kinyert érték az összes telemetriai környezetben a `ai_session` cookie-k jönnek létre a felhasználó böngészőben futó ApplicationInsights JavaScript instrumentation kóddal.</span><span class="sxs-lookup"><span data-stu-id="09470-189">`SessionTelemetryInitializer` updates the `Id` property of the `Session` context for all telemetry items with value extracted from the `ai_session` cookie generated by the ApplicationInsights JavaScript instrumentation code running in the user's browser.</span></span>
* <span data-ttu-id="09470-190">`SyntheticTelemetryInitializer`vagy `SyntheticUserAgentTelemetryInitializer` frissítések a `User`, `Session` és `Operation` összes telemetriai elemek környezetek tulajdonságainak nyomon követ, egy kérelem egy szintetikus forrásból kezelésekor, például a rendelkezésre állási tesztelése, vagy végezzen keresést a motor botot.</span><span class="sxs-lookup"><span data-stu-id="09470-190">`SyntheticTelemetryInitializer` or `SyntheticUserAgentTelemetryInitializer` updates the `User`, `Session` and `Operation` contexts properties of all telemetry items tracked when handling a request from a synthetic source, such as an availability test or search engine bot.</span></span> <span data-ttu-id="09470-191">Alapértelmezés szerint [Metrikaböngésző](app-insights-metrics-explorer.md) szintetikus telemetriai adatok nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="09470-191">By default, [Metrics Explorer](app-insights-metrics-explorer.md) does not display synthetic telemetry.</span></span>

    <span data-ttu-id="09470-192">A `<Filters>` azonosítása a kérelem tulajdonságok beállítása.</span><span class="sxs-lookup"><span data-stu-id="09470-192">The `<Filters>` set identifying properties of the requests.</span></span>
* <span data-ttu-id="09470-193">`UserAgentTelemetryInitializer`frissítések a `UserAgent` tulajdonsága a `User` környezetben az összes telemetriai elem alapján a `User-Agent` a kérelem HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="09470-193">`UserAgentTelemetryInitializer` updates the `UserAgent` property of the `User` context of all telemetry items based on the `User-Agent` HTTP header of the request.</span></span>
* <span data-ttu-id="09470-194">`UserTelemetryInitializer`frissítések a `Id` és `AcquisitionDate` tulajdonságainak `User` kinyert értékekkel az összes telemetriai környezetben a `ai_user` cookie-k az Application Insights JavaScript instrumentation kódot a felhasználó böngészőben futó állítja elő.</span><span class="sxs-lookup"><span data-stu-id="09470-194">`UserTelemetryInitializer` updates the `Id` and `AcquisitionDate` properties of `User` context for all telemetry items with values extracted from the `ai_user` cookie generated by the Application Insights JavaScript instrumentation code running in the user's browser.</span></span>
* <span data-ttu-id="09470-195">`WebTestTelemetryInitializer`a felhasználói azonosítóját, a munkamenet-azonosító és a szintetikus adatforrások tulajdonságainak beállítása a HTTP-kérelmek származó [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="09470-195">`WebTestTelemetryInitializer` sets the user id, session id and synthetic source properties for HTTP requests that come from [availability tests](app-insights-monitor-web-app-availability.md).</span></span>
  <span data-ttu-id="09470-196">A `<Filters>` azonosítása a kérelem tulajdonságok beállítása.</span><span class="sxs-lookup"><span data-stu-id="09470-196">The `<Filters>` set identifying properties of the requests.</span></span>

<span data-ttu-id="09470-197">A Service Fabric-beli .NET-alkalmazásokban, megadhatja a `Microsoft.ApplicationInsights.ServiceFabric` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="09470-197">For .NET applications running in Service Fabric, you can include the `Microsoft.ApplicationInsights.ServiceFabric` NuGet package.</span></span> <span data-ttu-id="09470-198">Ez a csomag tartalmaz egy `FabricTelemetryInitializer`, amely a Service Fabric további tulajdonságokkal bővít telemetriai elemek.</span><span class="sxs-lookup"><span data-stu-id="09470-198">This package includes a `FabricTelemetryInitializer`, which adds Service Fabric properties to telemetry items.</span></span> <span data-ttu-id="09470-199">További információkért lásd: a [GitHub-oldalon](https://go.microsoft.com/fwlink/?linkid=848457) vett fel a NuGet csomag tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="09470-199">For more information, see the [GitHub page](https://go.microsoft.com/fwlink/?linkid=848457) about the properties added by this NuGet package.</span></span>

## <a name="telemetry-processors-aspnet"></a><span data-ttu-id="09470-200">Telemetria processzor (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="09470-200">Telemetry Processors (ASP.NET)</span></span>
<span data-ttu-id="09470-201">Telemetria processzorok szűréséhez, és minden telemetriai cikk módosítása, közvetlenül az SDK-ból a portálon való továbbítás előtt.</span><span class="sxs-lookup"><span data-stu-id="09470-201">Telemetry processors can filter and modify each telemetry item just before it is sent from the SDK to the portal.</span></span>

<span data-ttu-id="09470-202">Is [saját telemetriai processzorok írási](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="09470-202">You can [write your own telemetry processors](app-insights-api-filtering-sampling.md#filtering).</span></span>

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a><span data-ttu-id="09470-203">Adaptív mintavételi telemetriai processzor (a 2.0.0-beta3)</span><span class="sxs-lookup"><span data-stu-id="09470-203">Adaptive sampling telemetry processor (from 2.0.0-beta3)</span></span>
<span data-ttu-id="09470-204">Ez e beállítás alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="09470-204">This is enabled by default.</span></span> <span data-ttu-id="09470-205">Az alkalmazás nagy mennyiségű telemetriai adatokat küld, ha a processzor eltávolítja bizonyos része.</span><span class="sxs-lookup"><span data-stu-id="09470-205">If your app sends a lot of telemetry, this processor removes some of it.</span></span>

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="09470-206">A paraméter a cél eléréséhez, próbálja meg az algoritmus biztosít.</span><span class="sxs-lookup"><span data-stu-id="09470-206">The parameter provides the target that the algorithm tries to achieve.</span></span> <span data-ttu-id="09470-207">Az SDK-példányokhoz egymástól függetlenül, működik, így ha a kiszolgáló egy fürt több gépek, a telemetriai adatok tényleges mennyiségét és ennek megfelelően kell-e.</span><span class="sxs-lookup"><span data-stu-id="09470-207">Each instance of the SDK works independently, so if your server is a cluster of several machines, the actual volume of telemetry will be multiplied accordingly.</span></span>

<span data-ttu-id="09470-208">[További információ a mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="09470-208">[Learn more about sampling](app-insights-sampling.md).</span></span>

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a><span data-ttu-id="09470-209">Rögzített mintavételi telemetriai processzor (a 2.0.0-beta1)</span><span class="sxs-lookup"><span data-stu-id="09470-209">Fixed-rate sampling telemetry processor (from 2.0.0-beta1)</span></span>
<span data-ttu-id="09470-210">Szerepel továbbá egy szabványos [telemetriai processzor mintavételi](app-insights-api-filtering-sampling.md) (a 2.0.1):</span><span class="sxs-lookup"><span data-stu-id="09470-210">There is also a standard [sampling telemetry processor](app-insights-api-filtering-sampling.md) (from 2.0.1):</span></span>

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close to 100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a><span data-ttu-id="09470-211">A csatornaparaméterek (Java)</span><span class="sxs-lookup"><span data-stu-id="09470-211">Channel parameters (Java)</span></span>
<span data-ttu-id="09470-212">Ezek a paraméterek befolyásolják, hogyan a Java SDK kell tárolni, valamint a telemetriai adatait, amely összegyűjti az.</span><span class="sxs-lookup"><span data-stu-id="09470-212">These parameters affect how the Java SDK should store and flush the telemetry data that it collects.</span></span>

#### <a name="maxtelemetrybuffercapacity"></a><span data-ttu-id="09470-213">MaxTelemetryBufferCapacity</span><span class="sxs-lookup"><span data-stu-id="09470-213">MaxTelemetryBufferCapacity</span></span>
<span data-ttu-id="09470-214">A telemetriai adatok elemek száma, amelyek az SDK-val memórián belüli tároló tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="09470-214">The number of telemetry items that can be stored in the SDK's in-memory storage.</span></span> <span data-ttu-id="09470-215">A számnak az elérésekor, a telemetria puffer ki van ürítve, – ez azt jelenti, hogy a telemetriai adatok elemek az Application Insights-kiszolgálónak küldött.</span><span class="sxs-lookup"><span data-stu-id="09470-215">When this number is reached, the telemetry buffer is flushed - that is, the telemetry items are sent to the Application Insights server.</span></span>

* <span data-ttu-id="09470-216">Minimum: 1</span><span class="sxs-lookup"><span data-stu-id="09470-216">Min: 1</span></span>
* <span data-ttu-id="09470-217">Maximális: 1000</span><span class="sxs-lookup"><span data-stu-id="09470-217">Max: 1000</span></span>
* <span data-ttu-id="09470-218">Alapértelmezett: 500</span><span class="sxs-lookup"><span data-stu-id="09470-218">Default: 500</span></span>

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a><span data-ttu-id="09470-219">FlushIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="09470-219">FlushIntervalInSeconds</span></span>
<span data-ttu-id="09470-220">Meghatározza, hogy milyen gyakran az adatok tárolása a memóriában tárolt kell kiürítése (Application insights szolgáltatásnak elküldött).</span><span class="sxs-lookup"><span data-stu-id="09470-220">Determines how often the data that is stored in the in-memory storage should be flushed (sent to Application Insights).</span></span>

* <span data-ttu-id="09470-221">Minimum: 1</span><span class="sxs-lookup"><span data-stu-id="09470-221">Min: 1</span></span>
* <span data-ttu-id="09470-222">Maximális: 300</span><span class="sxs-lookup"><span data-stu-id="09470-222">Max: 300</span></span>
* <span data-ttu-id="09470-223">Alapértelmezett: 5</span><span class="sxs-lookup"><span data-stu-id="09470-223">Default: 5</span></span>

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a><span data-ttu-id="09470-224">MaxTransmissionStorageCapacityInMB</span><span class="sxs-lookup"><span data-stu-id="09470-224">MaxTransmissionStorageCapacityInMB</span></span>
<span data-ttu-id="09470-225">Meghatározza a maximális mérete (MB), amely számára engedélyezett az állandó tároló a helyi lemezen való.</span><span class="sxs-lookup"><span data-stu-id="09470-225">Determines the maximum size in MB that is allotted to the persistent storage on the local disk.</span></span> <span data-ttu-id="09470-226">Ez a tároló nem sikerült továbbítani az Application Insights végpont tárolásakor telemetriai elemekre szolgál.</span><span class="sxs-lookup"><span data-stu-id="09470-226">This storage is used for persisting telemetry items that failed to be transmitted to the Application Insights endpoint.</span></span> <span data-ttu-id="09470-227">A tároló mérete teljesülésekor új telemetriai elemek elvesznek.</span><span class="sxs-lookup"><span data-stu-id="09470-227">When the storage size has been met, new telemetry items will be discarded.</span></span>

* <span data-ttu-id="09470-228">Minimum: 1</span><span class="sxs-lookup"><span data-stu-id="09470-228">Min: 1</span></span>
* <span data-ttu-id="09470-229">Maximum: 100</span><span class="sxs-lookup"><span data-stu-id="09470-229">Max: 100</span></span>
* <span data-ttu-id="09470-230">Alapértelmezett: 10</span><span class="sxs-lookup"><span data-stu-id="09470-230">Default: 10</span></span>

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a><span data-ttu-id="09470-231">InstrumentationKey</span><span class="sxs-lookup"><span data-stu-id="09470-231">InstrumentationKey</span></span>
<span data-ttu-id="09470-232">Ez határozza meg az Application Insights-erőforrást, amelyen az adatok megjelenik.</span><span class="sxs-lookup"><span data-stu-id="09470-232">This determines the Application Insights resource in which your data appears.</span></span> <span data-ttu-id="09470-233">Általában létrehozhat egy különálló erőforrás külön kulccsal, minden, az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="09470-233">Typically you create a separate resource, with a separate key, for each of your applications.</span></span>

<span data-ttu-id="09470-234">Ha be szeretné állítani a kulcs dinamikusan – például ha az eredmények elküldi a különböző erőforrások – az alkalmazásból hagyja ki ezt a kulcsot a konfigurációs fájlból, és helyette állítsa a kódban.</span><span class="sxs-lookup"><span data-stu-id="09470-234">If you want to set the key dynamically - for example if you want to send results from your application to different resources - you can omit the key from the configuration file, and set it in code instead.</span></span>

<span data-ttu-id="09470-235">A kulcs beállítása TelemetryClient összes példánya esetén, beleértve a szabványos telemetriai modulok, kulcsát állítsa a TelemetryConfiguration.Active a.</span><span class="sxs-lookup"><span data-stu-id="09470-235">To set the key for all instances of TelemetryClient, including standard telemetry modules, set the key in TelemetryConfiguration.Active.</span></span> <span data-ttu-id="09470-236">Egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="09470-236">Do this in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

<span data-ttu-id="09470-237">Ha csak egy meghatározott események küldése egy másik erőforráscsoportban, a kulcs az egy adott TelemetryClient állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="09470-237">If you just want to send a specific set of events to a different resource, you can set the key for a specific TelemetryClient:</span></span>

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

<span data-ttu-id="09470-238">Egy új kulcs beszerzése [hozzon létre egy új erőforrást az Application Insights portáljáról][new].</span><span class="sxs-lookup"><span data-stu-id="09470-238">To get a new key, [create a new resource in the Application Insights portal][new].</span></span>

## <a name="next-steps"></a><span data-ttu-id="09470-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09470-239">Next steps</span></span>
<span data-ttu-id="09470-240">[További tudnivalók az API-t][api].</span><span class="sxs-lookup"><span data-stu-id="09470-240">[Learn more about the API][api].</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

---
title: aaaApplicationInsights.config referencia - Azure |} Microsoft Docs
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
ms.openlocfilehash: 76cb11349d87dfc508ec8b1c454259a0b079c48a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-hello-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a><span data-ttu-id="e9d85-103">Application Insights SDK hello konfigurálása az ApplicationInsights.config vagy .xml</span><span class="sxs-lookup"><span data-stu-id="e9d85-103">Configuring hello Application Insights SDK with ApplicationInsights.config or .xml</span></span>
<span data-ttu-id="e9d85-104">hello Application Insights .NET SDK NuGet-csomagok számos áll.</span><span class="sxs-lookup"><span data-stu-id="e9d85-104">hello Application Insights .NET SDK consists of a number of NuGet packages.</span></span> <span data-ttu-id="e9d85-105">A [core csomag](http://www.nuget.org/packages/Microsoft.ApplicationInsights) hello Application Insights telemetria küldi hello API biztosít.</span><span class="sxs-lookup"><span data-stu-id="e9d85-105">The [core package](http://www.nuget.org/packages/Microsoft.ApplicationInsights) provides hello API for sending telemetry to hello Application Insights.</span></span> <span data-ttu-id="e9d85-106">[További csomagok](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) adja meg a telemetriai adatok *modulok* és *inicializálók* automatikusan nyomon követése a telemetriai adatok az alkalmazás és a környezetben.</span><span class="sxs-lookup"><span data-stu-id="e9d85-106">[Additional packages](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) provide telemetry *modules* and *initializers* for automatically tracking telemetry from your application and its context.</span></span> <span data-ttu-id="e9d85-107">Hello konfigurációs fájl módosításával engedélyezze vagy tiltsa le a telemetria-modulokat és az inicializálók, és némelyikük paramétereinek megadása.</span><span class="sxs-lookup"><span data-stu-id="e9d85-107">By adjusting hello configuration file, you can enable or disable telemetry modules and initializers, and set parameters for some of them.</span></span>

<span data-ttu-id="e9d85-108">hello konfigurációs fájl neve `ApplicationInsights.config` vagy `ApplicationInsights.xml`, attól függően, az alkalmazás hello típusú.</span><span class="sxs-lookup"><span data-stu-id="e9d85-108">hello configuration file is named `ApplicationInsights.config` or `ApplicationInsights.xml`, depending on hello type of your application.</span></span> <span data-ttu-id="e9d85-109">Az automatikusan bekerül az tooyour projekt mikor meg [hello SDK legtöbb változatának telepítése][start].</span><span class="sxs-lookup"><span data-stu-id="e9d85-109">It is automatically added tooyour project when you [install most versions of hello SDK][start].</span></span> <span data-ttu-id="e9d85-110">Webalkalmazás tooa által is megjelenik [állapotfigyelő az IIS-kiszolgáló][redfield], vagy hello Appplication Insights kiválasztásakor [bővítmény, az Azure webhelyén vagy a virtuális gép](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="e9d85-110">It is also added tooa web app by [Status Monitor on an IIS server][redfield], or when you select hello Appplication Insights [extension for an Azure website or VM](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="e9d85-111">Nem áll rendelkezésre egy egyenértékű fájl toocontrol hello [SDK-t egy weblap][client].</span><span class="sxs-lookup"><span data-stu-id="e9d85-111">There isn't an equivalent file toocontrol hello [SDK in a web page][client].</span></span>

<span data-ttu-id="e9d85-112">Ez a dokumentum ismerteti a hello szakaszok látható hello konfigurációs fájlban, hogyan szabályozza azok a hello összetevői hello SDK-t, és melyik NuGet-csomagok betölteni az összetevőket.</span><span class="sxs-lookup"><span data-stu-id="e9d85-112">This document describes hello sections you see in hello configuration file, how they control hello components of hello SDK, and which NuGet packages load those components.</span></span>

## <a name="telemetry-modules-aspnet"></a><span data-ttu-id="e9d85-113">Telemetria modulok (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e9d85-113">Telemetry Modules (ASP.NET)</span></span>
<span data-ttu-id="e9d85-114">Minden telemetriai modul egy adott típusú adatokat gyűjt, és hello core API toosend hello adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="e9d85-114">Each telemetry module collects a specific type of data and uses hello core API toosend hello data.</span></span> <span data-ttu-id="e9d85-115">hello modulok különböző NuGet-csomagok, amelyek hello szükséges sorok toohello .config fájl is telepíti.</span><span class="sxs-lookup"><span data-stu-id="e9d85-115">hello modules are installed by different NuGet packages, which also add hello required lines toohello .config file.</span></span>

<span data-ttu-id="e9d85-116">Van a csomópont minden modul hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="e9d85-116">There's a node in hello configuration file for each module.</span></span> <span data-ttu-id="e9d85-117">toodisable egy modul hello csomópont törlése, vagy el megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="e9d85-117">toodisable a module, delete hello node or comment it out.</span></span>

### <a name="dependency-tracking"></a><span data-ttu-id="e9d85-118">A függőségi nyomon követése</span><span class="sxs-lookup"><span data-stu-id="e9d85-118">Dependency Tracking</span></span>
<span data-ttu-id="e9d85-119">[Követés függőségi](app-insights-asp-net-dependencies.md) az alkalmazás lehetővé teszi a toodatabases és külső szolgáltatások adatbázisok hívások vonatkozó telemetriai adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="e9d85-119">[Dependency tracking](app-insights-asp-net-dependencies.md) collects telemetry about calls your app makes toodatabases and external services and databases.</span></span> <span data-ttu-id="e9d85-120">tooallow a modul toowork az IIS-kiszolgálót, kell túl[Állapotmonitor telepítése][redfield].</span><span class="sxs-lookup"><span data-stu-id="e9d85-120">tooallow this module toowork in an IIS server, you need too[install Status Monitor][redfield].</span></span> <span data-ttu-id="e9d85-121">toouse az Azure-webalkalmazásokban vagy a virtuális gépek, [hello Application Insights-bővítmény kiválasztása](app-insights-azure-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="e9d85-121">toouse it in Azure web apps or VMs, [select hello Application Insights extension](app-insights-azure-web-apps.md).</span></span>

<span data-ttu-id="e9d85-122">Követés kód hello segítségével a saját függőségi is írhat [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span><span class="sxs-lookup"><span data-stu-id="e9d85-122">You can also write your own dependency tracking code using hello [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).</span></span>

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* <span data-ttu-id="e9d85-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-123">[Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet package.</span></span>

### <a name="performance-collector"></a><span data-ttu-id="e9d85-124">Teljesítmény-gyűjtő</span><span class="sxs-lookup"><span data-stu-id="e9d85-124">Performance collector</span></span>
<span data-ttu-id="e9d85-125">[Gyűjti a rendszerteljesítmény-számlálók](app-insights-performance-counters.md) például CPU és memória- és hálózati betöltése az IIS telepítése.</span><span class="sxs-lookup"><span data-stu-id="e9d85-125">[Collects system performance counters](app-insights-performance-counters.md) such as CPU, memory and network load from IIS installations.</span></span> <span data-ttu-id="e9d85-126">Megadhatja, hogy mely számlálók toocollect, beleértve a saját kezűleg beállított teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="e9d85-126">You can specify which counters toocollect, including performance counters you have set up yourself.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* <span data-ttu-id="e9d85-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-127">[Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet package.</span></span>

### <a name="application-insights-diagnostics-telemetry"></a><span data-ttu-id="e9d85-128">Application Insights diagnosztika Telemetria</span><span class="sxs-lookup"><span data-stu-id="e9d85-128">Application Insights Diagnostics Telemetry</span></span>
<span data-ttu-id="e9d85-129">Hello `DiagnosticsTelemetryModule` az Application Insights instrumentation forráskód hello hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="e9d85-129">hello `DiagnosticsTelemetryModule` reports errors in hello Application Insights instrumentation code itself.</span></span> <span data-ttu-id="e9d85-130">Például ha hello kódja nem tudja elérni a teljesítményszámlálókat, vagy ha egy `ITelemetryInitializer` kivételt jelez.</span><span class="sxs-lookup"><span data-stu-id="e9d85-130">For example, if hello code cannot access performance counters or if an `ITelemetryInitializer` throws an exception.</span></span> <span data-ttu-id="e9d85-131">Ez a modul követik – nyomkövetési telemetria hello megjelenik [diagnosztikai keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="e9d85-131">Trace telemetry tracked by this module appears in hello [Diagnostic Search][diagnostic].</span></span> <span data-ttu-id="e9d85-132">Diagnosztikai adatok toodc.services.vsallin.net küld.</span><span class="sxs-lookup"><span data-stu-id="e9d85-132">Sends diagnostic data toodc.services.vsallin.net.</span></span>

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* <span data-ttu-id="e9d85-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-133">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="e9d85-134">Ha csak telepíteni ezt a csomagot, hello ApplicationInsights.config fájl nem automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="e9d85-134">If you only install this package, hello ApplicationInsights.config file is not automatically created.</span></span>

### <a name="developer-mode"></a><span data-ttu-id="e9d85-135">Fejlesztői mód</span><span class="sxs-lookup"><span data-stu-id="e9d85-135">Developer Mode</span></span>
<span data-ttu-id="e9d85-136">`DeveloperModeWithDebuggerAttachedTelemetryModule`kényszeríti az Application Insights hello `TelemetryChannel` azonnal, toosend adatok több telemetriai tétel egyszerre, ha hibakereső van csatlakoztatva toohello alkalmazás folyamata.</span><span class="sxs-lookup"><span data-stu-id="e9d85-136">`DeveloperModeWithDebuggerAttachedTelemetryModule` forces hello Application Insights `TelemetryChannel` toosend data immediately, one telemetry item at a time, when a debugger is attached toohello application process.</span></span> <span data-ttu-id="e9d85-137">Ez csökkenti a hello időn közötti hello néhány percet, ha az alkalmazás nyomon követi a telemetriai adatok, illetve ha hello Application Insights portál megjelenik.</span><span class="sxs-lookup"><span data-stu-id="e9d85-137">This reduces hello amount of time between hello moment when your application tracks telemetry and when it appears on hello Application Insights portal.</span></span> <span data-ttu-id="e9d85-138">A Processzor- és hálózati sávszélesség jelentős terhelést okoz.</span><span class="sxs-lookup"><span data-stu-id="e9d85-138">It causes significant overhead in CPU and network bandwidth.</span></span>

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* <span data-ttu-id="e9d85-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="e9d85-139">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package</span></span>

### <a name="web-request-tracking"></a><span data-ttu-id="e9d85-140">Webes kérelem nyomon követése</span><span class="sxs-lookup"><span data-stu-id="e9d85-140">Web Request Tracking</span></span>
<span data-ttu-id="e9d85-141">Jelentések hello [időt és az eredmény válaszkód](app-insights-asp-net.md) a HTTP-kérések.</span><span class="sxs-lookup"><span data-stu-id="e9d85-141">Reports hello [response time and result code](app-insights-asp-net.md) of HTTP requests.</span></span>

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* <span data-ttu-id="e9d85-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="e9d85-142">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>

### <a name="exception-tracking"></a><span data-ttu-id="e9d85-143">Kivétel követése</span><span class="sxs-lookup"><span data-stu-id="e9d85-143">Exception tracking</span></span>
<span data-ttu-id="e9d85-144">`ExceptionTrackingTelemetryModule`a webalkalmazás kezeletlen kivételek nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="e9d85-144">`ExceptionTrackingTelemetryModule` tracks unhandled exceptions in your web app.</span></span> <span data-ttu-id="e9d85-145">Lásd: [hibákat és kivételeket][exceptions].</span><span class="sxs-lookup"><span data-stu-id="e9d85-145">See [Failures and exceptions][exceptions].</span></span>

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* <span data-ttu-id="e9d85-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="e9d85-146">[Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet package</span></span>
* <span data-ttu-id="e9d85-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-számok [feladat kivételek észrevétlen](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span><span class="sxs-lookup"><span data-stu-id="e9d85-147">`Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` - tracks [unobserved task exceptions](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).</span></span>
* <span data-ttu-id="e9d85-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-a feldolgozói szerepköröket, a központi windows-szolgáltatások és a konzol alkalmazások nem kezelt kivételek nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="e9d85-148">`Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` - tracks unhandled exceptions for worker roles, windows services, and console applications.</span></span>
* <span data-ttu-id="e9d85-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-149">[Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet package.</span></span>

### <a name="eventsource-tracking"></a><span data-ttu-id="e9d85-150">EventSource nyomon követése</span><span class="sxs-lookup"><span data-stu-id="e9d85-150">EventSource Tracking</span></span>
<span data-ttu-id="e9d85-151">`EventSourceTelemetryModule`lehetővé teszi a tooconfigure EventSource események toobe tooApplication Insights küldése a nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="e9d85-151">`EventSourceTelemetryModule` allows you tooconfigure EventSource events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="e9d85-152">Információ az EventSource nyomon követés: [használatával EventSource események](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span><span class="sxs-lookup"><span data-stu-id="e9d85-152">For information on tracking EventSource events, see [Using EventSource Events](app-insights-asp-net-trace-logs.md#using-eventsource-events).</span></span>

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [<span data-ttu-id="e9d85-153">Microsoft.ApplicationInsights.EventSourceListener</span><span class="sxs-lookup"><span data-stu-id="e9d85-153">Microsoft.ApplicationInsights.EventSourceListener</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a><span data-ttu-id="e9d85-154">ETW-események követése</span><span class="sxs-lookup"><span data-stu-id="e9d85-154">ETW Event Tracking</span></span>
<span data-ttu-id="e9d85-155">`EtwCollectorTelemetryModule`lehetővé teszi a tooconfigure események az ETW-szolgáltatók toobe tooApplication Insights küldése a nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="e9d85-155">`EtwCollectorTelemetryModule` allows you tooconfigure events from ETW providers toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="e9d85-156">Információ az ETW-események nyomon követése: [ETW-esemény használatával](app-insights-asp-net-trace-logs.md#using-etw-events).</span><span class="sxs-lookup"><span data-stu-id="e9d85-156">For information on tracking ETW events, see [Using ETW Events](app-insights-asp-net-trace-logs.md#using-etw-events).</span></span>

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [<span data-ttu-id="e9d85-157">Microsoft.ApplicationInsights.EtwCollector</span><span class="sxs-lookup"><span data-stu-id="e9d85-157">Microsoft.ApplicationInsights.EtwCollector</span></span>](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a><span data-ttu-id="e9d85-158">Microsoft.ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="e9d85-158">Microsoft.ApplicationInsights</span></span>
<span data-ttu-id="e9d85-159">hello Microsoft.ApplicationInsights csomag biztosít hello [API alapvető](https://msdn.microsoft.com/library/mt420197.aspx) az hello SDK.</span><span class="sxs-lookup"><span data-stu-id="e9d85-159">hello Microsoft.ApplicationInsights package provides hello [core API](https://msdn.microsoft.com/library/mt420197.aspx) of hello SDK.</span></span> <span data-ttu-id="e9d85-160">hello egyéb telemetriai modulok használja, és is [toodefine használni a saját telemetriai](app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="e9d85-160">hello other telemetry modules use this, and you can also [use it toodefine your own telemetry](app-insights-api-custom-events-metrics.md).</span></span>

* <span data-ttu-id="e9d85-161">Nem találhatók bejegyzések applicationinsights.config.</span><span class="sxs-lookup"><span data-stu-id="e9d85-161">No entry in ApplicationInsights.config.</span></span>
* <span data-ttu-id="e9d85-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-162">[Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet package.</span></span> <span data-ttu-id="e9d85-163">Ha most telepíti a NuGet, nem .config fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="e9d85-163">If you just install this NuGet, no .config file is generated.</span></span>

## <a name="telemetry-channel"></a><span data-ttu-id="e9d85-164">Telemetria csatorna</span><span class="sxs-lookup"><span data-stu-id="e9d85-164">Telemetry Channel</span></span>
<span data-ttu-id="e9d85-165">hello telemetriai csatorna pufferelés és továbbítása telemetriai toohello Application Insights szolgáltatás kezeli.</span><span class="sxs-lookup"><span data-stu-id="e9d85-165">hello telemetry channel manages buffering and transmission of telemetry toohello Application Insights service.</span></span>

* <span data-ttu-id="e9d85-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`az alapértelmezett csatornán hello szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e9d85-166">`Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` is hello default channel for services.</span></span> <span data-ttu-id="e9d85-167">Adatok a memóriában puffereli azt.</span><span class="sxs-lookup"><span data-stu-id="e9d85-167">It buffers data in memory.</span></span>
* <span data-ttu-id="e9d85-168">`Microsoft.ApplicationInsights.PersistenceChannel`a konzol alkalmazások alternatív van.</span><span class="sxs-lookup"><span data-stu-id="e9d85-168">`Microsoft.ApplicationInsights.PersistenceChannel` is an alternative for console applications.</span></span> <span data-ttu-id="e9d85-169">Bármely unflushed toopersistent adattárolás azt is mentheti, ha az alkalmazás bezárása után, és visszaküldi azt hello alkalmazás indításakor újra.</span><span class="sxs-lookup"><span data-stu-id="e9d85-169">It can save any unflushed data toopersistent storage when your app closes down, and will send it when hello app starts again.</span></span>

## <a name="telemetry-initializers-aspnet"></a><span data-ttu-id="e9d85-170">Telemetria inicializálók (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e9d85-170">Telemetry Initializers (ASP.NET)</span></span>
<span data-ttu-id="e9d85-171">Telemetria inicializálók tulajdonságainak környezetben küldött telemetriai minden elem mellett.</span><span class="sxs-lookup"><span data-stu-id="e9d85-171">Telemetry initializers set context properties that are sent along with every item of telemetry.</span></span>

<span data-ttu-id="e9d85-172">Is [saját inicializálók írási](app-insights-api-filtering-sampling.md#add-properties) tooset környezeti tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e9d85-172">You can [write your own initializers](app-insights-api-filtering-sampling.md#add-properties) tooset context properties.</span></span>

<span data-ttu-id="e9d85-173">hello szabványos inicializálók be vannak állítva vagy hello Web vagy WindowsServer NuGet-csomagot:</span><span class="sxs-lookup"><span data-stu-id="e9d85-173">hello standard initializers are all set either by hello Web or WindowsServer NuGet packages:</span></span>

* <span data-ttu-id="e9d85-174">`AccountIdTelemetryInitializer`hello AccountId tulajdonság beállítása.</span><span class="sxs-lookup"><span data-stu-id="e9d85-174">`AccountIdTelemetryInitializer` sets hello AccountId property.</span></span>
* <span data-ttu-id="e9d85-175">`AuthenticatedUserIdTelemetryInitializer`hello AuthenticatedUserId tulajdonság beállítása beállított hello JavaScript SDK által.</span><span class="sxs-lookup"><span data-stu-id="e9d85-175">`AuthenticatedUserIdTelemetryInitializer` sets hello AuthenticatedUserId property as set by hello JavaScript SDK.</span></span>
* <span data-ttu-id="e9d85-176">`AzureRoleEnvironmentTelemetryInitializer`frissítések hello `RoleName` és `RoleInstance` hello tulajdonságainak `Device` hello Azure futtatókörnyezetben kinyert adatokkal az összes telemetriai adatokat a környezetben.</span><span class="sxs-lookup"><span data-stu-id="e9d85-176">`AzureRoleEnvironmentTelemetryInitializer` updates hello `RoleName` and `RoleInstance` properties of hello `Device` context for all telemetry items with information extracted from hello Azure runtime environment.</span></span>
* <span data-ttu-id="e9d85-177">`BuildInfoConfigComponentVersionTelemetryInitializer`frissítések hello `Version` hello tulajdonságának `Component` hello kinyert hello értékű az összes telemetriai környezetben `BuildInfo.config` MS Build által.</span><span class="sxs-lookup"><span data-stu-id="e9d85-177">`BuildInfoConfigComponentVersionTelemetryInitializer` updates hello `Version` property of hello `Component` context for all telemetry items with hello value extracted from hello `BuildInfo.config` file produced by MS Build.</span></span>
* <span data-ttu-id="e9d85-178">`ClientIpHeaderTelemetryInitializer`frissítések `Ip` hello tulajdonságának `Location` környezetben az összes telemetriai elem alapján hello `X-Forwarded-For` hello kérelem HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="e9d85-178">`ClientIpHeaderTelemetryInitializer` updates `Ip` property of hello `Location` context of all telemetry items based on hello `X-Forwarded-For` HTTP header of hello request.</span></span>
* <span data-ttu-id="e9d85-179">`DeviceTelemetryInitializer`a következő hello tulajdonságainak frissítések hello `Device` környezetben az összes telemetriai adat.</span><span class="sxs-lookup"><span data-stu-id="e9d85-179">`DeviceTelemetryInitializer` updates hello following properties of hello `Device` context for all telemetry items.</span></span>
  * <span data-ttu-id="e9d85-180">`Type`túl értéke "PC"</span><span class="sxs-lookup"><span data-stu-id="e9d85-180">`Type` is set too"PC"</span></span>
  * <span data-ttu-id="e9d85-181">`Id`be van állítva hello számítógép tartománynevét toohello hello webes alkalmazást futtató.</span><span class="sxs-lookup"><span data-stu-id="e9d85-181">`Id` is set toohello domain name of hello computer where hello web application is running.</span></span>
  * <span data-ttu-id="e9d85-182">`OemName`hello kinyert toohello érték beállítása `Win32_ComputerSystem.Manufacturer` mezőben a WMI segítségével.</span><span class="sxs-lookup"><span data-stu-id="e9d85-182">`OemName` is set toohello value extracted from hello `Win32_ComputerSystem.Manufacturer` field using WMI.</span></span>
  * <span data-ttu-id="e9d85-183">`Model`hello kinyert toohello érték beállítása `Win32_ComputerSystem.Model` mezőben a WMI segítségével.</span><span class="sxs-lookup"><span data-stu-id="e9d85-183">`Model` is set toohello value extracted from hello `Win32_ComputerSystem.Model` field using WMI.</span></span>
  * <span data-ttu-id="e9d85-184">`NetworkType`hello kinyert toohello érték beállítása `NetworkInterface`.</span><span class="sxs-lookup"><span data-stu-id="e9d85-184">`NetworkType` is set toohello value extracted from hello `NetworkInterface`.</span></span>
  * <span data-ttu-id="e9d85-185">`Language`hello toohello neve van beállítva `CurrentCulture`.</span><span class="sxs-lookup"><span data-stu-id="e9d85-185">`Language` is set toohello name of hello `CurrentCulture`.</span></span>
* <span data-ttu-id="e9d85-186">`DomainNameRoleInstanceTelemetryInitializer`frissítések hello `RoleInstance` hello tulajdonságának `Device` telemetriai szereplő összes hello tartománynévvel hello hello webes alkalmazást futtató számítógép a környezetben.</span><span class="sxs-lookup"><span data-stu-id="e9d85-186">`DomainNameRoleInstanceTelemetryInitializer` updates hello `RoleInstance` property of hello `Device` context for all telemetry items with hello domain name of hello computer where hello web application is running.</span></span>
* <span data-ttu-id="e9d85-187">`OperationNameTelemetryInitializer`frissítések hello `Name` hello tulajdonságának `RequestTelemetry` és hello `Name` hello tulajdonságának `Operation` környezetben az összes telemetriai elem alapján hello HTTP-metódus, valamint az ASP.NET MVC-vezérlő és a művelet a meghívott tooprocess hello nevei a kérést.</span><span class="sxs-lookup"><span data-stu-id="e9d85-187">`OperationNameTelemetryInitializer` updates hello `Name` property of hello `RequestTelemetry` and hello `Name` property of hello `Operation` context of all telemetry items based on hello HTTP method, as well as names of ASP.NET MVC controller and action invoked tooprocess hello request.</span></span>
* <span data-ttu-id="e9d85-188">`OperationIdTelemetryInitializer`vagy `OperationCorrelationTelemetryInitializer` frissítések hello `Operation.Id` context tulajdonság az összes telemetriai elem nyomon követheti az automatikusan generált hello kérelem kezelése közben `RequestTelemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="e9d85-188">`OperationIdTelemetryInitializer` or `OperationCorrelationTelemetryInitializer` updates hello `Operation.Id` context property of all telemetry items tracked while handling a request with hello automatically generated `RequestTelemetry.Id`.</span></span>
* <span data-ttu-id="e9d85-189">`SessionTelemetryInitializer`frissítések hello `Id` hello tulajdonságának `Session` hello kinyert érték az összes telemetriai környezetben `ai_session` hello által generált cookie-k hello felhasználó böngészőjében futó ApplicationInsights JavaScript instrumentation kódot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-189">`SessionTelemetryInitializer` updates hello `Id` property of hello `Session` context for all telemetry items with value extracted from hello `ai_session` cookie generated by hello ApplicationInsights JavaScript instrumentation code running in hello user's browser.</span></span>
* <span data-ttu-id="e9d85-190">`SyntheticTelemetryInitializer`vagy `SyntheticUserAgentTelemetryInitializer` frissítések hello `User`, `Session` és `Operation` összes telemetriai elemek környezetek tulajdonságainak nyomon követ, egy kérelem egy szintetikus forrásból kezelésekor, például a rendelkezésre állási tesztelése, vagy végezzen keresést a motor botot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-190">`SyntheticTelemetryInitializer` or `SyntheticUserAgentTelemetryInitializer` updates hello `User`, `Session` and `Operation` contexts properties of all telemetry items tracked when handling a request from a synthetic source, such as an availability test or search engine bot.</span></span> <span data-ttu-id="e9d85-191">Alapértelmezés szerint [Metrikaböngésző](app-insights-metrics-explorer.md) szintetikus telemetriai adatok nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e9d85-191">By default, [Metrics Explorer](app-insights-metrics-explorer.md) does not display synthetic telemetry.</span></span>

    <span data-ttu-id="e9d85-192">Hello `<Filters>` azonosító hello kérelmek tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="e9d85-192">hello `<Filters>` set identifying properties of hello requests.</span></span>
* <span data-ttu-id="e9d85-193">`UserAgentTelemetryInitializer`frissítések hello `UserAgent` hello tulajdonságának `User` környezetben az összes telemetriai elem alapján hello `User-Agent` hello kérelem HTTP-fejléc.</span><span class="sxs-lookup"><span data-stu-id="e9d85-193">`UserAgentTelemetryInitializer` updates hello `UserAgent` property of hello `User` context of all telemetry items based on hello `User-Agent` HTTP header of hello request.</span></span>
* <span data-ttu-id="e9d85-194">`UserTelemetryInitializer`frissítések hello `Id` és `AcquisitionDate` tulajdonságainak `User` hello kinyert értékekkel az összes telemetriai környezetben `ai_user` hello Application Insights JavaScript instrumentation kód hello futó által generált cookie-k felhasználó böngészőjében.</span><span class="sxs-lookup"><span data-stu-id="e9d85-194">`UserTelemetryInitializer` updates hello `Id` and `AcquisitionDate` properties of `User` context for all telemetry items with values extracted from hello `ai_user` cookie generated by hello Application Insights JavaScript instrumentation code running in hello user's browser.</span></span>
* <span data-ttu-id="e9d85-195">`WebTestTelemetryInitializer`készletek hello felhasználói azonosítót, a munkamenet-azonosító és a szintetikus tulajdonságait a HTTP-kérelmek származó [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md).</span><span class="sxs-lookup"><span data-stu-id="e9d85-195">`WebTestTelemetryInitializer` sets hello user id, session id and synthetic source properties for HTTP requests that come from [availability tests](app-insights-monitor-web-app-availability.md).</span></span>
  <span data-ttu-id="e9d85-196">Hello `<Filters>` azonosító hello kérelmek tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="e9d85-196">hello `<Filters>` set identifying properties of hello requests.</span></span>

<span data-ttu-id="e9d85-197">A Service Fabric-beli .NET-alkalmazásokban, megadhat hello `Microsoft.ApplicationInsights.ServiceFabric` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="e9d85-197">For .NET applications running in Service Fabric, you can include hello `Microsoft.ApplicationInsights.ServiceFabric` NuGet package.</span></span> <span data-ttu-id="e9d85-198">Ez a csomag tartalmaz egy `FabricTelemetryInitializer`, amely a Service Fabric tulajdonságok tootelemetry elemek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="e9d85-198">This package includes a `FabricTelemetryInitializer`, which adds Service Fabric properties tootelemetry items.</span></span> <span data-ttu-id="e9d85-199">További információkért lásd: hello [GitHub-oldalon](https://go.microsoft.com/fwlink/?linkid=848457) a NuGet csomag által hozzáadott hello tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="e9d85-199">For more information, see hello [GitHub page](https://go.microsoft.com/fwlink/?linkid=848457) about hello properties added by this NuGet package.</span></span>

## <a name="telemetry-processors-aspnet"></a><span data-ttu-id="e9d85-200">Telemetria processzor (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e9d85-200">Telemetry Processors (ASP.NET)</span></span>
<span data-ttu-id="e9d85-201">Telemetriai processzorok szűréséhez, és minden telemetriai elem módosítása hello SDK toohello portálról elküldés előtt.</span><span class="sxs-lookup"><span data-stu-id="e9d85-201">Telemetry processors can filter and modify each telemetry item just before it is sent from hello SDK toohello portal.</span></span>

<span data-ttu-id="e9d85-202">Is [saját telemetriai processzorok írási](app-insights-api-filtering-sampling.md#filtering).</span><span class="sxs-lookup"><span data-stu-id="e9d85-202">You can [write your own telemetry processors](app-insights-api-filtering-sampling.md#filtering).</span></span>

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a><span data-ttu-id="e9d85-203">Adaptív mintavételi telemetriai processzor (a 2.0.0-beta3)</span><span class="sxs-lookup"><span data-stu-id="e9d85-203">Adaptive sampling telemetry processor (from 2.0.0-beta3)</span></span>
<span data-ttu-id="e9d85-204">Ez e beállítás alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="e9d85-204">This is enabled by default.</span></span> <span data-ttu-id="e9d85-205">Az alkalmazás nagy mennyiségű telemetriai adatokat küld, ha a processzor eltávolítja bizonyos része.</span><span class="sxs-lookup"><span data-stu-id="e9d85-205">If your app sends a lot of telemetry, this processor removes some of it.</span></span>

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="e9d85-206">hello paraméter biztosít hello target algoritmust hello megpróbál tooachieve.</span><span class="sxs-lookup"><span data-stu-id="e9d85-206">hello parameter provides hello target that hello algorithm tries tooachieve.</span></span> <span data-ttu-id="e9d85-207">Minden példánya hello SDK függetlenül működik, így ha a kiszolgáló egy fürt több gépek, telemetriai adatok tényleges mennyiségét hello és ennek megfelelően kell-e.</span><span class="sxs-lookup"><span data-stu-id="e9d85-207">Each instance of hello SDK works independently, so if your server is a cluster of several machines, hello actual volume of telemetry will be multiplied accordingly.</span></span>

<span data-ttu-id="e9d85-208">[További információ a mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="e9d85-208">[Learn more about sampling](app-insights-sampling.md).</span></span>

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a><span data-ttu-id="e9d85-209">Rögzített mintavételi telemetriai processzor (a 2.0.0-beta1)</span><span class="sxs-lookup"><span data-stu-id="e9d85-209">Fixed-rate sampling telemetry processor (from 2.0.0-beta1)</span></span>
<span data-ttu-id="e9d85-210">Szerepel továbbá egy szabványos [telemetriai processzor mintavételi](app-insights-api-filtering-sampling.md) (a 2.0.1):</span><span class="sxs-lookup"><span data-stu-id="e9d85-210">There is also a standard [sampling telemetry processor](app-insights-api-filtering-sampling.md) (from 2.0.1):</span></span>

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a><span data-ttu-id="e9d85-211">A csatornaparaméterek (Java)</span><span class="sxs-lookup"><span data-stu-id="e9d85-211">Channel parameters (Java)</span></span>
<span data-ttu-id="e9d85-212">Ezek a paraméterek arról, hogy a Java SDK hello hogyan kell tárolni és ürítse ki az általa gyűjtött telemetriaadatok hello érintik.</span><span class="sxs-lookup"><span data-stu-id="e9d85-212">These parameters affect how hello Java SDK should store and flush hello telemetry data that it collects.</span></span>

#### <a name="maxtelemetrybuffercapacity"></a><span data-ttu-id="e9d85-213">MaxTelemetryBufferCapacity</span><span class="sxs-lookup"><span data-stu-id="e9d85-213">MaxTelemetryBufferCapacity</span></span>
<span data-ttu-id="e9d85-214">hello telemetriai elemek száma, amelyek hello SDK memórián belüli tároló tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="e9d85-214">hello number of telemetry items that can be stored in hello SDK's in-memory storage.</span></span> <span data-ttu-id="e9d85-215">A számnak az elérésekor, hello telemetriai puffer ki van ürítve, – a Ez azt jelenti, hogy hello telemetriai elemek toohello Application Insights server küldi el.</span><span class="sxs-lookup"><span data-stu-id="e9d85-215">When this number is reached, hello telemetry buffer is flushed - that is, hello telemetry items are sent toohello Application Insights server.</span></span>

* <span data-ttu-id="e9d85-216">Minimum: 1</span><span class="sxs-lookup"><span data-stu-id="e9d85-216">Min: 1</span></span>
* <span data-ttu-id="e9d85-217">Maximális: 1000</span><span class="sxs-lookup"><span data-stu-id="e9d85-217">Max: 1000</span></span>
* <span data-ttu-id="e9d85-218">Alapértelmezett: 500</span><span class="sxs-lookup"><span data-stu-id="e9d85-218">Default: 500</span></span>

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a><span data-ttu-id="e9d85-219">FlushIntervalInSeconds</span><span class="sxs-lookup"><span data-stu-id="e9d85-219">FlushIntervalInSeconds</span></span>
<span data-ttu-id="e9d85-220">Megállapítja, milyen gyakran hello hello memórián belüli tároló tárolt adatok legyen kiürített (elküldött tooApplication Insights).</span><span class="sxs-lookup"><span data-stu-id="e9d85-220">Determines how often hello data that is stored in hello in-memory storage should be flushed (sent tooApplication Insights).</span></span>

* <span data-ttu-id="e9d85-221">Minimum: 1</span><span class="sxs-lookup"><span data-stu-id="e9d85-221">Min: 1</span></span>
* <span data-ttu-id="e9d85-222">Maximális: 300</span><span class="sxs-lookup"><span data-stu-id="e9d85-222">Max: 300</span></span>
* <span data-ttu-id="e9d85-223">Alapértelmezett: 5</span><span class="sxs-lookup"><span data-stu-id="e9d85-223">Default: 5</span></span>

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a><span data-ttu-id="e9d85-224">MaxTransmissionStorageCapacityInMB</span><span class="sxs-lookup"><span data-stu-id="e9d85-224">MaxTransmissionStorageCapacityInMB</span></span>
<span data-ttu-id="e9d85-225">Meghatározza, hogy hello maximális mérete (MB), amely számára engedélyezett az állandó tároló toohello hello helyi lemezen.</span><span class="sxs-lookup"><span data-stu-id="e9d85-225">Determines hello maximum size in MB that is allotted toohello persistent storage on hello local disk.</span></span> <span data-ttu-id="e9d85-226">Ez a tároló nem sikerült továbbítani toobe toohello Application Insights végpont tárolásakor telemetriai elemekre szolgál.</span><span class="sxs-lookup"><span data-stu-id="e9d85-226">This storage is used for persisting telemetry items that failed toobe transmitted toohello Application Insights endpoint.</span></span> <span data-ttu-id="e9d85-227">Hello tárméret teljesülésekor új telemetriai elemek elvesznek.</span><span class="sxs-lookup"><span data-stu-id="e9d85-227">When hello storage size has been met, new telemetry items will be discarded.</span></span>

* <span data-ttu-id="e9d85-228">Minimum: 1</span><span class="sxs-lookup"><span data-stu-id="e9d85-228">Min: 1</span></span>
* <span data-ttu-id="e9d85-229">Maximum: 100</span><span class="sxs-lookup"><span data-stu-id="e9d85-229">Max: 100</span></span>
* <span data-ttu-id="e9d85-230">Alapértelmezett: 10</span><span class="sxs-lookup"><span data-stu-id="e9d85-230">Default: 10</span></span>

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a><span data-ttu-id="e9d85-231">InstrumentationKey</span><span class="sxs-lookup"><span data-stu-id="e9d85-231">InstrumentationKey</span></span>
<span data-ttu-id="e9d85-232">Ez határozza meg az adatok meg hello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="e9d85-232">This determines hello Application Insights resource in which your data appears.</span></span> <span data-ttu-id="e9d85-233">Általában létrehozhat egy különálló erőforrás külön kulccsal, minden, az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e9d85-233">Typically you create a separate resource, with a separate key, for each of your applications.</span></span>

<span data-ttu-id="e9d85-234">Ha tooset hello kulcsot dinamikusan – például ha az alkalmazás toodifferent erőforrásoktól - toosend eredményét hello konfigurációs fájlból hello kulcs nincs megadva, és helyette állítsa a kódban.</span><span class="sxs-lookup"><span data-stu-id="e9d85-234">If you want tooset hello key dynamically - for example if you want toosend results from your application toodifferent resources - you can omit hello key from hello configuration file, and set it in code instead.</span></span>

<span data-ttu-id="e9d85-235">tooset hello kulcs TelemetryClient, beleértve a szabványos telemetriai modulok összes példánya esetén TelemetryConfiguration.Active hello kulcs beállítva.</span><span class="sxs-lookup"><span data-stu-id="e9d85-235">tooset hello key for all instances of TelemetryClient, including standard telemetry modules, set hello key in TelemetryConfiguration.Active.</span></span> <span data-ttu-id="e9d85-236">Egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e9d85-236">Do this in an initialization method, such as global.aspx.cs in an ASP.NET service:</span></span>

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

<span data-ttu-id="e9d85-237">Ha csak toosend egy adott események tooa különböző erőforrás megadásához egy adott TelemetryClient hello kulcs adhatja meg:</span><span class="sxs-lookup"><span data-stu-id="e9d85-237">If you just want toosend a specific set of events tooa different resource, you can set hello key for a specific TelemetryClient:</span></span>

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

<span data-ttu-id="e9d85-238">egy új kulcsot, tooget [hozzon létre egy új erőforrást a hello Application Insights portál][new].</span><span class="sxs-lookup"><span data-stu-id="e9d85-238">tooget a new key, [create a new resource in hello Application Insights portal][new].</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9d85-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e9d85-239">Next steps</span></span>
<span data-ttu-id="e9d85-240">[További tudnivalók hello API][api].</span><span class="sxs-lookup"><span data-stu-id="e9d85-240">[Learn more about hello API][api].</span></span>

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

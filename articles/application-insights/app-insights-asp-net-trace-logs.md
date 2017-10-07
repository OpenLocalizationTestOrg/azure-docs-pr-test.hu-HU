---
title: "aaaExplore .NET nyomkövetési naplók az Application Insightsban"
description: "Keresse meg a nyomkövetési, NLog és Log4Net létrehozott naplók."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/3/2017
ms.author: bwren
ms.openlocfilehash: 6bfcd9e5751c3656236d7eb2fc09321740171a70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a><span data-ttu-id="9d56c-103">.NET nyomkövetési naplók megtekintése az Application Insights felfedezése</span><span class="sxs-lookup"><span data-stu-id="9d56c-103">Explore .NET trace logs in Application Insights</span></span>
<span data-ttu-id="9d56c-104">NLog, a log4Net, vagy a System.Diagnostics.Trace a diagnosztikai nyomkövetés az ASP.NET-alkalmazás használatakor a naplók küldött túl lehet[Azure Application Insights][start], ahol vizsgálatát és keresése őket.</span><span class="sxs-lookup"><span data-stu-id="9d56c-104">If you use NLog, log4Net or System.Diagnostics.Trace for diagnostic tracing in your ASP.NET application, you can have your logs sent too[Azure Application Insights][start], where you can explore and search them.</span></span> <span data-ttu-id="9d56c-105">A naplók egyesül hello egyéb telemetriai adatokat, így megállapítható, hogy az alkalmazás érkező hello társított minden egyes felhasználói kérelem karbantartási nyomkövetéseket, és a kivizsgált más események és a kivétel jelentések.</span><span class="sxs-lookup"><span data-stu-id="9d56c-105">Your logs will be merged with hello other telemetry coming from your application, so that you can identify hello traces associated with servicing each user request, and correlate them with other events and exception reports.</span></span>

> [!NOTE]
> <span data-ttu-id="9d56c-106">Hello rögzítési naplómoduljának kell?</span><span class="sxs-lookup"><span data-stu-id="9d56c-106">Do you need hello log capture module?</span></span> <span data-ttu-id="9d56c-107">A 3. fél figyelő szoftverek hasznos adaptert, de ha már nem használt NLog, log4Net, vagy a System.Diagnostics.Trace, fontolja meg a csak hívó [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="9d56c-107">It's a useful adapter for 3rd-party loggers, but if you aren't already using NLog, log4Net or System.Diagnostics.Trace, consider just calling [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) directly.</span></span>
>
>

## <a name="install-logging-on-your-app"></a><span data-ttu-id="9d56c-108">Bejelentkezés az alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="9d56c-108">Install logging on your app</span></span>
<span data-ttu-id="9d56c-109">A kiválasztott naplózási keretrendszer telepítése a projektben.</span><span class="sxs-lookup"><span data-stu-id="9d56c-109">Install your chosen logging framework in your project.</span></span> <span data-ttu-id="9d56c-110">Ennek eredménye egy app.config vagy a web.config bejegyzése.</span><span class="sxs-lookup"><span data-stu-id="9d56c-110">This should result in an entry in app.config or web.config.</span></span>

<span data-ttu-id="9d56c-111">System.Diagnostics.Trace használata, ha egy bejegyzés tooweb.config tooadd szüksége:</span><span class="sxs-lookup"><span data-stu-id="9d56c-111">If you're using System.Diagnostics.Trace, you need tooadd an entry tooweb.config:</span></span>

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-toocollect-logs"></a><span data-ttu-id="9d56c-112">Az Application Insights toocollect naplók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d56c-112">Configure Application Insights toocollect logs</span></span>
<span data-ttu-id="9d56c-113">**[Az Application Insights tooyour projekt hozzáadása](app-insights-asp-net.md)**  még ezt nem tette meg, ha.</span><span class="sxs-lookup"><span data-stu-id="9d56c-113">**[Add Application Insights tooyour project](app-insights-asp-net.md)** if you haven't done that yet.</span></span> <span data-ttu-id="9d56c-114">Egy beállítás tooinclude hello naplógyűjtő láthatja.</span><span class="sxs-lookup"><span data-stu-id="9d56c-114">You'll see an option tooinclude hello log collector.</span></span>

<span data-ttu-id="9d56c-115">Vagy **konfigurálja az Application Insights** kattintson a jobb gombbal a projektben a Megoldáskezelőre.</span><span class="sxs-lookup"><span data-stu-id="9d56c-115">Or **Configure Application Insights** by right-clicking your project in Solution Explorer.</span></span> <span data-ttu-id="9d56c-116">A beállításnak a hello túl**gyűjtemény konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="9d56c-116">Select hello option too**Configure trace collection**.</span></span>

<span data-ttu-id="9d56c-117">*Nincs Application Insights menü vagy a napló adatgyűjtő lehetőség?*</span><span class="sxs-lookup"><span data-stu-id="9d56c-117">*No Application Insights menu or log collector option?*</span></span> <span data-ttu-id="9d56c-118">Próbálja [hibaelhárítási](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="9d56c-118">Try [Troubleshooting](#troubleshooting).</span></span>

## <a name="manual-installation"></a><span data-ttu-id="9d56c-119">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="9d56c-119">Manual installation</span></span>
<span data-ttu-id="9d56c-120">Akkor használja ezt a módszert, ha a projekt típusa nem támogatott hello (például a Windows asztali projekt) az Application Insights telepítővel.</span><span class="sxs-lookup"><span data-stu-id="9d56c-120">Use this method if your project type isn't supported by hello Application Insights installer (for example a Windows desktop project).</span></span>

1. <span data-ttu-id="9d56c-121">Ha azt tervezi, toouse log4Net vagy NLog, telepítse a projektet.</span><span class="sxs-lookup"><span data-stu-id="9d56c-121">If you plan toouse log4Net or NLog, install it in your project.</span></span>
2. <span data-ttu-id="9d56c-122">A Megoldáskezelőben kattintson jobb gombbal a projektre, és válassza a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="9d56c-122">In Solution Explorer, right-click your project and choose **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="9d56c-123">Az „Application Insights” kifejezés keresése</span><span class="sxs-lookup"><span data-stu-id="9d56c-123">Search for "Application Insights"</span></span>
4. <span data-ttu-id="9d56c-124">Válassza ki a hello megfelelő csomagot - egyikét:</span><span class="sxs-lookup"><span data-stu-id="9d56c-124">Select hello appropriate package - one of:</span></span>

   * <span data-ttu-id="9d56c-125">Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace hívás)</span><span class="sxs-lookup"><span data-stu-id="9d56c-125">Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace calls)</span></span>
   * <span data-ttu-id="9d56c-126">Microsoft.ApplicationInsights.EventSourceListener (toocapture EventSource esemény)</span><span class="sxs-lookup"><span data-stu-id="9d56c-126">Microsoft.ApplicationInsights.EventSourceListener (toocapture EventSource events)</span></span>
   * <span data-ttu-id="9d56c-127">Microsoft.ApplicationInsights.EtwListener (toocapture ETW-esemény)</span><span class="sxs-lookup"><span data-stu-id="9d56c-127">Microsoft.ApplicationInsights.EtwListener (toocapture ETW events)</span></span>
   * <span data-ttu-id="9d56c-128">Microsoft.ApplicationInsights.NLogTarget</span><span class="sxs-lookup"><span data-stu-id="9d56c-128">Microsoft.ApplicationInsights.NLogTarget</span></span>
   * <span data-ttu-id="9d56c-129">Microsoft.ApplicationInsights.Log4NetAppender</span><span class="sxs-lookup"><span data-stu-id="9d56c-129">Microsoft.ApplicationInsights.Log4NetAppender</span></span>

<span data-ttu-id="9d56c-130">hello NuGet csomag telepíti hello szükséges szerelvényeket, valamint módosítja a web.config vagy az App.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="9d56c-130">hello NuGet package installs hello necessary assemblies, and also modifies web.config or app.config.</span></span>

## <a name="insert-diagnostic-log-calls"></a><span data-ttu-id="9d56c-131">Helyezze be a diagnosztikai naplófájl hívások</span><span class="sxs-lookup"><span data-stu-id="9d56c-131">Insert diagnostic log calls</span></span>
<span data-ttu-id="9d56c-132">Ha a System.Diagnostics.Trace használatához tipikus hívás lenne:</span><span class="sxs-lookup"><span data-stu-id="9d56c-132">If you use System.Diagnostics.Trace, a typical call would be:</span></span>

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

<span data-ttu-id="9d56c-133">Ha inkább log4net vagy NLog:</span><span class="sxs-lookup"><span data-stu-id="9d56c-133">If you prefer log4net or NLog:</span></span>

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a><span data-ttu-id="9d56c-134">EventSource eseményeket használ</span><span class="sxs-lookup"><span data-stu-id="9d56c-134">Using EventSource events</span></span>
<span data-ttu-id="9d56c-135">Konfigurálható [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) események küldött toobe tooApplication Insights, mint a nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="9d56c-135">You can configure [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="9d56c-136">Először telepítse a hello `Microsoft.ApplicationInsights.EventSourceListener` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="9d56c-136">First, install hello `Microsoft.ApplicationInsights.EventSourceListener` NuGet package.</span></span> <span data-ttu-id="9d56c-137">Szerkessze `TelemetryModules` hello szakasza [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.</span><span class="sxs-lookup"><span data-stu-id="9d56c-137">Then edit `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="9d56c-138">Az egyes források állíthatók be a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="9d56c-138">For each source, you can set hello following parameters:</span></span>
 * <span data-ttu-id="9d56c-139">`Name`hello EventSource toocollect hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="9d56c-139">`Name` specifies hello name of hello EventSource toocollect.</span></span>
 * <span data-ttu-id="9d56c-140">`Level`Adja meg a naplózási szint toocollect hello.</span><span class="sxs-lookup"><span data-stu-id="9d56c-140">`Level` specifies hello logging level toocollect.</span></span> <span data-ttu-id="9d56c-141">Egyike lehet `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="9d56c-141">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="9d56c-142">`Keywords`(Nem kötelező) Itt adhatja meg a kulcsszavak kombinációk toouse hello egész értéket.</span><span class="sxs-lookup"><span data-stu-id="9d56c-142">`Keywords` (Optional) specifies hello integer value of keywords combinations toouse.</span></span>

## <a name="using-diagnosticsource-events"></a><span data-ttu-id="9d56c-143">DiagnosticSource eseményeket használ</span><span class="sxs-lookup"><span data-stu-id="9d56c-143">Using DiagnosticSource events</span></span>
<span data-ttu-id="9d56c-144">Konfigurálható [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) események küldött toobe tooApplication Insights, mint a nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="9d56c-144">You can configure [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="9d56c-145">Először telepítse a hello [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="9d56c-145">First, install hello [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet package.</span></span> <span data-ttu-id="9d56c-146">Szerkessze a hello `TelemetryModules` hello szakasza [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.</span><span class="sxs-lookup"><span data-stu-id="9d56c-146">Then edit hello `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

<span data-ttu-id="9d56c-147">Minden egyes DiagnosticSource keresi tootrace, adjon hozzá egy bejegyzést a hello `Name` attribútum a DiagnosticSource toohello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="9d56c-147">For each DiagnosticSource you want tootrace, add an entry with hello `Name` attribute  set toohello name of your DiagnosticSource.</span></span>

## <a name="using-etw-events"></a><span data-ttu-id="9d56c-148">Használja az ETW-események</span><span class="sxs-lookup"><span data-stu-id="9d56c-148">Using ETW events</span></span>
<span data-ttu-id="9d56c-149">Konfigurálhatja az ETW-események toobe tooApplication Insights küldése a nyomkövetési adatokat.</span><span class="sxs-lookup"><span data-stu-id="9d56c-149">You can configure ETW events toobe sent tooApplication Insights as traces.</span></span> <span data-ttu-id="9d56c-150">Először telepítse a hello `Microsoft.ApplicationInsights.EtwCollector` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="9d56c-150">First, install hello `Microsoft.ApplicationInsights.EtwCollector` NuGet package.</span></span> <span data-ttu-id="9d56c-151">Szerkessze `TelemetryModules` hello szakasza [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.</span><span class="sxs-lookup"><span data-stu-id="9d56c-151">Then edit `TelemetryModules` section of hello [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

> [!NOTE] 
> <span data-ttu-id="9d56c-152">ETW-események csak be kell, ha hello folyamat üzemeltetési hello SDK alatt fut, amely tagja "Teljesítménynapló felhasználói" vagy a rendszergazdák az identitást.</span><span class="sxs-lookup"><span data-stu-id="9d56c-152">ETW events can only be collected if hello process hosting hello SDK is running under an identity that is a member of "Performance Log Users" or Administrators.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="9d56c-153">Az egyes források állíthatók be a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="9d56c-153">For each source, you can set hello following parameters:</span></span>
 * <span data-ttu-id="9d56c-154">`ProviderName`az ETW-szolgáltató toocollect hello hello név.</span><span class="sxs-lookup"><span data-stu-id="9d56c-154">`ProviderName` is hello name of hello ETW provider toocollect.</span></span>
 * <span data-ttu-id="9d56c-155">`ProviderGuid`Adja meg a hello GUID-hello ETW-szolgáltató toocollect, ahelyett, hogy használható `ProviderName`.</span><span class="sxs-lookup"><span data-stu-id="9d56c-155">`ProviderGuid` specifies hello GUID of hello ETW provider toocollect, can be used instead of `ProviderName`.</span></span>
 * <span data-ttu-id="9d56c-156">`Level`naplózási szint toocollect hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="9d56c-156">`Level` sets hello logging level toocollect.</span></span> <span data-ttu-id="9d56c-157">Egyike lehet `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="9d56c-157">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="9d56c-158">`Keywords`(Választható) beállítása hello kulcsszó kombinációk toouse egész értéket.</span><span class="sxs-lookup"><span data-stu-id="9d56c-158">`Keywords` (Optional) sets hello integer value of keyword combinations toouse.</span></span>

## <a name="using-hello-trace-api-directly"></a><span data-ttu-id="9d56c-159">Hello nyomkövetési API közvetlen használatával</span><span class="sxs-lookup"><span data-stu-id="9d56c-159">Using hello Trace API directly</span></span>
<span data-ttu-id="9d56c-160">Hívása hello Application Insights nyomkövetési API közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="9d56c-160">You can call hello Application Insights trace API directly.</span></span> <span data-ttu-id="9d56c-161">hello naplózási adapterek Ez az API használnak.</span><span class="sxs-lookup"><span data-stu-id="9d56c-161">hello logging adapters use this API.</span></span>

<span data-ttu-id="9d56c-162">Példa:</span><span class="sxs-lookup"><span data-stu-id="9d56c-162">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

<span data-ttu-id="9d56c-163">TrackTrace előnye, hogy viszonylag hosszú adatok helyezhetik hello üzenetben.</span><span class="sxs-lookup"><span data-stu-id="9d56c-163">An advantage of TrackTrace is that you can put relatively long data in hello message.</span></span> <span data-ttu-id="9d56c-164">Például sikerült kódolni nincs POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="9d56c-164">For example, you could encode POST data there.</span></span>

<span data-ttu-id="9d56c-165">Emellett egy súlyossági szint tooyour üzenetet is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="9d56c-165">In addition, you can add a severity level tooyour message.</span></span> <span data-ttu-id="9d56c-166">És egyéb telemetriai adatok, például értékeket is hozzáadhat tulajdonság használható toohelp szűrőt, vagy keressen a nyomkövetési más-más részhalmazához.</span><span class="sxs-lookup"><span data-stu-id="9d56c-166">And, like other telemetry, you can add property values that you can use toohelp filter or search for different sets of traces.</span></span> <span data-ttu-id="9d56c-167">Példa:</span><span class="sxs-lookup"><span data-stu-id="9d56c-167">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="9d56c-168">Ez lehetővé tenné, hogy a [keresési][diagnostic], tooeasily szűrő egy adott adatbázis tooa vonatkozó adott súlyossági szint minden köszönőüzenetei ki.</span><span class="sxs-lookup"><span data-stu-id="9d56c-168">This would enable you, in [Search][diagnostic], tooeasily filter out all hello messages of a particular severity level relating tooa particular database.</span></span>

## <a name="explore-your-logs"></a><span data-ttu-id="9d56c-169">A naplók felfedezés</span><span class="sxs-lookup"><span data-stu-id="9d56c-169">Explore your logs</span></span>
<span data-ttu-id="9d56c-170">Futtassa az alkalmazást, vagy a hibakeresési módban, vagy telepítheti élő.</span><span class="sxs-lookup"><span data-stu-id="9d56c-170">Run your app, either in debug mode or deploy it live.</span></span>

<span data-ttu-id="9d56c-171">Az alkalmazás áttekintése panelen a [hello Application Insights portál][portal], válassza a [keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="9d56c-171">In your app's overview blade in [hello Application Insights portal][portal], choose [Search][diagnostic].</span></span>

![Az Application insights részére válassza ki a keresés](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Keresés](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

<span data-ttu-id="9d56c-174">Akkor is, például:</span><span class="sxs-lookup"><span data-stu-id="9d56c-174">You can, for example:</span></span>

* <span data-ttu-id="9d56c-175">A naplókivonatokat, vagy az adott tulajdonságokkal rendelkező elemek szűrése</span><span class="sxs-lookup"><span data-stu-id="9d56c-175">Filter on log traces, or on items with specific properties</span></span>
* <span data-ttu-id="9d56c-176">Egy adott cikk részletesen vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="9d56c-176">Inspect a specific item in detail.</span></span>
* <span data-ttu-id="9d56c-177">Más toohello vonatkozó telemetriai található azonos felhasználói kérelem (Ez azt jelenti, hogy a hello azonos OperationID azonosítójú)</span><span class="sxs-lookup"><span data-stu-id="9d56c-177">Find other telemetry relating toohello same user request (that is, with hello same OperationId)</span></span>
* <span data-ttu-id="9d56c-178">Ezen a lapon hello konfigurációjának mentése a Kedvencek közé</span><span class="sxs-lookup"><span data-stu-id="9d56c-178">Save hello configuration of this page as a Favorite</span></span>

> [!NOTE]
> <span data-ttu-id="9d56c-179">**Mintavételi.**</span><span class="sxs-lookup"><span data-stu-id="9d56c-179">**Sampling.**</span></span> <span data-ttu-id="9d56c-180">Ha az alkalmazás nagy mennyiségű adatot küld, és hello Application Insights SDK for ASP.NET verzió 2.0.0-beta3 vagy újabb használ, hello adaptív mintavételi szolgáltatás működtetésében, valamint csak százalékaként a telemetriai adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="9d56c-180">If your application sends a lot of data and you are using hello Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="9d56c-181">További tudnivalók a mintavételezésről.</span><span class="sxs-lookup"><span data-stu-id="9d56c-181">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <a name="next-steps"></a><span data-ttu-id="9d56c-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d56c-182">Next steps</span></span>
<span data-ttu-id="9d56c-183">[Diagnosztizálja a hibákat és kivételeket az ASP.NET][exceptions]</span><span class="sxs-lookup"><span data-stu-id="9d56c-183">[Diagnose failures and exceptions in ASP.NET][exceptions]</span></span>

<span data-ttu-id="9d56c-184">[További információ a keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="9d56c-184">[Learn more about Search][diagnostic].</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="9d56c-185">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="9d56c-185">Troubleshooting</span></span>
### <a name="how-do-i-do-this-for-java"></a><span data-ttu-id="9d56c-186">Hogyan ez Java?</span><span class="sxs-lookup"><span data-stu-id="9d56c-186">How do I do this for Java?</span></span>
<span data-ttu-id="9d56c-187">Használjon hello [Java napló adapterek](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="9d56c-187">Use hello [Java log adapters](app-insights-java-trace-logs.md).</span></span>

### <a name="theres-no-application-insights-option-on-hello-project-context-menu"></a><span data-ttu-id="9d56c-188">Nincs Application Insights lehetőség hello projekt helyi menüben</span><span class="sxs-lookup"><span data-stu-id="9d56c-188">There's no Application Insights option on hello project context menu</span></span>
* <span data-ttu-id="9d56c-189">Ellenőrizze, hogy az Application Insights-eszközök telepítve van a fejlesztési számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9d56c-189">Check Application Insights tools is installed on this development machine.</span></span> <span data-ttu-id="9d56c-190">A Visual Studio menüjében eszközök bővítmények és frissítések, keresse meg az Application Insights-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="9d56c-190">In Visual Studio menu Tools, Extensions and Updates, look for Application Insights Tools.</span></span> <span data-ttu-id="9d56c-191">Ha nem, akkor hello telepített lapon, hello Online lap megnyitásához, és telepítse.</span><span class="sxs-lookup"><span data-stu-id="9d56c-191">If it isn't in hello Installed tab, open hello Online tab and install it.</span></span>
* <span data-ttu-id="9d56c-192">Ez a projekt Application Insights-eszközök által nem támogatott típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="9d56c-192">This might be a type of project not supported by Application Insights tools.</span></span> <span data-ttu-id="9d56c-193">Használjon [manuális telepítés](#manual-installation).</span><span class="sxs-lookup"><span data-stu-id="9d56c-193">Use [manual installation](#manual-installation).</span></span>

### <a name="no-log-adapter-option-in-hello-configuration-tool"></a><span data-ttu-id="9d56c-194">Nincs napló adapter lehetőség hello konfigurálása eszköz</span><span class="sxs-lookup"><span data-stu-id="9d56c-194">No log adapter option in hello configuration tool</span></span>
* <span data-ttu-id="9d56c-195">Először meg kell tooinstall hello naplózási keretrendszert.</span><span class="sxs-lookup"><span data-stu-id="9d56c-195">You need tooinstall hello logging framework first.</span></span>
* <span data-ttu-id="9d56c-196">Ha a System.Diagnostics.Trace használata esetén ellenőrizze, hogy [legyen konfigurálva `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="9d56c-196">If you're using System.Diagnostics.Trace, make sure you [configured it in `web.config`](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span></span>
* <span data-ttu-id="9d56c-197">Rendelkezik készült Application Insights hello legújabb verzióját?</span><span class="sxs-lookup"><span data-stu-id="9d56c-197">Have you got hello latest version of Application Insights?</span></span> <span data-ttu-id="9d56c-198">A Visual Studio **eszközök** menüben válasszon **bővítmények és frissítések**, és nyissa meg hello **frissítések** lapon. Fejlesztői elemzőeszközök nincs, kattintson a tooupdate azt.</span><span class="sxs-lookup"><span data-stu-id="9d56c-198">In Visual Studio **Tools** menu, choose **Extensions and Updates**, and open hello **Updates** tab. If Developer Analytics tools is there, click tooupdate it.</span></span>

### <span data-ttu-id="9d56c-199"><a name="emptykey"></a>Hiba jelenik meg: "Instrumentation kulcs nem lehet üres"</span><span class="sxs-lookup"><span data-stu-id="9d56c-199"><a name="emptykey"></a>I get an error "Instrumentation key cannot be empty"</span></span>
<span data-ttu-id="9d56c-200">A jelek szerint a naplózás adapter Nuget-csomagot az Application Insights telepítése nélkül hello telepítette.</span><span class="sxs-lookup"><span data-stu-id="9d56c-200">Looks like you installed hello logging adapter Nuget package without installing Application Insights.</span></span>

<span data-ttu-id="9d56c-201">A Megoldáskezelőben kattintson a jobb gombbal `ApplicationInsights.config` válassza **frissítés az Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="9d56c-201">In Solution Explorer, right-click `ApplicationInsights.config` and choose **Update Application Insights**.</span></span> <span data-ttu-id="9d56c-202">Kaphat olyan párbeszédpanel, amely hozzáfűzendő toosign a tooAzure, és létrehozza az Application Insights-erőforrást, vagy használja ismét egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="9d56c-202">You'll get a dialog that invites you toosign in tooAzure and either create an Application Insights resource, or re-use an existing one.</span></span> <span data-ttu-id="9d56c-203">Ez segít kell azt.</span><span class="sxs-lookup"><span data-stu-id="9d56c-203">That should fix it.</span></span>

### <a name="i-can-see-traces-in-diagnostic-search-but-not-hello-other-events"></a><span data-ttu-id="9d56c-204">Tudom lásd: a diagnosztikai keresési nyomkövetési adatokat, de nem hello más események</span><span class="sxs-lookup"><span data-stu-id="9d56c-204">I can see traces in diagnostic search, but not hello other events</span></span>
<span data-ttu-id="9d56c-205">Néha eltarthat egy ideig, míg minden hello események és a kérelmek tooget hello-feldolgozási folyamaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="9d56c-205">It can sometimes take a while for all hello events and requests tooget through hello pipeline.</span></span>

### <span data-ttu-id="9d56c-206"><a name="limits"></a>Mennyi adatot megmarad?</span><span class="sxs-lookup"><span data-stu-id="9d56c-206"><a name="limits"></a>How much data is retained?</span></span>
<span data-ttu-id="9d56c-207">Számos tényező befolyásolja a hello megőrzött adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="9d56c-207">Several factors impact hello amount of data retained.</span></span> <span data-ttu-id="9d56c-208">Lásd: hello [korlátok](app-insights-api-custom-events-metrics.md#limits) hello ügyfél esemény metrikák lap további információk szakasza.</span><span class="sxs-lookup"><span data-stu-id="9d56c-208">See hello [limits](app-insights-api-custom-events-metrics.md#limits) section of hello customer event metrics page for more information.</span></span> 

### <a name="im-not-seeing-some-of-hello-log-entries-that-i-expect"></a><span data-ttu-id="9d56c-209">Nem látható az egyes várt hello naplóbejegyzések</span><span class="sxs-lookup"><span data-stu-id="9d56c-209">I'm not seeing some of hello log entries that I expect</span></span>
<span data-ttu-id="9d56c-210">Ha az alkalmazás nagy mennyiségű adatot küld, és hello Application Insights SDK for ASP.NET verzió 2.0.0-beta3 vagy újabb használ, hello adaptív mintavételi szolgáltatás működtetésében, valamint csak százalékaként a telemetriai adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="9d56c-210">If your application sends a lot of data and you are using hello Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, hello adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="9d56c-211">További tudnivalók a mintavételezésről.</span><span class="sxs-lookup"><span data-stu-id="9d56c-211">Learn more about sampling.</span></span>](app-insights-sampling.md)

## <span data-ttu-id="9d56c-212"><a name="add"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d56c-212"><a name="add"></a>Next steps</span></span>
* <span data-ttu-id="9d56c-213">[Rendelkezésre állási és reakcióidőt tesztek beállítása][availability]</span><span class="sxs-lookup"><span data-stu-id="9d56c-213">[Set up availability and responsiveness tests][availability]</span></span>
* <span data-ttu-id="9d56c-214">[Hibaelhárítás][qna]</span><span class="sxs-lookup"><span data-stu-id="9d56c-214">[Troubleshooting][qna]</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

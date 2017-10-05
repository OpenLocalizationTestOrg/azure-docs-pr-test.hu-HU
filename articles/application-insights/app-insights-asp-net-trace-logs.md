---
title: ".NET nyomkövetési naplók megtekintése az Application Insights felfedezése"
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
ms.openlocfilehash: 68e03bf10167ecde675d62782de7063aea9e81d9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a><span data-ttu-id="b3f26-103">.NET nyomkövetési naplók megtekintése az Application Insights felfedezése</span><span class="sxs-lookup"><span data-stu-id="b3f26-103">Explore .NET trace logs in Application Insights</span></span>
<span data-ttu-id="b3f26-104">Ha NLog, a log4Net, vagy a System.Diagnostics.Trace a diagnosztikai nyomkövetés az ASP.NET-alkalmazás lehet küldeni a naplókat [Azure Application Insights][start], ahol vizsgálatát, és keresse meg őket.</span><span class="sxs-lookup"><span data-stu-id="b3f26-104">If you use NLog, log4Net or System.Diagnostics.Trace for diagnostic tracing in your ASP.NET application, you can have your logs sent to [Azure Application Insights][start], where you can explore and search them.</span></span> <span data-ttu-id="b3f26-105">A naplók az alkalmazásból érkező, hogy azonosítsa a nyomkövetési adatokat társított minden egyes felhasználói kérelem karbantartás, és a kivizsgált más események és a kivétel jelentések más telemetriai adatok egyesül.</span><span class="sxs-lookup"><span data-stu-id="b3f26-105">Your logs will be merged with the other telemetry coming from your application, so that you can identify the traces associated with servicing each user request, and correlate them with other events and exception reports.</span></span>

> [!NOTE]
> <span data-ttu-id="b3f26-106">A rögzítési naplómoduljának kell?</span><span class="sxs-lookup"><span data-stu-id="b3f26-106">Do you need the log capture module?</span></span> <span data-ttu-id="b3f26-107">A 3. fél figyelő szoftverek hasznos adaptert, de ha már nem használt NLog, log4Net, vagy a System.Diagnostics.Trace, fontolja meg a csak hívó [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="b3f26-107">It's a useful adapter for 3rd-party loggers, but if you aren't already using NLog, log4Net or System.Diagnostics.Trace, consider just calling [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) directly.</span></span>
>
>

## <a name="install-logging-on-your-app"></a><span data-ttu-id="b3f26-108">Bejelentkezés az alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="b3f26-108">Install logging on your app</span></span>
<span data-ttu-id="b3f26-109">A kiválasztott naplózási keretrendszer telepítése a projektben.</span><span class="sxs-lookup"><span data-stu-id="b3f26-109">Install your chosen logging framework in your project.</span></span> <span data-ttu-id="b3f26-110">Ennek eredménye egy app.config vagy a web.config bejegyzése.</span><span class="sxs-lookup"><span data-stu-id="b3f26-110">This should result in an entry in app.config or web.config.</span></span>

<span data-ttu-id="b3f26-111">Ha System.Diagnostics.Trace használ, adjon hozzá egy bejegyzést a Web.config fájlba szeretné:</span><span class="sxs-lookup"><span data-stu-id="b3f26-111">If you're using System.Diagnostics.Trace, you need to add an entry to web.config:</span></span>

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
## <a name="configure-application-insights-to-collect-logs"></a><span data-ttu-id="b3f26-112">Az Application Insights gyűjtött naplók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b3f26-112">Configure Application Insights to collect logs</span></span>
<span data-ttu-id="b3f26-113">**[Az Application Insights hozzáadása a projekthez](app-insights-asp-net.md)**  még ezt nem tette meg, ha.</span><span class="sxs-lookup"><span data-stu-id="b3f26-113">**[Add Application Insights to your project](app-insights-asp-net.md)** if you haven't done that yet.</span></span> <span data-ttu-id="b3f26-114">Megjelenik egy lehetőség, hogy a naplógyűjtő tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b3f26-114">You'll see an option to include the log collector.</span></span>

<span data-ttu-id="b3f26-115">Vagy **konfigurálja az Application Insights** kattintson a jobb gombbal a projektben a Megoldáskezelőre.</span><span class="sxs-lookup"><span data-stu-id="b3f26-115">Or **Configure Application Insights** by right-clicking your project in Solution Explorer.</span></span> <span data-ttu-id="b3f26-116">Jelölje be a **gyűjtemény konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="b3f26-116">Select the option to **Configure trace collection**.</span></span>

<span data-ttu-id="b3f26-117">*Nincs Application Insights menü vagy a napló adatgyűjtő lehetőség?*</span><span class="sxs-lookup"><span data-stu-id="b3f26-117">*No Application Insights menu or log collector option?*</span></span> <span data-ttu-id="b3f26-118">Próbálja [hibaelhárítási](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="b3f26-118">Try [Troubleshooting](#troubleshooting).</span></span>

## <a name="manual-installation"></a><span data-ttu-id="b3f26-119">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="b3f26-119">Manual installation</span></span>
<span data-ttu-id="b3f26-120">Akkor használja ezt a módszert, ha a projekt típusa nem támogatott az Application Insights telepítővel (például a Windows asztali projekt).</span><span class="sxs-lookup"><span data-stu-id="b3f26-120">Use this method if your project type isn't supported by the Application Insights installer (for example a Windows desktop project).</span></span>

1. <span data-ttu-id="b3f26-121">Ha azt tervezi, a log4Net, vagy a NLog, telepítse a projektet.</span><span class="sxs-lookup"><span data-stu-id="b3f26-121">If you plan to use log4Net or NLog, install it in your project.</span></span>
2. <span data-ttu-id="b3f26-122">A Megoldáskezelőben kattintson jobb gombbal a projektre, és válassza a **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="b3f26-122">In Solution Explorer, right-click your project and choose **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="b3f26-123">Az „Application Insights” kifejezés keresése</span><span class="sxs-lookup"><span data-stu-id="b3f26-123">Search for "Application Insights"</span></span>
4. <span data-ttu-id="b3f26-124">Válassza ki a megfelelő csomag - egyikét:</span><span class="sxs-lookup"><span data-stu-id="b3f26-124">Select the appropriate package - one of:</span></span>

   * <span data-ttu-id="b3f26-125">Microsoft.ApplicationInsights.TraceListener (System.Diagnostics.Trace hívások rögzítéséhez)</span><span class="sxs-lookup"><span data-stu-id="b3f26-125">Microsoft.ApplicationInsights.TraceListener (to capture System.Diagnostics.Trace calls)</span></span>
   * <span data-ttu-id="b3f26-126">(Az EventSource események rögzítése) Microsoft.ApplicationInsights.EventSourceListener</span><span class="sxs-lookup"><span data-stu-id="b3f26-126">Microsoft.ApplicationInsights.EventSourceListener (to capture EventSource events)</span></span>
   * <span data-ttu-id="b3f26-127">(Az ETW-események rögzítése) Microsoft.ApplicationInsights.EtwListener</span><span class="sxs-lookup"><span data-stu-id="b3f26-127">Microsoft.ApplicationInsights.EtwListener (to capture ETW events)</span></span>
   * <span data-ttu-id="b3f26-128">Microsoft.ApplicationInsights.NLogTarget</span><span class="sxs-lookup"><span data-stu-id="b3f26-128">Microsoft.ApplicationInsights.NLogTarget</span></span>
   * <span data-ttu-id="b3f26-129">Microsoft.ApplicationInsights.Log4NetAppender</span><span class="sxs-lookup"><span data-stu-id="b3f26-129">Microsoft.ApplicationInsights.Log4NetAppender</span></span>

<span data-ttu-id="b3f26-130">A NuGet csomag telepíti a szükséges szerelvényeket, valamint módosítja a web.config vagy az App.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3f26-130">The NuGet package installs the necessary assemblies, and also modifies web.config or app.config.</span></span>

## <a name="insert-diagnostic-log-calls"></a><span data-ttu-id="b3f26-131">Helyezze be a diagnosztikai naplófájl hívások</span><span class="sxs-lookup"><span data-stu-id="b3f26-131">Insert diagnostic log calls</span></span>
<span data-ttu-id="b3f26-132">Ha a System.Diagnostics.Trace használatához tipikus hívás lenne:</span><span class="sxs-lookup"><span data-stu-id="b3f26-132">If you use System.Diagnostics.Trace, a typical call would be:</span></span>

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

<span data-ttu-id="b3f26-133">Ha inkább log4net vagy NLog:</span><span class="sxs-lookup"><span data-stu-id="b3f26-133">If you prefer log4net or NLog:</span></span>

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a><span data-ttu-id="b3f26-134">EventSource eseményeket használ</span><span class="sxs-lookup"><span data-stu-id="b3f26-134">Using EventSource events</span></span>
<span data-ttu-id="b3f26-135">Konfigurálható [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) eseményeket az Application Insights nyomkövetési adatokat, küldendő.</span><span class="sxs-lookup"><span data-stu-id="b3f26-135">You can configure [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="b3f26-136">Először telepítse a `Microsoft.ApplicationInsights.EventSourceListener` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="b3f26-136">First, install the `Microsoft.ApplicationInsights.EventSourceListener` NuGet package.</span></span> <span data-ttu-id="b3f26-137">Szerkessze `TelemetryModules` szakasza a [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3f26-137">Then edit `TelemetryModules` section of the [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="b3f26-138">Az egyes források állíthatók be a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="b3f26-138">For each source, you can set the following parameters:</span></span>
 * <span data-ttu-id="b3f26-139">`Name`Adja meg a gyűjtendő EventSource neve.</span><span class="sxs-lookup"><span data-stu-id="b3f26-139">`Name` specifies the name of the EventSource to collect.</span></span>
 * <span data-ttu-id="b3f26-140">`Level`Adja meg a naplózási szint gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="b3f26-140">`Level` specifies the logging level to collect.</span></span> <span data-ttu-id="b3f26-141">Egyike lehet `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="b3f26-141">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="b3f26-142">`Keywords`(Nem kötelező) adja meg a kulcsszavak kombinációk használni az egész értéket.</span><span class="sxs-lookup"><span data-stu-id="b3f26-142">`Keywords` (Optional) specifies the integer value of keywords combinations to use.</span></span>

## <a name="using-diagnosticsource-events"></a><span data-ttu-id="b3f26-143">DiagnosticSource eseményeket használ</span><span class="sxs-lookup"><span data-stu-id="b3f26-143">Using DiagnosticSource events</span></span>
<span data-ttu-id="b3f26-144">Konfigurálható [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) eseményeket az Application Insights nyomkövetési adatokat, küldendő.</span><span class="sxs-lookup"><span data-stu-id="b3f26-144">You can configure [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="b3f26-145">Először telepítse a [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="b3f26-145">First, install the [`Microsoft.ApplicationInsights.DiagnosticSourceListener`](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet package.</span></span> <span data-ttu-id="b3f26-146">Szerkessze a `TelemetryModules` szakasza a [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3f26-146">Then edit the `TelemetryModules` section of the [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

<span data-ttu-id="b3f26-147">Az egyes DiagnosticSource követni kívánt, adja hozzá a bejegyzés a `Name` attribútum értékének beállítása a DiagnosticSource nevét.</span><span class="sxs-lookup"><span data-stu-id="b3f26-147">For each DiagnosticSource you want to trace, add an entry with the `Name` attribute  set to the name of your DiagnosticSource.</span></span>

## <a name="using-etw-events"></a><span data-ttu-id="b3f26-148">Használja az ETW-események</span><span class="sxs-lookup"><span data-stu-id="b3f26-148">Using ETW events</span></span>
<span data-ttu-id="b3f26-149">Application Insights nyomkövetési adatokat, küldendő ETW-események konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b3f26-149">You can configure ETW events to be sent to Application Insights as traces.</span></span> <span data-ttu-id="b3f26-150">Először telepítse a `Microsoft.ApplicationInsights.EtwCollector` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="b3f26-150">First, install the `Microsoft.ApplicationInsights.EtwCollector` NuGet package.</span></span> <span data-ttu-id="b3f26-151">Szerkessze `TelemetryModules` szakasza a [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3f26-151">Then edit `TelemetryModules` section of the [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) file.</span></span>

> [!NOTE] 
> <span data-ttu-id="b3f26-152">ETW-események csak be kell, ha az SDK-t tartalmazó folyamat, amely tagja az "Teljesítménynapló felhasználói" vagy a rendszergazdák identitás alatt fut.</span><span class="sxs-lookup"><span data-stu-id="b3f26-152">ETW events can only be collected if the process hosting the SDK is running under an identity that is a member of "Performance Log Users" or Administrators.</span></span>

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

<span data-ttu-id="b3f26-153">Az egyes források állíthatók be a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="b3f26-153">For each source, you can set the following parameters:</span></span>
 * <span data-ttu-id="b3f26-154">`ProviderName`a gyűjtendő ETW-szolgáltató neve van.</span><span class="sxs-lookup"><span data-stu-id="b3f26-154">`ProviderName` is the name of the ETW provider to collect.</span></span>
 * <span data-ttu-id="b3f26-155">`ProviderGuid`Adja meg a gyűjteni kívánt ETW-szolgáltató GUID helyett használható `ProviderName`.</span><span class="sxs-lookup"><span data-stu-id="b3f26-155">`ProviderGuid` specifies the GUID of the ETW provider to collect, can be used instead of `ProviderName`.</span></span>
 * <span data-ttu-id="b3f26-156">`Level`Beállítja a naplózási szint gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="b3f26-156">`Level` sets the logging level to collect.</span></span> <span data-ttu-id="b3f26-157">Egyike lehet `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span><span class="sxs-lookup"><span data-stu-id="b3f26-157">Can be one of `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.</span></span>
 * <span data-ttu-id="b3f26-158">`Keywords`(Nem kötelező) kulcsszó kombinációk használni az egész értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="b3f26-158">`Keywords` (Optional) sets the integer value of keyword combinations to use.</span></span>

## <a name="using-the-trace-api-directly"></a><span data-ttu-id="b3f26-159">A nyomkövetés API közvetlen használatával</span><span class="sxs-lookup"><span data-stu-id="b3f26-159">Using the Trace API directly</span></span>
<span data-ttu-id="b3f26-160">Hívása az Application Insights nyomkövetési API közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="b3f26-160">You can call the Application Insights trace API directly.</span></span> <span data-ttu-id="b3f26-161">A naplózási adapterek Ez az API használnak.</span><span class="sxs-lookup"><span data-stu-id="b3f26-161">The logging adapters use this API.</span></span>

<span data-ttu-id="b3f26-162">Példa:</span><span class="sxs-lookup"><span data-stu-id="b3f26-162">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

<span data-ttu-id="b3f26-163">TrackTrace előnye, hogy viszonylag hosszú adatok helyezhetik az üzenetben.</span><span class="sxs-lookup"><span data-stu-id="b3f26-163">An advantage of TrackTrace is that you can put relatively long data in the message.</span></span> <span data-ttu-id="b3f26-164">Például sikerült kódolni nincs POST-adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3f26-164">For example, you could encode POST data there.</span></span>

<span data-ttu-id="b3f26-165">Emellett egy súlyossági szintet adhat hozzá az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="b3f26-165">In addition, you can add a severity level to your message.</span></span> <span data-ttu-id="b3f26-166">És egyéb telemetriai adatok, például hozzáadhat, amelyek segítségével szűrőt, vagy keressen a nyomkövetési más-más részhalmazához.</span><span class="sxs-lookup"><span data-stu-id="b3f26-166">And, like other telemetry, you can add property values that you can use to help filter or search for different sets of traces.</span></span> <span data-ttu-id="b3f26-167">Példa:</span><span class="sxs-lookup"><span data-stu-id="b3f26-167">For example:</span></span>

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

<span data-ttu-id="b3f26-168">Ez lehetővé tenné, hogy a [keresési][diagnostic], az adott adatbázishoz vonatkozó adott súlyossági szintet az üzenetek egyszerűen kiszűrésére.</span><span class="sxs-lookup"><span data-stu-id="b3f26-168">This would enable you, in [Search][diagnostic], to easily filter out all the messages of a particular severity level relating to a particular database.</span></span>

## <a name="explore-your-logs"></a><span data-ttu-id="b3f26-169">A naplók felfedezés</span><span class="sxs-lookup"><span data-stu-id="b3f26-169">Explore your logs</span></span>
<span data-ttu-id="b3f26-170">Futtassa az alkalmazást, vagy a hibakeresési módban, vagy telepítheti élő.</span><span class="sxs-lookup"><span data-stu-id="b3f26-170">Run your app, either in debug mode or deploy it live.</span></span>

<span data-ttu-id="b3f26-171">Az alkalmazás áttekintése panelen a [az Application Insights portáljáról][portal], válassza a [keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="b3f26-171">In your app's overview blade in [the Application Insights portal][portal], choose [Search][diagnostic].</span></span>

![Az Application insights részére válassza ki a keresés](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Keresés](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

<span data-ttu-id="b3f26-174">Akkor is, például:</span><span class="sxs-lookup"><span data-stu-id="b3f26-174">You can, for example:</span></span>

* <span data-ttu-id="b3f26-175">A naplókivonatokat, vagy az adott tulajdonságokkal rendelkező elemek szűrése</span><span class="sxs-lookup"><span data-stu-id="b3f26-175">Filter on log traces, or on items with specific properties</span></span>
* <span data-ttu-id="b3f26-176">Egy adott cikk részletesen vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="b3f26-176">Inspect a specific item in detail.</span></span>
* <span data-ttu-id="b3f26-177">Található más felhasználó kérésben vonatkozó telemetriai adatokat (Ez azt jelenti, hogy az azonos OperationID azonosítójú rendelkező)</span><span class="sxs-lookup"><span data-stu-id="b3f26-177">Find other telemetry relating to the same user request (that is, with the same OperationId)</span></span>
* <span data-ttu-id="b3f26-178">Ezen a lapon konfigurációjának mentése a Kedvencek közé</span><span class="sxs-lookup"><span data-stu-id="b3f26-178">Save the configuration of this page as a Favorite</span></span>

> [!NOTE]
> <span data-ttu-id="b3f26-179">**Mintavételi.**</span><span class="sxs-lookup"><span data-stu-id="b3f26-179">**Sampling.**</span></span> <span data-ttu-id="b3f26-180">Ha az alkalmazása sok adatot küld, és az Application Insights SDK-t az ASP.NET 2.0.0-beta3 vagy újabb verziójához használja, működhet az adaptív mintavételezés funkció és lehet, hogy csak a telemetria valamely százalékát küldi el.</span><span class="sxs-lookup"><span data-stu-id="b3f26-180">If your application sends a lot of data and you are using the Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, the adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="b3f26-181">További tudnivalók a mintavételezésről.</span><span class="sxs-lookup"><span data-stu-id="b3f26-181">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <a name="next-steps"></a><span data-ttu-id="b3f26-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3f26-182">Next steps</span></span>
<span data-ttu-id="b3f26-183">[Diagnosztizálja a hibákat és kivételeket az ASP.NET][exceptions]</span><span class="sxs-lookup"><span data-stu-id="b3f26-183">[Diagnose failures and exceptions in ASP.NET][exceptions]</span></span>

<span data-ttu-id="b3f26-184">[További információ a keresési][diagnostic].</span><span class="sxs-lookup"><span data-stu-id="b3f26-184">[Learn more about Search][diagnostic].</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b3f26-185">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="b3f26-185">Troubleshooting</span></span>
### <a name="how-do-i-do-this-for-java"></a><span data-ttu-id="b3f26-186">Hogyan ez Java?</span><span class="sxs-lookup"><span data-stu-id="b3f26-186">How do I do this for Java?</span></span>
<span data-ttu-id="b3f26-187">Használja a [Java napló adapterek](app-insights-java-trace-logs.md).</span><span class="sxs-lookup"><span data-stu-id="b3f26-187">Use the [Java log adapters](app-insights-java-trace-logs.md).</span></span>

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a><span data-ttu-id="b3f26-188">A projekt helyi menüben nincs Application Insights lehetőség</span><span class="sxs-lookup"><span data-stu-id="b3f26-188">There's no Application Insights option on the project context menu</span></span>
* <span data-ttu-id="b3f26-189">Ellenőrizze, hogy az Application Insights-eszközök telepítve van a fejlesztési számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b3f26-189">Check Application Insights tools is installed on this development machine.</span></span> <span data-ttu-id="b3f26-190">A Visual Studio menüjében eszközök bővítmények és frissítések, keresse meg az Application Insights-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="b3f26-190">In Visual Studio menu Tools, Extensions and Updates, look for Application Insights Tools.</span></span> <span data-ttu-id="b3f26-191">Ha nem, akkor a telepített lap, nyissa meg az Online lapot, és telepítse.</span><span class="sxs-lookup"><span data-stu-id="b3f26-191">If it isn't in the Installed tab, open the Online tab and install it.</span></span>
* <span data-ttu-id="b3f26-192">Ez a projekt Application Insights-eszközök által nem támogatott típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="b3f26-192">This might be a type of project not supported by Application Insights tools.</span></span> <span data-ttu-id="b3f26-193">Használjon [manuális telepítés](#manual-installation).</span><span class="sxs-lookup"><span data-stu-id="b3f26-193">Use [manual installation](#manual-installation).</span></span>

### <a name="no-log-adapter-option-in-the-configuration-tool"></a><span data-ttu-id="b3f26-194">A kiszolgálókonfigurációs eszköz nincs napló adapter lehetőség</span><span class="sxs-lookup"><span data-stu-id="b3f26-194">No log adapter option in the configuration tool</span></span>
* <span data-ttu-id="b3f26-195">A naplózási keretrendszer először telepítenie kell.</span><span class="sxs-lookup"><span data-stu-id="b3f26-195">You need to install the logging framework first.</span></span>
* <span data-ttu-id="b3f26-196">Ha a System.Diagnostics.Trace használata esetén ellenőrizze, hogy [legyen konfigurálva `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3f26-196">If you're using System.Diagnostics.Trace, make sure you [configured it in `web.config`](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).</span></span>
* <span data-ttu-id="b3f26-197">Rendelkezik készült Application Insights legújabb verzióját?</span><span class="sxs-lookup"><span data-stu-id="b3f26-197">Have you got the latest version of Application Insights?</span></span> <span data-ttu-id="b3f26-198">A Visual Studio **eszközök** menüben válasszon **bővítmények és frissítések**, és nyissa meg a **frissítések** lapon. Fejlesztői elemzőeszközök nincs, kattintson a frissítést.</span><span class="sxs-lookup"><span data-stu-id="b3f26-198">In Visual Studio **Tools** menu, choose **Extensions and Updates**, and open the **Updates** tab. If Developer Analytics tools is there, click to update it.</span></span>

### <span data-ttu-id="b3f26-199"><a name="emptykey"></a>Hiba jelenik meg: "Instrumentation kulcs nem lehet üres"</span><span class="sxs-lookup"><span data-stu-id="b3f26-199"><a name="emptykey"></a>I get an error "Instrumentation key cannot be empty"</span></span>
<span data-ttu-id="b3f26-200">A jelek szerint a naplózás adapter Nuget-csomagot telepítette az Application Insights telepítése nélkül.</span><span class="sxs-lookup"><span data-stu-id="b3f26-200">Looks like you installed the logging adapter Nuget package without installing Application Insights.</span></span>

<span data-ttu-id="b3f26-201">A Megoldáskezelőben kattintson a jobb gombbal `ApplicationInsights.config` válassza **frissítés az Application Insights**.</span><span class="sxs-lookup"><span data-stu-id="b3f26-201">In Solution Explorer, right-click `ApplicationInsights.config` and choose **Update Application Insights**.</span></span> <span data-ttu-id="b3f26-202">Olyan párbeszédpanel, amely felkéri, hogy jelentkezzen be az Azure-bA kap, és létrehozza az Application Insights-erőforrást, vagy használja ismét egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="b3f26-202">You'll get a dialog that invites you to sign in to Azure and either create an Application Insights resource, or re-use an existing one.</span></span> <span data-ttu-id="b3f26-203">Ez segít kell azt.</span><span class="sxs-lookup"><span data-stu-id="b3f26-203">That should fix it.</span></span>

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a><span data-ttu-id="b3f26-204">Láthatom valahol mappában lévő diagnosztikai keresési, de nem tartozó más események</span><span class="sxs-lookup"><span data-stu-id="b3f26-204">I can see traces in diagnostic search, but not the other events</span></span>
<span data-ttu-id="b3f26-205">Azt is néha időigényes az összes olyan események és kérelmek lekérni a feldolgozási folyamaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b3f26-205">It can sometimes take a while for all the events and requests to get through the pipeline.</span></span>

### <span data-ttu-id="b3f26-206"><a name="limits"></a>Mennyi adatot megmarad?</span><span class="sxs-lookup"><span data-stu-id="b3f26-206"><a name="limits"></a>How much data is retained?</span></span>
<span data-ttu-id="b3f26-207">Számos tényező befolyásolja a megőrzött adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="b3f26-207">Several factors impact the amount of data retained.</span></span> <span data-ttu-id="b3f26-208">Tekintse meg a [korlátok](app-insights-api-custom-events-metrics.md#limits) szakasz az ügyfél esemény metrikák oldal további információt.</span><span class="sxs-lookup"><span data-stu-id="b3f26-208">See the [limits](app-insights-api-custom-events-metrics.md#limits) section of the customer event metrics page for more information.</span></span> 

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a><span data-ttu-id="b3f26-209">Nem látható az egyes a naplóbejegyzéseket, amelyeket a várt</span><span class="sxs-lookup"><span data-stu-id="b3f26-209">I'm not seeing some of the log entries that I expect</span></span>
<span data-ttu-id="b3f26-210">Ha az alkalmazása sok adatot küld, és az Application Insights SDK-t az ASP.NET 2.0.0-beta3 vagy újabb verziójához használja, működhet az adaptív mintavételezés funkció és lehet, hogy csak a telemetria valamely százalékát küldi el.</span><span class="sxs-lookup"><span data-stu-id="b3f26-210">If your application sends a lot of data and you are using the Application Insights SDK for ASP.NET version 2.0.0-beta3 or later, the adaptive sampling feature may operate and send only a percentage of your telemetry.</span></span> [<span data-ttu-id="b3f26-211">További tudnivalók a mintavételezésről.</span><span class="sxs-lookup"><span data-stu-id="b3f26-211">Learn more about sampling.</span></span>](app-insights-sampling.md)

## <span data-ttu-id="b3f26-212"><a name="add"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3f26-212"><a name="add"></a>Next steps</span></span>
* <span data-ttu-id="b3f26-213">[Rendelkezésre állási és reakcióidőt tesztek beállítása][availability]</span><span class="sxs-lookup"><span data-stu-id="b3f26-213">[Set up availability and responsiveness tests][availability]</span></span>
* <span data-ttu-id="b3f26-214">[Hibaelhárítás][qna]</span><span class="sxs-lookup"><span data-stu-id="b3f26-214">[Troubleshooting][qna]</span></span>

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

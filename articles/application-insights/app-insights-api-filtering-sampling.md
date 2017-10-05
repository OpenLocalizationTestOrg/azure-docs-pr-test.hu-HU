---
title: "Szűrés és az Azure Application Insights SDK előfeldolgozása |} Microsoft Docs"
description: "Az írási Telemetriai processzorok és a telemetriai adatok inicializálók, az SDK-ban való vagy tulajdonságokat adhat az adatok az Application Insights portál telemetriai adatok elküldése előtt."
services: application-insights
documentationcenter: 
author: beckylino
manager: carmonm
ms.assetid: 38a9e454-43d5-4dba-a0f0-bd7cd75fb97b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 17e66775dd2cd1c858594102f1ddb32e2fbbccc8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-the-application-insights-sdk"></a><span data-ttu-id="41b00-103">Szűrés és az Application Insights SDK a telemetriai adatok előfeldolgozása</span><span class="sxs-lookup"><span data-stu-id="41b00-103">Filtering and preprocessing telemetry in the Application Insights SDK</span></span>


<span data-ttu-id="41b00-104">Írása, és beépülő modulokat az Application Insights SDK testreszabása hogyan telemetriai adatokat rögzíti, majd dolgozni, mielőtt továbbítja az Application Insights szolgáltatás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="41b00-104">You can write and configure plug-ins for the Application Insights SDK to customize how telemetry is captured and processed before it is sent to the Application Insights service.</span></span>

* <span data-ttu-id="41b00-105">[A mintavételi](app-insights-sampling.md) telemetriai adatok mennyiségét csökkenti a statisztika befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="41b00-105">[Sampling](app-insights-sampling.md) reduces the volume of telemetry without affecting your statistics.</span></span> <span data-ttu-id="41b00-106">Azonos tartja kapcsolatos adatok mutat, így közöttük, ha a probléma diagnosztizálása navigálhat.</span><span class="sxs-lookup"><span data-stu-id="41b00-106">It keeps together related data points so that you can navigate between them when diagnosing a problem.</span></span> <span data-ttu-id="41b00-107">A portálon a teljes számlálás szorozni, hogy a mintavételi kártalanítja.</span><span class="sxs-lookup"><span data-stu-id="41b00-107">In the portal, the total counts are multiplied to compensate for the sampling.</span></span>
* <span data-ttu-id="41b00-108">Szűrés Telemetriai processzorokat [ASP.NET](#filtering) vagy [Java](app-insights-java-filter-telemetry.md) lehetővé teszi, hogy válasszon, vagy módosítsa a az SDK-telemetriai adatokat a kiszolgálón való továbbítás előtt.</span><span class="sxs-lookup"><span data-stu-id="41b00-108">Filtering with Telemetry Processors [for ASP.NET](#filtering) or [Java](app-insights-java-filter-telemetry.md) lets you select or modify telemetry in the SDK before it is sent to the server.</span></span> <span data-ttu-id="41b00-109">A telemetriai adatok mennyiségének csökkentheti például kérelmek kizárja a robotokat.</span><span class="sxs-lookup"><span data-stu-id="41b00-109">For example, you could reduce the volume of telemetry by excluding requests from robots.</span></span> <span data-ttu-id="41b00-110">De szűrés több egyszerű megközelítése mint mintavételi forgalom csökkentése.</span><span class="sxs-lookup"><span data-stu-id="41b00-110">But filtering is a more basic approach to reducing traffic than sampling.</span></span> <span data-ttu-id="41b00-111">Mi továbbított teljesebb körű vezérlése lehetővé teszi, de vegye figyelembe, hogy az hatással van a statisztika – például ki az összes sikeres kérelmek szűrése kell.</span><span class="sxs-lookup"><span data-stu-id="41b00-111">It allows you more control over what is transmitted, but you have to be aware that it affects your statistics - for example, if you filter out all successful requests.</span></span>
* <span data-ttu-id="41b00-112">[Telemetria inicializálók tulajdonságok hozzáadása](#add-properties) bármely telemetriai adatok küldése az alkalmazásból, beleértve a szabványos modulból telemetriai való.</span><span class="sxs-lookup"><span data-stu-id="41b00-112">[Telemetry Initializers add properties](#add-properties) to any telemetry sent from your app, including telemetry from the standard modules.</span></span> <span data-ttu-id="41b00-113">Például hozzáadhatja számított értékeket; vagy a portál adatainak szűréséhez verziószámokra.</span><span class="sxs-lookup"><span data-stu-id="41b00-113">For example, you could add calculated values; or version numbers by which to filter the data in the portal.</span></span>
* <span data-ttu-id="41b00-114">[Az SDK API-t](app-insights-api-custom-events-metrics.md) küldése egyéni események és metrikák használatával.</span><span class="sxs-lookup"><span data-stu-id="41b00-114">[The SDK API](app-insights-api-custom-events-metrics.md) is used to send custom events and metrics.</span></span>

<span data-ttu-id="41b00-115">Előkészületek:</span><span class="sxs-lookup"><span data-stu-id="41b00-115">Before you start:</span></span>

* <span data-ttu-id="41b00-116">Telepítse az Application Insights [SDK for ASP.NET](app-insights-asp-net.md) vagy [SDK Java](app-insights-java-get-started.md) az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="41b00-116">Install the Application Insights [SDK for ASP.NET](app-insights-asp-net.md) or [SDK for Java](app-insights-java-get-started.md) in your app.</span></span>

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a><span data-ttu-id="41b00-117">Szűrés: ITelemetryProcessor</span><span class="sxs-lookup"><span data-stu-id="41b00-117">Filtering: ITelemetryProcessor</span></span>
<span data-ttu-id="41b00-118">Ez a módszer lehetővé teszi több közvetlen ellenőrzése alatt tartja a mi van, illetve tiltani szeretné a telemetriai adatok adatfolyamból.</span><span class="sxs-lookup"><span data-stu-id="41b00-118">This technique gives you more direct control over what is included or excluded from the telemetry stream.</span></span> <span data-ttu-id="41b00-119">A mintavételi, párhuzamosan használható vagy külön-külön.</span><span class="sxs-lookup"><span data-stu-id="41b00-119">You can use it in conjunction with Sampling, or separately.</span></span>

<span data-ttu-id="41b00-120">Telemetriai adatok szűrése, telemetriai processzort írása, és regisztrálja az SDK-val.</span><span class="sxs-lookup"><span data-stu-id="41b00-120">To filter telemetry, you write a telemetry processor and register it with the SDK.</span></span> <span data-ttu-id="41b00-121">A processzor végighalad az összes telemetriai adat, és dobja el, az adatfolyamból, vagy vegye fel a Tulajdonságok választhatja.</span><span class="sxs-lookup"><span data-stu-id="41b00-121">All telemetry goes through your processor, and you can choose to drop it from the stream, or add properties.</span></span> <span data-ttu-id="41b00-122">Ez magában foglalja a szabványos modulból, mint a HTTP-kérelem adatgyűjtő és a függőségi adatgyűjtő telemetriai, valamint saját kezűleg írt telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="41b00-122">This includes telemetry from the standard modules such as the HTTP request collector and the dependency collector, as well as telemetry you have written yourself.</span></span> <span data-ttu-id="41b00-123">Például szűrhetők telemetriai adatainak robots vagy sikeres függőségi hívások esetében érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="41b00-123">You can, for example, filter out telemetry about requests from robots, or successful dependency calls.</span></span>

> [!WARNING]
> <span data-ttu-id="41b00-124">Az SDK által küldött telemetriai adatok szűrése feldolgozók segítségével döntés a statisztika, melyek megjelennek a portálon, és nehéz hajtsa végre a kapcsolódó elemek.</span><span class="sxs-lookup"><span data-stu-id="41b00-124">Filtering the telemetry sent from the SDK using processors can skew the statistics that you see in the portal, and make it difficult to follow related items.</span></span>
>
> <span data-ttu-id="41b00-125">Ehelyett érdemes [mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="41b00-125">Instead, consider using [sampling](app-insights-sampling.md).</span></span>
>
>

### <a name="create-a-telemetry-processor-c"></a><span data-ttu-id="41b00-126">Hozzon létre egy telemetriai processzor (C#)</span><span class="sxs-lookup"><span data-stu-id="41b00-126">Create a telemetry processor (C#)</span></span>
1. <span data-ttu-id="41b00-127">Győződjön meg arról, hogy az Application Insights SDK célverzióját 2.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="41b00-127">Verify that the Application Insights SDK in your project is  version 2.0.0 or later.</span></span> <span data-ttu-id="41b00-128">Kattintson a jobb gombbal a projektre a Visual Studio Solution Explorerben, és válassza ki a NuGet-csomagok kezelése.</span><span class="sxs-lookup"><span data-stu-id="41b00-128">Right-click your project in Visual Studio Solution Explorer and choose Manage NuGet Packages.</span></span> <span data-ttu-id="41b00-129">A NuGet-Csomagkezelő ellenőrizze a Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="41b00-129">In NuGet package manager, check Microsoft.ApplicationInsights.Web.</span></span>
2. <span data-ttu-id="41b00-130">Szűrő létrehozásához ITelemetryProcessor megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="41b00-130">To create a filter, implement ITelemetryProcessor.</span></span> <span data-ttu-id="41b00-131">Ez egy másik bővítési pontot például telemetriai modul, telemetriai inicializáló és telemetriai csatorna.</span><span class="sxs-lookup"><span data-stu-id="41b00-131">This is another extensibility point like telemetry module, telemetry initializer, and telemetry channel.</span></span>

    <span data-ttu-id="41b00-132">Figyelje meg, hogy a Telemetriai processzor feldolgozási láncolata összeállításához.</span><span class="sxs-lookup"><span data-stu-id="41b00-132">Notice that Telemetry Processors construct a chain of processing.</span></span> <span data-ttu-id="41b00-133">Telemetria processzort hozható létre, ha át egy hivatkozást a következő feldolgozó a láncban.</span><span class="sxs-lookup"><span data-stu-id="41b00-133">When you instantiate a telemetry processor, you pass a link to the next processor in the chain.</span></span> <span data-ttu-id="41b00-134">Ha telemetriai adatpont folyamat metódus számára, ne a tevékenységeket, majd majd hívja a következő Telemetriai processzor a lánc.</span><span class="sxs-lookup"><span data-stu-id="41b00-134">When a telemetry data point is passed to the Process method, it does its work and then calls the next Telemetry Processor in the chain.</span></span>

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors to each other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // To filter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify the item if required
            ModifyItem(item);

            this.Next.Process(item);
        }

        // Example: replace with your own criteria.
        private bool OKtoSend (ITelemetry item)
        {
            var dependency = item as DependencyTelemetry;
            if (dependency == null) return true;

            return dependency.Success != true;
        }

        // Example: replace with your own modifiers.
        private void ModifyItem (ITelemetry item)
        {
            item.Context.Properties.Add("app-version", "1." + MyParamFromConfigFile);
        }
    }

    ```
1. <span data-ttu-id="41b00-135">Helyezze be a ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="41b00-135">Insert this in ApplicationInsights.config:</span></span>

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="41b00-136">(Ez a szakaszában azonos ahol inicializálni a mintavételi szűrő.)</span><span class="sxs-lookup"><span data-stu-id="41b00-136">(This is the same section where you initialize a sampling filter.)</span></span>

<span data-ttu-id="41b00-137">Az osztály nyilvános elnevezett tulajdonságok megadásával átadhatók karakterlánc-értékek a .config fájlból.</span><span class="sxs-lookup"><span data-stu-id="41b00-137">You can pass string values from the .config file by providing public named properties in your class.</span></span>

> [!WARNING]
> <span data-ttu-id="41b00-138">A nevét, és az osztály és a tulajdonság nevét, a kódban az .config fájl bármely tulajdonságnevek megfelelően kezeli.</span><span class="sxs-lookup"><span data-stu-id="41b00-138">Take care to match the type name and any property names in the .config file to the class and property names in the code.</span></span> <span data-ttu-id="41b00-139">Ha a .config fájl egy nem létező típus vagy a tulajdonságra hivatkozik, az SDK-t is csendes nem minden telemetriai adatokat küldhet.</span><span class="sxs-lookup"><span data-stu-id="41b00-139">If the .config file references a non-existent type or property, the SDK may silently fail to send any telemetry.</span></span>
>
>

<span data-ttu-id="41b00-140">**Másik lehetőségként** tudja inicializálni a kódban a szűrőt.</span><span class="sxs-lookup"><span data-stu-id="41b00-140">**Alternatively,** you can initialize the filter in code.</span></span> <span data-ttu-id="41b00-141">A megfelelő inicializálása az osztály - például a Global.asax.cs AppStart - a processzor beilleszteni a lánc:</span><span class="sxs-lookup"><span data-stu-id="41b00-141">In a suitable initialization class - for example AppStart in Global.asax.cs - insert your processor into the chain:</span></span>

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="41b00-142">Ezt a pontot fogja használni a processzor után létrehozott TelemetryClients.</span><span class="sxs-lookup"><span data-stu-id="41b00-142">TelemetryClients created after this point will use your processors.</span></span>

### <a name="example-filters"></a><span data-ttu-id="41b00-143">Példa szűrők</span><span class="sxs-lookup"><span data-stu-id="41b00-143">Example filters</span></span>
#### <a name="synthetic-requests"></a><span data-ttu-id="41b00-144">Szintetikus kérelmek</span><span class="sxs-lookup"><span data-stu-id="41b00-144">Synthetic requests</span></span>
<span data-ttu-id="41b00-145">Botok és a webalkalmazás-tesztek szűrik.</span><span class="sxs-lookup"><span data-stu-id="41b00-145">Filter out bots and web tests.</span></span> <span data-ttu-id="41b00-146">Bár a Metrikaböngésző felajánlja a szintetikus források szűrheti, ezt a beállítást, az SDK szűréssel csökken a forgalom.</span><span class="sxs-lookup"><span data-stu-id="41b00-146">Although Metrics Explorer gives you the option to filter out synthetic sources, this option reduces traffic by filtering them at the SDK.</span></span>

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a><span data-ttu-id="41b00-147">Sikertelen hitelesítési</span><span class="sxs-lookup"><span data-stu-id="41b00-147">Failed authentication</span></span>
<span data-ttu-id="41b00-148">Szűrheti kérések "401-es" választ.</span><span class="sxs-lookup"><span data-stu-id="41b00-148">Filter out requests with a "401" response.</span></span>

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // To filter out an item, just terminate the chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a><span data-ttu-id="41b00-149">Kiszűrhetők a gyorsan távoli függőségi hívások esetében</span><span class="sxs-lookup"><span data-stu-id="41b00-149">Filter out fast remote dependency calls</span></span>
<span data-ttu-id="41b00-150">Ha szeretné, amelyek lassú hívások diagnosztizálása, kiszűrhetők a gyorsan megfelelően.</span><span class="sxs-lookup"><span data-stu-id="41b00-150">If you only want to diagnose calls that are slow, filter out the fast ones.</span></span>

> [!NOTE]
> <span data-ttu-id="41b00-151">Ez fogja döntés a statisztika, megjelenik a portálon.</span><span class="sxs-lookup"><span data-stu-id="41b00-151">This will skew the statistics you see on the portal.</span></span> <span data-ttu-id="41b00-152">A függőség diagram, ha a függőségi hívások esetében minden hibák fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="41b00-152">The dependency chart will look as if the dependency calls are all failures.</span></span>
>
>

``` C#

public void Process(ITelemetry item)
{
    var request = item as DependencyTelemetry;

    if (request != null && request.Duration.TotalMilliseconds < 100)
    {
        return;
    }
    this.Next.Process(item);
}

```

#### <a name="diagnose-dependency-issues"></a><span data-ttu-id="41b00-153">Függőségi problémák diagnosztikája</span><span class="sxs-lookup"><span data-stu-id="41b00-153">Diagnose dependency issues</span></span>
<span data-ttu-id="41b00-154">[Ebben a blogban](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) függőségi eseményadatokat elküldésével automatikusan rendszeres pingelésre függőségek projekt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="41b00-154">[This blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) describes a project to diagnose dependency issues by automatically sending regular pings to dependencies.</span></span>


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a><span data-ttu-id="41b00-155">Adja hozzá a tulajdonságok: ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="41b00-155">Add properties: ITelemetryInitializer</span></span>
<span data-ttu-id="41b00-156">Telemetria inicializálók segítségével az összes telemetriai adat; küldött általános tulajdonságainak megadása és a szabványos telemetriai modulok kijelölt működés felülbírálásához.</span><span class="sxs-lookup"><span data-stu-id="41b00-156">Use telemetry initializers to define global properties that are sent with all telemetry; and to override selected behavior of the standard telemetry modules.</span></span>

<span data-ttu-id="41b00-157">Az Application Insights webes csomag például HTTP-kérelmekre vonatkozó telemetriai adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="41b00-157">For example, the Application Insights for Web package collects telemetry about HTTP requests.</span></span> <span data-ttu-id="41b00-158">Alapértelmezés szerint azt észleli, ha bármely kérelem válaszkód sikertelenként > = 400.</span><span class="sxs-lookup"><span data-stu-id="41b00-158">By default, it flags as failed any request with a response code >= 400.</span></span> <span data-ttu-id="41b00-159">Azonban ha azt szeretné kezelni a 400 sikeres, megadhatja a telemetriai adatok inicializáló, amely beállítja a sikeres tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="41b00-159">But if you want to treat 400 as a success, you can provide a telemetry initializer that sets the Success property.</span></span>

<span data-ttu-id="41b00-160">Ha megad egy telemetriai inicializáló, azt nevezzük, amikor a Track*() módszerekkel nevezik.</span><span class="sxs-lookup"><span data-stu-id="41b00-160">If you provide a telemetry initializer, it is called whenever any of the Track*() methods is called.</span></span> <span data-ttu-id="41b00-161">Ez magában foglalja a szabványos telemetriai modulok által meghívott módszerek.</span><span class="sxs-lookup"><span data-stu-id="41b00-161">This includes methods called by the standard telemetry modules.</span></span> <span data-ttu-id="41b00-162">Konvenció ezek a modulok egyik tulajdonságnak sem, amely már be van állítva egy inicializáló által nem állít be.</span><span class="sxs-lookup"><span data-stu-id="41b00-162">By convention, these modules do not set any property that has already been set by an initializer.</span></span>

<span data-ttu-id="41b00-163">**Adja meg az inicializáló**</span><span class="sxs-lookup"><span data-stu-id="41b00-163">**Define your initializer**</span></span>

<span data-ttu-id="41b00-164">*C#*</span><span class="sxs-lookup"><span data-stu-id="41b00-164">*C#*</span></span>

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides the default SDK
       * behavior of treating response codes >= 400 as failed requests
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            var requestTelemetry = telemetry as RequestTelemetry;
            // Is this a TrackRequest() ?
            if (requestTelemetry == null) return;
            int code;
            bool parsed = Int32.TryParse(requestTelemetry.ResponseCode, out code);
            if (!parsed) return;
            if (code >= 400 && code < 500)
            {
                // If we set the Success property, the SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us to filter these requests in the portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave the SDK to set the Success property      
        }
      }
    }
```

<span data-ttu-id="41b00-165">**Az inicializáló betöltése**</span><span class="sxs-lookup"><span data-stu-id="41b00-165">**Load your initializer**</span></span>

<span data-ttu-id="41b00-166">Az ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="41b00-166">In ApplicationInsights.config:</span></span>

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

<span data-ttu-id="41b00-167">*Másik lehetőségként* az inicializáló a kód, például a Global.aspx.cs osztályból példányosítható:</span><span class="sxs-lookup"><span data-stu-id="41b00-167">*Alternatively,* you can instantiate the initializer in code, for example in Global.aspx.cs:</span></span>

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[<span data-ttu-id="41b00-168">Ez a minta több látszik.</span><span class="sxs-lookup"><span data-stu-id="41b00-168">See more of this sample.</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a><span data-ttu-id="41b00-169">JavaScript telemetriai inicializálók</span><span class="sxs-lookup"><span data-stu-id="41b00-169">JavaScript telemetry initializers</span></span>
<span data-ttu-id="41b00-170">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="41b00-170">*JavaScript*</span></span>

<span data-ttu-id="41b00-171">Szúrja be a telemetriai adatok inicializáló közvetlenül a portálon kapott inicializálási kód után:</span><span class="sxs-lookup"><span data-stu-id="41b00-171">Insert a telemetry initializer immediately after the initialization code that you got from the portal:</span></span>

```JS

    <script type="text/javascript">
        // ... initialization code
        ...({
            instrumentationKey: "your instrumentation key"
        });
        window.appInsights = appInsights;


        // Adding telemetry initializer.
        // This is called whenever a new telemetry item
        // is created.

        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;

                // To check the telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // To set custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // To set custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

<span data-ttu-id="41b00-172">Elérhető a telemetryItem nem egyéni tulajdonság, témakör [Application Insights exportálása adatmodell](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="41b00-172">For a summary of the non-custom properties available on the telemetryItem, see [Application Insights Export Data Model](app-insights-export-data-model.md).</span></span>

<span data-ttu-id="41b00-173">Tetszőleges számú inicializálók szerint adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="41b00-173">You can add as many initializers as you like.</span></span>

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a><span data-ttu-id="41b00-174">ITelemetryProcessor és ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="41b00-174">ITelemetryProcessor and ITelemetryInitializer</span></span>
<span data-ttu-id="41b00-175">Mi az a telemetriai adatok processzorok és telemetriai inicializálók közötti különbség?</span><span class="sxs-lookup"><span data-stu-id="41b00-175">What's the difference between telemetry processors and telemetry initializers?</span></span>

* <span data-ttu-id="41b00-176">Van néhány átfedéseket, mi mindent velük a: Tulajdonságok hozzáadása telemetriai egyaránt használható.</span><span class="sxs-lookup"><span data-stu-id="41b00-176">There are some overlaps in what you can do with them: both can be used to add properties to telemetry.</span></span>
* <span data-ttu-id="41b00-177">TelemetryInitializers TelemetryProcessors előtt mindig fusson.</span><span class="sxs-lookup"><span data-stu-id="41b00-177">TelemetryInitializers always run before TelemetryProcessors.</span></span>
* <span data-ttu-id="41b00-178">TelemetryProcessors engedélyezi, hogy teljesen cserélje le, vagy vesse el a telemetriai adatok elemet.</span><span class="sxs-lookup"><span data-stu-id="41b00-178">TelemetryProcessors allow you to completely replace or discard a telemetry item.</span></span>
* <span data-ttu-id="41b00-179">TelemetryProcessors teljesítmény számláló telemetriai nem feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="41b00-179">TelemetryProcessors don't process performance counter telemetry.</span></span>


## <a name="reference-docs"></a><span data-ttu-id="41b00-180">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="41b00-180">Reference docs</span></span>
* [<span data-ttu-id="41b00-181">API – Áttekintés</span><span class="sxs-lookup"><span data-stu-id="41b00-181">API Overview</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="41b00-182">ASP.NET-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="41b00-182">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a><span data-ttu-id="41b00-183">SDK-kód</span><span class="sxs-lookup"><span data-stu-id="41b00-183">SDK Code</span></span>
* [<span data-ttu-id="41b00-184">Az ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="41b00-184">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="41b00-185">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="41b00-185">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="41b00-186">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="41b00-186">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)

## <span data-ttu-id="41b00-187"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41b00-187"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="41b00-188">Keresési események és a naplókat</span><span class="sxs-lookup"><span data-stu-id="41b00-188">Search events and logs</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="41b00-189">Mintavételezés</span><span class="sxs-lookup"><span data-stu-id="41b00-189">Sampling</span></span>](app-insights-sampling.md)
* [<span data-ttu-id="41b00-190">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="41b00-190">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)

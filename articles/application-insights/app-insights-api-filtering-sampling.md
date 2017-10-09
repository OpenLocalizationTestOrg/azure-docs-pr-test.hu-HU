---
title: "aaaFiltering és a előfeldolgozása hello Azure Application Insights SDK |} Microsoft Docs"
description: "Telemetriai processzor- és Telemetria inicializálók írása hello SDK toofilter vagy tulajdonságok toohello adatok hozzáadása a hello telemetriai toohello Application Insights portál elküldése előtt."
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
ms.openlocfilehash: 51b9db69b2375b8799718f1b0e1af77620dc2692
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filtering-and-preprocessing-telemetry-in-hello-application-insights-sdk"></a><span data-ttu-id="44a30-103">Szűrés és az Application Insights SDK hello telemetriai előfeldolgozása</span><span class="sxs-lookup"><span data-stu-id="44a30-103">Filtering and preprocessing telemetry in hello Application Insights SDK</span></span>


<span data-ttu-id="44a30-104">Írása, és beépülő modulokat hello Application Insights SDK toocustomize hogyan telemetriai adatokat rögzíti, majd az Application Insights szolgáltatás toohello elküldés előtt feldolgozott konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="44a30-104">You can write and configure plug-ins for hello Application Insights SDK toocustomize how telemetry is captured and processed before it is sent toohello Application Insights service.</span></span>

* <span data-ttu-id="44a30-105">[A mintavételi](app-insights-sampling.md) anélkül, hogy befolyásolná a statisztika telemetriai hello mennyiségét csökkenti.</span><span class="sxs-lookup"><span data-stu-id="44a30-105">[Sampling](app-insights-sampling.md) reduces hello volume of telemetry without affecting your statistics.</span></span> <span data-ttu-id="44a30-106">Azonos tartja kapcsolatos adatok mutat, így közöttük, ha a probléma diagnosztizálása navigálhat.</span><span class="sxs-lookup"><span data-stu-id="44a30-106">It keeps together related data points so that you can navigate between them when diagnosing a problem.</span></span> <span data-ttu-id="44a30-107">Hello portálon hello teljes számok pedig hello mintavételi rekordokkal toocompensate.</span><span class="sxs-lookup"><span data-stu-id="44a30-107">In hello portal, hello total counts are multiplied toocompensate for hello sampling.</span></span>
* <span data-ttu-id="44a30-108">Szűrés Telemetriai processzorokat [ASP.NET](#filtering) vagy [Java](app-insights-java-filter-telemetry.md) lehetővé teszi, hogy válasszon, vagy módosítsa az hello SDK telemetriai toohello server elküldés előtt.</span><span class="sxs-lookup"><span data-stu-id="44a30-108">Filtering with Telemetry Processors [for ASP.NET](#filtering) or [Java](app-insights-java-filter-telemetry.md) lets you select or modify telemetry in hello SDK before it is sent toohello server.</span></span> <span data-ttu-id="44a30-109">Telemetriai adatok mennyisége hello csökkentheti például kérelmek kizárja a robotokat.</span><span class="sxs-lookup"><span data-stu-id="44a30-109">For example, you could reduce hello volume of telemetry by excluding requests from robots.</span></span> <span data-ttu-id="44a30-110">De szűrés mintavételi-nál több alapvető megközelítés tooreducing forgalmat.</span><span class="sxs-lookup"><span data-stu-id="44a30-110">But filtering is a more basic approach tooreducing traffic than sampling.</span></span> <span data-ttu-id="44a30-111">Mi kerül továbbításra teljesebb körű vezérlése lehetővé teszi, de vegye figyelembe, hogy az hatással van a statisztika - például toobe rendelkezik ki az összes sikeres kérelmek szűrése.</span><span class="sxs-lookup"><span data-stu-id="44a30-111">It allows you more control over what is transmitted, but you have toobe aware that it affects your statistics - for example, if you filter out all successful requests.</span></span>
* <span data-ttu-id="44a30-112">[Telemetria inicializálók tulajdonságok hozzáadása](#add-properties) tooany telemetriai adatok küldése az alkalmazásból, beleértve a szabványos modulokban hello telemetriai.</span><span class="sxs-lookup"><span data-stu-id="44a30-112">[Telemetry Initializers add properties](#add-properties) tooany telemetry sent from your app, including telemetry from hello standard modules.</span></span> <span data-ttu-id="44a30-113">Például hozzáadhatja számított értékeket; vagy verziószámok hello portálon toofilter hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="44a30-113">For example, you could add calculated values; or version numbers by which toofilter hello data in hello portal.</span></span>
* <span data-ttu-id="44a30-114">[hello SDK API](app-insights-api-custom-events-metrics.md) használt toosend egyéni események és metrikák.</span><span class="sxs-lookup"><span data-stu-id="44a30-114">[hello SDK API](app-insights-api-custom-events-metrics.md) is used toosend custom events and metrics.</span></span>

<span data-ttu-id="44a30-115">Előkészületek:</span><span class="sxs-lookup"><span data-stu-id="44a30-115">Before you start:</span></span>

* <span data-ttu-id="44a30-116">Telepítse az Application Insights hello [SDK for ASP.NET](app-insights-asp-net.md) vagy [SDK Java](app-insights-java-get-started.md) az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="44a30-116">Install hello Application Insights [SDK for ASP.NET](app-insights-asp-net.md) or [SDK for Java](app-insights-java-get-started.md) in your app.</span></span>

<a name="filtering"></a>

## <a name="filtering-itelemetryprocessor"></a><span data-ttu-id="44a30-117">Szűrés: ITelemetryProcessor</span><span class="sxs-lookup"><span data-stu-id="44a30-117">Filtering: ITelemetryProcessor</span></span>
<span data-ttu-id="44a30-118">Ez a módszer lehetővé teszi több közvetlen ellenőrzése alatt tartja a mi van, illetve tiltani szeretné hello telemetriai adatfolyamból.</span><span class="sxs-lookup"><span data-stu-id="44a30-118">This technique gives you more direct control over what is included or excluded from hello telemetry stream.</span></span> <span data-ttu-id="44a30-119">A mintavételi, párhuzamosan használható vagy külön-külön.</span><span class="sxs-lookup"><span data-stu-id="44a30-119">You can use it in conjunction with Sampling, or separately.</span></span>

<span data-ttu-id="44a30-120">toofilter telemetriai adatokat, akkor írási telemetriai processzort, és regisztrálhatja azt az hello SDK.</span><span class="sxs-lookup"><span data-stu-id="44a30-120">toofilter telemetry, you write a telemetry processor and register it with hello SDK.</span></span> <span data-ttu-id="44a30-121">A processzor végighalad az összes telemetriai adat, és választhat toodrop hello le adatfolyam, vagy vegye fel a tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="44a30-121">All telemetry goes through your processor, and you can choose toodrop it from hello stream, or add properties.</span></span> <span data-ttu-id="44a30-122">Ez magában foglalja a telemetriai hello szabványos modulokban például hello HTTP-kérelem adatgyűjtő és hello függőségi adatgyűjtő, valamint saját kezűleg írt telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="44a30-122">This includes telemetry from hello standard modules such as hello HTTP request collector and hello dependency collector, as well as telemetry you have written yourself.</span></span> <span data-ttu-id="44a30-123">Például szűrhetők telemetriai adatainak robots vagy sikeres függőségi hívások esetében érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="44a30-123">You can, for example, filter out telemetry about requests from robots, or successful dependency calls.</span></span>

> [!WARNING]
> <span data-ttu-id="44a30-124">Szűrés hello SDK által küldött hello telemetriai processzorokat használó is eltolódhat hello statisztika kapcsolatban a hello portálon, és könnyebben nehéz toofollow kapcsolódó elemeket.</span><span class="sxs-lookup"><span data-stu-id="44a30-124">Filtering hello telemetry sent from hello SDK using processors can skew hello statistics that you see in hello portal, and make it difficult toofollow related items.</span></span>
>
> <span data-ttu-id="44a30-125">Ehelyett érdemes [mintavételi](app-insights-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="44a30-125">Instead, consider using [sampling](app-insights-sampling.md).</span></span>
>
>

### <a name="create-a-telemetry-processor-c"></a><span data-ttu-id="44a30-126">Hozzon létre egy telemetriai processzor (C#)</span><span class="sxs-lookup"><span data-stu-id="44a30-126">Create a telemetry processor (C#)</span></span>
1. <span data-ttu-id="44a30-127">Győződjön meg arról, hogy hello Application Insights SDK célverzióját 2.0.0 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="44a30-127">Verify that hello Application Insights SDK in your project is  version 2.0.0 or later.</span></span> <span data-ttu-id="44a30-128">Kattintson a jobb gombbal a projektre a Visual Studio Solution Explorerben, és válassza ki a NuGet-csomagok kezelése.</span><span class="sxs-lookup"><span data-stu-id="44a30-128">Right-click your project in Visual Studio Solution Explorer and choose Manage NuGet Packages.</span></span> <span data-ttu-id="44a30-129">A NuGet-Csomagkezelő ellenőrizze a Microsoft.ApplicationInsights.Web.</span><span class="sxs-lookup"><span data-stu-id="44a30-129">In NuGet package manager, check Microsoft.ApplicationInsights.Web.</span></span>
2. <span data-ttu-id="44a30-130">a szűrő toocreate ITelemetryProcessor valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="44a30-130">toocreate a filter, implement ITelemetryProcessor.</span></span> <span data-ttu-id="44a30-131">Ez egy másik bővítési pontot például telemetriai modul, telemetriai inicializáló és telemetriai csatorna.</span><span class="sxs-lookup"><span data-stu-id="44a30-131">This is another extensibility point like telemetry module, telemetry initializer, and telemetry channel.</span></span>

    <span data-ttu-id="44a30-132">Figyelje meg, hogy a Telemetriai processzor feldolgozási láncolata összeállításához.</span><span class="sxs-lookup"><span data-stu-id="44a30-132">Notice that Telemetry Processors construct a chain of processing.</span></span> <span data-ttu-id="44a30-133">Telemetria processzort hozható létre, ha egy hivatkozás toohello következő processzor át hello lánc.</span><span class="sxs-lookup"><span data-stu-id="44a30-133">When you instantiate a telemetry processor, you pass a link toohello next processor in hello chain.</span></span> <span data-ttu-id="44a30-134">Ha a telemetriai adatok adatpont toohello folyamat metódus, a tevékenységeket végez el, és majd hívások hello hello láncban következő Telemetriai processzor.</span><span class="sxs-lookup"><span data-stu-id="44a30-134">When a telemetry data point is passed toohello Process method, it does its work and then calls hello next Telemetry Processor in hello chain.</span></span>

    ``` C#

    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    public class SuccessfulDependencyFilter : ITelemetryProcessor
      {

        private ITelemetryProcessor Next { get; set; }

        // You can pass values from .config
        public string MyParamFromConfigFile { get; set; }

        // Link processors tooeach other in a chain.
        public SuccessfulDependencyFilter(ITelemetryProcessor next)
        {
            this.Next = next;
        }
        public void Process(ITelemetry item)
        {
            // toofilter out an item, just return
            if (!OKtoSend(item)) { return; }
            // Modify hello item if required
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
1. <span data-ttu-id="44a30-135">Helyezze be a ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="44a30-135">Insert this in ApplicationInsights.config:</span></span>

```XML

    <TelemetryProcessors>
      <Add Type="WebApplication9.SuccessfulDependencyFilter, WebApplication9">
         <!-- Set public property -->
         <MyParamFromConfigFile>2-beta</MyParamFromConfigFile>
      </Add>
    </TelemetryProcessors>

```

<span data-ttu-id="44a30-136">(Ez az hello ahol inicializálni a mintavételi szűrő szakaszában azonos.)</span><span class="sxs-lookup"><span data-stu-id="44a30-136">(This is hello same section where you initialize a sampling filter.)</span></span>

<span data-ttu-id="44a30-137">Az osztály nyilvános elnevezett tulajdonságok megadásával átadhatók karakterlánc-értékek hello .config fájlból.</span><span class="sxs-lookup"><span data-stu-id="44a30-137">You can pass string values from hello .config file by providing public named properties in your class.</span></span>

> [!WARNING]
> <span data-ttu-id="44a30-138">Mi gondoskodunk toomatch hello nevét és minden tulajdonságnevek hello .config fájl toohello osztály és a tulajdonságnevek hello kódban.</span><span class="sxs-lookup"><span data-stu-id="44a30-138">Take care toomatch hello type name and any property names in hello .config file toohello class and property names in hello code.</span></span> <span data-ttu-id="44a30-139">Ha hello .config fájl egy nem létező típus vagy a tulajdonságra hivatkozik, hello SDK csendes sikertelenek lehetnek toosend bármely telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="44a30-139">If hello .config file references a non-existent type or property, hello SDK may silently fail toosend any telemetry.</span></span>
>
>

<span data-ttu-id="44a30-140">**Másik lehetőségként** tudja inicializálni a kódban hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="44a30-140">**Alternatively,** you can initialize hello filter in code.</span></span> <span data-ttu-id="44a30-141">A megfelelő inicializálási osztályban – például a Global.asax.cs AppStart - hello lánc a processzor beszúrása:</span><span class="sxs-lookup"><span data-stu-id="44a30-141">In a suitable initialization class - for example AppStart in Global.asax.cs - insert your processor into hello chain:</span></span>

```C#

    var builder = TelemetryConfiguration.Active.TelemetryProcessorChainBuilder;
    builder.Use((next) => new SuccessfulDependencyFilter(next));

    // If you have more processors:
    builder.Use((next) => new AnotherProcessor(next));

    builder.Build();

```

<span data-ttu-id="44a30-142">Ezt a pontot fogja használni a processzor után létrehozott TelemetryClients.</span><span class="sxs-lookup"><span data-stu-id="44a30-142">TelemetryClients created after this point will use your processors.</span></span>

### <a name="example-filters"></a><span data-ttu-id="44a30-143">Példa szűrők</span><span class="sxs-lookup"><span data-stu-id="44a30-143">Example filters</span></span>
#### <a name="synthetic-requests"></a><span data-ttu-id="44a30-144">Szintetikus kérelmek</span><span class="sxs-lookup"><span data-stu-id="44a30-144">Synthetic requests</span></span>
<span data-ttu-id="44a30-145">Botok és a webalkalmazás-tesztek szűrik.</span><span class="sxs-lookup"><span data-stu-id="44a30-145">Filter out bots and web tests.</span></span> <span data-ttu-id="44a30-146">Bár a Metrikaböngésző ad meg hello kimenő szintetikus források beállítás toofilter, ezt a beállítást, hello SDK szűréssel csökkenti a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="44a30-146">Although Metrics Explorer gives you hello option toofilter out synthetic sources, this option reduces traffic by filtering them at hello SDK.</span></span>

``` C#

    public void Process(ITelemetry item)
    {
      if (!string.IsNullOrEmpty(item.Context.Operation.SyntheticSource)) {return;}

      // Send everything else:
      this.Next.Process(item);
    }

```

#### <a name="failed-authentication"></a><span data-ttu-id="44a30-147">Sikertelen hitelesítési</span><span class="sxs-lookup"><span data-stu-id="44a30-147">Failed authentication</span></span>
<span data-ttu-id="44a30-148">Szűrheti kérések "401-es" választ.</span><span class="sxs-lookup"><span data-stu-id="44a30-148">Filter out requests with a "401" response.</span></span>

```C#

public void Process(ITelemetry item)
{
    var request = item as RequestTelemetry;

    if (request != null &&
    request.ResponseCode.Equals("401", StringComparison.OrdinalIgnoreCase))
    {
        // toofilter out an item, just terminate hello chain:
        return;
    }
    // Send everything else:
    this.Next.Process(item);
}

```

#### <a name="filter-out-fast-remote-dependency-calls"></a><span data-ttu-id="44a30-149">Kiszűrhetők a gyorsan távoli függőségi hívások esetében</span><span class="sxs-lookup"><span data-stu-id="44a30-149">Filter out fast remote dependency calls</span></span>
<span data-ttu-id="44a30-150">Ha azt szeretné, amelyek lassú hívások toodiagnose csak, kiszűrhetők a hello gyors néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="44a30-150">If you only want toodiagnose calls that are slow, filter out hello fast ones.</span></span>

> [!NOTE]
> <span data-ttu-id="44a30-151">Ez fogja döntés hello statisztika hello portálon megjelenik.</span><span class="sxs-lookup"><span data-stu-id="44a30-151">This will skew hello statistics you see on hello portal.</span></span> <span data-ttu-id="44a30-152">hello függőségi diagram fog kinézni, mintha hello függőségi hívások esetében is az összes sikertelen.</span><span class="sxs-lookup"><span data-stu-id="44a30-152">hello dependency chart will look as if hello dependency calls are all failures.</span></span>
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

#### <a name="diagnose-dependency-issues"></a><span data-ttu-id="44a30-153">Függőségi problémák diagnosztikája</span><span class="sxs-lookup"><span data-stu-id="44a30-153">Diagnose dependency issues</span></span>
<span data-ttu-id="44a30-154">[Ebben a blogban](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) rendszeres pingelésre toodependencies automatikusan küldésével egy projekt toodiagnose függőségi problémákat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="44a30-154">[This blog](https://azure.microsoft.com/blog/implement-an-application-insights-telemetry-processor/) describes a project toodiagnose dependency issues by automatically sending regular pings toodependencies.</span></span>


<a name="add-properties"></a>

## <a name="add-properties-itelemetryinitializer"></a><span data-ttu-id="44a30-155">Adja hozzá a tulajdonságok: ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="44a30-155">Add properties: ITelemetryInitializer</span></span>
<span data-ttu-id="44a30-156">Használja a telemetriai adatok inicializálók toodefine globális tulajdonságait küldött az összes telemetriai adat; és toooverride kijelölt hello szabványos telemetriai modulok viselkedését.</span><span class="sxs-lookup"><span data-stu-id="44a30-156">Use telemetry initializers toodefine global properties that are sent with all telemetry; and toooverride selected behavior of hello standard telemetry modules.</span></span>

<span data-ttu-id="44a30-157">Hello Application Insights webes csomag például HTTP-kérelmekre vonatkozó telemetriai adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="44a30-157">For example, hello Application Insights for Web package collects telemetry about HTTP requests.</span></span> <span data-ttu-id="44a30-158">Alapértelmezés szerint azt észleli, ha bármely kérelem válaszkód sikertelenként > = 400.</span><span class="sxs-lookup"><span data-stu-id="44a30-158">By default, it flags as failed any request with a response code >= 400.</span></span> <span data-ttu-id="44a30-159">Azonban ha azt szeretné, hogy egy sikeres 400 tootreat, megadhatja a telemetriai adatok inicializáló, amely beállítja hello sikeres tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="44a30-159">But if you want tootreat 400 as a success, you can provide a telemetry initializer that sets hello Success property.</span></span>

<span data-ttu-id="44a30-160">Ha megad egy telemetriai inicializáló, ha bármelyik hello Track*() nevezik módszert nevezik.</span><span class="sxs-lookup"><span data-stu-id="44a30-160">If you provide a telemetry initializer, it is called whenever any of hello Track*() methods is called.</span></span> <span data-ttu-id="44a30-161">Ez magában foglalja a hello szabványos telemetriai modulok által meghívott módszerek.</span><span class="sxs-lookup"><span data-stu-id="44a30-161">This includes methods called by hello standard telemetry modules.</span></span> <span data-ttu-id="44a30-162">Konvenció ezek a modulok egyik tulajdonságnak sem, amely már be van állítva egy inicializáló által nem állít be.</span><span class="sxs-lookup"><span data-stu-id="44a30-162">By convention, these modules do not set any property that has already been set by an initializer.</span></span>

<span data-ttu-id="44a30-163">**Adja meg az inicializáló**</span><span class="sxs-lookup"><span data-stu-id="44a30-163">**Define your initializer**</span></span>

<span data-ttu-id="44a30-164">*C#*</span><span class="sxs-lookup"><span data-stu-id="44a30-164">*C#*</span></span>

```C#

    using System;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.DataContracts;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that overrides hello default SDK
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
                // If we set hello Success property, hello SDK won't change it:
                requestTelemetry.Success = true;
                // Allow us toofilter these requests in hello portal:
                requestTelemetry.Context.Properties["Overridden400s"] = "true";
            }
            // else leave hello SDK tooset hello Success property      
        }
      }
    }
```

<span data-ttu-id="44a30-165">**Az inicializáló betöltése**</span><span class="sxs-lookup"><span data-stu-id="44a30-165">**Load your initializer**</span></span>

<span data-ttu-id="44a30-166">Az ApplicationInsights.config:</span><span class="sxs-lookup"><span data-stu-id="44a30-166">In ApplicationInsights.config:</span></span>

    <ApplicationInsights>
      <TelemetryInitializers>
        <!-- Fully qualified type name, assembly name: -->
        <Add Type="MvcWebRole.Telemetry.MyTelemetryInitializer, MvcWebRole"/>
        ...
      </TelemetryInitializers>
    </ApplicationInsights>

<span data-ttu-id="44a30-167">*Másik lehetőségként* hello inicializáló a kód, például a Global.aspx.cs osztályból példányosítható:</span><span class="sxs-lookup"><span data-stu-id="44a30-167">*Alternatively,* you can instantiate hello initializer in code, for example in Global.aspx.cs:</span></span>

```C#
    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }
```


[<span data-ttu-id="44a30-168">Ez a minta több látszik.</span><span class="sxs-lookup"><span data-stu-id="44a30-168">See more of this sample.</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole)

<a name="js-initializer"></a>

### <a name="javascript-telemetry-initializers"></a><span data-ttu-id="44a30-169">JavaScript telemetriai inicializálók</span><span class="sxs-lookup"><span data-stu-id="44a30-169">JavaScript telemetry initializers</span></span>
<span data-ttu-id="44a30-170">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="44a30-170">*JavaScript*</span></span>

<span data-ttu-id="44a30-171">Szúrja be a telemetriai adatok inicializáló közvetlenül a portáltól kapott hello portal hello inicializálási kód után:</span><span class="sxs-lookup"><span data-stu-id="44a30-171">Insert a telemetry initializer immediately after hello initialization code that you got from hello portal:</span></span>

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

                // toocheck hello telemetry item’s type - for example PageView:
                if (envelope.name == Microsoft.ApplicationInsights.Telemetry.PageView.envelopeType) {
                    // this statement removes url from all page view documents
                    telemetryItem.url = "URL CENSORED";
                }

                // tooset custom properties:
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["globalProperty"] = "boo";

                // tooset custom metrics:
                telemetryItem.measurements = telemetryItem.measurements || {};
                telemetryItem.measurements["globalMetric"] = 100;
            });
        });

        // End of inserted code.

        appInsights.trackPageView();
    </script>
```

<span data-ttu-id="44a30-172">Hello nem egyéni tulajdonságok hello telemetryItem elérhető összefoglalását lásd: [Application Insights exportálása adatmodell](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="44a30-172">For a summary of hello non-custom properties available on hello telemetryItem, see [Application Insights Export Data Model](app-insights-export-data-model.md).</span></span>

<span data-ttu-id="44a30-173">Tetszőleges számú inicializálók szerint adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="44a30-173">You can add as many initializers as you like.</span></span>

## <a name="itelemetryprocessor-and-itelemetryinitializer"></a><span data-ttu-id="44a30-174">ITelemetryProcessor és ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="44a30-174">ITelemetryProcessor and ITelemetryInitializer</span></span>
<span data-ttu-id="44a30-175">Mi az a telemetriai adatok processzorral és telemetriai inicializálók hello különbségének?</span><span class="sxs-lookup"><span data-stu-id="44a30-175">What's hello difference between telemetry processors and telemetry initializers?</span></span>

* <span data-ttu-id="44a30-176">Van néhány átfedéseket, mi mindent velük a: használt tooadd tulajdonságok tootelemetry is lehet.</span><span class="sxs-lookup"><span data-stu-id="44a30-176">There are some overlaps in what you can do with them: both can be used tooadd properties tootelemetry.</span></span>
* <span data-ttu-id="44a30-177">TelemetryInitializers TelemetryProcessors előtt mindig fusson.</span><span class="sxs-lookup"><span data-stu-id="44a30-177">TelemetryInitializers always run before TelemetryProcessors.</span></span>
* <span data-ttu-id="44a30-178">TelemetryProcessors toocompletely csere engedélyezése, vagy vesse el a telemetriai adatok elemet.</span><span class="sxs-lookup"><span data-stu-id="44a30-178">TelemetryProcessors allow you toocompletely replace or discard a telemetry item.</span></span>
* <span data-ttu-id="44a30-179">TelemetryProcessors teljesítmény számláló telemetriai nem feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="44a30-179">TelemetryProcessors don't process performance counter telemetry.</span></span>


## <a name="reference-docs"></a><span data-ttu-id="44a30-180">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="44a30-180">Reference docs</span></span>
* [<span data-ttu-id="44a30-181">API – Áttekintés</span><span class="sxs-lookup"><span data-stu-id="44a30-181">API Overview</span></span>](app-insights-api-custom-events-metrics.md)
* [<span data-ttu-id="44a30-182">ASP.NET-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="44a30-182">ASP.NET reference</span></span>](https://msdn.microsoft.com/library/dn817570.aspx)

## <a name="sdk-code"></a><span data-ttu-id="44a30-183">SDK-kód</span><span class="sxs-lookup"><span data-stu-id="44a30-183">SDK Code</span></span>
* [<span data-ttu-id="44a30-184">Az ASP.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="44a30-184">ASP.NET Core SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [<span data-ttu-id="44a30-185">ASP.NET 5</span><span class="sxs-lookup"><span data-stu-id="44a30-185">ASP.NET 5</span></span>](https://github.com/Microsoft/ApplicationInsights-aspnet5)
* [<span data-ttu-id="44a30-186">JavaScript SDK</span><span class="sxs-lookup"><span data-stu-id="44a30-186">JavaScript SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-JS)

## <span data-ttu-id="44a30-187"><a name="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44a30-187"><a name="next"></a>Next steps</span></span>
* [<span data-ttu-id="44a30-188">Keresési események és a naplókat</span><span class="sxs-lookup"><span data-stu-id="44a30-188">Search events and logs</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="44a30-189">Mintavételezés</span><span class="sxs-lookup"><span data-stu-id="44a30-189">Sampling</span></span>](app-insights-sampling.md)
* [<span data-ttu-id="44a30-190">hibaelhárítással</span><span class="sxs-lookup"><span data-stu-id="44a30-190">Troubleshooting</span></span>](app-insights-troubleshoot-faq.md)

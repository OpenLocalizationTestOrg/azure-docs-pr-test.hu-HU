---
title: "a Java-webalkalmazás az Azure Application Insights telemetria aaaFilter |} Microsoft Docs"
description: "Telemetria forgalom csökkentése hello események kiszűrésével toomonitor nem szükséges."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: bwren
ms.openlocfilehash: 95713e11d5f86472777c67e4e7f3177fbf2cd0b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filter-telemetry-in-your-java-web-app"></a><span data-ttu-id="e8aa6-103">A Java-webalkalmazás az telemetriai szűrése</span><span class="sxs-lookup"><span data-stu-id="e8aa6-103">Filter telemetry in your Java web app</span></span>

<span data-ttu-id="e8aa6-104">Szűrők adjon meg egy módon tooselect hello telemetriai adat, amely a [Java-webalkalmazás küld tooApplication Insights](app-insights-java-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e8aa6-104">Filters provide a way tooselect hello telemetry that your [Java web app sends tooApplication Insights](app-insights-java-get-started.md).</span></span> <span data-ttu-id="e8aa6-105">Néhány, a-kész szűrő használható, és a saját egyéni szűrők is írhat.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-105">There are some out-of-the-box filters that you can use, and you can also write your own custom filters.</span></span>

<span data-ttu-id="e8aa6-106">hello out-of-az-box szűrők a következők:</span><span class="sxs-lookup"><span data-stu-id="e8aa6-106">hello out-of-the-box filters include:</span></span>

* <span data-ttu-id="e8aa6-107">Nyomkövetési súlyossági szint</span><span class="sxs-lookup"><span data-stu-id="e8aa6-107">Trace severity level</span></span>
* <span data-ttu-id="e8aa6-108">Adott URL-címek, kulcsszavak vagy válaszkódok</span><span class="sxs-lookup"><span data-stu-id="e8aa6-108">Specific URLs, keywords or response codes</span></span>
* <span data-ttu-id="e8aa6-109">Ez azt jelenti, hogy a kérelmek toowhich gyorsan megválaszolhatók - az alkalmazás válaszolt tooquickly</span><span class="sxs-lookup"><span data-stu-id="e8aa6-109">Fast responses - that is, requests toowhich your app responded tooquickly</span></span>
* <span data-ttu-id="e8aa6-110">Adott események neve</span><span class="sxs-lookup"><span data-stu-id="e8aa6-110">Specific event names</span></span>

> [!NOTE]
> <span data-ttu-id="e8aa6-111">Szűrők az alkalmazás hello metrikák eltolódhat.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-111">Filters skew hello metrics of your app.</span></span> <span data-ttu-id="e8aa6-112">Dönthet például úgy, toodiagnose lassú válaszokban ahhoz, hogy beállítsa a szűrő toodiscard gyors válaszidők.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-112">For example, you might decide that, in order toodiagnose slow responses, you will set a filter toodiscard fast response times.</span></span> <span data-ttu-id="e8aa6-113">De vegye figyelembe, hogy hello átlagos válaszideje az Application Insights által jelentett hello sebesség lassabb lesz, és a kérelmek hello száma kisebb, mint hello valós szám lesz.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-113">But you must be aware that hello average response times reported by Application Insights will then be slower than hello true speed, and hello count of requests will be smaller than hello real count.</span></span>
> <span data-ttu-id="e8aa6-114">Ha ez problémát jelent, akkor [mintavételi](app-insights-sampling.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-114">If this is a concern, use [Sampling](app-insights-sampling.md) instead.</span></span>

## <a name="setting-filters"></a><span data-ttu-id="e8aa6-115">Szűrők beállítása</span><span class="sxs-lookup"><span data-stu-id="e8aa6-115">Setting filters</span></span>

<span data-ttu-id="e8aa6-116">A ApplicationInsights.xml, adjon hozzá egy `TelemetryProcessors` szakasz ebben a példában például:</span><span class="sxs-lookup"><span data-stu-id="e8aa6-116">In ApplicationInsights.xml, add a `TelemetryProcessors` section like this example:</span></span>


```XML

    <ApplicationInsights>
      <TelemetryProcessors>

        <BuiltInProcessors>
           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="100"/>
                  <Add name="NotNeededResponseCodes" value="200-400"/>
           </Processor>

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="100"/>
                  <Add name="NotNeededNames" value="home,index"/>
                  <Add name="NotNeededUrls" value=".jpg,.css"/>
           </Processor>

           <Processor type="TelemetryEventFilter">
                  <!-- Names of events we don't want toosee -->
                  <Add name="NotNeededNames" value="Start,Stop,Pause"/>
           </Processor>

           <!-- Exclude telemetry from availability tests and bots -->
           <Processor type="SyntheticSourceFilter">
                <!-- Optional: specify which synthetic sources,
                     comma-separated
                     - default is all synthetics -->
                <Add name="NotNeededSources" value="Application Insights Availability Monitoring,BingPreview"
           </Processor>

        </BuiltInProcessors>

        <CustomProcessors>
          <Processor type="com.fabrikam.MyFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>

      </TelemetryProcessors>
    </ApplicationInsights>

```




<span data-ttu-id="e8aa6-117">[Vizsgálja meg a beépített processzor teljes készletét hello](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span><span class="sxs-lookup"><span data-stu-id="e8aa6-117">[Inspect hello full set of built-in processors](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).</span></span>

## <a name="built-in-filters"></a><span data-ttu-id="e8aa6-118">A beépített szűrők</span><span class="sxs-lookup"><span data-stu-id="e8aa6-118">Built-in filters</span></span>

### <a name="metric-telemetry-filter"></a><span data-ttu-id="e8aa6-119">Metrika Telemetriai szűrő</span><span class="sxs-lookup"><span data-stu-id="e8aa6-119">Metric Telemetry filter</span></span>

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* <span data-ttu-id="e8aa6-120">`NotNeeded`-Egyéni metrika nevek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-120">`NotNeeded` - Comma-separated list of custom metric names.</span></span>


### <a name="page-view-telemetry-filter"></a><span data-ttu-id="e8aa6-121">Lap nézet Telemetriai szűrő</span><span class="sxs-lookup"><span data-stu-id="e8aa6-121">Page View Telemetry filter</span></span>

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* <span data-ttu-id="e8aa6-122">`DurationThresholdInMS`-Időtartam toohello idő tooload hello lap hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-122">`DurationThresholdInMS` - Duration refers toohello time taken tooload hello page.</span></span> <span data-ttu-id="e8aa6-123">Ha ez be van állítva, a lapok, amelyek a gyorsabb, mint a jelenleg betöltött nem jelenti.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-123">If this is set, pages that loaded faster than this time are not reported.</span></span>
* <span data-ttu-id="e8aa6-124">`NotNeededNames`-Lap neve vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-124">`NotNeededNames` - Comma-separated list of page names.</span></span>
* <span data-ttu-id="e8aa6-125">`NotNeededUrls`-Töredékei URL vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-125">`NotNeededUrls` - Comma-separated list of URL fragments.</span></span> <span data-ttu-id="e8aa6-126">Például `"home"` kiszűri összes lapok, amelyek "otthoni" hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-126">For example, `"home"` filters out all pages that have "home" in hello URL.</span></span>


### <a name="request-telemetry-filter"></a><span data-ttu-id="e8aa6-127">Telemetria szűrő kérése</span><span class="sxs-lookup"><span data-stu-id="e8aa6-127">Request Telemetry Filter</span></span>


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a><span data-ttu-id="e8aa6-128">Szintetikus forrásszűrő</span><span class="sxs-lookup"><span data-stu-id="e8aa6-128">Synthetic Source filter</span></span>

<span data-ttu-id="e8aa6-129">Hello SyntheticSource tulajdonság értéket tartalmazó összes telemetriai adat kiszűri.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-129">Filters out all telemetry that have values in hello SyntheticSource property.</span></span> <span data-ttu-id="e8aa6-130">Ezek közé tartozik a botok, csillag és rendelkezésreállás figyelésére szolgáló tesztek érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-130">These include requests from bots, spiders and availability tests.</span></span>

<span data-ttu-id="e8aa6-131">Összes szintetikus kérelem telemetriai szűrheti:</span><span class="sxs-lookup"><span data-stu-id="e8aa6-131">Filter out telemetry for all synthetic requests:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" />
```

<span data-ttu-id="e8aa6-132">Adott szintetikus források telemetriai szűrheti:</span><span class="sxs-lookup"><span data-stu-id="e8aa6-132">Filter out telemetry for specific synthetic sources:</span></span>


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* <span data-ttu-id="e8aa6-133">`NotNeeded`-Szintetikus adatforrások nevének vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-133">`NotNeeded` - Comma-separated list of synthetic source names.</span></span>

### <a name="telemetry-event-filter"></a><span data-ttu-id="e8aa6-134">Telemetria szűrő</span><span class="sxs-lookup"><span data-stu-id="e8aa6-134">Telemetry Event filter</span></span>

<span data-ttu-id="e8aa6-135">Egyéni események szűrése (használja a rendszer naplózza [trackevent() függvény](app-insights-api-custom-events-metrics.md#trackevent)).</span><span class="sxs-lookup"><span data-stu-id="e8aa6-135">Filters custom events (logged using [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent)).</span></span>


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* <span data-ttu-id="e8aa6-136">`NotNeededNames`-Esemény nevek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-136">`NotNeededNames` - Comma-separated list of event names.</span></span>


### <a name="trace-telemetry-filter"></a><span data-ttu-id="e8aa6-137">Nyomkövetési Telemetria szűrő</span><span class="sxs-lookup"><span data-stu-id="e8aa6-137">Trace Telemetry filter</span></span>

<span data-ttu-id="e8aa6-138">Szűrők nyomkövetési naplófájl (használja a rendszer naplózza [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) vagy egy [naplózási keretrendszer adatgyűjtő](app-insights-java-trace-logs.md)).</span><span class="sxs-lookup"><span data-stu-id="e8aa6-138">Filters log traces (logged using [TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) or a [logging framework collector](app-insights-java-trace-logs.md)).</span></span>

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* <span data-ttu-id="e8aa6-139">`FromSeverityLevel`érvényes értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="e8aa6-139">`FromSeverityLevel` valid values are:</span></span>
 *  <span data-ttu-id="e8aa6-140">KI - szűrheti az összes olyan nyomkövetés</span><span class="sxs-lookup"><span data-stu-id="e8aa6-140">OFF             - Filter out ALL traces</span></span>
 *  <span data-ttu-id="e8aa6-141">NYOMKÖVETÉS - nincs szűrés.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-141">TRACE           - No filtering.</span></span> <span data-ttu-id="e8aa6-142">tooTrace szint egyenlő</span><span class="sxs-lookup"><span data-stu-id="e8aa6-142">equals tooTrace level</span></span>
 *  <span data-ttu-id="e8aa6-143">INFORMÁCIÓ - szűrő kimenő NYOMKÖVETÉSI szint</span><span class="sxs-lookup"><span data-stu-id="e8aa6-143">INFO            - Filter out TRACE level</span></span>
 *  <span data-ttu-id="e8aa6-144">Figyelmeztetés - szűrő NYOMKÖVETÉSI és információ</span><span class="sxs-lookup"><span data-stu-id="e8aa6-144">WARN            - Filter out TRACE and INFO</span></span>
 *  <span data-ttu-id="e8aa6-145">Hiba – kimenő figyelmeztetés, információ, a NYOMKÖVETÉSI szűrő</span><span class="sxs-lookup"><span data-stu-id="e8aa6-145">ERROR           - Filter out WARN, INFO, TRACE</span></span>
 *  <span data-ttu-id="e8aa6-146">KRITIKUS - szűrő, az összes kritikus</span><span class="sxs-lookup"><span data-stu-id="e8aa6-146">CRITICAL        - filter out all but CRITICAL</span></span>


## <a name="custom-filters"></a><span data-ttu-id="e8aa6-147">Egyéni szűrők</span><span class="sxs-lookup"><span data-stu-id="e8aa6-147">Custom filters</span></span>

### <a name="1-code-your-filter"></a><span data-ttu-id="e8aa6-148">1. A szűrő-kód</span><span class="sxs-lookup"><span data-stu-id="e8aa6-148">1. Code your filter</span></span>

<span data-ttu-id="e8aa6-149">A kódban, hozzon létre egy osztályt, amely megvalósítja az `TelemetryProcessor`:</span><span class="sxs-lookup"><span data-stu-id="e8aa6-149">In your code, create a class that implements `TelemetryProcessor`:</span></span>

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required toosupport hello filter.*/
       private final String successful;

       /* Initializers for hello parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry toobe sent.
          Return false toodiscard it.
          Return true tooallow other processors tooinspect it. */
       @Override
       public boolean process(Telemetry telemetry) {
        if (telemetry == null) { return true; }
        if (telemetry instanceof RequestTelemetry)
        {
            RequestTelemetry requestTelemetry = (RequestTelemetry)telemetry;
            return request.getSuccess() == successful;
        }
        return true;
       }
    }

```


### <a name="2-invoke-your-filter-in-hello-configuration-file"></a><span data-ttu-id="e8aa6-150">2. A szűrő hello konfigurációs fájlban meghívása</span><span class="sxs-lookup"><span data-stu-id="e8aa6-150">2. Invoke your filter in hello configuration file</span></span>

<span data-ttu-id="e8aa6-151">A ApplicationInsights.xml:</span><span class="sxs-lookup"><span data-stu-id="e8aa6-151">In ApplicationInsights.xml:</span></span>

```XML


    <ApplicationInsights>
      <TelemetryProcessors>
        <CustomProcessors>
          <Processor type="com.fabrikam.SuccessFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>
      </TelemetryProcessors>
    </ApplicationInsights>

```

## <a name="troubleshooting"></a><span data-ttu-id="e8aa6-152">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="e8aa6-152">Troubleshooting</span></span>

<span data-ttu-id="e8aa6-153">*A szűrő nem működik.*</span><span class="sxs-lookup"><span data-stu-id="e8aa6-153">*My filter isn't working.*</span></span>

* <span data-ttu-id="e8aa6-154">Ellenőrizze, hogy megadta-e érvényes paraméterértékekért.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-154">Check that you have provided valid parameter values.</span></span> <span data-ttu-id="e8aa6-155">Például időtartamok egész számnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-155">For example, durations should be integers.</span></span> <span data-ttu-id="e8aa6-156">Érvénytelen értékeket, akkor hello szűrő toobe figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-156">Invalid values will cause hello filter toobe ignored.</span></span> <span data-ttu-id="e8aa6-157">Ha az egyéni szűrő konstruktor vagy set metódus kivételt jelez, akkor figyelmen kívül.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-157">If your custom filter throws an exception from a constructor or set method, it will be ignored.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e8aa6-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e8aa6-158">Next steps</span></span>

* <span data-ttu-id="e8aa6-159">[A mintavételi](app-insights-sampling.md) -érdemes lehet a metrikákat nem döntés alternatívájaként mintavételi.</span><span class="sxs-lookup"><span data-stu-id="e8aa6-159">[Sampling](app-insights-sampling.md) - Consider sampling as an alternative that does not skew your metrics.</span></span>

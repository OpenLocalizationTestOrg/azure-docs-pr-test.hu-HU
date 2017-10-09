---
title: "aaaApplication Insights Java webes alkalmazásokat, amelyek már élő"
description: "A kiszolgálón már futó webalkalmazás figyelése"
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/10/2016
ms.author: bwren
ms.openlocfilehash: 2b01cd61657522ccf1d2d97b2a29cdeb08ec9a18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a><span data-ttu-id="5390a-103">Az Application Insights Java webes alkalmazásokhoz, amelyek még élő</span><span class="sxs-lookup"><span data-stu-id="5390a-103">Application Insights for Java web apps that are already live</span></span>


<span data-ttu-id="5390a-104">Ha már a J2EE kiszolgálón futó webalkalmazás, figyelés megkezdése a [Application Insights](app-insights-overview.md) hello nélkül kell toomake módosítások code vagy fordítsa újra a projektet.</span><span class="sxs-lookup"><span data-stu-id="5390a-104">If you have a web application that is already running on your J2EE server, you can start monitoring it with [Application Insights](app-insights-overview.md) without hello need toomake code changes or recompile your project.</span></span> <span data-ttu-id="5390a-105">Ezzel a lehetőséggel, HTTP-kérelmek küldött tooyour server, a nem kezelt kivételek és a teljesítményszámlálók adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5390a-105">With this option, you get information about HTTP requests sent tooyour server, unhandled exceptions, and performance counters.</span></span>

<span data-ttu-id="5390a-106">Szüksége lesz egy előfizetés túl[Microsoft Azure](https://azure.com).</span><span class="sxs-lookup"><span data-stu-id="5390a-106">You'll need a subscription too[Microsoft Azure](https://azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="5390a-107">Ezen az oldalon hello eljárás hello SDK tooyour webalkalmazás futásidejű ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="5390a-107">hello procedure on this page adds hello SDK tooyour web app at runtime.</span></span> <span data-ttu-id="5390a-108">A futásidejű instrumentation akkor hasznos, ha nem szeretné, hogy tooupdate vagy építse újra a forráskódot.</span><span class="sxs-lookup"><span data-stu-id="5390a-108">This runtime instrumentation is useful if you don't want tooupdate or rebuild your source code.</span></span> <span data-ttu-id="5390a-109">De ha lehetséges, javasoljuk, hogy [hello SDK toohello forráskód hozzáadása](app-insights-java-get-started.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="5390a-109">But if you can, we recommend you [add hello SDK toohello source code](app-insights-java-get-started.md) instead.</span></span> <span data-ttu-id="5390a-110">Amely esetén több lehetősége is van például kód tootrack felhasználói tevékenység írása.</span><span class="sxs-lookup"><span data-stu-id="5390a-110">That gives you more options such as writing code tootrack user activity.</span></span>
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="5390a-111">1. Application Insights-kialakítási kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="5390a-111">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="5390a-112">Jelentkezzen be toohello [Microsoft Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="5390a-112">Sign in toohello [Microsoft Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="5390a-113">Hozzon létre egy új Application Insights-erőforrást, és állítsa be a hello alkalmazás típusú tooJava webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5390a-113">Create a new Application Insights resource and set hello application type tooJava web application.</span></span>
   
    ![Adjon meg egy nevet, válassza ki a Java webalkalmazást, és kattintson a Létrehozás gombra.](./media/app-insights-java-live/02-create.png)

    <span data-ttu-id="5390a-115">hello erőforrás néhány másodpercen belül jön létre.</span><span class="sxs-lookup"><span data-stu-id="5390a-115">hello resource is created in a few seconds.</span></span>

4. <span data-ttu-id="5390a-116">Nyissa meg a hello új erőforrás és a rendszerállapot-kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="5390a-116">Open hello new resource and get its instrumentation key.</span></span> <span data-ttu-id="5390a-117">Szüksége lesz toopaste ezt a kulcsot a kód projektben hamarosan.</span><span class="sxs-lookup"><span data-stu-id="5390a-117">You'll need toopaste this key into your code project shortly.</span></span>
   
    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-live/03-key.png)

## <a name="2-download-hello-sdk"></a><span data-ttu-id="5390a-119">2. Hello SDK letöltése</span><span class="sxs-lookup"><span data-stu-id="5390a-119">2. Download hello SDK</span></span>
1. <span data-ttu-id="5390a-120">Töltse le a hello [Javához készült Application Insights SDK](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="5390a-120">Download hello [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span> 
2. <span data-ttu-id="5390a-121">A kiszolgálón bontsa ki a hello SDK tartalma toohello könyvtárát, amelyből a projekt bináris be van töltve.</span><span class="sxs-lookup"><span data-stu-id="5390a-121">On your server, extract hello SDK contents toohello directory from which your project binaries are loaded.</span></span> <span data-ttu-id="5390a-122">Ha Tomcat használata esetén ez a könyvtár általában kell a`webapps/<your_app_name>/WEB-INF/lib`</span><span class="sxs-lookup"><span data-stu-id="5390a-122">If you’re using Tomcat, this directory would typically be under `webapps/<your_app_name>/WEB-INF/lib`</span></span>

<span data-ttu-id="5390a-123">Ne feledje, hogy toorepeat ez összes server-példányt, és az egyes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="5390a-123">Note that you need toorepeat this on each server instance, and for each app.</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="5390a-124">3. Az Application Insights XML-fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5390a-124">3. Add an Application Insights xml file</span></span>
<span data-ttu-id="5390a-125">Hozzon létre ApplicationInsights.xml hello SDK hozzá hello mappában.</span><span class="sxs-lookup"><span data-stu-id="5390a-125">Create ApplicationInsights.xml in hello folder in which you added hello SDK.</span></span> <span data-ttu-id="5390a-126">Kerüljenek, a következő XML hello.</span><span class="sxs-lookup"><span data-stu-id="5390a-126">Put into it hello following XML.</span></span>

<span data-ttu-id="5390a-127">Helyettesítő hello instrumentation kulcs portáltól kapott hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5390a-127">Substitute hello instrumentation key that you got from hello Azure portal.</span></span>

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- hello key from hello portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data tooeach event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* <span data-ttu-id="5390a-128">minden telemetriai tétel együtt küldött hello instrumentation kulcs, és közli az Application Insights toodisplay legyen az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5390a-128">hello instrumentation key is sent along with every item of telemetry and tells Application Insights toodisplay it in your resource.</span></span>
* <span data-ttu-id="5390a-129">hello HTTP-kérelem összetevő nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="5390a-129">hello HTTP Request component is optional.</span></span> <span data-ttu-id="5390a-130">Automatikusan elküldi a telemetriai kérelem és válasz alkalommal toohello portál.</span><span class="sxs-lookup"><span data-stu-id="5390a-130">It automatically sends telemetry about requests and response times toohello portal.</span></span>
* <span data-ttu-id="5390a-131">Korrelációs események egy hozzáadása toohello HTTP-kérelem összetevő.</span><span class="sxs-lookup"><span data-stu-id="5390a-131">Events correlation is an addition toohello HTTP request component.</span></span> <span data-ttu-id="5390a-132">Hozzárendel egy azonosító tooeach kérelem hello kiszolgáló által fogadott, és felveszi ezt az azonosítót telemetriai adatot tulajdonság tooevery elemként hello tulajdonság "Operation.Id".</span><span class="sxs-lookup"><span data-stu-id="5390a-132">It assigns an identifier tooeach request received by hello server, and adds this identifier as a property tooevery item of telemetry as hello property 'Operation.Id'.</span></span> <span data-ttu-id="5390a-133">Állítsa be a szűrőt minden kérelemhez társított toocorrelate hello telemetriai lehetővé teszi [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="5390a-133">It allows you toocorrelate hello telemetry associated with each request by setting a filter in [diagnostic search](app-insights-diagnostic-search.md).</span></span>

## <a name="4-add-an-http-filter"></a><span data-ttu-id="5390a-134">4. HTTP-szűrő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5390a-134">4. Add an HTTP filter</span></span>
<span data-ttu-id="5390a-135">Keresse meg, és nyissa meg a projekt, és a következő kódrészletét hello webalkalmazás csomópont alatt, ahol az alkalmazás szűrőit egyesítési hello hello web.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="5390a-135">Locate and open hello web.xml file in your project, and merge hello following snippet of code under hello web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="5390a-136">hello szűrő tooget hello legpontosabb eredmények leképezéshez előtt az összes többi szűrőt.</span><span class="sxs-lookup"><span data-stu-id="5390a-136">tooget hello most accurate results, hello filter should be mapped before all other filters.</span></span>

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>
```

## <a name="5-check-firewall-exceptions"></a><span data-ttu-id="5390a-137">5. Ellenőrizze a tűzfal kivételei közé</span><span class="sxs-lookup"><span data-stu-id="5390a-137">5. Check firewall exceptions</span></span>
<span data-ttu-id="5390a-138">Szükség lehet túl[értéke kivételeket toosend kimenő adatok](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="5390a-138">You might need too[set exceptions toosend outgoing data](app-insights-ip-addresses.md).</span></span>

## <a name="6-restart-your-web-app"></a><span data-ttu-id="5390a-139">6. A webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="5390a-139">6. Restart your web app</span></span>
## <a name="7-view-your-telemetry-in-application-insights"></a><span data-ttu-id="5390a-140">7. A telemetria megtekintése az Application Insights szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="5390a-140">7. View your telemetry in Application Insights</span></span>
<span data-ttu-id="5390a-141">Térjen vissza az Application Insights-erőforrás tooyour [Microsoft Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5390a-141">Return tooyour Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="5390a-142">Telemetriai adatainak HTTP-kérelmek hello áttekintése panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5390a-142">Telemetry about HTTP requests appears on hello overview blade.</span></span> <span data-ttu-id="5390a-143">(Ha nincsenek ott, várjon néhány másodpercig, majd kattintson a Frissítés gombra.)</span><span class="sxs-lookup"><span data-stu-id="5390a-143">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![mintaadatok](./media/app-insights-java-live/5-results.png)

<span data-ttu-id="5390a-145">Kattintson a diagram toosee keresztül metrikák részletes.</span><span class="sxs-lookup"><span data-stu-id="5390a-145">Click through any chart toosee more detailed metrics.</span></span> 

![](./media/app-insights-java-live/6-barchart.png)

<span data-ttu-id="5390a-146">És egy kérelem hello tulajdonságainak megtekintésekor láthatja hello telemetriai események például a kérelmek és kivételek társítva.</span><span class="sxs-lookup"><span data-stu-id="5390a-146">And when viewing hello properties of a request, you can see hello telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-live/7-instance.png)

[<span data-ttu-id="5390a-147">További információk a metrikákról.</span><span class="sxs-lookup"><span data-stu-id="5390a-147">Learn more about metrics.</span></span>](app-insights-metrics-explorer.md)

## <a name="next-steps"></a><span data-ttu-id="5390a-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5390a-148">Next steps</span></span>
* <span data-ttu-id="5390a-149">[Adja hozzá a telemetriai adatok tooyour weblapok](app-insights-javascript.md) toomonitor nézetek és a felhasználó metrikák lapon.</span><span class="sxs-lookup"><span data-stu-id="5390a-149">[Add telemetry tooyour web pages](app-insights-javascript.md) toomonitor page views and user metrics.</span></span>
* <span data-ttu-id="5390a-150">[Webalkalmazás-tesztek beállítása](app-insights-monitor-web-app-availability.md) toomake meg arról, hogy az alkalmazás élő és rugalmas marad.</span><span class="sxs-lookup"><span data-stu-id="5390a-150">[Set up web tests](app-insights-monitor-web-app-availability.md) toomake sure your application stays live and responsive.</span></span>
* [<span data-ttu-id="5390a-151">Naplózási nyomkövetés rögzítése</span><span class="sxs-lookup"><span data-stu-id="5390a-151">Capture log traces</span></span>](app-insights-java-trace-logs.md)
* <span data-ttu-id="5390a-152">[Keresést az események és a naplók](app-insights-diagnostic-search.md) toohelp problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="5390a-152">[Search events and logs](app-insights-diagnostic-search.md) toohelp diagnose problems.</span></span>


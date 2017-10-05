---
title: "Az Application Insights Java webes alkalmazásokhoz, amelyek még élő"
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
ms.openlocfilehash: a2731e3e44f8f3d104d8abc7dbe71fe3a4c3a690
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a><span data-ttu-id="919c8-103">Az Application Insights Java webes alkalmazásokhoz, amelyek még élő</span><span class="sxs-lookup"><span data-stu-id="919c8-103">Application Insights for Java web apps that are already live</span></span>


<span data-ttu-id="919c8-104">Ha már a J2EE kiszolgálón futó webalkalmazás, figyelés megkezdése a [Application Insights](app-insights-overview.md) kód módosításokat, vagy a projekt újrafordítása szükségessége nélkül.</span><span class="sxs-lookup"><span data-stu-id="919c8-104">If you have a web application that is already running on your J2EE server, you can start monitoring it with [Application Insights](app-insights-overview.md) without the need to make code changes or recompile your project.</span></span> <span data-ttu-id="919c8-105">Ezzel a kapcsolóval akkor a kiszolgáló, a nem kezelt kivételek és a teljesítményszámlálók elküldött HTTP-kérelmek adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="919c8-105">With this option, you get information about HTTP requests sent to your server, unhandled exceptions, and performance counters.</span></span>

<span data-ttu-id="919c8-106">Ehhez egy [Microsoft Azure](https://azure.com)-előfizetésre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="919c8-106">You'll need a subscription to [Microsoft Azure](https://azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="919c8-107">Az eljárás ezen az oldalon az SDK hozzáadása a webes alkalmazás futásidőben.</span><span class="sxs-lookup"><span data-stu-id="919c8-107">The procedure on this page adds the SDK to your web app at runtime.</span></span> <span data-ttu-id="919c8-108">A futásidejű instrumentation akkor hasznos, ha nem szeretné frissíteni, vagy építse újra a forráskódot.</span><span class="sxs-lookup"><span data-stu-id="919c8-108">This runtime instrumentation is useful if you don't want to update or rebuild your source code.</span></span> <span data-ttu-id="919c8-109">De ha lehetséges, javasoljuk, hogy [adja hozzá az SDK forráskódja](app-insights-java-get-started.md) helyette.</span><span class="sxs-lookup"><span data-stu-id="919c8-109">But if you can, we recommend you [add the SDK to the source code](app-insights-java-get-started.md) instead.</span></span> <span data-ttu-id="919c8-110">Amely több lehetőséget biztosít kódot ír, például felhasználói tevékenységek nyomon követésére.</span><span class="sxs-lookup"><span data-stu-id="919c8-110">That gives you more options such as writing code to track user activity.</span></span>
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a><span data-ttu-id="919c8-111">1. Application Insights-kialakítási kulcs beszerzése</span><span class="sxs-lookup"><span data-stu-id="919c8-111">1. Get an Application Insights instrumentation key</span></span>
1. <span data-ttu-id="919c8-112">Jelentkezzen be a [Microsoft Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="919c8-112">Sign in to the [Microsoft Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="919c8-113">Hozzon létre egy új Application Insights-erőforrást, és állítsa be az alkalmazás típusának Java-webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="919c8-113">Create a new Application Insights resource and set the application type to Java web application.</span></span>
   
    ![Adjon meg egy nevet, válassza ki a Java webalkalmazást, és kattintson a Létrehozás gombra.](./media/app-insights-java-live/02-create.png)

    <span data-ttu-id="919c8-115">Az erőforrás néhány másodpercen belül jön létre.</span><span class="sxs-lookup"><span data-stu-id="919c8-115">The resource is created in a few seconds.</span></span>

4. <span data-ttu-id="919c8-116">Nyissa meg az új erőforrást, és a rendszerállapot-kulcs beszerzése.</span><span class="sxs-lookup"><span data-stu-id="919c8-116">Open the new resource and get its instrumentation key.</span></span> <span data-ttu-id="919c8-117">Ezt a kulcsot nemsokára a kódprojektbe kell illesztenie.</span><span class="sxs-lookup"><span data-stu-id="919c8-117">You'll need to paste this key into your code project shortly.</span></span>
   
    ![Az új erőforrás áttekintésében kattintson a Tulajdonságok gombra, és másolja le a kialakítási kulcsot](./media/app-insights-java-live/03-key.png)

## <a name="2-download-the-sdk"></a><span data-ttu-id="919c8-119">2. Az SDK letöltése</span><span class="sxs-lookup"><span data-stu-id="919c8-119">2. Download the SDK</span></span>
1. <span data-ttu-id="919c8-120">Töltse le a [Javához készült Application Insights SDK-t](https://aka.ms/aijavasdk).</span><span class="sxs-lookup"><span data-stu-id="919c8-120">Download the [Application Insights SDK for Java](https://aka.ms/aijavasdk).</span></span> 
2. <span data-ttu-id="919c8-121">A kiszolgálón bontsa ki az SDK tartalma, amelyből be vannak töltve a projekt bináris fájlokat a könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="919c8-121">On your server, extract the SDK contents to the directory from which your project binaries are loaded.</span></span> <span data-ttu-id="919c8-122">Ha Tomcat használata esetén ez a könyvtár általában kell a`webapps/<your_app_name>/WEB-INF/lib`</span><span class="sxs-lookup"><span data-stu-id="919c8-122">If you’re using Tomcat, this directory would typically be under `webapps/<your_app_name>/WEB-INF/lib`</span></span>

<span data-ttu-id="919c8-123">Vegye figyelembe, hogy meg kell ismételni ezt összes server-példányt, és az egyes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="919c8-123">Note that you need to repeat this on each server instance, and for each app.</span></span>

## <a name="3-add-an-application-insights-xml-file"></a><span data-ttu-id="919c8-124">3. Az Application Insights XML-fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="919c8-124">3. Add an Application Insights xml file</span></span>
<span data-ttu-id="919c8-125">Hozzon létre a mappában, az SDK hozzá ApplicationInsights.xml.</span><span class="sxs-lookup"><span data-stu-id="919c8-125">Create ApplicationInsights.xml in the folder in which you added the SDK.</span></span> <span data-ttu-id="919c8-126">Helyezze be a következő XML.</span><span class="sxs-lookup"><span data-stu-id="919c8-126">Put into it the following XML.</span></span>

<span data-ttu-id="919c8-127">Helyettesítse be az Azure Portalról kapott kialakítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="919c8-127">Substitute the instrumentation key that you got from the Azure portal.</span></span>

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->

      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->

      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* <span data-ttu-id="919c8-128">A kialakítási kulcsot a telemetria minden elemével megkapja, és ez közli az Application Insights eszközzel, hogy megjelenítse azt az erőforrásban.</span><span class="sxs-lookup"><span data-stu-id="919c8-128">The instrumentation key is sent along with every item of telemetry and tells Application Insights to display it in your resource.</span></span>
* <span data-ttu-id="919c8-129">A HTTP-kérelemösszetevő nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="919c8-129">The HTTP Request component is optional.</span></span> <span data-ttu-id="919c8-130">Automatikusan telemetriát küld a kérelmekkel és válaszidőkkel kapcsolatban a portálra.</span><span class="sxs-lookup"><span data-stu-id="919c8-130">It automatically sends telemetry about requests and response times to the portal.</span></span>
* <span data-ttu-id="919c8-131">Az eseménykorreláció a HTTP-kérelemösszetevő további eleme.</span><span class="sxs-lookup"><span data-stu-id="919c8-131">Events correlation is an addition to the HTTP request component.</span></span> <span data-ttu-id="919c8-132">Azonosítót rendel a kiszolgáló által fogadott összes kérelemhez, és az azonosítót „Operation.Id” tulajdonságként hozzáadja a telemetria minden eleméhez.</span><span class="sxs-lookup"><span data-stu-id="919c8-132">It assigns an identifier to each request received by the server, and adds this identifier as a property to every item of telemetry as the property 'Operation.Id'.</span></span> <span data-ttu-id="919c8-133">Ez lehetővé teszi, hogy minden kérelemhez társított úgy, hogy egy szűrőt telemetriai adatok összefüggéseket [diagnosztikai keresési](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="919c8-133">It allows you to correlate the telemetry associated with each request by setting a filter in [diagnostic search](app-insights-diagnostic-search.md).</span></span>

## <a name="4-add-an-http-filter"></a><span data-ttu-id="919c8-134">4. HTTP-szűrő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="919c8-134">4. Add an HTTP filter</span></span>
<span data-ttu-id="919c8-135">Keresse meg és nyissa meg a web.xml fájlt a projektben, és a webalkalmazás csomópont alatt, ahol az alkalmazás szűrők vannak konfigurálva a következő kódrészletét egyesíteni.</span><span class="sxs-lookup"><span data-stu-id="919c8-135">Locate and open the web.xml file in your project, and merge the following snippet of code under the web-app node, where your application filters are configured.</span></span>

<span data-ttu-id="919c8-136">A legpontosabb eredmények érdekében le kell képezni a szűrőt az összes többi szűrő előtt.</span><span class="sxs-lookup"><span data-stu-id="919c8-136">To get the most accurate results, the filter should be mapped before all other filters.</span></span>

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

## <a name="5-check-firewall-exceptions"></a><span data-ttu-id="919c8-137">5. Ellenőrizze a tűzfal kivételei közé</span><span class="sxs-lookup"><span data-stu-id="919c8-137">5. Check firewall exceptions</span></span>
<span data-ttu-id="919c8-138">Szükség lehet [kimenő adatküldés kivételeket](app-insights-ip-addresses.md).</span><span class="sxs-lookup"><span data-stu-id="919c8-138">You might need to [set exceptions to send outgoing data](app-insights-ip-addresses.md).</span></span>

## <a name="6-restart-your-web-app"></a><span data-ttu-id="919c8-139">6. A webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="919c8-139">6. Restart your web app</span></span>
## <a name="7-view-your-telemetry-in-application-insights"></a><span data-ttu-id="919c8-140">7. A telemetria megtekintése az Application Insights szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="919c8-140">7. View your telemetry in Application Insights</span></span>
<span data-ttu-id="919c8-141">Térjen vissza az Application Insights-erőforráshoz a [Microsoft Azure Portalon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="919c8-141">Return to your Application Insights resource in [Microsoft Azure portal](https://portal.azure.com).</span></span>

<span data-ttu-id="919c8-142">Telemetriai adatainak HTTP-kérelmek az Áttekintés panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="919c8-142">Telemetry about HTTP requests appears on the overview blade.</span></span> <span data-ttu-id="919c8-143">(Ha nincsenek ott, várjon néhány másodpercig, majd kattintson a Frissítés gombra.)</span><span class="sxs-lookup"><span data-stu-id="919c8-143">(If it isn't there, wait a few seconds and then click Refresh.)</span></span>

![mintaadatok](./media/app-insights-java-live/5-results.png)

<span data-ttu-id="919c8-145">Részletesebb mérőszámokért kattintson bármelyik diagramra.</span><span class="sxs-lookup"><span data-stu-id="919c8-145">Click through any chart to see more detailed metrics.</span></span> 

![](./media/app-insights-java-live/6-barchart.png)

<span data-ttu-id="919c8-146">És egy kérelem tulajdonságainak megtekintésekor láthatja a telemetriai események például a kérelmek és kivételek társítva.</span><span class="sxs-lookup"><span data-stu-id="919c8-146">And when viewing the properties of a request, you can see the telemetry events associated with it such as requests and exceptions.</span></span>

![](./media/app-insights-java-live/7-instance.png)

[<span data-ttu-id="919c8-147">További információk a metrikákról.</span><span class="sxs-lookup"><span data-stu-id="919c8-147">Learn more about metrics.</span></span>](app-insights-metrics-explorer.md)

## <a name="next-steps"></a><span data-ttu-id="919c8-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="919c8-148">Next steps</span></span>
* <span data-ttu-id="919c8-149">[Telemetriai adatok felvétele a weblapok](app-insights-javascript.md) figyelő Lapmegtekintések és felhasználói metrikákat.</span><span class="sxs-lookup"><span data-stu-id="919c8-149">[Add telemetry to your web pages](app-insights-javascript.md) to monitor page views and user metrics.</span></span>
* <span data-ttu-id="919c8-150">[Webalkalmazás-tesztek beállítása](app-insights-monitor-web-app-availability.md) győződjön meg arról, az alkalmazás marad élő és rugalmas.</span><span class="sxs-lookup"><span data-stu-id="919c8-150">[Set up web tests](app-insights-monitor-web-app-availability.md) to make sure your application stays live and responsive.</span></span>
* [<span data-ttu-id="919c8-151">Naplózási nyomkövetés rögzítése</span><span class="sxs-lookup"><span data-stu-id="919c8-151">Capture log traces</span></span>](app-insights-java-trace-logs.md)
* <span data-ttu-id="919c8-152">[Keresést az események és a naplók](app-insights-diagnostic-search.md) problémák diagnosztizálásához.</span><span class="sxs-lookup"><span data-stu-id="919c8-152">[Search events and logs](app-insights-diagnostic-search.md) to help diagnose problems.</span></span>


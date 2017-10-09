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
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a>Az Application Insights Java webes alkalmazásokhoz, amelyek még élő


Ha már a J2EE kiszolgálón futó webalkalmazás, figyelés megkezdése a [Application Insights](app-insights-overview.md) hello nélkül kell toomake módosítások code vagy fordítsa újra a projektet. Ezzel a lehetőséggel, HTTP-kérelmek küldött tooyour server, a nem kezelt kivételek és a teljesítményszámlálók adatainak beolvasása.

Szüksége lesz egy előfizetés túl[Microsoft Azure](https://azure.com).

> [!NOTE]
> Ezen az oldalon hello eljárás hello SDK tooyour webalkalmazás futásidejű ad hozzá. A futásidejű instrumentation akkor hasznos, ha nem szeretné, hogy tooupdate vagy építse újra a forráskódot. De ha lehetséges, javasoljuk, hogy [hello SDK toohello forráskód hozzáadása](app-insights-java-get-started.md) helyette. Amely esetén több lehetősége is van például kód tootrack felhasználói tevékenység írása.
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Application Insights-kialakítási kulcs beszerzése
1. Jelentkezzen be toohello [Microsoft Azure-portálon](https://portal.azure.com)
2. Hozzon létre egy új Application Insights-erőforrást, és állítsa be a hello alkalmazás típusú tooJava webes alkalmazást.
   
    ![Adjon meg egy nevet, válassza ki a Java webalkalmazást, és kattintson a Létrehozás gombra.](./media/app-insights-java-live/02-create.png)

    hello erőforrás néhány másodpercen belül jön létre.

4. Nyissa meg a hello új erőforrás és a rendszerállapot-kulcs beszerzése. Szüksége lesz toopaste ezt a kulcsot a kód projektben hamarosan.
   
    ![Hello új erőforrás-áttekintés kattintson a tulajdonságok és hello Instrumentation kulcs másolása](./media/app-insights-java-live/03-key.png)

## <a name="2-download-hello-sdk"></a>2. Hello SDK letöltése
1. Töltse le a hello [Javához készült Application Insights SDK](https://aka.ms/aijavasdk). 
2. A kiszolgálón bontsa ki a hello SDK tartalma toohello könyvtárát, amelyből a projekt bináris be van töltve. Ha Tomcat használata esetén ez a könyvtár általában kell a`webapps/<your_app_name>/WEB-INF/lib`

Ne feledje, hogy toorepeat ez összes server-példányt, és az egyes alkalmazásokhoz.

## <a name="3-add-an-application-insights-xml-file"></a>3. Az Application Insights XML-fájl hozzáadása
Hozzon létre ApplicationInsights.xml hello SDK hozzá hello mappában. Kerüljenek, a következő XML hello.

Helyettesítő hello instrumentation kulcs portáltól kapott hello Azure-portálon.

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

* minden telemetriai tétel együtt küldött hello instrumentation kulcs, és közli az Application Insights toodisplay legyen az erőforrás.
* hello HTTP-kérelem összetevő nem kötelező megadni. Automatikusan elküldi a telemetriai kérelem és válasz alkalommal toohello portál.
* Korrelációs események egy hozzáadása toohello HTTP-kérelem összetevő. Hozzárendel egy azonosító tooeach kérelem hello kiszolgáló által fogadott, és felveszi ezt az azonosítót telemetriai adatot tulajdonság tooevery elemként hello tulajdonság "Operation.Id". Állítsa be a szűrőt minden kérelemhez társított toocorrelate hello telemetriai lehetővé teszi [diagnosztikai keresési](app-insights-diagnostic-search.md).

## <a name="4-add-an-http-filter"></a>4. HTTP-szűrő hozzáadása
Keresse meg, és nyissa meg a projekt, és a következő kódrészletét hello webalkalmazás csomópont alatt, ahol az alkalmazás szűrőit egyesítési hello hello web.xml fájlt.

hello szűrő tooget hello legpontosabb eredmények leképezéshez előtt az összes többi szűrőt.

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

## <a name="5-check-firewall-exceptions"></a>5. Ellenőrizze a tűzfal kivételei közé
Szükség lehet túl[értéke kivételeket toosend kimenő adatok](app-insights-ip-addresses.md).

## <a name="6-restart-your-web-app"></a>6. A webalkalmazás újraindítása
## <a name="7-view-your-telemetry-in-application-insights"></a>7. A telemetria megtekintése az Application Insights szolgáltatásban
Térjen vissza az Application Insights-erőforrás tooyour [Microsoft Azure-portálon](https://portal.azure.com).

Telemetriai adatainak HTTP-kérelmek hello áttekintése panelen jelenik meg. (Ha nincsenek ott, várjon néhány másodpercig, majd kattintson a Frissítés gombra.)

![mintaadatok](./media/app-insights-java-live/5-results.png)

Kattintson a diagram toosee keresztül metrikák részletes. 

![](./media/app-insights-java-live/6-barchart.png)

És egy kérelem hello tulajdonságainak megtekintésekor láthatja hello telemetriai események például a kérelmek és kivételek társítva.

![](./media/app-insights-java-live/7-instance.png)

[További információk a metrikákról.](app-insights-metrics-explorer.md)

## <a name="next-steps"></a>Következő lépések
* [Adja hozzá a telemetriai adatok tooyour weblapok](app-insights-javascript.md) toomonitor nézetek és a felhasználó metrikák lapon.
* [Webalkalmazás-tesztek beállítása](app-insights-monitor-web-app-availability.md) toomake meg arról, hogy az alkalmazás élő és rugalmas marad.
* [Naplózási nyomkövetés rögzítése](app-insights-java-trace-logs.md)
* [Keresést az események és a naplók](app-insights-diagnostic-search.md) toohelp problémák diagnosztizálásához.


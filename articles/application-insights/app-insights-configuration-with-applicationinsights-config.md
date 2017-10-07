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
# <a name="configuring-hello-application-insights-sdk-with-applicationinsightsconfig-or-xml"></a>Application Insights SDK hello konfigurálása az ApplicationInsights.config vagy .xml
hello Application Insights .NET SDK NuGet-csomagok számos áll. A [core csomag](http://www.nuget.org/packages/Microsoft.ApplicationInsights) hello Application Insights telemetria küldi hello API biztosít. [További csomagok](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) adja meg a telemetriai adatok *modulok* és *inicializálók* automatikusan nyomon követése a telemetriai adatok az alkalmazás és a környezetben. Hello konfigurációs fájl módosításával engedélyezze vagy tiltsa le a telemetria-modulokat és az inicializálók, és némelyikük paramétereinek megadása.

hello konfigurációs fájl neve `ApplicationInsights.config` vagy `ApplicationInsights.xml`, attól függően, az alkalmazás hello típusú. Az automatikusan bekerül az tooyour projekt mikor meg [hello SDK legtöbb változatának telepítése][start]. Webalkalmazás tooa által is megjelenik [állapotfigyelő az IIS-kiszolgáló][redfield], vagy hello Appplication Insights kiválasztásakor [bővítmény, az Azure webhelyén vagy a virtuális gép](app-insights-azure-web-apps.md).

Nem áll rendelkezésre egy egyenértékű fájl toocontrol hello [SDK-t egy weblap][client].

Ez a dokumentum ismerteti a hello szakaszok látható hello konfigurációs fájlban, hogyan szabályozza azok a hello összetevői hello SDK-t, és melyik NuGet-csomagok betölteni az összetevőket.

## <a name="telemetry-modules-aspnet"></a>Telemetria modulok (ASP.NET)
Minden telemetriai modul egy adott típusú adatokat gyűjt, és hello core API toosend hello adatokat használja. hello modulok különböző NuGet-csomagok, amelyek hello szükséges sorok toohello .config fájl is telepíti.

Van a csomópont minden modul hello konfigurációs fájlban. toodisable egy modul hello csomópont törlése, vagy el megjegyzés.

### <a name="dependency-tracking"></a>A függőségi nyomon követése
[Követés függőségi](app-insights-asp-net-dependencies.md) az alkalmazás lehetővé teszi a toodatabases és külső szolgáltatások adatbázisok hívások vonatkozó telemetriai adatokat gyűjt. tooallow a modul toowork az IIS-kiszolgálót, kell túl[Állapotmonitor telepítése][redfield]. toouse az Azure-webalkalmazásokban vagy a virtuális gépek, [hello Application Insights-bővítmény kiválasztása](app-insights-azure-web-apps.md).

Követés kód hello segítségével a saját függőségi is írhat [TrackDependency API](app-insights-api-custom-events-metrics.md#trackdependency).

* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet-csomagot.

### <a name="performance-collector"></a>Teljesítmény-gyűjtő
[Gyűjti a rendszerteljesítmény-számlálók](app-insights-performance-counters.md) például CPU és memória- és hálózati betöltése az IIS telepítése. Megadhatja, hogy mely számlálók toocollect, beleértve a saját kezűleg beállított teljesítményszámlálók.

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet-csomagot.

### <a name="application-insights-diagnostics-telemetry"></a>Application Insights diagnosztika Telemetria
Hello `DiagnosticsTelemetryModule` az Application Insights instrumentation forráskód hello hibát jelez. Például ha hello kódja nem tudja elérni a teljesítményszámlálókat, vagy ha egy `ITelemetryInitializer` kivételt jelez. Ez a modul követik – nyomkövetési telemetria hello megjelenik [diagnosztikai keresési][diagnostic]. Diagnosztikai adatok toodc.services.vsallin.net küld.

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-csomagot. Ha csak telepíteni ezt a csomagot, hello ApplicationInsights.config fájl nem automatikusan létrejön.

### <a name="developer-mode"></a>Fejlesztői mód
`DeveloperModeWithDebuggerAttachedTelemetryModule`kényszeríti az Application Insights hello `TelemetryChannel` azonnal, toosend adatok több telemetriai tétel egyszerre, ha hibakereső van csatlakoztatva toohello alkalmazás folyamata. Ez csökkenti a hello időn közötti hello néhány percet, ha az alkalmazás nyomon követi a telemetriai adatok, illetve ha hello Application Insights portál megjelenik. A Processzor- és hálózati sávszélesség jelentős terhelést okoz.

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-csomag

### <a name="web-request-tracking"></a>Webes kérelem nyomon követése
Jelentések hello [időt és az eredmény válaszkód](app-insights-asp-net.md) a HTTP-kérések.

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-csomag

### <a name="exception-tracking"></a>Kivétel követése
`ExceptionTrackingTelemetryModule`a webalkalmazás kezeletlen kivételek nyomon követi. Lásd: [hibákat és kivételeket][exceptions].

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet-csomag
* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule`-számok [feladat kivételek észrevétlen](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx).
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule`-a feldolgozói szerepköröket, a központi windows-szolgáltatások és a konzol alkalmazások nem kezelt kivételek nyomon követi.
* [Application Insights Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet-csomagot.

### <a name="eventsource-tracking"></a>EventSource nyomon követése
`EventSourceTelemetryModule`lehetővé teszi a tooconfigure EventSource események toobe tooApplication Insights küldése a nyomkövetési adatokat. Információ az EventSource nyomon követés: [használatával EventSource események](app-insights-asp-net-trace-logs.md#using-eventsource-events).

* `Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule`
* [Microsoft.ApplicationInsights.EventSourceListener](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener) 

### <a name="etw-event-tracking"></a>ETW-események követése
`EtwCollectorTelemetryModule`lehetővé teszi a tooconfigure események az ETW-szolgáltatók toobe tooApplication Insights küldése a nyomkövetési adatokat. Információ az ETW-események nyomon követése: [ETW-esemény használatával](app-insights-asp-net-trace-logs.md#using-etw-events).

* `Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule`
* [Microsoft.ApplicationInsights.EtwCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector) 

### <a name="microsoftapplicationinsights"></a>Microsoft.ApplicationInsights
hello Microsoft.ApplicationInsights csomag biztosít hello [API alapvető](https://msdn.microsoft.com/library/mt420197.aspx) az hello SDK. hello egyéb telemetriai modulok használja, és is [toodefine használni a saját telemetriai](app-insights-api-custom-events-metrics.md).

* Nem találhatók bejegyzések applicationinsights.config.
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet-csomagot. Ha most telepíti a NuGet, nem .config fájl jön létre.

## <a name="telemetry-channel"></a>Telemetria csatorna
hello telemetriai csatorna pufferelés és továbbítása telemetriai toohello Application Insights szolgáltatás kezeli.

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`az alapértelmezett csatornán hello szolgáltatások. Adatok a memóriában puffereli azt.
* `Microsoft.ApplicationInsights.PersistenceChannel`a konzol alkalmazások alternatív van. Bármely unflushed toopersistent adattárolás azt is mentheti, ha az alkalmazás bezárása után, és visszaküldi azt hello alkalmazás indításakor újra.

## <a name="telemetry-initializers-aspnet"></a>Telemetria inicializálók (ASP.NET)
Telemetria inicializálók tulajdonságainak környezetben küldött telemetriai minden elem mellett.

Is [saját inicializálók írási](app-insights-api-filtering-sampling.md#add-properties) tooset környezeti tulajdonságok.

hello szabványos inicializálók be vannak állítva vagy hello Web vagy WindowsServer NuGet-csomagot:

* `AccountIdTelemetryInitializer`hello AccountId tulajdonság beállítása.
* `AuthenticatedUserIdTelemetryInitializer`hello AuthenticatedUserId tulajdonság beállítása beállított hello JavaScript SDK által.
* `AzureRoleEnvironmentTelemetryInitializer`frissítések hello `RoleName` és `RoleInstance` hello tulajdonságainak `Device` hello Azure futtatókörnyezetben kinyert adatokkal az összes telemetriai adatokat a környezetben.
* `BuildInfoConfigComponentVersionTelemetryInitializer`frissítések hello `Version` hello tulajdonságának `Component` hello kinyert hello értékű az összes telemetriai környezetben `BuildInfo.config` MS Build által.
* `ClientIpHeaderTelemetryInitializer`frissítések `Ip` hello tulajdonságának `Location` környezetben az összes telemetriai elem alapján hello `X-Forwarded-For` hello kérelem HTTP-fejléc.
* `DeviceTelemetryInitializer`a következő hello tulajdonságainak frissítések hello `Device` környezetben az összes telemetriai adat.
  * `Type`túl értéke "PC"
  * `Id`be van állítva hello számítógép tartománynevét toohello hello webes alkalmazást futtató.
  * `OemName`hello kinyert toohello érték beállítása `Win32_ComputerSystem.Manufacturer` mezőben a WMI segítségével.
  * `Model`hello kinyert toohello érték beállítása `Win32_ComputerSystem.Model` mezőben a WMI segítségével.
  * `NetworkType`hello kinyert toohello érték beállítása `NetworkInterface`.
  * `Language`hello toohello neve van beállítva `CurrentCulture`.
* `DomainNameRoleInstanceTelemetryInitializer`frissítések hello `RoleInstance` hello tulajdonságának `Device` telemetriai szereplő összes hello tartománynévvel hello hello webes alkalmazást futtató számítógép a környezetben.
* `OperationNameTelemetryInitializer`frissítések hello `Name` hello tulajdonságának `RequestTelemetry` és hello `Name` hello tulajdonságának `Operation` környezetben az összes telemetriai elem alapján hello HTTP-metódus, valamint az ASP.NET MVC-vezérlő és a művelet a meghívott tooprocess hello nevei a kérést.
* `OperationIdTelemetryInitializer`vagy `OperationCorrelationTelemetryInitializer` frissítések hello `Operation.Id` context tulajdonság az összes telemetriai elem nyomon követheti az automatikusan generált hello kérelem kezelése közben `RequestTelemetry.Id`.
* `SessionTelemetryInitializer`frissítések hello `Id` hello tulajdonságának `Session` hello kinyert érték az összes telemetriai környezetben `ai_session` hello által generált cookie-k hello felhasználó böngészőjében futó ApplicationInsights JavaScript instrumentation kódot.
* `SyntheticTelemetryInitializer`vagy `SyntheticUserAgentTelemetryInitializer` frissítések hello `User`, `Session` és `Operation` összes telemetriai elemek környezetek tulajdonságainak nyomon követ, egy kérelem egy szintetikus forrásból kezelésekor, például a rendelkezésre állási tesztelése, vagy végezzen keresést a motor botot. Alapértelmezés szerint [Metrikaböngésző](app-insights-metrics-explorer.md) szintetikus telemetriai adatok nem jelennek meg.

    Hello `<Filters>` azonosító hello kérelmek tulajdonságainak beállítása.
* `UserAgentTelemetryInitializer`frissítések hello `UserAgent` hello tulajdonságának `User` környezetben az összes telemetriai elem alapján hello `User-Agent` hello kérelem HTTP-fejléc.
* `UserTelemetryInitializer`frissítések hello `Id` és `AcquisitionDate` tulajdonságainak `User` hello kinyert értékekkel az összes telemetriai környezetben `ai_user` hello Application Insights JavaScript instrumentation kód hello futó által generált cookie-k felhasználó böngészőjében.
* `WebTestTelemetryInitializer`készletek hello felhasználói azonosítót, a munkamenet-azonosító és a szintetikus tulajdonságait a HTTP-kérelmek származó [rendelkezésreállás figyelésére szolgáló tesztek](app-insights-monitor-web-app-availability.md).
  Hello `<Filters>` azonosító hello kérelmek tulajdonságainak beállítása.

A Service Fabric-beli .NET-alkalmazásokban, megadhat hello `Microsoft.ApplicationInsights.ServiceFabric` NuGet-csomagot. Ez a csomag tartalmaz egy `FabricTelemetryInitializer`, amely a Service Fabric tulajdonságok tootelemetry elemek hozzáadása. További információkért lásd: hello [GitHub-oldalon](https://go.microsoft.com/fwlink/?linkid=848457) a NuGet csomag által hozzáadott hello tulajdonságok.

## <a name="telemetry-processors-aspnet"></a>Telemetria processzor (ASP.NET)
Telemetriai processzorok szűréséhez, és minden telemetriai elem módosítása hello SDK toohello portálról elküldés előtt.

Is [saját telemetriai processzorok írási](app-insights-api-filtering-sampling.md#filtering).

#### <a name="adaptive-sampling-telemetry-processor-from-200-beta3"></a>Adaptív mintavételi telemetriai processzor (a 2.0.0-beta3)
Ez e beállítás alapértelmezés szerint engedélyezve van. Az alkalmazás nagy mennyiségű telemetriai adatokat küld, ha a processzor eltávolítja bizonyos része.

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>

```

hello paraméter biztosít hello target algoritmust hello megpróbál tooachieve. Minden példánya hello SDK függetlenül működik, így ha a kiszolgáló egy fürt több gépek, telemetriai adatok tényleges mennyiségét hello és ennek megfelelően kell-e.

[További információ a mintavételi](app-insights-sampling.md).

#### <a name="fixed-rate-sampling-telemetry-processor-from-200-beta1"></a>Rögzített mintavételi telemetriai processzor (a 2.0.0-beta1)
Szerepel továbbá egy szabványos [telemetriai processzor mintavételi](app-insights-api-filtering-sampling.md) (a 2.0.1):

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     <!-- Set a percentage close too100/N where N is an integer. -->
     <!-- E.g. 50 (=100/2), 33.33 (=100/3), 25 (=100/4), 20, 1 (=100/100), 0.1 (=100/1000) -->
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>

```



## <a name="channel-parameters-java"></a>A csatornaparaméterek (Java)
Ezek a paraméterek arról, hogy a Java SDK hello hogyan kell tárolni és ürítse ki az általa gyűjtött telemetriaadatok hello érintik.

#### <a name="maxtelemetrybuffercapacity"></a>MaxTelemetryBufferCapacity
hello telemetriai elemek száma, amelyek hello SDK memórián belüli tároló tárolhatja. A számnak az elérésekor, hello telemetriai puffer ki van ürítve, – a Ez azt jelenti, hogy hello telemetriai elemek toohello Application Insights server küldi el.

* Minimum: 1
* Maximális: 1000
* Alapértelmezett: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### <a name="flushintervalinseconds"></a>FlushIntervalInSeconds
Megállapítja, milyen gyakran hello hello memórián belüli tároló tárolt adatok legyen kiürített (elküldött tooApplication Insights).

* Minimum: 1
* Maximális: 300
* Alapértelmezett: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### <a name="maxtransmissionstoragecapacityinmb"></a>MaxTransmissionStorageCapacityInMB
Meghatározza, hogy hello maximális mérete (MB), amely számára engedélyezett az állandó tároló toohello hello helyi lemezen. Ez a tároló nem sikerült továbbítani toobe toohello Application Insights végpont tárolásakor telemetriai elemekre szolgál. Hello tárméret teljesülésekor új telemetriai elemek elvesznek.

* Minimum: 1
* Maximum: 100
* Alapértelmezett: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## <a name="instrumentationkey"></a>InstrumentationKey
Ez határozza meg az adatok meg hello Application Insights-erőforrást. Általában létrehozhat egy különálló erőforrás külön kulccsal, minden, az alkalmazások.

Ha tooset hello kulcsot dinamikusan – például ha az alkalmazás toodifferent erőforrásoktól - toosend eredményét hello konfigurációs fájlból hello kulcs nincs megadva, és helyette állítsa a kódban.

tooset hello kulcs TelemetryClient, beleértve a szabványos telemetriai modulok összes példánya esetén TelemetryConfiguration.Active hello kulcs beállítva. Egy inicializálási metódust, például egy ASP.NET-szolgáltatásban Global.aspx.cs osztályból tegye a következőket:

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

Ha csak toosend egy adott események tooa különböző erőforrás megadásához egy adott TelemetryClient hello kulcs adhatja meg:

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...

```

egy új kulcsot, tooget [hozzon létre egy új erőforrást a hello Application Insights portál][new].

## <a name="next-steps"></a>Következő lépések
[További tudnivalók hello API][api].

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[client]: app-insights-javascript.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[netlogs]: app-insights-asp-net-trace-logs.md
[new]: app-insights-create-new-resource.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md

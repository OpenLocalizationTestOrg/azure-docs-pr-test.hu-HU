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
# <a name="explore-net-trace-logs-in-application-insights"></a>.NET nyomkövetési naplók megtekintése az Application Insights felfedezése
NLog, a log4Net, vagy a System.Diagnostics.Trace a diagnosztikai nyomkövetés az ASP.NET-alkalmazás használatakor a naplók küldött túl lehet[Azure Application Insights][start], ahol vizsgálatát és keresése őket. A naplók egyesül hello egyéb telemetriai adatokat, így megállapítható, hogy az alkalmazás érkező hello társított minden egyes felhasználói kérelem karbantartási nyomkövetéseket, és a kivizsgált más események és a kivétel jelentések.

> [!NOTE]
> Hello rögzítési naplómoduljának kell? A 3. fél figyelő szoftverek hasznos adaptert, de ha már nem használt NLog, log4Net, vagy a System.Diagnostics.Trace, fontolja meg a csak hívó [Application Insights TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) közvetlenül.
>
>

## <a name="install-logging-on-your-app"></a>Bejelentkezés az alkalmazás telepítése
A kiválasztott naplózási keretrendszer telepítése a projektben. Ennek eredménye egy app.config vagy a web.config bejegyzése.

System.Diagnostics.Trace használata, ha egy bejegyzés tooweb.config tooadd szüksége:

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
## <a name="configure-application-insights-toocollect-logs"></a>Az Application Insights toocollect naplók konfigurálása
**[Az Application Insights tooyour projekt hozzáadása](app-insights-asp-net.md)**  még ezt nem tette meg, ha. Egy beállítás tooinclude hello naplógyűjtő láthatja.

Vagy **konfigurálja az Application Insights** kattintson a jobb gombbal a projektben a Megoldáskezelőre. A beállításnak a hello túl**gyűjtemény konfigurálása**.

*Nincs Application Insights menü vagy a napló adatgyűjtő lehetőség?* Próbálja [hibaelhárítási](#troubleshooting).

## <a name="manual-installation"></a>Manuális telepítés
Akkor használja ezt a módszert, ha a projekt típusa nem támogatott hello (például a Windows asztali projekt) az Application Insights telepítővel.

1. Ha azt tervezi, toouse log4Net vagy NLog, telepítse a projektet.
2. A Megoldáskezelőben kattintson jobb gombbal a projektre, és válassza a **NuGet-csomagok kezelése**.
3. Az „Application Insights” kifejezés keresése
4. Válassza ki a hello megfelelő csomagot - egyikét:

   * Microsoft.ApplicationInsights.TraceListener (toocapture System.Diagnostics.Trace hívás)
   * Microsoft.ApplicationInsights.EventSourceListener (toocapture EventSource esemény)
   * Microsoft.ApplicationInsights.EtwListener (toocapture ETW-esemény)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

hello NuGet csomag telepíti hello szükséges szerelvényeket, valamint módosítja a web.config vagy az App.config fájlt.

## <a name="insert-diagnostic-log-calls"></a>Helyezze be a diagnosztikai naplófájl hívások
Ha a System.Diagnostics.Trace használatához tipikus hívás lenne:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Ha inkább log4net vagy NLog:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>EventSource eseményeket használ
Konfigurálható [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) események küldött toobe tooApplication Insights, mint a nyomkövetési adatokat. Először telepítse a hello `Microsoft.ApplicationInsights.EventSourceListener` NuGet-csomagot. Szerkessze `TelemetryModules` hello szakasza [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Az egyes források állíthatók be a következő paraméterek hello:
 * `Name`hello EventSource toocollect hello nevét adja meg.
 * `Level`Adja meg a naplózási szint toocollect hello. Egyike lehet `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Nem kötelező) Itt adhatja meg a kulcsszavak kombinációk toouse hello egész értéket.

## <a name="using-diagnosticsource-events"></a>DiagnosticSource eseményeket használ
Konfigurálható [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) események küldött toobe tooApplication Insights, mint a nyomkövetési adatokat. Először telepítse a hello [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet-csomagot. Szerkessze a hello `TelemetryModules` hello szakasza [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

Minden egyes DiagnosticSource keresi tootrace, adjon hozzá egy bejegyzést a hello `Name` attribútum a DiagnosticSource toohello nevének beállítása.

## <a name="using-etw-events"></a>Használja az ETW-események
Konfigurálhatja az ETW-események toobe tooApplication Insights küldése a nyomkövetési adatokat. Először telepítse a hello `Microsoft.ApplicationInsights.EtwCollector` NuGet-csomagot. Szerkessze `TelemetryModules` hello szakasza [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md) fájlt.

> [!NOTE] 
> ETW-események csak be kell, ha hello folyamat üzemeltetési hello SDK alatt fut, amely tagja "Teljesítménynapló felhasználói" vagy a rendszergazdák az identitást.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Az egyes források állíthatók be a következő paraméterek hello:
 * `ProviderName`az ETW-szolgáltató toocollect hello hello név.
 * `ProviderGuid`Adja meg a hello GUID-hello ETW-szolgáltató toocollect, ahelyett, hogy használható `ProviderName`.
 * `Level`naplózási szint toocollect hello beállítása. Egyike lehet `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(Választható) beállítása hello kulcsszó kombinációk toouse egész értéket.

## <a name="using-hello-trace-api-directly"></a>Hello nyomkövetési API közvetlen használatával
Hívása hello Application Insights nyomkövetési API közvetlenül. hello naplózási adapterek Ez az API használnak.

Példa:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

TrackTrace előnye, hogy viszonylag hosszú adatok helyezhetik hello üzenetben. Például sikerült kódolni nincs POST-adatokat.

Emellett egy súlyossági szint tooyour üzenetet is hozzáadhat. És egyéb telemetriai adatok, például értékeket is hozzáadhat tulajdonság használható toohelp szűrőt, vagy keressen a nyomkövetési más-más részhalmazához. Példa:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Ez lehetővé tenné, hogy a [keresési][diagnostic], tooeasily szűrő egy adott adatbázis tooa vonatkozó adott súlyossági szint minden köszönőüzenetei ki.

## <a name="explore-your-logs"></a>A naplók felfedezés
Futtassa az alkalmazást, vagy a hibakeresési módban, vagy telepítheti élő.

Az alkalmazás áttekintése panelen a [hello Application Insights portál][portal], válassza a [keresési][diagnostic].

![Az Application insights részére válassza ki a keresés](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Keresés](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Akkor is, például:

* A naplókivonatokat, vagy az adott tulajdonságokkal rendelkező elemek szűrése
* Egy adott cikk részletesen vizsgálhatja meg.
* Más toohello vonatkozó telemetriai található azonos felhasználói kérelem (Ez azt jelenti, hogy a hello azonos OperationID azonosítójú)
* Ezen a lapon hello konfigurációjának mentése a Kedvencek közé

> [!NOTE]
> **Mintavételi.** Ha az alkalmazás nagy mennyiségű adatot küld, és hello Application Insights SDK for ASP.NET verzió 2.0.0-beta3 vagy újabb használ, hello adaptív mintavételi szolgáltatás működtetésében, valamint csak százalékaként a telemetriai adatok küldése. [További tudnivalók a mintavételezésről.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Következő lépések
[Diagnosztizálja a hibákat és kivételeket az ASP.NET][exceptions]

[További információ a keresési][diagnostic].

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="how-do-i-do-this-for-java"></a>Hogyan ez Java?
Használjon hello [Java napló adapterek](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-hello-project-context-menu"></a>Nincs Application Insights lehetőség hello projekt helyi menüben
* Ellenőrizze, hogy az Application Insights-eszközök telepítve van a fejlesztési számítógépen. A Visual Studio menüjében eszközök bővítmények és frissítések, keresse meg az Application Insights-eszközökkel. Ha nem, akkor hello telepített lapon, hello Online lap megnyitásához, és telepítse.
* Ez a projekt Application Insights-eszközök által nem támogatott típusú lehet. Használjon [manuális telepítés](#manual-installation).

### <a name="no-log-adapter-option-in-hello-configuration-tool"></a>Nincs napló adapter lehetőség hello konfigurálása eszköz
* Először meg kell tooinstall hello naplózási keretrendszert.
* Ha a System.Diagnostics.Trace használata esetén ellenőrizze, hogy [legyen konfigurálva `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Rendelkezik készült Application Insights hello legújabb verzióját? A Visual Studio **eszközök** menüben válasszon **bővítmények és frissítések**, és nyissa meg hello **frissítések** lapon. Fejlesztői elemzőeszközök nincs, kattintson a tooupdate azt.

### <a name="emptykey"></a>Hiba jelenik meg: "Instrumentation kulcs nem lehet üres"
A jelek szerint a naplózás adapter Nuget-csomagot az Application Insights telepítése nélkül hello telepítette.

A Megoldáskezelőben kattintson a jobb gombbal `ApplicationInsights.config` válassza **frissítés az Application Insights**. Kaphat olyan párbeszédpanel, amely hozzáfűzendő toosign a tooAzure, és létrehozza az Application Insights-erőforrást, vagy használja ismét egy meglévőt. Ez segít kell azt.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-hello-other-events"></a>Tudom lásd: a diagnosztikai keresési nyomkövetési adatokat, de nem hello más események
Néha eltarthat egy ideig, míg minden hello események és a kérelmek tooget hello-feldolgozási folyamaton keresztül.

### <a name="limits"></a>Mennyi adatot megmarad?
Számos tényező befolyásolja a hello megőrzött adatok mennyiségét. Lásd: hello [korlátok](app-insights-api-custom-events-metrics.md#limits) hello ügyfél esemény metrikák lap további információk szakasza. 

### <a name="im-not-seeing-some-of-hello-log-entries-that-i-expect"></a>Nem látható az egyes várt hello naplóbejegyzések
Ha az alkalmazás nagy mennyiségű adatot küld, és hello Application Insights SDK for ASP.NET verzió 2.0.0-beta3 vagy újabb használ, hello adaptív mintavételi szolgáltatás működtetésében, valamint csak százalékaként a telemetriai adatok küldése. [További tudnivalók a mintavételezésről.](app-insights-sampling.md)

## <a name="add"></a>Következő lépések
* [Rendelkezésre állási és reakcióidőt tesztek beállítása][availability]
* [Hibaelhárítás][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md

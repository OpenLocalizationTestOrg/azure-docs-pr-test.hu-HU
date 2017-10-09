---
title: "Service Fabric szint Alkalmazásfigyelés aaaAzure |} Microsoft Docs"
description: "Alkalmazással kapcsolatos további szolgáltatás szintű esemény és és naplók toomonitor használt Azure Service Fabric-fürtök diagnosztizálásához."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 4f4da1eaad4b88428eaa3a2100ac25c8a285a727
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Alkalmazás és szolgáltatás szintű esemény és a napló létrehozása

## <a name="instrumenting-hello-code-with-custom-events"></a>Egyéni események rendszerállapot hello kódot

Hello kód tagolása alapja hello legtöbb egyéb elemeinek a szolgáltatások figyelése. Instrumentation hello csak úgy, hogy valami probléma, és rögzített toobe minek toodiagnose tudhatja. Bár technikailag lehetséges tooconnect hibakereső tooa éles szolgáltatásként, nincs gyakori eljárásnak számít. Úgy hogy a WMI-adatok részletes fontos.

Egyes termékek automatikusan állíthatnak be a kódját. Bár ezek a megoldások is működőképesek, kézi instrumentation szinte mindig szükség. A hello végén elegendő információt tooforensically hello alkalmazás hibakeresése kell rendelkeznie. Ez a dokumentum ismerteti a különböző szempontok tooinstrumenting a kódot, és amikor készíthető elő toochoose, egy másikkal.

## <a name="eventsource"></a>Eseményforrás
A Service Fabric megoldást a Visual Studio sablonból létrehozásakor egy **EventSource**-osztályt (**ServiceEventSource** vagy **ActorEventSource**) jön létre . A sablon jön létre, az alkalmazás vagy szolgáltatás események adhat hozzá. Hello **EventSource** neve **kell** egyedinek lennie, és karakterláncból hello alapértelmezett sablon értéket - érdemes nevezhető&lt;megoldás&gt; - &lt; projekt&gt;. Több **EventSource** használó definíciók hello azonos név okok egy problémáját futási időt. Minden egyes meghatározott esemény egyedi azonosítóval kell rendelkeznie. Ha egy azonosító nem egyedi, futásidejű hiba történik. Egyes szervezetek preassign azonosítók tooavoid ütközések külön fejlesztési csapat közötti értékeket a tartományait. További információkért lásd: [előlegbekérő 's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) vagy hello [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Strukturált EventSource eseményeket használ

Hello események hello ebben a szakaszban szereplő példák a megadva egy különleges esetben, például amikor egy szolgáltatástípus regisztrálva van. Amikor üzenetek definiált használati eset, adatok hozzáadható hello hiba hello szövegét és is több egyszerűen keressen, és hello nevek vagy hello értékei alapján szűrő megadott tulajdonságok. Hello instrumentation kimeneti szerkezetének kialakítása teszi, hogy könnyebben tooconsume, de további gondolat igényel és az időt toodefine új esemény az egyes használati eset. Néhány esemény definíciókat meg lehet osztani hello teljes alkalmazás között. Például egy metódus indítási vagy leállítási esemény volna használni az alkalmazáson belül számos szolgáltatásban. Tartományspecifikus szolgáltatás, például egy rendelés rendszer rendelkezhet egy **CreateOrder** esemény, amelynek a saját egyedi esemény. Ezt a módszert előfordulhat, hogy sok események generálásához, és előfordulhat, hogy a azonosítók összehangolását projekt csapatok. 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // hello instance constructor is private tooenforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // hello ServiceTypeRegistered event contains a unique identifier, an event attribute that defined hello event, and hello code implementation of hello event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // hello ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined hello event, and hello code implementation of hello event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a>EventSource általános használatával

Adott események meghatározása nehéz lehet, mert sokan meghatározása néhány események, amelyek általában kimenete egy karakterlánc adataikat paraméterek közös állítja be. Nagy részét strukturált hello aspektus elvész, és nehezebb toosearch és szűrő hello eredmények. A ezt a módszert használja néhány események általában megfelelő toohello naplózási szintek határozzák meg. a következő kódrészletet hello meghatározza, hogy a hibakeresési és a hiba üzenet:

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // hello Instance constructor is private, tooenforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```

A strukturált és általános instrumentation hibrid használatával is is működőképesek. Strukturált instrumentation és metrikákat szolgál. Általános események használható hello részletes naplózás, amely hibaelhárítási mérnökök által felhasznált.

## <a name="aspnet-core-logging"></a>Az ASP.NET Core naplózása

Az fontos toocarefully megtervezése, hogyan fogja eszköz a kód. hello jobb instrumentation terv segítséget nyújt a potenciálisan destabilizing a kódbázis, és ezután kellene tooreinstrument hello kód elkerülése érdekében. tooreduce kockázat, dönthet úgy, mint egy rendszerállapot-tára [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), amely Microsoft ASP.NET Core része. Az ASP.NET Core rendelkezik egy [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) felülete, ugyanakkor minimalizálja a meglévő kódot hello hatással az Ön által választott hello szolgáltató használható. Hello kódot használja a Windows és Linux ASP.NET Core, és a hello teljes .NET-keretrendszer, így a instrumentation kódot szabványosított. Ez további felfedezte alatt:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>A Service Fabric Microsoft.Extensions.Logging használata

1. Hello Microsoft.Extensions.Logging NuGet csomag toohello projekt tooinstrument szeretne hozzáadni. Továbbá adja hozzá a szolgáltató csomagokat (a külső csomag, lásd a következő példa hello). További információkért lásd: [ASP.NET Core-naplózás](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Adja hozzá a **használatával** Microsoft.Extensions.Logging tooyour szolgáltatás fájl direktíva.
3. A szolgáltatás osztályon belül a titkos változó megadása.

  ```csharp
  private ILogger _logger = null;

  ```
4. A szolgáltatás osztály konstruktorában hello adja hozzá ezt a kódot:

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. Indítsa el a kódot a metódusok leírására. Az alábbiakban néhány minták:

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and hello duration of hello request.
  // Later in hello article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a>Más naplózási szolgáltatók használata

Egyes külső szolgáltatók hello előző szakaszban leírt hello a módszert használja, beleértve a [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), és [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Ezen az ASP.NET Core naplózás csatlakoztatható, illetve külön-külön használhatja őket. Serilog egy szolgáltatás, amely a következőképpen színesíti a naplózó küldött üzenetek rendelkezik. Ez a szolgáltatás lehet hasznos toooutput hello szolgáltatás neve, típusa és a partíciónak az adatait. toouse ezt a lehetőséget az ASP.NET alapvető infrastruktúra hello hajtsa végre az alábbi lépéseket:

1. Adja hozzá a hello Serilog, Serilog.Extensions.Logging, és Serilog.Sinks.Observable NuGet-csomagok toohello projekt. Például hello tovább is hozzáadhat Serilog.Sinks.Literate. Jobb megközelítés a cikk későbbi részében látható.
2. A Serilog hozzon létre LoggerConfiguration és hello naplózó-példányt.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Adja hozzá a Serilog.ILogger argumentum toohello szolgáltatás konstruktort, és adja át az újonnan létrehozott naplózó hello.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. Hello szolgáltatás konstruktor, adja hozzá a következő kódra, amely létrehoz hello tulajdonság enrichers a hello hello **ServiceTypeName**, **szolgáltatásnév**, **PartitionId**, és  **InstanceId** hello szolgáltatás tulajdonságait. Bővíti ezenkívül egy tulajdonság enricher toohello ASP.NET Core naplózási gyári, így Microsoft.Extensions.Logging.ILogger használhatja a kódban.

  ```csharp
  public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
      : base(context)
  {
      PropertyEnricher[] properties = new PropertyEnricher[]
      {
          new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
          new PropertyEnricher("ServiceName", context.ServiceName),
          new PropertyEnricher("PartitionId", context.PartitionId),
          new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
      };

      serilog.ForContext(properties);

      _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
  }
  ```

5. Eszköz hello kód hello ugyanaz, mintha az ASP.NET Core nélkül Serilog használta.

  >[!NOTE]
  >Azt javasoljuk, hogy ne használjon hello statikus az előző példa hello Log.Logger. A Service Fabric, rendelkezhet több példánya a hello azonos szolgáltatástípust egyetlen eljáráson belül. Ha statikus Log.Logger hello, a legutolsó író hello hello tulajdonság enrichers a futtatják szoftverpéldányok értékeit megjeleníti. Ez az egyik oka hello _logger változó hello szolgáltatásosztály saját tagváltozó. Emellett meg kell győződnie hello _logger elérhető toocommon kódot, szolgáltatásban felhasználható.

## <a name="choosing-a-logging-provider"></a>Egy naplózási szolgáltató kiválasztása

Ha az alkalmazás nagy teljesítményű, **EventSource** van általában egy jó módszer. **EventSource** *általában* kevesebb erőforrást használ, és jobban naplózás ASP.NET Core, sem egyetlen hello elérhető külső megoldások hajt végre.  Ez számos szolgáltatás, de ha a szolgáltatás-e a teljesítmény-központú, használja a problémát nem **EventSource** megfelelőbb választás lehet. Azonban tooget ezek előnyeit strukturált naplózás **EventSource** egy nagyobb befektetési a mérnöki csapat igényel. Ha lehetséges, hajtsa végre egy gyors prototípus néhány naplózási beállításokat, és válassza a hello egy, az igényeinek leginkább megfelelő.

## <a name="next-steps"></a>Következő lépések

Miután kiválasztotta a naplózás szolgáltató tooinstrument az alkalmazások és szolgáltatások, a naplók és események kell toobe összesítve ezek tooany analysis platform küldése előtt. További információ a [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) és [ÜVEGVATTA](service-fabric-diagnostics-event-aggregation-wad.md) toobetter megismerését hello ajánlott beállítások.

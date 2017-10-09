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
# <a name="application-and-service-level-event-and-log-generation"></a><span data-ttu-id="f1061-103">Alkalmazás és szolgáltatás szintű esemény és a napló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1061-103">Application and service level event and log generation</span></span>

## <a name="instrumenting-hello-code-with-custom-events"></a><span data-ttu-id="f1061-104">Egyéni események rendszerállapot hello kódot</span><span class="sxs-lookup"><span data-stu-id="f1061-104">Instrumenting hello code with custom events</span></span>

<span data-ttu-id="f1061-105">Hello kód tagolása alapja hello legtöbb egyéb elemeinek a szolgáltatások figyelése.</span><span class="sxs-lookup"><span data-stu-id="f1061-105">Instrumenting hello code is hello basis for most other aspects of monitoring your services.</span></span> <span data-ttu-id="f1061-106">Instrumentation hello csak úgy, hogy valami probléma, és rögzített toobe minek toodiagnose tudhatja.</span><span class="sxs-lookup"><span data-stu-id="f1061-106">Instrumentation is hello only way you can know that something is wrong, and toodiagnose what needs toobe fixed.</span></span> <span data-ttu-id="f1061-107">Bár technikailag lehetséges tooconnect hibakereső tooa éles szolgáltatásként, nincs gyakori eljárásnak számít.</span><span class="sxs-lookup"><span data-stu-id="f1061-107">Although technically it's possible tooconnect a debugger tooa production service, it's not a common practice.</span></span> <span data-ttu-id="f1061-108">Úgy hogy a WMI-adatok részletes fontos.</span><span class="sxs-lookup"><span data-stu-id="f1061-108">So, having detailed instrumentation data is important.</span></span>

<span data-ttu-id="f1061-109">Egyes termékek automatikusan állíthatnak be a kódját.</span><span class="sxs-lookup"><span data-stu-id="f1061-109">Some products automatically instrument your code.</span></span> <span data-ttu-id="f1061-110">Bár ezek a megoldások is működőképesek, kézi instrumentation szinte mindig szükség.</span><span class="sxs-lookup"><span data-stu-id="f1061-110">Although these solutions can work well, manual instrumentation is almost always required.</span></span> <span data-ttu-id="f1061-111">A hello végén elegendő információt tooforensically hello alkalmazás hibakeresése kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f1061-111">In hello end, you must have enough information tooforensically debug hello application.</span></span> <span data-ttu-id="f1061-112">Ez a dokumentum ismerteti a különböző szempontok tooinstrumenting a kódot, és amikor készíthető elő toochoose, egy másikkal.</span><span class="sxs-lookup"><span data-stu-id="f1061-112">This document describes different approaches tooinstrumenting your code, and when toochoose one approach over another.</span></span>

## <a name="eventsource"></a><span data-ttu-id="f1061-113">Eseményforrás</span><span class="sxs-lookup"><span data-stu-id="f1061-113">EventSource</span></span>
<span data-ttu-id="f1061-114">A Service Fabric megoldást a Visual Studio sablonból létrehozásakor egy **EventSource**-osztályt (**ServiceEventSource** vagy **ActorEventSource**) jön létre .</span><span class="sxs-lookup"><span data-stu-id="f1061-114">When you create a Service Fabric solution from a template in Visual Studio, an **EventSource**-derived class (**ServiceEventSource** or **ActorEventSource**) is generated.</span></span> <span data-ttu-id="f1061-115">A sablon jön létre, az alkalmazás vagy szolgáltatás események adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="f1061-115">A template is created, in which you can add events for your application or service.</span></span> <span data-ttu-id="f1061-116">Hello **EventSource** neve **kell** egyedinek lennie, és karakterláncból hello alapértelmezett sablon értéket - érdemes nevezhető&lt;megoldás&gt; - &lt; projekt&gt;.</span><span class="sxs-lookup"><span data-stu-id="f1061-116">hello **EventSource** name **must** be unique, and should be renamed from hello default template string MyCompany-&lt;solution&gt;-&lt;project&gt;.</span></span> <span data-ttu-id="f1061-117">Több **EventSource** használó definíciók hello azonos név okok egy problémáját futási időt.</span><span class="sxs-lookup"><span data-stu-id="f1061-117">Having multiple **EventSource** definitions that use hello same name causes an issue at run time.</span></span> <span data-ttu-id="f1061-118">Minden egyes meghatározott esemény egyedi azonosítóval kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f1061-118">Each defined event must have a unique identifier.</span></span> <span data-ttu-id="f1061-119">Ha egy azonosító nem egyedi, futásidejű hiba történik.</span><span class="sxs-lookup"><span data-stu-id="f1061-119">If an identifier is not unique, a runtime failure occurs.</span></span> <span data-ttu-id="f1061-120">Egyes szervezetek preassign azonosítók tooavoid ütközések külön fejlesztési csapat közötti értékeket a tartományait.</span><span class="sxs-lookup"><span data-stu-id="f1061-120">Some organizations preassign ranges of values for identifiers tooavoid conflicts between separate development teams.</span></span> <span data-ttu-id="f1061-121">További információkért lásd: [előlegbekérő 's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) vagy hello [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span><span class="sxs-lookup"><span data-stu-id="f1061-121">For more information, see [Vance's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) or hello [MSDN documentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span></span>

### <a name="using-structured-eventsource-events"></a><span data-ttu-id="f1061-122">Strukturált EventSource eseményeket használ</span><span class="sxs-lookup"><span data-stu-id="f1061-122">Using structured EventSource events</span></span>

<span data-ttu-id="f1061-123">Hello események hello ebben a szakaszban szereplő példák a megadva egy különleges esetben, például amikor egy szolgáltatástípus regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="f1061-123">Each of hello events in hello code examples in this section are defined for a specific case, for example, when a service type is registered.</span></span> <span data-ttu-id="f1061-124">Amikor üzenetek definiált használati eset, adatok hozzáadható hello hiba hello szövegét és is több egyszerűen keressen, és hello nevek vagy hello értékei alapján szűrő megadott tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="f1061-124">When you define messages by use case, data can be packaged with hello text of hello error, and you can more easily search and filter based on hello names or values of hello specified properties.</span></span> <span data-ttu-id="f1061-125">Hello instrumentation kimeneti szerkezetének kialakítása teszi, hogy könnyebben tooconsume, de további gondolat igényel és az időt toodefine új esemény az egyes használati eset.</span><span class="sxs-lookup"><span data-stu-id="f1061-125">Structuring hello instrumentation output makes it easier tooconsume, but requires more thought and time toodefine a new event for each use case.</span></span> <span data-ttu-id="f1061-126">Néhány esemény definíciókat meg lehet osztani hello teljes alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="f1061-126">Some event definitions can be shared across hello entire application.</span></span> <span data-ttu-id="f1061-127">Például egy metódus indítási vagy leállítási esemény volna használni az alkalmazáson belül számos szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f1061-127">For example, a method start or stop event would be reused across many services within an application.</span></span> <span data-ttu-id="f1061-128">Tartományspecifikus szolgáltatás, például egy rendelés rendszer rendelkezhet egy **CreateOrder** esemény, amelynek a saját egyedi esemény.</span><span class="sxs-lookup"><span data-stu-id="f1061-128">A domain-specific service, like an order system, might have a **CreateOrder** event, which has its own unique event.</span></span> <span data-ttu-id="f1061-129">Ezt a módszert előfordulhat, hogy sok események generálásához, és előfordulhat, hogy a azonosítók összehangolását projekt csapatok.</span><span class="sxs-lookup"><span data-stu-id="f1061-129">This approach might generate many events, and potentially require coordination of identifiers across project teams.</span></span> 

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

### <a name="using-eventsource-generically"></a><span data-ttu-id="f1061-130">EventSource általános használatával</span><span class="sxs-lookup"><span data-stu-id="f1061-130">Using EventSource generically</span></span>

<span data-ttu-id="f1061-131">Adott események meghatározása nehéz lehet, mert sokan meghatározása néhány események, amelyek általában kimenete egy karakterlánc adataikat paraméterek közös állítja be.</span><span class="sxs-lookup"><span data-stu-id="f1061-131">Because defining specific events can be difficult, many people define a few events with a common set of parameters that generally output their information as a string.</span></span> <span data-ttu-id="f1061-132">Nagy részét strukturált hello aspektus elvész, és nehezebb toosearch és szűrő hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="f1061-132">Much of hello structured aspect is lost, and it's more difficult toosearch and filter hello results.</span></span> <span data-ttu-id="f1061-133">A ezt a módszert használja néhány események általában megfelelő toohello naplózási szintek határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="f1061-133">In this approach, a few events that usually correspond toohello logging levels are defined.</span></span> <span data-ttu-id="f1061-134">a következő kódrészletet hello meghatározza, hogy a hibakeresési és a hiba üzenet:</span><span class="sxs-lookup"><span data-stu-id="f1061-134">hello following snippet defines a debug and error message:</span></span>

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

<span data-ttu-id="f1061-135">A strukturált és általános instrumentation hibrid használatával is is működőképesek.</span><span class="sxs-lookup"><span data-stu-id="f1061-135">Using a hybrid of structured and generic instrumentation also can work well.</span></span> <span data-ttu-id="f1061-136">Strukturált instrumentation és metrikákat szolgál.</span><span class="sxs-lookup"><span data-stu-id="f1061-136">Structured instrumentation is used for reporting errors and metrics.</span></span> <span data-ttu-id="f1061-137">Általános események használható hello részletes naplózás, amely hibaelhárítási mérnökök által felhasznált.</span><span class="sxs-lookup"><span data-stu-id="f1061-137">Generic events can be used for hello detailed logging that is consumed by engineers for troubleshooting.</span></span>

## <a name="aspnet-core-logging"></a><span data-ttu-id="f1061-138">Az ASP.NET Core naplózása</span><span class="sxs-lookup"><span data-stu-id="f1061-138">ASP.NET Core logging</span></span>

<span data-ttu-id="f1061-139">Az fontos toocarefully megtervezése, hogyan fogja eszköz a kód.</span><span class="sxs-lookup"><span data-stu-id="f1061-139">It's important toocarefully plan how you will instrument your code.</span></span> <span data-ttu-id="f1061-140">hello jobb instrumentation terv segítséget nyújt a potenciálisan destabilizing a kódbázis, és ezután kellene tooreinstrument hello kód elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="f1061-140">hello right instrumentation plan can help you avoid potentially destabilizing your code base, and then needing tooreinstrument hello code.</span></span> <span data-ttu-id="f1061-141">tooreduce kockázat, dönthet úgy, mint egy rendszerállapot-tára [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), amely Microsoft ASP.NET Core része.</span><span class="sxs-lookup"><span data-stu-id="f1061-141">tooreduce risk, you can choose an instrumentation library like [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), which is part of Microsoft ASP.NET Core.</span></span> <span data-ttu-id="f1061-142">Az ASP.NET Core rendelkezik egy [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) felülete, ugyanakkor minimalizálja a meglévő kódot hello hatással az Ön által választott hello szolgáltató használható.</span><span class="sxs-lookup"><span data-stu-id="f1061-142">ASP.NET Core has an [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface that you can use with hello provider of your choice, while minimizing hello effect on existing code.</span></span> <span data-ttu-id="f1061-143">Hello kódot használja a Windows és Linux ASP.NET Core, és a hello teljes .NET-keretrendszer, így a instrumentation kódot szabványosított.</span><span class="sxs-lookup"><span data-stu-id="f1061-143">You can use hello code in ASP.NET Core on Windows and Linux, and in hello full .NET Framework, so your instrumentation code is standardized.</span></span> <span data-ttu-id="f1061-144">Ez további felfedezte alatt:</span><span class="sxs-lookup"><span data-stu-id="f1061-144">This is further explored below:</span></span>

### <a name="using-microsoftextensionslogging-in-service-fabric"></a><span data-ttu-id="f1061-145">A Service Fabric Microsoft.Extensions.Logging használata</span><span class="sxs-lookup"><span data-stu-id="f1061-145">Using Microsoft.Extensions.Logging in Service Fabric</span></span>

1. <span data-ttu-id="f1061-146">Hello Microsoft.Extensions.Logging NuGet csomag toohello projekt tooinstrument szeretne hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="f1061-146">Add hello Microsoft.Extensions.Logging NuGet package toohello project you want tooinstrument.</span></span> <span data-ttu-id="f1061-147">Továbbá adja hozzá a szolgáltató csomagokat (a külső csomag, lásd a következő példa hello).</span><span class="sxs-lookup"><span data-stu-id="f1061-147">Also, add any provider packages (for a third-party package, see hello following example).</span></span> <span data-ttu-id="f1061-148">További információkért lásd: [ASP.NET Core-naplózás](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="f1061-148">For more information, see [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span></span>
2. <span data-ttu-id="f1061-149">Adja hozzá a **használatával** Microsoft.Extensions.Logging tooyour szolgáltatás fájl direktíva.</span><span class="sxs-lookup"><span data-stu-id="f1061-149">Add a **using** directive for Microsoft.Extensions.Logging tooyour service file.</span></span>
3. <span data-ttu-id="f1061-150">A szolgáltatás osztályon belül a titkos változó megadása.</span><span class="sxs-lookup"><span data-stu-id="f1061-150">Define a private variable within your service class.</span></span>

  ```csharp
  private ILogger _logger = null;

  ```
4. <span data-ttu-id="f1061-151">A szolgáltatás osztály konstruktorában hello adja hozzá ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="f1061-151">In hello constructor of your service class, add this code:</span></span>

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. <span data-ttu-id="f1061-152">Indítsa el a kódot a metódusok leírására.</span><span class="sxs-lookup"><span data-stu-id="f1061-152">Start instrumenting your code in your methods.</span></span> <span data-ttu-id="f1061-153">Az alábbiakban néhány minták:</span><span class="sxs-lookup"><span data-stu-id="f1061-153">Here are a few samples:</span></span>

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and hello duration of hello request.
  // Later in hello article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a><span data-ttu-id="f1061-154">Más naplózási szolgáltatók használata</span><span class="sxs-lookup"><span data-stu-id="f1061-154">Using other logging providers</span></span>

<span data-ttu-id="f1061-155">Egyes külső szolgáltatók hello előző szakaszban leírt hello a módszert használja, beleértve a [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), és [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span><span class="sxs-lookup"><span data-stu-id="f1061-155">Some third-party providers use hello approach described in hello preceding section, including [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), and [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span></span> <span data-ttu-id="f1061-156">Ezen az ASP.NET Core naplózás csatlakoztatható, illetve külön-külön használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="f1061-156">You can plug each of these into ASP.NET Core logging, or you can use them separately.</span></span> <span data-ttu-id="f1061-157">Serilog egy szolgáltatás, amely a következőképpen színesíti a naplózó küldött üzenetek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f1061-157">Serilog has a feature that enriches all messages sent from a logger.</span></span> <span data-ttu-id="f1061-158">Ez a szolgáltatás lehet hasznos toooutput hello szolgáltatás neve, típusa és a partíciónak az adatait.</span><span class="sxs-lookup"><span data-stu-id="f1061-158">This feature can be useful toooutput hello service name, type, and partition information.</span></span> <span data-ttu-id="f1061-159">toouse ezt a lehetőséget az ASP.NET alapvető infrastruktúra hello hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f1061-159">toouse this capability in hello ASP.NET Core infrastructure, do these steps:</span></span>

1. <span data-ttu-id="f1061-160">Adja hozzá a hello Serilog, Serilog.Extensions.Logging, és Serilog.Sinks.Observable NuGet-csomagok toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="f1061-160">Add hello Serilog, Serilog.Extensions.Logging, and Serilog.Sinks.Observable NuGet packages toohello project.</span></span> <span data-ttu-id="f1061-161">Például hello tovább is hozzáadhat Serilog.Sinks.Literate.</span><span class="sxs-lookup"><span data-stu-id="f1061-161">For hello next example, also add Serilog.Sinks.Literate.</span></span> <span data-ttu-id="f1061-162">Jobb megközelítés a cikk későbbi részében látható.</span><span class="sxs-lookup"><span data-stu-id="f1061-162">A better approach is shown later in this article.</span></span>
2. <span data-ttu-id="f1061-163">A Serilog hozzon létre LoggerConfiguration és hello naplózó-példányt.</span><span class="sxs-lookup"><span data-stu-id="f1061-163">In Serilog, create a LoggerConfiguration and hello logger instance.</span></span>

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. <span data-ttu-id="f1061-164">Adja hozzá a Serilog.ILogger argumentum toohello szolgáltatás konstruktort, és adja át az újonnan létrehozott naplózó hello.</span><span class="sxs-lookup"><span data-stu-id="f1061-164">Add a Serilog.ILogger argument toohello service constructor, and pass hello newly created logger.</span></span>

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. <span data-ttu-id="f1061-165">Hello szolgáltatás konstruktor, adja hozzá a következő kódra, amely létrehoz hello tulajdonság enrichers a hello hello **ServiceTypeName**, **szolgáltatásnév**, **PartitionId**, és  **InstanceId** hello szolgáltatás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f1061-165">In hello service constructor, add hello following code, which creates hello property enrichers for hello **ServiceTypeName**, **ServiceName**, **PartitionId**, and **InstanceId** properties of hello service.</span></span> <span data-ttu-id="f1061-166">Bővíti ezenkívül egy tulajdonság enricher toohello ASP.NET Core naplózási gyári, így Microsoft.Extensions.Logging.ILogger használhatja a kódban.</span><span class="sxs-lookup"><span data-stu-id="f1061-166">It also adds a property enricher toohello ASP.NET Core logging factory, so you can use Microsoft.Extensions.Logging.ILogger in your code.</span></span>

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

5. <span data-ttu-id="f1061-167">Eszköz hello kód hello ugyanaz, mintha az ASP.NET Core nélkül Serilog használta.</span><span class="sxs-lookup"><span data-stu-id="f1061-167">Instrument hello code hello same as if you were using ASP.NET Core without Serilog.</span></span>

  >[!NOTE]
  ><span data-ttu-id="f1061-168">Azt javasoljuk, hogy ne használjon hello statikus az előző példa hello Log.Logger.</span><span class="sxs-lookup"><span data-stu-id="f1061-168">We recommend that you don't use hello static Log.Logger with hello preceding example.</span></span> <span data-ttu-id="f1061-169">A Service Fabric, rendelkezhet több példánya a hello azonos szolgáltatástípust egyetlen eljáráson belül.</span><span class="sxs-lookup"><span data-stu-id="f1061-169">Service Fabric can host multiple instances of hello same service type within a single process.</span></span> <span data-ttu-id="f1061-170">Ha statikus Log.Logger hello, a legutolsó író hello hello tulajdonság enrichers a futtatják szoftverpéldányok értékeit megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="f1061-170">If you use hello static Log.Logger, hello last writer of hello property enrichers will show values for all instances that are running.</span></span> <span data-ttu-id="f1061-171">Ez az egyik oka hello _logger változó hello szolgáltatásosztály saját tagváltozó.</span><span class="sxs-lookup"><span data-stu-id="f1061-171">This is one reason why hello _logger variable is a private member variable of hello service class.</span></span> <span data-ttu-id="f1061-172">Emellett meg kell győződnie hello _logger elérhető toocommon kódot, szolgáltatásban felhasználható.</span><span class="sxs-lookup"><span data-stu-id="f1061-172">Also, you must make hello _logger available toocommon code, which might be used across services.</span></span>

## <a name="choosing-a-logging-provider"></a><span data-ttu-id="f1061-173">Egy naplózási szolgáltató kiválasztása</span><span class="sxs-lookup"><span data-stu-id="f1061-173">Choosing a logging provider</span></span>

<span data-ttu-id="f1061-174">Ha az alkalmazás nagy teljesítményű, **EventSource** van általában egy jó módszer.</span><span class="sxs-lookup"><span data-stu-id="f1061-174">If your application relies on high performance, **EventSource** is usually a good approach.</span></span> <span data-ttu-id="f1061-175">**EventSource** *általában* kevesebb erőforrást használ, és jobban naplózás ASP.NET Core, sem egyetlen hello elérhető külső megoldások hajt végre.</span><span class="sxs-lookup"><span data-stu-id="f1061-175">**EventSource** *generally* uses fewer resources and performs better than ASP.NET Core logging or any of hello available third-party solutions.</span></span>  <span data-ttu-id="f1061-176">Ez számos szolgáltatás, de ha a szolgáltatás-e a teljesítmény-központú, használja a problémát nem **EventSource** megfelelőbb választás lehet.</span><span class="sxs-lookup"><span data-stu-id="f1061-176">This isn't an issue for many services, but if your service is performance-oriented, using **EventSource** might be a better choice.</span></span> <span data-ttu-id="f1061-177">Azonban tooget ezek előnyeit strukturált naplózás **EventSource** egy nagyobb befektetési a mérnöki csapat igényel.</span><span class="sxs-lookup"><span data-stu-id="f1061-177">However, tooget these benefits of structured logging, **EventSource** requires a larger investment from your engineering team.</span></span> <span data-ttu-id="f1061-178">Ha lehetséges, hajtsa végre egy gyors prototípus néhány naplózási beállításokat, és válassza a hello egy, az igényeinek leginkább megfelelő.</span><span class="sxs-lookup"><span data-stu-id="f1061-178">If possible, do a quick prototype of a few logging options, and then choose hello one that best meets your needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1061-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1061-179">Next steps</span></span>

<span data-ttu-id="f1061-180">Miután kiválasztotta a naplózás szolgáltató tooinstrument az alkalmazások és szolgáltatások, a naplók és események kell toobe összesítve ezek tooany analysis platform küldése előtt.</span><span class="sxs-lookup"><span data-stu-id="f1061-180">Once you have chosen your logging provider tooinstrument your applications and services, your logs and events need toobe aggregated before they can be sent tooany analysis platform.</span></span> <span data-ttu-id="f1061-181">További információ a [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) és [ÜVEGVATTA](service-fabric-diagnostics-event-aggregation-wad.md) toobetter megismerését hello ajánlott beállítások.</span><span class="sxs-lookup"><span data-stu-id="f1061-181">Read about [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) and [WAD](service-fabric-diagnostics-event-aggregation-wad.md) toobetter understand some of hello recommended options.</span></span>

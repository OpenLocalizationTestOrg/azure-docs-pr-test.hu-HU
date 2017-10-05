---
title: "Az Azure Service Fabric alkalmazásszintű figyelése |} Microsoft Docs"
description: "Tudnivalók az alkalmazás és szolgáltatás szintű esemény és figyelése és diagnosztizálása Azure Service Fabric-fürtök használt naplókat."
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
ms.openlocfilehash: 3c472904641108b7383cd0f1416c47460f8de11a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a><span data-ttu-id="80976-103">Alkalmazás és szolgáltatás szintű esemény és a napló létrehozása</span><span class="sxs-lookup"><span data-stu-id="80976-103">Application and service level event and log generation</span></span>

## <a name="instrumenting-the-code-with-custom-events"></a><span data-ttu-id="80976-104">Egyéni események kód tagolása</span><span class="sxs-lookup"><span data-stu-id="80976-104">Instrumenting the code with custom events</span></span>

<span data-ttu-id="80976-105">A kód tagolása alapja legtöbb egyéb elemeinek a szolgáltatások figyelése.</span><span class="sxs-lookup"><span data-stu-id="80976-105">Instrumenting the code is the basis for most other aspects of monitoring your services.</span></span> <span data-ttu-id="80976-106">Instrumentation csak úgy is, hogy valami nem megfelelő, és javításra szorul diagnosztizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="80976-106">Instrumentation is the only way you can know that something is wrong, and to diagnose what needs to be fixed.</span></span> <span data-ttu-id="80976-107">Bár technikailag lehetséges hibakereső kapcsolódni egy éles szolgáltatáshoz, nincs gyakori eljárásnak számít.</span><span class="sxs-lookup"><span data-stu-id="80976-107">Although technically it's possible to connect a debugger to a production service, it's not a common practice.</span></span> <span data-ttu-id="80976-108">Úgy hogy a WMI-adatok részletes fontos.</span><span class="sxs-lookup"><span data-stu-id="80976-108">So, having detailed instrumentation data is important.</span></span>

<span data-ttu-id="80976-109">Egyes termékek automatikusan állíthatnak be a kódját.</span><span class="sxs-lookup"><span data-stu-id="80976-109">Some products automatically instrument your code.</span></span> <span data-ttu-id="80976-110">Bár ezek a megoldások is működőképesek, kézi instrumentation szinte mindig szükség.</span><span class="sxs-lookup"><span data-stu-id="80976-110">Although these solutions can work well, manual instrumentation is almost always required.</span></span> <span data-ttu-id="80976-111">A végén az alkalmazás hibakeresése forensically elegendő információt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="80976-111">In the end, you must have enough information to forensically debug the application.</span></span> <span data-ttu-id="80976-112">Ez a dokumentum ismerteti a kódot, és ha egyik módszer kiválasztásához másikkal tagolása eltérő megközelítést.</span><span class="sxs-lookup"><span data-stu-id="80976-112">This document describes different approaches to instrumenting your code, and when to choose one approach over another.</span></span>

## <a name="eventsource"></a><span data-ttu-id="80976-113">Eseményforrás</span><span class="sxs-lookup"><span data-stu-id="80976-113">EventSource</span></span>
<span data-ttu-id="80976-114">A Service Fabric megoldást a Visual Studio sablonból létrehozásakor egy **EventSource**-osztályt (**ServiceEventSource** vagy **ActorEventSource**) jön létre .</span><span class="sxs-lookup"><span data-stu-id="80976-114">When you create a Service Fabric solution from a template in Visual Studio, an **EventSource**-derived class (**ServiceEventSource** or **ActorEventSource**) is generated.</span></span> <span data-ttu-id="80976-115">A sablon jön létre, az alkalmazás vagy szolgáltatás események adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="80976-115">A template is created, in which you can add events for your application or service.</span></span> <span data-ttu-id="80976-116">A **EventSource** neve **kell** egyedinek lennie, és az alapértelmezett sablon karakterláncból értéket - érdemes nevezhető&lt;megoldás&gt;-&lt;projekt &gt;.</span><span class="sxs-lookup"><span data-stu-id="80976-116">The **EventSource** name **must** be unique, and should be renamed from the default template string MyCompany-&lt;solution&gt;-&lt;project&gt;.</span></span> <span data-ttu-id="80976-117">Több **EventSource** tulajdonságdefiníciót használja ugyanazt a nevet a futási időben problémát okoz.</span><span class="sxs-lookup"><span data-stu-id="80976-117">Having multiple **EventSource** definitions that use the same name causes an issue at run time.</span></span> <span data-ttu-id="80976-118">Minden egyes meghatározott esemény egyedi azonosítóval kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="80976-118">Each defined event must have a unique identifier.</span></span> <span data-ttu-id="80976-119">Ha egy azonosító nem egyedi, futásidejű hiba történik.</span><span class="sxs-lookup"><span data-stu-id="80976-119">If an identifier is not unique, a runtime failure occurs.</span></span> <span data-ttu-id="80976-120">Egyes szervezetek preassign külön fejlesztési csapat közötti ütközések elkerülésével azonosítói értékeit a tartományait.</span><span class="sxs-lookup"><span data-stu-id="80976-120">Some organizations preassign ranges of values for identifiers to avoid conflicts between separate development teams.</span></span> <span data-ttu-id="80976-121">További információkért lásd: [előlegbekérő 's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) vagy a [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span><span class="sxs-lookup"><span data-stu-id="80976-121">For more information, see [Vance's blog](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) or the [MSDN documentation](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).</span></span>

### <a name="using-structured-eventsource-events"></a><span data-ttu-id="80976-122">Strukturált EventSource eseményeket használ</span><span class="sxs-lookup"><span data-stu-id="80976-122">Using structured EventSource events</span></span>

<span data-ttu-id="80976-123">Az eseményeket az ebben a szakaszban szereplő példák mindegyikében megadva egy különleges esetben, például amikor egy szolgáltatástípus regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="80976-123">Each of the events in the code examples in this section are defined for a specific case, for example, when a service type is registered.</span></span> <span data-ttu-id="80976-124">Üzenetek definiált használati eset, adatok hozzáadható a szöveget a hiba, és több is egyszerűen keressen, és a szűrő nevét vagy a megadott tulajdonságok értékei alapján.</span><span class="sxs-lookup"><span data-stu-id="80976-124">When you define messages by use case, data can be packaged with the text of the error, and you can more easily search and filter based on the names or values of the specified properties.</span></span> <span data-ttu-id="80976-125">Szerkezetének kialakítása a instrumentation kimeneti teszi adathalmazokban való felhasználását, de szükséges, további gondolat és időt adja meg egy új eseményt minden használati eset.</span><span class="sxs-lookup"><span data-stu-id="80976-125">Structuring the instrumentation output makes it easier to consume, but requires more thought and time to define a new event for each use case.</span></span> <span data-ttu-id="80976-126">Néhány esemény definíciókat meg lehet osztani a teljes alkalmazás között.</span><span class="sxs-lookup"><span data-stu-id="80976-126">Some event definitions can be shared across the entire application.</span></span> <span data-ttu-id="80976-127">Például egy metódus indítási vagy leállítási esemény volna használni az alkalmazáson belül számos szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="80976-127">For example, a method start or stop event would be reused across many services within an application.</span></span> <span data-ttu-id="80976-128">Tartományspecifikus szolgáltatás, például egy rendelés rendszer rendelkezhet egy **CreateOrder** esemény, amelynek a saját egyedi esemény.</span><span class="sxs-lookup"><span data-stu-id="80976-128">A domain-specific service, like an order system, might have a **CreateOrder** event, which has its own unique event.</span></span> <span data-ttu-id="80976-129">Ezt a módszert előfordulhat, hogy sok események generálásához, és előfordulhat, hogy a azonosítók összehangolását projekt csapatok.</span><span class="sxs-lookup"><span data-stu-id="80976-129">This approach might generate many events, and potentially require coordination of identifiers across project teams.</span></span> 

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The instance constructor is private to enforce singleton semantics.
        private ServiceEventSource() : base() { }

        ...

        // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```

### <a name="using-eventsource-generically"></a><span data-ttu-id="80976-130">EventSource általános használatával</span><span class="sxs-lookup"><span data-stu-id="80976-130">Using EventSource generically</span></span>

<span data-ttu-id="80976-131">Adott események meghatározása nehéz lehet, mert sokan meghatározása néhány események, amelyek általában kimenete egy karakterlánc adataikat paraméterek közös állítja be.</span><span class="sxs-lookup"><span data-stu-id="80976-131">Because defining specific events can be difficult, many people define a few events with a common set of parameters that generally output their information as a string.</span></span> <span data-ttu-id="80976-132">Nagy részét a strukturált aspektus elvész, és nehezebb keresheti ki és az eredmények szűréséhez.</span><span class="sxs-lookup"><span data-stu-id="80976-132">Much of the structured aspect is lost, and it's more difficult to search and filter the results.</span></span> <span data-ttu-id="80976-133">A ezt a módszert használja néhány olyan események, amelyek általában felelnek meg a naplózási szintek határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="80976-133">In this approach, a few events that usually correspond to the logging levels are defined.</span></span> <span data-ttu-id="80976-134">A következő kódrészletet a hibakeresési és a hiba üzenet határozza meg:</span><span class="sxs-lookup"><span data-stu-id="80976-134">The following snippet defines a debug and error message:</span></span>

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // The Instance constructor is private, to enforce singleton semantics.
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

<span data-ttu-id="80976-135">A strukturált és általános instrumentation hibrid használatával is is működőképesek.</span><span class="sxs-lookup"><span data-stu-id="80976-135">Using a hybrid of structured and generic instrumentation also can work well.</span></span> <span data-ttu-id="80976-136">Strukturált instrumentation és metrikákat szolgál.</span><span class="sxs-lookup"><span data-stu-id="80976-136">Structured instrumentation is used for reporting errors and metrics.</span></span> <span data-ttu-id="80976-137">Általános események a részletes naplózást, a hibaelhárítási mérnökök által felhasznált használható.</span><span class="sxs-lookup"><span data-stu-id="80976-137">Generic events can be used for the detailed logging that is consumed by engineers for troubleshooting.</span></span>

## <a name="aspnet-core-logging"></a><span data-ttu-id="80976-138">Az ASP.NET Core naplózása</span><span class="sxs-lookup"><span data-stu-id="80976-138">ASP.NET Core logging</span></span>

<span data-ttu-id="80976-139">Fontos alapos megtervezéséről, hogyan fogja beállíthatják a kódot.</span><span class="sxs-lookup"><span data-stu-id="80976-139">It's important to carefully plan how you will instrument your code.</span></span> <span data-ttu-id="80976-140">A jobb oldali instrumentation terv segítséget nyújt a potenciálisan destabilizing kódbázis, és hogy a kód reinstrument majd kellene elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="80976-140">The right instrumentation plan can help you avoid potentially destabilizing your code base, and then needing to reinstrument the code.</span></span> <span data-ttu-id="80976-141">Kockázatok csökkentése érdekében dönthet úgy, mint egy rendszerállapot-tára [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), amely Microsoft ASP.NET Core része.</span><span class="sxs-lookup"><span data-stu-id="80976-141">To reduce risk, you can choose an instrumentation library like [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), which is part of Microsoft ASP.NET Core.</span></span> <span data-ttu-id="80976-142">Az ASP.NET Core rendelkezik egy [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) felület, amely csak akkor használhatja a szolgáltató az Ön által választott, ugyanakkor minimalizálja a meglévő kódot hatással.</span><span class="sxs-lookup"><span data-stu-id="80976-142">ASP.NET Core has an [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) interface that you can use with the provider of your choice, while minimizing the effect on existing code.</span></span> <span data-ttu-id="80976-143">Az ASP.NET Core Windows és Linux kódot is használhatja, és a teljes .NET-keretrendszer, ezért a rendszerállapot-kód szabványosított.</span><span class="sxs-lookup"><span data-stu-id="80976-143">You can use the code in ASP.NET Core on Windows and Linux, and in the full .NET Framework, so your instrumentation code is standardized.</span></span> <span data-ttu-id="80976-144">Ez további felfedezte alatt:</span><span class="sxs-lookup"><span data-stu-id="80976-144">This is further explored below:</span></span>

### <a name="using-microsoftextensionslogging-in-service-fabric"></a><span data-ttu-id="80976-145">A Service Fabric Microsoft.Extensions.Logging használata</span><span class="sxs-lookup"><span data-stu-id="80976-145">Using Microsoft.Extensions.Logging in Service Fabric</span></span>

1. <span data-ttu-id="80976-146">A Microsoft.Extensions.Logging NuGet-csomag hozzáadása a projekthez kívánt eszközt.</span><span class="sxs-lookup"><span data-stu-id="80976-146">Add the Microsoft.Extensions.Logging NuGet package to the project you want to instrument.</span></span> <span data-ttu-id="80976-147">Továbbá adja hozzá a szolgáltató csomagokat (a külső csomag, lásd az alábbi példában).</span><span class="sxs-lookup"><span data-stu-id="80976-147">Also, add any provider packages (for a third-party package, see the following example).</span></span> <span data-ttu-id="80976-148">További információkért lásd: [ASP.NET Core-naplózás](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span><span class="sxs-lookup"><span data-stu-id="80976-148">For more information, see [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).</span></span>
2. <span data-ttu-id="80976-149">Adja hozzá a **használatával** irányelv Microsoft.Extensions.Logging számára a szolgáltatás fájlba.</span><span class="sxs-lookup"><span data-stu-id="80976-149">Add a **using** directive for Microsoft.Extensions.Logging to your service file.</span></span>
3. <span data-ttu-id="80976-150">A szolgáltatás osztályon belül a titkos változó megadása.</span><span class="sxs-lookup"><span data-stu-id="80976-150">Define a private variable within your service class.</span></span>

  ```csharp
  private ILogger _logger = null;

  ```
4. <span data-ttu-id="80976-151">Az osztály konstruktorában adja hozzá ezt a kódot:</span><span class="sxs-lookup"><span data-stu-id="80976-151">In the constructor of your service class, add this code:</span></span>

  ```csharp
  _logger = new LoggerFactory().CreateLogger<Stateless>();

  ```
5. <span data-ttu-id="80976-152">Indítsa el a kódot a metódusok leírására.</span><span class="sxs-lookup"><span data-stu-id="80976-152">Start instrumenting your code in your methods.</span></span> <span data-ttu-id="80976-153">Az alábbiakban néhány minták:</span><span class="sxs-lookup"><span data-stu-id="80976-153">Here are a few samples:</span></span>

  ```csharp
  _logger.LogDebug("Debug-level event from Microsoft.Logging");
  _logger.LogInformation("Informational-level event from Microsoft.Logging");

  // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
  // Later in the article, we discuss why this step is useful.
  _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);

  ```

## <a name="using-other-logging-providers"></a><span data-ttu-id="80976-154">Más naplózási szolgáltatók használata</span><span class="sxs-lookup"><span data-stu-id="80976-154">Using other logging providers</span></span>

<span data-ttu-id="80976-155">Egyes harmadik fél szolgáltató használja a megközelítést az előző szakaszban leírt beleértve [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), és [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span><span class="sxs-lookup"><span data-stu-id="80976-155">Some third-party providers use the approach described in the preceding section, including [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), and [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging).</span></span> <span data-ttu-id="80976-156">Ezen az ASP.NET Core naplózás csatlakoztatható, illetve külön-külön használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="80976-156">You can plug each of these into ASP.NET Core logging, or you can use them separately.</span></span> <span data-ttu-id="80976-157">Serilog egy szolgáltatás, amely a következőképpen színesíti a naplózó küldött üzenetek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="80976-157">Serilog has a feature that enriches all messages sent from a logger.</span></span> <span data-ttu-id="80976-158">Ez a szolgáltatás a szolgáltatás neve, típusa és a partíciónak az adatait a mentendő hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="80976-158">This feature can be useful to output the service name, type, and partition information.</span></span> <span data-ttu-id="80976-159">Az ASP.NET alapvető infrastruktúra Ez a funkció használatához tegye ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80976-159">To use this capability in the ASP.NET Core infrastructure, do these steps:</span></span>

1. <span data-ttu-id="80976-160">A Serilog Serilog.Extensions.Logging és Serilog.Sinks.Observable NuGet-csomagok hozzáadása a projekthez.</span><span class="sxs-lookup"><span data-stu-id="80976-160">Add the Serilog, Serilog.Extensions.Logging, and Serilog.Sinks.Observable NuGet packages to the project.</span></span> <span data-ttu-id="80976-161">A következő példában is hozzáadhat Serilog.Sinks.Literate.</span><span class="sxs-lookup"><span data-stu-id="80976-161">For the next example, also add Serilog.Sinks.Literate.</span></span> <span data-ttu-id="80976-162">Jobb megközelítés a cikk későbbi részében látható.</span><span class="sxs-lookup"><span data-stu-id="80976-162">A better approach is shown later in this article.</span></span>
2. <span data-ttu-id="80976-163">Serilog hozzon létre egy LoggerConfiguration és a tranzakciónaplókat tartalmazó példány.</span><span class="sxs-lookup"><span data-stu-id="80976-163">In Serilog, create a LoggerConfiguration and the logger instance.</span></span>

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. <span data-ttu-id="80976-164">Adja hozzá a Serilog.ILogger argumentum a szolgáltatás konstruktor, és adja át az újonnan létrehozott naplózó.</span><span class="sxs-lookup"><span data-stu-id="80976-164">Add a Serilog.ILogger argument to the service constructor, and pass the newly created logger.</span></span>

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. <span data-ttu-id="80976-165">A szolgáltatás konstruktort, adja hozzá az alábbi kód, amely létrehozza a tulajdonság enrichers a a **ServiceTypeName**, **szolgáltatásnév**, **PartitionId**, és **InstanceId** a szolgáltatás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="80976-165">In the service constructor, add the following code, which creates the property enrichers for the **ServiceTypeName**, **ServiceName**, **PartitionId**, and **InstanceId** properties of the service.</span></span> <span data-ttu-id="80976-166">Hozzáadja a is egy tulajdonság enricher az ASP.NET Core naplózási gyári, hogy használhassa Microsoft.Extensions.Logging.ILogger a kódban.</span><span class="sxs-lookup"><span data-stu-id="80976-166">It also adds a property enricher to the ASP.NET Core logging factory, so you can use Microsoft.Extensions.Logging.ILogger in your code.</span></span>

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

5. <span data-ttu-id="80976-167">Állíthatnak be a kódját azonos, ha az ASP.NET Core Serilog nélkül.</span><span class="sxs-lookup"><span data-stu-id="80976-167">Instrument the code the same as if you were using ASP.NET Core without Serilog.</span></span>

  >[!NOTE]
  ><span data-ttu-id="80976-168">Azt javasoljuk, hogy nem adja meg a statikus Log.Logger az előző példában a.</span><span class="sxs-lookup"><span data-stu-id="80976-168">We recommend that you don't use the static Log.Logger with the preceding example.</span></span> <span data-ttu-id="80976-169">A Service Fabric tárolhatja, a szolgáltatás ugyanolyan egyetlen eljáráson belül több példányát.</span><span class="sxs-lookup"><span data-stu-id="80976-169">Service Fabric can host multiple instances of the same service type within a single process.</span></span> <span data-ttu-id="80976-170">A statikus Log.Logger használja, ha a tulajdonság enrichers utolsó írója futtatják szoftverpéldányok értékeinek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="80976-170">If you use the static Log.Logger, the last writer of the property enrichers will show values for all instances that are running.</span></span> <span data-ttu-id="80976-171">Ez az egyik oka a _logger változó szolgáltatásosztály saját tagként változó.</span><span class="sxs-lookup"><span data-stu-id="80976-171">This is one reason why the _logger variable is a private member variable of the service class.</span></span> <span data-ttu-id="80976-172">Is meg kell adni a _logger közös kód, amely felhasználható szolgáltatásban elérhető.</span><span class="sxs-lookup"><span data-stu-id="80976-172">Also, you must make the _logger available to common code, which might be used across services.</span></span>

## <a name="choosing-a-logging-provider"></a><span data-ttu-id="80976-173">Egy naplózási szolgáltató kiválasztása</span><span class="sxs-lookup"><span data-stu-id="80976-173">Choosing a logging provider</span></span>

<span data-ttu-id="80976-174">Ha az alkalmazás nagy teljesítményű, **EventSource** van általában egy jó módszer.</span><span class="sxs-lookup"><span data-stu-id="80976-174">If your application relies on high performance, **EventSource** is usually a good approach.</span></span> <span data-ttu-id="80976-175">**EventSource** *általában* kevesebb erőforrást használ, és az ASP.NET Core naplózás, illetve a rendelkezésre álló külső megoldások jobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="80976-175">**EventSource** *generally* uses fewer resources and performs better than ASP.NET Core logging or any of the available third-party solutions.</span></span>  <span data-ttu-id="80976-176">Ez számos szolgáltatás, de ha a szolgáltatás-e a teljesítmény-központú, használja a problémát nem **EventSource** megfelelőbb választás lehet.</span><span class="sxs-lookup"><span data-stu-id="80976-176">This isn't an issue for many services, but if your service is performance-oriented, using **EventSource** might be a better choice.</span></span> <span data-ttu-id="80976-177">Azonban ezek előnyök a naplózás, strukturált **EventSource** egy nagyobb befektetési a mérnöki csapat igényel.</span><span class="sxs-lookup"><span data-stu-id="80976-177">However, to get these benefits of structured logging, **EventSource** requires a larger investment from your engineering team.</span></span> <span data-ttu-id="80976-178">Ha lehetséges néhány naplózási beállítások gyors prototípus tegye, és válassza ki azt, amelyik az igényeinek leginkább megfelelő.</span><span class="sxs-lookup"><span data-stu-id="80976-178">If possible, do a quick prototype of a few logging options, and then choose the one that best meets your needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="80976-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80976-179">Next steps</span></span>

<span data-ttu-id="80976-180">Miután kiválasztotta a naplózás szolgáltató is beállíthatják az alkalmazások és szolgáltatások, a naplók és események összesíthető ahhoz, azok bármely analysis platform lehet küldeni kell.</span><span class="sxs-lookup"><span data-stu-id="80976-180">Once you have chosen your logging provider to instrument your applications and services, your logs and events need to be aggregated before they can be sent to any analysis platform.</span></span> <span data-ttu-id="80976-181">További információ a [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) és [ÜVEGVATTA](service-fabric-diagnostics-event-aggregation-wad.md) jobb megértése érdekében ajánlott beállítások egy része.</span><span class="sxs-lookup"><span data-stu-id="80976-181">Read about [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) and [WAD](service-fabric-diagnostics-event-aggregation-wad.md) to better understand some of the recommended options.</span></span>

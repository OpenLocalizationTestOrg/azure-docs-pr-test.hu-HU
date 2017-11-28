---
title: "Nyomon követheti a Azure Application Insights .NET SDK-val egyéni műveleteket |} Microsoft Docs"
description: "Azure Application Insights .NET SDK-val egyéni műveletek nyomon követése"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/31/2017
ms.author: sergkanz
ms.openlocfilehash: b31d38fe2f7060597956a1ee9c66f43ce39d7240
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a><span data-ttu-id="216c8-103">Application Insights .NET SDK-val egyéni műveletek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="216c8-103">Track custom operations with Application Insights .NET SDK</span></span>

<span data-ttu-id="216c8-104">Az Azure Application Insights SDK-k automatikusan követi nyomon a bejövő HTTP-kérelmek és a függő szolgáltatások, például HTTP-kérelmek és az SQL-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="216c8-104">Azure Application Insights SDKs automatically track incoming HTTP requests and calls to dependent services, such as HTTP requests and SQL queries.</span></span> <span data-ttu-id="216c8-105">Nyomon követését és a kérelmek és a függőségek korrelációs biztosítanak a teljes alkalmazáshoz válaszidejét és megbízhatóság láthatósága összes mikroszolgáltatások létrehozására, az alkalmazás felhasználó között.</span><span class="sxs-lookup"><span data-stu-id="216c8-105">Tracking and correlation of requests and dependencies give you visibility into the whole application's responsiveness and reliability across all microservices that combine this application.</span></span> 

<span data-ttu-id="216c8-106">Nincs alkalmazás mintáról olvashat, amelyek nem tudják támogatni az általános osztály.</span><span class="sxs-lookup"><span data-stu-id="216c8-106">There is a class of application patterns that can't be supported generically.</span></span> <span data-ttu-id="216c8-107">Az ilyen minták megfelelő figyeléshez szükséges manuális kód instrumentation.</span><span class="sxs-lookup"><span data-stu-id="216c8-107">Proper monitoring of such patterns requires manual code instrumentation.</span></span> <span data-ttu-id="216c8-108">Ez a cikk manuális instrumentation egyéni várólista feldolgozása és a hosszú futású háttérfeladatok futtatása például szükség lehet néhány mintázatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="216c8-108">This article covers a few patterns that might require manual instrumentation, such as custom queue processing and running long-running background tasks.</span></span>

<span data-ttu-id="216c8-109">Ez a dokumentum útmutatást nyújt a történő nyomon követheti az Application Insights SDK-val egyéni műveleteket.</span><span class="sxs-lookup"><span data-stu-id="216c8-109">This document provides guidance on how to track custom operations with the Application Insights SDK.</span></span> <span data-ttu-id="216c8-110">Ebben a dokumentációban fontos:</span><span class="sxs-lookup"><span data-stu-id="216c8-110">This documentation is relevant for:</span></span>

- <span data-ttu-id="216c8-111">Az Application Insights .NET (más néven alapvető SDK) verziójának 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="216c8-111">Application Insights for .NET (also known as Base SDK) version 2.4+.</span></span>
- <span data-ttu-id="216c8-112">Az Application Insights webes (ASP.NET futó) alkalmazások verziójához 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="216c8-112">Application Insights for web applications (running ASP.NET) version 2.4+.</span></span>
- <span data-ttu-id="216c8-113">Az Application Insights az ASP.NET Core 2.1-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="216c8-113">Application Insights for ASP.NET Core version 2.1+.</span></span>

## <a name="overview"></a><span data-ttu-id="216c8-114">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="216c8-114">Overview</span></span>
<span data-ttu-id="216c8-115">Egy művelet egy logikai munkákat az alkalmazások futtatása.</span><span class="sxs-lookup"><span data-stu-id="216c8-115">An operation is a logical piece of work run by an application.</span></span> <span data-ttu-id="216c8-116">Az idő, időtartam, eredmény és olyan környezetben, például a felhasználónevet, a tulajdonságok és az eredmény végrehajtás start neve van.</span><span class="sxs-lookup"><span data-stu-id="216c8-116">It has a name, start time, duration, result, and a context of execution like user name, properties, and result.</span></span> <span data-ttu-id="216c8-117">Ha A műveletet kezdeményezett művelet B, majd művelet B be van állítva az A. szülője Egy művelet csak egy szülőhöz lehet, de sok gyermek művelet veheti fel.</span><span class="sxs-lookup"><span data-stu-id="216c8-117">If operation A was initiated by operation B, then operation B is set as a parent for A. An operation can have only one parent, but it can have many child operations.</span></span> <span data-ttu-id="216c8-118">Műveletek és telemetriai korrelációs további információkért lásd: [Azure Application Insights telemetria korrelációs](application-insights-correlation.md).</span><span class="sxs-lookup"><span data-stu-id="216c8-118">For more information on operations and telemetry correlation, see [Azure Application Insights telemetry correlation](application-insights-correlation.md).</span></span>

<span data-ttu-id="216c8-119">Az Application Insights .NET SDK a művelet leírását a absztrakt osztály [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) és a leszármazottai [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) és [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span><span class="sxs-lookup"><span data-stu-id="216c8-119">In the Application Insights .NET SDK, the operation is described by the abstract class [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) and its descendants [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) and [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span></span>

## <a name="incoming-operations-tracking"></a><span data-ttu-id="216c8-120">Bejövő műveletek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="216c8-120">Incoming operations tracking</span></span> 
<span data-ttu-id="216c8-121">Az Application Insights webes SDK-t automatikusan gyűjti az ASP.NET futtatni egy IIS-feldolgozási folyamat összes ASP.NET Core alkalmazásokat és a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="216c8-121">The Application Insights web SDK automatically collects HTTP requests for ASP.NET applications that run in an IIS pipeline and all ASP.NET Core applications.</span></span> <span data-ttu-id="216c8-122">Nincsenek megoldások Közösség által támogatott más platformok és -keretrendszerek számára.</span><span class="sxs-lookup"><span data-stu-id="216c8-122">There are community-supported solutions for other platforms and frameworks.</span></span> <span data-ttu-id="216c8-123">Azonban ha az alkalmazás nem támogatja a standard vagy a Közösség által támogatott megoldások bármelyikét, hogy állíthatnak be azt manuálisan.</span><span class="sxs-lookup"><span data-stu-id="216c8-123">However, if the application isn't supported by any of the standard or community-supported solutions, you can instrument it manually.</span></span>

<span data-ttu-id="216c8-124">Egy másik, amelyhez az egyéni nyomkövetési: a worker, amely megkapja a cikkek az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="216c8-124">Another example that requires custom tracking is the worker that receives items from the queue.</span></span> <span data-ttu-id="216c8-125">Egyes várólisták üzenet hozzáadása a várólista-hívás a függőség beállításához nyomon követni.</span><span class="sxs-lookup"><span data-stu-id="216c8-125">For some queues, the call to add a message to this queue is tracked as a dependency.</span></span> <span data-ttu-id="216c8-126">Azonban a magas szintű műveletet leíró üzenet feldolgozása nem automatikusan összegyűjtött.</span><span class="sxs-lookup"><span data-stu-id="216c8-126">However, the high-level operation that describes message processing is not automatically collected.</span></span>

<span data-ttu-id="216c8-127">Nézzük meg, hogyan azt nyomon az ilyen műveleteket.</span><span class="sxs-lookup"><span data-stu-id="216c8-127">Let's see how we can track such operations.</span></span>

<span data-ttu-id="216c8-128">Magas szinten, a feladatütemezés, ha a `RequestTelemetry` és ismert tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="216c8-128">On a high level, the task is to create `RequestTelemetry` and set known properties.</span></span> <span data-ttu-id="216c8-129">A művelet befejezése után a telemetriai adatok nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="216c8-129">After the operation is finished, you track the telemetry.</span></span> <span data-ttu-id="216c8-130">A következő példa bemutatja, ezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="216c8-130">The following example demonstrates this task.</span></span>

### <a name="http-request-in-owin-self-hosted-app"></a><span data-ttu-id="216c8-131">Önálló üzemeltetett alkalmazás Owin HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="216c8-131">HTTP request in Owin self-hosted app</span></span>
<span data-ttu-id="216c8-132">Ebben a példában azt kövesse a [HTTP protokoll a korrelációs](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="216c8-132">In this example, we follow the [HTTP Protocol for Correlation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span></span> <span data-ttu-id="216c8-133">Fogadási hiba ismertetett fejlécek kell látnia.</span><span class="sxs-lookup"><span data-stu-id="216c8-133">You should expect to receive headers that are described there.</span></span>

``` C#
public class ApplicationInsightsMiddleware : OwinMiddleware
{
    private readonly TelemetryClient telemetryClient = new TelemetryClient(TelemetryConfiguration.Active);
    
    public ApplicationInsightsMiddleware(OwinMiddleware next) : base(next) {}

    public override async Task Invoke(IOwinContext context)
    {
        // Let's create and start RequestTelemetry.
        var requestTelemetry = new RequestTelemetry
        {
            Name = $"{context.Request.Method} {context.Request.Uri.GetLeftPart(UriPartial.Path)}"
        };

        // If there is a Request-Id received from the upstream service, set the telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process the request.
        try
        {
            await Next.Invoke(context);
        }
        catch (Exception e)
        {
            requestTelemetry.Success = false;
            telemetryClient.TrackException(e);
            throw;
        }
        finally
        {
            // Update status code and success as appropriate.
            if (context.Response != null)
            {
                requestTelemetry.ResponseCode = context.Response.StatusCode.ToString();
                requestTelemetry.Success = context.Response.StatusCode >= 200 && context.Response.StatusCode <= 299;
            }
            else
            {
                requestTelemetry.Success = false;
            }

            // Now it's time to stop the operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns the root ID from the '|' to the first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

<span data-ttu-id="216c8-134">A HTTP protokoll a korrelációs is deklarálja a `Correlation-Context` fejléc.</span><span class="sxs-lookup"><span data-stu-id="216c8-134">The HTTP Protocol for Correlation also declares the `Correlation-Context` header.</span></span> <span data-ttu-id="216c8-135">Azonban ki van hagyva itt az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="216c8-135">However, it's omitted here for simplicity.</span></span>

## <a name="queue-instrumentation"></a><span data-ttu-id="216c8-136">Várólista instrumentation</span><span class="sxs-lookup"><span data-stu-id="216c8-136">Queue instrumentation</span></span>
<span data-ttu-id="216c8-137">HTTP-kommunikációhoz létrehoztunk Önnek egy protokoll felelt meg a korrelációs részleteit.</span><span class="sxs-lookup"><span data-stu-id="216c8-137">For HTTP communication, we've created a protocol to pass correlation details.</span></span> <span data-ttu-id="216c8-138">Az egyes üzenetsorok protokollok adja át további metaadatokat együtt az üzenetet, és másokkal, nem lehet.</span><span class="sxs-lookup"><span data-stu-id="216c8-138">With some queues' protocols, you can pass additional metadata along with the message, and with others you can't.</span></span>

### <a name="service-bus-queue"></a><span data-ttu-id="216c8-139">Service Bus-üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="216c8-139">Service Bus queue</span></span>
<span data-ttu-id="216c8-140">Az Azure-ral [Service Bus-üzenetsorba](../service-bus-messaging/index.md), átadhatók egy tulajdonságcsomagot és az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="216c8-140">With the Azure [Service Bus queue](../service-bus-messaging/index.md), you can pass a property bag along with the message.</span></span> <span data-ttu-id="216c8-141">Felelt meg a korrelációs azonosító használjuk</span><span class="sxs-lookup"><span data-stu-id="216c8-141">We use it to pass the correlation ID.</span></span>

<span data-ttu-id="216c8-142">A Service Bus-üzenetsorba TCP-alapú technológiát használ.</span><span class="sxs-lookup"><span data-stu-id="216c8-142">The Service Bus queue uses TCP-based protocols.</span></span> <span data-ttu-id="216c8-143">Az Application Insights nem automatikusan nyomon követheti a várólista műveleteket, így manuálisan azt nyomon őket.</span><span class="sxs-lookup"><span data-stu-id="216c8-143">Application Insights doesn't automatically track queue operations, so we track them manually.</span></span> <span data-ttu-id="216c8-144">A dequeue művelet egy leküldéses stílusú API-t, és a rendszer nem tudja nyomon követni azt.</span><span class="sxs-lookup"><span data-stu-id="216c8-144">The dequeue operation is a push-style API, and we're unable to track it.</span></span>

#### <a name="enqueue"></a><span data-ttu-id="216c8-145">Sorba helyezni</span><span class="sxs-lookup"><span data-stu-id="216c8-145">Enqueue</span></span>

```C#
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes the telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows the property bag to pass along with the message.
    // We will use them to pass our correlation identifiers (and other context)
    // to the consumer.
    message.Properties.Add("ParentId", operation.Telemetry.Id);
    message.Properties.Add("RootId", operation.Telemetry.Context.Operation.Id);

    try
    {
        await queue.SendAsync(message);
        
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = true;
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = false;
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

#### <a name="process"></a><span data-ttu-id="216c8-146">Folyamat</span><span class="sxs-lookup"><span data-stu-id="216c8-146">Process</span></span>
```C#
public async Task Process(BrokeredMessage message)
{
    // After the message is taken from the queue, create RequestTelemetry to track its processing.
    // It might also make sense to get the name from the message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
    requestTelemetry.Context.Operation.Id = rootId;
    requestTelemetry.Context.Operation.ParentId = parentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

### <a name="azure-storage-queue"></a><span data-ttu-id="216c8-147">Az Azure Storage üzenetsorába</span><span class="sxs-lookup"><span data-stu-id="216c8-147">Azure Storage queue</span></span>
<span data-ttu-id="216c8-148">A következő példa bemutatja, hogyan nyomon követéséhez a [Azure Storage üzenetsorába](../storage/queues/storage-dotnet-how-to-use-queues.md) műveletek és a gyártó fogyasztói, és az Azure Storage közötti összefüggésbe telemetriai.</span><span class="sxs-lookup"><span data-stu-id="216c8-148">The following example shows how to track the [Azure Storage queue](../storage/queues/storage-dotnet-how-to-use-queues.md) operations and correlate telemetry between the producer, the consumer, and Azure Storage.</span></span> 

<span data-ttu-id="216c8-149">A tároló várólista rendelkezik egy HTTP API-t.</span><span class="sxs-lookup"><span data-stu-id="216c8-149">The Storage queue has an HTTP API.</span></span> <span data-ttu-id="216c8-150">A várólistára minden hívást követi az Application Insights függőségi gyűjtő a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="216c8-150">All calls to the queue are tracked by the Application Insights Dependency Collector for HTTP requests.</span></span>
<span data-ttu-id="216c8-151">Győződjön meg arról, hogy `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` a `applicationInsights.config`.</span><span class="sxs-lookup"><span data-stu-id="216c8-151">Make sure you have `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config`.</span></span> <span data-ttu-id="216c8-152">Ha nem rendelkezik, adja hozzá a programozott módon [szűrést és az Azure Application Insights SDK előfeldolgozása](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="216c8-152">If you don't have it, add it programmatically as described in [Filtering and Preprocessing in the Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span></span>

<span data-ttu-id="216c8-153">Ha manuálisan adja meg az Application Insights, győződjön meg arról, létrehozása és inicializálása `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` mint:</span><span class="sxs-lookup"><span data-stu-id="216c8-153">If you configure Application Insights manually, make sure you create and initialize `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` similarly to:</span></span>
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection to some domains by adding it to the excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on the Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget to dispose of the module during application shutdown.
```

<span data-ttu-id="216c8-154">Érdemes azt is összefüggéseket az Application Insights Műveletazonosító tároló kérelemben azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="216c8-154">You also might want to correlate the Application Insights operation ID with the Storage request ID.</span></span> <span data-ttu-id="216c8-155">Állítsa be, és egy tároló kérelemben ügyfél és a kiszolgáló Kérelemazonosító információkért lásd: [figyelése, diagnosztizálása és elhárítása az Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span><span class="sxs-lookup"><span data-stu-id="216c8-155">For information on how to set and get a Storage request client and a server request ID, see [Monitor, diagnose, and troubleshoot Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span></span>

#### <a name="enqueue"></a><span data-ttu-id="216c8-156">Sorba helyezni</span><span class="sxs-lookup"><span data-stu-id="216c8-156">Enqueue</span></span>
<span data-ttu-id="216c8-157">Tárolási sorok támogatja a HTTP API-t, mert a sor összes művelet automatikusan követi az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="216c8-157">Because Storage queues support the HTTP API, all operations with the queue are automatically tracked by Application Insights.</span></span> <span data-ttu-id="216c8-158">Sok esetben elegendő a instrumentation kell lennie.</span><span class="sxs-lookup"><span data-stu-id="216c8-158">In many cases, this instrumentation should be enough.</span></span> <span data-ttu-id="216c8-159">Azonban összefüggéseket készítő nyomkövetési adatokat a fogyasztói oldalon a nyomkövetéseket, át kell korrelációs környezete hasonlóképpen hogyan azt ehhez a HTTP protokoll korrelációhoz.</span><span class="sxs-lookup"><span data-stu-id="216c8-159">However, to correlate traces on the consumer side with producer traces, you must pass some correlation context similarly to how we do it in the HTTP Protocol for Correlation.</span></span> 

<span data-ttu-id="216c8-160">Ebben a példában azt a nem kötelező követni `Enqueue` műveletet.</span><span class="sxs-lookup"><span data-stu-id="216c8-160">In this example, we track the optional `Enqueue` operation.</span></span> <span data-ttu-id="216c8-161">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="216c8-161">You can:</span></span>

 - <span data-ttu-id="216c8-162">**Összefüggéseket találni az újrapróbálkozások között (ha van ilyen)**: minden rendelkeznek egy közös szülő, amely rendelkezik a `Enqueue` műveletet.</span><span class="sxs-lookup"><span data-stu-id="216c8-162">**Correlate retries (if any)**: They all have one common parent that's the `Enqueue` operation.</span></span> <span data-ttu-id="216c8-163">Ellenkező esetben ezek még nyomon követi a bejövő kérelem gyermekeként.</span><span class="sxs-lookup"><span data-stu-id="216c8-163">Otherwise, they're tracked as children of the incoming request.</span></span> <span data-ttu-id="216c8-164">Ha a várólista logikai kéréseket, hívást eredményezett újrapróbálkozások található nehéz lehet.</span><span class="sxs-lookup"><span data-stu-id="216c8-164">If there are multiple logical requests to the queue, it might be difficult to find which call resulted in retries.</span></span>
 - <span data-ttu-id="216c8-165">**A tárolási naplófájljai összefüggéseket (ha szükséges)**: azok az Application Insights telemetria most tartozzanak.</span><span class="sxs-lookup"><span data-stu-id="216c8-165">**Correlate Storage logs (if and when needed)**: They're correlated with Application Insights telemetry.</span></span>

<span data-ttu-id="216c8-166">A `Enqueue` művelet egy szülő műveletet (például egy bejövő HTTP-kérelem) gyermeke.</span><span class="sxs-lookup"><span data-stu-id="216c8-166">The `Enqueue` operation is the child of a parent operation (for example, an incoming HTTP request).</span></span> <span data-ttu-id="216c8-167">A HTTP-függőségi hívás nem gyermeke a `Enqueue` művelet és a bejövő kérelem unoka:</span><span class="sxs-lookup"><span data-stu-id="216c8-167">The HTTP dependency call is the child of the `Enqueue` operation and the grandchild of the incoming request:</span></span>

```C#
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose to pass payload serialized to JSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message to process'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id to the OperationContext to correlate Storage logs and Application Insights telemetry.
    OperationContext context = new OperationContext { ClientRequestID = operation.Telemetry.Id};

    try
    {
        await queue.AddMessageAsync(queueMessage, null, null, new QueueRequestOptions(), context);
    }
    catch (StorageException e)
    {
        operation.Telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.Telemetry.Success = false;
        operation.Telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}  
```

<span data-ttu-id="216c8-168">A telemetriai adatok mennyiségének csökkentésére az alkalmazás jelentéseket, vagy ha nem kívánja nyomon követni a `Enqueue` más okok miatt, használja a `Activity` API közvetlenül:</span><span class="sxs-lookup"><span data-stu-id="216c8-168">To reduce the amount of telemetry your application reports or if you don't want to track the `Enqueue` operation for other reasons, use the `Activity` API directly:</span></span>

- <span data-ttu-id="216c8-169">Hozzon létre (és indítása) egy új `Activity` helyett az Application Insights művelet elindítása.</span><span class="sxs-lookup"><span data-stu-id="216c8-169">Create (and start) a new `Activity` instead of starting the Application Insights operation.</span></span> <span data-ttu-id="216c8-170">Ezt megteheti *nem* kell hozzárendelni kívánt tulajdonságokat rajta a művelet nevén kívül.</span><span class="sxs-lookup"><span data-stu-id="216c8-170">You do *not* need to assign any properties on it except the operation name.</span></span>
- <span data-ttu-id="216c8-171">Szerializálható `yourActivity.Id` ahelyett, hogy a üzenetadatokat be `operation.Telemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="216c8-171">Serialize `yourActivity.Id` into the message payload instead of `operation.Telemetry.Id`.</span></span> <span data-ttu-id="216c8-172">Is `Activity.Current.Id`.</span><span class="sxs-lookup"><span data-stu-id="216c8-172">You can also use `Activity.Current.Id`.</span></span>


#### <a name="dequeue"></a><span data-ttu-id="216c8-173">Created</span><span class="sxs-lookup"><span data-stu-id="216c8-173">Dequeue</span></span>
<span data-ttu-id="216c8-174">Hasonlóképpen, a `Enqueue`, a tároló várólista tényleges HTTP-kérelem automatikusan követi nyomon az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="216c8-174">Similarly to `Enqueue`, an actual HTTP request to the Storage queue is automatically tracked by Application Insights.</span></span> <span data-ttu-id="216c8-175">Azonban a `Enqueue` művelet feltehetően történik, a szülő környezetben, például egy bejövő kérelem környezete.</span><span class="sxs-lookup"><span data-stu-id="216c8-175">However, the `Enqueue` operation presumably happens in the parent context, such as an incoming request context.</span></span> <span data-ttu-id="216c8-176">Application Insights SDK-k automatikusan összefüggéseket ilyen művelet (és a HTTP része) a szülő kérelem és egyéb telemetriai jelentett ugyanabban a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="216c8-176">Application Insights SDKs automatically correlate such an operation (and its HTTP part) with the parent request and other telemetry reported in the same scope.</span></span>

<span data-ttu-id="216c8-177">A `Dequeue` legbonyolultabb művelet.</span><span class="sxs-lookup"><span data-stu-id="216c8-177">The `Dequeue` operation is tricky.</span></span> <span data-ttu-id="216c8-178">Az Application Insights SDK automatikusan nyomon követi a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="216c8-178">The Application Insights SDK automatically tracks HTTP requests.</span></span> <span data-ttu-id="216c8-179">A korrelációs környezetben azonban azt nem ismert mindaddig, amíg az üzenetet a rendszer értelmezi.</span><span class="sxs-lookup"><span data-stu-id="216c8-179">However, it doesn't know the correlation context until the message is parsed.</span></span> <span data-ttu-id="216c8-180">Nincs lehetőség a HTTP-kérelem és a telemetriai adatokat a többi üzenet összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="216c8-180">It's not possible to correlate the HTTP request to get the message with the rest of the telemetry.</span></span>

<span data-ttu-id="216c8-181">Sok esetben hasznos lehet a más nyomkövetési adatokat, valamint a HTTP-kérelem a várólista összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="216c8-181">In many cases, it might be useful to correlate the HTTP request to the queue with other traces as well.</span></span> <span data-ttu-id="216c8-182">A következő példa bemutatja, hogyan teheti meg:</span><span class="sxs-lookup"><span data-stu-id="216c8-182">The following example demonstrates how to do it:</span></span>

``` C#
public async Task<MessagePayload> Dequeue(CloudQueue queue)
{
    var telemetry = new DependencyTelemetry
    {
        Type = "Queue",
        Name = "Dequeue " + queue.Name
    };

    telemetry.Start();

    try
    {
        var message = await queue.GetMessageAsync();

        if (message != null)
        {
            var payload = JsonConvert.DeserializeObject<MessagePayload>(message.AsString);

            // If there is a message, we want to correlate the Dequeue operation with processing.
            // However, we will only know what correlation ID to use after we get it from the message,
            // so we will report telemetry after we know the IDs.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // Delete the message.
            return payload;
        }
    }
    catch (StorageException e)
    {
        telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        telemetry.Success = false;
        telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetry.Stop();
        telemetryClient.Track(telemetry);
    }

    return null;
}
```

#### <a name="process"></a><span data-ttu-id="216c8-183">Folyamat</span><span class="sxs-lookup"><span data-stu-id="216c8-183">Process</span></span>

<span data-ttu-id="216c8-184">A következő példában azt egy bejövő üzenet módon hasonlóképpen követéséhez hogyan azt egy bejövő HTTP-kérelem nyomon követése:</span><span class="sxs-lookup"><span data-stu-id="216c8-184">In the following example, we trace an incoming message in a manner similarly to how we trace an incoming HTTP request:</span></span>

```C#
public async Task Process(MessagePayload message)
{
    // After the message is dequeued from the queue, create RequestTelemetry to track its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // It might also make sense to get the name from the message.
    requestTelemetry.Context.Operation.Id = message.RootId;
    requestTelemetry.Context.Operation.ParentId = message.ParentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

<span data-ttu-id="216c8-185">Hasonlóképpen a többi üzenetsor-műveletet tagolva is.</span><span class="sxs-lookup"><span data-stu-id="216c8-185">Similarly, other queue operations can be instrumented.</span></span> <span data-ttu-id="216c8-186">Egy betekintés művelet egy dequeue művelet, hasonló módon lesznek tagolva.</span><span class="sxs-lookup"><span data-stu-id="216c8-186">A peek operation should be instrumented in a similar way as a dequeue operation.</span></span> <span data-ttu-id="216c8-187">Üzenetsor-kezelési műveletet tagolása nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="216c8-187">Instrumenting queue management operations isn't necessary.</span></span> <span data-ttu-id="216c8-188">Az Application Insights nyomon követi a műveletek, például HTTP, és a legtöbb esetben elegendő.</span><span class="sxs-lookup"><span data-stu-id="216c8-188">Application Insights tracks operations such as HTTP, and in most cases, it's enough.</span></span>

<span data-ttu-id="216c8-189">Amikor állíthatnak üzenet törlése, ellenőrizze, hogy beállította a művelet (korrelációs) azonosítót.</span><span class="sxs-lookup"><span data-stu-id="216c8-189">When you instrument message deletion, make sure you set the operation (correlation) identifiers.</span></span> <span data-ttu-id="216c8-190">Másik lehetőségként használhatja a `Activity` API.</span><span class="sxs-lookup"><span data-stu-id="216c8-190">Alternatively, you can use the `Activity` API.</span></span> <span data-ttu-id="216c8-191">Majd nincs szükség művelet azonosítók a telemetriai adatok elemek beállítani, mert az Application Insights minderre meg:</span><span class="sxs-lookup"><span data-stu-id="216c8-191">Then you don't need to set operation identifiers on the telemetry items because Application Insights does it for you:</span></span>

- <span data-ttu-id="216c8-192">Hozzon létre egy új `Activity` után egy elemet az üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="216c8-192">Create a new `Activity` after you've got an item from the queue.</span></span>
- <span data-ttu-id="216c8-193">Használjon `Activity.SetParentId(message.ParentId)` fogyasztói és gyártó naplók összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="216c8-193">Use `Activity.SetParentId(message.ParentId)` to correlate consumer and producer logs.</span></span>
- <span data-ttu-id="216c8-194">Indítsa el a `Activity`.</span><span class="sxs-lookup"><span data-stu-id="216c8-194">Start the `Activity`.</span></span>
- <span data-ttu-id="216c8-195">Track created, feldolgozása és törlési műveletek használatával `Start/StopOperation` segítő.</span><span class="sxs-lookup"><span data-stu-id="216c8-195">Track dequeue, process, and delete operations by using `Start/StopOperation` helpers.</span></span> <span data-ttu-id="216c8-196">Ehhez a a azonos aszinkron folyamatábrán (a végrehajtási környezet).</span><span class="sxs-lookup"><span data-stu-id="216c8-196">Do it from the same asynchronous control flow (execution context).</span></span> <span data-ttu-id="216c8-197">Ezzel a módszerrel azok még korrelált megfelelően.</span><span class="sxs-lookup"><span data-stu-id="216c8-197">In this way, they're correlated properly.</span></span>
- <span data-ttu-id="216c8-198">Állítsa le a `Activity`.</span><span class="sxs-lookup"><span data-stu-id="216c8-198">Stop the `Activity`.</span></span>
- <span data-ttu-id="216c8-199">Használjon `Start/StopOperation`, vagy hívja az `Track` telemetriai manuálisan.</span><span class="sxs-lookup"><span data-stu-id="216c8-199">Use `Start/StopOperation`, or call `Track` telemetry manually.</span></span>

### <a name="batch-processing"></a><span data-ttu-id="216c8-200">Kötegfeldolgozási</span><span class="sxs-lookup"><span data-stu-id="216c8-200">Batch processing</span></span>
<span data-ttu-id="216c8-201">Az egyes üzenetsorok egy kérelem több üzenetet is created.</span><span class="sxs-lookup"><span data-stu-id="216c8-201">With some queues, you can dequeue multiple messages with one request.</span></span> <span data-ttu-id="216c8-202">Ilyen üzenetek feldolgozása feltételezhetően független, és a különböző logikai műveletek tartozik.</span><span class="sxs-lookup"><span data-stu-id="216c8-202">Processing such messages is presumably independent and belongs to the different logical operations.</span></span> <span data-ttu-id="216c8-203">Ebben az esetben nincs lehetőség a összefüggéseket a `Dequeue` adott üzenetfeldolgozást művelet.</span><span class="sxs-lookup"><span data-stu-id="216c8-203">In this case, it's not possible to correlate the `Dequeue` operation to particular message processing.</span></span>

<span data-ttu-id="216c8-204">Minden üzenet fel kell dolgozni a saját aszinkron vezérlési folyamatában.</span><span class="sxs-lookup"><span data-stu-id="216c8-204">Each message should be processed in its own asynchronous control flow.</span></span> <span data-ttu-id="216c8-205">További információkért lásd: a [követési kimenő függőségek](#outgoing-dependencies-tracking) szakasz.</span><span class="sxs-lookup"><span data-stu-id="216c8-205">For more information, see the [Outgoing dependencies tracking](#outgoing-dependencies-tracking) section.</span></span>

## <a name="long-running-background-tasks"></a><span data-ttu-id="216c8-206">Hosszan futó háttérfeladatok</span><span class="sxs-lookup"><span data-stu-id="216c8-206">Long-running background tasks</span></span>
<span data-ttu-id="216c8-207">Egyes alkalmazások start hosszú futású műveleteket, amelyek a felhasználói kérelmek oka lehet.</span><span class="sxs-lookup"><span data-stu-id="216c8-207">Some applications start long-running operations that might be caused by user requests.</span></span> <span data-ttu-id="216c8-208">A nyomkövetés/instrumentation szempontjából nincs eltér a kérelem vagy függőségi instrumentation:</span><span class="sxs-lookup"><span data-stu-id="216c8-208">From the tracing/instrumentation perspective, it's not different from request or dependency instrumentation:</span></span> 

``` C#
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<RequestTelemetry>(taskName);
    operation.Telemetry.Type = "Background";
    try
    {
        int progress = 0;
        while (progress < 100)
        {
            // Process the task.
            telemetryClient.TrackTrace($"done {progress++}%");
        }
        // Update status code and success as appropriate.
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Update status code and success as appropriate.
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

<span data-ttu-id="216c8-209">A jelen példában használjuk `telemetryClient.StartOperation` létrehozásához `RequestTelemetry` , és adja meg a korrelációs környezetben.</span><span class="sxs-lookup"><span data-stu-id="216c8-209">In this example, we use `telemetryClient.StartOperation` to create `RequestTelemetry` and fill the correlation context.</span></span> <span data-ttu-id="216c8-210">Tegyük fel, a bejövő kérelmeket, amelyek ütemezett a művelet által létrehozott szülő művelet van.</span><span class="sxs-lookup"><span data-stu-id="216c8-210">Let's say you have a parent operation that was created by incoming requests that scheduled the operation.</span></span> <span data-ttu-id="216c8-211">Amennyiben a megjelölt `BackgroundTask` ugyanazon aszinkron elindul egy bejövő kérelem folyamata kontrolljához szülő művelet tartozzanak.</span><span class="sxs-lookup"><span data-stu-id="216c8-211">As long as `BackgroundTask` starts in the same asynchronous control flow as an incoming request, it's correlated with that parent operation.</span></span> <span data-ttu-id="216c8-212">`BackgroundTask`és minden beágyazott telemetriai elem mellékel a kérelemhez, amely a, a kérelem befejeződését követően automatikusan közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="216c8-212">`BackgroundTask` and all nested telemetry items are automatically correlated with the request that caused it, even after the request ends.</span></span>

<span data-ttu-id="216c8-213">A feladat indításakor a háttérben szálból, amely nem tartalmaz semmilyen műveletet (`Activity`) társított, `BackgroundTask` bármelyik szülő nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="216c8-213">When the task starts from the background thread that doesn't have any operation (`Activity`) associated with it, `BackgroundTask` doesn't have any parent.</span></span> <span data-ttu-id="216c8-214">Azonban azt is beágyazott műveletek.</span><span class="sxs-lookup"><span data-stu-id="216c8-214">However, it can have nested operations.</span></span> <span data-ttu-id="216c8-215">A feladat jelentett összes telemetriai elemet összefüggő a `RequestTelemetry` létrehozott `BackgroundTask`.</span><span class="sxs-lookup"><span data-stu-id="216c8-215">All telemetry items reported from the task are correlated to the `RequestTelemetry` created in `BackgroundTask`.</span></span>

## <a name="outgoing-dependencies-tracking"></a><span data-ttu-id="216c8-216">Kimenő függőségek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="216c8-216">Outgoing dependencies tracking</span></span>
<span data-ttu-id="216c8-217">Nyomon követheti a saját függőségi fajta vagy az Application Insights nem támogatja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="216c8-217">You can track your own dependency kind or an operation that's not supported by Application Insights.</span></span>

<span data-ttu-id="216c8-218">A `Enqueue` metódus a Service Bus-üzenetsorba, vagy a tároló várólista ilyen egyéni követési példák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="216c8-218">The `Enqueue` method in the Service Bus queue or the Storage queue can serve as examples for such custom tracking.</span></span>

<span data-ttu-id="216c8-219">Az általános módszer követési egyéni függőséget, hogy:</span><span class="sxs-lookup"><span data-stu-id="216c8-219">The general approach for custom dependency tracking is to:</span></span>

- <span data-ttu-id="216c8-220">Hívja a `TelemetryClient.StartOperation` (kiterjesztés) módszer, amelynek változó kitöltése a `DependencyTelemetry` összefüggések keresésére és néhány egyéb tulajdonság szükséges tulajdonságok (kezdő időpont stamp, időtartama).</span><span class="sxs-lookup"><span data-stu-id="216c8-220">Call the `TelemetryClient.StartOperation` (extension) method that fills the `DependencyTelemetry` properties that are needed for correlation and some other properties (start  time stamp, duration).</span></span>
- <span data-ttu-id="216c8-221">Egyéb egyéni tulajdonságának beállítása a `DependencyTelemetry`, például a nevét és minden egyéb környezetre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="216c8-221">Set other custom properties on the `DependencyTelemetry`, such as the name and any other context you need.</span></span>
- <span data-ttu-id="216c8-222">Ellenőrizze a függőségi hívás, és várjon, amíg az.</span><span class="sxs-lookup"><span data-stu-id="216c8-222">Make a dependency call and wait for it.</span></span>
- <span data-ttu-id="216c8-223">Állítsa le a műveletet a `StopOperation` amikor befejeződött.</span><span class="sxs-lookup"><span data-stu-id="216c8-223">Stop the operation with `StopOperation` when it's finished.</span></span>
- <span data-ttu-id="216c8-224">Kivétel kezelése.</span><span class="sxs-lookup"><span data-stu-id="216c8-224">Handle exceptions.</span></span>

<span data-ttu-id="216c8-225">`StopOperation`csak leállítja a műveletet, amely elkezdődött.</span><span class="sxs-lookup"><span data-stu-id="216c8-225">`StopOperation` only stops the operation that was started.</span></span> <span data-ttu-id="216c8-226">Ha az aktuális futó művelet nem egyezik meg szeretné szüntetni, azzal `StopOperation` nincs semmi hatása.</span><span class="sxs-lookup"><span data-stu-id="216c8-226">If the current running operation doesn't match the one you want to stop, `StopOperation` does nothing.</span></span> <span data-ttu-id="216c8-227">Ez a helyzet akkor fordulhat elő, ha azonos végrehajtási környezetében párhuzamosan több művelet indítása:</span><span class="sxs-lookup"><span data-stu-id="216c8-227">This situation might happen if you start multiple operations in parallel in the same execution context:</span></span>

```C#
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// This will do nothing and will not report telemetry for the first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

<span data-ttu-id="216c8-228">Győződjön meg arról, hogy mindig hívja `StartOperation` és a feladat futtatásához a saját környezetben:</span><span class="sxs-lookup"><span data-stu-id="216c8-228">Make sure you always call `StartOperation` and run your task in its own context:</span></span>
```C#
public async Task RunMyTaskAsync()
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
    try 
    {
        var myTask = await StartMyTaskAsync();
        // Update status code and success as appropriate.
    }
    catch(...) 
    {
        // Update status code and success as appropriate.
    }
    finally 
    {
        telemetryClient.StopOperation(operation);
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="216c8-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="216c8-229">Next steps</span></span>

- <span data-ttu-id="216c8-230">Az alapvető [telemetriai korrelációs](application-insights-correlation.md) az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="216c8-230">Learn the basics of [telemetry correlation](application-insights-correlation.md) in Application Insights.</span></span>
- <span data-ttu-id="216c8-231">Tekintse meg a [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="216c8-231">See the [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="216c8-232">Egyéni jelentést [eseményeket és metrikákat](app-insights-api-custom-events-metrics.md) az Application Insights részére.</span><span class="sxs-lookup"><span data-stu-id="216c8-232">Report custom [events and metrics](app-insights-api-custom-events-metrics.md) to Application Insights.</span></span>
- <span data-ttu-id="216c8-233">Tekintse meg a standard [konfigurációs](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) környezeti tulajdonságok gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="216c8-233">Check out standard [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) for context properties collection.</span></span>
- <span data-ttu-id="216c8-234">Ellenőrizze a [System.Diagnostics.Activity felhasználói útmutató](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) hogyan azt összefüggéseket telemetriai adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="216c8-234">Check the [System.Diagnostics.Activity User Guide](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) to see how we correlate telemetry.</span></span>

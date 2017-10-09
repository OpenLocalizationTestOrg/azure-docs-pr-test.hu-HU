---
title: "Egyéni műveletek aaaTrack Azure Application Insights .NET SDK-val |} Microsoft Docs"
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
ms.openlocfilehash: fe338d3e2b17a3dae43c96c60a19f57b3f46f0a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a><span data-ttu-id="d849f-103">Application Insights .NET SDK-val egyéni műveletek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="d849f-103">Track custom operations with Application Insights .NET SDK</span></span>

<span data-ttu-id="d849f-104">Az Azure Application Insights SDK-k automatikusan nyomon követése a bejövő HTTP-kérelmek, és meghívja a toodependent szolgáltatások, például a HTTP-kérelmek és az SQL-lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="d849f-104">Azure Application Insights SDKs automatically track incoming HTTP requests and calls toodependent services, such as HTTP requests and SQL queries.</span></span> <span data-ttu-id="d849f-105">Nyomon követését és a kérelmek és a függőségek korrelációs biztosítanak hello teljes alkalmazás válaszidejét és megbízhatóság láthatósága összes mikroszolgáltatások létrehozására, az alkalmazás felhasználó között.</span><span class="sxs-lookup"><span data-stu-id="d849f-105">Tracking and correlation of requests and dependencies give you visibility into hello whole application's responsiveness and reliability across all microservices that combine this application.</span></span> 

<span data-ttu-id="d849f-106">Nincs alkalmazás mintáról olvashat, amelyek nem tudják támogatni az általános osztály.</span><span class="sxs-lookup"><span data-stu-id="d849f-106">There is a class of application patterns that can't be supported generically.</span></span> <span data-ttu-id="d849f-107">Az ilyen minták megfelelő figyeléshez szükséges manuális kód instrumentation.</span><span class="sxs-lookup"><span data-stu-id="d849f-107">Proper monitoring of such patterns requires manual code instrumentation.</span></span> <span data-ttu-id="d849f-108">Ez a cikk manuális instrumentation egyéni várólista feldolgozása és a hosszú futású háttérfeladatok futtatása például szükség lehet néhány mintázatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d849f-108">This article covers a few patterns that might require manual instrumentation, such as custom queue processing and running long-running background tasks.</span></span>

<span data-ttu-id="d849f-109">Ez a dokumentum hogyan egyéni műveletek tootrack hello Application Insights SDK nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="d849f-109">This document provides guidance on how tootrack custom operations with hello Application Insights SDK.</span></span> <span data-ttu-id="d849f-110">Ebben a dokumentációban fontos:</span><span class="sxs-lookup"><span data-stu-id="d849f-110">This documentation is relevant for:</span></span>

- <span data-ttu-id="d849f-111">Az Application Insights .NET (más néven alapvető SDK) verziójának 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="d849f-111">Application Insights for .NET (also known as Base SDK) version 2.4+.</span></span>
- <span data-ttu-id="d849f-112">Az Application Insights webes (ASP.NET futó) alkalmazások verziójához 2.4 +.</span><span class="sxs-lookup"><span data-stu-id="d849f-112">Application Insights for web applications (running ASP.NET) version 2.4+.</span></span>
- <span data-ttu-id="d849f-113">Az Application Insights az ASP.NET Core 2.1-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d849f-113">Application Insights for ASP.NET Core version 2.1+.</span></span>

## <a name="overview"></a><span data-ttu-id="d849f-114">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d849f-114">Overview</span></span>
<span data-ttu-id="d849f-115">Egy művelet egy logikai munkákat az alkalmazások futtatása.</span><span class="sxs-lookup"><span data-stu-id="d849f-115">An operation is a logical piece of work run by an application.</span></span> <span data-ttu-id="d849f-116">Az idő, időtartam, eredmény és olyan környezetben, például a felhasználónevet, a tulajdonságok és az eredmény végrehajtás start neve van.</span><span class="sxs-lookup"><span data-stu-id="d849f-116">It has a name, start time, duration, result, and a context of execution like user name, properties, and result.</span></span> <span data-ttu-id="d849f-117">Ha A műveletet kezdeményezett művelet B, majd művelet B be van állítva az A. szülője Egy művelet csak egy szülőhöz lehet, de sok gyermek művelet veheti fel.</span><span class="sxs-lookup"><span data-stu-id="d849f-117">If operation A was initiated by operation B, then operation B is set as a parent for A. An operation can have only one parent, but it can have many child operations.</span></span> <span data-ttu-id="d849f-118">Műveletek és telemetriai korrelációs további információkért lásd: [Azure Application Insights telemetria korrelációs](application-insights-correlation.md).</span><span class="sxs-lookup"><span data-stu-id="d849f-118">For more information on operations and telemetry correlation, see [Azure Application Insights telemetry correlation](application-insights-correlation.md).</span></span>

<span data-ttu-id="d849f-119">Az Application Insights .NET SDK hello, hello művelet le hello absztrakt osztály [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) és a leszármazottai [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) és [DependencyTelemetry ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span><span class="sxs-lookup"><span data-stu-id="d849f-119">In hello Application Insights .NET SDK, hello operation is described by hello abstract class [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) and its descendants [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) and [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).</span></span>

## <a name="incoming-operations-tracking"></a><span data-ttu-id="d849f-120">Bejövő műveletek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="d849f-120">Incoming operations tracking</span></span> 
<span data-ttu-id="d849f-121">hello Application Insights webes SDK automatikusan gyűjti az ASP.NET futtatni egy IIS-feldolgozási folyamat összes ASP.NET Core alkalmazásokat és a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="d849f-121">hello Application Insights web SDK automatically collects HTTP requests for ASP.NET applications that run in an IIS pipeline and all ASP.NET Core applications.</span></span> <span data-ttu-id="d849f-122">Nincsenek megoldások Közösség által támogatott más platformok és -keretrendszerek számára.</span><span class="sxs-lookup"><span data-stu-id="d849f-122">There are community-supported solutions for other platforms and frameworks.</span></span> <span data-ttu-id="d849f-123">Azonban hello alkalmazás bármelyik hello standard vagy a Közösség által támogatott megoldások nem támogatott, ha Ön állíthatnak be azt manuálisan.</span><span class="sxs-lookup"><span data-stu-id="d849f-123">However, if hello application isn't supported by any of hello standard or community-supported solutions, you can instrument it manually.</span></span>

<span data-ttu-id="d849f-124">Egy másik, amelyhez az egyéni nyomkövetési: hello munkavégző, amely megkapja a hello várólistában lévő elemeket.</span><span class="sxs-lookup"><span data-stu-id="d849f-124">Another example that requires custom tracking is hello worker that receives items from hello queue.</span></span> <span data-ttu-id="d849f-125">Egyes várólisták hello toothis várólista követhető nyomon a függőség beállításához üzenet hívja tooadd.</span><span class="sxs-lookup"><span data-stu-id="d849f-125">For some queues, hello call tooadd a message toothis queue is tracked as a dependency.</span></span> <span data-ttu-id="d849f-126">Azonban hello magas szintű műveletet leíró üzenet feldolgozása nem automatikusan összegyűjtött.</span><span class="sxs-lookup"><span data-stu-id="d849f-126">However, hello high-level operation that describes message processing is not automatically collected.</span></span>

<span data-ttu-id="d849f-127">Nézzük meg, hogyan azt nyomon az ilyen műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d849f-127">Let's see how we can track such operations.</span></span>

<span data-ttu-id="d849f-128">Magas szinten hello feladata toocreate `RequestTelemetry` és ismert tulajdonságainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="d849f-128">On a high level, hello task is toocreate `RequestTelemetry` and set known properties.</span></span> <span data-ttu-id="d849f-129">Hello művelet befejezése után hello telemetriai nyomon követéséhez.</span><span class="sxs-lookup"><span data-stu-id="d849f-129">After hello operation is finished, you track hello telemetry.</span></span> <span data-ttu-id="d849f-130">hello a következő példa azt mutatja be ezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="d849f-130">hello following example demonstrates this task.</span></span>

### <a name="http-request-in-owin-self-hosted-app"></a><span data-ttu-id="d849f-131">Önálló üzemeltetett alkalmazás Owin HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="d849f-131">HTTP request in Owin self-hosted app</span></span>
<span data-ttu-id="d849f-132">Ebben a példában, azt követve hello [HTTP protokoll a korrelációs](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d849f-132">In this example, we follow hello [HTTP Protocol for Correlation](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).</span></span> <span data-ttu-id="d849f-133">Nem ismertetett tooreceive fejlécek kell látnia.</span><span class="sxs-lookup"><span data-stu-id="d849f-133">You should expect tooreceive headers that are described there.</span></span>

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

        // If there is a Request-Id received from hello upstream service, set hello telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get hello operation ID from hello Request-Id (if you follow hello HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process hello request.
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

            // Now it's time toostop hello operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns hello root ID from hello '|' toohello first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

<span data-ttu-id="d849f-134">hello korrelációs HTTP protokollt is deklarál hello `Correlation-Context` fejléc.</span><span class="sxs-lookup"><span data-stu-id="d849f-134">hello HTTP Protocol for Correlation also declares hello `Correlation-Context` header.</span></span> <span data-ttu-id="d849f-135">Azonban ki van hagyva itt az egyszerűség érdekében.</span><span class="sxs-lookup"><span data-stu-id="d849f-135">However, it's omitted here for simplicity.</span></span>

## <a name="queue-instrumentation"></a><span data-ttu-id="d849f-136">Várólista instrumentation</span><span class="sxs-lookup"><span data-stu-id="d849f-136">Queue instrumentation</span></span>
<span data-ttu-id="d849f-137">HTTP-kommunikációhoz létrehoztunk Önnek egy protokoll toopass korrelációs részleteit.</span><span class="sxs-lookup"><span data-stu-id="d849f-137">For HTTP communication, we've created a protocol toopass correlation details.</span></span> <span data-ttu-id="d849f-138">Az egyes üzenetsorok protokollok adja át további metaadatokat üdvözlőüzenetére együtt, és nem lehet másokkal.</span><span class="sxs-lookup"><span data-stu-id="d849f-138">With some queues' protocols, you can pass additional metadata along with hello message, and with others you can't.</span></span>

### <a name="service-bus-queue"></a><span data-ttu-id="d849f-139">Service Bus-üzenetsorba</span><span class="sxs-lookup"><span data-stu-id="d849f-139">Service Bus queue</span></span>
<span data-ttu-id="d849f-140">Az hello Azure [Service Bus-üzenetsorba](../service-bus-messaging/index.md), átadhatók egy tulajdonságcsomagot üdvözlőüzenetére együtt.</span><span class="sxs-lookup"><span data-stu-id="d849f-140">With hello Azure [Service Bus queue](../service-bus-messaging/index.md), you can pass a property bag along with hello message.</span></span> <span data-ttu-id="d849f-141">Használjuk, toopass hello korrelációs azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d849f-141">We use it toopass hello correlation ID.</span></span>

<span data-ttu-id="d849f-142">Service Bus-üzenetsorba hello TCP-alapú technológiát használ.</span><span class="sxs-lookup"><span data-stu-id="d849f-142">hello Service Bus queue uses TCP-based protocols.</span></span> <span data-ttu-id="d849f-143">Az Application Insights nem automatikusan nyomon követheti a várólista műveleteket, így manuálisan azt nyomon őket.</span><span class="sxs-lookup"><span data-stu-id="d849f-143">Application Insights doesn't automatically track queue operations, so we track them manually.</span></span> <span data-ttu-id="d849f-144">hello created művelet egy leküldéses stílusú API-t, és nem tootrack el azt.</span><span class="sxs-lookup"><span data-stu-id="d849f-144">hello dequeue operation is a push-style API, and we're unable tootrack it.</span></span>

#### <a name="enqueue"></a><span data-ttu-id="d849f-145">Sorba helyezni</span><span class="sxs-lookup"><span data-stu-id="d849f-145">Enqueue</span></span>

```C#
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes hello telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows hello property bag toopass along with hello message.
    // We will use them toopass our correlation identifiers (and other context)
    // toohello consumer.
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

#### <a name="process"></a><span data-ttu-id="d849f-146">Folyamat</span><span class="sxs-lookup"><span data-stu-id="d849f-146">Process</span></span>
```C#
public async Task Process(BrokeredMessage message)
{
    // After hello message is taken from hello queue, create RequestTelemetry tootrack its processing.
    // It might also make sense tooget hello name from hello message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get hello operation ID from hello Request-Id (if you follow hello HTTP Protocol for Correlation).
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

### <a name="azure-storage-queue"></a><span data-ttu-id="d849f-147">Az Azure Storage üzenetsorába</span><span class="sxs-lookup"><span data-stu-id="d849f-147">Azure Storage queue</span></span>
<span data-ttu-id="d849f-148">a következő példa azt mutatja meg hogyan hello tootrack hello [Azure Storage üzenetsorába](../storage/queues/storage-dotnet-how-to-use-queues.md) műveletek és összefüggésbe telemetriai hello készítő, hello fogyasztói és az Azure Storage között.</span><span class="sxs-lookup"><span data-stu-id="d849f-148">hello following example shows how tootrack hello [Azure Storage queue](../storage/queues/storage-dotnet-how-to-use-queues.md) operations and correlate telemetry between hello producer, hello consumer, and Azure Storage.</span></span> 

<span data-ttu-id="d849f-149">hello tárolási várólistának egy HTTP API-t.</span><span class="sxs-lookup"><span data-stu-id="d849f-149">hello Storage queue has an HTTP API.</span></span> <span data-ttu-id="d849f-150">Minden hívások toohello várólista kötetblokkok hello Application Insights függőségi adatgyűjtő a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="d849f-150">All calls toohello queue are tracked by hello Application Insights Dependency Collector for HTTP requests.</span></span>
<span data-ttu-id="d849f-151">Győződjön meg arról, hogy `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` a `applicationInsights.config`.</span><span class="sxs-lookup"><span data-stu-id="d849f-151">Make sure you have `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` in `applicationInsights.config`.</span></span> <span data-ttu-id="d849f-152">Ha nem rendelkezik, adja hozzá a programozott módon [szűrést és a hello Azure Application Insights SDK előfeldolgozása](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="d849f-152">If you don't have it, add it programmatically as described in [Filtering and Preprocessing in hello Azure Application Insights SDK](app-insights-api-filtering-sampling.md).</span></span>

<span data-ttu-id="d849f-153">Ha manuálisan adja meg az Application Insights, győződjön meg arról, létrehozása és inicializálása `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` mint:</span><span class="sxs-lookup"><span data-stu-id="d849f-153">If you configure Application Insights manually, make sure you create and initialize `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` similarly to:</span></span>
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection toosome domains by adding it toohello excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on hello Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget toodispose of hello module during application shutdown.
```

<span data-ttu-id="d849f-154">Érdemes azt is toocorrelate hello Application Insights Műveletazonosító hello tároló kérelemben azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="d849f-154">You also might want toocorrelate hello Application Insights operation ID with hello Storage request ID.</span></span> <span data-ttu-id="d849f-155">Hogyan tooset és a tárolási get kérelmek ügyfél és a kiszolgáló Kérelemazonosító információkért lásd: [figyelése, diagnosztizálása és elhárítása az Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span><span class="sxs-lookup"><span data-stu-id="d849f-155">For information on how tooset and get a Storage request client and a server request ID, see [Monitor, diagnose, and troubleshoot Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).</span></span>

#### <a name="enqueue"></a><span data-ttu-id="d849f-156">Sorba helyezni</span><span class="sxs-lookup"><span data-stu-id="d849f-156">Enqueue</span></span>
<span data-ttu-id="d849f-157">Tárolási sorok hello HTTP API támogatja, mert hello várakozási sorral rendelkező összes művelet automatikusan követi az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d849f-157">Because Storage queues support hello HTTP API, all operations with hello queue are automatically tracked by Application Insights.</span></span> <span data-ttu-id="d849f-158">Sok esetben elegendő a instrumentation kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d849f-158">In many cases, this instrumentation should be enough.</span></span> <span data-ttu-id="d849f-159">Azonban a gyártó nyomkövetések hello fogyasztói oldalán hívásláncainak toocorrelate, át kell adnia néhány korrelációs környezetben hasonlóképpen toohow végezzük azt a HTTP protokoll a korrelációs hello.</span><span class="sxs-lookup"><span data-stu-id="d849f-159">However, toocorrelate traces on hello consumer side with producer traces, you must pass some correlation context similarly toohow we do it in hello HTTP Protocol for Correlation.</span></span> 

<span data-ttu-id="d849f-160">Ebben a példában a Microsoft hello választható nyomon `Enqueue` műveletet.</span><span class="sxs-lookup"><span data-stu-id="d849f-160">In this example, we track hello optional `Enqueue` operation.</span></span> <span data-ttu-id="d849f-161">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="d849f-161">You can:</span></span>

 - <span data-ttu-id="d849f-162">**Összefüggéseket találni az újrapróbálkozások között (ha van ilyen)**: minden rendelkeznek, amely hello egy közös szülő `Enqueue` műveletet.</span><span class="sxs-lookup"><span data-stu-id="d849f-162">**Correlate retries (if any)**: They all have one common parent that's hello `Enqueue` operation.</span></span> <span data-ttu-id="d849f-163">Ellenkező esetben ezek még nyomon hello bejövő kérelem gyermekeként.</span><span class="sxs-lookup"><span data-stu-id="d849f-163">Otherwise, they're tracked as children of hello incoming request.</span></span> <span data-ttu-id="d849f-164">Ha több logikai kérelmek toohello várólista, a hívást eredményezett újrapróbálkozások nehéz toofind lehet.</span><span class="sxs-lookup"><span data-stu-id="d849f-164">If there are multiple logical requests toohello queue, it might be difficult toofind which call resulted in retries.</span></span>
 - <span data-ttu-id="d849f-165">**A tárolási naplófájljai összefüggéseket (ha szükséges)**: azok az Application Insights telemetria most tartozzanak.</span><span class="sxs-lookup"><span data-stu-id="d849f-165">**Correlate Storage logs (if and when needed)**: They're correlated with Application Insights telemetry.</span></span>

<span data-ttu-id="d849f-166">Hello `Enqueue` művelet egy szülő műveletet (például egy bejövő HTTP-kérelem) hello gyermeke.</span><span class="sxs-lookup"><span data-stu-id="d849f-166">hello `Enqueue` operation is hello child of a parent operation (for example, an incoming HTTP request).</span></span> <span data-ttu-id="d849f-167">hello HTTP függőségi hívás hello hello gyermeke `Enqueue` művelet és hello unoka hello bejövő kérelem:</span><span class="sxs-lookup"><span data-stu-id="d849f-167">hello HTTP dependency call is hello child of hello `Enqueue` operation and hello grandchild of hello incoming request:</span></span>

```C#
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose toopass payload serialized tooJSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message tooprocess'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id toohello OperationContext toocorrelate Storage logs and Application Insights telemetry.
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

<span data-ttu-id="d849f-168">telemetriai adatok mennyisége tooreduce hello az alkalmazás jelentéseket, vagy ha nem szeretné, hogy tootrack hello `Enqueue` más okokból használata hello művelet `Activity` közvetlenül API:</span><span class="sxs-lookup"><span data-stu-id="d849f-168">tooreduce hello amount of telemetry your application reports or if you don't want tootrack hello `Enqueue` operation for other reasons, use hello `Activity` API directly:</span></span>

- <span data-ttu-id="d849f-169">Hozzon létre (és indítása) egy új `Activity` helyett hello Application Insights művelet elindítása.</span><span class="sxs-lookup"><span data-stu-id="d849f-169">Create (and start) a new `Activity` instead of starting hello Application Insights operation.</span></span> <span data-ttu-id="d849f-170">Ezt megteheti *nem* kell tooassign hello művelet nevén kívül a kívánt tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="d849f-170">You do *not* need tooassign any properties on it except hello operation name.</span></span>
- <span data-ttu-id="d849f-171">Szerializálható `yourActivity.Id` hello üzenetadatokat ahelyett, hogy a `operation.Telemetry.Id`.</span><span class="sxs-lookup"><span data-stu-id="d849f-171">Serialize `yourActivity.Id` into hello message payload instead of `operation.Telemetry.Id`.</span></span> <span data-ttu-id="d849f-172">Is `Activity.Current.Id`.</span><span class="sxs-lookup"><span data-stu-id="d849f-172">You can also use `Activity.Current.Id`.</span></span>


#### <a name="dequeue"></a><span data-ttu-id="d849f-173">Created</span><span class="sxs-lookup"><span data-stu-id="d849f-173">Dequeue</span></span>
<span data-ttu-id="d849f-174">Hasonlóképpen túl`Enqueue`, egy tényleges HTTP toohello tároló várólistája automatikusan követi nyomon az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d849f-174">Similarly too`Enqueue`, an actual HTTP request toohello Storage queue is automatically tracked by Application Insights.</span></span> <span data-ttu-id="d849f-175">Azonban hello `Enqueue` művelet feltehetően hello szülő környezetben, például egy bejövő kérelem környezete zajlik.</span><span class="sxs-lookup"><span data-stu-id="d849f-175">However, hello `Enqueue` operation presumably happens in hello parent context, such as an incoming request context.</span></span> <span data-ttu-id="d849f-176">Application Insights SDK-k automatikusan egyeztetéséhez ilyen művelet (és a HTTP-rész) hello szülő kérelemmel és egyéb telemetriai jelentett hello azonos hatókör.</span><span class="sxs-lookup"><span data-stu-id="d849f-176">Application Insights SDKs automatically correlate such an operation (and its HTTP part) with hello parent request and other telemetry reported in hello same scope.</span></span>

<span data-ttu-id="d849f-177">Hello `Dequeue` legbonyolultabb művelet.</span><span class="sxs-lookup"><span data-stu-id="d849f-177">hello `Dequeue` operation is tricky.</span></span> <span data-ttu-id="d849f-178">Application Insights SDK hello automatikusan nyomon követi a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="d849f-178">hello Application Insights SDK automatically tracks HTTP requests.</span></span> <span data-ttu-id="d849f-179">Hello korrelációs környezetben azonban azt nem ismert amíg üdvözlőüzenetére elemzi.</span><span class="sxs-lookup"><span data-stu-id="d849f-179">However, it doesn't know hello correlation context until hello message is parsed.</span></span> <span data-ttu-id="d849f-180">Már nem lehetséges toocorrelate hello HTTP tooget hello kérelemüzenet hello többi hello telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="d849f-180">It's not possible toocorrelate hello HTTP request tooget hello message with hello rest of hello telemetry.</span></span>

<span data-ttu-id="d849f-181">Sok esetben hasznos toocorrelate hello HTTP kérelem toohello várólista más nyomkövetések is lehet.</span><span class="sxs-lookup"><span data-stu-id="d849f-181">In many cases, it might be useful toocorrelate hello HTTP request toohello queue with other traces as well.</span></span> <span data-ttu-id="d849f-182">hello következő példa bemutatja, hogyan toodo azt:</span><span class="sxs-lookup"><span data-stu-id="d849f-182">hello following example demonstrates how toodo it:</span></span>

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

            // If there is a message, we want toocorrelate hello Dequeue operation with processing.
            // However, we will only know what correlation ID toouse after we get it from hello message,
            // so we will report telemetry after we know hello IDs.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // Delete hello message.
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

#### <a name="process"></a><span data-ttu-id="d849f-183">Folyamat</span><span class="sxs-lookup"><span data-stu-id="d849f-183">Process</span></span>

<span data-ttu-id="d849f-184">A következő példa hello, azt követni a bejövő üzenet módon hasonlóképpen toohow azt nyomon követni a bejövő HTTP kérelem:</span><span class="sxs-lookup"><span data-stu-id="d849f-184">In hello following example, we trace an incoming message in a manner similarly toohow we trace an incoming HTTP request:</span></span>

```C#
public async Task Process(MessagePayload message)
{
    // After hello message is dequeued from hello queue, create RequestTelemetry tootrack its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // It might also make sense tooget hello name from hello message.
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

<span data-ttu-id="d849f-185">Hasonlóképpen a többi üzenetsor-műveletet tagolva is.</span><span class="sxs-lookup"><span data-stu-id="d849f-185">Similarly, other queue operations can be instrumented.</span></span> <span data-ttu-id="d849f-186">Egy betekintés művelet egy dequeue művelet, hasonló módon lesznek tagolva.</span><span class="sxs-lookup"><span data-stu-id="d849f-186">A peek operation should be instrumented in a similar way as a dequeue operation.</span></span> <span data-ttu-id="d849f-187">Üzenetsor-kezelési műveletet tagolása nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d849f-187">Instrumenting queue management operations isn't necessary.</span></span> <span data-ttu-id="d849f-188">Az Application Insights nyomon követi a műveletek, például HTTP, és a legtöbb esetben elegendő.</span><span class="sxs-lookup"><span data-stu-id="d849f-188">Application Insights tracks operations such as HTTP, and in most cases, it's enough.</span></span>

<span data-ttu-id="d849f-189">Amikor állíthatnak üzenet törlése, ellenőrizze, hogy beállította hello művelet (korrelációs) azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d849f-189">When you instrument message deletion, make sure you set hello operation (correlation) identifiers.</span></span> <span data-ttu-id="d849f-190">Másik lehetőségként használhatja a hello `Activity` API.</span><span class="sxs-lookup"><span data-stu-id="d849f-190">Alternatively, you can use hello `Activity` API.</span></span> <span data-ttu-id="d849f-191">Nem szükséges a tooset művelet azonosítók hello telemetriai elemek Application Insights minderre, mert:</span><span class="sxs-lookup"><span data-stu-id="d849f-191">Then you don't need tooset operation identifiers on hello telemetry items because Application Insights does it for you:</span></span>

- <span data-ttu-id="d849f-192">Hozzon létre egy új `Activity` után egy elem hello üzenetsorból.</span><span class="sxs-lookup"><span data-stu-id="d849f-192">Create a new `Activity` after you've got an item from hello queue.</span></span>
- <span data-ttu-id="d849f-193">Használjon `Activity.SetParentId(message.ParentId)` toocorrelate fogyasztói, a gyártó naplókat.</span><span class="sxs-lookup"><span data-stu-id="d849f-193">Use `Activity.SetParentId(message.ParentId)` toocorrelate consumer and producer logs.</span></span>
- <span data-ttu-id="d849f-194">Indítsa el a hello `Activity`.</span><span class="sxs-lookup"><span data-stu-id="d849f-194">Start hello `Activity`.</span></span>
- <span data-ttu-id="d849f-195">Track created, feldolgozása és törlési műveletek használatával `Start/StopOperation` segítő.</span><span class="sxs-lookup"><span data-stu-id="d849f-195">Track dequeue, process, and delete operations by using `Start/StopOperation` helpers.</span></span> <span data-ttu-id="d849f-196">Ehhez a hello azonos aszinkron szabályozása folyamata (a végrehajtási környezet).</span><span class="sxs-lookup"><span data-stu-id="d849f-196">Do it from hello same asynchronous control flow (execution context).</span></span> <span data-ttu-id="d849f-197">Ezzel a módszerrel azok még korrelált megfelelően.</span><span class="sxs-lookup"><span data-stu-id="d849f-197">In this way, they're correlated properly.</span></span>
- <span data-ttu-id="d849f-198">Állítsa le hello `Activity`.</span><span class="sxs-lookup"><span data-stu-id="d849f-198">Stop hello `Activity`.</span></span>
- <span data-ttu-id="d849f-199">Használjon `Start/StopOperation`, vagy hívja az `Track` telemetriai manuálisan.</span><span class="sxs-lookup"><span data-stu-id="d849f-199">Use `Start/StopOperation`, or call `Track` telemetry manually.</span></span>

### <a name="batch-processing"></a><span data-ttu-id="d849f-200">Kötegfeldolgozási</span><span class="sxs-lookup"><span data-stu-id="d849f-200">Batch processing</span></span>
<span data-ttu-id="d849f-201">Az egyes üzenetsorok egy kérelem több üzenetet is created.</span><span class="sxs-lookup"><span data-stu-id="d849f-201">With some queues, you can dequeue multiple messages with one request.</span></span> <span data-ttu-id="d849f-202">Ilyen üzenetek feldolgozása feltételezhetően független, és különböző logikai műveletek toohello tartozik.</span><span class="sxs-lookup"><span data-stu-id="d849f-202">Processing such messages is presumably independent and belongs toohello different logical operations.</span></span> <span data-ttu-id="d849f-203">Ebben az esetben nincs lehetséges toocorrelate hello `Dequeue` művelet tooparticular üzenet feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="d849f-203">In this case, it's not possible toocorrelate hello `Dequeue` operation tooparticular message processing.</span></span>

<span data-ttu-id="d849f-204">Minden üzenet fel kell dolgozni a saját aszinkron vezérlési folyamatában.</span><span class="sxs-lookup"><span data-stu-id="d849f-204">Each message should be processed in its own asynchronous control flow.</span></span> <span data-ttu-id="d849f-205">További információkért lásd: hello [követési kimenő függőségek](#outgoing-dependencies-tracking) szakasz.</span><span class="sxs-lookup"><span data-stu-id="d849f-205">For more information, see hello [Outgoing dependencies tracking](#outgoing-dependencies-tracking) section.</span></span>

## <a name="long-running-background-tasks"></a><span data-ttu-id="d849f-206">Hosszan futó háttérfeladatok</span><span class="sxs-lookup"><span data-stu-id="d849f-206">Long-running background tasks</span></span>
<span data-ttu-id="d849f-207">Egyes alkalmazások start hosszú futású műveleteket, amelyek a felhasználói kérelmek oka lehet.</span><span class="sxs-lookup"><span data-stu-id="d849f-207">Some applications start long-running operations that might be caused by user requests.</span></span> <span data-ttu-id="d849f-208">Hello nyomkövetés/instrumentation szempontjából nincs eltér a kérelem vagy függőségi instrumentation:</span><span class="sxs-lookup"><span data-stu-id="d849f-208">From hello tracing/instrumentation perspective, it's not different from request or dependency instrumentation:</span></span> 

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
            // Process hello task.
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

<span data-ttu-id="d849f-209">A jelen példában használjuk `telemetryClient.StartOperation` toocreate `RequestTelemetry` és kitöltési hello korrelációs környezetet.</span><span class="sxs-lookup"><span data-stu-id="d849f-209">In this example, we use `telemetryClient.StartOperation` toocreate `RequestTelemetry` and fill hello correlation context.</span></span> <span data-ttu-id="d849f-210">Tegyük fel, a bejövő kérelmeket, amelyek ütemezett hello művelet által létrehozott szülő művelet van.</span><span class="sxs-lookup"><span data-stu-id="d849f-210">Let's say you have a parent operation that was created by incoming requests that scheduled hello operation.</span></span> <span data-ttu-id="d849f-211">Amennyiben a megjelölt `BackgroundTask` induljon el, egy bejövő kérelem azonos aszinkron folyamatábrán hello, azt, hogy a szülő művelet tartozzanak.</span><span class="sxs-lookup"><span data-stu-id="d849f-211">As long as `BackgroundTask` starts in hello same asynchronous control flow as an incoming request, it's correlated with that parent operation.</span></span> <span data-ttu-id="d849f-212">`BackgroundTask`és minden beágyazott telemetriai elem automatikusan, ami miatt, hello kérelem befejeződését követően hello kérelemmel közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="d849f-212">`BackgroundTask` and all nested telemetry items are automatically correlated with hello request that caused it, even after hello request ends.</span></span>

<span data-ttu-id="d849f-213">Hello feladat indításakor hello háttér szálból, amely nem tartalmaz semmilyen műveletet (`Activity`) társított, `BackgroundTask` bármelyik szülő nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d849f-213">When hello task starts from hello background thread that doesn't have any operation (`Activity`) associated with it, `BackgroundTask` doesn't have any parent.</span></span> <span data-ttu-id="d849f-214">Azonban azt is beágyazott műveletek.</span><span class="sxs-lookup"><span data-stu-id="d849f-214">However, it can have nested operations.</span></span> <span data-ttu-id="d849f-215">Az összes telemetriai hello feladat jelentett cikkeket korrelált toohello `RequestTelemetry` létrehozott `BackgroundTask`.</span><span class="sxs-lookup"><span data-stu-id="d849f-215">All telemetry items reported from hello task are correlated toohello `RequestTelemetry` created in `BackgroundTask`.</span></span>

## <a name="outgoing-dependencies-tracking"></a><span data-ttu-id="d849f-216">Kimenő függőségek nyomon követése</span><span class="sxs-lookup"><span data-stu-id="d849f-216">Outgoing dependencies tracking</span></span>
<span data-ttu-id="d849f-217">Nyomon követheti a saját függőségi fajta vagy az Application Insights nem támogatja a műveletet.</span><span class="sxs-lookup"><span data-stu-id="d849f-217">You can track your own dependency kind or an operation that's not supported by Application Insights.</span></span>

<span data-ttu-id="d849f-218">Hello `Enqueue` metódus a Service Bus-üzenetsorba hello vagy hello Storage üzenetsorába ilyen egyéni követési példák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="d849f-218">hello `Enqueue` method in hello Service Bus queue or hello Storage queue can serve as examples for such custom tracking.</span></span>

<span data-ttu-id="d849f-219">általános megközelítés hello egyéni függőségi nyomon követése, hogy:</span><span class="sxs-lookup"><span data-stu-id="d849f-219">hello general approach for custom dependency tracking is to:</span></span>

- <span data-ttu-id="d849f-220">Hello hívás `TelemetryClient.StartOperation` (kiterjesztés) módszer, amelynek változó kitöltése hello `DependencyTelemetry` összefüggések keresésére és néhány egyéb tulajdonság szükséges tulajdonságok (kezdő időpont stamp, időtartama).</span><span class="sxs-lookup"><span data-stu-id="d849f-220">Call hello `TelemetryClient.StartOperation` (extension) method that fills hello `DependencyTelemetry` properties that are needed for correlation and some other properties (start  time stamp, duration).</span></span>
- <span data-ttu-id="d849f-221">Egyéb egyéni tulajdonságainak beállítása hello `DependencyTelemetry`, például hello nevét és minden egyéb környezetre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d849f-221">Set other custom properties on hello `DependencyTelemetry`, such as hello name and any other context you need.</span></span>
- <span data-ttu-id="d849f-222">Ellenőrizze a függőségi hívás, és várjon, amíg az.</span><span class="sxs-lookup"><span data-stu-id="d849f-222">Make a dependency call and wait for it.</span></span>
- <span data-ttu-id="d849f-223">Állítsa le a hello műveletet `StopOperation` amikor befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d849f-223">Stop hello operation with `StopOperation` when it's finished.</span></span>
- <span data-ttu-id="d849f-224">Kivétel kezelése.</span><span class="sxs-lookup"><span data-stu-id="d849f-224">Handle exceptions.</span></span>

<span data-ttu-id="d849f-225">`StopOperation`csak leállítja az elindított hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="d849f-225">`StopOperation` only stops hello operation that was started.</span></span> <span data-ttu-id="d849f-226">Ha hello aktuális futó művelet nem egyezik meg a kívánt toostop, egy hello `StopOperation` nincs semmi hatása.</span><span class="sxs-lookup"><span data-stu-id="d849f-226">If hello current running operation doesn't match hello one you want toostop, `StopOperation` does nothing.</span></span> <span data-ttu-id="d849f-227">Ez a helyzet akkor fordulhat elő, ha több operations hello párhuzamosan ugyanazt a végrehajtási környezet:</span><span class="sxs-lookup"><span data-stu-id="d849f-227">This situation might happen if you start multiple operations in parallel in hello same execution context:</span></span>

```C#
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// This will do nothing and will not report telemetry for hello first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

<span data-ttu-id="d849f-228">Győződjön meg arról, hogy mindig hívja `StartOperation` és a feladat futtatásához a saját környezetben:</span><span class="sxs-lookup"><span data-stu-id="d849f-228">Make sure you always call `StartOperation` and run your task in its own context:</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="d849f-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d849f-229">Next steps</span></span>

- <span data-ttu-id="d849f-230">A hello alapvető [telemetriai korrelációs](application-insights-correlation.md) az Application insights szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d849f-230">Learn hello basics of [telemetry correlation](application-insights-correlation.md) in Application Insights.</span></span>
- <span data-ttu-id="d849f-231">Lásd: hello [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="d849f-231">See hello [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="d849f-232">Egyéni jelentést [eseményeket és metrikákat](app-insights-api-custom-events-metrics.md) tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="d849f-232">Report custom [events and metrics](app-insights-api-custom-events-metrics.md) tooApplication Insights.</span></span>
- <span data-ttu-id="d849f-233">Tekintse meg a standard [konfigurációs](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) környezeti tulajdonságok gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="d849f-233">Check out standard [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) for context properties collection.</span></span>
- <span data-ttu-id="d849f-234">Ellenőrizze a hello [System.Diagnostics.Activity felhasználói útmutató](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee hogyan azt összefüggéseket telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="d849f-234">Check hello [System.Diagnostics.Activity User Guide](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee how we correlate telemetry.</span></span>

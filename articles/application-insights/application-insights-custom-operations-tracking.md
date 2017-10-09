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
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Application Insights .NET SDK-val egyéni műveletek nyomon követése

Az Azure Application Insights SDK-k automatikusan nyomon követése a bejövő HTTP-kérelmek, és meghívja a toodependent szolgáltatások, például a HTTP-kérelmek és az SQL-lekérdezések. Nyomon követését és a kérelmek és a függőségek korrelációs biztosítanak hello teljes alkalmazás válaszidejét és megbízhatóság láthatósága összes mikroszolgáltatások létrehozására, az alkalmazás felhasználó között. 

Nincs alkalmazás mintáról olvashat, amelyek nem tudják támogatni az általános osztály. Az ilyen minták megfelelő figyeléshez szükséges manuális kód instrumentation. Ez a cikk manuális instrumentation egyéni várólista feldolgozása és a hosszú futású háttérfeladatok futtatása például szükség lehet néhány mintázatokat tartalmazza.

Ez a dokumentum hogyan egyéni műveletek tootrack hello Application Insights SDK nyújt útmutatást. Ebben a dokumentációban fontos:

- Az Application Insights .NET (más néven alapvető SDK) verziójának 2.4 +.
- Az Application Insights webes (ASP.NET futó) alkalmazások verziójához 2.4 +.
- Az Application Insights az ASP.NET Core 2.1-es vagy újabb.

## <a name="overview"></a>Áttekintés
Egy művelet egy logikai munkákat az alkalmazások futtatása. Az idő, időtartam, eredmény és olyan környezetben, például a felhasználónevet, a tulajdonságok és az eredmény végrehajtás start neve van. Ha A műveletet kezdeményezett művelet B, majd művelet B be van állítva az A. szülője Egy művelet csak egy szülőhöz lehet, de sok gyermek művelet veheti fel. Műveletek és telemetriai korrelációs további információkért lásd: [Azure Application Insights telemetria korrelációs](application-insights-correlation.md).

Az Application Insights .NET SDK hello, hello művelet le hello absztrakt osztály [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/Extensibility/Implementation/OperationTelemetry.cs) és a leszármazottai [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/RequestTelemetry.cs) és [DependencyTelemetry ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Core/Managed/Shared/DataContracts/DependencyTelemetry.cs).

## <a name="incoming-operations-tracking"></a>Bejövő műveletek nyomon követése 
hello Application Insights webes SDK automatikusan gyűjti az ASP.NET futtatni egy IIS-feldolgozási folyamat összes ASP.NET Core alkalmazásokat és a HTTP-kérelmekre. Nincsenek megoldások Közösség által támogatott más platformok és -keretrendszerek számára. Azonban hello alkalmazás bármelyik hello standard vagy a Közösség által támogatott megoldások nem támogatott, ha Ön állíthatnak be azt manuálisan.

Egy másik, amelyhez az egyéni nyomkövetési: hello munkavégző, amely megkapja a hello várólistában lévő elemeket. Egyes várólisták hello toothis várólista követhető nyomon a függőség beállításához üzenet hívja tooadd. Azonban hello magas szintű műveletet leíró üzenet feldolgozása nem automatikusan összegyűjtött.

Nézzük meg, hogyan azt nyomon az ilyen műveleteket.

Magas szinten hello feladata toocreate `RequestTelemetry` és ismert tulajdonságainak beállítása. Hello művelet befejezése után hello telemetriai nyomon követéséhez. hello a következő példa azt mutatja be ezt a feladatot.

### <a name="http-request-in-owin-self-hosted-app"></a>Önálló üzemeltetett alkalmazás Owin HTTP-kérelem
Ebben a példában, azt követve hello [HTTP protokoll a korrelációs](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Nem ismertetett tooreceive fejlécek kell látnia.

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

hello korrelációs HTTP protokollt is deklarál hello `Correlation-Context` fejléc. Azonban ki van hagyva itt az egyszerűség érdekében.

## <a name="queue-instrumentation"></a>Várólista instrumentation
HTTP-kommunikációhoz létrehoztunk Önnek egy protokoll toopass korrelációs részleteit. Az egyes üzenetsorok protokollok adja át további metaadatokat üdvözlőüzenetére együtt, és nem lehet másokkal.

### <a name="service-bus-queue"></a>Service Bus-üzenetsorba
Az hello Azure [Service Bus-üzenetsorba](../service-bus-messaging/index.md), átadhatók egy tulajdonságcsomagot üdvözlőüzenetére együtt. Használjuk, toopass hello korrelációs azonosítója.

Service Bus-üzenetsorba hello TCP-alapú technológiát használ. Az Application Insights nem automatikusan nyomon követheti a várólista műveleteket, így manuálisan azt nyomon őket. hello created művelet egy leküldéses stílusú API-t, és nem tootrack el azt.

#### <a name="enqueue"></a>Sorba helyezni

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

#### <a name="process"></a>Folyamat
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

### <a name="azure-storage-queue"></a>Az Azure Storage üzenetsorába
a következő példa azt mutatja meg hogyan hello tootrack hello [Azure Storage üzenetsorába](../storage/queues/storage-dotnet-how-to-use-queues.md) műveletek és összefüggésbe telemetriai hello készítő, hello fogyasztói és az Azure Storage között. 

hello tárolási várólistának egy HTTP API-t. Minden hívások toohello várólista kötetblokkok hello Application Insights függőségi adatgyűjtő a HTTP-kérelmekre.
Győződjön meg arról, hogy `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` a `applicationInsights.config`. Ha nem rendelkezik, adja hozzá a programozott módon [szűrést és a hello Azure Application Insights SDK előfeldolgozása](app-insights-api-filtering-sampling.md).

Ha manuálisan adja meg az Application Insights, győződjön meg arról, létrehozása és inicializálása `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` mint:
 
``` C#
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection toosome domains by adding it toohello excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on hello Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget toodispose of hello module during application shutdown.
```

Érdemes azt is toocorrelate hello Application Insights Műveletazonosító hello tároló kérelemben azonosítóval. Hogyan tooset és a tárolási get kérelmek ügyfél és a kiszolgáló Kérelemazonosító információkért lásd: [figyelése, diagnosztizálása és elhárítása az Azure Storage](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>Sorba helyezni
Tárolási sorok hello HTTP API támogatja, mert hello várakozási sorral rendelkező összes művelet automatikusan követi az Application Insights. Sok esetben elegendő a instrumentation kell lennie. Azonban a gyártó nyomkövetések hello fogyasztói oldalán hívásláncainak toocorrelate, át kell adnia néhány korrelációs környezetben hasonlóképpen toohow végezzük azt a HTTP protokoll a korrelációs hello. 

Ebben a példában a Microsoft hello választható nyomon `Enqueue` műveletet. A következőket teheti:

 - **Összefüggéseket találni az újrapróbálkozások között (ha van ilyen)**: minden rendelkeznek, amely hello egy közös szülő `Enqueue` műveletet. Ellenkező esetben ezek még nyomon hello bejövő kérelem gyermekeként. Ha több logikai kérelmek toohello várólista, a hívást eredményezett újrapróbálkozások nehéz toofind lehet.
 - **A tárolási naplófájljai összefüggéseket (ha szükséges)**: azok az Application Insights telemetria most tartozzanak.

Hello `Enqueue` művelet egy szülő műveletet (például egy bejövő HTTP-kérelem) hello gyermeke. hello HTTP függőségi hívás hello hello gyermeke `Enqueue` művelet és hello unoka hello bejövő kérelem:

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

telemetriai adatok mennyisége tooreduce hello az alkalmazás jelentéseket, vagy ha nem szeretné, hogy tootrack hello `Enqueue` más okokból használata hello művelet `Activity` közvetlenül API:

- Hozzon létre (és indítása) egy új `Activity` helyett hello Application Insights művelet elindítása. Ezt megteheti *nem* kell tooassign hello művelet nevén kívül a kívánt tulajdonságokat.
- Szerializálható `yourActivity.Id` hello üzenetadatokat ahelyett, hogy a `operation.Telemetry.Id`. Is `Activity.Current.Id`.


#### <a name="dequeue"></a>Created
Hasonlóképpen túl`Enqueue`, egy tényleges HTTP toohello tároló várólistája automatikusan követi nyomon az Application Insights. Azonban hello `Enqueue` művelet feltehetően hello szülő környezetben, például egy bejövő kérelem környezete zajlik. Application Insights SDK-k automatikusan egyeztetéséhez ilyen művelet (és a HTTP-rész) hello szülő kérelemmel és egyéb telemetriai jelentett hello azonos hatókör.

Hello `Dequeue` legbonyolultabb művelet. Application Insights SDK hello automatikusan nyomon követi a HTTP-kérelmekre. Hello korrelációs környezetben azonban azt nem ismert amíg üdvözlőüzenetére elemzi. Már nem lehetséges toocorrelate hello HTTP tooget hello kérelemüzenet hello többi hello telemetriai adatokat.

Sok esetben hasznos toocorrelate hello HTTP kérelem toohello várólista más nyomkövetések is lehet. hello következő példa bemutatja, hogyan toodo azt:

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

#### <a name="process"></a>Folyamat

A következő példa hello, azt követni a bejövő üzenet módon hasonlóképpen toohow azt nyomon követni a bejövő HTTP kérelem:

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

Hasonlóképpen a többi üzenetsor-műveletet tagolva is. Egy betekintés művelet egy dequeue művelet, hasonló módon lesznek tagolva. Üzenetsor-kezelési műveletet tagolása nem szükséges. Az Application Insights nyomon követi a műveletek, például HTTP, és a legtöbb esetben elegendő.

Amikor állíthatnak üzenet törlése, ellenőrizze, hogy beállította hello művelet (korrelációs) azonosítót. Másik lehetőségként használhatja a hello `Activity` API. Nem szükséges a tooset művelet azonosítók hello telemetriai elemek Application Insights minderre, mert:

- Hozzon létre egy új `Activity` után egy elem hello üzenetsorból.
- Használjon `Activity.SetParentId(message.ParentId)` toocorrelate fogyasztói, a gyártó naplókat.
- Indítsa el a hello `Activity`.
- Track created, feldolgozása és törlési műveletek használatával `Start/StopOperation` segítő. Ehhez a hello azonos aszinkron szabályozása folyamata (a végrehajtási környezet). Ezzel a módszerrel azok még korrelált megfelelően.
- Állítsa le hello `Activity`.
- Használjon `Start/StopOperation`, vagy hívja az `Track` telemetriai manuálisan.

### <a name="batch-processing"></a>Kötegfeldolgozási
Az egyes üzenetsorok egy kérelem több üzenetet is created. Ilyen üzenetek feldolgozása feltételezhetően független, és különböző logikai műveletek toohello tartozik. Ebben az esetben nincs lehetséges toocorrelate hello `Dequeue` művelet tooparticular üzenet feldolgozása.

Minden üzenet fel kell dolgozni a saját aszinkron vezérlési folyamatában. További információkért lásd: hello [követési kimenő függőségek](#outgoing-dependencies-tracking) szakasz.

## <a name="long-running-background-tasks"></a>Hosszan futó háttérfeladatok
Egyes alkalmazások start hosszú futású műveleteket, amelyek a felhasználói kérelmek oka lehet. Hello nyomkövetés/instrumentation szempontjából nincs eltér a kérelem vagy függőségi instrumentation: 

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

A jelen példában használjuk `telemetryClient.StartOperation` toocreate `RequestTelemetry` és kitöltési hello korrelációs környezetet. Tegyük fel, a bejövő kérelmeket, amelyek ütemezett hello művelet által létrehozott szülő művelet van. Amennyiben a megjelölt `BackgroundTask` induljon el, egy bejövő kérelem azonos aszinkron folyamatábrán hello, azt, hogy a szülő művelet tartozzanak. `BackgroundTask`és minden beágyazott telemetriai elem automatikusan, ami miatt, hello kérelem befejeződését követően hello kérelemmel közötti kapcsolatot.

Hello feladat indításakor hello háttér szálból, amely nem tartalmaz semmilyen műveletet (`Activity`) társított, `BackgroundTask` bármelyik szülő nem rendelkezik. Azonban azt is beágyazott műveletek. Az összes telemetriai hello feladat jelentett cikkeket korrelált toohello `RequestTelemetry` létrehozott `BackgroundTask`.

## <a name="outgoing-dependencies-tracking"></a>Kimenő függőségek nyomon követése
Nyomon követheti a saját függőségi fajta vagy az Application Insights nem támogatja a műveletet.

Hello `Enqueue` metódus a Service Bus-üzenetsorba hello vagy hello Storage üzenetsorába ilyen egyéni követési példák lehetnek.

általános megközelítés hello egyéni függőségi nyomon követése, hogy:

- Hello hívás `TelemetryClient.StartOperation` (kiterjesztés) módszer, amelynek változó kitöltése hello `DependencyTelemetry` összefüggések keresésére és néhány egyéb tulajdonság szükséges tulajdonságok (kezdő időpont stamp, időtartama).
- Egyéb egyéni tulajdonságainak beállítása hello `DependencyTelemetry`, például hello nevét és minden egyéb környezetre van szüksége.
- Ellenőrizze a függőségi hívás, és várjon, amíg az.
- Állítsa le a hello műveletet `StopOperation` amikor befejeződött.
- Kivétel kezelése.

`StopOperation`csak leállítja az elindított hello műveletet. Ha hello aktuális futó művelet nem egyezik meg a kívánt toostop, egy hello `StopOperation` nincs semmi hatása. Ez a helyzet akkor fordulhat elő, ha több operations hello párhuzamosan ugyanazt a végrehajtási környezet:

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

Győződjön meg arról, hogy mindig hívja `StartOperation` és a feladat futtatásához a saját környezetben:
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

## <a name="next-steps"></a>Következő lépések

- A hello alapvető [telemetriai korrelációs](application-insights-correlation.md) az Application insights szolgáltatással.
- Lásd: hello [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.
- Egyéni jelentést [eseményeket és metrikákat](app-insights-api-custom-events-metrics.md) tooApplication Insights.
- Tekintse meg a standard [konfigurációs](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) környezeti tulajdonságok gyűjtemény.
- Ellenőrizze a hello [System.Diagnostics.Activity felhasználói útmutató](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) toosee hogyan azt összefüggéseket telemetriai adatokat.

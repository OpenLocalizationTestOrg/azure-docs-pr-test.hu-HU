---
title: "aaaUsing .NET osztály az Azure Functions szalagtárak |} Microsoft Docs"
description: "Ismerje meg, hogyan tooauthor alkalmazás számára az Azure Functions használata"
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Azure functions, Funkciók, Eseményfeldolgozási, dinamikus számítási kiszolgáló nélküli architektúrája"
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/09/2017
ms.author: donnam
ms.openlocfilehash: 4e0fd954b554006ba1d8ecc47403a9fb1c67c3b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a>Alkalmazás az Azure Functions használatával

Tooscript fájlok, az Azure Functions támogatja továbbá egy osztálytár közzététele, egy vagy több funkciók hello implementációja. Azt javasoljuk, hogy használja-e hello [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).

## <a name="prerequisites"></a>Előfeltételek 

Ez a cikk a következő előfeltételek hello rendelkezik:

- [A Visual Studio 2017 15.3 előzetes](https://www.visualstudio.com/vs/preview/). Hello munkaterhelések telepítése **ASP.NET és a webes fejlesztési** és **Azure fejlesztési**.
- [Az Azure-függvény Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a>Functions hordozhatóosztálytár-projektjének

A Visual Studio eszközből az Azure Functions új projekt létrehozása. hello új projektsablon hello fájlokat hozza létre *host.json* és *local.settings.json*. Is [host.json az Azure Functions futásidejű beállításokat](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json). 

hello fájl *local.settings.json* tárolja Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításait. toolearn struktúrájától kapcsolatos további információkért lásd: [kódot és az Azure functions helyi tesztelése](functions-run-local.md#local-settings).

### <a name="functionname-attribute"></a>Függvénynév attribútum

hello attribútum [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) jelöli meg a metódus egy függvény belépési pontként. Pontosan egy eseményindító együtt kell használni, és 0 vagy több bemeneti és kimeneti kötéseket.

### <a name="conversion-toofunctionjson"></a>Átalakítás toofunction.json

Ha az Azure Functions projektek épül, akkor fájlt hoz létre `function.json` által meghatározott hello függvény névnek megfelelő hello könyvtárban `[FunctionName]`. Eseményindítók és kötések és pontok toohello szerelvény projektfájlt határoz meg.

Hello NuGet-csomagja végzi el az átalakításhoz [Microsoft\.NET\.Sdk\.funkciók](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). hello forrás álljon rendelkezésre a GitHub-tárház hello [azure\-funkciók\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="triggers-and-bindings"></a>Eseményindítók és kötések

hello következő táblázatban hello eseményindítók és kötések, az Azure Functions hordozhatóosztálytár-projektjének elérhető. Minden attribútumokat hello névtérben `Microsoft.Azure.WebJobs`.

| Kötelező | Attribútum | NuGet-csomag |
|------   | ------    | ------        |
| [A BLOB storage eseményindító, bemeneti, kimeneti](#blob-storage) | [BlobAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | [A blob storage] |
| [A cosmos DB bemeneti és kimeneti kötése](#cosmos-db) | [DocumentDBAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] | 
| [Event Hubs eseményindító és a kimeneti](#event-hub) | [EventHubTriggerAttribute], [EventHubAttribute] | [Microsoft.Azure.WebJobs.ServiceBus] |
| [Külső fájl bemeneti és kimeneti](#api-hub) | [ApiHubFileAttribute] | [Microsoft.Azure.WebJobs.Extensions.ApiHub] |
| [A HTTP és a webhook eseményindító](#http) | [HttpTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.Http] |
| [Mobile Apps bemeneti és kimeneti](#mobile-apps) | [MobileTableAttribute] | [Microsoft.Azure.WebJobs.Extensions.MobileApps] | 
| [Notification Hubs kimeneti](#nh) | [NotificationHubAttribute] | [Microsoft.Azure.WebJobs.Extensions.NotificationHubs] | 
| [Várólista tárolási eseményindító és a kimeneti](#queue) | [QueueAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [SendGrid kimeneti](#sendgrid) | [SendGridAttribute] | [Microsoft.Azure.WebJobs.Extensions.SendGrid] | 
| [A Service Bus eseményindító és a kimeneti](#service-bus) | [ServiceBusAttribute], [ServiceBusAccountAttribute] | [Microsoft.Azure.WebJobs.ServiceBus]
| [TABLE storage bemeneti és kimeneti](#table) | [TableAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [Időzítő eseményindító](#timer) | [TimerTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions] | 
| [Twilio-kimenet](#twilio) | [TwilioSmsAttribute] | [Microsoft.Azure.WebJobs.Extensions.Twilio] | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a>A BLOB storage eseményindító, bemeneti és kimeneti kötések

Az Azure Functions támogatja eseményindító, adjon meg, és kimeneti Azure Blob Storage tárolóban kötéseit. A kötés kifejezések és a metaadatok további információkért lásd: [Azure Functions Blob storage kötések](functions-bindings-storage-blob.md).

Egy blob eseményindító van definiálva hello `[BlobTrigger]` attribútum. Használhatja a hello attribútum `[StorageAccount]` toodefine hello tárfiók egy teljes vagy a osztály által használt.

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Hello segítségével meghatározott BLOB bemeneti és kimeneti `[Blob]` attribútumot, jelszavat rendelkező egy `FileAccess` paraméter jelző olvasására vagy írására. egy blob eseményindító a következő példában hello és blob kimeneti kötése.

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-input-and-output-bindings"></a>Cosmos DB bemeneti és kimeneti kötések

Az Azure Functions támogatja bemeneti és kimeneti Cosmos DB kötései. toolearn hello szolgáltatások hello Cosmos DB kötés, bővebben lásd: [Azure Functions Cosmos DB kötések](functions-bindings-documentdb.md).

toobind tooa Cosmos DB dokumentum hello attribútum használata a `[DocumentDB]` hello NuGet-csomagot a [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. a következő példa hello várólista eseményindító és a DocumentDB API kimeneti kötése van:

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", ConnectionStringSetting = "DocDBConnection")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a>Event Hubs eseményindító és a kimeneti

Az Azure Functions támogatja indítható el, és az Event Hubs kötései kimeneti. További információkért lásd: [Azure Functions Eseményközpont kötések](functions-bindings-event-hubs.md).

hello típusok `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` és `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.ServiceBus]. 

hello következő példában egy Eseményközpontba eseményindító:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

hello következő például rendelkezik egy Eseményközpontot, kimeneti hello metódus visszatérési értéke hello kimenetként használata:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a>Külső fájl bemeneti és kimeneti

Az Azure Functions támogatja eseményindító, bemeneti és kimeneti kötések külső fájlokat, például a Google-meghajtó, a Dropbox és a onedrive vállalati verzió. több, lásd: toolearn [Azure Functions külső fájl kötések](functions-bindings-external-file.md). attribútumok hello `[ExternalFileTrigger]` és `[ExternalFile]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.ApiHub].

a következő példa C# hello mutatja be, külső fájlban bemeneti és kimeneti kötése. hello kódot másolja a bemeneti fájl toohello kimeneti fájl hello.

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a>HTTP és webhookok

Használjon hello `HttpTrigger` attribútum toodefine HTTP eseményindító vagy webhook. Ez az attribútum van megadva hello NuGet-csomag [Microsoft.Azure.WebJobs.Extensions.Http]. Hello jogosultsági szintet, a webhook típusa, az útvonal és a módszerek személyre is szabhatja. hello alábbi példa meghatározza a névtelen hitelesítés egy HTTP-eseményindítóval és _genericJson_ webhook típusa.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a>Mobile Apps bemeneti és kimeneti

Azure Functions támogatja bemeneti és kimeneti Mobile Apps kötéseit. több, lásd: toolearn [Azure Functions Mobile Apps kötések](functions-bindings-mobile-apps.md). hello attribútum `[MobileTable]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.MobileApps].

hello következő példa bemutatja a Mobile Apps kimeneti kötése:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a>Notification Hubs kimeneti

Az Azure Functions a Notification Hubs egy kimeneti kötése támogatja. több, lásd: toolearn [Azure Functions Notification Hub kimeneti kötése](functions-bindings-notification-hubs.md). hello attribútum `[NotificationHub]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a>Várólista tárolási eseményindító és a kimeneti

Az Azure Functions támogatja aktiválhatja, és kimeneti Azure várólisták kötései. További információkért lásd: [Azure Functions a Queue Storage kötések](functions-bindings-storage-queue.md).

hello következő példa bemutatja, hogyan toouse hello függvény visszatérési-típus egy várólista kimeneti kötelező, segítségével hello `[Queue]` attribútum. egy várólista eseményindító toodefine hello használata `[QueueTrigger]` attribútum.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }

    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```

<a name="sendgrid"></a>

### <a name="sendgrid-output"></a>SendGrid kimeneti

Az Azure Functions támogatja a SendGrid kimeneti kötése programozott módon küld e-mailt. több, lásd: toolearn [Azure Functions SendGrid kötések](functions-bindings-sendgrid.md).

hello attribútum `[SendGrid]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.SendGrid].

hello az alábbiakban látható példa Service Bus várólista eseményindítót használ, és a SendGrid kimeneti kötés használatával `SendGridMessage`:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string too{ get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a>A Service Bus eseményindító és a kimeneti

Az Azure Functions támogatja aktiválhatja és Service Bus-üzenetsorok és témakörök kimeneti. Kötések konfigurálásával kapcsolatos további információkért lásd: [Azure Functions Service Bus kötések](functions-bindings-service-bus.md).

attribútumok hello `[ServiceBusTrigger]` és `[ServiceBus]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.ServiceBus]. 

hello az alábbiakban látható egy példa egy Service Bus várólista eseményindító:

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

hello az alábbiakban látható egy példa egy Service Bus kimeneti kötése, hello metódus visszatérési típusának hello kimenetként használata:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a>TABLE storage bemeneti és kimeneti

Az Azure Functions támogatja bemeneti és kimeneti Azure Table storage kötései. több, lásd: toolearn [Azure Functions Table storage kötések](functions-bindings-storage-table.md).

hello következő példa egy olyan osztály, amelynek két funkciót, a table storage kimeneti és a bemeneti kötések, amely tartalmazza. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use hello metadata parameter "queueTrigger" toobind hello queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a>Időzítő eseményindító

Az Azure Functions lehetővé teszi, hogy a kódra a függvény a meghatározott ütemezés szerint időzítő eseményindító-kötéssel rendelkezik. toolearn hello szolgáltatások hello kötés, bővebben lásd: [kód végrehajtását az Azure Functions](functions-bindings-timer.md).

Hello fogyasztás terv, ütemezés a definiálhat egy [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression). Használata egy App Service-csomag, a TimeSpan karakterlánc is használható. 

a következő példa hello meghatározza, hogy egy időzítő indítófeltételt 5 percenként futtató:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a>Twilio-kimenet

Az Azure Functions támogatja Twilio kimeneti kötések tooenable a funkciók toosend SMS szöveges üzenetek. több, lásd: toolearn [az SMS küldése az Azure Functions használatával hello Twilio kimeneti kötése](functions-bindings-twilio.md). 

hello attribútum `[TwilioSms]` hello csomagban van definiálva [Microsoft.Azure.WebJobs.Extensions.Twilio].

hello alábbi C# példában várólista eseményindító és a Twilio kimeneti kötése:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        too= order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a>Következő lépések

További információ az Azure Functions használatával a C# scripting, talál [Azure Functions C\# fejlesztői leírás parancsfájl](functions-reference-csharp.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4
[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1


<!-- Links toosource --> 
[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs
[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs
[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs
[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs
[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs 
[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs
[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs
[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs
[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs
[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs
[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs
[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs
[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs
[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs
[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs
[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs

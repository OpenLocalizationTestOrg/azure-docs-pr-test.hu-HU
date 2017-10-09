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
# <a name="using-net-class-libraries-with-azure-functions"></a><span data-ttu-id="639c1-104">Alkalmazás az Azure Functions használatával</span><span class="sxs-lookup"><span data-stu-id="639c1-104">Using .NET class libraries with Azure Functions</span></span>

<span data-ttu-id="639c1-105">Tooscript fájlok, az Azure Functions támogatja továbbá egy osztálytár közzététele, egy vagy több funkciók hello implementációja.</span><span class="sxs-lookup"><span data-stu-id="639c1-105">In addition tooscript files, Azure Functions supports publishing a class library as hello implementation for one or more functions.</span></span> <span data-ttu-id="639c1-106">Azt javasoljuk, hogy használja-e hello [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="639c1-106">We recommend that you use hello [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="639c1-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="639c1-107">Prerequisites</span></span> 

<span data-ttu-id="639c1-108">Ez a cikk a következő előfeltételek hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="639c1-108">This article has hello following prerequisites:</span></span>

- <span data-ttu-id="639c1-109">[A Visual Studio 2017 15.3 előzetes](https://www.visualstudio.com/vs/preview/).</span><span class="sxs-lookup"><span data-stu-id="639c1-109">[Visual Studio 2017 15.3 Preview](https://www.visualstudio.com/vs/preview/).</span></span> <span data-ttu-id="639c1-110">Hello munkaterhelések telepítése **ASP.NET és a webes fejlesztési** és **Azure fejlesztési**.</span><span class="sxs-lookup"><span data-stu-id="639c1-110">Install hello workloads **ASP.NET and web development** and **Azure development**.</span></span>
- [<span data-ttu-id="639c1-111">Az Azure-függvény Tools for Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="639c1-111">Azure Function Tools for Visual Studio 2017</span></span>](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a><span data-ttu-id="639c1-112">Functions hordozhatóosztálytár-projektjének</span><span class="sxs-lookup"><span data-stu-id="639c1-112">Functions class library project</span></span>

<span data-ttu-id="639c1-113">A Visual Studio eszközből az Azure Functions új projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="639c1-113">From Visual Studio, create a new Azure Functions project.</span></span> <span data-ttu-id="639c1-114">hello új projektsablon hello fájlokat hozza létre *host.json* és *local.settings.json*.</span><span class="sxs-lookup"><span data-stu-id="639c1-114">hello new project template creates hello files *host.json* and *local.settings.json*.</span></span> <span data-ttu-id="639c1-115">Is [host.json az Azure Functions futásidejű beállításokat](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="639c1-115">You can [customize Azure Functions runtime settings in host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span> 

<span data-ttu-id="639c1-116">hello fájl *local.settings.json* tárolja Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításait.</span><span class="sxs-lookup"><span data-stu-id="639c1-116">hello file *local.settings.json* stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="639c1-117">toolearn struktúrájától kapcsolatos további információkért lásd: [kódot és az Azure functions helyi tesztelése](functions-run-local.md#local-settings).</span><span class="sxs-lookup"><span data-stu-id="639c1-117">toolearn more about its structure, see [Code and test Azure functions locally](functions-run-local.md#local-settings).</span></span>

### <a name="functionname-attribute"></a><span data-ttu-id="639c1-118">Függvénynév attribútum</span><span class="sxs-lookup"><span data-stu-id="639c1-118">FunctionName attribute</span></span>

<span data-ttu-id="639c1-119">hello attribútum [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) jelöli meg a metódus egy függvény belépési pontként.</span><span class="sxs-lookup"><span data-stu-id="639c1-119">hello attribute [`FunctionNameAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) marks a method as a function entry point.</span></span> <span data-ttu-id="639c1-120">Pontosan egy eseményindító együtt kell használni, és 0 vagy több bemeneti és kimeneti kötéseket.</span><span class="sxs-lookup"><span data-stu-id="639c1-120">It must be used with exactly one trigger and 0 or more input and output bindings.</span></span>

### <a name="conversion-toofunctionjson"></a><span data-ttu-id="639c1-121">Átalakítás toofunction.json</span><span class="sxs-lookup"><span data-stu-id="639c1-121">Conversion toofunction.json</span></span>

<span data-ttu-id="639c1-122">Ha az Azure Functions projektek épül, akkor fájlt hoz létre `function.json` által meghatározott hello függvény névnek megfelelő hello könyvtárban `[FunctionName]`.</span><span class="sxs-lookup"><span data-stu-id="639c1-122">When an Azure Functions project is built, it produces a file `function.json` in hello directory matching hello function name defined by `[FunctionName]`.</span></span> <span data-ttu-id="639c1-123">Eseményindítók és kötések és pontok toohello szerelvény projektfájlt határoz meg.</span><span class="sxs-lookup"><span data-stu-id="639c1-123">It specifies triggers and bindings and points toohello project assembly file.</span></span>

<span data-ttu-id="639c1-124">Hello NuGet-csomagja végzi el az átalakításhoz [Microsoft\.NET\.Sdk\.funkciók](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span><span class="sxs-lookup"><span data-stu-id="639c1-124">This conversion is performed by hello NuGet package [Microsoft\.NET\.Sdk\.Functions](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span></span> <span data-ttu-id="639c1-125">hello forrás álljon rendelkezésre a GitHub-tárház hello [azure\-funkciók\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span><span class="sxs-lookup"><span data-stu-id="639c1-125">hello source is available in hello GitHub repo [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span></span>

## <a name="triggers-and-bindings"></a><span data-ttu-id="639c1-126">Eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="639c1-126">Triggers and bindings</span></span>

<span data-ttu-id="639c1-127">hello következő táblázatban hello eseményindítók és kötések, az Azure Functions hordozhatóosztálytár-projektjének elérhető.</span><span class="sxs-lookup"><span data-stu-id="639c1-127">hello following table lists hello triggers and bindings that are available in an Azure Functions class library project.</span></span> <span data-ttu-id="639c1-128">Minden attribútumokat hello névtérben `Microsoft.Azure.WebJobs`.</span><span class="sxs-lookup"><span data-stu-id="639c1-128">All attributes are in hello namespace `Microsoft.Azure.WebJobs`.</span></span>

| <span data-ttu-id="639c1-129">Kötelező</span><span class="sxs-lookup"><span data-stu-id="639c1-129">Binding</span></span> | <span data-ttu-id="639c1-130">Attribútum</span><span class="sxs-lookup"><span data-stu-id="639c1-130">Attribute</span></span> | <span data-ttu-id="639c1-131">NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="639c1-131">NuGet package</span></span> |
|------   | ------    | ------        |
| [<span data-ttu-id="639c1-132">A BLOB storage eseményindító, bemeneti, kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-132">Blob storage trigger, input, output</span></span>](#blob-storage) | <span data-ttu-id="639c1-133">[BlobAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-133">[BlobAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="639c1-134">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="639c1-134">[Microsoft.Azure.WebJobs]</span></span> | <span data-ttu-id="639c1-135">[A blob storage]</span><span class="sxs-lookup"><span data-stu-id="639c1-135">[Blob storage]</span></span> |
| [<span data-ttu-id="639c1-136">A cosmos DB bemeneti és kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="639c1-136">Cosmos DB input and output binding</span></span>](#cosmos-db) | <span data-ttu-id="639c1-137">[DocumentDBAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-137">[DocumentDBAttribute]</span></span> | <span data-ttu-id="639c1-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span><span class="sxs-lookup"><span data-stu-id="639c1-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span></span> | 
| [<span data-ttu-id="639c1-139">Event Hubs eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-139">Event Hubs trigger and output</span></span>](#event-hub) | <span data-ttu-id="639c1-140">[EventHubTriggerAttribute], [EventHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-140">[EventHubTriggerAttribute], [EventHubAttribute]</span></span> | <span data-ttu-id="639c1-141">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="639c1-141">[Microsoft.Azure.WebJobs.ServiceBus]</span></span> |
| [<span data-ttu-id="639c1-142">Külső fájl bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-142">External file input and output</span></span>](#api-hub) | <span data-ttu-id="639c1-143">[ApiHubFileAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-143">[ApiHubFileAttribute]</span></span> | <span data-ttu-id="639c1-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span><span class="sxs-lookup"><span data-stu-id="639c1-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span></span> |
| [<span data-ttu-id="639c1-145">A HTTP és a webhook eseményindító</span><span class="sxs-lookup"><span data-stu-id="639c1-145">HTTP and webhook trigger</span></span>](#http) | <span data-ttu-id="639c1-146">[HttpTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-146">[HttpTriggerAttribute]</span></span> | <span data-ttu-id="639c1-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span><span class="sxs-lookup"><span data-stu-id="639c1-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span></span> |
| [<span data-ttu-id="639c1-148">Mobile Apps bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-148">Mobile Apps input and output</span></span>](#mobile-apps) | <span data-ttu-id="639c1-149">[MobileTableAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-149">[MobileTableAttribute]</span></span> | <span data-ttu-id="639c1-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span><span class="sxs-lookup"><span data-stu-id="639c1-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span></span> | 
| [<span data-ttu-id="639c1-151">Notification Hubs kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-151">Notification Hubs output</span></span>](#nh) | <span data-ttu-id="639c1-152">[NotificationHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-152">[NotificationHubAttribute]</span></span> | <span data-ttu-id="639c1-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span><span class="sxs-lookup"><span data-stu-id="639c1-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span></span> | 
| [<span data-ttu-id="639c1-154">Várólista tárolási eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-154">Queue storage trigger and output</span></span>](#queue) | <span data-ttu-id="639c1-155">[QueueAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-155">[QueueAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="639c1-156">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="639c1-156">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="639c1-157">SendGrid kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-157">SendGrid output</span></span>](#sendgrid) | <span data-ttu-id="639c1-158">[SendGridAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-158">[SendGridAttribute]</span></span> | <span data-ttu-id="639c1-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span><span class="sxs-lookup"><span data-stu-id="639c1-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span></span> | 
| [<span data-ttu-id="639c1-160">A Service Bus eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-160">Service Bus trigger and output</span></span>](#service-bus) | <span data-ttu-id="639c1-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span></span> | <span data-ttu-id="639c1-162">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="639c1-162">[Microsoft.Azure.WebJobs.ServiceBus]</span></span>
| [<span data-ttu-id="639c1-163">TABLE storage bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-163">Table storage input and output</span></span>](#table) | <span data-ttu-id="639c1-164">[TableAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-164">[TableAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="639c1-165">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="639c1-165">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="639c1-166">Időzítő eseményindító</span><span class="sxs-lookup"><span data-stu-id="639c1-166">Timer trigger</span></span>](#timer) | <span data-ttu-id="639c1-167">[TimerTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-167">[TimerTriggerAttribute]</span></span> | <span data-ttu-id="639c1-168">[Microsoft.Azure.WebJobs.Extensions]</span><span class="sxs-lookup"><span data-stu-id="639c1-168">[Microsoft.Azure.WebJobs.Extensions]</span></span> | 
| [<span data-ttu-id="639c1-169">Twilio-kimenet</span><span class="sxs-lookup"><span data-stu-id="639c1-169">Twilio output</span></span>](#twilio) | <span data-ttu-id="639c1-170">[TwilioSmsAttribute]</span><span class="sxs-lookup"><span data-stu-id="639c1-170">[TwilioSmsAttribute]</span></span> | <span data-ttu-id="639c1-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span><span class="sxs-lookup"><span data-stu-id="639c1-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span></span> | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a><span data-ttu-id="639c1-172">A BLOB storage eseményindító, bemeneti és kimeneti kötések</span><span class="sxs-lookup"><span data-stu-id="639c1-172">Blob storage trigger, input, and output bindings</span></span>

<span data-ttu-id="639c1-173">Az Azure Functions támogatja eseményindító, adjon meg, és kimeneti Azure Blob Storage tárolóban kötéseit.</span><span class="sxs-lookup"><span data-stu-id="639c1-173">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="639c1-174">A kötés kifejezések és a metaadatok további információkért lásd: [Azure Functions Blob storage kötések](functions-bindings-storage-blob.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-174">For more information on binding expressions and metadata, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>

<span data-ttu-id="639c1-175">Egy blob eseményindító van definiálva hello `[BlobTrigger]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="639c1-175">A blob trigger is defined with hello `[BlobTrigger]` attribute.</span></span> <span data-ttu-id="639c1-176">Használhatja a hello attribútum `[StorageAccount]` toodefine hello tárfiók egy teljes vagy a osztály által használt.</span><span class="sxs-lookup"><span data-stu-id="639c1-176">You can use hello attribute `[StorageAccount]` toodefine hello storage account that is used by an entire function or class.</span></span>

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

<span data-ttu-id="639c1-177">Hello segítségével meghatározott BLOB bemeneti és kimeneti `[Blob]` attribútumot, jelszavat rendelkező egy `FileAccess` paraméter jelző olvasására vagy írására.</span><span class="sxs-lookup"><span data-stu-id="639c1-177">Blob input and output are defined using hello `[Blob]` attribute, along with a `FileAccess` parameter indicating read or write.</span></span> <span data-ttu-id="639c1-178">egy blob eseményindító a következő példában hello és blob kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="639c1-178">hello following example uses a blob trigger and blob output binding.</span></span>

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

### <a name="cosmos-db-input-and-output-bindings"></a><span data-ttu-id="639c1-179">Cosmos DB bemeneti és kimeneti kötések</span><span class="sxs-lookup"><span data-stu-id="639c1-179">Cosmos DB input and output bindings</span></span>

<span data-ttu-id="639c1-180">Az Azure Functions támogatja bemeneti és kimeneti Cosmos DB kötései.</span><span class="sxs-lookup"><span data-stu-id="639c1-180">Azure Functions supports input and output bindings for Cosmos DB.</span></span> <span data-ttu-id="639c1-181">toolearn hello szolgáltatások hello Cosmos DB kötés, bővebben lásd: [Azure Functions Cosmos DB kötések](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-181">toolearn more about hello features of hello Cosmos DB binding, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>

<span data-ttu-id="639c1-182">toobind tooa Cosmos DB dokumentum hello attribútum használata a `[DocumentDB]` hello NuGet-csomagot a [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span><span class="sxs-lookup"><span data-stu-id="639c1-182">toobind tooa Cosmos DB document, use hello attribute `[DocumentDB]` in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span></span> <span data-ttu-id="639c1-183">a következő példa hello várólista eseményindító és a DocumentDB API kimeneti kötése van:</span><span class="sxs-lookup"><span data-stu-id="639c1-183">hello following example has a queue trigger and a DocumentDB API output binding:</span></span>

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

### <a name="event-hubs-trigger-and-output"></a><span data-ttu-id="639c1-184">Event Hubs eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-184">Event Hubs trigger and output</span></span>

<span data-ttu-id="639c1-185">Az Azure Functions támogatja indítható el, és az Event Hubs kötései kimeneti.</span><span class="sxs-lookup"><span data-stu-id="639c1-185">Azure Functions supports trigger and output bindings for Event Hubs.</span></span> <span data-ttu-id="639c1-186">További információkért lásd: [Azure Functions Eseményközpont kötések](functions-bindings-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-186">For more information, see  [Azure Functions Event Hub bindings](functions-bindings-event-hubs.md).</span></span>

<span data-ttu-id="639c1-187">hello típusok `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` és `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="639c1-187">hello types `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` and `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` are defined in hello NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="639c1-188">hello következő példában egy Eseményközpontba eseményindító:</span><span class="sxs-lookup"><span data-stu-id="639c1-188">hello following example uses an Event Hub trigger:</span></span>

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="639c1-189">hello következő például rendelkezik egy Eseményközpontot, kimeneti hello metódus visszatérési értéke hello kimenetként használata:</span><span class="sxs-lookup"><span data-stu-id="639c1-189">hello following example has an Event Hub output, using hello method return value as hello output:</span></span>

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

### <a name="external-file-input-and-output"></a><span data-ttu-id="639c1-190">Külső fájl bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-190">External file input and output</span></span>

<span data-ttu-id="639c1-191">Az Azure Functions támogatja eseményindító, bemeneti és kimeneti kötések külső fájlokat, például a Google-meghajtó, a Dropbox és a onedrive vállalati verzió.</span><span class="sxs-lookup"><span data-stu-id="639c1-191">Azure Functions supports trigger, input, and output bindings for external files, such as Google Drive, Dropbox, and OneDrive.</span></span> <span data-ttu-id="639c1-192">több, lásd: toolearn [Azure Functions külső fájl kötések](functions-bindings-external-file.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-192">toolearn more, see [Azure Functions External File bindings](functions-bindings-external-file.md).</span></span> <span data-ttu-id="639c1-193">attribútumok hello `[ExternalFileTrigger]` és `[ExternalFile]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span><span class="sxs-lookup"><span data-stu-id="639c1-193">hello attributes `[ExternalFileTrigger]` and `[ExternalFile]` are defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span></span>

<span data-ttu-id="639c1-194">a következő példa C# hello mutatja be, külső fájlban bemeneti és kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="639c1-194">hello following C# example demonstrates an external file input and output binding.</span></span> <span data-ttu-id="639c1-195">hello kódot másolja a bemeneti fájl toohello kimeneti fájl hello.</span><span class="sxs-lookup"><span data-stu-id="639c1-195">hello code copies hello input file toohello output file.</span></span>

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

### <a name="http-and-webhooks"></a><span data-ttu-id="639c1-196">HTTP és webhookok</span><span class="sxs-lookup"><span data-stu-id="639c1-196">HTTP and webhooks</span></span>

<span data-ttu-id="639c1-197">Használjon hello `HttpTrigger` attribútum toodefine HTTP eseményindító vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="639c1-197">Use hello `HttpTrigger` attribute toodefine an HTTP trigger or webhook.</span></span> <span data-ttu-id="639c1-198">Ez az attribútum van megadva hello NuGet-csomag [Microsoft.Azure.WebJobs.Extensions.Http].</span><span class="sxs-lookup"><span data-stu-id="639c1-198">This attribute is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.Http].</span></span> <span data-ttu-id="639c1-199">Hello jogosultsági szintet, a webhook típusa, az útvonal és a módszerek személyre is szabhatja.</span><span class="sxs-lookup"><span data-stu-id="639c1-199">You can customize hello authorization level, webhook type, route, and methods.</span></span> <span data-ttu-id="639c1-200">hello alábbi példa meghatározza a névtelen hitelesítés egy HTTP-eseményindítóval és _genericJson_ webhook típusa.</span><span class="sxs-lookup"><span data-stu-id="639c1-200">hello following example defines an HTTP trigger with anonymous authentication and _genericJson_ webhook type.</span></span>

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a><span data-ttu-id="639c1-201">Mobile Apps bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-201">Mobile Apps input and output</span></span>

<span data-ttu-id="639c1-202">Azure Functions támogatja bemeneti és kimeneti Mobile Apps kötéseit.</span><span class="sxs-lookup"><span data-stu-id="639c1-202">Azure Functions supports input and output bindings for Mobile Apps.</span></span> <span data-ttu-id="639c1-203">több, lásd: toolearn [Azure Functions Mobile Apps kötések](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-203">toolearn more, see [Azure Functions Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span> <span data-ttu-id="639c1-204">hello attribútum `[MobileTable]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span><span class="sxs-lookup"><span data-stu-id="639c1-204">hello attribute `[MobileTable]` is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span></span>

<span data-ttu-id="639c1-205">hello következő példa bemutatja a Mobile Apps kimeneti kötése:</span><span class="sxs-lookup"><span data-stu-id="639c1-205">hello following example demonstrates a Mobile Apps output binding:</span></span>

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a><span data-ttu-id="639c1-206">Notification Hubs kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-206">Notification Hubs output</span></span>

<span data-ttu-id="639c1-207">Az Azure Functions a Notification Hubs egy kimeneti kötése támogatja.</span><span class="sxs-lookup"><span data-stu-id="639c1-207">Azure Functions supports an output binding for Notification Hubs.</span></span> <span data-ttu-id="639c1-208">több, lásd: toolearn [Azure Functions Notification Hub kimeneti kötése](functions-bindings-notification-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-208">toolearn more, see [Azure Functions Notification Hub output binding](functions-bindings-notification-hubs.md).</span></span> <span data-ttu-id="639c1-209">hello attribútum `[NotificationHub]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span><span class="sxs-lookup"><span data-stu-id="639c1-209">hello attribute `[NotificationHub]` is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span></span>

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a><span data-ttu-id="639c1-210">Várólista tárolási eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-210">Queue storage trigger and output</span></span>

<span data-ttu-id="639c1-211">Az Azure Functions támogatja aktiválhatja, és kimeneti Azure várólisták kötései.</span><span class="sxs-lookup"><span data-stu-id="639c1-211">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="639c1-212">További információkért lásd: [Azure Functions a Queue Storage kötések](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-212">For more information, see [Azure Functions Queue Storage bindings](functions-bindings-storage-queue.md).</span></span>

<span data-ttu-id="639c1-213">hello következő példa bemutatja, hogyan toouse hello függvény visszatérési-típus egy várólista kimeneti kötelező, segítségével hello `[Queue]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="639c1-213">hello following example shows how toouse hello function return type with a queue output binding, using hello `[Queue]` attribute.</span></span> <span data-ttu-id="639c1-214">egy várólista eseményindító toodefine hello használata `[QueueTrigger]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="639c1-214">toodefine a queue trigger, use hello `[QueueTrigger]` attribute.</span></span>

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

### <a name="sendgrid-output"></a><span data-ttu-id="639c1-215">SendGrid kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-215">SendGrid output</span></span>

<span data-ttu-id="639c1-216">Az Azure Functions támogatja a SendGrid kimeneti kötése programozott módon küld e-mailt.</span><span class="sxs-lookup"><span data-stu-id="639c1-216">Azure Functions supports a SendGrid output binding for sending email programmatically.</span></span> <span data-ttu-id="639c1-217">több, lásd: toolearn [Azure Functions SendGrid kötések](functions-bindings-sendgrid.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-217">toolearn more, see [Azure Functions SendGrid bindings](functions-bindings-sendgrid.md).</span></span>

<span data-ttu-id="639c1-218">hello attribútum `[SendGrid]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span><span class="sxs-lookup"><span data-stu-id="639c1-218">hello attribute `[SendGrid]` is defined in hello NuGet package [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span></span>

<span data-ttu-id="639c1-219">hello az alábbiakban látható példa Service Bus várólista eseményindítót használ, és a SendGrid kimeneti kötés használatával `SendGridMessage`:</span><span class="sxs-lookup"><span data-stu-id="639c1-219">hello following is an example of using a Service Bus queue trigger and a SendGrid output binding using `SendGridMessage`:</span></span>

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

### <a name="service-bus-trigger-and-output"></a><span data-ttu-id="639c1-220">A Service Bus eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-220">Service Bus trigger and output</span></span>

<span data-ttu-id="639c1-221">Az Azure Functions támogatja aktiválhatja és Service Bus-üzenetsorok és témakörök kimeneti.</span><span class="sxs-lookup"><span data-stu-id="639c1-221">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span> <span data-ttu-id="639c1-222">Kötések konfigurálásával kapcsolatos további információkért lásd: [Azure Functions Service Bus kötések](functions-bindings-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-222">For more information on configuring bindings, see [Azure Functions Service Bus bindings](functions-bindings-service-bus.md).</span></span>

<span data-ttu-id="639c1-223">attribútumok hello `[ServiceBusTrigger]` és `[ServiceBus]` hello NuGet-csomag meghatározott [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="639c1-223">hello attributes `[ServiceBusTrigger]` and `[ServiceBus]` are defined in hello NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="639c1-224">hello az alábbiakban látható egy példa egy Service Bus várólista eseményindító:</span><span class="sxs-lookup"><span data-stu-id="639c1-224">hello following is an example of a Service Bus queue trigger:</span></span>

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<span data-ttu-id="639c1-225">hello az alábbiakban látható egy példa egy Service Bus kimeneti kötése, hello metódus visszatérési típusának hello kimenetként használata:</span><span class="sxs-lookup"><span data-stu-id="639c1-225">hello following is an example of a Service Bus output binding, using hello method return type as hello output:</span></span>

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

### <a name="table-storage-input-and-output"></a><span data-ttu-id="639c1-226">TABLE storage bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="639c1-226">Table storage input and output</span></span>

<span data-ttu-id="639c1-227">Az Azure Functions támogatja bemeneti és kimeneti Azure Table storage kötései.</span><span class="sxs-lookup"><span data-stu-id="639c1-227">Azure Functions supports input and output bindings for Azure Table storage.</span></span> <span data-ttu-id="639c1-228">több, lásd: toolearn [Azure Functions Table storage kötések](functions-bindings-storage-table.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-228">toolearn more, see [Azure Functions Table storage bindings](functions-bindings-storage-table.md).</span></span>

<span data-ttu-id="639c1-229">hello következő példa egy olyan osztály, amelynek két funkciót, a table storage kimeneti és a bemeneti kötések, amely tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="639c1-229">hello following example is a class with two functions, demonstrating table storage output and input bindings.</span></span> 

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

### <a name="timer-trigger"></a><span data-ttu-id="639c1-230">Időzítő eseményindító</span><span class="sxs-lookup"><span data-stu-id="639c1-230">Timer trigger</span></span>

<span data-ttu-id="639c1-231">Az Azure Functions lehetővé teszi, hogy a kódra a függvény a meghatározott ütemezés szerint időzítő eseményindító-kötéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="639c1-231">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> <span data-ttu-id="639c1-232">toolearn hello szolgáltatások hello kötés, bővebben lásd: [kód végrehajtását az Azure Functions](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-232">toolearn more about hello features of hello binding, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>

<span data-ttu-id="639c1-233">Hello fogyasztás terv, ütemezés a definiálhat egy [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span><span class="sxs-lookup"><span data-stu-id="639c1-233">On hello Consumption plan, you can define schedules with a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span></span> <span data-ttu-id="639c1-234">Használata egy App Service-csomag, a TimeSpan karakterlánc is használható.</span><span class="sxs-lookup"><span data-stu-id="639c1-234">If you're using an App Service Plan, you can also use a TimeSpan string.</span></span> 

<span data-ttu-id="639c1-235">a következő példa hello meghatározza, hogy egy időzítő indítófeltételt 5 percenként futtató:</span><span class="sxs-lookup"><span data-stu-id="639c1-235">hello following example defines a timer trigger that runs every 5 minutes:</span></span>

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a><span data-ttu-id="639c1-236">Twilio-kimenet</span><span class="sxs-lookup"><span data-stu-id="639c1-236">Twilio output</span></span>

<span data-ttu-id="639c1-237">Az Azure Functions támogatja Twilio kimeneti kötések tooenable a funkciók toosend SMS szöveges üzenetek.</span><span class="sxs-lookup"><span data-stu-id="639c1-237">Azure Functions supports Twilio output bindings tooenable your functions toosend SMS text messages.</span></span> <span data-ttu-id="639c1-238">több, lásd: toolearn [az SMS küldése az Azure Functions használatával hello Twilio kimeneti kötése](functions-bindings-twilio.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-238">toolearn more, see [Send SMS messages from Azure Functions using hello Twilio output binding](functions-bindings-twilio.md).</span></span> 

<span data-ttu-id="639c1-239">hello attribútum `[TwilioSms]` hello csomagban van definiálva [Microsoft.Azure.WebJobs.Extensions.Twilio].</span><span class="sxs-lookup"><span data-stu-id="639c1-239">hello attribute `[TwilioSms]` is defined in hello package [Microsoft.Azure.WebJobs.Extensions.Twilio].</span></span>

<span data-ttu-id="639c1-240">hello alábbi C# példában várólista eseményindító és a Twilio kimeneti kötése:</span><span class="sxs-lookup"><span data-stu-id="639c1-240">hello following C# example uses a queue trigger and a Twilio output binding:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="639c1-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="639c1-241">Next steps</span></span>

<span data-ttu-id="639c1-242">További információ az Azure Functions használatával a C# scripting, talál [Azure Functions C\# fejlesztői leírás parancsfájl](functions-reference-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="639c1-242">For more information on using Azure Functions in C# scripting, see [Azure Functions C\# script developer reference](functions-reference-csharp.md).</span></span>

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

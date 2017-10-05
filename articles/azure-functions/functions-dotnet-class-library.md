---
title: "Alkalmazás használatával az Azure Functions |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre forgatókönyveket alkalmazás használható az Azure Functions"
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
ms.openlocfilehash: 0613bb96d3afb85ff7e684246b128e4eef518d23
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a><span data-ttu-id="1d6c0-104">Alkalmazás az Azure Functions használatával</span><span class="sxs-lookup"><span data-stu-id="1d6c0-104">Using .NET class libraries with Azure Functions</span></span>

<span data-ttu-id="1d6c0-105">Parancsfájl fájlok mellett az Azure Functions támogatja egy osztálytár közzététele, egy vagy több funkciók végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-105">In addition to script files, Azure Functions supports publishing a class library as the implementation for one or more functions.</span></span> <span data-ttu-id="1d6c0-106">Azt javasoljuk, hogy használja a [Azure Functions Visual Studio 2017 eszközök](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-106">We recommend that you use the [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d6c0-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d6c0-107">Prerequisites</span></span> 

<span data-ttu-id="1d6c0-108">Ez a cikk előfeltételei a következők:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-108">This article has the following prerequisites:</span></span>

- <span data-ttu-id="1d6c0-109">[A Visual Studio 2017 15.3 előzetes](https://www.visualstudio.com/vs/preview/).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-109">[Visual Studio 2017 15.3 Preview](https://www.visualstudio.com/vs/preview/).</span></span> <span data-ttu-id="1d6c0-110">Telepítse a munkaterhelések **ASP.NET és a webes fejlesztési** és **Azure fejlesztési**.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-110">Install the workloads **ASP.NET and web development** and **Azure development**.</span></span>
- [<span data-ttu-id="1d6c0-111">Az Azure-függvény Tools for Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1d6c0-111">Azure Function Tools for Visual Studio 2017</span></span>](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a><span data-ttu-id="1d6c0-112">Functions hordozhatóosztálytár-projektjének</span><span class="sxs-lookup"><span data-stu-id="1d6c0-112">Functions class library project</span></span>

<span data-ttu-id="1d6c0-113">A Visual Studio eszközből az Azure Functions új projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-113">From Visual Studio, create a new Azure Functions project.</span></span> <span data-ttu-id="1d6c0-114">Az új projekt sablont hoz létre a fájlok *host.json* és *local.settings.json*.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-114">The new project template creates the files *host.json* and *local.settings.json*.</span></span> <span data-ttu-id="1d6c0-115">Is [host.json az Azure Functions futásidejű beállításokat](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-115">You can [customize Azure Functions runtime settings in host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span> 

<span data-ttu-id="1d6c0-116">A fájl *local.settings.json* tárolja Alkalmazásbeállítások, a kapcsolati karakterláncok és az Azure Functions Core eszközök beállításait.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-116">The file *local.settings.json* stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="1d6c0-117">A struktúra kapcsolatos további információkért lásd: [kódot és az Azure functions helyi tesztelése](functions-run-local.md#local-settings).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-117">To learn more about its structure, see [Code and test Azure functions locally](functions-run-local.md#local-settings).</span></span>

### <a name="functionname-attribute"></a><span data-ttu-id="1d6c0-118">Függvénynév attribútum</span><span class="sxs-lookup"><span data-stu-id="1d6c0-118">FunctionName attribute</span></span>

<span data-ttu-id="1d6c0-119">Az attribútum [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) jelöli meg a metódus egy függvény belépési pontként.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-119">The attribute [`FunctionNameAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) marks a method as a function entry point.</span></span> <span data-ttu-id="1d6c0-120">Pontosan egy eseményindító együtt kell használni, és 0 vagy több bemeneti és kimeneti kötéseket.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-120">It must be used with exactly one trigger and 0 or more input and output bindings.</span></span>

### <a name="conversion-to-functionjson"></a><span data-ttu-id="1d6c0-121">Function.json átalakítása</span><span class="sxs-lookup"><span data-stu-id="1d6c0-121">Conversion to function.json</span></span>

<span data-ttu-id="1d6c0-122">Ha az Azure Functions projektek épül, akkor fájlt hoz létre `function.json` által definiált függvény névnek megfelelő könyvtárban `[FunctionName]`.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-122">When an Azure Functions project is built, it produces a file `function.json` in the directory matching the function name defined by `[FunctionName]`.</span></span> <span data-ttu-id="1d6c0-123">Azt adja meg, eseményindítók és kötések és a projekt szerelvényfájl mutat.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-123">It specifies triggers and bindings and points to the project assembly file.</span></span>

<span data-ttu-id="1d6c0-124">Az átalakításhoz végzi el a NuGet-csomag [Microsoft\.NET\.Sdk\.funkciók](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-124">This conversion is performed by the NuGet package [Microsoft\.NET\.Sdk\.Functions](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span></span> <span data-ttu-id="1d6c0-125">A forrás álljon rendelkezésre a GitHub-tárházban [azure\-funkciók\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-125">The source is available in the GitHub repo [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span></span>

## <a name="triggers-and-bindings"></a><span data-ttu-id="1d6c0-126">Eseményindítók és kötések</span><span class="sxs-lookup"><span data-stu-id="1d6c0-126">Triggers and bindings</span></span>

<span data-ttu-id="1d6c0-127">A következő táblázat az eseményindítók és kötések az Azure Functions hordozhatóosztálytár-projektjének elérhető.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-127">The following table lists the triggers and bindings that are available in an Azure Functions class library project.</span></span> <span data-ttu-id="1d6c0-128">A következő névtérbeli attribútumok összes `Microsoft.Azure.WebJobs`.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-128">All attributes are in the namespace `Microsoft.Azure.WebJobs`.</span></span>

| <span data-ttu-id="1d6c0-129">Kötelező</span><span class="sxs-lookup"><span data-stu-id="1d6c0-129">Binding</span></span> | <span data-ttu-id="1d6c0-130">Attribútum</span><span class="sxs-lookup"><span data-stu-id="1d6c0-130">Attribute</span></span> | <span data-ttu-id="1d6c0-131">NuGet-csomag</span><span class="sxs-lookup"><span data-stu-id="1d6c0-131">NuGet package</span></span> |
|------   | ------    | ------        |
| [<span data-ttu-id="1d6c0-132">A BLOB storage eseményindító, bemeneti, kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-132">Blob storage trigger, input, output</span></span>](#blob-storage) | <span data-ttu-id="1d6c0-133">[BlobAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-133">[BlobAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="1d6c0-134">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-134">[Microsoft.Azure.WebJobs]</span></span> | <span data-ttu-id="1d6c0-135">[A blob storage]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-135">[Blob storage]</span></span> |
| [<span data-ttu-id="1d6c0-136">A cosmos DB bemeneti és kimeneti kötése</span><span class="sxs-lookup"><span data-stu-id="1d6c0-136">Cosmos DB input and output binding</span></span>](#cosmos-db) | <span data-ttu-id="1d6c0-137">[DocumentDBAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-137">[DocumentDBAttribute]</span></span> | <span data-ttu-id="1d6c0-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span></span> | 
| [<span data-ttu-id="1d6c0-139">Event Hubs eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-139">Event Hubs trigger and output</span></span>](#event-hub) | <span data-ttu-id="1d6c0-140">[EventHubTriggerAttribute], [EventHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-140">[EventHubTriggerAttribute], [EventHubAttribute]</span></span> | <span data-ttu-id="1d6c0-141">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-141">[Microsoft.Azure.WebJobs.ServiceBus]</span></span> |
| [<span data-ttu-id="1d6c0-142">Külső fájl bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-142">External file input and output</span></span>](#api-hub) | <span data-ttu-id="1d6c0-143">[ApiHubFileAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-143">[ApiHubFileAttribute]</span></span> | <span data-ttu-id="1d6c0-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span></span> |
| [<span data-ttu-id="1d6c0-145">A HTTP és a webhook eseményindító</span><span class="sxs-lookup"><span data-stu-id="1d6c0-145">HTTP and webhook trigger</span></span>](#http) | <span data-ttu-id="1d6c0-146">[HttpTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-146">[HttpTriggerAttribute]</span></span> | <span data-ttu-id="1d6c0-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span></span> |
| [<span data-ttu-id="1d6c0-148">Mobile Apps bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-148">Mobile Apps input and output</span></span>](#mobile-apps) | <span data-ttu-id="1d6c0-149">[MobileTableAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-149">[MobileTableAttribute]</span></span> | <span data-ttu-id="1d6c0-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span></span> | 
| [<span data-ttu-id="1d6c0-151">Notification Hubs kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-151">Notification Hubs output</span></span>](#nh) | <span data-ttu-id="1d6c0-152">[NotificationHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-152">[NotificationHubAttribute]</span></span> | <span data-ttu-id="1d6c0-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span></span> | 
| [<span data-ttu-id="1d6c0-154">Várólista tárolási eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-154">Queue storage trigger and output</span></span>](#queue) | <span data-ttu-id="1d6c0-155">[QueueAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-155">[QueueAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="1d6c0-156">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-156">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="1d6c0-157">SendGrid kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-157">SendGrid output</span></span>](#sendgrid) | <span data-ttu-id="1d6c0-158">[SendGridAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-158">[SendGridAttribute]</span></span> | <span data-ttu-id="1d6c0-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span></span> | 
| [<span data-ttu-id="1d6c0-160">A Service Bus eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-160">Service Bus trigger and output</span></span>](#service-bus) | <span data-ttu-id="1d6c0-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span></span> | <span data-ttu-id="1d6c0-162">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-162">[Microsoft.Azure.WebJobs.ServiceBus]</span></span>
| [<span data-ttu-id="1d6c0-163">TABLE storage bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-163">Table storage input and output</span></span>](#table) | <span data-ttu-id="1d6c0-164">[TableAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-164">[TableAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="1d6c0-165">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-165">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="1d6c0-166">Időzítő eseményindító</span><span class="sxs-lookup"><span data-stu-id="1d6c0-166">Timer trigger</span></span>](#timer) | <span data-ttu-id="1d6c0-167">[TimerTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-167">[TimerTriggerAttribute]</span></span> | <span data-ttu-id="1d6c0-168">[Microsoft.Azure.WebJobs.Extensions]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-168">[Microsoft.Azure.WebJobs.Extensions]</span></span> | 
| [<span data-ttu-id="1d6c0-169">Twilio-kimenet</span><span class="sxs-lookup"><span data-stu-id="1d6c0-169">Twilio output</span></span>](#twilio) | <span data-ttu-id="1d6c0-170">[TwilioSmsAttribute]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-170">[TwilioSmsAttribute]</span></span> | <span data-ttu-id="1d6c0-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span><span class="sxs-lookup"><span data-stu-id="1d6c0-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span></span> | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a><span data-ttu-id="1d6c0-172">A BLOB storage eseményindító, bemeneti és kimeneti kötések</span><span class="sxs-lookup"><span data-stu-id="1d6c0-172">Blob storage trigger, input, and output bindings</span></span>

<span data-ttu-id="1d6c0-173">Az Azure Functions támogatja eseményindító, adjon meg, és kimeneti Azure Blob Storage tárolóban kötéseit.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-173">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="1d6c0-174">A kötés kifejezések és a metaadatok további információkért lásd: [Azure Functions Blob storage kötések](functions-bindings-storage-blob.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-174">For more information on binding expressions and metadata, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>

<span data-ttu-id="1d6c0-175">Egy blob eseményindító van definiálva a `[BlobTrigger]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-175">A blob trigger is defined with the `[BlobTrigger]` attribute.</span></span> <span data-ttu-id="1d6c0-176">Az attribútum használható `[StorageAccount]` a tárfiók egy teljes vagy a osztály által használt meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-176">You can use the attribute `[StorageAccount]` to define the storage account that is used by an entire function or class.</span></span>

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

<span data-ttu-id="1d6c0-177">BLOB bemeneti és kimeneti meghatározása a `[Blob]` attribútumot, jelszavat rendelkező egy `FileAccess` paraméter jelző olvasására vagy írására.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-177">Blob input and output are defined using the `[Blob]` attribute, along with a `FileAccess` parameter indicating read or write.</span></span> <span data-ttu-id="1d6c0-178">Az alábbi példában egy blob eseményindító és blob kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-178">The following example uses a blob trigger and blob output binding.</span></span>

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

### <a name="cosmos-db-input-and-output-bindings"></a><span data-ttu-id="1d6c0-179">Cosmos DB bemeneti és kimeneti kötések</span><span class="sxs-lookup"><span data-stu-id="1d6c0-179">Cosmos DB input and output bindings</span></span>

<span data-ttu-id="1d6c0-180">Az Azure Functions támogatja bemeneti és kimeneti Cosmos DB kötései.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-180">Azure Functions supports input and output bindings for Cosmos DB.</span></span> <span data-ttu-id="1d6c0-181">A Cosmos DB kötés funkciókkal kapcsolatos további tudnivalókért lásd: [Azure Functions Cosmos DB kötések](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-181">To learn more about the features of the Cosmos DB binding, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>

<span data-ttu-id="1d6c0-182">Kötési Cosmos DB dokumentumhoz, használja a attribútum `[DocumentDB]` a NuGet-csomagot a [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-182">To bind to a Cosmos DB document, use the attribute `[DocumentDB]` in the NuGet package [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span></span> <span data-ttu-id="1d6c0-183">A következő példa a várólista eseményindító és a DocumentDB API kimeneti kötése van:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-183">The following example has a queue trigger and a DocumentDB API output binding:</span></span>

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

### <a name="event-hubs-trigger-and-output"></a><span data-ttu-id="1d6c0-184">Event Hubs eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-184">Event Hubs trigger and output</span></span>

<span data-ttu-id="1d6c0-185">Az Azure Functions támogatja indítható el, és az Event Hubs kötései kimeneti.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-185">Azure Functions supports trigger and output bindings for Event Hubs.</span></span> <span data-ttu-id="1d6c0-186">További információkért lásd: [Azure Functions Eseményközpont kötések](functions-bindings-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-186">For more information, see  [Azure Functions Event Hub bindings](functions-bindings-event-hubs.md).</span></span>

<span data-ttu-id="1d6c0-187">A típusok `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` és `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` határozzák meg a NuGet-csomag [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-187">The types `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` and `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` are defined in the NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="1d6c0-188">A következő példában egy Eseményközpontba eseményindító:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-188">The following example uses an Event Hub trigger:</span></span>

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="1d6c0-189">A következő példa egy Eseményközpontba kimeneti, a metódus visszatérési értéket használja, mint a kimeneti rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-189">The following example has an Event Hub output, using the method return value as the output:</span></span>

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

### <a name="external-file-input-and-output"></a><span data-ttu-id="1d6c0-190">Külső fájl bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-190">External file input and output</span></span>

<span data-ttu-id="1d6c0-191">Az Azure Functions támogatja eseményindító, bemeneti és kimeneti kötések külső fájlokat, például a Google-meghajtó, a Dropbox és a onedrive vállalati verzió.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-191">Azure Functions supports trigger, input, and output bindings for external files, such as Google Drive, Dropbox, and OneDrive.</span></span> <span data-ttu-id="1d6c0-192">További tudnivalókért lásd: [Azure Functions külső fájl kötések](functions-bindings-external-file.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-192">To learn more, see [Azure Functions External File bindings](functions-bindings-external-file.md).</span></span> <span data-ttu-id="1d6c0-193">Az attribútumok `[ExternalFileTrigger]` és `[ExternalFile]` határozzák meg a NuGet-csomag [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-193">The attributes `[ExternalFileTrigger]` and `[ExternalFile]` are defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span></span>

<span data-ttu-id="1d6c0-194">A következő C# példa bemutatja a külső fájlban bemeneti és kimeneti kötése.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-194">The following C# example demonstrates an external file input and output binding.</span></span> <span data-ttu-id="1d6c0-195">A kódot másolja a kimeneti fájl a bemeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-195">The code copies the input file to the output file.</span></span>

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

### <a name="http-and-webhooks"></a><span data-ttu-id="1d6c0-196">HTTP és webhookok</span><span class="sxs-lookup"><span data-stu-id="1d6c0-196">HTTP and webhooks</span></span>

<span data-ttu-id="1d6c0-197">Használja a `HttpTrigger` attribútum használatával definiálja a egy HTTP-eseményindítóval vagy webhook.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-197">Use the `HttpTrigger` attribute to define an HTTP trigger or webhook.</span></span> <span data-ttu-id="1d6c0-198">Ez az attribútum van megadva a NuGet-csomag [Microsoft.Azure.WebJobs.Extensions.Http].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-198">This attribute is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.Http].</span></span> <span data-ttu-id="1d6c0-199">A jogosultsági szintet, a webhook típusa, az útvonal és a metódusok személyre is szabhatja.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-199">You can customize the authorization level, webhook type, route, and methods.</span></span> <span data-ttu-id="1d6c0-200">Az alábbi példa meghatározza a névtelen hitelesítés egy HTTP-eseményindítóval és _genericJson_ webhook típusa.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-200">The following example defines an HTTP trigger with anonymous authentication and _genericJson_ webhook type.</span></span>

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a><span data-ttu-id="1d6c0-201">Mobile Apps bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-201">Mobile Apps input and output</span></span>

<span data-ttu-id="1d6c0-202">Azure Functions támogatja bemeneti és kimeneti Mobile Apps kötéseit.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-202">Azure Functions supports input and output bindings for Mobile Apps.</span></span> <span data-ttu-id="1d6c0-203">További tudnivalókért lásd: [Azure Functions Mobile Apps kötések](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-203">To learn more, see [Azure Functions Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span> <span data-ttu-id="1d6c0-204">Az attribútum `[MobileTable]` definiálva van a NuGet-csomag [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-204">The attribute `[MobileTable]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span></span>

<span data-ttu-id="1d6c0-205">A következő példa bemutatja a Mobile Apps kimeneti kötése:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-205">The following example demonstrates a Mobile Apps output binding:</span></span>

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a><span data-ttu-id="1d6c0-206">Notification Hubs kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-206">Notification Hubs output</span></span>

<span data-ttu-id="1d6c0-207">Az Azure Functions a Notification Hubs egy kimeneti kötése támogatja.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-207">Azure Functions supports an output binding for Notification Hubs.</span></span> <span data-ttu-id="1d6c0-208">További tudnivalókért lásd: [Azure Functions Notification Hub kimeneti kötése](functions-bindings-notification-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-208">To learn more, see [Azure Functions Notification Hub output binding](functions-bindings-notification-hubs.md).</span></span> <span data-ttu-id="1d6c0-209">Az attribútum `[NotificationHub]` definiálva van a NuGet-csomag [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-209">The attribute `[NotificationHub]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span></span>

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a><span data-ttu-id="1d6c0-210">Várólista tárolási eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-210">Queue storage trigger and output</span></span>

<span data-ttu-id="1d6c0-211">Az Azure Functions támogatja aktiválhatja, és kimeneti Azure várólisták kötései.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-211">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="1d6c0-212">További információkért lásd: [Azure Functions a Queue Storage kötések](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-212">For more information, see [Azure Functions Queue Storage bindings](functions-bindings-storage-queue.md).</span></span>

<span data-ttu-id="1d6c0-213">A következő példa bemutatja, hogyan használható a függvény visszatérési típusa egy várólista kimeneti kötelező, segítségével a `[Queue]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-213">The following example shows how to use the function return type with a queue output binding, using the `[Queue]` attribute.</span></span> <span data-ttu-id="1d6c0-214">A várólista eseményindító megadásához használja a `[QueueTrigger]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-214">To define a queue trigger, use the `[QueueTrigger]` attribute.</span></span>

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

### <a name="sendgrid-output"></a><span data-ttu-id="1d6c0-215">SendGrid kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-215">SendGrid output</span></span>

<span data-ttu-id="1d6c0-216">Az Azure Functions támogatja a SendGrid kimeneti kötése programozott módon küld e-mailt.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-216">Azure Functions supports a SendGrid output binding for sending email programmatically.</span></span> <span data-ttu-id="1d6c0-217">További tudnivalókért lásd: [Azure Functions SendGrid kötések](functions-bindings-sendgrid.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-217">To learn more, see [Azure Functions SendGrid bindings](functions-bindings-sendgrid.md).</span></span>

<span data-ttu-id="1d6c0-218">Az attribútum `[SendGrid]` definiálva van a NuGet-csomag [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-218">The attribute `[SendGrid]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span></span>

<span data-ttu-id="1d6c0-219">Az alábbiakban egy példa Service Bus várólista eseményindítót használ, és a SendGrid kimeneti kötés használatával `SendGridMessage`:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-219">The following is an example of using a Service Bus queue trigger and a SendGrid output binding using `SendGridMessage`:</span></span>

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
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a><span data-ttu-id="1d6c0-220">A Service Bus eseményindító és a kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-220">Service Bus trigger and output</span></span>

<span data-ttu-id="1d6c0-221">Az Azure Functions támogatja aktiválhatja és Service Bus-üzenetsorok és témakörök kimeneti.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-221">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span> <span data-ttu-id="1d6c0-222">Kötések konfigurálásával kapcsolatos további információkért lásd: [Azure Functions Service Bus kötések](functions-bindings-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-222">For more information on configuring bindings, see [Azure Functions Service Bus bindings](functions-bindings-service-bus.md).</span></span>

<span data-ttu-id="1d6c0-223">Az attribútumok `[ServiceBusTrigger]` és `[ServiceBus]` határozzák meg a NuGet-csomag [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-223">The attributes `[ServiceBusTrigger]` and `[ServiceBus]` are defined in the NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="1d6c0-224">A következő egy példa egy Service Bus várólista eseményindító:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-224">The following is an example of a Service Bus queue trigger:</span></span>

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<span data-ttu-id="1d6c0-225">A következő egy példa egy Service Bus: kötelező, segítségével a metódus visszatérési típusának kimeneteként:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-225">The following is an example of a Service Bus output binding, using the method return type as the output:</span></span>

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

### <a name="table-storage-input-and-output"></a><span data-ttu-id="1d6c0-226">TABLE storage bemeneti és kimeneti</span><span class="sxs-lookup"><span data-stu-id="1d6c0-226">Table storage input and output</span></span>

<span data-ttu-id="1d6c0-227">Az Azure Functions támogatja bemeneti és kimeneti Azure Table storage kötései.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-227">Azure Functions supports input and output bindings for Azure Table storage.</span></span> <span data-ttu-id="1d6c0-228">További tudnivalókért lásd: [Azure Functions Table storage kötések](functions-bindings-storage-table.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-228">To learn more, see [Azure Functions Table storage bindings](functions-bindings-storage-table.md).</span></span>

<span data-ttu-id="1d6c0-229">A következő példa az osztály két függvényekkel, table storage kimeneti és a bemeneti kötések, amely tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-229">The following example is a class with two functions, demonstrating table storage output and input bindings.</span></span> 

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

    // use the metadata parameter "queueTrigger" to bind the queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a><span data-ttu-id="1d6c0-230">Időzítő eseményindító</span><span class="sxs-lookup"><span data-stu-id="1d6c0-230">Timer trigger</span></span>

<span data-ttu-id="1d6c0-231">Az Azure Functions lehetővé teszi, hogy a kódra a függvény a meghatározott ütemezés szerint időzítő eseményindító-kötéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-231">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> <span data-ttu-id="1d6c0-232">A kötés funkciókkal kapcsolatos további tudnivalókért lásd: [kód végrehajtását az Azure Functions](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-232">To learn more about the features of the binding, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>

<span data-ttu-id="1d6c0-233">A felhasználási terv ütemezés a definiálhat egy [CRON-kifejezés](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-233">On the Consumption plan, you can define schedules with a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span></span> <span data-ttu-id="1d6c0-234">Használata egy App Service-csomag, a TimeSpan karakterlánc is használható.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-234">If you're using an App Service Plan, you can also use a TimeSpan string.</span></span> 

<span data-ttu-id="1d6c0-235">Az alábbi példa meghatározza, hogy egy időzítő indítófeltételt 5 percenként futtató:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-235">The following example defines a timer trigger that runs every 5 minutes:</span></span>

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a><span data-ttu-id="1d6c0-236">Twilio-kimenet</span><span class="sxs-lookup"><span data-stu-id="1d6c0-236">Twilio output</span></span>

<span data-ttu-id="1d6c0-237">Az Azure Functions támogatja a Twilio kimeneti kötések SMS szöveges üzenetek küldéséhez a funkciók engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="1d6c0-237">Azure Functions supports Twilio output bindings to enable your functions to send SMS text messages.</span></span> <span data-ttu-id="1d6c0-238">További tudnivalókért lásd: [az SMS küldése az Azure Functions használatával a Twilio kimeneti kötése](functions-bindings-twilio.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-238">To learn more, see [Send SMS messages from Azure Functions using the Twilio output binding](functions-bindings-twilio.md).</span></span> 

<span data-ttu-id="1d6c0-239">Az attribútum `[TwilioSms]` a csomagban van definiálva [Microsoft.Azure.WebJobs.Extensions.Twilio].</span><span class="sxs-lookup"><span data-stu-id="1d6c0-239">The attribute `[TwilioSms]` is defined in the package [Microsoft.Azure.WebJobs.Extensions.Twilio].</span></span>

<span data-ttu-id="1d6c0-240">Az alábbi C# példában várólista eseményindító és a Twilio kimeneti kötése:</span><span class="sxs-lookup"><span data-stu-id="1d6c0-240">The following C# example uses a queue trigger and a Twilio output binding:</span></span>

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a><span data-ttu-id="1d6c0-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d6c0-241">Next steps</span></span>

<span data-ttu-id="1d6c0-242">További információ az Azure Functions használatával a C# scripting, talál [Azure Functions C\# fejlesztői leírás parancsfájl](functions-reference-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="1d6c0-242">For more information on using Azure Functions in C# scripting, see [Azure Functions C\# script developer reference](functions-reference-csharp.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
<span data-ttu-id="1d6c0-243">[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-243">[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-244">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-244">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-245">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-245">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-246">[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-246">[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-247">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-247">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-248">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-248">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-249">[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-249">[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-250">[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-250">[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1</span></span>
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
<span data-ttu-id="1d6c0-251">[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4</span><span class="sxs-lookup"><span data-stu-id="1d6c0-251">[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4</span></span>
<span data-ttu-id="1d6c0-252">[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-252">[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1</span></span>
<span data-ttu-id="1d6c0-253">[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="1d6c0-253">[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1</span></span>


<!-- Links to source --> 
<span data-ttu-id="1d6c0-254">[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-254">[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs</span></span>
<span data-ttu-id="1d6c0-255">[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-255">[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs</span></span>
<span data-ttu-id="1d6c0-256">[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-256">[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs</span></span>
<span data-ttu-id="1d6c0-257">[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-257">[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs</span></span>
<span data-ttu-id="1d6c0-258">[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs </span><span class="sxs-lookup"><span data-stu-id="1d6c0-258">[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs </span></span>
<span data-ttu-id="1d6c0-259">[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-259">[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs</span></span>
<span data-ttu-id="1d6c0-260">[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-260">[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs</span></span>
<span data-ttu-id="1d6c0-261">[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-261">[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs</span></span>
<span data-ttu-id="1d6c0-262">[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-262">[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs</span></span>
<span data-ttu-id="1d6c0-263">[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-263">[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs</span></span>
<span data-ttu-id="1d6c0-264">[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-264">[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs</span></span>
<span data-ttu-id="1d6c0-265">[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-265">[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs</span></span>
<span data-ttu-id="1d6c0-266">[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-266">[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs</span></span>
<span data-ttu-id="1d6c0-267">[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-267">[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs</span></span>
<span data-ttu-id="1d6c0-268">[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-268">[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs</span></span>
<span data-ttu-id="1d6c0-269">[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="1d6c0-269">[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs</span></span>

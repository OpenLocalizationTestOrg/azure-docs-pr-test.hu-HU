---
title: "How to use Azure blob storage with the WebJobs SDK (Az Azure Blob Storage használata a WebJobs SDK-val)"
description: "További tudnivalók az Azure blob storage használata a WebJobs SDK-val. Indul el egy folyamatot, ha egy új blob a tárolóban lévő jelenik meg, és \"poison blobok\" kezelni."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.assetid: bf32f919-f7bc-4aaa-916e-461c02f2e26c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: e0a792ccdf8097d5cde254d6d4690a64838378ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a><span data-ttu-id="5b6f0-104">How to use Azure blob storage with the WebJobs SDK (Az Azure Blob Storage használata a WebJobs SDK-val)</span><span class="sxs-lookup"><span data-stu-id="5b6f0-104">How to use Azure blob storage with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="5b6f0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5b6f0-105">Overview</span></span>
<span data-ttu-id="5b6f0-106">Ez az útmutató C# mintakódok bemutatják, hogyan indul el egy folyamatot, ha az Azure blob létrehozásakor vagy frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-106">This guide provides C# code samples that show how to trigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="5b6f0-107">A kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-107">The code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="5b6f0-108">Blobok létrehozását mutatják be mintakódok, lásd: [Azure a queue storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="5b6f0-108">For code samples that show how to create blobs, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="5b6f0-109">Az útmutató azt feltételezi, hogy tudja [hogyan webjobs-feladat-projekt létrehozása a Visual Studio kapcsolati karakterláncok a tárfiókhoz adott pontra](websites-dotnet-webjobs-sdk-get-started.md) vagy [több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="5b6f0-109">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md) or to [multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

## <span data-ttu-id="5b6f0-110"><a id="trigger"></a>Hogyan indul el egy function, ha egy blob létrehozásakor vagy frissítésekor</span><span class="sxs-lookup"><span data-stu-id="5b6f0-110"><a id="trigger"></a> How to trigger a function when a blob is created or updated</span></span>
<span data-ttu-id="5b6f0-111">Ez a szakasz bemutatja, hogyan használja a `BlobTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-111">This section shows how to use the `BlobTrigger` attribute.</span></span> 

> [!NOTE]
> <span data-ttu-id="5b6f0-112">A WebJobs SDK megvizsgálja az új vagy módosított blobok figyelendő naplófájljait.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-112">The WebJobs SDK scans log files to watch for new or changed blobs.</span></span> <span data-ttu-id="5b6f0-113">Ez a folyamat nincs valós idejű; egy függvény előfordulhat, hogy nem get indulnak el, néhány percig, vagy már a blob létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-113">This process is not real-time; a function might not get triggered until several minutes or longer after the blob is created.</span></span> <span data-ttu-id="5b6f0-114">Emellett [tárolási naplófájlokat hoz létre a "legjobb erőfeszítéseket"](https://msdn.microsoft.com/library/azure/hh343262.aspx) időközönként; nincs garancia, hogy az összes esemény rögzíteni kell.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-114">In addition, [storage logs are created on a "best efforts"](https://msdn.microsoft.com/library/azure/hh343262.aspx) basis; there is no guarantee that all events will be captured.</span></span> <span data-ttu-id="5b6f0-115">Bizonyos körülmények között előfordulhat, hogy lehet kihagyni a naplókat.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-115">Under some conditions, logs might be missed.</span></span> <span data-ttu-id="5b6f0-116">Ha gyorsabb és megbízhatóbb vonatkozó korlátozások blob eseményindítók nem elfogadható az alkalmazáshoz, az ajánlott módszer a blob létrehozásakor, és használja egy üzenetsor létrehozásához-e a [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribútum helyett a `BlobTrigger` a függvény, amely feldolgozza a blob attribútum.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-116">If the speed and reliability limitations of blob triggers are not acceptable for your application, the recommended method is to create a queue message when you create the blob, and use the [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of the `BlobTrigger` attribute on the function that processes the blob.</span></span>
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="5b6f0-117">A blob kiterjesztésű egyetlen helyőrzője</span><span class="sxs-lookup"><span data-stu-id="5b6f0-117">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="5b6f0-118">A következő példakód másolja át a megjelenő szöveg blobok a *bemeneti* tárolót, hogy a *kimeneti* tároló:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-118">The following code sample copies text blobs that appear in the *input* container to the *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="5b6f0-119">Az attribútum konstruktora a, amelyben a tároló neve és a blob nevének helyőrzője karakterlánc-paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-119">The attribute constructor takes a string parameter that specifies the container name and a placeholder for the blob name.</span></span> <span data-ttu-id="5b6f0-120">Ebben a példában, ha egy blob neve *Blob1.txt* jön létre a *bemeneti* tároló, a függvény létrehoz egy blob nevű *Blob1.txt* a a *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-120">In this example, if a blob named *Blob1.txt* is created in the *input* container, the function creates a blob named *Blob1.txt* in the *output* container.</span></span> 

<span data-ttu-id="5b6f0-121">Megadhatja egy mintát a blob nevének helyőrzője, a következő kód mintában látható módon:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-121">You can specify a name pattern with the blob name placeholder, as shown in the following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="5b6f0-122">Ez a kód csak blobok kezdve "eredeti-" nevű másolja.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-122">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="5b6f0-123">Például *eredeti-Blob1.txt* a a *bemeneti* tároló másolódik *másolási-Blob1.txt* a a *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-123">For example, *original-Blob1.txt* in the *input* container is copied to *copy-Blob1.txt* in the *output* container.</span></span>

<span data-ttu-id="5b6f0-124">Ha meg kell adnia egy mintát a blob neve kapcsos zárójelek a neve, dupla a kapcsos zárójelek.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-124">If you need to specify a name pattern for blob names that have curly braces in the name, double the curly braces.</span></span> <span data-ttu-id="5b6f0-125">Például, ha a keresett blobot, amely a *képek* ilyen nevű tárolóban:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-125">For example, if you want to find blobs in the *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="5b6f0-126">Ezt a mintát használja:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-126">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="5b6f0-127">A példában a *neve* helyőrző értéke lenne *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-127">In the example, the *name* placeholder value would be *soundfile.mp3*.</span></span> 

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="5b6f0-128">Külön blob-névnek és kiterjesztésnek helyőrzők</span><span class="sxs-lookup"><span data-stu-id="5b6f0-128">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="5b6f0-129">A következő példakód módosítja a fájlnévkiterjesztés, a megjelenő blobot másol a *bemeneti* tárolót, hogy a *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-129">The following code sample changes the file extension as it copies blobs that appear in the *input* container to the *output* container.</span></span> <span data-ttu-id="5b6f0-130">A kód kiterjesztését naplózza a *bemeneti* blob-, és beállítja a kiterjesztését a *kimeneti* a blob *.txt*.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-130">The code logs the extension of the *input* blob and sets the extension of the *output* blob to *.txt*.</span></span>

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <span data-ttu-id="5b6f0-131"><a id="types"></a>Blobok köthető típusai</span><span class="sxs-lookup"><span data-stu-id="5b6f0-131"><a id="types"></a> Types that you can bind to blobs</span></span>
<span data-ttu-id="5b6f0-132">Használhatja a `BlobTrigger` attribútum a következő esetében:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-132">You can use the `BlobTrigger` attribute on the following types:</span></span>

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* <span data-ttu-id="5b6f0-133">Egyéb típusú általi [ICloudBlobStreamBinder](#icbsb)</span><span class="sxs-lookup"><span data-stu-id="5b6f0-133">Other types deserialized by [ICloudBlobStreamBinder](#icbsb)</span></span> 

<span data-ttu-id="5b6f0-134">Ha közvetlenül az Azure storage-fiók dolgozni szeretne, azt is megteheti egy `CloudStorageAccount` a metódus-aláírás paramétert.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-134">If you want to work directly with the Azure storage account, you can also add a `CloudStorageAccount` parameter to the method signature.</span></span>

<span data-ttu-id="5b6f0-135">Tekintse meg a [blob-kötés kód az azure-webjobs-sdk-tárházban github.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="5b6f0-135">For examples, see the [blob binding code in the azure-webjobs-sdk repository on GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span></span>

## <span data-ttu-id="5b6f0-136"><a id="string"></a>Szöveges blob tartalma karakterláncra kötés beolvasása</span><span class="sxs-lookup"><span data-stu-id="5b6f0-136"><a id="string"></a> Getting text blob content by binding to string</span></span>
<span data-ttu-id="5b6f0-137">Ha a szöveges BLOB várható, `BlobTrigger` alkalmazható egy `string` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-137">If text blobs are expected, `BlobTrigger` can be applied to a `string` parameter.</span></span> <span data-ttu-id="5b6f0-138">A következő példakód köti a szöveges blob egy `string` nevű paraméter `logMessage`.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-138">The following code sample binds a text blob to a `string` parameter named `logMessage`.</span></span> <span data-ttu-id="5b6f0-139">A funkció, hogy a paraméter a blob tartalmát írni a WebJobs SDK irányítópult.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-139">The function uses that parameter to write the contents of the blob to the WebJobs SDK dashboard.</span></span> 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <span data-ttu-id="5b6f0-140"><a id="icbsb"></a>Első szerializált blob tartalma ICloudBlobStreamBinder használatával</span><span class="sxs-lookup"><span data-stu-id="5b6f0-140"><a id="icbsb"></a> Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="5b6f0-141">A következő példakód használ, amely megvalósítja az osztály `ICloudBlobStreamBinder` engedélyezése a `BlobTrigger` attribútum egy blob kötni a `WebImage` típusa.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-141">The following code sample uses a class that implements `ICloudBlobStreamBinder` to enable the `BlobTrigger` attribute to bind a blob to the `WebImage` type.</span></span>

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

<span data-ttu-id="5b6f0-142">A `WebImage` kötés kód megtalálható egy `WebImageBinder` abból származó osztály `ICloudBlobStreamBinder`.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-142">The `WebImage` binding code is provided in a `WebImageBinder` class that derives from `ICloudBlobStreamBinder`.</span></span>

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a><span data-ttu-id="5b6f0-143">A blob elérési út az eseményindító blob beolvasása</span><span class="sxs-lookup"><span data-stu-id="5b6f0-143">Getting the blob path for the triggering blob</span></span>
<span data-ttu-id="5b6f0-144">A tároló neve és a blob neve, amely a függvény kiváltása BLOB, közé tartozik egy `blobTrigger` karakterlánc-paramétert függvény aláírásában.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-144">To get the container name and blob name of the blob that has triggered the function, include a `blobTrigger` string parameter in the function signature.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <span data-ttu-id="5b6f0-145"><a id="poison"></a>Az elhalt blobok kezelése</span><span class="sxs-lookup"><span data-stu-id="5b6f0-145"><a id="poison"></a> How to handle poison blobs</span></span>
<span data-ttu-id="5b6f0-146">Ha egy `BlobTrigger` függvény sikertelen, az SDK meghívja az ebben az esetben, ha a hibát egy átmeneti hiba okozta.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-146">When a `BlobTrigger` function fails, the SDK calls it again, in case the failure was caused by a transient error.</span></span> <span data-ttu-id="5b6f0-147">Ha a hiba oka a blob tartalmát, a függvény minden alkalommal, amikor megpróbálja feldolgozni a blob sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-147">If the failure is caused by the content of the blob, the function fails every time it tries to process the blob.</span></span> <span data-ttu-id="5b6f0-148">Alapértelmezés szerint az SDK meghívja a függvény legfeljebb 5-ször egy adott BLOB.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-148">By default, the SDK calls a function up to 5 times for a given blob.</span></span> <span data-ttu-id="5b6f0-149">Az ötödik próbálja meghiúsul, ha az SDK-t ad hozzá egy üzenet nevű várólista *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-149">If the fifth try fails, the SDK adds a message to a queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="5b6f0-150">Az újrapróbálkozások maximális számát lehet konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-150">The maximum number of retries is configurable.</span></span> <span data-ttu-id="5b6f0-151">Azonos [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) poison blob kezelésére és a várólista elhalt üzenetek kezelésének beállítást kell használni.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-151">The same [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span> 

<span data-ttu-id="5b6f0-152">Az elhalt blobok várólista üzenet egy JSON-objektum, amely tartalmazza a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-152">The queue message for poison blobs is a JSON object that contains the following properties:</span></span>

* <span data-ttu-id="5b6f0-153">FunctionId (formátumú *{webjobs-feladat neve}*. Működik. *{Függvény neve}*, például: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="5b6f0-153">FunctionId (in the format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="5b6f0-154">BlobType ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="5b6f0-154">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="5b6f0-155">ContainerName</span><span class="sxs-lookup"><span data-stu-id="5b6f0-155">ContainerName</span></span>
* <span data-ttu-id="5b6f0-156">Blobnév</span><span class="sxs-lookup"><span data-stu-id="5b6f0-156">BlobName</span></span>
* <span data-ttu-id="5b6f0-157">ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="5b6f0-157">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="5b6f0-158">Az alábbi példakódban az `CopyBlob` függvénynek kódot, amely azt eredményezi, hogy minden alkalommal, amikor a hívott sikertelen.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-158">In the following code sample, the `CopyBlob` function has code that causes it to fail every time it's called.</span></span> <span data-ttu-id="5b6f0-159">Miután az SDK meghívja az újrapróbálkozások maximális számát, az elhalt blob várólista jön létre, egy üzenet, és üzenetet dolgoz fel a `LogPoisonBlob` függvény.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-159">After the SDK calls it for the maximum number of retries, a message is created on the poison blob queue, and that message is processed by the `LogPoisonBlob` function.</span></span> 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

<span data-ttu-id="5b6f0-160">Az SDK automatikusan deserializes a JSON-üzenet.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-160">The SDK automatically deserializes the JSON message.</span></span> <span data-ttu-id="5b6f0-161">Itt a `PoisonBlobMessage` osztály:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-161">Here is the `PoisonBlobMessage` class:</span></span> 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <span data-ttu-id="5b6f0-162"><a id="polling"></a>A BLOB lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="5b6f0-162"><a id="polling"></a> Blob polling algorithm</span></span>
<span data-ttu-id="5b6f0-163">A WebJobs SDK megvizsgálja az összes tároló által megadott `BlobTrigger` alkalmazás indításkor attribútumok.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-163">The WebJobs SDK scans all containers specified by `BlobTrigger` attributes at application start.</span></span> <span data-ttu-id="5b6f0-164">Nagy tárfiókokban Ez a vizsgálat hosszabb ideig is tarthat, így előfordulhat, hogy egy kis ideig, mielőtt új blobok találhatók és `BlobTrigger` funkciók lesznek végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-164">In a large storage account this scan can take some time, so it might be a while before new blobs are found and `BlobTrigger` functions are executed.</span></span>

<span data-ttu-id="5b6f0-165">Új vagy módosított blobok észleléséhez az alkalmazás indítása után, az SDK rendszeres időközönként olvassa be a blob storage-naplók.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-165">To detect new or changed blobs after application start, the SDK periodically reads from the blob storage logs.</span></span> <span data-ttu-id="5b6f0-166">A blob naplók pufferelve van-e, és csak fizikailag írása 10 percenként vagy Igen, ezért előfordulhat, hogy jelentős késleltetés után blob létrehozásakor vagy frissítésekor a megfelelő előtt `BlobTrigger` függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-166">The blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before the corresponding `BlobTrigger` function executes.</span></span> 

<span data-ttu-id="5b6f0-167">A blobok, amelyek használatával hoz létre kivétel történik a `Blob` attribútum.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-167">There is an exception for blobs that you create by using the `Blob` attribute.</span></span> <span data-ttu-id="5b6f0-168">Ha a WebJobs SDK hoz létre egy új blob, adja át az új blob azonnal bármely megfelelő `BlobTrigger` funkciók.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-168">When the WebJobs SDK creates a new blob, it passes the new blob immediately to any matching `BlobTrigger` functions.</span></span> <span data-ttu-id="5b6f0-169">Ezért ha fenntartása blob bemenetekhez és kimenetekhez, az SDK-t tud feldolgozni azokat hatékony.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-169">Therefore if you have a chain of blob inputs and outputs, the SDK can process them efficiently.</span></span> <span data-ttu-id="5b6f0-170">Ha azt szeretné, hogy fut a blob feldolgozási funkciók a létrehozott vagy más módon frissítve blobok kis késés, azt javasoljuk, de `QueueTrigger` helyett `BlobTrigger`.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-170">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using `QueueTrigger` rather than `BlobTrigger`.</span></span>

### <span data-ttu-id="5b6f0-171"><a id="receipts"></a>A BLOB visszaigazolások</span><span class="sxs-lookup"><span data-stu-id="5b6f0-171"><a id="receipts"></a> Blob receipts</span></span>
<span data-ttu-id="5b6f0-172">A WebJobs SDK beállításnál ellenőrizze, hogy nincs `BlobTrigger` függvény egynél többször a azonos új vagy frissített BLOB meghívása megtörténik.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-172">The WebJobs SDK makes sure that no `BlobTrigger` function gets called more than once for the same new or updated blob.</span></span> <span data-ttu-id="5b6f0-173">Ennek érdekében karbantartása *blob-visszaigazolások* annak meghatározására, ha egy adott blobverzió feldolgozása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-173">It does this by maintaining *blob receipts* in order to determine if a given blob version has been processed.</span></span>

<span data-ttu-id="5b6f0-174">A BLOB visszaigazolások nevű tárolóban vannak tárolva *azure-webjobs-állomások* AzureWebJobsStorage kapcsolati karakterlánc által meghatározott az Azure storage-fiókban.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-174">Blob receipts are stored in a container named *azure-webjobs-hosts* in the Azure storage account specified by the AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="5b6f0-175">Egy blob fogadását rendelkezik a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-175">A blob receipt has the following  information:</span></span>

* <span data-ttu-id="5b6f0-176">A BLOB hívott függvény ("*{webjobs-feladat neve}*. Működik. *{Függvény neve}*", például:"WebJob1.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="5b6f0-176">The function that was called for the blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="5b6f0-177">A tároló neve</span><span class="sxs-lookup"><span data-stu-id="5b6f0-177">The container name</span></span>
* <span data-ttu-id="5b6f0-178">A blob típusú ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="5b6f0-178">The blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="5b6f0-179">A blob neve</span><span class="sxs-lookup"><span data-stu-id="5b6f0-179">The blob name</span></span>
* <span data-ttu-id="5b6f0-180">Az ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="5b6f0-180">The ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="5b6f0-181">Ha egy blobot újrafeldolgozása kényszeríteni kívánja, a blob fogadását, hogy a BLOB manuálisan törölheti a *azure-webjobs-állomások* tároló.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-181">If you want to force reprocessing of a blob, you can manually delete the blob receipt for that blob from the *azure-webjobs-hosts* container.</span></span>

## <span data-ttu-id="5b6f0-182"><a id="queues"></a>A várólisták a cikkben említett kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="5b6f0-182"><a id="queues"></a>Related topics covered by the queues article</span></span>
<span data-ttu-id="5b6f0-183">Hogyan kezelje a blob feldolgozási sor üzenetet által indított információt, vagy a WebJobs SDK forgatókönyvek nem kizárólag a blobra feldolgozása, lásd [Azure a queue storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="5b6f0-183">For information about how to handle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific to blob processing, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="5b6f0-184">Kapcsolódó témakörök hivatkozásra, ha a cikkben ismertetett közé tartoznak a következők:</span><span class="sxs-lookup"><span data-stu-id="5b6f0-184">Related topics covered in that article include the following:</span></span>

* <span data-ttu-id="5b6f0-185">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="5b6f0-185">Async functions</span></span>
* <span data-ttu-id="5b6f0-186">Több példánya</span><span class="sxs-lookup"><span data-stu-id="5b6f0-186">Multiple instances</span></span>
* <span data-ttu-id="5b6f0-187">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="5b6f0-187">Graceful shutdown</span></span>
* <span data-ttu-id="5b6f0-188">Egy függvény törzséhez a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="5b6f0-188">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="5b6f0-189">Az SDK-kapcsolati karakterláncok beállítása a kódban.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-189">Set the SDK connection strings in code.</span></span>
* <span data-ttu-id="5b6f0-190">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="5b6f0-190">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="5b6f0-191">Konfigurálása `MaxDequeueCount` poison blob kezelésére.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-191">Configure `MaxDequeueCount` for poison blob handling.</span></span>
* <span data-ttu-id="5b6f0-192">Manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="5b6f0-192">Trigger a function manually</span></span>
* <span data-ttu-id="5b6f0-193">Naplók írása</span><span class="sxs-lookup"><span data-stu-id="5b6f0-193">Write logs</span></span>

## <span data-ttu-id="5b6f0-194"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b6f0-194"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="5b6f0-195">Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezeli az Azure-blobokkal dolgozik gyakori forgatókönyvei.</span><span class="sxs-lookup"><span data-stu-id="5b6f0-195">This guide has provided code samples that show how to handle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="5b6f0-196">Azure webjobs-feladatok és a WebJobs SDK használatával kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="5b6f0-196">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>


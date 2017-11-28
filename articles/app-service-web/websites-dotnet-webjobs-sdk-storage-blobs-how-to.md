---
title: a WebJobs SDK hello Azure blob storage aaaHow toouse
description: "Ismerje meg, hogyan toouse Azure blob-tároló hello WebJobs SDK a. Indul el egy folyamatot, ha egy új blob a tárolóban lévő jelenik meg, és \"poison blobok\" kezelni."
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
ms.openlocfilehash: b34ea8cffee7c0475641886150dee521130a3132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-with-hello-webjobs-sdk"></a><span data-ttu-id="356b6-104">Hogyan toouse Azure blob-tároló hello WebJobs SDK a</span><span class="sxs-lookup"><span data-stu-id="356b6-104">How toouse Azure blob storage with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="356b6-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="356b6-105">Overview</span></span>
<span data-ttu-id="356b6-106">Ez az útmutató C# kódot, hogy hogyan minták tootrigger egy folyamatot, ha az Azure blob létrehozásakor vagy frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="356b6-106">This guide provides C# code samples that show how tootrigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="356b6-107">hello kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.</span><span class="sxs-lookup"><span data-stu-id="356b6-107">hello code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="356b6-108">Hogyan toocreate blobok megjelenítése mintakódok, lásd: [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="356b6-108">For code samples that show how toocreate blobs, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="356b6-109">hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md) vagy túl[több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="356b6-109">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md) or too[multiple storage accounts](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).</span></span>

## <span data-ttu-id="356b6-110"><a id="trigger"></a>Hogyan tootrigger egy függvény blob létrehozásakor vagy frissítésekor</span><span class="sxs-lookup"><span data-stu-id="356b6-110"><a id="trigger"></a> How tootrigger a function when a blob is created or updated</span></span>
<span data-ttu-id="356b6-111">Ez a szakasz bemutatja, hogyan toouse hello `BlobTrigger` attribútum.</span><span class="sxs-lookup"><span data-stu-id="356b6-111">This section shows how toouse hello `BlobTrigger` attribute.</span></span> 

> [!NOTE]
> <span data-ttu-id="356b6-112">hello WebJobs SDK vizsgálatok napló fájlok toowatch új vagy módosított blobot.</span><span class="sxs-lookup"><span data-stu-id="356b6-112">hello WebJobs SDK scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="356b6-113">Ez a folyamat nincs valós idejű; egy olyan függvényt előfordulhat, hogy nem beolvasása indulnak el, amíg több percig vagy már hello blob létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="356b6-113">This process is not real-time; a function might not get triggered until several minutes or longer after hello blob is created.</span></span> <span data-ttu-id="356b6-114">Emellett [tárolási naplófájlokat hoz létre a "legjobb erőfeszítéseket"](https://msdn.microsoft.com/library/azure/hh343262.aspx) időközönként; nincs garancia, hogy az összes esemény rögzíteni kell.</span><span class="sxs-lookup"><span data-stu-id="356b6-114">In addition, [storage logs are created on a "best efforts"](https://msdn.microsoft.com/library/azure/hh343262.aspx) basis; there is no guarantee that all events will be captured.</span></span> <span data-ttu-id="356b6-115">Bizonyos körülmények között előfordulhat, hogy lehet kihagyni a naplókat.</span><span class="sxs-lookup"><span data-stu-id="356b6-115">Under some conditions, logs might be missed.</span></span> <span data-ttu-id="356b6-116">Ha hello gyorsabb és megbízhatóbb korlátozásai blob eseményindítók nem elfogadható az alkalmazáshoz, hello ajánlott módszer toocreate egy üzenetsor hello blob létrehozásakor, és használja a hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribútum helyett Hello `BlobTrigger` hello blob hello függvény attribútum.</span><span class="sxs-lookup"><span data-stu-id="356b6-116">If hello speed and reliability limitations of blob triggers are not acceptable for your application, hello recommended method is toocreate a queue message when you create hello blob, and use hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of hello `BlobTrigger` attribute on hello function that processes hello blob.</span></span>
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="356b6-117">A blob kiterjesztésű egyetlen helyőrzője</span><span class="sxs-lookup"><span data-stu-id="356b6-117">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="356b6-118">hello alábbi kódminta átmásolja a terjesztendő megjelenő szöveg blobok hello *bemeneti* tároló toohello *kimeneti* tároló:</span><span class="sxs-lookup"><span data-stu-id="356b6-118">hello following code sample copies text blobs that appear in hello *input* container toohello *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="356b6-119">hello attribútum konstruktora a hello tároló nevének és hello blob nevét megadó karakterlánc-paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="356b6-119">hello attribute constructor takes a string parameter that specifies hello container name and a placeholder for hello blob name.</span></span> <span data-ttu-id="356b6-120">Ebben a példában, ha egy blob neve *Blob1.txt* jön létre az hello *bemeneti* tároló, hello funkció hoz létre egy blobot nevű *Blob1.txt* a hello *kimeneti*  tároló.</span><span class="sxs-lookup"><span data-stu-id="356b6-120">In this example, if a blob named *Blob1.txt* is created in hello *input* container, hello function creates a blob named *Blob1.txt* in hello *output* container.</span></span> 

<span data-ttu-id="356b6-121">Megadhatja egy mintát hello blob név helyőrzője, ahogy az alábbi példakód hello:</span><span class="sxs-lookup"><span data-stu-id="356b6-121">You can specify a name pattern with hello blob name placeholder, as shown in hello following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="356b6-122">Ez a kód csak blobok kezdve "eredeti-" nevű másolja.</span><span class="sxs-lookup"><span data-stu-id="356b6-122">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="356b6-123">Például *eredeti-Blob1.txt* a hello *bemeneti* tároló túl másolódik*másolási-Blob1.txt* a hello *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="356b6-123">For example, *original-Blob1.txt* in hello *input* container is copied too*copy-Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="356b6-124">Ha egy minta toospecify blob neveivel, amelyeknek kapcsos zárójelek hello neve, dupla hello kapcsos zárójelek.</span><span class="sxs-lookup"><span data-stu-id="356b6-124">If you need toospecify a name pattern for blob names that have curly braces in hello name, double hello curly braces.</span></span> <span data-ttu-id="356b6-125">Például, ha azt szeretné, hogy toofind blobot, amely hello *képek* ilyen nevű tárolóban:</span><span class="sxs-lookup"><span data-stu-id="356b6-125">For example, if you want toofind blobs in hello *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="356b6-126">Ezt a mintát használja:</span><span class="sxs-lookup"><span data-stu-id="356b6-126">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="356b6-127">Hello példában hello *neve* helyőrző értéke lenne *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="356b6-127">In hello example, hello *name* placeholder value would be *soundfile.mp3*.</span></span> 

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="356b6-128">Külön blob-névnek és kiterjesztésnek helyőrzők</span><span class="sxs-lookup"><span data-stu-id="356b6-128">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="356b6-129">hello következő minta kódmódosításokat hello fájlkiterjesztés, a blobok, amelyek szerepelnek a hello átmásolja *bemeneti* tároló toohello *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="356b6-129">hello following code sample changes hello file extension as it copies blobs that appear in hello *input* container toohello *output* container.</span></span> <span data-ttu-id="356b6-130">hello kód naplózza hello hello kiterjesztését *bemeneti* blob-, és beállítja a hello hello kiterjesztését *kimeneti* blob túl*.txt*.</span><span class="sxs-lookup"><span data-stu-id="356b6-130">hello code logs hello extension of hello *input* blob and sets hello extension of hello *output* blob too*.txt*.</span></span>

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

## <span data-ttu-id="356b6-131"><a id="types"></a>Hogy köthető tooblobs típusok</span><span class="sxs-lookup"><span data-stu-id="356b6-131"><a id="types"></a> Types that you can bind tooblobs</span></span>
<span data-ttu-id="356b6-132">Használhatja a hello `BlobTrigger` hello típusa a következő attribútumot:</span><span class="sxs-lookup"><span data-stu-id="356b6-132">You can use hello `BlobTrigger` attribute on hello following types:</span></span>

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
* <span data-ttu-id="356b6-133">Egyéb típusú általi [ICloudBlobStreamBinder](#icbsb)</span><span class="sxs-lookup"><span data-stu-id="356b6-133">Other types deserialized by [ICloudBlobStreamBinder](#icbsb)</span></span> 

<span data-ttu-id="356b6-134">Ha azt szeretné, közvetlenül az Azure storage-fiók hello toowork, azt is megteheti egy `CloudStorageAccount` paraméter toohello metódus-aláírás.</span><span class="sxs-lookup"><span data-stu-id="356b6-134">If you want toowork directly with hello Azure storage account, you can also add a `CloudStorageAccount` parameter toohello method signature.</span></span>

<span data-ttu-id="356b6-135">Tekintse meg a hello [blob-kötés kód hello azure-webjobs-sdk-tárházban github.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span><span class="sxs-lookup"><span data-stu-id="356b6-135">For examples, see hello [blob binding code in hello azure-webjobs-sdk repository on GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).</span></span>

## <span data-ttu-id="356b6-136"><a id="string"></a>Kötési toostring beolvasásakor szöveges blob tartalom</span><span class="sxs-lookup"><span data-stu-id="356b6-136"><a id="string"></a> Getting text blob content by binding toostring</span></span>
<span data-ttu-id="356b6-137">Ha a szöveges BLOB várható, `BlobTrigger` lehet alkalmazott tooa `string` paraméter.</span><span class="sxs-lookup"><span data-stu-id="356b6-137">If text blobs are expected, `BlobTrigger` can be applied tooa `string` parameter.</span></span> <span data-ttu-id="356b6-138">hello alábbi kódminta köti a szöveges blob tooa `string` nevű paraméter `logMessage`.</span><span class="sxs-lookup"><span data-stu-id="356b6-138">hello following code sample binds a text blob tooa `string` parameter named `logMessage`.</span></span> <span data-ttu-id="356b6-139">hello függvény adott hello blob toohello WebJobs SDK irányítópult paraméter toowrite hello tartalmát használja.</span><span class="sxs-lookup"><span data-stu-id="356b6-139">hello function uses that parameter toowrite hello contents of hello blob toohello WebJobs SDK dashboard.</span></span> 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <span data-ttu-id="356b6-140"><a id="icbsb"></a>Első szerializált blob tartalma ICloudBlobStreamBinder használatával</span><span class="sxs-lookup"><span data-stu-id="356b6-140"><a id="icbsb"></a> Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="356b6-141">hello alábbi kódminta használ, amely megvalósítja az osztály `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` attribútum egy blob toohello toobind `WebImage` típusa.</span><span class="sxs-lookup"><span data-stu-id="356b6-141">hello following code sample uses a class that implements `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` attribute toobind a blob toohello `WebImage` type.</span></span>

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

<span data-ttu-id="356b6-142">Hello `WebImage` kötés kód megtalálható egy `WebImageBinder` abból származó osztály `ICloudBlobStreamBinder`.</span><span class="sxs-lookup"><span data-stu-id="356b6-142">hello `WebImage` binding code is provided in a `WebImageBinder` class that derives from `ICloudBlobStreamBinder`.</span></span>

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

## <a name="getting-hello-blob-path-for-hello-triggering-blob"></a><span data-ttu-id="356b6-143">Hello blob elérési út hello időt. a blob beolvasása</span><span class="sxs-lookup"><span data-stu-id="356b6-143">Getting hello blob path for hello triggering blob</span></span>
<span data-ttu-id="356b6-144">tooget hello tároló neve és a blob neve, amely kiváltása hello függvény hello BLOB tartalmaznak egy `blobTrigger` karakterlánc-paramétert a hello függvény aláírásához.</span><span class="sxs-lookup"><span data-stu-id="356b6-144">tooget hello container name and blob name of hello blob that has triggered hello function, include a `blobTrigger` string parameter in hello function signature.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <span data-ttu-id="356b6-145"><a id="poison"></a>Hogyan toohandle poison blobok</span><span class="sxs-lookup"><span data-stu-id="356b6-145"><a id="poison"></a> How toohandle poison blobs</span></span>
<span data-ttu-id="356b6-146">Ha egy `BlobTrigger` függvény sikertelen, hello SDK meghívja az ebben az esetben, ha hello hibát egy átmeneti hiba okozta.</span><span class="sxs-lookup"><span data-stu-id="356b6-146">When a `BlobTrigger` function fails, hello SDK calls it again, in case hello failure was caused by a transient error.</span></span> <span data-ttu-id="356b6-147">Hello hiba okozta hello blob hello tartalmát, hello függvény sikertelen, minden alkalommal, amikor megkísérli tooprocess hello blob.</span><span class="sxs-lookup"><span data-stu-id="356b6-147">If hello failure is caused by hello content of hello blob, hello function fails every time it tries tooprocess hello blob.</span></span> <span data-ttu-id="356b6-148">Alapértelmezés szerint hello SDK hívásokat too5 alkalommal be a következő függvényt egy adott blob.</span><span class="sxs-lookup"><span data-stu-id="356b6-148">By default, hello SDK calls a function up too5 times for a given blob.</span></span> <span data-ttu-id="356b6-149">Hello ötödik próbálja meghibásodásakor hello SDK ad hozzá egy tooa üzenetsor nevű *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="356b6-149">If hello fifth try fails, hello SDK adds a message tooa queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="356b6-150">Az újrapróbálkozások maximális számát hello lehet konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="356b6-150">hello maximum number of retries is configurable.</span></span> <span data-ttu-id="356b6-151">hello azonos [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) poison blob kezelésére és a várólista elhalt üzenetek kezelésének beállítást kell használni.</span><span class="sxs-lookup"><span data-stu-id="356b6-151">hello same [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span> 

<span data-ttu-id="356b6-152">az elhalt blobok várólista üdvözlőüzenetére egy JSON-objektum, amely tartalmazza a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="356b6-152">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="356b6-153">FunctionId (hello formátumban *{webjobs-feladat neve}*. Működik. *{Függvény neve}*, például: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="356b6-153">FunctionId (in hello format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="356b6-154">BlobType ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="356b6-154">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="356b6-155">ContainerName</span><span class="sxs-lookup"><span data-stu-id="356b6-155">ContainerName</span></span>
* <span data-ttu-id="356b6-156">Blobnév</span><span class="sxs-lookup"><span data-stu-id="356b6-156">BlobName</span></span>
* <span data-ttu-id="356b6-157">ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="356b6-157">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="356b6-158">Hello alábbi mintakód, hello `CopyBlob` függvénynek kód kiváltó toofail minden alkalommal, amikor azt nevezzük.</span><span class="sxs-lookup"><span data-stu-id="356b6-158">In hello following code sample, hello `CopyBlob` function has code that causes it toofail every time it's called.</span></span> <span data-ttu-id="356b6-159">Miután hello SDK meghívja a hello az újrapróbálkozások maximális számát, hello poison blob várólista jön létre, egy üzenet, és üzenetet dolgoz fel hello `LogPoisonBlob` függvény.</span><span class="sxs-lookup"><span data-stu-id="356b6-159">After hello SDK calls it for hello maximum number of retries, a message is created on hello poison blob queue, and that message is processed by hello `LogPoisonBlob` function.</span></span> 

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

<span data-ttu-id="356b6-160">hello SDK automatikusan deserializes üdvözlőüzenetére JSON.</span><span class="sxs-lookup"><span data-stu-id="356b6-160">hello SDK automatically deserializes hello JSON message.</span></span> <span data-ttu-id="356b6-161">Íme hello `PoisonBlobMessage` osztály:</span><span class="sxs-lookup"><span data-stu-id="356b6-161">Here is hello `PoisonBlobMessage` class:</span></span> 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <span data-ttu-id="356b6-162"><a id="polling"></a>A BLOB lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="356b6-162"><a id="polling"></a> Blob polling algorithm</span></span>
<span data-ttu-id="356b6-163">hello WebJobs SDK megvizsgálja az összes tároló által megadott `BlobTrigger` alkalmazás indításkor attribútumok.</span><span class="sxs-lookup"><span data-stu-id="356b6-163">hello WebJobs SDK scans all containers specified by `BlobTrigger` attributes at application start.</span></span> <span data-ttu-id="356b6-164">Nagy tárfiókokban Ez a vizsgálat hosszabb ideig is tarthat, így előfordulhat, hogy egy kis ideig, mielőtt új blobok találhatók és `BlobTrigger` funkciók lesznek végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="356b6-164">In a large storage account this scan can take some time, so it might be a while before new blobs are found and `BlobTrigger` functions are executed.</span></span>

<span data-ttu-id="356b6-165">naplózza a SDK rendszeres időközönként olvassa be az hello blob-tároló hello toodetect új vagy módosított blobokkal alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="356b6-165">toodetect new or changed blobs after application start, hello SDK periodically reads from hello blob storage logs.</span></span> <span data-ttu-id="356b6-166">hello blob naplók pufferelve van-e, és csak fizikailag írása 10 percenként vagy Igen, vagyis jelentős késleltetés után blob létrehozásakor vagy frissítésekor előtt hello megfelelő `BlobTrigger` függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="356b6-166">hello blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before hello corresponding `BlobTrigger` function executes.</span></span> 

<span data-ttu-id="356b6-167">A blobok hello segítségével létrehozott kivétel `Blob` attribútum.</span><span class="sxs-lookup"><span data-stu-id="356b6-167">There is an exception for blobs that you create by using hello `Blob` attribute.</span></span> <span data-ttu-id="356b6-168">Amikor hello WebJobs SDK létrehoz egy új blob, átadja hello új blob azonnal tooany megfelelő `BlobTrigger` funkciók.</span><span class="sxs-lookup"><span data-stu-id="356b6-168">When hello WebJobs SDK creates a new blob, it passes hello new blob immediately tooany matching `BlobTrigger` functions.</span></span> <span data-ttu-id="356b6-169">Ezért ha fenntartása blob bemenetekhez és kimenetekhez, hello SDK segítségével dolgozza fel őket hatékony.</span><span class="sxs-lookup"><span data-stu-id="356b6-169">Therefore if you have a chain of blob inputs and outputs, hello SDK can process them efficiently.</span></span> <span data-ttu-id="356b6-170">Ha azt szeretné, hogy fut a blob feldolgozási funkciók a létrehozott vagy más módon frissítve blobok kis késés, azt javasoljuk, de `QueueTrigger` helyett `BlobTrigger`.</span><span class="sxs-lookup"><span data-stu-id="356b6-170">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using `QueueTrigger` rather than `BlobTrigger`.</span></span>

### <span data-ttu-id="356b6-171"><a id="receipts"></a>A BLOB visszaigazolások</span><span class="sxs-lookup"><span data-stu-id="356b6-171"><a id="receipts"></a> Blob receipts</span></span>
<span data-ttu-id="356b6-172">hello WebJobs SDK beállításnál ellenőrizze, hogy nincs `BlobTrigger` függvény menüelemnek egynél többször a hello azonos új vagy frissített blob.</span><span class="sxs-lookup"><span data-stu-id="356b6-172">hello WebJobs SDK makes sure that no `BlobTrigger` function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="356b6-173">Ennek érdekében karbantartása *blob-visszaigazolások* a rendelés toodetermine, ha egy adott blobverzió feldolgozása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="356b6-173">It does this by maintaining *blob receipts* in order toodetermine if a given blob version has been processed.</span></span>

<span data-ttu-id="356b6-174">BLOB visszaigazolások nevű tárolóban vannak tárolva *azure-webjobs-állomások* hello AzureWebJobsStorage kapcsolati karakterlánc által meghatározott hello az Azure storage-fiókban.</span><span class="sxs-lookup"><span data-stu-id="356b6-174">Blob receipts are stored in a container named *azure-webjobs-hosts* in hello Azure storage account specified by hello AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="356b6-175">Egy blob fogadását a következő információk hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="356b6-175">A blob receipt has hello following  information:</span></span>

* <span data-ttu-id="356b6-176">hello hello BLOB hívott függvény ("*{webjobs-feladat neve}*. Működik. *{Függvény neve}*", például:"WebJob1.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="356b6-176">hello function that was called for hello blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="356b6-177">hello tároló neve</span><span class="sxs-lookup"><span data-stu-id="356b6-177">hello container name</span></span>
* <span data-ttu-id="356b6-178">hello blob típushoz ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="356b6-178">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="356b6-179">hello blob neve</span><span class="sxs-lookup"><span data-stu-id="356b6-179">hello blob name</span></span>
* <span data-ttu-id="356b6-180">hello ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="356b6-180">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="356b6-181">Ha azt szeretné, hogy tooforce újrafeldolgozása blob, manuálisan törölheti, hogy a blob hello blob visszaigazolás hello *azure-webjobs-állomások* tároló.</span><span class="sxs-lookup"><span data-stu-id="356b6-181">If you want tooforce reprocessing of a blob, you can manually delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container.</span></span>

## <span data-ttu-id="356b6-182"><a id="queues"></a>Hello várólisták cikkben említett kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="356b6-182"><a id="queues"></a>Related topics covered by hello queues article</span></span>
<span data-ttu-id="356b6-183">Hogyan toohandle blob feldolgozási elindul egy üzenetsor-üzenetet, vagy a WebJobs SDK forgatókönyvek nem adott tooblob feldolgozása, lásd: [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="356b6-183">For information about how toohandle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific tooblob processing, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="356b6-184">Kapcsolódó témakörök hivatkozásra, ha a cikkben ismertetett hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="356b6-184">Related topics covered in that article include hello following:</span></span>

* <span data-ttu-id="356b6-185">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="356b6-185">Async functions</span></span>
* <span data-ttu-id="356b6-186">Több példánya</span><span class="sxs-lookup"><span data-stu-id="356b6-186">Multiple instances</span></span>
* <span data-ttu-id="356b6-187">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="356b6-187">Graceful shutdown</span></span>
* <span data-ttu-id="356b6-188">Egy függvény törzséhez hello a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="356b6-188">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="356b6-189">Hello SDK kapcsolati karakterláncok beállítása a kódban.</span><span class="sxs-lookup"><span data-stu-id="356b6-189">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="356b6-190">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="356b6-190">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="356b6-191">Konfigurálása `MaxDequeueCount` poison blob kezelésére.</span><span class="sxs-lookup"><span data-stu-id="356b6-191">Configure `MaxDequeueCount` for poison blob handling.</span></span>
* <span data-ttu-id="356b6-192">Manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="356b6-192">Trigger a function manually</span></span>
* <span data-ttu-id="356b6-193">Naplók írása</span><span class="sxs-lookup"><span data-stu-id="356b6-193">Write logs</span></span>

## <span data-ttu-id="356b6-194"><a id="nextsteps"></a> Következő lépések</span><span class="sxs-lookup"><span data-stu-id="356b6-194"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="356b6-195">Ez az útmutató nyújtott mintakódok, hogy hogyan toohandle gyakori helyzetek Azure végzett munka blobok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="356b6-195">This guide has provided code samples that show how toohandle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="356b6-196">További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="356b6-196">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>


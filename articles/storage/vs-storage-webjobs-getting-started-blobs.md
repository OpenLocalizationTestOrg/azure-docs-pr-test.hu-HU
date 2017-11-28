---
title: "a blob storage és a Visual Studio (webjobs-feladat projektek) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Szolgáltatások indításának a Blob storage használatával webjobs-feladat projektben csatlakoztassa a Visual Studio segítségével az Azure storage tooan tooget csatlakoztatva."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: cb2cecacbb1a59f093d5453a91fae33e0f279d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a><span data-ttu-id="098ca-103">Ismerkedés az Azure Blob storage és a Visual Studio csatlakoztatva (webjobs-feladat projektek) szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="098ca-103">Get started with Azure Blob storage and Visual Studio connected services (WebJob projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="098ca-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="098ca-104">Overview</span></span>
<span data-ttu-id="098ca-105">Ez a cikk ismerteti a C# kódot, hogy hogyan minták tootrigger egy folyamatot, ha az Azure blob létrehozásakor vagy frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="098ca-105">This article provides C# code samples that show how tootrigger a process when an Azure blob is created or updated.</span></span> <span data-ttu-id="098ca-106">Kódminták hello használata hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verzió 1.x.</span><span class="sxs-lookup"><span data-stu-id="098ca-106">hello code samples use hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) version 1.x.</span></span> <span data-ttu-id="098ca-107">Ha a tárolási fiók tooa webjobs-feladat projekt hozzáadása hello Visual Studio használatával **kapcsolódó szolgáltatások hozzáadása** párbeszédpanelen hello megfelelő Azure Storage NuGet-csomagja telepítve van, hello megfelelő .NET hivatkozások hozzáadott toohello projekt és hello tárfiók kapcsolati karakterláncok frissítése hello App.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="098ca-107">When you add a storage account tooa WebJob project by using hello Visual Studio **Add Connected Services** dialog, hello appropriate Azure Storage NuGet package is installed, hello appropriate .NET references are added toohello project, and connection strings for hello storage account are updated in hello App.config file.</span></span>

## <a name="how-tootrigger-a-function-when-a-blob-is-created-or-updated"></a><span data-ttu-id="098ca-108">Hogyan tootrigger egy függvény blob létrehozásakor vagy frissítésekor</span><span class="sxs-lookup"><span data-stu-id="098ca-108">How tootrigger a function when a blob is created or updated</span></span>
<span data-ttu-id="098ca-109">Ez a szakasz bemutatja, hogyan toouse hello **BlobTrigger** attribútum.</span><span class="sxs-lookup"><span data-stu-id="098ca-109">This section shows how toouse hello **BlobTrigger** attribute.</span></span>

 <span data-ttu-id="098ca-110">**Megjegyzés:** hello WebJobs SDK vizsgálatok napló fájlok toowatch új vagy módosított blobot.</span><span class="sxs-lookup"><span data-stu-id="098ca-110">**Note:** hello WebJobs SDK scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="098ca-111">A folyamat be nem eredendően lassú; egy olyan függvényt előfordulhat, hogy nem beolvasása indulnak el, amíg több percig vagy már hello blob létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="098ca-111">This process is inherently slow; a function might not get triggered until several minutes or longer after hello blob is created.</span></span>  <span data-ttu-id="098ca-112">Ha az alkalmazásnak tooprocess blobok azonnal, hello ajánlott módszer toocreate egy üzenetsor hello blob létrehozásakor, és használja a hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribútum helyett hello **BlobTrigger** hello blob hello függvény attribútum.</span><span class="sxs-lookup"><span data-stu-id="098ca-112">If your application needs tooprocess blobs immediately, hello recommended method is toocreate a queue message when you create hello blob, and use hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribute instead of hello **BlobTrigger** attribute on hello function that processes hello blob.</span></span>

### <a name="single-placeholder-for-blob-name-with-extension"></a><span data-ttu-id="098ca-113">A blob kiterjesztésű egyetlen helyőrzője</span><span class="sxs-lookup"><span data-stu-id="098ca-113">Single placeholder for blob name with extension</span></span>
<span data-ttu-id="098ca-114">hello alábbi kódminta átmásolja a terjesztendő megjelenő szöveg blobok hello *bemeneti* tároló toohello *kimeneti* tároló:</span><span class="sxs-lookup"><span data-stu-id="098ca-114">hello following code sample copies text blobs that appear in hello *input* container toohello *output* container:</span></span>

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="098ca-115">hello attribútum konstruktora a hello tároló nevének és hello blob nevét megadó karakterlánc-paramétert fogad.</span><span class="sxs-lookup"><span data-stu-id="098ca-115">hello attribute constructor takes a string parameter that specifies hello container name and a placeholder for hello blob name.</span></span> <span data-ttu-id="098ca-116">Ebben a példában, ha egy blob neve *Blob1.txt* jön létre az hello *bemeneti* tároló, hello funkció hoz létre egy blobot nevű *Blob1.txt* a hello *kimeneti*  tároló.</span><span class="sxs-lookup"><span data-stu-id="098ca-116">In this example, if a blob named *Blob1.txt* is created in hello *input* container, hello function creates a blob named *Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="098ca-117">Megadhatja egy mintát hello blob név helyőrzője, ahogy az alábbi példakód hello:</span><span class="sxs-lookup"><span data-stu-id="098ca-117">You can specify a name pattern with hello blob name placeholder, as shown in hello following code sample:</span></span>

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

<span data-ttu-id="098ca-118">Ez a kód csak blobok kezdve "eredeti-" nevű másolja.</span><span class="sxs-lookup"><span data-stu-id="098ca-118">This code copies only blobs that have names beginning with "original-".</span></span> <span data-ttu-id="098ca-119">Például *eredeti-Blob1.txt* a hello *bemeneti* tároló túl másolódik*másolási-Blob1.txt* a hello *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="098ca-119">For example, *original-Blob1.txt* in hello *input* container is copied too*copy-Blob1.txt* in hello *output* container.</span></span>

<span data-ttu-id="098ca-120">Ha egy minta toospecify blob neveivel, amelyeknek kapcsos zárójelek hello neve, dupla hello kapcsos zárójelek.</span><span class="sxs-lookup"><span data-stu-id="098ca-120">If you need toospecify a name pattern for blob names that have curly braces in hello name, double hello curly braces.</span></span> <span data-ttu-id="098ca-121">Például, ha azt szeretné, hogy toofind blobot, amely hello *képek* ilyen nevű tárolóban:</span><span class="sxs-lookup"><span data-stu-id="098ca-121">For example, if you want toofind blobs in hello *images* container that have names like this:</span></span>

        {20140101}-soundfile.mp3

<span data-ttu-id="098ca-122">Ezt a mintát használja:</span><span class="sxs-lookup"><span data-stu-id="098ca-122">use this for your pattern:</span></span>

        images/{{20140101}}-{name}

<span data-ttu-id="098ca-123">Hello példában hello *neve* helyőrző értéke lenne *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="098ca-123">In hello example, hello *name* placeholder value would be *soundfile.mp3*.</span></span>

### <a name="separate-blob-name-and-extension-placeholders"></a><span data-ttu-id="098ca-124">Külön blob-névnek és kiterjesztésnek helyőrzők</span><span class="sxs-lookup"><span data-stu-id="098ca-124">Separate blob name and extension placeholders</span></span>
<span data-ttu-id="098ca-125">hello következő minta kódmódosításokat hello fájlkiterjesztés, a blobok, amelyek szerepelnek a hello átmásolja *bemeneti* tároló toohello *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="098ca-125">hello following code sample changes hello file extension as it copies blobs that appear in hello *input* container toohello *output* container.</span></span> <span data-ttu-id="098ca-126">hello kód naplózza hello hello kiterjesztését *bemeneti* blob-, és beállítja a hello hello kiterjesztését *kimeneti* blob túl*.txt*.</span><span class="sxs-lookup"><span data-stu-id="098ca-126">hello code logs hello extension of hello *input* blob and sets hello extension of hello *output* blob too*.txt*.</span></span>

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

## <a name="types-that-you-can-bind-tooblobs"></a><span data-ttu-id="098ca-127">Hogy köthető tooblobs típusok</span><span class="sxs-lookup"><span data-stu-id="098ca-127">Types that you can bind tooblobs</span></span>
<span data-ttu-id="098ca-128">Használhatja a hello **BlobTrigger** hello típusa a következő attribútumot:</span><span class="sxs-lookup"><span data-stu-id="098ca-128">You can use hello **BlobTrigger** attribute on hello following types:</span></span>

* <span data-ttu-id="098ca-129">**karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="098ca-129">**string**</span></span>
* <span data-ttu-id="098ca-130">**TextReader**</span><span class="sxs-lookup"><span data-stu-id="098ca-130">**TextReader**</span></span>
* <span data-ttu-id="098ca-131">**Az adatfolyam**</span><span class="sxs-lookup"><span data-stu-id="098ca-131">**Stream**</span></span>
* <span data-ttu-id="098ca-132">**ICloudBlob**</span><span class="sxs-lookup"><span data-stu-id="098ca-132">**ICloudBlob**</span></span>
* <span data-ttu-id="098ca-133">**CloudBlockBlob**</span><span class="sxs-lookup"><span data-stu-id="098ca-133">**CloudBlockBlob**</span></span>
* <span data-ttu-id="098ca-134">**CloudPageBlob**</span><span class="sxs-lookup"><span data-stu-id="098ca-134">**CloudPageBlob**</span></span>
* <span data-ttu-id="098ca-135">Egyéb típusú általi [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span><span class="sxs-lookup"><span data-stu-id="098ca-135">Other types deserialized by [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)</span></span>

<span data-ttu-id="098ca-136">Ha azt szeretné, közvetlenül az Azure storage-fiók hello toowork, azt is megteheti egy **CloudStorageAccount** paraméter toohello metódus-aláírás.</span><span class="sxs-lookup"><span data-stu-id="098ca-136">If you want toowork directly with hello Azure storage account, you can also add a **CloudStorageAccount** parameter toohello method signature.</span></span>

## <a name="getting-text-blob-content-by-binding-toostring"></a><span data-ttu-id="098ca-137">Kötési toostring beolvasásakor szöveges blob tartalom</span><span class="sxs-lookup"><span data-stu-id="098ca-137">Getting text blob content by binding toostring</span></span>
<span data-ttu-id="098ca-138">Ha a szöveges BLOB várható, **BlobTrigger** lehet alkalmazott tooa **karakterlánc** paraméter.</span><span class="sxs-lookup"><span data-stu-id="098ca-138">If text blobs are expected, **BlobTrigger** can be applied tooa **string** parameter.</span></span> <span data-ttu-id="098ca-139">hello alábbi kódminta köti a szöveges blob tooa **karakterlánc** nevű paraméter **logMessage**.</span><span class="sxs-lookup"><span data-stu-id="098ca-139">hello following code sample binds a text blob tooa **string** parameter named **logMessage**.</span></span> <span data-ttu-id="098ca-140">hello függvény adott hello blob toohello WebJobs SDK irányítópult paraméter toowrite hello tartalmát használja.</span><span class="sxs-lookup"><span data-stu-id="098ca-140">hello function uses that parameter toowrite hello contents of hello blob toohello WebJobs SDK dashboard.</span></span>

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a><span data-ttu-id="098ca-141">Első szerializált blob tartalma ICloudBlobStreamBinder használatával</span><span class="sxs-lookup"><span data-stu-id="098ca-141">Getting serialized blob content by using ICloudBlobStreamBinder</span></span>
<span data-ttu-id="098ca-142">hello alábbi kódminta használ, amely megvalósítja az osztály **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** attribútum egy blob toohello toobind **WebImage** típusa.</span><span class="sxs-lookup"><span data-stu-id="098ca-142">hello following code sample uses a class that implements **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** attribute toobind a blob toohello **WebImage** type.</span></span>

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

<span data-ttu-id="098ca-143">Hello **WebImage** kötés kód megtalálható egy **WebImageBinder** abból származó osztály **ICloudBlobStreamBinder**.</span><span class="sxs-lookup"><span data-stu-id="098ca-143">hello **WebImage** binding code is provided in a **WebImageBinder** class that derives from **ICloudBlobStreamBinder**.</span></span>

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

## <a name="how-toohandle-poison-blobs"></a><span data-ttu-id="098ca-144">Hogyan toohandle poison blobok</span><span class="sxs-lookup"><span data-stu-id="098ca-144">How toohandle poison blobs</span></span>
<span data-ttu-id="098ca-145">Ha egy **BlobTrigger** függvény sikertelen, hello SDK meghívja az ebben az esetben, ha hello hibát egy átmeneti hiba okozta.</span><span class="sxs-lookup"><span data-stu-id="098ca-145">When a **BlobTrigger** function fails, hello SDK calls it again, in case hello failure was caused by a transient error.</span></span> <span data-ttu-id="098ca-146">Hello hiba okozta hello blob hello tartalmát, hello függvény sikertelen, minden alkalommal, amikor megkísérli tooprocess hello blob.</span><span class="sxs-lookup"><span data-stu-id="098ca-146">If hello failure is caused by hello content of hello blob, hello function fails every time it tries tooprocess hello blob.</span></span> <span data-ttu-id="098ca-147">Alapértelmezés szerint hello SDK hívásokat too5 alkalommal be a következő függvényt egy adott blob.</span><span class="sxs-lookup"><span data-stu-id="098ca-147">By default, hello SDK calls a function up too5 times for a given blob.</span></span> <span data-ttu-id="098ca-148">Hello ötödik próbálja meghibásodásakor hello SDK ad hozzá egy tooa üzenetsor nevű *webjobs-blobtrigger-poison*.</span><span class="sxs-lookup"><span data-stu-id="098ca-148">If hello fifth try fails, hello SDK adds a message tooa queue named *webjobs-blobtrigger-poison*.</span></span>

<span data-ttu-id="098ca-149">Az újrapróbálkozások maximális számát hello lehet konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="098ca-149">hello maximum number of retries is configurable.</span></span> <span data-ttu-id="098ca-150">hello azonos [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) poison blob kezelésére és a várólista elhalt üzenetek kezelésének beállítást kell használni.</span><span class="sxs-lookup"><span data-stu-id="098ca-150">hello same [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) setting is used for poison blob handling and poison queue message handling.</span></span>

<span data-ttu-id="098ca-151">az elhalt blobok várólista üdvözlőüzenetére egy JSON-objektum, amely tartalmazza a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="098ca-151">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="098ca-152">FunctionId (hello formátumban *{webjobs-feladat neve}*. Működik. *{Függvény neve}*, például: WebJob1.Functions.CopyBlob)</span><span class="sxs-lookup"><span data-stu-id="098ca-152">FunctionId (in hello format *{WebJob name}*.Functions.*{Function name}*, for example: WebJob1.Functions.CopyBlob)</span></span>
* <span data-ttu-id="098ca-153">BlobType ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="098ca-153">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="098ca-154">ContainerName</span><span class="sxs-lookup"><span data-stu-id="098ca-154">ContainerName</span></span>
* <span data-ttu-id="098ca-155">Blobnév</span><span class="sxs-lookup"><span data-stu-id="098ca-155">BlobName</span></span>
* <span data-ttu-id="098ca-156">ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="098ca-156">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="098ca-157">Hello alábbi mintakód, hello **CopyBlob** függvénynek kód kiváltó toofail minden alkalommal, amikor azt nevezzük.</span><span class="sxs-lookup"><span data-stu-id="098ca-157">In hello following code sample, hello **CopyBlob** function has code that causes it toofail every time it's called.</span></span> <span data-ttu-id="098ca-158">Miután hello SDK meghívja a hello az újrapróbálkozások maximális számát, hello poison blob várólista jön létre, egy üzenet, és üzenetet dolgoz fel hello **LogPoisonBlob** függvény.</span><span class="sxs-lookup"><span data-stu-id="098ca-158">After hello SDK calls it for hello maximum number of retries, a message is created on hello poison blob queue, and that message is processed by hello **LogPoisonBlob** function.</span></span>

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

<span data-ttu-id="098ca-159">hello SDK automatikusan deserializes üdvözlőüzenetére JSON.</span><span class="sxs-lookup"><span data-stu-id="098ca-159">hello SDK automatically deserializes hello JSON message.</span></span> <span data-ttu-id="098ca-160">Íme hello **PoisonBlobMessage** osztály:</span><span class="sxs-lookup"><span data-stu-id="098ca-160">Here is hello **PoisonBlobMessage** class:</span></span>

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a><span data-ttu-id="098ca-161">A BLOB lekérdezési algoritmus</span><span class="sxs-lookup"><span data-stu-id="098ca-161">Blob polling algorithm</span></span>
<span data-ttu-id="098ca-162">hello WebJobs SDK megvizsgálja az összes tároló által megadott **BlobTrigger** alkalmazás indításkor attribútumok.</span><span class="sxs-lookup"><span data-stu-id="098ca-162">hello WebJobs SDK scans all containers specified by **BlobTrigger** attributes at application start.</span></span> <span data-ttu-id="098ca-163">Nagy tárfiókokban Ez a vizsgálat hosszabb ideig is tarthat, így előfordulhat, hogy egy kis ideig, mielőtt új blobok találhatók és **BlobTrigger** funkciókat hajtja végre a rendszer.</span><span class="sxs-lookup"><span data-stu-id="098ca-163">In a large storage account this scan can take some time, so it might be a while before new blobs are found and **BlobTrigger** functions are executed.</span></span>

<span data-ttu-id="098ca-164">naplózza a SDK rendszeres időközönként olvassa be az hello blob-tároló hello toodetect új vagy módosított blobokkal alkalmazás indítása után.</span><span class="sxs-lookup"><span data-stu-id="098ca-164">toodetect new or changed blobs after application start, hello SDK periodically reads from hello blob storage logs.</span></span> <span data-ttu-id="098ca-165">hello blob naplók pufferelve van-e, és csak fizikailag írása 10 percenként vagy Igen, vagyis jelentős késleltetés után blob létrehozásakor vagy frissítésekor előtt hello megfelelő **BlobTrigger** függvény végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="098ca-165">hello blob logs are buffered and only get physically written every 10 minutes or so, so there may be significant delay after a blob is created or updated before hello corresponding **BlobTrigger** function executes.</span></span>

<span data-ttu-id="098ca-166">A blobok hello segítségével létrehozott kivétel **Blob** attribútum.</span><span class="sxs-lookup"><span data-stu-id="098ca-166">There is an exception for blobs that you create by using hello **Blob** attribute.</span></span> <span data-ttu-id="098ca-167">Amikor hello WebJobs SDK létrehoz egy új blob, átadja hello új blob azonnal tooany megfelelő **BlobTrigger** funkciók.</span><span class="sxs-lookup"><span data-stu-id="098ca-167">When hello WebJobs SDK creates a new blob, it passes hello new blob immediately tooany matching **BlobTrigger** functions.</span></span> <span data-ttu-id="098ca-168">Ezért ha fenntartása blob bemenetekhez és kimenetekhez, hello SDK segítségével dolgozza fel őket hatékony.</span><span class="sxs-lookup"><span data-stu-id="098ca-168">Therefore if you have a chain of blob inputs and outputs, hello SDK can process them efficiently.</span></span> <span data-ttu-id="098ca-169">Ha azt szeretné, hogy fut a blob feldolgozási funkciók a létrehozott vagy más módon frissítve blobok kis késés, azt javasoljuk, de **QueueTrigger** helyett **BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="098ca-169">But if you want low latency running your blob processing functions for blobs that are created or updated by other means, we recommend using **QueueTrigger** rather than **BlobTrigger**.</span></span>

### <a name="blob-receipts"></a><span data-ttu-id="098ca-170">A BLOB visszaigazolások</span><span class="sxs-lookup"><span data-stu-id="098ca-170">Blob receipts</span></span>
<span data-ttu-id="098ca-171">hello WebJobs SDK beállításnál ellenőrizze, hogy nincs **BlobTrigger** függvény menüelemnek egynél többször a hello azonos új vagy frissített blob.</span><span class="sxs-lookup"><span data-stu-id="098ca-171">hello WebJobs SDK makes sure that no **BlobTrigger** function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="098ca-172">Ennek érdekében karbantartása *blob-visszaigazolások* a rendelés toodetermine, ha egy adott blobverzió feldolgozása megtörtént.</span><span class="sxs-lookup"><span data-stu-id="098ca-172">It does this by maintaining *blob receipts* in order toodetermine if a given blob version has been processed.</span></span>

<span data-ttu-id="098ca-173">BLOB visszaigazolások nevű tárolóban vannak tárolva *azure-webjobs-állomások* hello AzureWebJobsStorage kapcsolati karakterlánc által meghatározott hello az Azure storage-fiókban.</span><span class="sxs-lookup"><span data-stu-id="098ca-173">Blob receipts are stored in a container named *azure-webjobs-hosts* in hello Azure storage account specified by hello AzureWebJobsStorage connection string.</span></span> <span data-ttu-id="098ca-174">Egy blob fogadását a következő információk hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="098ca-174">A blob receipt has hello following  information:</span></span>

* <span data-ttu-id="098ca-175">hello hello BLOB hívott függvény ("*{webjobs-feladat neve}*. Működik. *{Függvény neve}*", például:"WebJob1.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="098ca-175">hello function that was called for hello blob ("*{WebJob name}*.Functions.*{Function name}*", for example: "WebJob1.Functions.CopyBlob")</span></span>
* <span data-ttu-id="098ca-176">hello tároló neve</span><span class="sxs-lookup"><span data-stu-id="098ca-176">hello container name</span></span>
* <span data-ttu-id="098ca-177">hello blob típushoz ("BlockBlob" vagy "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="098ca-177">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="098ca-178">hello blob neve</span><span class="sxs-lookup"><span data-stu-id="098ca-178">hello blob name</span></span>
* <span data-ttu-id="098ca-179">hello ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="098ca-179">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="098ca-180">Ha azt szeretné, hogy tooforce újrafeldolgozása blob, manuálisan törölheti, hogy a blob hello blob visszaigazolás hello *azure-webjobs-állomások* tároló.</span><span class="sxs-lookup"><span data-stu-id="098ca-180">If you want tooforce reprocessing of a blob, you can manually delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container.</span></span>

## <a name="related-topics-covered-by-hello-queues-article"></a><span data-ttu-id="098ca-181">Hello várólisták cikkben említett kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="098ca-181">Related topics covered by hello queues article</span></span>
<span data-ttu-id="098ca-182">Hogyan toohandle blob feldolgozási elindul egy üzenetsor-üzenetet, vagy a WebJobs SDK forgatókönyvek nem adott tooblob feldolgozása, lásd: [hogyan toouse Azure várólista tároló hello WebJobs SDK a](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="098ca-182">For information about how toohandle blob processing triggered by a queue message, or for WebJobs SDK scenarios not specific tooblob processing, see [How toouse Azure queue storage with hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span>

<span data-ttu-id="098ca-183">Kapcsolódó témakörök hivatkozásra, ha a cikkben ismertetett hello alábbiakat foglalja magába:</span><span class="sxs-lookup"><span data-stu-id="098ca-183">Related topics covered in that article include hello following:</span></span>

* <span data-ttu-id="098ca-184">Aszinkron funkciók</span><span class="sxs-lookup"><span data-stu-id="098ca-184">Async functions</span></span>
* <span data-ttu-id="098ca-185">Több példánya</span><span class="sxs-lookup"><span data-stu-id="098ca-185">Multiple instances</span></span>
* <span data-ttu-id="098ca-186">Biztonságos leállításának</span><span class="sxs-lookup"><span data-stu-id="098ca-186">Graceful shutdown</span></span>
* <span data-ttu-id="098ca-187">Egy függvény törzséhez hello a WebJobs SDK attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="098ca-187">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="098ca-188">Hello SDK kapcsolati karakterláncok beállítása a kódban.</span><span class="sxs-lookup"><span data-stu-id="098ca-188">Set hello SDK connection strings in code.</span></span>
* <span data-ttu-id="098ca-189">Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot</span><span class="sxs-lookup"><span data-stu-id="098ca-189">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="098ca-190">Konfigurálása **MaxDequeueCount** poison blob kezelésére.</span><span class="sxs-lookup"><span data-stu-id="098ca-190">Configure **MaxDequeueCount** for poison blob handling.</span></span>
* <span data-ttu-id="098ca-191">Manuálisan kezdeményezi egy függvény</span><span class="sxs-lookup"><span data-stu-id="098ca-191">Trigger a function manually</span></span>
* <span data-ttu-id="098ca-192">Naplók írása</span><span class="sxs-lookup"><span data-stu-id="098ca-192">Write logs</span></span>

## <a name="next-steps"></a><span data-ttu-id="098ca-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="098ca-193">Next steps</span></span>
<span data-ttu-id="098ca-194">Ez a cikk nyújtott mintakódok, hogy hogyan toohandle gyakori helyzetek Azure végzett munka blobok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="098ca-194">This article has provided code samples that show how toohandle common scenarios for working with Azure blobs.</span></span> <span data-ttu-id="098ca-195">További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="098ca-195">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs documentation resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>


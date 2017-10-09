---
title: "Azure Media Encoder Standard tooauto aaaUse-létrehozni egy sávszélességű létra |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toouse Media Encoder Standard (MES) tooauto-létrehozni egy sávszélességű létra hello bemeneti feloldási és átviteli sebesség alapján. hello bemeneti megoldás és sávszélességű soha nem túllépése. Például ha hello bemeneti 720p, 3 MB/s, kimeneti lesz 720p legjobb maradnak, és alacsonyabb, mint 3 MB/s sebesség időpontban fog elindulni."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 63ed95da-1b82-44b0-b8ff-eebd535bc5c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 5437f54ac28c42ddd4f9d1986549d6da6261c5da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="use-azure-media-encoder-standard-tooauto-generate-a-bitrate-ladder"></a><span data-ttu-id="823a1-105">Használja az Azure Media Encoder Standard tooauto-egy sávszélességű létra készítése</span><span class="sxs-lookup"><span data-stu-id="823a1-105">Use Azure Media Encoder Standard tooauto-generate a bitrate ladder</span></span>

## <a name="overview"></a><span data-ttu-id="823a1-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="823a1-106">Overview</span></span>

<span data-ttu-id="823a1-107">Ez a témakör bemutatja, hogyan toouse Media Encoder Standard (MES) tooauto-hello bemeneti feloldási és átviteli sebesség alapján sávszélességű létra (sávszélességű felbontású párok) létrehozása.</span><span class="sxs-lookup"><span data-stu-id="823a1-107">This topic shows how toouse Media Encoder Standard (MES) tooauto-generate a bitrate ladder (bitrate-resolution pairs) based on hello input resolution and bitrate.</span></span> <span data-ttu-id="823a1-108">hello automatikusan generált előre definiált nem haladhatja meg a bemeneti hello megoldás és átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="823a1-108">hello auto-generated preset will never exceed hello input resolution and bitrate.</span></span> <span data-ttu-id="823a1-109">Például ha hello bemeneti 720p, 3 MB/s, kimeneti lesz 720p legjobb maradnak, és alacsonyabb, mint 3 MB/s sebesség időpontban fog elindulni.</span><span class="sxs-lookup"><span data-stu-id="823a1-109">For example, if hello input is 720p at 3Mbps, output will remain 720p at best, and will start at rates lower than 3Mbps.</span></span>

### <a name="encoding-for-streaming-only"></a><span data-ttu-id="823a1-110">Az adatfolyamként történő csak kódolás</span><span class="sxs-lookup"><span data-stu-id="823a1-110">Encoding for Streaming Only</span></span>

<span data-ttu-id="823a1-111">Ha a szándéka az tooencode a videó csak a streaming, akkor Ön %d használja a "Adaptív Streameléshez" hello forrás beállított egy kódolási feladat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="823a1-111">If your intent is tooencode your source video only for streaming, then you shoud use hello "Adaptive Streaming" preset when creating an encoding task.</span></span> <span data-ttu-id="823a1-112">Hello használatakor **adaptív Streameléshez** beállított, hello MES kódoló fog intelligens módon cap egy sávszélességű létra.</span><span class="sxs-lookup"><span data-stu-id="823a1-112">When using hello **Adaptive Streaming** preset, hello MES encoder will intelligently cap a bitrate ladder.</span></span> <span data-ttu-id="823a1-113">Azonban nem fog tudni toocontrol hello kódolás költségek, mivel hello szolgáltatást határozza meg, hogy hány rétegek toouse, és milyen felbontásban.</span><span class="sxs-lookup"><span data-stu-id="823a1-113">However, you will not be able toocontrol hello encoding costs, since hello service determines how many layers toouse and at what resolution.</span></span> <span data-ttu-id="823a1-114">Példa látható miatt hello kódolás MES által előállított kimeneti rétegek **adaptív Streameléshez** beállított hello Ez a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="823a1-114">You can see examples of output layers produced by MES as a result of encoding with hello **Adaptive Streaming** preset at hello end of this topic.</span></span> <span data-ttu-id="823a1-115">hello kimeneti eszköz MP4 fájljait fogja tartalmazni, ahol a hang és videó nem időosztásos.</span><span class="sxs-lookup"><span data-stu-id="823a1-115">hello output Asset will contain MP4 files where audio and video are not interleaved.</span></span>

### <a name="encoding-for-streaming-and-progressive-download"></a><span data-ttu-id="823a1-116">Adatfolyam-továbbításhoz és progresszív letöltés kódolása</span><span class="sxs-lookup"><span data-stu-id="823a1-116">Encoding for Streaming and Progressive Download</span></span>

<span data-ttu-id="823a1-117">Ha a szándéka az tooencode előre a forrás videó az adatfolyamként történő tooproduce MP4-fájlok progresszív letöltés, valamint hogy %d használhatják a "Tartalom adaptív több sávszélességű MP4" hello, egy kódolási feladat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="823a1-117">If your intent is tooencode your source video for streaming as well as tooproduce MP4 files for progressive download, then you shoud use hello "Content Adaptive Multiple Bitrate MP4" preset when creating an encoding task.</span></span> <span data-ttu-id="823a1-118">Hello használatakor **tartalom adaptív több sávszélességű MP4** beállított, hello MES kódoló érvényes kódolási logikák a fenti, de most hello kimeneti adategységen fogja tartalmazni MP4-fájlokat adott hang hello és a videó időosztásos.</span><span class="sxs-lookup"><span data-stu-id="823a1-118">When using hello **Content Adaptive Multiple Bitrate MP4** preset, hello MES encoder will apply hello same encoding logic as above, but now hello output asset will contain MP4 files where audio and video are interleaved.</span></span> <span data-ttu-id="823a1-119">A MP4-fájlok (például hello legmagasabb sávszélességű verzió) valamelyikét használhatja a progresszív letöltés fájlként.</span><span class="sxs-lookup"><span data-stu-id="823a1-119">You can use one of these MP4 files (for example, hello highest bitrate version) as a progressive download file.</span></span>

## <span data-ttu-id="823a1-120"><a id="encoding_with_dotnet"></a>A Media Services .NET SDK kódolás</span><span class="sxs-lookup"><span data-stu-id="823a1-120"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="823a1-121">a következő példakód hello Media Services .NET SDK tooperform hello feladatok a következő használja:</span><span class="sxs-lookup"><span data-stu-id="823a1-121">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

- <span data-ttu-id="823a1-122">Hozzon létre egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="823a1-122">Create an encoding job.</span></span>
- <span data-ttu-id="823a1-123">Egy hivatkozási toohello Media Encoder Standard encoder beolvasása.</span><span class="sxs-lookup"><span data-stu-id="823a1-123">Get a reference toohello Media Encoder Standard encoder.</span></span>
- <span data-ttu-id="823a1-124">A kódolási feladat toohello feladat hozzáadása, és adja meg a toouse hello **adaptív Streameléshez** előre.</span><span class="sxs-lookup"><span data-stu-id="823a1-124">Add an encoding task toohello job and specify toouse hello **Adaptive Streaming** preset.</span></span> 
- <span data-ttu-id="823a1-125">Hozzon létre egy kimeneti eszköz, amely kódolású hello eszköz fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="823a1-125">Create an output asset that will contain hello encoded asset.</span></span>
- <span data-ttu-id="823a1-126">Adjon hozzá egy esemény kezelő toocheck hello feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="823a1-126">Add an event handler toocheck hello job progress.</span></span>
- <span data-ttu-id="823a1-127">Hello feladat küldése</span><span class="sxs-lookup"><span data-stu-id="823a1-127">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="823a1-128">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="823a1-128">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="823a1-129">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="823a1-129">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="823a1-130">Példa</span><span class="sxs-lookup"><span data-stu-id="823a1-130">Example</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreamingMESPresest
    {
        class Program
        {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate hello output using hello "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
            case JobState.Error:

                // Cast sender as a job.
                IJob job = (IJob)sender;

                // Display or log error details as needed.
                break;
            default:
                break;
            }
        }
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
        }
    }

## <span data-ttu-id="823a1-131"><a id="output"></a>Kimeneti</span><span class="sxs-lookup"><span data-stu-id="823a1-131"><a id="output"></a>Output</span></span>

<span data-ttu-id="823a1-132">Ez a szakasz bemutatja a három példák miatt hello kódolás MES által előállított kimeneti rétegek **adaptív Streameléshez** előre.</span><span class="sxs-lookup"><span data-stu-id="823a1-132">This section shows three examples of output layers produced by MES as a result of encoding with hello **Adaptive Streaming** preset.</span></span> 

### <a name="example-1"></a><span data-ttu-id="823a1-133">1. példa</span><span class="sxs-lookup"><span data-stu-id="823a1-133">Example 1</span></span>
<span data-ttu-id="823a1-134">A magasság "1080" és "29.970" képkockasebességhez forrás 6 videó rétegek hoz létre:</span><span class="sxs-lookup"><span data-stu-id="823a1-134">Source with height "1080" and framerate "29.970" produces 6 video layers:</span></span>

|<span data-ttu-id="823a1-135">Réteg</span><span class="sxs-lookup"><span data-stu-id="823a1-135">Layer</span></span>|<span data-ttu-id="823a1-136">Magassága</span><span class="sxs-lookup"><span data-stu-id="823a1-136">Height</span></span>|<span data-ttu-id="823a1-137">Szélessége</span><span class="sxs-lookup"><span data-stu-id="823a1-137">Width</span></span>|<span data-ttu-id="823a1-138">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="823a1-138">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="823a1-139">1</span><span class="sxs-lookup"><span data-stu-id="823a1-139">1</span></span>|<span data-ttu-id="823a1-140">1080</span><span class="sxs-lookup"><span data-stu-id="823a1-140">1080</span></span>|<span data-ttu-id="823a1-141">1920</span><span class="sxs-lookup"><span data-stu-id="823a1-141">1920</span></span>|<span data-ttu-id="823a1-142">6780</span><span class="sxs-lookup"><span data-stu-id="823a1-142">6780</span></span>|
|<span data-ttu-id="823a1-143">2</span><span class="sxs-lookup"><span data-stu-id="823a1-143">2</span></span>|<span data-ttu-id="823a1-144">720</span><span class="sxs-lookup"><span data-stu-id="823a1-144">720</span></span>|<span data-ttu-id="823a1-145">1280</span><span class="sxs-lookup"><span data-stu-id="823a1-145">1280</span></span>|<span data-ttu-id="823a1-146">3520</span><span class="sxs-lookup"><span data-stu-id="823a1-146">3520</span></span>|
|<span data-ttu-id="823a1-147">3</span><span class="sxs-lookup"><span data-stu-id="823a1-147">3</span></span>|<span data-ttu-id="823a1-148">540</span><span class="sxs-lookup"><span data-stu-id="823a1-148">540</span></span>|<span data-ttu-id="823a1-149">960</span><span class="sxs-lookup"><span data-stu-id="823a1-149">960</span></span>|<span data-ttu-id="823a1-150">2210</span><span class="sxs-lookup"><span data-stu-id="823a1-150">2210</span></span>|
|<span data-ttu-id="823a1-151">4</span><span class="sxs-lookup"><span data-stu-id="823a1-151">4</span></span>|<span data-ttu-id="823a1-152">360</span><span class="sxs-lookup"><span data-stu-id="823a1-152">360</span></span>|<span data-ttu-id="823a1-153">640</span><span class="sxs-lookup"><span data-stu-id="823a1-153">640</span></span>|<span data-ttu-id="823a1-154">1150</span><span class="sxs-lookup"><span data-stu-id="823a1-154">1150</span></span>|
|<span data-ttu-id="823a1-155">5</span><span class="sxs-lookup"><span data-stu-id="823a1-155">5</span></span>|<span data-ttu-id="823a1-156">270</span><span class="sxs-lookup"><span data-stu-id="823a1-156">270</span></span>|<span data-ttu-id="823a1-157">480</span><span class="sxs-lookup"><span data-stu-id="823a1-157">480</span></span>|<span data-ttu-id="823a1-158">720</span><span class="sxs-lookup"><span data-stu-id="823a1-158">720</span></span>|
|<span data-ttu-id="823a1-159">6</span><span class="sxs-lookup"><span data-stu-id="823a1-159">6</span></span>|<span data-ttu-id="823a1-160">180</span><span class="sxs-lookup"><span data-stu-id="823a1-160">180</span></span>|<span data-ttu-id="823a1-161">320</span><span class="sxs-lookup"><span data-stu-id="823a1-161">320</span></span>|<span data-ttu-id="823a1-162">380</span><span class="sxs-lookup"><span data-stu-id="823a1-162">380</span></span>|

### <a name="example-2"></a><span data-ttu-id="823a1-163">2. példa</span><span class="sxs-lookup"><span data-stu-id="823a1-163">Example 2</span></span>
<span data-ttu-id="823a1-164">A magasság "720" és "23.970" képkockasebességhez forrás 5 videó rétegek hoz létre:</span><span class="sxs-lookup"><span data-stu-id="823a1-164">Source with height "720" and framerate "23.970" produces 5 video layers:</span></span>

|<span data-ttu-id="823a1-165">Réteg</span><span class="sxs-lookup"><span data-stu-id="823a1-165">Layer</span></span>|<span data-ttu-id="823a1-166">Magassága</span><span class="sxs-lookup"><span data-stu-id="823a1-166">Height</span></span>|<span data-ttu-id="823a1-167">Szélessége</span><span class="sxs-lookup"><span data-stu-id="823a1-167">Width</span></span>|<span data-ttu-id="823a1-168">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="823a1-168">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="823a1-169">1</span><span class="sxs-lookup"><span data-stu-id="823a1-169">1</span></span>|<span data-ttu-id="823a1-170">720</span><span class="sxs-lookup"><span data-stu-id="823a1-170">720</span></span>|<span data-ttu-id="823a1-171">1280</span><span class="sxs-lookup"><span data-stu-id="823a1-171">1280</span></span>|<span data-ttu-id="823a1-172">2940</span><span class="sxs-lookup"><span data-stu-id="823a1-172">2940</span></span>|
|<span data-ttu-id="823a1-173">2</span><span class="sxs-lookup"><span data-stu-id="823a1-173">2</span></span>|<span data-ttu-id="823a1-174">540</span><span class="sxs-lookup"><span data-stu-id="823a1-174">540</span></span>|<span data-ttu-id="823a1-175">960</span><span class="sxs-lookup"><span data-stu-id="823a1-175">960</span></span>|<span data-ttu-id="823a1-176">1850</span><span class="sxs-lookup"><span data-stu-id="823a1-176">1850</span></span>|
|<span data-ttu-id="823a1-177">3</span><span class="sxs-lookup"><span data-stu-id="823a1-177">3</span></span>|<span data-ttu-id="823a1-178">360</span><span class="sxs-lookup"><span data-stu-id="823a1-178">360</span></span>|<span data-ttu-id="823a1-179">640</span><span class="sxs-lookup"><span data-stu-id="823a1-179">640</span></span>|<span data-ttu-id="823a1-180">960</span><span class="sxs-lookup"><span data-stu-id="823a1-180">960</span></span>|
|<span data-ttu-id="823a1-181">4</span><span class="sxs-lookup"><span data-stu-id="823a1-181">4</span></span>|<span data-ttu-id="823a1-182">270</span><span class="sxs-lookup"><span data-stu-id="823a1-182">270</span></span>|<span data-ttu-id="823a1-183">480</span><span class="sxs-lookup"><span data-stu-id="823a1-183">480</span></span>|<span data-ttu-id="823a1-184">600</span><span class="sxs-lookup"><span data-stu-id="823a1-184">600</span></span>|
|<span data-ttu-id="823a1-185">5</span><span class="sxs-lookup"><span data-stu-id="823a1-185">5</span></span>|<span data-ttu-id="823a1-186">180</span><span class="sxs-lookup"><span data-stu-id="823a1-186">180</span></span>|<span data-ttu-id="823a1-187">320</span><span class="sxs-lookup"><span data-stu-id="823a1-187">320</span></span>|<span data-ttu-id="823a1-188">320</span><span class="sxs-lookup"><span data-stu-id="823a1-188">320</span></span>|

### <a name="example-3"></a><span data-ttu-id="823a1-189">3. példa</span><span class="sxs-lookup"><span data-stu-id="823a1-189">Example 3</span></span>
<span data-ttu-id="823a1-190">A magasság "360" és "29.970" képkockasebességhez forrás 3 videó rétegek hoz létre:</span><span class="sxs-lookup"><span data-stu-id="823a1-190">Source with height "360" and framerate "29.970" produces 3 video layers:</span></span>

|<span data-ttu-id="823a1-191">Réteg</span><span class="sxs-lookup"><span data-stu-id="823a1-191">Layer</span></span>|<span data-ttu-id="823a1-192">Magassága</span><span class="sxs-lookup"><span data-stu-id="823a1-192">Height</span></span>|<span data-ttu-id="823a1-193">Szélessége</span><span class="sxs-lookup"><span data-stu-id="823a1-193">Width</span></span>|<span data-ttu-id="823a1-194">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="823a1-194">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="823a1-195">1</span><span class="sxs-lookup"><span data-stu-id="823a1-195">1</span></span>|<span data-ttu-id="823a1-196">360</span><span class="sxs-lookup"><span data-stu-id="823a1-196">360</span></span>|<span data-ttu-id="823a1-197">640</span><span class="sxs-lookup"><span data-stu-id="823a1-197">640</span></span>|<span data-ttu-id="823a1-198">700</span><span class="sxs-lookup"><span data-stu-id="823a1-198">700</span></span>|
|<span data-ttu-id="823a1-199">2</span><span class="sxs-lookup"><span data-stu-id="823a1-199">2</span></span>|<span data-ttu-id="823a1-200">270</span><span class="sxs-lookup"><span data-stu-id="823a1-200">270</span></span>|<span data-ttu-id="823a1-201">480</span><span class="sxs-lookup"><span data-stu-id="823a1-201">480</span></span>|<span data-ttu-id="823a1-202">440</span><span class="sxs-lookup"><span data-stu-id="823a1-202">440</span></span>|
|<span data-ttu-id="823a1-203">3</span><span class="sxs-lookup"><span data-stu-id="823a1-203">3</span></span>|<span data-ttu-id="823a1-204">180</span><span class="sxs-lookup"><span data-stu-id="823a1-204">180</span></span>|<span data-ttu-id="823a1-205">320</span><span class="sxs-lookup"><span data-stu-id="823a1-205">320</span></span>|<span data-ttu-id="823a1-206">230</span><span class="sxs-lookup"><span data-stu-id="823a1-206">230</span></span>|
## <a name="media-services-learning-paths"></a><span data-ttu-id="823a1-207">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="823a1-207">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="823a1-208">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="823a1-208">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="823a1-209">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="823a1-209">See Also</span></span>
[<span data-ttu-id="823a1-210">Media Services kódolási áttekintése</span><span class="sxs-lookup"><span data-stu-id="823a1-210">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)


---
title: "Azure Media Encoder Standard segítségével automatikusan előállítja a sávszélességű létra |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan egy bemeneti feloldási és átviteli sebesség alapján sávszélességű létra automatikus létrehozása a Media Encoder Standard (MES) segítségével. A bemeneti megoldás és sávszélességű soha nem túllépése. Például ha a bemeneti 720p, 3 MB/s, kimeneti lesz 720p legjobb maradnak, és alacsonyabb, mint 3 MB/s sebesség időpontban fog elindulni."
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
ms.openlocfilehash: b5616aa9f8b15ab576d914fbae89a56f64c27f4a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
#  <a name="use-azure-media-encoder-standard-to-auto-generate-a-bitrate-ladder"></a><span data-ttu-id="6d6aa-105">Azure Media Encoder Standard segítségével egy sávszélességű létra automatikus létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d6aa-105">Use Azure Media Encoder Standard to auto-generate a bitrate ladder</span></span>

## <a name="overview"></a><span data-ttu-id="6d6aa-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6d6aa-106">Overview</span></span>

<span data-ttu-id="6d6aa-107">Ez a témakör bemutatja, hogyan sávszélességű létra (sávszélességű felbontású párok) a bemeneti feloldási és átviteli sebesség alapján automatikus létrehozása a Media Encoder Standard (MES) segítségével.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-107">This topic shows how to use Media Encoder Standard (MES) to auto-generate a bitrate ladder (bitrate-resolution pairs) based on the input resolution and bitrate.</span></span> <span data-ttu-id="6d6aa-108">Az automatikusan generált előre definiált nem haladhatja meg a bemeneti megoldás és átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-108">The auto-generated preset will never exceed the input resolution and bitrate.</span></span> <span data-ttu-id="6d6aa-109">Például ha a bemeneti 720p, 3 MB/s, kimeneti lesz 720p legjobb maradnak, és alacsonyabb, mint 3 MB/s sebesség időpontban fog elindulni.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-109">For example, if the input is 720p at 3Mbps, output will remain 720p at best, and will start at rates lower than 3Mbps.</span></span>

### <a name="encoding-for-streaming-only"></a><span data-ttu-id="6d6aa-110">Az adatfolyamként történő csak kódolás</span><span class="sxs-lookup"><span data-stu-id="6d6aa-110">Encoding for Streaming Only</span></span>

<span data-ttu-id="6d6aa-111">Ha a szándéka az, a forrás videó csak az adatfolyamként történő kódolásához, akkor %d használja, a "adaptív Streameléshez" egy kódolási feladat létrehozásakor beállított.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-111">If your intent is to encode your source video only for streaming, then you shoud use the "Adaptive Streaming" preset when creating an encoding task.</span></span> <span data-ttu-id="6d6aa-112">Használatakor a **adaptív Streameléshez** előzetesen létrehozott, a MES kódoló fog intelligens módon cap egy sávszélességű létra.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-112">When using the **Adaptive Streaming** preset, the MES encoder will intelligently cap a bitrate ladder.</span></span> <span data-ttu-id="6d6aa-113">Azonban nem lesz szabályozhatja a kódolás költségek, mivel a szolgáltatás határozza meg, hány rétegek használata, és milyen felbontásban.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-113">However, you will not be able to control the encoding costs, since the service determines how many layers to use and at what resolution.</span></span> <span data-ttu-id="6d6aa-114">Példa látható miatt kódolás MES által előállított kimeneti rétegek a **adaptív Streameléshez** beállított Ez a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-114">You can see examples of output layers produced by MES as a result of encoding with the **Adaptive Streaming** preset at the end of this topic.</span></span> <span data-ttu-id="6d6aa-115">Az eszköz fogja tartalmazni MP4-fájlokat, ahol a hang- és kimeneti nem időosztásos.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-115">The output Asset will contain MP4 files where audio and video are not interleaved.</span></span>

### <a name="encoding-for-streaming-and-progressive-download"></a><span data-ttu-id="6d6aa-116">Adatfolyam-továbbításhoz és progresszív letöltés kódolása</span><span class="sxs-lookup"><span data-stu-id="6d6aa-116">Encoding for Streaming and Progressive Download</span></span>

<span data-ttu-id="6d6aa-117">Ha a szándéka az, a forrás videó az adatfolyamként történő kódolásához, valamint a progresszív letöltés MP4-fájlok előállításához, akkor %d használja a "tartalom adaptív több sávszélességű MP4" egy kódolási feladat létrehozásakor beállított.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-117">If your intent is to encode your source video for streaming as well as to produce MP4 files for progressive download, then you shoud use the "Content Adaptive Multiple Bitrate MP4" preset when creating an encoding task.</span></span> <span data-ttu-id="6d6aa-118">Használatakor a **tartalom adaptív több sávszélességű MP4** előzetesen létrehozott, a MES kódoló a fenti kódolási logikák hatálya alá, de most a kimeneti adategységen fogja tartalmazni MP4-fájlokat, ahol hang és videó időosztásos.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-118">When using the **Content Adaptive Multiple Bitrate MP4** preset, the MES encoder will apply the same encoding logic as above, but now the output asset will contain MP4 files where audio and video are interleaved.</span></span> <span data-ttu-id="6d6aa-119">A MP4-fájlok (például a legmagasabb sávszélességű verzió) valamelyikét használhatja a progresszív letöltés fájlként.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-119">You can use one of these MP4 files (for example, the highest bitrate version) as a progressive download file.</span></span>

## <span data-ttu-id="6d6aa-120"><a id="encoding_with_dotnet"></a>A Media Services .NET SDK kódolás</span><span class="sxs-lookup"><span data-stu-id="6d6aa-120"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="6d6aa-121">Az alábbi példakód Media Services .NET SDK-t használja a következő feladatok végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="6d6aa-121">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

- <span data-ttu-id="6d6aa-122">Hozzon létre egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-122">Create an encoding job.</span></span>
- <span data-ttu-id="6d6aa-123">A Media Encoder Standard encoder mutató hivatkozás beszerzése.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-123">Get a reference to the Media Encoder Standard encoder.</span></span>
- <span data-ttu-id="6d6aa-124">Adja hozzá a feladatot egy kódolási feladatot, és adja meg, hogy használja a **adaptív Streameléshez** előre.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-124">Add an encoding task to the job and specify to use the **Adaptive Streaming** preset.</span></span> 
- <span data-ttu-id="6d6aa-125">Hozzon létre egy kimeneti eszközt, amely tartalmazza majd a kódolt objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-125">Create an output asset that will contain the encoded asset.</span></span>
- <span data-ttu-id="6d6aa-126">Adjon hozzá egy eseménykezelő, ellenőrizze a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-126">Add an event handler to check the job progress.</span></span>
- <span data-ttu-id="6d6aa-127">A feladat elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-127">Submit the job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="6d6aa-128">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d6aa-128">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="6d6aa-129">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-129">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="6d6aa-130">Példa</span><span class="sxs-lookup"><span data-stu-id="6d6aa-130">Example</span></span>

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreamingMESPresest
    {
        class Program
        {
        // Read values from the App.config file.
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

            // Encode and generate the output using the "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
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

## <span data-ttu-id="6d6aa-131"><a id="output"></a>Kimeneti</span><span class="sxs-lookup"><span data-stu-id="6d6aa-131"><a id="output"></a>Output</span></span>

<span data-ttu-id="6d6aa-132">Ez a szakasz ismerteti a kimeneti rétegek MES miatt kódolás három példái a **adaptív Streameléshez** előre.</span><span class="sxs-lookup"><span data-stu-id="6d6aa-132">This section shows three examples of output layers produced by MES as a result of encoding with the **Adaptive Streaming** preset.</span></span> 

### <a name="example-1"></a><span data-ttu-id="6d6aa-133">1. példa</span><span class="sxs-lookup"><span data-stu-id="6d6aa-133">Example 1</span></span>
<span data-ttu-id="6d6aa-134">A magasság "1080" és "29.970" képkockasebességhez forrás 6 videó rétegek hoz létre:</span><span class="sxs-lookup"><span data-stu-id="6d6aa-134">Source with height "1080" and framerate "29.970" produces 6 video layers:</span></span>

|<span data-ttu-id="6d6aa-135">Réteg</span><span class="sxs-lookup"><span data-stu-id="6d6aa-135">Layer</span></span>|<span data-ttu-id="6d6aa-136">Magassága</span><span class="sxs-lookup"><span data-stu-id="6d6aa-136">Height</span></span>|<span data-ttu-id="6d6aa-137">Szélessége</span><span class="sxs-lookup"><span data-stu-id="6d6aa-137">Width</span></span>|<span data-ttu-id="6d6aa-138">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="6d6aa-138">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="6d6aa-139">1</span><span class="sxs-lookup"><span data-stu-id="6d6aa-139">1</span></span>|<span data-ttu-id="6d6aa-140">1080</span><span class="sxs-lookup"><span data-stu-id="6d6aa-140">1080</span></span>|<span data-ttu-id="6d6aa-141">1920</span><span class="sxs-lookup"><span data-stu-id="6d6aa-141">1920</span></span>|<span data-ttu-id="6d6aa-142">6780</span><span class="sxs-lookup"><span data-stu-id="6d6aa-142">6780</span></span>|
|<span data-ttu-id="6d6aa-143">2</span><span class="sxs-lookup"><span data-stu-id="6d6aa-143">2</span></span>|<span data-ttu-id="6d6aa-144">720</span><span class="sxs-lookup"><span data-stu-id="6d6aa-144">720</span></span>|<span data-ttu-id="6d6aa-145">1280</span><span class="sxs-lookup"><span data-stu-id="6d6aa-145">1280</span></span>|<span data-ttu-id="6d6aa-146">3520</span><span class="sxs-lookup"><span data-stu-id="6d6aa-146">3520</span></span>|
|<span data-ttu-id="6d6aa-147">3</span><span class="sxs-lookup"><span data-stu-id="6d6aa-147">3</span></span>|<span data-ttu-id="6d6aa-148">540</span><span class="sxs-lookup"><span data-stu-id="6d6aa-148">540</span></span>|<span data-ttu-id="6d6aa-149">960</span><span class="sxs-lookup"><span data-stu-id="6d6aa-149">960</span></span>|<span data-ttu-id="6d6aa-150">2210</span><span class="sxs-lookup"><span data-stu-id="6d6aa-150">2210</span></span>|
|<span data-ttu-id="6d6aa-151">4</span><span class="sxs-lookup"><span data-stu-id="6d6aa-151">4</span></span>|<span data-ttu-id="6d6aa-152">360</span><span class="sxs-lookup"><span data-stu-id="6d6aa-152">360</span></span>|<span data-ttu-id="6d6aa-153">640</span><span class="sxs-lookup"><span data-stu-id="6d6aa-153">640</span></span>|<span data-ttu-id="6d6aa-154">1150</span><span class="sxs-lookup"><span data-stu-id="6d6aa-154">1150</span></span>|
|<span data-ttu-id="6d6aa-155">5</span><span class="sxs-lookup"><span data-stu-id="6d6aa-155">5</span></span>|<span data-ttu-id="6d6aa-156">270</span><span class="sxs-lookup"><span data-stu-id="6d6aa-156">270</span></span>|<span data-ttu-id="6d6aa-157">480</span><span class="sxs-lookup"><span data-stu-id="6d6aa-157">480</span></span>|<span data-ttu-id="6d6aa-158">720</span><span class="sxs-lookup"><span data-stu-id="6d6aa-158">720</span></span>|
|<span data-ttu-id="6d6aa-159">6</span><span class="sxs-lookup"><span data-stu-id="6d6aa-159">6</span></span>|<span data-ttu-id="6d6aa-160">180</span><span class="sxs-lookup"><span data-stu-id="6d6aa-160">180</span></span>|<span data-ttu-id="6d6aa-161">320</span><span class="sxs-lookup"><span data-stu-id="6d6aa-161">320</span></span>|<span data-ttu-id="6d6aa-162">380</span><span class="sxs-lookup"><span data-stu-id="6d6aa-162">380</span></span>|

### <a name="example-2"></a><span data-ttu-id="6d6aa-163">2. példa</span><span class="sxs-lookup"><span data-stu-id="6d6aa-163">Example 2</span></span>
<span data-ttu-id="6d6aa-164">A magasság "720" és "23.970" képkockasebességhez forrás 5 videó rétegek hoz létre:</span><span class="sxs-lookup"><span data-stu-id="6d6aa-164">Source with height "720" and framerate "23.970" produces 5 video layers:</span></span>

|<span data-ttu-id="6d6aa-165">Réteg</span><span class="sxs-lookup"><span data-stu-id="6d6aa-165">Layer</span></span>|<span data-ttu-id="6d6aa-166">Magassága</span><span class="sxs-lookup"><span data-stu-id="6d6aa-166">Height</span></span>|<span data-ttu-id="6d6aa-167">Szélessége</span><span class="sxs-lookup"><span data-stu-id="6d6aa-167">Width</span></span>|<span data-ttu-id="6d6aa-168">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="6d6aa-168">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="6d6aa-169">1</span><span class="sxs-lookup"><span data-stu-id="6d6aa-169">1</span></span>|<span data-ttu-id="6d6aa-170">720</span><span class="sxs-lookup"><span data-stu-id="6d6aa-170">720</span></span>|<span data-ttu-id="6d6aa-171">1280</span><span class="sxs-lookup"><span data-stu-id="6d6aa-171">1280</span></span>|<span data-ttu-id="6d6aa-172">2940</span><span class="sxs-lookup"><span data-stu-id="6d6aa-172">2940</span></span>|
|<span data-ttu-id="6d6aa-173">2</span><span class="sxs-lookup"><span data-stu-id="6d6aa-173">2</span></span>|<span data-ttu-id="6d6aa-174">540</span><span class="sxs-lookup"><span data-stu-id="6d6aa-174">540</span></span>|<span data-ttu-id="6d6aa-175">960</span><span class="sxs-lookup"><span data-stu-id="6d6aa-175">960</span></span>|<span data-ttu-id="6d6aa-176">1850</span><span class="sxs-lookup"><span data-stu-id="6d6aa-176">1850</span></span>|
|<span data-ttu-id="6d6aa-177">3</span><span class="sxs-lookup"><span data-stu-id="6d6aa-177">3</span></span>|<span data-ttu-id="6d6aa-178">360</span><span class="sxs-lookup"><span data-stu-id="6d6aa-178">360</span></span>|<span data-ttu-id="6d6aa-179">640</span><span class="sxs-lookup"><span data-stu-id="6d6aa-179">640</span></span>|<span data-ttu-id="6d6aa-180">960</span><span class="sxs-lookup"><span data-stu-id="6d6aa-180">960</span></span>|
|<span data-ttu-id="6d6aa-181">4</span><span class="sxs-lookup"><span data-stu-id="6d6aa-181">4</span></span>|<span data-ttu-id="6d6aa-182">270</span><span class="sxs-lookup"><span data-stu-id="6d6aa-182">270</span></span>|<span data-ttu-id="6d6aa-183">480</span><span class="sxs-lookup"><span data-stu-id="6d6aa-183">480</span></span>|<span data-ttu-id="6d6aa-184">600</span><span class="sxs-lookup"><span data-stu-id="6d6aa-184">600</span></span>|
|<span data-ttu-id="6d6aa-185">5</span><span class="sxs-lookup"><span data-stu-id="6d6aa-185">5</span></span>|<span data-ttu-id="6d6aa-186">180</span><span class="sxs-lookup"><span data-stu-id="6d6aa-186">180</span></span>|<span data-ttu-id="6d6aa-187">320</span><span class="sxs-lookup"><span data-stu-id="6d6aa-187">320</span></span>|<span data-ttu-id="6d6aa-188">320</span><span class="sxs-lookup"><span data-stu-id="6d6aa-188">320</span></span>|

### <a name="example-3"></a><span data-ttu-id="6d6aa-189">3. példa</span><span class="sxs-lookup"><span data-stu-id="6d6aa-189">Example 3</span></span>
<span data-ttu-id="6d6aa-190">A magasság "360" és "29.970" képkockasebességhez forrás 3 videó rétegek hoz létre:</span><span class="sxs-lookup"><span data-stu-id="6d6aa-190">Source with height "360" and framerate "29.970" produces 3 video layers:</span></span>

|<span data-ttu-id="6d6aa-191">Réteg</span><span class="sxs-lookup"><span data-stu-id="6d6aa-191">Layer</span></span>|<span data-ttu-id="6d6aa-192">Magassága</span><span class="sxs-lookup"><span data-stu-id="6d6aa-192">Height</span></span>|<span data-ttu-id="6d6aa-193">Szélessége</span><span class="sxs-lookup"><span data-stu-id="6d6aa-193">Width</span></span>|<span data-ttu-id="6d6aa-194">Bitrate(kbps)</span><span class="sxs-lookup"><span data-stu-id="6d6aa-194">Bitrate(kbps)</span></span>|
|---|---|---|---|
|<span data-ttu-id="6d6aa-195">1</span><span class="sxs-lookup"><span data-stu-id="6d6aa-195">1</span></span>|<span data-ttu-id="6d6aa-196">360</span><span class="sxs-lookup"><span data-stu-id="6d6aa-196">360</span></span>|<span data-ttu-id="6d6aa-197">640</span><span class="sxs-lookup"><span data-stu-id="6d6aa-197">640</span></span>|<span data-ttu-id="6d6aa-198">700</span><span class="sxs-lookup"><span data-stu-id="6d6aa-198">700</span></span>|
|<span data-ttu-id="6d6aa-199">2</span><span class="sxs-lookup"><span data-stu-id="6d6aa-199">2</span></span>|<span data-ttu-id="6d6aa-200">270</span><span class="sxs-lookup"><span data-stu-id="6d6aa-200">270</span></span>|<span data-ttu-id="6d6aa-201">480</span><span class="sxs-lookup"><span data-stu-id="6d6aa-201">480</span></span>|<span data-ttu-id="6d6aa-202">440</span><span class="sxs-lookup"><span data-stu-id="6d6aa-202">440</span></span>|
|<span data-ttu-id="6d6aa-203">3</span><span class="sxs-lookup"><span data-stu-id="6d6aa-203">3</span></span>|<span data-ttu-id="6d6aa-204">180</span><span class="sxs-lookup"><span data-stu-id="6d6aa-204">180</span></span>|<span data-ttu-id="6d6aa-205">320</span><span class="sxs-lookup"><span data-stu-id="6d6aa-205">320</span></span>|<span data-ttu-id="6d6aa-206">230</span><span class="sxs-lookup"><span data-stu-id="6d6aa-206">230</span></span>|
## <a name="media-services-learning-paths"></a><span data-ttu-id="6d6aa-207">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="6d6aa-207">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6d6aa-208">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="6d6aa-208">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="6d6aa-209">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6d6aa-209">See Also</span></span>
[<span data-ttu-id="6d6aa-210">Media Services kódolási áttekintése</span><span class="sxs-lookup"><span data-stu-id="6d6aa-210">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)


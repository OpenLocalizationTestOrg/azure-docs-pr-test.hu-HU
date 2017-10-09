---
title: "aaaUse Azure Media Videoindexképek tooCreate egy videó összegzésének |} Microsoft Docs"
description: "Videóösszegzés segítségével létre videók kijelölésével automatikusan érdekes kódtöredékek hello forrás videó. Ez akkor hasznos, ha azt szeretné, hogy milyen hosszú videó tooexpect gyors áttekintést tooprovide."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 0a8f0bba6c12a948b940114fe4937e675688a8c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-video-thumbnails-toocreate-a-video-summarization"></a><span data-ttu-id="169bb-104">Azure Media Videoindexképek tooCreate egy videó összegzésének használata</span><span class="sxs-lookup"><span data-stu-id="169bb-104">Use Azure Media Video Thumbnails tooCreate a Video Summarization</span></span>
## <a name="overview"></a><span data-ttu-id="169bb-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="169bb-105">Overview</span></span>
<span data-ttu-id="169bb-106">Hello **Azure Media videó miniatűrök** media processzor (MP) lehetővé teszi a toocreate hasznos toocustomers számára, akik csak toopreview hosszú videó összegzését videót összegzését.</span><span class="sxs-lookup"><span data-stu-id="169bb-106">hello **Azure Media Video Thumbnails** media processor (MP) enables you toocreate a summary of a video that is useful toocustomers who just want toopreview a summary of a long video.</span></span> <span data-ttu-id="169bb-107">Például az ügyfelek érdemes toosee rövid "összefoglaló videó" Ha azok rámutat egy miniatűr.</span><span class="sxs-lookup"><span data-stu-id="169bb-107">For example, customers might want toosee a short "summary video" when they hover over a thumbnail.</span></span> <span data-ttu-id="169bb-108">Hello paraméterei tökéletesítse által **Azure Media Videoindexképek** keresztül egy konfigurációs készlet hello MP hatékony hibaüzenetet észlelési segítségével, és kapott technológia tooalgorithmically leíró subclip készítése.</span><span class="sxs-lookup"><span data-stu-id="169bb-108">By tweaking hello parameters of **Azure Media Video Thumbnails** through a configuration preset, you can use hello MP's powerful shot detection and concatenation technology tooalgorithmically generate a descriptive subclip.</span></span>  

<span data-ttu-id="169bb-109">Hello **Azure Media videó miniatűr** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="169bb-109">hello **Azure Media Video Thumbnail** MP is currently in Preview.</span></span>

<span data-ttu-id="169bb-110">Ez a témakör kapcsolatos részleteket nyújt **Azure Media videó miniatűr** és bemutatja, hogyan toouse a Media Services SDK for .NET.</span><span class="sxs-lookup"><span data-stu-id="169bb-110">This topic gives details about  **Azure Media Video Thumbnail** and shows how toouse it with Media Services SDK for .NET.</span></span>

## <a name="limitations"></a><span data-ttu-id="169bb-111">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="169bb-111">Limitations</span></span>

<span data-ttu-id="169bb-112">Bizonyos esetekben a videó nem áll a különböző színfalak hello kimeneti csak lesz egy hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="169bb-112">In some cases, if your video is not comprised of different scenes, hello output will only be a single shot.</span></span>

## <a name="video-summary-example"></a><span data-ttu-id="169bb-113">Videó összefoglaló – példa</span><span class="sxs-lookup"><span data-stu-id="169bb-113">Video summary example</span></span>
<span data-ttu-id="169bb-114">Íme néhány példa a milyen hello Azure Media Videoindexképek media processzor teheti meg:</span><span class="sxs-lookup"><span data-stu-id="169bb-114">Here are some examples of what hello Azure Media Video Thumbnails media processor can do:</span></span>

### <a name="original-video"></a><span data-ttu-id="169bb-115">Eredeti videó</span><span class="sxs-lookup"><span data-stu-id="169bb-115">Original video</span></span>
[<span data-ttu-id="169bb-116">Eredeti videó</span><span class="sxs-lookup"><span data-stu-id="169bb-116">Original video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

### <a name="video-thumbnail-result"></a><span data-ttu-id="169bb-117">Videó miniatűr eredménye</span><span class="sxs-lookup"><span data-stu-id="169bb-117">Video thumbnail result</span></span>
[<span data-ttu-id="169bb-118">Videó miniatűr eredménye</span><span class="sxs-lookup"><span data-stu-id="169bb-118">Video thumbnail result</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="task-configuration-preset"></a><span data-ttu-id="169bb-119">A feladat konfigurációja (beállítás)</span><span class="sxs-lookup"><span data-stu-id="169bb-119">Task configuration (preset)</span></span>
<span data-ttu-id="169bb-120">A videó miniatűr feladat létrehozásakor **Azure Media videó miniatűrök**, meg kell adnia egy konfigurációs készletet.</span><span class="sxs-lookup"><span data-stu-id="169bb-120">When creating a video thumbnail task with **Azure Media Video Thumbnails**, you must specify a configuration preset.</span></span> <span data-ttu-id="169bb-121">a következő alapkonfiguráció JSON hello fent miniatűr minta hello hozták létre:</span><span class="sxs-lookup"><span data-stu-id="169bb-121">hello above thumbnail sample was created with hello following basic JSON configuration:</span></span>

    {"version":"1.0"}

<span data-ttu-id="169bb-122">Jelenleg a következő paraméterek hello módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="169bb-122">Currently, you can change hello following parameters:</span></span>

| <span data-ttu-id="169bb-123">Param</span><span class="sxs-lookup"><span data-stu-id="169bb-123">Param</span></span> | <span data-ttu-id="169bb-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="169bb-124">Description</span></span> |
| --- | --- |
| <span data-ttu-id="169bb-125">outputAudio</span><span class="sxs-lookup"><span data-stu-id="169bb-125">outputAudio</span></span> |<span data-ttu-id="169bb-126">Itt adhatja meg, függetlenül attól, hello eredő videó tartalmaz hangot.</span><span class="sxs-lookup"><span data-stu-id="169bb-126">Specifies whether or not hello resultant video contains any audio.</span></span> <br/><span data-ttu-id="169bb-127">Engedélyezett értékek: True vagy False.</span><span class="sxs-lookup"><span data-stu-id="169bb-127">Allowed values are: True or False.</span></span> <span data-ttu-id="169bb-128">Alapértelmezett érték a True.</span><span class="sxs-lookup"><span data-stu-id="169bb-128">Default is True.</span></span> |
| <span data-ttu-id="169bb-129">fadeInFadeOut</span><span class="sxs-lookup"><span data-stu-id="169bb-129">fadeInFadeOut</span></span> |<span data-ttu-id="169bb-130">Itt adhatja meg hello külön mozgásérzékelési miniatűrök között használt-e halványítási átmenetek.</span><span class="sxs-lookup"><span data-stu-id="169bb-130">Specifies whether or not fade transitions are used between hello separate motion thumbnails.</span></span>  <br/><span data-ttu-id="169bb-131">Engedélyezett értékek: True vagy False.</span><span class="sxs-lookup"><span data-stu-id="169bb-131">Allowed values are: True or False.</span></span>  <span data-ttu-id="169bb-132">Alapértelmezett érték a True.</span><span class="sxs-lookup"><span data-stu-id="169bb-132">Default is True.</span></span> |
| <span data-ttu-id="169bb-133">maxMotionThumbnailDurationInSecs</span><span class="sxs-lookup"><span data-stu-id="169bb-133">maxMotionThumbnailDurationInSecs</span></span> |<span data-ttu-id="169bb-134">Egész szám, amely meghatározza, hogy mennyi ideig hello teljes eredő videó kell lennie.</span><span class="sxs-lookup"><span data-stu-id="169bb-134">Integer that specifies how long hello entire resultant video shall be.</span></span>  <span data-ttu-id="169bb-135">Alapértelmezett eredeti videó időtartama függ.</span><span class="sxs-lookup"><span data-stu-id="169bb-135">Default depends on original video duration.</span></span> |

<span data-ttu-id="169bb-136">hello következő táblázatban található hello alapértelmezett időtartamot, amikor **maxMotionThumbnailInSecs** nem használatos.</span><span class="sxs-lookup"><span data-stu-id="169bb-136">hello following table describes hello default duration, when **maxMotionThumbnailInSecs** is not used.</span></span>

|  |  |  |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="169bb-137">Videó időtartama</span><span class="sxs-lookup"><span data-stu-id="169bb-137">Video duration</span></span> |<span data-ttu-id="169bb-138">d < 3 perc</span><span class="sxs-lookup"><span data-stu-id="169bb-138">d < 3 min</span></span> |<span data-ttu-id="169bb-139">3 perc < d < 15 perc</span><span class="sxs-lookup"><span data-stu-id="169bb-139">3 min < d < 15 min</span></span> |
| <span data-ttu-id="169bb-140">A miniatűr időtartama</span><span class="sxs-lookup"><span data-stu-id="169bb-140">Thumbnail duration</span></span> |<span data-ttu-id="169bb-141">15 másodperc (2-3 színfalak)</span><span class="sxs-lookup"><span data-stu-id="169bb-141">15 sec (2-3 scenes)</span></span> |<span data-ttu-id="169bb-142">30 másodperc (3-5 színfalak)</span><span class="sxs-lookup"><span data-stu-id="169bb-142">30 sec (3-5 scenes)</span></span> |

<span data-ttu-id="169bb-143">hello következő JSON paramétereinek érhető el.</span><span class="sxs-lookup"><span data-stu-id="169bb-143">hello following JSON sets available parameters.</span></span>

    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="net-sample-code"></a><span data-ttu-id="169bb-144">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="169bb-144">.NET sample code</span></span>

<span data-ttu-id="169bb-145">hello következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="169bb-145">hello following program shows how to:</span></span>

1. <span data-ttu-id="169bb-146">Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.</span><span class="sxs-lookup"><span data-stu-id="169bb-146">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="169bb-147">Létrehoz egy feladatot a json-készlet a következő hello tartalmazó konfigurációs fájl alapján videó miniatűr feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="169bb-147">Creates a job with a video thumbnail task based on a configuration file that contains hello following json preset.</span></span> 
   
        {                
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }
3. <span data-ttu-id="169bb-148">Hello kimeneti fájlokat tölti le.</span><span class="sxs-lookup"><span data-stu-id="169bb-148">Downloads hello output files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="169bb-149">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="169bb-149">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="169bb-150">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="169bb-150">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="169bb-151">Példa</span><span class="sxs-lookup"><span data-stu-id="169bb-151">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoSummarization
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


                // Run hello thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }

            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");

                // Get a reference tooAzure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);

                // Use hello following event handler toocheck job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch hello job.
                job.Submit();

                // Check job execution and wait for job toofinish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

                progressJobTask.Wait();

                // If job state is Error, hello event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }

                return job.OutputMediaAssets[0];
            }

            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);

                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);

                return asset;
            }

            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }

            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();

                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));

                return processor;
            }

            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);

                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
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
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }

        }
    }

### <a name="video-thumbnail-output"></a><span data-ttu-id="169bb-152">A miniatűr videokimenetéhez</span><span class="sxs-lookup"><span data-stu-id="169bb-152">Video thumbnail output</span></span>
[<span data-ttu-id="169bb-153">A miniatűr videokimenetéhez</span><span class="sxs-lookup"><span data-stu-id="169bb-153">Video thumbnail output</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="media-services-learning-paths"></a><span data-ttu-id="169bb-154">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="169bb-154">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="169bb-155">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="169bb-155">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="169bb-156">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="169bb-156">Related links</span></span>
[<span data-ttu-id="169bb-157">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="169bb-157">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="169bb-158">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="169bb-158">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


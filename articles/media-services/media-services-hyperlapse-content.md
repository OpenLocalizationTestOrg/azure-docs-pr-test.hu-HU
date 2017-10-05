---
title: "Médiafájlok feldolgozása az Azure Media hyperlapse használatával |} Microsoft Docs"
description: "Az Azure Media Hyperlapse zökkenőmentes idő letelt videók első, aki vagy művelet – kamera tartalmat hoz létre. Ez a témakör bemutatja, hogyan Media Indexer használatára."
services: media-services
documentationcenter: 
author: asolanki
manager: johndeu
editor: 
ms.assetid: 37d54db6-9cf3-4ae9-b3c6-0d29c744e965
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/02/2017
ms.author: adsolank
ms.openlocfilehash: 02f634c2af04b6b372642ab0e6a17a5d29f16450
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a><span data-ttu-id="7a187-104">Médiafájlok feldolgozása az Azure Media hyperlapse használatával</span><span class="sxs-lookup"><span data-stu-id="7a187-104">Hyperlapse Media Files with Azure Media Hyperlapse</span></span>
<span data-ttu-id="7a187-105">Az Azure Media Hyperlapse egy Media processzor (MP), amely zökkenőmentes idő letelt videók hoz első, aki vagy művelet – kamera tartalomról.</span><span class="sxs-lookup"><span data-stu-id="7a187-105">Azure Media Hyperlapse is a Media Processor (MP) that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="7a187-106">A felhő alapú testvérének [Microsoft Research asztali Hyperlapse Pro és Hyperlapse Mobile phone-alapú](http://aka.ms/hyperlapse), az Azure Media Services Microsoft Hyperlapse használja az Azure Media Services Media feldolgozása a jelentős mértékű vízszintes skálázása és parallelize platform tömeges Hyperlapse feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="7a187-106">The cloud-based sibling to [Microsoft Research's desktop Hyperlapse Pro and phone-based Hyperlapse Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse for Azure Media Services utilizes the massive scale of the Azure Media Services Media Processing platform to horizontally scale and parallelize bulk Hyperlapse processing.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7a187-107">Microsoft Hyperlapse legmegfelelőbb első, aki a tartalomhoz a mozgóátlag fényképezőgép célja.</span><span class="sxs-lookup"><span data-stu-id="7a187-107">Microsoft Hyperlapse is designed to work best on first-person content with a moving camera.</span></span>  <span data-ttu-id="7a187-108">Bár továbbra is-kamerák felvételei is működik, a teljesítmény és az Azure Media Hyperlapse Media processzor-minőségi nem garantálható tartalmat más típusú.</span><span class="sxs-lookup"><span data-stu-id="7a187-108">Although still-camera footage can still work, the performance and quality of the Azure Media Hyperlapse Media Processor cannot be guaranteed for other types of content.</span></span>  <span data-ttu-id="7a187-109">További információk a Microsoft Hyperlapse az Azure Media Services, és tekintse meg néhány Mintavideók, tekintse meg a [bevezető blogbejegyzés](http://aka.ms/azurehyperlapseblog) a nyilvános előzetes verziójához.</span><span class="sxs-lookup"><span data-stu-id="7a187-109">To learn more about Microsoft Hyperlapse for Azure Media Services and see some example videos, check out the [introductory blog post](http://aka.ms/azurehyperlapseblog) from the public preview.</span></span>
> 
> 

<span data-ttu-id="7a187-110">Egy Azure Media Hyperlapse feladat végrehajtásához szükséges, adjon meg egy MP4, MOV vagy WMV objektumfájlt együtt, amely meghatározza, melyik keretek videó idő letelt konfigurációs fájlt, és milyen sebességgel (pl. első 10 000 keretet 2 x).</span><span class="sxs-lookup"><span data-stu-id="7a187-110">An Azure Media Hyperlapse job takes as input an MP4, MOV, or WMV asset file along with a configuration file that specifies which frames of video should be time-lapsed and to what speed (e.g. first 10,000 frames at 2x).</span></span>  <span data-ttu-id="7a187-111">Az eredménye egy stabil és az idő letelt verzióinak a bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="7a187-111">The output is a stabilized and time-lapsed rendition of the input video.</span></span>

<span data-ttu-id="7a187-112">A legújabb Azure Media Hyperlapse frissítéseket, lásd: [Media Services blogok](https://azure.microsoft.com/blog/topics/media-services/).</span><span class="sxs-lookup"><span data-stu-id="7a187-112">For the latest Azure Media Hyperlapse updates, see [Media Services blogs](https://azure.microsoft.com/blog/topics/media-services/).</span></span>

## <a name="hyperlapse-an-asset"></a><span data-ttu-id="7a187-113">Hyperlapse egy eszköz</span><span class="sxs-lookup"><span data-stu-id="7a187-113">Hyperlapse an asset</span></span>
<span data-ttu-id="7a187-114">Először szüksége lesz Azure Media Services töltse fel a kívánt bemeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="7a187-114">First you will need to upload your desired input file to Azure Media Services.</span></span>  <span data-ttu-id="7a187-115">Adminisztrációjával kapcsolatos feltöltését és kielégíteni tartalomkezelési fogalmak kapcsolatos további tudnivalókért olvassa el a [tartalomkezelési cikk](media-services-portal-vod-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7a187-115">To learn more about the concepts involved with uploading and managing content, read the [content management article](media-services-portal-vod-get-started.md).</span></span>

### <span data-ttu-id="7a187-116"><a id="configuration"></a>Hyperlapse konfigurációs beállításkészlet</span><span class="sxs-lookup"><span data-stu-id="7a187-116"><a id="configuration"></a>Configuration Preset for Hyperlapse</span></span>
<span data-ttu-id="7a187-117">Ha a tartalom a Media Services-fiókját, szüksége lesz a konfigurációs készlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7a187-117">Once your content is in your Media Services account, you will need to construct your configuration preset.</span></span>  <span data-ttu-id="7a187-118">Az alábbi táblázat ismerteti a felhasználó által megadott mezőket:</span><span class="sxs-lookup"><span data-stu-id="7a187-118">The following table explains the user-specified fields:</span></span>

| <span data-ttu-id="7a187-119">Mező</span><span class="sxs-lookup"><span data-stu-id="7a187-119">Field</span></span> | <span data-ttu-id="7a187-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="7a187-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7a187-121">StartFrame</span><span class="sxs-lookup"><span data-stu-id="7a187-121">StartFrame</span></span> |<span data-ttu-id="7a187-122">A keret, amelyre a Microsoft Hyperlapse feldolgozási el kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="7a187-122">The frame upon which the Microsoft Hyperlapse processing should begin.</span></span> |
| <span data-ttu-id="7a187-123">NumFrames</span><span class="sxs-lookup"><span data-stu-id="7a187-123">NumFrames</span></span> |<span data-ttu-id="7a187-124">Keretek feldolgozni száma</span><span class="sxs-lookup"><span data-stu-id="7a187-124">The number of frames to process</span></span> |
| <span data-ttu-id="7a187-125">Gyorsaság</span><span class="sxs-lookup"><span data-stu-id="7a187-125">Speed</span></span> |<span data-ttu-id="7a187-126">A tényező, amellyel a bemeneti videó felgyorsítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="7a187-126">The factor with which to speed up the input video.</span></span> |

<span data-ttu-id="7a187-127">A következő egy példa egy szabványos konfigurációs fájl XML- és a JSON:</span><span class="sxs-lookup"><span data-stu-id="7a187-127">The following is an example of a conformant configuration file in XML and JSON:</span></span>

<span data-ttu-id="7a187-128">**XML-készletet:**</span><span class="sxs-lookup"><span data-stu-id="7a187-128">**XML preset:**</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

<span data-ttu-id="7a187-129">**JSON-készletet:**</span><span class="sxs-lookup"><span data-stu-id="7a187-129">**JSON preset:**</span></span>

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

### <span data-ttu-id="7a187-130"><a id="sample_code"></a>Microsoft Hyperlapse az AMS .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="7a187-130"><a id="sample_code"></a> Microsoft Hyperlapse with the AMS .NET SDK</span></span>
<span data-ttu-id="7a187-131">A következő metódus feltölt egy médiafájlt eszközként, és létrehoz egy feladatot az Azure Media Hyperlapse Media processzorral rendelkező.</span><span class="sxs-lookup"><span data-stu-id="7a187-131">The following method uploads a media file as an asset and creates a job with the Azure Media Hyperlapse Media Processor.</span></span>

> [!NOTE]
> <span data-ttu-id="7a187-132">Egy CloudMediaContext már rendelkezik a neve "context" a kód működéséhez a hatókörben.</span><span class="sxs-lookup"><span data-stu-id="7a187-132">You should already have a CloudMediaContext in scope with the name "context" for this code to work.</span></span>  <span data-ttu-id="7a187-133">Ezzel kapcsolatos további tudnivalókért olvassa el a [tartalomkezelési cikk](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7a187-133">To learn more about this, read the [content management article](media-services-dotnet-get-started.md).</span></span>
> 
> [!NOTE]
> <span data-ttu-id="7a187-134">A karakterlánc-argumentum "hyperConfig" kellene lennie a szabványos konfigurációs készlet JSON vagy XML fent leírt módon.</span><span class="sxs-lookup"><span data-stu-id="7a187-134">The string argument "hyperConfig" is expected to be a conformant configuration preset in either JSON or XML as described above.</span></span>
> 
> 

        static bool RunHyperlapseJob(string input, string output, string hyperConfig)
        {
            // create asset with input file
            IAsset asset = context
            .Assets
            .CreateAssetAndUploadSingleFile(input, "My Hyperlapse Input", AssetCreationOptions.None);

            // grab instances of Azure Media Hyperlapse MP
            IMediaProcessor mp = context
            .MediaProcessors
            .GetLatestMediaProcessorByName("Azure Media Hyperlapse");

            // create Job with Hyperlapse task
            IJob job = context
            .Jobs
            .Create(String.Format("Hyperlapse {0}", input));

            if (String.IsNullOrEmpty(hyperConfig))
            {
            // config cannot be empty
            return false;
            }

            hyperConfig = File.ReadAllText(hyperConfig);

            ITask hyperlapseTask = job.Tasks.AddNew("Hyperlapse task",
            mp,
            hyperConfig,
            TaskOptions.None);
            hyperlapseTask.InputAssets.Add(asset);
            hyperlapseTask.OutputAssets.AddNew("Hyperlapse output",
            AssetCreationOptions.None);

            job.Submit();

            // Create progress printing and querying tasks
            Task progressPrintTask = new Task(() =>
            {

            IJob jobQuery = null;
            do
            {
                var progressContext = context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}\t{2}",
                DateTime.Now,
                jobQuery.State,
                jobQuery.Tasks[0].Progress));
                Thread.Sleep(10000);
            }
            while (jobQuery.State != JobState.Finished &&
                                   jobQuery.State != JobState.Error &&
                                   jobQuery.State != JobState.Canceled);
                });
                
            progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));  
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <span data-ttu-id="7a187-135"><a id="file_types"></a>Támogatott fájltípusok</span><span class="sxs-lookup"><span data-stu-id="7a187-135"><a id="file_types"></a>Supported File types</span></span>
* <span data-ttu-id="7a187-136">MP4</span><span class="sxs-lookup"><span data-stu-id="7a187-136">MP4</span></span>
* <span data-ttu-id="7a187-137">MOV</span><span class="sxs-lookup"><span data-stu-id="7a187-137">MOV</span></span>
* <span data-ttu-id="7a187-138">WMV</span><span class="sxs-lookup"><span data-stu-id="7a187-138">WMV</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="7a187-139">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="7a187-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7a187-140">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7a187-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="7a187-141">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="7a187-141">Related links</span></span>
[<span data-ttu-id="7a187-142">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="7a187-142">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="7a187-143">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="7a187-143">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


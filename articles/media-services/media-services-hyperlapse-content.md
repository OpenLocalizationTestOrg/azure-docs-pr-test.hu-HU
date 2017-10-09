---
title: "az Azure Media Hyperlapse médiafájlok aaaHyperlapse |} Microsoft Docs"
description: "Az Azure Media Hyperlapse zökkenőmentes idő letelt videók első, aki vagy művelet – kamera tartalmat hoz létre. Ez a témakör bemutatja, hogyan toouse Media Indexer."
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
ms.openlocfilehash: 85bb07206d0ca2f5b2fd0767e6ed4904195d3ab6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a><span data-ttu-id="d2014-104">Médiafájlok feldolgozása az Azure Media hyperlapse használatával</span><span class="sxs-lookup"><span data-stu-id="d2014-104">Hyperlapse Media Files with Azure Media Hyperlapse</span></span>
<span data-ttu-id="d2014-105">Az Azure Media Hyperlapse egy Media processzor (MP), amely zökkenőmentes idő letelt videók hoz első, aki vagy művelet – kamera tartalomról.</span><span class="sxs-lookup"><span data-stu-id="d2014-105">Azure Media Hyperlapse is a Media Processor (MP) that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="d2014-106">felhőalapú testvér túl hello[Microsoft Research asztali Hyperlapse Pro és Hyperlapse Mobile phone-alapú](http://aka.ms/hyperlapse), az Azure Media Services Microsoft Hyperlapse hello jelentős mértékű az Azure Media Services Media hello használja A platform toohorizontally feldolgozási méretezhető és tömeges parallelize Hyperlapse feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="d2014-106">hello cloud-based sibling too[Microsoft Research's desktop Hyperlapse Pro and phone-based Hyperlapse Mobile](http://aka.ms/hyperlapse), Microsoft Hyperlapse for Azure Media Services utilizes hello massive scale of hello Azure Media Services Media Processing platform toohorizontally scale and parallelize bulk Hyperlapse processing.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2014-107">Microsoft Hyperlapse tervezett toowork ajánlott a mozgóátlag fényképezőgép az első, aki a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="d2014-107">Microsoft Hyperlapse is designed toowork best on first-person content with a moving camera.</span></span>  <span data-ttu-id="d2014-108">Bár továbbra is-kamerák felvételei is működik, hello teljesítményének és minőségének hello Azure Media Hyperlapse Media processzor nem garantálható tartalmat más típusú.</span><span class="sxs-lookup"><span data-stu-id="d2014-108">Although still-camera footage can still work, hello performance and quality of hello Azure Media Hyperlapse Media Processor cannot be guaranteed for other types of content.</span></span>  <span data-ttu-id="d2014-109">További információk az Azure Media Services Microsoft Hyperlapse toolearn és néhány Mintavideók megtekintéséhez tekintse meg a hello [bevezető blogbejegyzés](http://aka.ms/azurehyperlapseblog) a hello nyilvános előzetes verziójához.</span><span class="sxs-lookup"><span data-stu-id="d2014-109">toolearn more about Microsoft Hyperlapse for Azure Media Services and see some example videos, check out hello [introductory blog post](http://aka.ms/azurehyperlapseblog) from hello public preview.</span></span>
> 
> 

<span data-ttu-id="d2014-110">Egy Azure Media Hyperlapse feladat végrehajtásához szükséges, adjon meg egy MP4, MOV vagy WMV objektumfájlt egy konfigurációs fájl, amely meghatározza, melyik keretek videó idő letelt és toowhat sebesség együtt (pl. első 10 000 keretet 2 x).</span><span class="sxs-lookup"><span data-stu-id="d2014-110">An Azure Media Hyperlapse job takes as input an MP4, MOV, or WMV asset file along with a configuration file that specifies which frames of video should be time-lapsed and toowhat speed (e.g. first 10,000 frames at 2x).</span></span>  <span data-ttu-id="d2014-111">hello eredménye egy stabil és az idő letelt verzióinak hello bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="d2014-111">hello output is a stabilized and time-lapsed rendition of hello input video.</span></span>

<span data-ttu-id="d2014-112">Lásd: hello Azure Media Hyperlapse legújabb [Media Services blogok](https://azure.microsoft.com/blog/topics/media-services/).</span><span class="sxs-lookup"><span data-stu-id="d2014-112">For hello latest Azure Media Hyperlapse updates, see [Media Services blogs](https://azure.microsoft.com/blog/topics/media-services/).</span></span>

## <a name="hyperlapse-an-asset"></a><span data-ttu-id="d2014-113">Hyperlapse egy eszköz</span><span class="sxs-lookup"><span data-stu-id="d2014-113">Hyperlapse an asset</span></span>
<span data-ttu-id="d2014-114">Először be kell tooupload a kívánt bemeneti fájl tooAzure Media Services.</span><span class="sxs-lookup"><span data-stu-id="d2014-114">First you will need tooupload your desired input file tooAzure Media Services.</span></span>  <span data-ttu-id="d2014-115">További részletek toolearn hello fogalmakat feltöltése és tartalomkezelés, olvassa el a hello [tartalomkezelési cikk](media-services-portal-vod-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d2014-115">toolearn more about hello concepts involved with uploading and managing content, read hello [content management article](media-services-portal-vod-get-started.md).</span></span>

### <span data-ttu-id="d2014-116"><a id="configuration"></a>Hyperlapse konfigurációs beállításkészlet</span><span class="sxs-lookup"><span data-stu-id="d2014-116"><a id="configuration"></a>Configuration Preset for Hyperlapse</span></span>
<span data-ttu-id="d2014-117">Ha a tartalom a Media Services-fiókját, szüksége lesz a konfigurációs készlet tooconstruct.</span><span class="sxs-lookup"><span data-stu-id="d2014-117">Once your content is in your Media Services account, you will need tooconstruct your configuration preset.</span></span>  <span data-ttu-id="d2014-118">a következő táblázat hello hello felhasználó által megadott mezőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="d2014-118">hello following table explains hello user-specified fields:</span></span>

| <span data-ttu-id="d2014-119">Mező</span><span class="sxs-lookup"><span data-stu-id="d2014-119">Field</span></span> | <span data-ttu-id="d2014-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="d2014-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d2014-121">StartFrame</span><span class="sxs-lookup"><span data-stu-id="d2014-121">StartFrame</span></span> |<span data-ttu-id="d2014-122">hello keret, mely hello Microsoft Hyperlapse feldolgozási el kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="d2014-122">hello frame upon which hello Microsoft Hyperlapse processing should begin.</span></span> |
| <span data-ttu-id="d2014-123">NumFrames</span><span class="sxs-lookup"><span data-stu-id="d2014-123">NumFrames</span></span> |<span data-ttu-id="d2014-124">keretek tooprocess hello száma</span><span class="sxs-lookup"><span data-stu-id="d2014-124">hello number of frames tooprocess</span></span> |
| <span data-ttu-id="d2014-125">Gyorsaság</span><span class="sxs-lookup"><span data-stu-id="d2014-125">Speed</span></span> |<span data-ttu-id="d2014-126">hello tényező, mely toospeed hello bemeneti videó fel.</span><span class="sxs-lookup"><span data-stu-id="d2014-126">hello factor with which toospeed up hello input video.</span></span> |

<span data-ttu-id="d2014-127">hello az alábbiakban látható egy példa egy szabványos konfigurációs fájl XML- és a JSON:</span><span class="sxs-lookup"><span data-stu-id="d2014-127">hello following is an example of a conformant configuration file in XML and JSON:</span></span>

<span data-ttu-id="d2014-128">**XML-készletet:**</span><span class="sxs-lookup"><span data-stu-id="d2014-128">**XML preset:**</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

<span data-ttu-id="d2014-129">**JSON-készletet:**</span><span class="sxs-lookup"><span data-stu-id="d2014-129">**JSON preset:**</span></span>

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

### <span data-ttu-id="d2014-130"><a id="sample_code"></a>Az AMS .NET SDK hello Microsoft Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="d2014-130"><a id="sample_code"></a> Microsoft Hyperlapse with hello AMS .NET SDK</span></span>
<span data-ttu-id="d2014-131">hello következő metódus feltölt egy médiafájlt eszközként, és létrehoz egy feladatot az Azure Media Hyperlapse Media processzor hello.</span><span class="sxs-lookup"><span data-stu-id="d2014-131">hello following method uploads a media file as an asset and creates a job with hello Azure Media Hyperlapse Media Processor.</span></span>

> [!NOTE]
> <span data-ttu-id="d2014-132">Már rendelkezik a egy CloudMediaContext ez kód toowork a hello neve "context" hatókörébe.</span><span class="sxs-lookup"><span data-stu-id="d2014-132">You should already have a CloudMediaContext in scope with hello name "context" for this code toowork.</span></span>  <span data-ttu-id="d2014-133">További információk a, olvasási hello toolearn [tartalomkezelési cikk](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d2014-133">toolearn more about this, read hello [content management article](media-services-dotnet-get-started.md).</span></span>
> 
> [!NOTE]
> <span data-ttu-id="d2014-134">hello karakterlánc "hyperConfig" argumentum egy szabványos konfigurációs készlet JSON vagy a fent leírt módon XML várt toobe.</span><span class="sxs-lookup"><span data-stu-id="d2014-134">hello string argument "hyperConfig" is expected toobe a conformant configuration preset in either JSON or XML as described above.</span></span>
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

            // If job state is Error, hello event handling
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

### <span data-ttu-id="d2014-135"><a id="file_types"></a>Támogatott fájltípusok</span><span class="sxs-lookup"><span data-stu-id="d2014-135"><a id="file_types"></a>Supported File types</span></span>
* <span data-ttu-id="d2014-136">MP4</span><span class="sxs-lookup"><span data-stu-id="d2014-136">MP4</span></span>
* <span data-ttu-id="d2014-137">MOV</span><span class="sxs-lookup"><span data-stu-id="d2014-137">MOV</span></span>
* <span data-ttu-id="d2014-138">WMV</span><span class="sxs-lookup"><span data-stu-id="d2014-138">WMV</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="d2014-139">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="d2014-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="d2014-140">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="d2014-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="d2014-141">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="d2014-141">Related links</span></span>
[<span data-ttu-id="d2014-142">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="d2014-142">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="d2014-143">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="d2014-143">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


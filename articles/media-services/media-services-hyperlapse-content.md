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
# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Médiafájlok feldolgozása az Azure Media hyperlapse használatával
Az Azure Media Hyperlapse egy Media processzor (MP), amely zökkenőmentes idő letelt videók hoz első, aki vagy művelet – kamera tartalomról.  felhőalapú testvér túl hello[Microsoft Research asztali Hyperlapse Pro és Hyperlapse Mobile phone-alapú](http://aka.ms/hyperlapse), az Azure Media Services Microsoft Hyperlapse hello jelentős mértékű az Azure Media Services Media hello használja A platform toohorizontally feldolgozási méretezhető és tömeges parallelize Hyperlapse feldolgozása.

> [!IMPORTANT]
> Microsoft Hyperlapse tervezett toowork ajánlott a mozgóátlag fényképezőgép az első, aki a tartalomhoz.  Bár továbbra is-kamerák felvételei is működik, hello teljesítményének és minőségének hello Azure Media Hyperlapse Media processzor nem garantálható tartalmat más típusú.  További információk az Azure Media Services Microsoft Hyperlapse toolearn és néhány Mintavideók megtekintéséhez tekintse meg a hello [bevezető blogbejegyzés](http://aka.ms/azurehyperlapseblog) a hello nyilvános előzetes verziójához.
> 
> 

Egy Azure Media Hyperlapse feladat végrehajtásához szükséges, adjon meg egy MP4, MOV vagy WMV objektumfájlt egy konfigurációs fájl, amely meghatározza, melyik keretek videó idő letelt és toowhat sebesség együtt (pl. első 10 000 keretet 2 x).  hello eredménye egy stabil és az idő letelt verzióinak hello bemeneti videó.

Lásd: hello Azure Media Hyperlapse legújabb [Media Services blogok](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse egy eszköz
Először be kell tooupload a kívánt bemeneti fájl tooAzure Media Services.  További részletek toolearn hello fogalmakat feltöltése és tartalomkezelés, olvassa el a hello [tartalomkezelési cikk](media-services-portal-vod-get-started.md).

### <a id="configuration"></a>Hyperlapse konfigurációs beállításkészlet
Ha a tartalom a Media Services-fiókját, szüksége lesz a konfigurációs készlet tooconstruct.  a következő táblázat hello hello felhasználó által megadott mezőket ismerteti:

| Mező | Leírás |
| --- | --- |
| StartFrame |hello keret, mely hello Microsoft Hyperlapse feldolgozási el kell kezdődnie. |
| NumFrames |keretek tooprocess hello száma |
| Gyorsaság |hello tényező, mely toospeed hello bemeneti videó fel. |

hello az alábbiakban látható egy példa egy szabványos konfigurációs fájl XML- és a JSON:

**XML-készletet:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON-készletet:**

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

### <a id="sample_code"></a>Az AMS .NET SDK hello Microsoft Hyperlapse
hello következő metódus feltölt egy médiafájlt eszközként, és létrehoz egy feladatot az Azure Media Hyperlapse Media processzor hello.

> [!NOTE]
> Már rendelkezik a egy CloudMediaContext ez kód toowork a hello neve "context" hatókörébe.  További információk a, olvasási hello toolearn [tartalomkezelési cikk](media-services-dotnet-get-started.md).
> 
> [!NOTE]
> hello karakterlánc "hyperConfig" argumentum egy szabványos konfigurációs készlet JSON vagy a fent leírt módon XML várt toobe.
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

### <a id="file_types"></a>Támogatott fájltípusok
* MP4
* MOV
* WMV

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Kapcsolódó hivatkozások
[Azure Media Services elemző áttekintése](media-services-analytics-overview.md)

[Az Azure Médiaelemzés bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


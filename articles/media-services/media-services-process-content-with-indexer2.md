---
title: "Azure Media Indexer 2 előnézettel médiafájlok aaaIndexing |} Microsoft Docs"
description: "Az Azure Media Indexer lehetővé teszi a kereshető médiafájlok toomake tartalmát, és a teljes szöveges Beszélgetés szövegének toogenerate a feliratok és kulcsszavak lezárni. Ez a témakör azt mutatja be, hogyan toouse Media Indexer 2 megtekintése."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 85d25525-a498-44eb-ae3a-2ca5ceb8e53d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: f83fa0db58b828ffa29933d68ce108b4906dcd78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Az Azure Media Indexer 2 előzetes verziójú médiafájlok indexelő
## <a name="overview"></a>Áttekintés
Hello **Azure Media Indexer 2 Preview** media processzor (MP) lehetővé teszi toomake médiafájlok és a tartalom kereshető, valamint készítése lezárt feliratok nyomon követi. Az összehasonlított toohello korábbi verziójának [Azure Media Indexer](media-services-index-content.md), **Azure Media Indexer 2 Preview** elvégzi a gyorsabb indexelő és nyelvi szélesebb körű támogatást nyújt. Támogatott nyelvek angol, német, francia, német, olasz, kínai (Mandarin, egyszerűsített), portugál, arab és japán.

Hello **Azure Media Indexer 2 Preview** felügyeleti csomag jelenleg előzetes verzió.

Ez a témakör bemutatja, hogyan rendelkező toocreate indexelő feladat **Azure Media Indexer 2 Preview**.

> [!NOTE]
> a következő szempontok hello vonatkoznak:
> 
> Indexer 2 Azure Kínában és Azure Government nem támogatott.
> 
> Amikor indexelő tartalmat, győződjön meg arról, hogy toouse világossá beszéd (nélkül háttér zene, zaj, hatások vagy mikrofon hiss) rendelkező médiafájlok. Néhány példa a megfelelő tartalom: értekezletek, előadások vagy bemutatók rögzíti. hello következő tartalom nem feltétlenül alkalmas indexelő: filmek, tévéműsorok, semmi a vegyes hang- és hatást, rosszul rögzített háttérzaj (hiss) tartalmát.
> 
> 

Ez a témakör kapcsolatos részleteket nyújt **Azure Media Indexer 2 Preview** és bemutatja, hogyan toouse a Media Services SDK for .NET

## <a name="input-and-output-files"></a>Bemeneti és kimeneti fájlok
### <a name="input-files"></a>A bemeneti fájlok
Hang- vagy fájlok

### <a name="output-files"></a>Kimeneti fájlok
Az indexelő feladat feliratfájlokat fájlok hozhat létre a következő formátumok hello:  

* **SZÁMI**
* **TTML**
* **WebVTT**

Bezárt felirat (CC) fájlok az alábbi formátumokban lehet használt toomake hang, és a videó elérhető toopeople nyújtanak segítséget fogyatékkal fájlokat.

## <a name="task-configuration-preset"></a>A feladat konfigurációja (beállítás)
A feladat létrehozása az indexelő **Azure Media Indexer 2 Preview**, meg kell adnia egy konfigurációs készletet.

hello következő JSON paramétereinek érhető el.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

## <a name="supported-languages"></a>Támogatott nyelvek
Az Azure Media Indexer 2 Preview beszéd-szöveg hello (megadásakor hello Nyelvnév hello feladat konfigurációs, használjon 4 karakterből álló kódot szögletes zárójelbe alább látható módon) a következő nyelveket támogatja:

* Angol [EnUs]
* Spanyol [EsEs]
* Kínai (Mandarin, egyszerűsített) [ZhCn]
* Francia [FrFr]
* Német [DeDe]
* Olasz [ItIt]
* Portugál [PtBr]
* Arab (egyiptomi) [ArEg]
* Japán [JaJp]
* Orosz [RuRu]
* Brit angol [EnGb]
* Mexikói spanyol [EsMx] 

## <a name="supported-file-types"></a>Támogatott fájltípusok

Támogatott fájltípusok kapcsolatos információkért lásd: hello [kodekek/formátum támogatott](media-services-media-encoder-standard-formats.md#input-containerfile-formats) szakasz.

## <a name="net-sample-code"></a>.NET mintakód

hello következő program bemutatja hogyan:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.
2. Hozzon létre egy feladatot az indexelési feladat a json-készlet a következő hello tartalmazó konfigurációs fájl alapján.
   
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }
3. Hello kimeneti fájlok letöltéséhez. 
   
#### <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Példa

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace IndexContent
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

                // Run indexing job.
                var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                            @"C:\supportFiles\Indexer\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
            }

            static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Indexing Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Indexing Job");

                // Get a reference tooAzure Media Indexer 2 Preview.
                string MediaProcessorName = "Azure Media Indexer 2 Preview";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Indexing Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset toobe indexed.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Kapcsolódó hivatkozások
[Azure Media Services elemző áttekintése](media-services-analytics-overview.md)

[Az Azure Médiaelemzés bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


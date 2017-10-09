---
title: az Azure Media Analytics aaaDetect mozdulatok |} Microsoft Docs
description: "hello Azure Media mozgásérzékelő media processzor (MP) lehetővé teszi, hogy Ön tooefficiently azonosítani egy egyébként hosszú és Eseménytelen video házirendsablonokkal szakaszait."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: d144f813-1a55-442f-a895-5c4cb6d0aeae
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: milanga;juliako;
ms.openlocfilehash: cb431375c92222053ed2239dd4e45767524dab68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a>Az Azure Media Analytics mozdulatok észlelése
## <a name="overview"></a>Áttekintés
Hello **Azure Media mozgásérzékelő** media processzor (MP) lehetővé teszi, hogy Ön tooefficiently azonosítani egy egyébként hosszú és Eseménytelen video házirendsablonokkal szakaszait. Mozgásérzékelés statikus kamera felvételei tooidentify szakaszok hello videó hol mozgásérzékelési jelentkezik is használhatók. A metaadatok időbélyegeket és a régió, ahol hello esemény történt határolókeret hello tartalmazó JSON-fájlt hoz létre.

Cél biztonsági videó hírcsatornák, ez a technológia képes toocategorize mozgásérzékelési kapcsolódó eseményeket és vakriasztások például árnyékok és megvilágítási módosításokat. Ez lehetővé teszi a kamera hírcsatornák toogenerate biztonsági riasztások alatt címünkre végtelen irreleváns események, ugyanakkor nem képes tooextract perc múlva a rendkívül hosszú felügyeleti videók érdeklő nélkül.

Hello **Azure Media mozgásérzékelő** felügyeleti csomag jelenleg előzetes verzió.

Ez a témakör kapcsolatos részleteket nyújt **Azure Media mozgásérzékelő** és bemutatja, hogyan toouse a Media Services SDK for .NET

## <a name="motion-detector-input-files"></a>Mozgásérzékelő – érzékelő bemeneti fájlok
Videofájlok lejátszását. Jelenleg a következő formátumok hello támogatottak: MP4 MOV és WMV.

## <a name="task-configuration-preset"></a>A feladat konfigurációja (beállítás)
A feladat létrehozásakor **Azure Media mozgásérzékelő**, meg kell adnia egy konfigurációs készletet. 

### <a name="parameters"></a>Paraméterek
A következő paraméterek hello használhatja:

| Név | Beállítások | Leírás | Alapértelmezett |
| --- | --- | --- | --- |
| sensitivityLevel |Karakterlánc: "alacsony", "közepes", "magas" |Beállítja hello bizalmassági szint mely mozdulatok jelenti. Állítsa be úgy a vakriasztások tooadjust mennyiségét. |"közepes" |
| frameSamplingValue |Pozitív egész szám |Beállítja hello gyakoriság mely algoritmus futtatása. 1 egyenlő minden keret, 2 azt jelenti, hogy minden 2. keret, és így tovább. |1 |
| detectLightChange |Logikai: "true", "false" |Megadja, hogy e könnyű módosítások kimutatott hello eredmények |"False" |
| mergeTimeThreshold |Idő xs: Óó: pp:<br/>. Példa: 00:00:03 |Megadja, ahol 2 esemény lesz kombinált és 1 jelentett mozgásérzékelési események között hello időkerete. |00:00:00 |
| detectionZones |A tömb észlelési zónák:<br/>-Észlelési zóna: 3 vagy több pontok bájttömb<br/>-Pont egy x és y koordináta a 0 too1. |Ismerteti a sokszög észlelési zónák toobe használt hello listája.<br/>Az ID, az első egy lesznek "id" hello hello zónák lesz jelentve az eredmények: 0 |Egy zóna, amely magában foglalja a teljes keret hello. |

### <a name="json-example"></a>JSON-példa
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }


## <a name="motion-detector-output-files"></a>Mozgásérzékelő – érzékelő kimeneti fájlok
A mozgás észlelése hello kimeneti eszköz, amely hello mozgásérzékelési riasztások és azok kategóriák belül hello videó ismerteti adja vissza egy JSON-fájlt. hello fájl hello időpontja és időtartama található videó hello mozgási információkat tartalmaznak.

hello Mozgásérzékelési érzékelő API mutatók biztosít, ha nincsenek objektumok mozgásérzékelési rögzített háttér video (pl. egy felügyeleti videó). Mozgásérzékelő hello képzett tooreduce téves riasztásokat, például a megvilágítási és árnyékmásolat módosítások. Aktuális korlátozások hello algoritmusok éjszakai stratégiai videók, félig átlátható és kis objektumok tartalmaznak.

### <a id="output_elements"></a>Hello kimeneti JSON-fájl elemeinek
> [!NOTE]
> Hello a legfrissebb verzióban hello kimeneti JSON formátumban megváltozott, és az egyes ügyfelek használhatatlanná tévő változást jelöl.
> 
> 

hello következő táblázatban hello kimeneti JSON-fájl elemeinek.

| Elem | Leírás |
| --- | --- |
| Verzió |Hello videó API verziója toohello utal. hello jelenlegi verzió: 2. |
| időskálára |"Ticks" hello videó másodpercenként. |
| Eltolás |a "ticks" időbélyegeket hello időeltolódás. Videó API-k 1.0-s verziójában az mindig 0 lesz. A jövőben támogatott forgatókönyveket, ezt az értéket módosíthatja. |
| képkockasebességhez |Videó hello képkockasebessége. |
| Szélesség, Hosszúság |Toohello szélessége és magassága képpontban videó hello hivatkozik. |
| Indítás |hello indítsa el a "ticks" időbélyegző. |
| Időtartam |hello esemény, a "ticks" Hello hossza. |
| időköz |minden egyes bejegyzés hello esemény, a "ticks" Hello időközt. |
| Események |Minden esemény-töredéket adott időtartamig belül észlelt hello mozgásérzékelési tartalmazza. |
| Típus |Az aktuális verzióban hello Ez a "2" általános mozgásérzékelési. Ez a címke videó API-k hello rugalmasságot toocategorize mozgásérzékelő – adja meg a jövőbeli verziókkal. |
| RegionID |Ahogy fent, az mindig 0 lesz ebben a verzióban. Ezt a címkét ad Video API hello rugalmasságot toofind mozgásérzékelési különböző régiókban futó későbbi verzióiban. |
| Régiók |Toohello terület a videó ahol az Ön számára legfontosabb mozgásérzékelési hivatkozik. <br/><br/>-az "id" jelöli hello régió terület – ebben a verzióban van csak egy, azonosító: 0. <br/>-"type" az Ön számára legfontosabb a mozgásérzékelő – hello régió hello alakzat jelöli. Jelenleg a "téglalap", "sokszög" támogatottak.<br/> Ha a megadott "téglalap" hello régió dimenziója van X, Y, szélességét és magasságát. hello X és Y koordinátáit képviselő hello felső bal oldali Oxykiszolgáló koordinátáit 0.0 too1.0 normalizált skálája hello-régiót. hello szélességének és magasságának képviselő hello terület a 0.0 too1.0 normalizált skálája hello méretét. Hello aktuális verzió X, Y, szélességének és magasságának mindig rögzített 0, 0 és 1, 1. <br/>Ha a megadott "sokszög" hello régió pontokon dimenziója van. <br/> |
| töredék |hello metaadatok töredék nevű különböző részekre van darabolásos fel. Minden töredéke a start, a időtartama, a időszakának száma és a esemény (eke) tartalmaz. Nincsenek események a töredéket azt jelenti, hogy semmilyen adott kezdő időpontja és időtartama során észlelt. |
| Szögletes zárójelbe] |Minden egyes zárójel hello esemény egy időköz jelöli. Üres szögletes az adott időköz azt jelenti, hogy semmilyen volt észlelhető. |
| Helyek |Az új bejegyzést események hello helyre, ahol hello mozgásérzékelési történt sorolja fel. Ez a pontosabb, mint hello észlelési zónák. |

hello az alábbiakban látható egy JSON kimeneti példa

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],

    …
## <a name="limitations"></a>Korlátozások
* hello támogatott bemeneti videó formátumnak tartalmaznia kell MP4 MOV és WMV.
* Mozgásérzékelés álló háttér videók van optimalizálva. téves riasztásokat, például a megvilágítási érintő változtatások, valamint az árnyékok csökkentése hello algoritmus összpontosít.
* Néhány mozgásérzékelési tootechnical kihívást; miatt nem észlelhető például éjszaka stratégiai videók, félig átlátható és kis objektumok.

## <a name="net-sample-code"></a>.NET mintakód

hello következő program bemutatja hogyan:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.
2. Hozzon létre egy feladatot a json-készlet a következő hello tartalmazó konfigurációs fájl alapján Videós mozgásérzékelési észlelési feladatokkal. 
   
        {
          "Version": "1.0",
          "Options": {
            "SensitivityLevel": "medium",
            "FrameSamplingValue": 1,
            "DetectLightChange": "False",
            "MergeTimeThreshold":
            "00:00:02",
            "DetectionZones": [
              [
                {"x": 0, "y": 0},
                {"x": 0.5, "y": 0},
                {"x": 0, "y": 1}
               ],
              [
                {"x": 0.3, "y": 0.3},
                {"x": 0.55, "y": 0.3},
                {"x": 0.8, "y": 0.3},
                {"x": 0.8, "y": 0.55},
                {"x": 0.8, "y": 0.8},
                {"x": 0.55, "y": 0.8},
                {"x": 0.3, "y": 0.8},
                {"x": 0.3, "y": 0.55}
              ]
            ]
          }
        }
3. Hello kimeneti JSON-fájlok letöltéséhez. 

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

    namespace VideoMotionDetection
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

                // Run hello VideoMotionDetection job.
                var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoMotionDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
            }

            static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Motion Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Motion Detection Job");

                // Get a reference tooAzure Media Motion Detector.
                string MediaProcessorName = "Azure Media Motion Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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
[Az Azure Media Services mozgásérzékelő blog](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services elemző áttekintése](media-services-analytics-overview.md)

[Az Azure Médiaelemzés bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


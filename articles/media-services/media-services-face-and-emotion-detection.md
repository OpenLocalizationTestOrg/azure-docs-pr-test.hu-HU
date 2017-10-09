---
title: "aaaDetect Arcfelismerés és az Azure Media Analytics Érzelemfelismerési |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toodetect néz és az Azure Media Analytics érzelmek."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: f58d81d82dde08a694cdb4d92c6bab6a40a9c157
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Arcfelismerés és az Azure Media Analytics Érzelemfelismerési észlelése
## <a name="overview"></a>Áttekintés
Hello **Azure Media Arcfelismerési érzékelő** media processzor (MP) lehetővé teszi a toocount, nyomon követése típusú áthelyezések, és még a mérőműszer célközönség részvételét és reakciót arcfelismerést kifejezések keresztül. Ez a szolgáltatás két funkciókat tartalmazza: 

* **Arcfelismerési észlelése**
  
    Arcfelismerési észlelési talál, és nyomon követi a videó emberi lapjaira. Több lapokat észlelhető, és ezt követően nyomon követhetők egy JSON-fájl által visszaadott hello ideje és helye metaadatokkal körül, mozgás. Nyomon követése, során a program megpróbálja toogive egy egységes azonosító toohello azonos szembesülhetnek, amíg hello személy van Navigálás a képernyőn, még akkor is, ha kényszerítő vagy röviden hagyja hello keret.
  
  > [!NOTE]
  > Ez a szolgáltatás nem végez arcfelismerést. Személy hello keret hagyja, vagy a válik fedhetik túl hosszú kap egy új Azonosítót amikor azok tér vissza.
  > 
  > 
* **Érzelemfelismerés**
  
    Érzelemfelismerés hello Arcfelismerési észlelési Media processzor ad vissza elemzés több érzelmi attribútum hello lapok észlel, például Boldogsága, sadness, félelem, utasítás és egyéb választható összetevőként. 

Hello **Azure Media Arcfelismerési érzékelő** felügyeleti csomag jelenleg előzetes verzió.

Ez a témakör kapcsolatos részleteket nyújt **Azure Media Arcfelismerési érzékelő** és bemutatja, hogyan toouse a Media Services SDK for .NET.

## <a name="face-detector-input-files"></a>A bemeneti fájlok érzékelő szembesülhetnek
Videofájlok lejátszását. Jelenleg a következő formátumok hello támogatottak: MP4 MOV és WMV.

## <a name="face-detector-output-files"></a>Szembesülhetnek érzékelő kimeneti fájlok
hello arcfelismerési felderítését és a nyomon követési API biztosít magas pontosság arcfelismerési hely felderítését és a nyomkövetési, amely észlelni tudja a too64 emberi lapokat a videó be. Elülső lapok hello legjobb eredmények elérése érdekében ügyféloldali lapokat és kis lapok közben adja meg (kevesebb, mint vagy egyenlő too24x24 képpont) nem lehet olyan pontos.

hello észlelt és a nyomon követett lapokat a rendszer visszairányítja koordináták (bal, felső, szélességét és magasságát) jelző hello kép képpontban lapok hello helyét, valamint egy oldallal azonosító szám, amely jelzi, hello az, hogy egyes követését. Arcfelismerési azonosítószámát esetén körülmények nagyon eséllyel fordulnak elő tooreset hello elülső arcfelismerési elvesztése vagy átfedésben hello keretében, néhány első hozzárendelt több azonosítók egyének eredményez.

## <a id="output_elements"></a>Hello kimeneti JSON-fájl elemeinek

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

Arcfelismerési érzékelő (ahol hello események törik fel, ha túl nagy elérték) töredezettsége (ahol az időalapú adattömbök hello metaadatait is osztható fel és letöltheti a csak találja), és a szegmentálási technikák használja. Néhány egyszerű számítások segítségével hello adatok. Például, ha egy esemény használatába 6300 (ticks), egy időskálára 2997 (ticks/másodperc), és a 29,97 (keret/mp), majd képkockasebességhez:

* Start/időskálára = 2.1 másodperc
* X Framerate másodperc 63 keretek =

## <a name="face-detection-input-and-output-example"></a>Szembesülhetnek észlelési bemeneti és kimeneti példa
### <a name="input-video"></a>A bemeneti videó
[A bemeneti videó](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>A feladat konfigurációja (beállítás)
A feladat létrehozásakor **Azure Media Arcfelismerési érzékelő**, meg kell adnia egy konfigurációs készletet. a következő konfigurációs készlet hello folyamat csak arcfelismerési észlelése.

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a>Az attribútumok leírása
| Attribútum neve | Leírás |
| --- | --- |
| Mód |Gyors - feldolgozása gyors sebességét, de kevésbé pontos (alapértelmezett).|

### <a name="json-output"></a>JSON kimeneti
a következő példa a JSON-kimenetét hello csonkolódott.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

## <a name="emotion-detection-input-and-output-example"></a>Érzelemfelismerés bemeneti és kimeneti példa
### <a name="input-video"></a>A bemeneti videó
[A bemeneti videó](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>A feladat konfigurációja (beállítás)
A feladat létrehozásakor **Azure Media Arcfelismerési érzékelő**, meg kell adnia egy konfigurációs készletet. a következő konfigurációs készlet hello toocreate JSON hello érzelemfelismerés alapján határozza meg.

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a>Az attribútumok leírása
| Attribútum neve | Leírás |
| --- | --- |
| Mód |Lapok: Csak szembesülhetnek észlelése.<br/>PerFaceEmotion: Visszatérési érzelemfelismerési egymástól függetlenül az egyes arcfelismerési észlelése.<br/>AggregateEmotion: Minden lap keretében átlagos érzelemfelismerési visszatérési értékei. |
| AggregateEmotionWindowMs |Ha a kiválasztott AggregateEmotion módot használja. Adja meg videó használt tooproduce hello hosszát minden összesített eredmény. |
| AggregateEmotionIntervalMs |Ha a kiválasztott AggregateEmotion módot használja. Határozza meg, milyen gyakoriság tooproduce összesített eredmények. |

#### <a name="aggregate-defaults"></a>Összesített alapértelmezései
Alább javasoltak hello összesítő és időköz beállítások értékeit. AggregateEmotionWindowMs hosszabb, mint AggregateEmotionIntervalMs kell lennie.

|| Alapértelmezett (s) | Min(s) | Max(s) |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0.5 |2 |0.25|
| AggregateEmotionIntervalMs |0.5 |1 |0.25|

### <a name="json-output"></a>JSON kimeneti
Összesített érzelemfelismerési (csonkolt) a kimeneti JSON:

    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,

## <a name="limitations"></a>Korlátozások
* hello támogatott bemeneti videó formátumnak tartalmaznia kell MP4 MOV és WMV.
* hello észlelhető arcfelismerési mérete tartománya 24 x 24 too2048x2048 képpont. hello lapokat a tartományon kívül nem fogja észlelni.
* Minden videó visszaadott lapok maximális számát hello esetén 64.
* Bizonyos lapok tootechnical kihívást; miatt nem észlelhető például a nagyon nagy arcfelismerési szögek (head-jelentő), és nagy hangelnyelés. Elülső és közelében elülső lapok rendelkezik hello legjobb eredmények elérése érdekében.

## <a name="net-sample-code"></a>.NET mintakód

hello következő program bemutatja hogyan:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.
2. Hozzon létre egy feladatot a json-készlet a következő hello tartalmazó konfigurációs fájl alapján arcfelismerési észlelési feladatokkal. 
   
        {
            "version": "1.0"
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

    namespace FaceDetection
    {
        class Program
        {
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

                // Run hello FaceDetection job.
                var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\FaceDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
            }

            static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Face Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Face Detection Job");

                // Get a reference tooAzure Media Face Detector.
                string MediaProcessorName = "Azure Media Face Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Face Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

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

[Az Azure Médiaelemzés bemutatók](http://amslabs.azurewebsites.net/demos/Analytics.html)


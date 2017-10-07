---
title: "az Azure Media Analytics OCR aaaDigitize szöveg |} Microsoft Docs"
description: "Az Azure Media Analytics OCR (optikai karakter felismerés) lehetővé teszi tooconvert szöveges tartalom videofájlok szerkeszthető, kereshető digitális szöveggé.  Ez lehetővé teszi tooautomate hello kivonása jelentéssel bíró metaadatok hello videó Signal médiafájlt."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 0476c3ba3942b2c5182a34a429909adbf5c75ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-analytics-tooconvert-text-content-in-video-files-into-digital-text"></a>Azure Media Analytics tooconvert szöveges tartalom digitális szöveggé videofájlok használata
## <a name="overview"></a>Áttekintés
Ha tooextract szöveges tartalom kell a video-fájlokból és létrehozni egy szerkeszthető, kereshető digitális szöveget, az Azure Media Analytics OCR (optikai karakter használata) kell használnia. Az Azure Media processzor szöveges tartalom észleli a videofájlok lejátszását, és állít elő, a szöveg fájljait. OCR lehetővé teszi, hogy Ön tooautomate hello kibontásával jelentéssel bíró metaadatok hello videó Signal médiafájlt.

Ha együtt használják a keresőprogramok találatait, egyszerűen médiatartalmak index a szöveg, és hello felderíthetőség a tartalom javítása érdekében. Ez különösen fontos például videó-felvételt vagy diavetítési bemutató képernyőfelvétel a magas szöveges videó. hello Azure OCR Media processzor digitális szöveg van optimalizálva.

Hello **Azure Media OCR** media processzor jelenleg előzetes verzió.

Ez a témakör kapcsolatos részleteket nyújt **Azure Media OCR** és bemutatja, hogyan toouse a Media Services SDK for .NET. További információt és példákat lásd: [ebben a blogban](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

## <a name="ocr-input-files"></a>A bemeneti fájlok OCR
Videofájlok lejátszását. Jelenleg a következő formátumok hello támogatottak: MP4 MOV és WMV.

## <a name="task-configuration"></a>A feladat konfigurációja
A feladat konfigurációja (beállítás). A feladat létrehozásakor **Azure Media OCR**, meg kell adnia egy készlet használatával JSON vagy XML-konfiguráció. 

>[!NOTE]
>hello OCR motor mindössze egy lemezkép a régióban, minimális 40 képpont toomaximum 32000 képpont mindkét magasság/szélesség egy érvényes bemeneti adatként.
>

### <a name="attribute-descriptions"></a>Az attribútumok leírása
| Attribútum neve | Leírás |
| --- | --- |
|AdvancedOutput| Ha AdvancedOutput tootrue, hello JSON-kimenetét (a hozzáadása toophrases és régiók) minden egyszavas pozicionális adatait fogja tartalmazni. Ha nem szeretné, hogy toosee ezeket az adatokat, állítsa be a hello jelző toofalse. hello alapértelmezett értéke hamis. További információkért lásd: [ebben a blogban](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).|
| Nyelv |(választható) ismerteti, mely toolook szövege hello nyelvét. Hello a következők egyikét: automatikus felismerés (alapértelmezett), arab, ChineseSimplified, ChineseTraditional, dán Cseh, holland, angol, finn, francia, német, görög, magyar, olasz, japán, koreai, lengyel, lengyel, portugál, román, orosz, SerbianCyrillic, SerbianLatin, szlovák, spanyol, svéd, török. |
| TextOrientation |(választható) ismerteti, mely toolook szövege hello tájolását.  "Bal oldali" azt jelenti, hogy az összes felső hello felé hello bal vannak változó.  Alapértelmezett szöveg (például, amely itt található: egy könyv) hívható "Mentés" objektumorientált.  Hello a következők egyikét: AutoDetect (alapértelmezett), akár, jobbra, lefelé, balra. |
| TimeInterval |(választható) hello mintavételi ráta ismerteti.  Alapértelmezett érték 1/2 másodpercenként.<br/>JSON formátumban – óó: pp:. Biztonságos tár szolgáltatás (alapértelmezett 00:00:00.500)<br/>XML-formátum – a W3C XSD időtartama egyszerű (alapértelmezett PT0.5) |
| DetectRegions |(választható) Melyik toodetect szöveget ad meg a régiók hello videó keretbe DetectRegion objektumokból álló tömb.<br/>Egy DetectRegion objektum hello négy egész érték a következő történik:<br/>Left – a bal margó hello képpontban megadva<br/>Felső – hello felső-margónál képpontban megadva<br/>Szélesség – szélessége képpontban hello régió<br/>Magasság – magassága képpontban hello régió |

#### <a name="json-preset-example"></a>Előre definiált JSON-példa

    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }


#### <a name="xml-preset-example"></a>Előre definiált XML-példa
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

## <a name="ocr-output-files"></a>OCR kimeneti fájlok
hello hello OCR media processzor eredménye egy JSON-fájlt.

### <a name="elements-of-hello-output-json-file"></a>Hello kimeneti JSON-fájl elemeinek
hello videó OCR kimeneti idő szegmentált adatokat biztosít a videó megtalálható hello karaktere.  Attribútumok nyelven vagy toohone a tájolás például, hogy érdekli elemzése pontosan hello szavak is használhatja. 

hello kimenet tartalmazza a következő attribútumok hello:

| Elem | Leírás |
| --- | --- |
| időskálára |"ticks" hello videó másodpercenként |
| Eltolás |az időbélyegekhez időeltolódás. Videó API-k 1.0-s verziójában az mindig 0 lesz. |
| képkockasebességhez |Videó hello képkockasebessége |
| Szélessége |videó képpontban hello szélessége |
| Magassága |videó képpontban hello magassága |
| töredék |mely hello be metaadatok darabolásos van videó időalapú adattömbök tömbje |
| start |a "ticks" töredéket kezdete |
| Időtartam |a "ticks" töredéket hossza |
| interval |minden esemény belül töredéket adott hello időköz |
| események |régiók tartalmazó tömb |
| Régió |szavakat vagy kifejezéseket objektumot képviselő észlelt |
| nyelv |régión belül észlelt hello szöveg |
| Tájolás |régión belül észlelt hello szöveg irányának |
| sorok |a tömb sornyi szöveget észlelt régión belül |
| Szöveg |hello tényleges szöveg |

### <a name="json-output-example"></a>JSON kimeneti példa
hello alábbi példa a kimenetre hello általános videó információkat és tartalmaz több videó töredék. Minden videó töredékben szereplő minden egyes régió OCR MP hello nyelv és a szöveg tájolás által észlelt tartalmaz. hello régió minden word sor ebben a régióban hello sor szöveget, hello sor helye és a sor minden word információkkal (word tartalmat, és adott abban, hogy) is tartalmaz. hello az alábbiakban látható egy példa, és szeretnék bizonyos megjegyzéseket beágyazott helyezése

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // hello time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // hello detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including hello text and hello position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="net-sample-code"></a>.NET mintakód

hello következő program bemutatja hogyan:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.
2. Hozzon létre egy feladatot OCR konfigurációs/előre definiált fájllal.
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

    namespace OCR
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

                // Run hello OCR job.
                var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                            @"C:\supportFiles\OCR\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
            }

            static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My OCR Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My OCR Job");

                // Get a reference tooAzure Media OCR.
                string MediaProcessorName = "Azure Media OCR";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My OCR Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

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


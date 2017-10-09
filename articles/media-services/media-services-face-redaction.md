---
title: az Azure Media Analytics aaaRedact lapok |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan tooredact előtt álló az Azure médiaelemzés használatával."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5b6d8b8c-5f4d-4fef-b3d6-dc22c6b5a0f5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;
ms.openlocfilehash: 1f5688a8c6374151c526a9c702b904d8c3e46164
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a>Az Azure Media Analytics lapok kivonása
## <a name="overview"></a>Áttekintés
**Az Azure Media Redactor** van egy [Azure Médiaelemzés használatával](media-services-analytics-overview.md) media processzor (MP), amely méretezhető arcfelismerési kivonási hello felhőben nyújt. Arcfelismerési kivonási lehetővé teszi, hogy Ön toomodify a rendelés tooblur felületei kijelölt személyek a videó. Érdemes lehet toouse hello arcfelismerési kivonási szolgáltatás nyilvános biztonsági és hírek media forgatókönyvekben. Több lapokat tartalmazó felvételei, néhány percet is igénybe vehet óra tooredact manuálisan, de a szolgáltatás hello arcfelismerési a kivonási folyamat néhány egyszerű lépésben szükséges. További információkért lásd: [ez](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

Ez a témakör kapcsolatos részleteket nyújt **Azure Media Redactor** és bemutatja, hogyan toouse a Media Services SDK for .NET.

Hello **Azure Media Redactor** felügyeleti csomag jelenleg előzetes verzió. Érhető el az összes Azure-régiók, valamint Amerikai Egyesült államokbeli kormányzati és Kína adatközpontokban. Ez az előnézet jelenleg díjmentesen. 

## <a name="face-redaction-modes"></a>Arcfelismerési kivonási módok
Arcfelismerést kivonási works található videó lapok észlelésére, és nyomon követni a hello arcfelismerési objektumot is előre és hátra időben, hogy hello azonos egyéni is lehet homályos más szögek, valamint a. hello automatizált kivonási folyamat nagyon összetett és nem mindig készít 100 %-a kívánt kimeneti, emiatt Media Analytics tartalmazza a több módon toomodify hello végső kimenetet.

Továbbá tooa teljesen automatikus üzemmódban van; nincs lehetővé teszi, hogy hello kijelölés/inaktiválása-selection talált lapok keresztül azonosítók listáját, amelyek a két-fázis munkafolyamat. Toomake keret módosításának hello felügyeleti csomag egy tetszőleges ugyancsak a metaadatfájl JSON formátumban. Ez a munkafolyamat oszlik **elemzés** és **Redact** módot. Kombinálhatja a két mód hello egyetlen menetben, amely mindkét feladat fut egy feladat; Ebben a módban nevezik **kombinált**.

### <a name="combined-mode"></a>Kombinált mód
Ez a művelet létrehoz egy kivont mp4 automatikusan szükséges bemeneti manuális nélkül.

| Fázis | Fájlnév | Megjegyzések |
| --- | --- | --- |
| Bemeneti eszköz |foo.bar |Videó WMV, MOV vagy MP4 formátumban |
| Bemeneti config |Konfigurációs feladat az adott néven beállítás |{"version": "1.0', a"beállítások": {"mode":"kombinált"}} |
| Kimeneti eszköz |foo_redacted.mp4 |Az alkalmazott fellazítja videó |

#### <a name="input-example"></a>Bemeneti. példa:
[Ez a videó megtekintése](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Példa a kimenetre:
[Ez a videó megtekintése](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Mód elemzése
Hello **elemzése** hello két-fázis munkafolyamat menete videó bemenetből fogad adatokat, és arcfelismerési helyek JSON fájlt hoz létre, és jpg lemezképek az egyes észlelt arcfelismerési.

| Fázis | Fájlnév | Megjegyzések |
| --- | --- | --- |
| Bemeneti eszköz |foo.bar |Videó WMV, MPV vagy MP4 formátumban |
| Bemeneti config |Konfigurációs feladat az adott néven beállítás |{"version": "1.0', a"beállítások": {"mode":"elemezni"}} |
| Kimeneti eszköz |foo_annotations.JSON |A jegyzet adatok arcfelismerési helyek JSON formátumban. Ez módosítható használatával hello felhasználói toomodify hello fellazítja határolókeret jelölőnégyzetéből. Lásd az alábbi minta. |
| Kimeneti eszköz |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Az egyes levágott jpg arc, ahol hello számot jelzi hello címkeazonosító hello felületen észlelt |

#### <a name="output-example"></a>Példa a kimenetre:

    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated

### <a name="redact-mode"></a>Mód kivonása
második fázisában hello hello munkafolyamat egyetlen eszköz kombinálni kell felhasználandó nagyobb számú vesz igénybe.

Ez magában foglalja a azonosítók tooblur, hello eredeti videó és hello jegyzetek JSON listáját. Ebben a módban a hello jegyzetek tooapply fellazítja hello bemeneti videóhoz használja.

hello hello elemzési fázis kimenete nem tartalmaz hello eredeti videó. videó hello kell toobe hello Redact mód feladat hello bemeneti objektumba feltöltött és hello elsődleges fájl választotta.

| Fázis | Fájlnév | Megjegyzések |
| --- | --- | --- |
| Bemeneti eszköz |foo.bar |Videó WMV, MPV vagy MP4 formátumban. Ugyanaz, mint 1. lépés videó. |
| Bemeneti eszköz |foo_annotations.JSON |első lépése, opcionális módosításokkal jegyzetek metaadatfájl. |
| Bemeneti eszköz |foo_IDList.txt (nem kötelező) |Választható Sortöréssel elválasztott arcfelismerési azonosítók tooredact. Ha üresen hagyja a mezőt, ez minden életleníti. |
| Bemeneti config |Konfigurációs feladat az adott néven beállítás |{"version": "1.0', a"beállítások": {"mode":"Kihagyás"}} |
| Kimeneti eszköz |foo_redacted.mp4 |Videó fellazítja alkalmazott jegyzetek alapján |

#### <a name="example-output"></a>Példa a kimenetre
Ez a kiválasztott egy azonosítójú egy IDList hello kimenete.

[Ez a videó megtekintése](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Példa foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Életlenítés típusok

A hello **kombinált** vagy **Redact** módban keresztül hello JSON bemeneti konfigurációs választhat 5 különböző életlenítés módot: **alacsony**, **Med**, **Magas**, **Debug**, és **fekete**. Alapértelmezés szerint **Med** szolgál.

Hello mintái az alábbi típusok életlenítés találja.

### <a name="example-json"></a>JSON-NÁ. példa:

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a>Alacsony

![Alacsony](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a>Med

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a>Magas

![Magas](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a>Hibakeresés

![Hibakeresés](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a>Fekete

![Fekete](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-hello-output-json-file"></a>Hello kimeneti JSON-fájl elemeinek

hello kivonási felügyeleti csomag biztosít magas pontosság arcfelismerési hely felderítését és a nyomkövetési, amely észlelni tudja a too64 emberi lapok videó keret mentése. Elülső lapok hello legjobb eredmények elérése érdekében ügyféloldali lapokat és kis lapok közben adja meg (kevesebb mint vagy egyenlő too24x24 képpont) kihívást is.

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>.NET mintakód

hello következő program bemutatja hogyan:

1. Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.
2. Hozzon létre egy feladatot a json-készlet a következő hello tartalmazó konfigurációs fájl alapján arcfelismerési kivonási feladatokkal. 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
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

    namespace FaceRedaction
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

            // Run hello FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download hello job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload hello input media file toostorage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference tooAzure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from hello specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with hello encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify hello input asset.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Kapcsolódó hivatkozások
[Azure Media Services elemző áttekintése](media-services-analytics-overview.md)

[Az Azure Médiaelemzés bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


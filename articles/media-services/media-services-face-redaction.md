---
title: "Az Azure Media Analytics lapok kivonása |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan kivonása az Azure media analytics lapokat."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako;
ms.openlocfilehash: 2e936379968f74eb8bea420916acea2b8d96bb24
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a>Az Azure Media Analytics lapok kivonása
## <a name="overview"></a>Áttekintés
**Az Azure Media Redactor** van egy [Azure Médiaelemzés használatával](media-services-analytics-overview.md) media processzor (MP), amely a felhőben méretezhető arcfelismerési kivonási nyújt. Arcfelismerési kivonási lehetővé teszi, hogy a videó ahhoz, hogy a kijelölt személyeket felületei életlenítés módosíthatja. Érdemes lehet nyilvános biztonsági és hírek media helyzetekben használhatja a tapasztalt kivonási szolgáltatás. Több lapokat tartalmazó felvételei, néhány perc múlva a kivonás a manuálisan órát is igénybe vehet, de ezzel a szolgáltatással a tapasztalt kivonási folyamat néhány egyszerű lépésben szükséges. További információkért lásd: [ez](https://azure.microsoft.com/blog/azure-media-redactor/) blog.

Ez a cikk részletezi kapcsolatos **Azure Media Redactor** és a .NET-keretrendszerhez készült Media Services SDK-val való használatát ismerteti.

## <a name="face-redaction-modes"></a>Arcfelismerési kivonási módok
Arcfelismerést kivonási működik található videó lapok észlelésére és nyomon követni a tapasztalt objektum mindkét előre és hátra időben, ugyanabból is homályos, a más szögek is. Az automatizált kivonási folyamata bonyolult, és nem nem mindig által előállított 100 %-a kívánt kimeneti, ezért Media Analytics tartalmazza a több módon módosítani a végső kimenetet.

Mellett a teljesen automatikus mód nincs két-hozzáférési munkafolyamat, amely lehetővé teszi, hogy a kijelölés/inaktiválása-selection a tényleges lapok keresztül azonosítók listáját. Ellenőrizze a keret módosításának a felügyeleti csomag egy tetszőleges is, használja a metaadatfájl JSON formátumban. Ez a munkafolyamat oszlik **elemzés** és **Redact** módot. A két mód, amely mindkét feladat fut egy feladat; egyetlen menetben kombinálva Ebben a módban nevezik **kombinált**.

### <a name="combined-mode"></a>Kombinált mód
Ezzel létrehozza a kivont mp4 automatikusan szükséges bemeneti manuális nélkül.

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
A **elemzése** fázis a két-fázis munkafolyamat videó bemenetből fogad adatokat, és arcfelismerési helyek JSON fájlt hoz létre, és jpg lemezképek az egyes észlelt arcfelismerési.

| Fázis | Fájlnév | Megjegyzések |
| --- | --- | --- |
| Bemeneti eszköz |foo.bar |Videó WMV, MPV vagy MP4 formátumban |
| Bemeneti config |Konfigurációs feladat az adott néven beállítás |{"version": "1.0', a"beállítások": {"mode":"elemezni"}} |
| Kimeneti eszköz |foo_annotations.JSON |A jegyzet adatok arcfelismerési helyek JSON formátumban. Ez a felhasználó módosítása a mezőkbe határolókeret fellazítja szerkeszthető. Lásd az alábbi minta. |
| Kimeneti eszköz |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Az egyes levágott jpg arc, ahol a számot jelzi a felületen címkeazonosító észlelt |

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
A második fázis a munkafolyamat bemenetei, egyetlen eszköz kombinálni kell nagyobb számú vesz igénybe.

Ez magában foglalja a azonosítók lehetőséget, az eredeti video-, és a jegyzetek JSON listáját. Ebben a módban a jegyzetek a segítségével a bemeneti videóhoz a fellazítja.

Az elemzési fázis kimenete nem tartalmazza az eredeti videó. A videó kell a Redact mód tevékenység bemeneti objektumba feltölthetők és az elsődleges fájl választotta.

| Fázis | Fájlnév | Megjegyzések |
| --- | --- | --- |
| Bemeneti eszköz |foo.bar |Videó WMV, MPV vagy MP4 formátumban. Ugyanaz, mint 1. lépés videó. |
| Bemeneti eszköz |foo_annotations.JSON |első lépése, opcionális módosításokkal jegyzetek metaadatfájl. |
| Bemeneti eszköz |foo_IDList.txt (nem kötelező) |Választható új sor tagolt arcfelismerési kivonás azonosítók listája. Ha üresen hagyja a mezőt, ez minden életleníti. |
| Bemeneti config |Konfigurációs feladat az adott néven beállítás |{"version": "1.0', a"beállítások": {"mode":"Kihagyás"}} |
| Kimeneti eszköz |foo_redacted.mp4 |Videó fellazítja alkalmazott jegyzetek alapján |

#### <a name="example-output"></a>Példa a kimenetre
Ez a kiválasztott egy azonosítójú egy IDList kimenetét.

[Ez a videó megtekintése](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Példa foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Életlenítés típusok

A a **kombinált** vagy **Redact** mód közül választhat a bemeneti JSON-konfigurációs keresztül 5 különböző életlenítés módot: **alacsony**, **Med**, **Magas**, **mezőben**, és **fekete**. Alapértelmezés szerint **Med** szolgál.

A életlenítés típusú mintában találja.

### <a name="example-json"></a>JSON-NÁ. példa:

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a>Alacsony

![Alacsony](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a>Med

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a>Magas

![Magas](./media/media-services-face-redaction/blur3.png)

#### <a name="box"></a>Box

![Box](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a>Fekete

![Fekete](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-the-output-json-file"></a>A kimeneti JSON-fájl elemeinek

A kivonási MP magas pontosság arcfelismerési hely észleli és nyomon követése, mely legfeljebb 64 emberi lapok észlelheti a képkockák. Elülső lapok nyújtanak a legjobb eredmények elérése érdekében, közben ügyféloldali lapok és a kis (legfeljebb 24 x 24 képpont) lapok kihívást.

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>.NET mintakód

A következő program bemutatja hogyan:

1. Hozzon létre egy eszközt, és adathordozó-fájl feltöltése az objektumba.
2. Hozzon létre egy feladatot a tapasztalt kivonási tevékenységek, a következő json-készletet tartalmazó konfigurációs fájl alapján: 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
3. A kimeneti JSON-fájlok letöltésére. 

#### <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint. 

#### <a name="example"></a>Példa

```
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
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Run the FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference to Azure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

            // Use the following event handler to check job progress.  
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

            // Launch the job.
            job.Submit();

            // Check job execution and wait for job to finish.
            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

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
```

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Kapcsolódó hivatkozások
[Azure Media Services elemző áttekintése](media-services-analytics-overview.md)

[Az Azure Médiaelemzés bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


---
title: "Azure Media Encoder Standard tooauto aaaUse-létrehozni egy sávszélességű létra |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toouse Media Encoder Standard (MES) tooauto-létrehozni egy sávszélességű létra hello bemeneti feloldási és átviteli sebesség alapján. hello bemeneti megoldás és sávszélességű soha nem túllépése. Például ha hello bemeneti 720p, 3 MB/s, kimeneti lesz 720p legjobb maradnak, és alacsonyabb, mint 3 MB/s sebesség időpontban fog elindulni."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 63ed95da-1b82-44b0-b8ff-eebd535bc5c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 5437f54ac28c42ddd4f9d1986549d6da6261c5da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="use-azure-media-encoder-standard-tooauto-generate-a-bitrate-ladder"></a>Használja az Azure Media Encoder Standard tooauto-egy sávszélességű létra készítése

## <a name="overview"></a>Áttekintés

Ez a témakör bemutatja, hogyan toouse Media Encoder Standard (MES) tooauto-hello bemeneti feloldási és átviteli sebesség alapján sávszélességű létra (sávszélességű felbontású párok) létrehozása. hello automatikusan generált előre definiált nem haladhatja meg a bemeneti hello megoldás és átviteli sebesség. Például ha hello bemeneti 720p, 3 MB/s, kimeneti lesz 720p legjobb maradnak, és alacsonyabb, mint 3 MB/s sebesség időpontban fog elindulni.

### <a name="encoding-for-streaming-only"></a>Az adatfolyamként történő csak kódolás

Ha a szándéka az tooencode a videó csak a streaming, akkor Ön %d használja a "Adaptív Streameléshez" hello forrás beállított egy kódolási feladat létrehozásakor. Hello használatakor **adaptív Streameléshez** beállított, hello MES kódoló fog intelligens módon cap egy sávszélességű létra. Azonban nem fog tudni toocontrol hello kódolás költségek, mivel hello szolgáltatást határozza meg, hogy hány rétegek toouse, és milyen felbontásban. Példa látható miatt hello kódolás MES által előállított kimeneti rétegek **adaptív Streameléshez** beállított hello Ez a témakör végén. hello kimeneti eszköz MP4 fájljait fogja tartalmazni, ahol a hang és videó nem időosztásos.

### <a name="encoding-for-streaming-and-progressive-download"></a>Adatfolyam-továbbításhoz és progresszív letöltés kódolása

Ha a szándéka az tooencode előre a forrás videó az adatfolyamként történő tooproduce MP4-fájlok progresszív letöltés, valamint hogy %d használhatják a "Tartalom adaptív több sávszélességű MP4" hello, egy kódolási feladat létrehozásakor. Hello használatakor **tartalom adaptív több sávszélességű MP4** beállított, hello MES kódoló érvényes kódolási logikák a fenti, de most hello kimeneti adategységen fogja tartalmazni MP4-fájlokat adott hang hello és a videó időosztásos. A MP4-fájlok (például hello legmagasabb sávszélességű verzió) valamelyikét használhatja a progresszív letöltés fájlként.

## <a id="encoding_with_dotnet"></a>A Media Services .NET SDK kódolás

a következő példakód hello Media Services .NET SDK tooperform hello feladatok a következő használja:

- Hozzon létre egy kódolási feladat.
- Egy hivatkozási toohello Media Encoder Standard encoder beolvasása.
- A kódolási feladat toohello feladat hozzáadása, és adja meg a toouse hello **adaptív Streameléshez** előre. 
- Hozzon létre egy kimeneti eszköz, amely kódolású hello eszköz fogja tartalmazni.
- Adjon hozzá egy esemény kezelő toocheck hello feladat előrehaladását.
- Hello feladat küldése

#### <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Példa

    using System;
    using System.Configuration;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace AdaptiveStreamingMESPresest
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

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate hello output using hello "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass tooit hello name of hello 
            // processor toouse for hello specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify hello input asset toobe encoded.
            task.InputAssets.Add(asset);
            // Add an output asset toocontain hello results of hello job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means hello output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                break;
            default:
                break;
            }
        }
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
        }
    }

## <a id="output"></a>Kimeneti

Ez a szakasz bemutatja a három példák miatt hello kódolás MES által előállított kimeneti rétegek **adaptív Streameléshez** előre. 

### <a name="example-1"></a>1. példa
A magasság "1080" és "29.970" képkockasebességhez forrás 6 videó rétegek hoz létre:

|Réteg|Magassága|Szélessége|Bitrate(kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>2. példa
A magasság "720" és "23.970" képkockasebességhez forrás 5 videó rétegek hoz létre:

|Réteg|Magassága|Szélessége|Bitrate(kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>3. példa
A magasság "360" és "29.970" képkockasebességhez forrás 3 videó rétegek hoz létre:

|Réteg|Magassága|Szélessége|Bitrate(kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Media Services kódolási áttekintése](media-services-encode-asset.md)


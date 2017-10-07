---
title: "egy eszköz, a Media Encoder Standard használatával .NET aaaEncode |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toouse .NET tooencode Media Encoder Strandard keresztül."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 25e274c3b67168f4afc8b8ab04af2d654c9dd6e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Egy eszköz kódolása a Media Encoder Standard .NET használatával
Kódolási feladatok olyan hello leggyakoribb feldolgozási műveletek a Media Services. Kódolási feladatok tooconvert médiafájlok hoz létre egy kódolási tooanother. Amikor kódolására, hello Media Services beépített Media Encoder is használhatja. Egy Media Services partner; által biztosított kódoló is használható harmadik féltől származó kódolók hello Azure piactéren keresztül érhetők el. 

Ez a témakör bemutatja, hogyan toouse .NET tooencode az eszközök a Media Encoder Standard (MES). Media Encoder Standard segítségével konfigurálható: az egyik hello kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Tooalways a forrásfájl kódolása egy adaptív sávszélességű MP4 állítsa be, és alakítsa át hello beállítása toohello kívánt formátumot hello használata javasolt [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md). 

Ha az kimeneti adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét. További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md).

> [!NOTE]
> Olyan nevet, amely a kimeneti fájl első 32 karakterét hello MES előállított hello bemeneti fájl nevét. hello neve alapján hello előre definiált fájl megadottal. Például a "fájlnevet": "{Index} {bővítmény} {Basename} _". {Basename} helyébe hello hello bemeneti fájl nevének első 32 karakter.
> 
> 

### <a name="mes-formats"></a>MES formátumok
[Formátumok és kodekek](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a>MES-beállításkészletek
Media Encoder Standard segítségével konfigurálható: az egyik hello kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Bemeneti és kimeneti metaadatok
Amikor kódol MES használatával egy bemeneti eszköz (vagy eszközök), a kimeneti eszköz beolvasása hello: sikeres befejezését, amely feladat kódolása. hello kimeneti eszköz tartalmazza a videó, hang, miniatűröket, jegyzék, stb. alapján hello kódolási beállításkészlet használja.

hello kimeneti adategységen hello bemeneti eszköz metaadatait tartalmazó fájl is tartalmaz. hello hello metaadatok XML-fájl neve van hello a következő formátumban: < asset_id > _metadata.xml (például 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), ahol a < asset_id > hello bemeneti eszköz hello AssetId értéke. hello sémája a bemeneti XML metaadatok leírását [Itt](media-services-input-metadata-schema.md).

hello kimeneti adategységen hello kimeneti adategységen vonatkozó metaadatok fájlt is tartalmaz. hello hello metaadatok XML-fájl neve van hello a következő formátumban: < source_file_name > _manifest.xml (például BigBuckBunny_manifest.xml). a kimeneti metaadatok XML leírt hello sémája [Itt](media-services-output-metadata-schema.md).

Ha azt szeretné, tooexamine vagy hello két Metaadatfájlok, hozzon létre egy SAS-kereső, és töltse le a hello fájl tooyour helyi számítógép. Megtalálhatja például a hogyan toocreate egy SAS-kereső és a letöltési egy fájl használata hello Media Services .NET SDK-bővítmények.

## <a name="download-sample"></a>Minta letöltése
Get, és futtasson egy mintát, amely bemutatja, hogyan tooencode a MES rendelkező [Itt](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="net-sample-code"></a>.NET mintakód

a következő példakód hello Media Services .NET SDK tooperform hello feladatok a következő használja:

* Hozzon létre egy kódolási feladat.
* Egy hivatkozási toohello Media Encoder Standard encoder beolvasása.
* Adja meg a toouse hello [adaptív Streameléshez](media-services-autogen-bitrate-ladder-with-mes.md) előre. 
* Adjon hozzá egy kódolási feladat toohello feladatban. 
* Adjon meg bemeneti hello eszköz toobe kódolású.
* Hozzon létre egy kimeneti eszköz, amely kódolású hello eszköz fogja tartalmazni.
* Adjon hozzá egy esemény kezelő toocheck hello feladat előrehaladását.
* Hello feladat küldése

#### <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 

#### <a name="example"></a>Példa 

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderStandardSample
        {
            class Program
            {
                private static readonly string _AADTenantDomain =
                    ConfigurationManager.AppSettings["AADTenantDomain"];
                private static readonly string _RESTAPIEndpoint =
                    ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

                // Field for service context.
                private static CloudMediaContext _context = null;

                private static readonly string _supportFiles =
                    Path.GetFullPath(@"../..\Media");

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

                    // Create a task with hello encoding details, using a string preset.
                    // In this case "Adaptive Streaming" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
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

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Következő lépések
[Hogyan .NET Media Encoder Standard használatával toogenerate miniatűr](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services kódolási áttekintése](media-services-encode-asset.md)


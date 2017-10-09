---
title: "Media Encoder prémium munkafolyamat kódolás aaaAdvanced |} Microsoft Docs"
description: "Megtudhatja, hogyan tooencode Media Encoder prémium a munkafolyamathoz. Kódminták C# nyelven íródtak, és a Media Services SDK hello használata a .NET-hez."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 0f4c87ac-810a-4d42-8df8-923dff2016c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: juliako
ms.openlocfilehash: 5a1c3d019a5c8fbf9bda2da751a7eff4c4907d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Speciális Media Encoder prémium munkafolyamat kódolás
> [!NOTE]
> A jelen témakörben bemutatott Media Encoder prémium munkafolyamat media processzor Kínában nem érhető el.
>
>

Prémium szintű kódolás kérdéseket tesz fel a Microsoft.com mepd e-mail.

## <a name="overview"></a>Áttekintés
A Microsoft Azure Media Services bevezeti a hello **Media Encoder prémium munkafolyamat** media processzor. A processzor ajánlatok előzetes lehetőségeket biztosít a prémium szintű igény szerinti munkafolyamatok kódolást.

hello témakörök szerkezeti túl kapcsolatos részleteket**Media Encoder prémium munkafolyamat**:

* [Támogatott fájlformátumok által hello Media Encoder prémium munkafolyamat](media-services-premium-workflow-encoder-formats.md) – hello fájlformátumokat és a kodek által támogatott ismerteti **Media Encoder prémium munkafolyamat**.
* [Áttekintés és az igény szerinti media kódolók Azure összehasonlítása](media-services-encode-asset.md) hasonlítja össze hello kódolási képességeit **Media Encoder prémium munkafolyamat** és **Media Encoder Standard**.

Ez a témakör bemutatja, hogyan rendelkező tooencode **Media Encoder prémium munkafolyamat** .NET használatával.

Kódolási feladatok hello **Media Encoder prémium munkafolyamat** külön konfigurációs fájlt, egy munkafolyamat-fájlnak nevezett igényelnek. Ezek a fájlok .workflow kiterjesztést és hello használatával hozhatók létre [munkafolyamat-Tervező](media-services-workflow-designer.md) eszköz.

Is megkapható hello alapértelmezett munkafolyamat-fájlok [Itt](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). hello mappa ezeket a fájlokat hello leírását is tartalmazza.

hello munkafolyamat fájlok feltöltése toobe tooyour Media Services-fiók eszközként kell, és ez az eszköz a kódolási feladat toohello kell átadni.

## <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md). 

## <a name="encoding-example"></a>Kódolási – példa

hello következő példa bemutatja, hogyan rendelkező tooencode **Media Encoder prémium munkafolyamat**.

hello a következő lépéseket kell végrehajtani:

1. Hozzon létre egy eszközt, és a munkafolyamat-fájl feltöltése.
2. Hozzon létre egy eszközt, és a forrás-adathordozó fájl feltöltése.
3. Hello "Media Encoder prémium Workflow" media processzor beolvasása.
4. Hozzon létre egy feladatot, és egy feladatot.

    A legtöbb esetben hello konfigurációs hello feladat karakterlánca üres (például az alábbi példa hello). Néhány speciális esetben (igénylő tootooset futásidejű tulajdonságok dinamikusan) ebben az esetben meg kellene adnia egy XML karakterlánc toohello kódolási feladat. Ilyen forgatókönyvek például a következők: egy átmeneti területre, párhuzamos vagy egymást követő adathordozók varrással, feliratozás létrehozása.
5. Adja hozzá a két bemeneti eszközök toohello feladatot.

    1. 1 – hello munkafolyamat eszköz.
    2. 2. – hello video asset.

    >[!NOTE]
    >hello munkafolyamat eszköz toohello feladat hello media eszköz előtt hozzá kell adni.
   Ehhez a feladathoz hello konfigurációs karakterlánc üresnek kell lennie.
   
6. Hello kódolási feladat küldése

        using System;
        using System.Linq;
        using System.Configuration;
        using System.IO;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;

        namespace MediaEncoderPremiumWorkflowSample
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

                private static readonly string _workflowFilePath =
                    Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");

                private static readonly string _singleMP4InputFilePath =
                    Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");

                static void Main(string[] args)
                {
                    var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
                    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                    _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

                    var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                    var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                    IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset);

                }

                static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

                    var fileName = Path.GetFileName(singleFilePath);

                    var assetFile = asset.AssetFiles.Create(fileName);

                    Console.WriteLine("Created assetFile {0}", assetFile.Name);

                    Console.WriteLine("Upload {0}", assetFile.Name);

                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);

                    return asset;
                }

                static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                    // Get a media processor reference, and pass tooit hello name of the
                    // processor toouse for hello specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

                    // Create a task with hello encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                        processor,
                        "",
                        TaskOptions.None);

                    // Specify hello input asset toobe encoded.
                    task.InputAssets.Add(workflow);
                    task.InputAssets.Add(video); // we add one asset
                                                 // Add an output asset toocontain hello results of hello job.
                                                 // This output is specified as AssetCreationOptions.None, which
                                                 // means hello output asset is not encrypted.
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);

                    // Use hello following event handler toocheck job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);

                    // Launch hello job.
                    job.Submit();

                    // Check job execution and wait for job toofinish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();

                    // If job state is Error hello event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        throw new Exception("\nExiting method due toojob error.");
                    }

                    return job.OutputMediaAssets[0];
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

Prémium szintű kódolás kérdéseket tesz fel a Microsoft.com mepd e-mail.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

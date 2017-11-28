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
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a><span data-ttu-id="6da48-103">Egy eszköz kódolása a Media Encoder Standard .NET használatával</span><span class="sxs-lookup"><span data-stu-id="6da48-103">Encode an asset with Media Encoder Standard using .NET</span></span>
<span data-ttu-id="6da48-104">Kódolási feladatok olyan hello leggyakoribb feldolgozási műveletek a Media Services.</span><span class="sxs-lookup"><span data-stu-id="6da48-104">Encoding jobs are one of hello most common processing operations in Media Services.</span></span> <span data-ttu-id="6da48-105">Kódolási feladatok tooconvert médiafájlok hoz létre egy kódolási tooanother.</span><span class="sxs-lookup"><span data-stu-id="6da48-105">You create encoding jobs tooconvert media files from one encoding tooanother.</span></span> <span data-ttu-id="6da48-106">Amikor kódolására, hello Media Services beépített Media Encoder is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6da48-106">When you encode, you can use hello Media Services built-in Media Encoder.</span></span> <span data-ttu-id="6da48-107">Egy Media Services partner; által biztosított kódoló is használható harmadik féltől származó kódolók hello Azure piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6da48-107">You can also use an encoder provided by a Media Services partner; third party encoders are available through hello Azure Marketplace.</span></span> 

<span data-ttu-id="6da48-108">Ez a témakör bemutatja, hogyan toouse .NET tooencode az eszközök a Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="6da48-108">This topic shows how toouse .NET tooencode your assets with Media Encoder Standard (MES).</span></span> <span data-ttu-id="6da48-109">Media Encoder Standard segítségével konfigurálható: az egyik hello kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6da48-109">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

<span data-ttu-id="6da48-110">Tooalways a forrásfájl kódolása egy adaptív sávszélességű MP4 állítsa be, és alakítsa át hello beállítása toohello kívánt formátumot hello használata javasolt [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6da48-110">It is recommended tooalways encode your source files into an adaptive bitrate MP4 set and then convert hello set toohello desired format using hello [Dynamic Packaging](media-services-dynamic-packaging-overview.md).</span></span> 

<span data-ttu-id="6da48-111">Ha az kimeneti adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="6da48-111">If your output asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="6da48-112">További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6da48-112">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6da48-113">Olyan nevet, amely a kimeneti fájl első 32 karakterét hello MES előállított hello bemeneti fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="6da48-113">MES produces an output file with a name that contains hello first 32 characters of hello input file name.</span></span> <span data-ttu-id="6da48-114">hello neve alapján hello előre definiált fájl megadottal.</span><span class="sxs-lookup"><span data-stu-id="6da48-114">hello name is based on what is specified in hello preset file.</span></span> <span data-ttu-id="6da48-115">Például a "fájlnevet": "{Index} {bővítmény} {Basename} _".</span><span class="sxs-lookup"><span data-stu-id="6da48-115">For example, "FileName": "{Basename}_{Index}{Extension}".</span></span> <span data-ttu-id="6da48-116">{Basename} helyébe hello hello bemeneti fájl nevének első 32 karakter.</span><span class="sxs-lookup"><span data-stu-id="6da48-116">{Basename} is replaced by hello first 32 characters of hello input file name.</span></span>
> 
> 

### <a name="mes-formats"></a><span data-ttu-id="6da48-117">MES formátumok</span><span class="sxs-lookup"><span data-stu-id="6da48-117">MES Formats</span></span>
[<span data-ttu-id="6da48-118">Formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="6da48-118">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a><span data-ttu-id="6da48-119">MES-beállításkészletek</span><span class="sxs-lookup"><span data-stu-id="6da48-119">MES Presets</span></span>
<span data-ttu-id="6da48-120">Media Encoder Standard segítségével konfigurálható: az egyik hello kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="6da48-120">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="6da48-121">Bemeneti és kimeneti metaadatok</span><span class="sxs-lookup"><span data-stu-id="6da48-121">Input and output metadata</span></span>
<span data-ttu-id="6da48-122">Amikor kódol MES használatával egy bemeneti eszköz (vagy eszközök), a kimeneti eszköz beolvasása hello: sikeres befejezését, amely feladat kódolása.</span><span class="sxs-lookup"><span data-stu-id="6da48-122">When you encode an input asset (or assets) using MES, you get an output asset at hello successful completion of that encode task.</span></span> <span data-ttu-id="6da48-123">hello kimeneti eszköz tartalmazza a videó, hang, miniatűröket, jegyzék, stb. alapján hello kódolási beállításkészlet használja.</span><span class="sxs-lookup"><span data-stu-id="6da48-123">hello output asset contains video, audio, thumbnails, manifest, etc. based on hello encoding preset you use.</span></span>

<span data-ttu-id="6da48-124">hello kimeneti adategységen hello bemeneti eszköz metaadatait tartalmazó fájl is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6da48-124">hello output asset also contains a file with metadata about hello input asset.</span></span> <span data-ttu-id="6da48-125">hello hello metaadatok XML-fájl neve van hello a következő formátumban: < asset_id > _metadata.xml (például 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), ahol a < asset_id > hello bemeneti eszköz hello AssetId értéke.</span><span class="sxs-lookup"><span data-stu-id="6da48-125">hello name of hello metadata XML file has hello following format: <asset_id>_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where <asset_id> is hello AssetId value of hello input asset.</span></span> <span data-ttu-id="6da48-126">hello sémája a bemeneti XML metaadatok leírását [Itt](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="6da48-126">hello schema of this input metadata XML is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="6da48-127">hello kimeneti adategységen hello kimeneti adategységen vonatkozó metaadatok fájlt is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6da48-127">hello output asset also contains a file with metadata about hello output asset.</span></span> <span data-ttu-id="6da48-128">hello hello metaadatok XML-fájl neve van hello a következő formátumban: < source_file_name > _manifest.xml (például BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="6da48-128">hello name of hello metadata XML file has hello following format: <source_file_name>_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span> <span data-ttu-id="6da48-129">a kimeneti metaadatok XML leírt hello sémája [Itt](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="6da48-129">hello schema of this output metadata XML is described [here](media-services-output-metadata-schema.md).</span></span>

<span data-ttu-id="6da48-130">Ha azt szeretné, tooexamine vagy hello két Metaadatfájlok, hozzon létre egy SAS-kereső, és töltse le a hello fájl tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="6da48-130">If you want tooexamine either of hello two metadata files, you can create a SAS locator and download hello file tooyour local computer.</span></span> <span data-ttu-id="6da48-131">Megtalálhatja például a hogyan toocreate egy SAS-kereső és a letöltési egy fájl használata hello Media Services .NET SDK-bővítmények.</span><span class="sxs-lookup"><span data-stu-id="6da48-131">You can find an example on how toocreate a SAS locator and download a file Using hello Media Services .NET SDK Extensions.</span></span>

## <a name="download-sample"></a><span data-ttu-id="6da48-132">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="6da48-132">Download sample</span></span>
<span data-ttu-id="6da48-133">Get, és futtasson egy mintát, amely bemutatja, hogyan tooencode a MES rendelkező [Itt](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="6da48-133">You can get and run a sample that shows how tooencode with MES from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="6da48-134">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="6da48-134">.NET sample code</span></span>

<span data-ttu-id="6da48-135">a következő példakód hello Media Services .NET SDK tooperform hello feladatok a következő használja:</span><span class="sxs-lookup"><span data-stu-id="6da48-135">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

* <span data-ttu-id="6da48-136">Hozzon létre egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="6da48-136">Create an encoding job.</span></span>
* <span data-ttu-id="6da48-137">Egy hivatkozási toohello Media Encoder Standard encoder beolvasása.</span><span class="sxs-lookup"><span data-stu-id="6da48-137">Get a reference toohello Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="6da48-138">Adja meg a toouse hello [adaptív Streameléshez](media-services-autogen-bitrate-ladder-with-mes.md) előre.</span><span class="sxs-lookup"><span data-stu-id="6da48-138">Specify toouse hello [Adaptive Streaming](media-services-autogen-bitrate-ladder-with-mes.md) preset.</span></span> 
* <span data-ttu-id="6da48-139">Adjon hozzá egy kódolási feladat toohello feladatban.</span><span class="sxs-lookup"><span data-stu-id="6da48-139">Add a single encoding task toohello job.</span></span> 
* <span data-ttu-id="6da48-140">Adjon meg bemeneti hello eszköz toobe kódolású.</span><span class="sxs-lookup"><span data-stu-id="6da48-140">Specify hello input asset toobe encoded.</span></span>
* <span data-ttu-id="6da48-141">Hozzon létre egy kimeneti eszköz, amely kódolású hello eszköz fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="6da48-141">Create an output asset that will contain hello encoded asset.</span></span>
* <span data-ttu-id="6da48-142">Adjon hozzá egy esemény kezelő toocheck hello feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="6da48-142">Add an event handler toocheck hello job progress.</span></span>
* <span data-ttu-id="6da48-143">Hello feladat küldése</span><span class="sxs-lookup"><span data-stu-id="6da48-143">Submit hello job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="6da48-144">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6da48-144">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="6da48-145">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="6da48-145">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="6da48-146">Példa</span><span class="sxs-lookup"><span data-stu-id="6da48-146">Example</span></span> 

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="6da48-147">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="6da48-147">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6da48-148">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="6da48-148">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="6da48-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6da48-149">Next steps</span></span>
<span data-ttu-id="6da48-150">[Hogyan .NET Media Encoder Standard használatával toogenerate miniatűr](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services kódolási áttekintése](media-services-encode-asset.md)</span><span class="sxs-lookup"><span data-stu-id="6da48-150">[How toogenerate thumbnail using Media Encoder Standard with .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding Overview](media-services-encode-asset.md)</span></span>


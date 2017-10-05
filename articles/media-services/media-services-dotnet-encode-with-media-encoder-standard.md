---
title: "Egy eszköz kódolása a Media Encoder Standard .NET használatával |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan .NET használatával kódolása a Media Encoder Strandard keresztül."
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
ms.openlocfilehash: 929592368501c54277748bf46b2160c9058db3fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a><span data-ttu-id="ea6c1-103">Egy eszköz kódolása a Media Encoder Standard .NET használatával</span><span class="sxs-lookup"><span data-stu-id="ea6c1-103">Encode an asset with Media Encoder Standard using .NET</span></span>
<span data-ttu-id="ea6c1-104">A kódolási feladat a Media Services egyik leggyakrabban használt művelete.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-104">Encoding jobs are one of the most common processing operations in Media Services.</span></span> <span data-ttu-id="ea6c1-105">A kódolási feladat a médiafájlokat alakítja át egy meghatározott kódolásból egy másikra.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-105">You create encoding jobs to convert media files from one encoding to another.</span></span> <span data-ttu-id="ea6c1-106">Amikor a kódolására, a Media Services beépített Media Encoder is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-106">When you encode, you can use the Media Services built-in Media Encoder.</span></span> <span data-ttu-id="ea6c1-107">Egy Media Services partner; által biztosított kódoló is használható harmadik féltől származó kódolókkal az Azure piactéren keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-107">You can also use an encoder provided by a Media Services partner; third party encoders are available through the Azure Marketplace.</span></span> 

<span data-ttu-id="ea6c1-108">Ez a témakör bemutatja, hogyan lehet .NET segítségével a kódolása a Media Encoder Standard (MES).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-108">This topic shows how to use .NET to encode your assets with Media Encoder Standard (MES).</span></span> <span data-ttu-id="ea6c1-109">Media Encoder Standard segítségével konfigurálható: az egyik a kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-109">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

<span data-ttu-id="ea6c1-110">Javasoljuk, hogy mindig kódolása egy adaptív sávszélességű MP4 állítsa be a forrásfájlokat, és a set átalakítása a kívánt formátum használatával az [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-110">It is recommended to always encode your source files into an adaptive bitrate MP4 set and then convert the set to the desired format using the [Dynamic Packaging](media-services-dynamic-packaging-overview.md).</span></span> 

<span data-ttu-id="ea6c1-111">Ha az kimeneti adategységen tárolótitkosítást alkalmaz, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-111">If your output asset is storage encrypted, you must configure asset delivery policy.</span></span> <span data-ttu-id="ea6c1-112">További információ: [objektumtovábbítási szabályzat konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-112">For more information see [Configuring asset delivery policy](media-services-dotnet-configure-asset-delivery-policy.md).</span></span>

> [!NOTE]
> <span data-ttu-id="ea6c1-113">MES hoz létre a kimeneti fájl a bemeneti fájl nevét az első 32 karakternél néven.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-113">MES produces an output file with a name that contains the first 32 characters of the input file name.</span></span> <span data-ttu-id="ea6c1-114">A név alapján Mi az előre definiált fájlban megadott.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-114">The name is based on what is specified in the preset file.</span></span> <span data-ttu-id="ea6c1-115">Például a "fájlnevet": "{Index} {bővítmény} {Basename} _".</span><span class="sxs-lookup"><span data-stu-id="ea6c1-115">For example, "FileName": "{Basename}_{Index}{Extension}".</span></span> <span data-ttu-id="ea6c1-116">{Basename} helyébe az első 32 karakter megegyezik a bemeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-116">{Basename} is replaced by the first 32 characters of the input file name.</span></span>
> 
> 

### <a name="mes-formats"></a><span data-ttu-id="ea6c1-117">MES formátumok</span><span class="sxs-lookup"><span data-stu-id="ea6c1-117">MES Formats</span></span>
[<span data-ttu-id="ea6c1-118">Formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="ea6c1-118">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a><span data-ttu-id="ea6c1-119">MES-beállításkészletek</span><span class="sxs-lookup"><span data-stu-id="ea6c1-119">MES Presets</span></span>
<span data-ttu-id="ea6c1-120">Media Encoder Standard segítségével konfigurálható: az egyik a kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-120">Media Encoder Standard is configured using one of the encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="ea6c1-121">Bemeneti és kimeneti metaadatok</span><span class="sxs-lookup"><span data-stu-id="ea6c1-121">Input and output metadata</span></span>
<span data-ttu-id="ea6c1-122">Amikor kódol MES használatával egy bemeneti eszköz (vagy eszközök), a kimeneti eszköz, a sikeres beolvasása, amely megvalósításának feladat kódolása.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-122">When you encode an input asset (or assets) using MES, you get an output asset at the successful completion of that encode task.</span></span> <span data-ttu-id="ea6c1-123">A kimeneti adategységen videó, hang, miniatűröket, jegyzék, stb. alapján használja a kódolási beállításkészlet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-123">The output asset contains video, audio, thumbnails, manifest, etc. based on the encoding preset you use.</span></span>

<span data-ttu-id="ea6c1-124">A kimeneti adategységen is tartalmaz a bemeneti eszköz metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-124">The output asset also contains a file with metadata about the input asset.</span></span> <span data-ttu-id="ea6c1-125">A metaadatok XML-fájl nevének formátuma a következő: < asset_id > _metadata.xml (például 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), ahol a < asset_id > a bemeneti eszköz AssetId értékét.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-125">The name of the metadata XML file has the following format: <asset_id>_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where <asset_id> is the AssetId value of the input asset.</span></span> <span data-ttu-id="ea6c1-126">A bemeneti XML metaadatok sémája leírt [Itt](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-126">The schema of this input metadata XML is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="ea6c1-127">A kimeneti adategységen a kimeneti adategységen vonatkozó metaadatok fájlt is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-127">The output asset also contains a file with metadata about the output asset.</span></span> <span data-ttu-id="ea6c1-128">A metaadatok XML-fájl nevének formátuma a következő: < source_file_name > _manifest.xml (például BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-128">The name of the metadata XML file has the following format: <source_file_name>_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span> <span data-ttu-id="ea6c1-129">A kimeneti metaadatok XML leírt sémája [Itt](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-129">The schema of this output metadata XML is described [here](media-services-output-metadata-schema.md).</span></span>

<span data-ttu-id="ea6c1-130">Ha meg szeretné vizsgálni a két Metaadatfájlok valamelyikét, hozzon létre egy SAS-kereső, és töltse le a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-130">If you want to examine either of the two metadata files, you can create a SAS locator and download the file to your local computer.</span></span> <span data-ttu-id="ea6c1-131">Egy példa található SAS-kereső létrehozásával, és töltse le a fájlt, a Media Services .NET SDK-bővítmények használatával.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-131">You can find an example on how to create a SAS locator and download a file Using the Media Services .NET SDK Extensions.</span></span>

## <a name="download-sample"></a><span data-ttu-id="ea6c1-132">Minta letöltése</span><span class="sxs-lookup"><span data-stu-id="ea6c1-132">Download sample</span></span>
<span data-ttu-id="ea6c1-133">Lekérni, és futtasson egy mintát, amely bemutatja, hogyan rendelkező a MES kódolni [Itt](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="ea6c1-133">You can get and run a sample that shows how to encode with MES from [here](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="ea6c1-134">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="ea6c1-134">.NET sample code</span></span>

<span data-ttu-id="ea6c1-135">Az alábbi példakód Media Services .NET SDK-t használja a következő feladatok végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="ea6c1-135">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

* <span data-ttu-id="ea6c1-136">Hozzon létre egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-136">Create an encoding job.</span></span>
* <span data-ttu-id="ea6c1-137">A Media Encoder Standard encoder mutató hivatkozás beszerzése.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-137">Get a reference to the Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="ea6c1-138">Adja meg, hogy használja a [adaptív Streameléshez](media-services-autogen-bitrate-ladder-with-mes.md) előre.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-138">Specify to use the [Adaptive Streaming](media-services-autogen-bitrate-ladder-with-mes.md) preset.</span></span> 
* <span data-ttu-id="ea6c1-139">Egy kódolási feladat hozzáadása a projekthez.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-139">Add a single encoding task to the job.</span></span> 
* <span data-ttu-id="ea6c1-140">Adja meg a bemeneti eszköz kódolni kell.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-140">Specify the input asset to be encoded.</span></span>
* <span data-ttu-id="ea6c1-141">Hozzon létre egy kimeneti eszközt, amely tartalmazza majd a kódolt objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-141">Create an output asset that will contain the encoded asset.</span></span>
* <span data-ttu-id="ea6c1-142">Adjon hozzá egy eseménykezelő, ellenőrizze a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-142">Add an event handler to check the job progress.</span></span>
* <span data-ttu-id="ea6c1-143">A feladat elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-143">Submit the job.</span></span>

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="ea6c1-144">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ea6c1-144">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="ea6c1-145">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="ea6c1-145">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="ea6c1-146">Példa</span><span class="sxs-lookup"><span data-stu-id="ea6c1-146">Example</span></span> 

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

                    // Encode and generate the output using the "Adaptive Streaming" preset.
                    EncodeToAdaptiveBitrateMP4Set(asset);

                    Console.ReadLine();
                }

                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

                    // Create a task with the encoding details, using a string preset.
                    // In this case "Adaptive Streaming" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "Adaptive Streaming",
                        TaskOptions.None);

                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="ea6c1-147">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="ea6c1-147">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ea6c1-148">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="ea6c1-148">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="ea6c1-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea6c1-149">Next steps</span></span>
<span data-ttu-id="ea6c1-150">[.NET Media Encoder Standard használatával miniatűr létrehozásával](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services kódolási áttekintése](media-services-encode-asset.md)</span><span class="sxs-lookup"><span data-stu-id="ea6c1-150">[How to generate thumbnail using Media Encoder Standard with .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Encoding Overview](media-services-encode-asset.md)</span></span>


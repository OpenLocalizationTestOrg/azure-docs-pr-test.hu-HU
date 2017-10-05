---
title: "Media Encoder Standard készletek testreszabása |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan kódolásra speciális Media Encoder Standard feladat készletek testreszabásával. A témakör bemutatja, hogyan kódolási feladat és feladat létrehozása a Media Services .NET SDK használatával. Azt is bemutatja, hogyan fogja tartalmazni az egyéni készletek a kódolási feladat."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec95392f-d34a-4c22-a6df-5274eaac445b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: juliako
ms.openlocfilehash: b4d25f07349043da8cb745930fde3371c98f9960
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="customizing-media-encoder-standard-presets"></a><span data-ttu-id="3fda4-105">Testreszabás Media Encoder Standard készletek</span><span class="sxs-lookup"><span data-stu-id="3fda4-105">Customizing Media Encoder Standard presets</span></span>

## <a name="overview"></a><span data-ttu-id="3fda4-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3fda4-106">Overview</span></span>

<span data-ttu-id="3fda4-107">Ez a témakör bemutatja, hogyan végrehajtásához advanced kódolása a Media Encoder Standard (MES) egy egyéni készlettel.</span><span class="sxs-lookup"><span data-stu-id="3fda4-107">This topic shows how to perform advanced encoding with Media Encoder Standard (MES) using a custom preset.</span></span> <span data-ttu-id="3fda4-108">A témakör .NET használatával hozzon létre egy kódolási feladat és egy feladatot, amely végrehajtja ezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="3fda4-108">The topic uses .NET to create an encoding task and a job that executes this task.</span></span>  

<span data-ttu-id="3fda4-109">Ebben a témakörben egy előre definiált egyéni megtételével megjelenik a [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) beállított és a rétegek számának csökkentését.</span><span class="sxs-lookup"><span data-stu-id="3fda4-109">In this topic you will see how to customize a preset by taking the [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) preset and reducing the number of layers.</span></span> <span data-ttu-id="3fda4-110">A [Media Encoder Standard testreszabása készletek](media-services-advanced-encoding-with-mes.md) a témakörben bemutatjuk egyéni készletek speciális kódolási feladatok végrehajtásához használható.</span><span class="sxs-lookup"><span data-stu-id="3fda4-110">The [Customizing Media Encoder Standard presets](media-services-advanced-encoding-with-mes.md) topic demonstrates custom presets that can be used to perform advanced encoding tasks.</span></span>

## <span data-ttu-id="3fda4-111"><a id="customizing_presets"></a>Egy MES készletet testreszabása</span><span class="sxs-lookup"><span data-stu-id="3fda4-111"><a id="customizing_presets"></a> Customizing a MES preset</span></span>

### <a name="original-preset"></a><span data-ttu-id="3fda4-112">Eredeti készlet</span><span class="sxs-lookup"><span data-stu-id="3fda4-112">Original preset</span></span>

<span data-ttu-id="3fda4-113">A JSON-ban meghatározott mentése a [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) néhány .JSON kiterjesztésű kiterjesztésű fájl témakörében.</span><span class="sxs-lookup"><span data-stu-id="3fda4-113">Save the JSON defined in the [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) topic in some file with .json extension.</span></span> <span data-ttu-id="3fda4-114">Például **CustomPreset_JSON.json**.</span><span class="sxs-lookup"><span data-stu-id="3fda4-114">For example, **CustomPreset_JSON.json**.</span></span>

### <a name="customized-preset"></a><span data-ttu-id="3fda4-115">Testreszabott beállítások</span><span class="sxs-lookup"><span data-stu-id="3fda4-115">Customized preset</span></span>

<span data-ttu-id="3fda4-116">Nyissa meg a **CustomPreset_JSON.json** fájlt, és távolítsa el az első három rétegek **H264Layers** , a fájl néz ki.</span><span class="sxs-lookup"><span data-stu-id="3fda4-116">Open the **CustomPreset_JSON.json** file and remove first three layers from **H264Layers** so your file looks like this.</span></span>

    
    {  
      "Version": 1.0,  
      "Codecs": [  
        {  
          "KeyFrameInterval": "00:00:02",  
          "H264Layers": [  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 1000,  
              "MaxBitrate": 1000,  
              "BufferWindow": "00:00:05",  
              "Width": 640,  
              "Height": 360,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            },  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 650,  
              "MaxBitrate": 650,  
              "BufferWindow": "00:00:05",  
              "Width": 640,  
              "Height": 360,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            },  
            {  
              "Profile": "Auto",  
              "Level": "auto",  
              "Bitrate": 400,  
              "MaxBitrate": 400,  
              "BufferWindow": "00:00:05",  
              "Width": 320,  
              "Height": 180,  
              "BFrames": 3,  
              "ReferenceFrames": 3,  
              "AdaptiveBFrame": true,  
              "Type": "H264Layer",  
              "FrameRate": "0/1"  
            }  
          ],  
          "Type": "H264Video"  
        },  
        {  
          "Profile": "AACLC",  
          "Channels": 2,  
          "SamplingRate": 48000,  
          "Bitrate": 128,  
          "Type": "AACAudio"  
        }  
      ],  
      "Outputs": [  
        {  
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
          "Format": {  
            "Type": "MP4Format"  
          }  
        }  
      ]  
    }  
    

## <span data-ttu-id="3fda4-117"><a id="encoding_with_dotnet"></a>A Media Services .NET SDK kódolás</span><span class="sxs-lookup"><span data-stu-id="3fda4-117"><a id="encoding_with_dotnet"></a>Encoding with Media Services .NET SDK</span></span>

<span data-ttu-id="3fda4-118">Az alábbi példakód Media Services .NET SDK-t használja a következő feladatok végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="3fda4-118">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

- <span data-ttu-id="3fda4-119">Hozzon létre egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="3fda4-119">Create an encoding job.</span></span>
- <span data-ttu-id="3fda4-120">A Media Encoder Standard encoder mutató hivatkozás beszerzése.</span><span class="sxs-lookup"><span data-stu-id="3fda4-120">Get a reference to the Media Encoder Standard encoder.</span></span>
- <span data-ttu-id="3fda4-121">Az egyéni JSON-készlet, amely az előző szakaszban létrehozott betöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="3fda4-121">Load the custom JSON preset that you created in the previous section.</span></span> 
  
        // Load the JSON from the local file.
        string configuration = File.ReadAllText(fileName);  

- <span data-ttu-id="3fda4-122">Egy kódolási feladat hozzáadása a projekthez.</span><span class="sxs-lookup"><span data-stu-id="3fda4-122">Add an encoding task to the job.</span></span> 
- <span data-ttu-id="3fda4-123">Adja meg a bemeneti eszköz kódolni kell.</span><span class="sxs-lookup"><span data-stu-id="3fda4-123">Specify the input asset to be encoded.</span></span>
- <span data-ttu-id="3fda4-124">Hozzon létre egy kimeneti eszközt, amely tartalmazza majd a kódolt objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="3fda4-124">Create an output asset that will contain the encoded asset.</span></span>
- <span data-ttu-id="3fda4-125">Adjon hozzá egy eseménykezelő, ellenőrizze a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="3fda4-125">Add an event handler to check the job progress.</span></span>
- <span data-ttu-id="3fda4-126">A feladat elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="3fda4-126">Submit the job.</span></span>
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="3fda4-127">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3fda4-127">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="3fda4-128">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="3fda4-128">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="3fda4-129">Példa</span><span class="sxs-lookup"><span data-stu-id="3fda4-129">Example</span></span>   

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;

    namespace CustomizeMESPresests
    {
        class Program
        {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
        ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
        ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
            Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate the output using custom presets.
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

            // Load the XML (or JSON) from the local file.
            string configuration = File.ReadAllText("CustomPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
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

## <a name="media-services-learning-paths"></a><span data-ttu-id="3fda4-130">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3fda4-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3fda4-131">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3fda4-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="3fda4-132">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3fda4-132">See Also</span></span>
[<span data-ttu-id="3fda4-133">Media Services kódolási áttekintése</span><span class="sxs-lookup"><span data-stu-id="3fda4-133">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)


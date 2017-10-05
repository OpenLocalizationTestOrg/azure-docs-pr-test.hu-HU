---
title: "Miniatűrök létrehozása a .NET-es Media Encoder Standard használatával"
description: "Ez a témakör bemutatja, hogyan .NET használatával egy eszköz kódolni és indexképének létrehozására használnak Media Encoder Standard használatával egyszerre."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: b8dab73a-1d91-4b6d-9741-a92ad39fc3f7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: juliako
ms.openlocfilehash: 89d15cbdf71a140e78f34e07ff208776b7d4cab3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a><span data-ttu-id="65571-103">Miniatűrök létrehozása a .NET-es Media Encoder Standard használatával</span><span class="sxs-lookup"><span data-stu-id="65571-103">How to generate thumbnails using Media Encoder Standard with .NET</span></span>

<span data-ttu-id="65571-104">Használhatja Media Encoder Standard használatával hozzon létre egy vagy több miniatűrök a bemeneti videóhoz a [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), vagy [BMP](https://en.wikipedia.org/wiki/BMP_file_format) fájlformátumok lemezképet.</span><span class="sxs-lookup"><span data-stu-id="65571-104">You can use Media Encoder Standard to generate one or more thumbnails from your input video in [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), or [BMP](https://en.wikipedia.org/wiki/BMP_file_format) image file formats.</span></span> <span data-ttu-id="65571-105">Elküldheti a feladatokat, amelyek csak a képeket, vagy kombinálhatja a miniatűr generációs kódolással.</span><span class="sxs-lookup"><span data-stu-id="65571-105">You can submit Tasks that produce only images, or you can combine thumbnail generation with encoding.</span></span> <span data-ttu-id="65571-106">Ez a témakör néhány minta XML és JSON miniatűr készletek ilyen forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="65571-106">This topic provides a few sample XML and JSON thumbnail presets for such scenarios.</span></span> <span data-ttu-id="65571-107">A témakör végén van egy [példakód](#code_sample) , amely bemutatja, hogyan kódolási a feladatnak a Media Services .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="65571-107">At the end of the topic, there is a [sample code](#code_sample) that shows how to use the Media Services .NET SDK to accomplish the encoding task.</span></span>

<span data-ttu-id="65571-108">A minta készletek használt elemein további részletekért tekintse át [Media Encoder Standard séma](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="65571-108">For more details on the elements that are used in sample presets, you should review [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>

<span data-ttu-id="65571-109">Mindenképpen tekintse át a [szempontok](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) szakasz.</span><span class="sxs-lookup"><span data-stu-id="65571-109">Make sure to review the [Considerations](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) section.</span></span>

## <a name="example--single-png-file"></a><span data-ttu-id="65571-110">Példa – egyetlen PNG-fájl</span><span class="sxs-lookup"><span data-stu-id="65571-110">Example – single PNG file</span></span>

<span data-ttu-id="65571-111">A következő JSON és az XML-készlet feladatvégrehajtás egyetlen kimeneti PNG fájlok kívül az első néhány másodpercet a bemeneti videóhoz, ahol a kódolás lehetővé teszi a legjobb kísérlet egy "érdekes" keret keresése használható.</span><span class="sxs-lookup"><span data-stu-id="65571-111">The following JSON and XML preset can be used to produce a single output PNG file out of the first few seconds of the input video, where the encoder makes a best-effort attempt at finding an “interesting” frame.</span></span> <span data-ttu-id="65571-112">Vegye figyelembe, hogy a kimeneti kép méretei állították be, hogy a 100 %, ami azt jelenti, ezek a dimenziók a bemeneti videó fog egyezni.</span><span class="sxs-lookup"><span data-stu-id="65571-112">Note that the output image dimensions have been set to 100%, meaning these will match the dimensions of the input video.</span></span> <span data-ttu-id="65571-113">Azt is fontos megjegyezni, hogyan "Kimenetek" a "Format" beállítást kell felel meg a "Kodekek" szakasz "PngLayers" használatát.</span><span class="sxs-lookup"><span data-stu-id="65571-113">Note also how the “Format” setting in “Outputs” is required to match the use of “PngLayers” in the “Codecs” section.</span></span> 

### <a name="json-preset"></a><span data-ttu-id="65571-114">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-114">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "PngImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a><span data-ttu-id="65571-115">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-115">XML preset</span></span>

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <PngImage Start="{Best}">
          <PngLayers>
            <PngLayer>
              <Width>100%</Width>
              <Height>100%</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example--a-series-of-jpeg-images"></a><span data-ttu-id="65571-116">JPEG-képek sorozata – – példa</span><span class="sxs-lookup"><span data-stu-id="65571-116">Example – a series of JPEG images</span></span>

<span data-ttu-id="65571-117">A következő JSON és az XML-készlet segítségével létrehoznak egy 10 kép időbélyegeket 5, %, 15 %,..., 95 %-a bemeneti ütemterv, ahol a képméret ilyen van megadva a negyedév, amely a bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="65571-117">The following JSON and XML preset can be used to produce a set of 10 images at timestamps of 5%, 15%, …, 95% of the input timeline, where the image size is specified to be one quarter that of the input video.</span></span>

### <a name="json-preset"></a><span data-ttu-id="65571-118">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-118">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "5%",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }

### <a name="xml-preset"></a><span data-ttu-id="65571-119">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-119">XML preset</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="5%" Step="10%" Range="96%">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="example--one-image-at-a-specific-timestamp"></a><span data-ttu-id="65571-120">Példa – egy lemezképet egy adott időbélyeg</span><span class="sxs-lookup"><span data-stu-id="65571-120">Example – one image at a specific timestamp</span></span>

<span data-ttu-id="65571-121">A következő JSON és az XML-készletet, a 30 második be van jelölve a bemeneti videó egyetlen JPEG-képek létrehozására használható.</span><span class="sxs-lookup"><span data-stu-id="65571-121">The following JSON and XML preset can be used to produce a single JPEG image at the 30 second mark of the input video.</span></span> <span data-ttu-id="65571-122">Az előre definiált vár a bemeneti idő több mint 30 másodperc lehet (ellenkező esetben a feladat sikertelen lesz).</span><span class="sxs-lookup"><span data-stu-id="65571-122">This preset expects the input to be more than 30 seconds in duration (else the job will fail).</span></span>

### <a name="json-preset"></a><span data-ttu-id="65571-123">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-123">JSON preset</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "00:00:30",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
    
### <a name="xml-preset"></a><span data-ttu-id="65571-124">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-124">XML preset</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="00:00:30" Step="00:00:02" Range="00:00:01">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <span data-ttu-id="65571-125"><a id="code_sample"></a>Példa – videó kódolása és miniatűr készítése</span><span class="sxs-lookup"><span data-stu-id="65571-125"><a id="code_sample"></a>Example – encode video and generate thumbnail</span></span>

<span data-ttu-id="65571-126">Az alábbi példakód Media Services .NET SDK-t használja a következő feladatok végezhetők el:</span><span class="sxs-lookup"><span data-stu-id="65571-126">The following code example uses Media Services .NET SDK to perform the following tasks:</span></span>

* <span data-ttu-id="65571-127">Hozzon létre egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="65571-127">Create an encoding job.</span></span>
* <span data-ttu-id="65571-128">A Media Encoder Standard encoder mutató hivatkozás beszerzése.</span><span class="sxs-lookup"><span data-stu-id="65571-128">Get a reference to the Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="65571-129">A készlet betöltéséhez [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) vagy [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) , amelyek tartalmazzák a kódolást és a miniatűrök létrehozásához szükséges információkat az adott néven beállítás.</span><span class="sxs-lookup"><span data-stu-id="65571-129">Load the preset [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) that contain the encoding preset as well as information needed to generate thumbnails.</span></span> <span data-ttu-id="65571-130">Ez mentheti [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) vagy [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) a fájl- és használja a következő kódot betölteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="65571-130">You can save this  [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) in a file and use the following code to load the file.</span></span>
  
        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
* <span data-ttu-id="65571-131">Egy kódolási feladat hozzáadása a projekthez.</span><span class="sxs-lookup"><span data-stu-id="65571-131">Add a single encoding task to the job.</span></span> 
* <span data-ttu-id="65571-132">Adja meg a bemeneti eszköz kódolni kell.</span><span class="sxs-lookup"><span data-stu-id="65571-132">Specify the input asset to be encoded.</span></span>
* <span data-ttu-id="65571-133">Hozzon létre egy kimeneti eszközt, amely tartalmazza majd a kódolt objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="65571-133">Create an output asset that will contain the encoded asset.</span></span>
* <span data-ttu-id="65571-134">Adjon hozzá egy eseménykezelő, ellenőrizze a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="65571-134">Add an event handler to check the job progress.</span></span>
* <span data-ttu-id="65571-135">A feladat elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="65571-135">Submit the job.</span></span>

<span data-ttu-id="65571-136">Tekintse meg a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md) témakör utasításait a fejlesztési környezet beállítását ismerteti.</span><span class="sxs-lookup"><span data-stu-id="65571-136">See the [Media Services development with .NET](media-services-dotnet-how-to-use.md) topic for directions on how to set up your dev environment.</span></span>

        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;

        namespace EncodeAndGenerateThumbnails
        {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

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

            // Encode and generate the thumbnails.
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
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

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

## <span data-ttu-id="65571-137"><a id="json"></a>A miniatűr JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-137"><a id="json"></a>Thumbnail JSON preset</span></span>
<span data-ttu-id="65571-138">Séma kapcsolatos információkért lásd: [ez](https://msdn.microsoft.com/library/mt269962.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="65571-138">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>

    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
    
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Resolution}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <span data-ttu-id="65571-139"><a id="xml"></a>A miniatűr XML-készlet</span><span class="sxs-lookup"><span data-stu-id="65571-139"><a id="xml"></a>Thumbnail XML preset</span></span>
<span data-ttu-id="65571-140">Séma kapcsolatos információkért lásd: [ez](https://msdn.microsoft.com/library/mt269962.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="65571-140">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>100%</Width>
              <Height>100%/Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Resolution}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>

## <a name="considerations"></a><span data-ttu-id="65571-141">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="65571-141">Considerations</span></span>
<span data-ttu-id="65571-142">A következők érvényesek:</span><span class="sxs-lookup"><span data-stu-id="65571-142">The following considerations apply:</span></span>

* <span data-ttu-id="65571-143">Start/lépés/címtartomány explicit időbélyegeket használatát feltételezi, hogy a bemeneti forrás legalább 1 percnek hosszú.</span><span class="sxs-lookup"><span data-stu-id="65571-143">The use of explicit timestamps for Start/Step/Range assumes that the input source is at least 1 minute long.</span></span>
* <span data-ttu-id="65571-144">JPG vagy Png/BmpImage rendelkezniük Start, lépés és karakterlánc-attribútumok tartomány – ezek úgy:</span><span class="sxs-lookup"><span data-stu-id="65571-144">Jpg/Png/BmpImage elements have Start, Step and Range string attributes – these can be interpreted as:</span></span>
  
  * <span data-ttu-id="65571-145">Ha nem negatív egész számokat, például azok veremkeret száma.</span><span class="sxs-lookup"><span data-stu-id="65571-145">Frame Number if they are non-negative integers, eg.</span></span> <span data-ttu-id="65571-146">"Start": "120"</span><span class="sxs-lookup"><span data-stu-id="65571-146">"Start": "120",</span></span>
  * <span data-ttu-id="65571-147">Ha kifejezett % utótaggal, pl. forrás időtartama viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="65571-147">Relative to source duration if expressed as %-suffixed, eg.</span></span> <span data-ttu-id="65571-148">"Start": "15 %", vagy</span><span class="sxs-lookup"><span data-stu-id="65571-148">"Start": "15%", OR</span></span>
  * <span data-ttu-id="65571-149">Ha óó: pp: kifejezett időbélyeg...</span><span class="sxs-lookup"><span data-stu-id="65571-149">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="65571-150">formátumban.</span><span class="sxs-lookup"><span data-stu-id="65571-150">format.</span></span> <span data-ttu-id="65571-151">EG.</span><span class="sxs-lookup"><span data-stu-id="65571-151">Eg.</span></span> <span data-ttu-id="65571-152">"Start": "00: 01:00"</span><span class="sxs-lookup"><span data-stu-id="65571-152">"Start" : "00:01:00"</span></span>
    
    <span data-ttu-id="65571-153">Ön szabadon kombinálhatók jelölések, ha Ön adja.</span><span class="sxs-lookup"><span data-stu-id="65571-153">You can mix and match notations as you please.</span></span>
    
    <span data-ttu-id="65571-154">Emellett Start is támogatja a speciális makró: {ajánlott}, amely kísérli meg meghatározni a tartalom "érdekes" első keretében: (lépés, és a tartomány figyelmen kívül lesznek hagyva Ha kezdő {legjobb} értékre van állítva)</span><span class="sxs-lookup"><span data-stu-id="65571-154">Additionally, Start also supports a special Macro:{Best}, which attempts to determine the first “interesting” frame of the content NOTE: (Step and Range are ignored when Start is set to {Best})</span></span>
  * <span data-ttu-id="65571-155">Alapértelmezett: Start: {legjobb}</span><span class="sxs-lookup"><span data-stu-id="65571-155">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="65571-156">Kimeneti formátumot kell explicit módon meg kell adni az egyes képformátum: Jpg vagy Png/BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="65571-156">Output format needs to be explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="65571-157">Ha létezik, MES és így tovább a JpgFormat JpgVideo felel meg.</span><span class="sxs-lookup"><span data-stu-id="65571-157">When present, MES will match JpgVideo to JpgFormat and so on.</span></span> <span data-ttu-id="65571-158">OutputFormat bevezet egy új lemezkép-kodek adott makró: {Index}, mely rendelkeznie kell megjeleníteni az (egyszer és csak egyszer) kimeneti képformátum.</span><span class="sxs-lookup"><span data-stu-id="65571-158">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs to be present (once and only once) for image output formats.</span></span>

## <a name="next-steps"></a><span data-ttu-id="65571-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65571-159">Next steps</span></span>

<span data-ttu-id="65571-160">Ellenőrizheti a [folyamatban lévő feladat](media-services-check-job-progress.md) közben a kódolási feladat folyamatban.</span><span class="sxs-lookup"><span data-stu-id="65571-160">You can check the [job progress](media-services-check-job-progress.md) while the encoding job is pending.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="65571-161">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="65571-161">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="65571-162">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="65571-162">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="65571-163">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="65571-163">See Also</span></span>
[<span data-ttu-id="65571-164">Media Services kódolási áttekintése</span><span class="sxs-lookup"><span data-stu-id="65571-164">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)


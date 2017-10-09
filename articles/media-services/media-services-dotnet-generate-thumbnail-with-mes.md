---
title: "a .NET Media Encoder Standard használatával aaaHow toogenerate miniatűrök"
description: "Ez a témakör bemutatja, hogyan toouse .NET tooencode egy eszköz és a hello indexképének létrehozására használnak Media Encoder Standard használatával egyszerre."
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
ms.openlocfilehash: 23d3e4d9bf64a688d45499c045f19d2792167990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-thumbnails-using-media-encoder-standard-with-net"></a><span data-ttu-id="54f90-103">Hogyan toogenerate miniatűrök .NET Media Encoder Standard használatával</span><span class="sxs-lookup"><span data-stu-id="54f90-103">How toogenerate thumbnails using Media Encoder Standard with .NET</span></span>

<span data-ttu-id="54f90-104">Használhat Media Encoder Standard toogenerate egy vagy több miniatűrök a bemeneti videó [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), vagy [BMP](https://en.wikipedia.org/wiki/BMP_file_format) fájlformátumok lemezképet.</span><span class="sxs-lookup"><span data-stu-id="54f90-104">You can use Media Encoder Standard toogenerate one or more thumbnails from your input video in [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), or [BMP](https://en.wikipedia.org/wiki/BMP_file_format) image file formats.</span></span> <span data-ttu-id="54f90-105">Elküldheti a feladatokat, amelyek csak a képeket, vagy kombinálhatja a miniatűr generációs kódolással.</span><span class="sxs-lookup"><span data-stu-id="54f90-105">You can submit Tasks that produce only images, or you can combine thumbnail generation with encoding.</span></span> <span data-ttu-id="54f90-106">Ez a témakör néhány minta XML és JSON miniatűr készletek ilyen forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="54f90-106">This topic provides a few sample XML and JSON thumbnail presets for such scenarios.</span></span> <span data-ttu-id="54f90-107">Hello témakör hello végén van egy [példakód](#code_sample) , amely bemutatja, hogyan toouse hello Media Services .NET SDK tooaccomplish hello kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="54f90-107">At hello end of hello topic, there is a [sample code](#code_sample) that shows how toouse hello Media Services .NET SDK tooaccomplish hello encoding task.</span></span>

<span data-ttu-id="54f90-108">A minta készletek használt hello elemek további részletekért tekintse át [Media Encoder Standard séma](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="54f90-108">For more details on hello elements that are used in sample presets, you should review [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>

<span data-ttu-id="54f90-109">Győződjön meg arról, hogy tooreview hello [szempontok](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) szakasz.</span><span class="sxs-lookup"><span data-stu-id="54f90-109">Make sure tooreview hello [Considerations](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) section.</span></span>

## <a name="example--single-png-file"></a><span data-ttu-id="54f90-110">Példa – egyetlen PNG-fájl</span><span class="sxs-lookup"><span data-stu-id="54f90-110">Example – single PNG file</span></span>

<span data-ttu-id="54f90-111">a következő JSON hello, és XML-készlet használt tooproduce egy egyetlen kimeneti PNG fájlt hello kívül néhány másodperc hello bemeneti videó, ahol hello kódoló lehetővé teszi a legjobb kísérlet egy "érdekes" keret keresése.</span><span class="sxs-lookup"><span data-stu-id="54f90-111">hello following JSON and XML preset can be used tooproduce a single output PNG file out of hello first few seconds of hello input video, where hello encoder makes a best-effort attempt at finding an “interesting” frame.</span></span> <span data-ttu-id="54f90-112">Vegye figyelembe, hogy hello kimeneti kép méretei van állítva too100 %, ami azt jelenti, ezek fog egyezni a hello dimenziók hello bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="54f90-112">Note that hello output image dimensions have been set too100%, meaning these will match hello dimensions of hello input video.</span></span> <span data-ttu-id="54f90-113">Ügyeljen arra is, hogy a "Kimenetek" hello "Formátum" beállításra akkor szükség toomatch hello használata "PngLayers" hello "Kodekek" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="54f90-113">Note also how hello “Format” setting in “Outputs” is required toomatch hello use of “PngLayers” in hello “Codecs” section.</span></span> 

### <a name="json-preset"></a><span data-ttu-id="54f90-114">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-114">JSON preset</span></span>

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
    
### <a name="xml-preset"></a><span data-ttu-id="54f90-115">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-115">XML preset</span></span>

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

## <a name="example--a-series-of-jpeg-images"></a><span data-ttu-id="54f90-116">JPEG-képek sorozata – – példa</span><span class="sxs-lookup"><span data-stu-id="54f90-116">Example – a series of JPEG images</span></span>

<span data-ttu-id="54f90-117">a következő JSON hello, és XML-készlet használt tooproduce 5 %-os időbélyegeket 10 lemezképeit készlete 15 %,..., hello bemeneti ütemterv, ahol hello lemezkép mérete megadott toobe egy negyedév, hello bemeneti videó 95 %-át.</span><span class="sxs-lookup"><span data-stu-id="54f90-117">hello following JSON and XML preset can be used tooproduce a set of 10 images at timestamps of 5%, 15%, …, 95% of hello input timeline, where hello image size is specified toobe one quarter that of hello input video.</span></span>

### <a name="json-preset"></a><span data-ttu-id="54f90-118">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-118">JSON preset</span></span>

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

### <a name="xml-preset"></a><span data-ttu-id="54f90-119">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-119">XML preset</span></span>
    
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

## <a name="example--one-image-at-a-specific-timestamp"></a><span data-ttu-id="54f90-120">Példa – egy lemezképet egy adott időbélyeg</span><span class="sxs-lookup"><span data-stu-id="54f90-120">Example – one image at a specific timestamp</span></span>

<span data-ttu-id="54f90-121">JSON- és XML-készlet a következő hello használt tooproduce egy egyetlen hello képnek JPEG hello bemeneti videó megjelölni 30 másodperc is lehet.</span><span class="sxs-lookup"><span data-stu-id="54f90-121">hello following JSON and XML preset can be used tooproduce a single JPEG image at hello 30 second mark of hello input video.</span></span> <span data-ttu-id="54f90-122">Az előre definiált vár hello bemeneti toobe több mint 30 másodperc az időtartam (más hello feladat sikertelen lesz).</span><span class="sxs-lookup"><span data-stu-id="54f90-122">This preset expects hello input toobe more than 30 seconds in duration (else hello job will fail).</span></span>

### <a name="json-preset"></a><span data-ttu-id="54f90-123">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-123">JSON preset</span></span>

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
    
### <a name="xml-preset"></a><span data-ttu-id="54f90-124">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-124">XML preset</span></span>
    
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

## <span data-ttu-id="54f90-125"><a id="code_sample"></a>Példa – videó kódolása és miniatűr készítése</span><span class="sxs-lookup"><span data-stu-id="54f90-125"><a id="code_sample"></a>Example – encode video and generate thumbnail</span></span>

<span data-ttu-id="54f90-126">a következő példakód hello Media Services .NET SDK tooperform hello feladatok a következő használja:</span><span class="sxs-lookup"><span data-stu-id="54f90-126">hello following code example uses Media Services .NET SDK tooperform hello following tasks:</span></span>

* <span data-ttu-id="54f90-127">Hozzon létre egy kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="54f90-127">Create an encoding job.</span></span>
* <span data-ttu-id="54f90-128">Egy hivatkozási toohello Media Encoder Standard encoder beolvasása.</span><span class="sxs-lookup"><span data-stu-id="54f90-128">Get a reference toohello Media Encoder Standard encoder.</span></span>
* <span data-ttu-id="54f90-129">Betöltési hello beállított [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) vagy [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) hello kódolás beállított információk toogenerate miniatűrök igény szerint is tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="54f90-129">Load hello preset [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) that contain hello encoding preset as well as information needed toogenerate thumbnails.</span></span> <span data-ttu-id="54f90-130">Ez mentheti [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) vagy [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) a következő kód tooload hello fájl egy fájl és -felhasználási hello.</span><span class="sxs-lookup"><span data-stu-id="54f90-130">You can save this  [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) or [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) in a file and use hello following code tooload hello file.</span></span>
  
        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(fileName);  
* <span data-ttu-id="54f90-131">Adjon hozzá egy kódolási feladat toohello feladatban.</span><span class="sxs-lookup"><span data-stu-id="54f90-131">Add a single encoding task toohello job.</span></span> 
* <span data-ttu-id="54f90-132">Adjon meg bemeneti hello eszköz toobe kódolású.</span><span class="sxs-lookup"><span data-stu-id="54f90-132">Specify hello input asset toobe encoded.</span></span>
* <span data-ttu-id="54f90-133">Hozzon létre egy kimeneti eszköz, amely kódolású hello eszköz fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="54f90-133">Create an output asset that will contain hello encoded asset.</span></span>
* <span data-ttu-id="54f90-134">Adjon hozzá egy esemény kezelő toocheck hello feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="54f90-134">Add an event handler toocheck hello job progress.</span></span>
* <span data-ttu-id="54f90-135">Hello feladat küldése</span><span class="sxs-lookup"><span data-stu-id="54f90-135">Submit hello job.</span></span>

<span data-ttu-id="54f90-136">Lásd: hello [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md) kapcsolatos utasításokat a témakör a fejlesztési környezet létrehozása tooset.</span><span class="sxs-lookup"><span data-stu-id="54f90-136">See hello [Media Services development with .NET](media-services-dotnet-how-to-use.md) topic for directions on how tooset up your dev environment.</span></span>

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
            // Read values from hello App.config file.
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

            // Encode and generate hello thumbnails.
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

            // Load hello XML (or JSON) from hello local file.
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                processor,
                configuration,
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

## <span data-ttu-id="54f90-137"><a id="json"></a>A miniatűr JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-137"><a id="json"></a>Thumbnail JSON preset</span></span>
<span data-ttu-id="54f90-138">Séma kapcsolatos információkért lásd: [ez](https://msdn.microsoft.com/library/mt269962.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="54f90-138">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>

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

## <span data-ttu-id="54f90-139"><a id="xml"></a>A miniatűr XML-készlet</span><span class="sxs-lookup"><span data-stu-id="54f90-139"><a id="xml"></a>Thumbnail XML preset</span></span>
<span data-ttu-id="54f90-140">Séma kapcsolatos információkért lásd: [ez](https://msdn.microsoft.com/library/mt269962.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="54f90-140">For information about schema, see [this](https://msdn.microsoft.com/library/mt269962.aspx) topic.</span></span>
    
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

## <a name="considerations"></a><span data-ttu-id="54f90-141">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="54f90-141">Considerations</span></span>
<span data-ttu-id="54f90-142">a következő szempontok hello vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="54f90-142">hello following considerations apply:</span></span>

* <span data-ttu-id="54f90-143">Start/lépés/címtartomány explicit időbélyegeket hello használata feltételezi, hogy hello bemeneti forrás legalább 1 percnek hosszú.</span><span class="sxs-lookup"><span data-stu-id="54f90-143">hello use of explicit timestamps for Start/Step/Range assumes that hello input source is at least 1 minute long.</span></span>
* <span data-ttu-id="54f90-144">JPG vagy Png/BmpImage rendelkezniük Start, lépés és karakterlánc-attribútumok tartomány – ezek úgy:</span><span class="sxs-lookup"><span data-stu-id="54f90-144">Jpg/Png/BmpImage elements have Start, Step and Range string attributes – these can be interpreted as:</span></span>
  
  * <span data-ttu-id="54f90-145">Ha nem negatív egész számokat, például azok veremkeret száma.</span><span class="sxs-lookup"><span data-stu-id="54f90-145">Frame Number if they are non-negative integers, eg.</span></span> <span data-ttu-id="54f90-146">"Start": "120"</span><span class="sxs-lookup"><span data-stu-id="54f90-146">"Start": "120",</span></span>
  * <span data-ttu-id="54f90-147">Ha kifejezett % utótaggal, pl. relatív toosource időtartama.</span><span class="sxs-lookup"><span data-stu-id="54f90-147">Relative toosource duration if expressed as %-suffixed, eg.</span></span> <span data-ttu-id="54f90-148">"Start": "15 %", vagy</span><span class="sxs-lookup"><span data-stu-id="54f90-148">"Start": "15%", OR</span></span>
  * <span data-ttu-id="54f90-149">Ha óó: pp: kifejezett időbélyeg...</span><span class="sxs-lookup"><span data-stu-id="54f90-149">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="54f90-150">formátumban.</span><span class="sxs-lookup"><span data-stu-id="54f90-150">format.</span></span> <span data-ttu-id="54f90-151">EG.</span><span class="sxs-lookup"><span data-stu-id="54f90-151">Eg.</span></span> <span data-ttu-id="54f90-152">"Start": "00: 01:00"</span><span class="sxs-lookup"><span data-stu-id="54f90-152">"Start" : "00:01:00"</span></span>
    
    <span data-ttu-id="54f90-153">Ön szabadon kombinálhatók jelölések, ha Ön adja.</span><span class="sxs-lookup"><span data-stu-id="54f90-153">You can mix and match notations as you please.</span></span>
    
    <span data-ttu-id="54f90-154">Emellett Start is támogatja a speciális makró: {ajánlott}, amely megpróbál toodetermine hello első "érdekes" keret hello tartalom Megjegyzés: (lépés, és a tartomány figyelmen kívül lesznek hagyva Start túl beállításakor {legjobb})</span><span class="sxs-lookup"><span data-stu-id="54f90-154">Additionally, Start also supports a special Macro:{Best}, which attempts toodetermine hello first “interesting” frame of hello content NOTE: (Step and Range are ignored when Start is set too{Best})</span></span>
  * <span data-ttu-id="54f90-155">Alapértelmezett: Start: {legjobb}</span><span class="sxs-lookup"><span data-stu-id="54f90-155">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="54f90-156">Kimeneti formátumot kell minden egyes képformátum explicit módon megadott toobe: Jpg vagy Png/BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="54f90-156">Output format needs toobe explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="54f90-157">Ha létezik, a MES JpgVideo tooJpgFormat és így tovább fog egyezni.</span><span class="sxs-lookup"><span data-stu-id="54f90-157">When present, MES will match JpgVideo tooJpgFormat and so on.</span></span> <span data-ttu-id="54f90-158">OutputFormat bevezet egy új lemezkép-kodek adott makró: {Index}, amely kell toobe jelen (egyszer és csak egyszer) kép kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="54f90-158">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs toobe present (once and only once) for image output formats.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54f90-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54f90-159">Next steps</span></span>

<span data-ttu-id="54f90-160">Ellenőrizheti a hello [folyamatban lévő feladat](media-services-check-job-progress.md) közben hello kódolási feladat folyamatban.</span><span class="sxs-lookup"><span data-stu-id="54f90-160">You can check hello [job progress](media-services-check-job-progress.md) while hello encoding job is pending.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="54f90-161">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="54f90-161">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="54f90-162">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="54f90-162">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="54f90-163">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="54f90-163">See Also</span></span>
[<span data-ttu-id="54f90-164">Media Services kódolási áttekintése</span><span class="sxs-lookup"><span data-stu-id="54f90-164">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)


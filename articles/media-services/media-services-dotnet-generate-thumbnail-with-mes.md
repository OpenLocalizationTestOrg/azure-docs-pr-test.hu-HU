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
# <a name="how-toogenerate-thumbnails-using-media-encoder-standard-with-net"></a>Hogyan toogenerate miniatűrök .NET Media Encoder Standard használatával

Használhat Media Encoder Standard toogenerate egy vagy több miniatűrök a bemeneti videó [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), vagy [BMP](https://en.wikipedia.org/wiki/BMP_file_format) fájlformátumok lemezképet. Elküldheti a feladatokat, amelyek csak a képeket, vagy kombinálhatja a miniatűr generációs kódolással. Ez a témakör néhány minta XML és JSON miniatűr készletek ilyen forgatókönyvek esetén. Hello témakör hello végén van egy [példakód](#code_sample) , amely bemutatja, hogyan toouse hello Media Services .NET SDK tooaccomplish hello kódolási feladat.

A minta készletek használt hello elemek további részletekért tekintse át [Media Encoder Standard séma](media-services-mes-schema.md).

Győződjön meg arról, hogy tooreview hello [szempontok](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) szakasz.

## <a name="example--single-png-file"></a>Példa – egyetlen PNG-fájl

a következő JSON hello, és XML-készlet használt tooproduce egy egyetlen kimeneti PNG fájlt hello kívül néhány másodperc hello bemeneti videó, ahol hello kódoló lehetővé teszi a legjobb kísérlet egy "érdekes" keret keresése. Vegye figyelembe, hogy hello kimeneti kép méretei van állítva too100 %, ami azt jelenti, ezek fog egyezni a hello dimenziók hello bemeneti videó. Ügyeljen arra is, hogy a "Kimenetek" hello "Formátum" beállításra akkor szükség toomatch hello használata "PngLayers" hello "Kodekek" szakaszában. 

### <a name="json-preset"></a>JSON-készlet

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
    
### <a name="xml-preset"></a>XML-készlet

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

## <a name="example--a-series-of-jpeg-images"></a>JPEG-képek sorozata – – példa

a következő JSON hello, és XML-készlet használt tooproduce 5 %-os időbélyegeket 10 lemezképeit készlete 15 %,..., hello bemeneti ütemterv, ahol hello lemezkép mérete megadott toobe egy negyedév, hello bemeneti videó 95 %-át.

### <a name="json-preset"></a>JSON-készlet

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

### <a name="xml-preset"></a>XML-készlet
    
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

## <a name="example--one-image-at-a-specific-timestamp"></a>Példa – egy lemezképet egy adott időbélyeg

JSON- és XML-készlet a következő hello használt tooproduce egy egyetlen hello képnek JPEG hello bemeneti videó megjelölni 30 másodperc is lehet. Az előre definiált vár hello bemeneti toobe több mint 30 másodperc az időtartam (más hello feladat sikertelen lesz).

### <a name="json-preset"></a>JSON-készlet

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
    
### <a name="xml-preset"></a>XML-készlet
    
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

## <a id="code_sample"></a>Példa – videó kódolása és miniatűr készítése

a következő példakód hello Media Services .NET SDK tooperform hello feladatok a következő használja:

* Hozzon létre egy kódolási feladat.
* Egy hivatkozási toohello Media Encoder Standard encoder beolvasása.
* Betöltési hello beállított [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) vagy [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) hello kódolás beállított információk toogenerate miniatűrök igény szerint is tartalmaznak. Ez mentheti [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) vagy [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) a következő kód tooload hello fájl egy fájl és -felhasználási hello.
  
        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(fileName);  
* Adjon hozzá egy kódolási feladat toohello feladatban. 
* Adjon meg bemeneti hello eszköz toobe kódolású.
* Hozzon létre egy kimeneti eszköz, amely kódolású hello eszköz fogja tartalmazni.
* Adjon hozzá egy esemény kezelő toocheck hello feladat előrehaladását.
* Hello feladat küldése

Lásd: hello [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md) kapcsolatos utasításokat a témakör a fejlesztési környezet létrehozása tooset.

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

## <a id="json"></a>A miniatűr JSON-készlet
Séma kapcsolatos információkért lásd: [ez](https://msdn.microsoft.com/library/mt269962.aspx) témakör.

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

## <a id="xml"></a>A miniatűr XML-készlet
Séma kapcsolatos információkért lásd: [ez](https://msdn.microsoft.com/library/mt269962.aspx) témakör.
    
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

## <a name="considerations"></a>Megfontolandó szempontok
a következő szempontok hello vonatkoznak:

* Start/lépés/címtartomány explicit időbélyegeket hello használata feltételezi, hogy hello bemeneti forrás legalább 1 percnek hosszú.
* JPG vagy Png/BmpImage rendelkezniük Start, lépés és karakterlánc-attribútumok tartomány – ezek úgy:
  
  * Ha nem negatív egész számokat, például azok veremkeret száma. "Start": "120"
  * Ha kifejezett % utótaggal, pl. relatív toosource időtartama. "Start": "15 %", vagy
  * Ha óó: pp: kifejezett időbélyeg... formátumban. EG. "Start": "00: 01:00"
    
    Ön szabadon kombinálhatók jelölések, ha Ön adja.
    
    Emellett Start is támogatja a speciális makró: {ajánlott}, amely megpróbál toodetermine hello első "érdekes" keret hello tartalom Megjegyzés: (lépés, és a tartomány figyelmen kívül lesznek hagyva Start túl beállításakor {legjobb})
  * Alapértelmezett: Start: {legjobb}
* Kimeneti formátumot kell minden egyes képformátum explicit módon megadott toobe: Jpg vagy Png/BmpFormat. Ha létezik, a MES JpgVideo tooJpgFormat és így tovább fog egyezni. OutputFormat bevezet egy új lemezkép-kodek adott makró: {Index}, amely kell toobe jelen (egyszer és csak egyszer) kép kimeneti formátum.

## <a name="next-steps"></a>Következő lépések

Ellenőrizheti a hello [folyamatban lévő feladat](media-services-check-job-progress.md) közben hello kódolási feladat folyamatban.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Media Services kódolási áttekintése](media-services-encode-asset.md)


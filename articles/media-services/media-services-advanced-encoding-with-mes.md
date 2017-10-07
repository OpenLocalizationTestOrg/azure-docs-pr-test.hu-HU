---
title: "aaaPerform speciális MES készletek testreszabásával kódolás |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooperform speciális Media Encoder Standard feladat készletek testreszabásával kódolást."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: juliako
ms.openlocfilehash: 9caa68fafacaf51f91f0554c5bafe491928d8c77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a>Kódolásra speciális MES készletek testreszabásával. 

## <a name="overview"></a>Áttekintés

Ez a témakör bemutatja, hogyan Media Encoder Standard toocustomize készleteket. Hello [kódolása a Media Encoder Standard használatával egyéni készletek](media-services-custom-mes-presets-with-dotnet.md) a témakör bemutatja, hogyan toouse .NET toocreate kódolással feladat és egy feladatot, amely végrehajtja ezt a feladatot. Miután testre egy készletet, adjon meg hello egyéni készletek toohello kódolási feladat. 

>[!NOTE]
>Ha egy XML-készletet használ, győződjön meg arról, hogy toopreserve hello elemek sorrendjét, ahogy az alábbi XML-minták (például KeyFrameInterval előtt kell állnia SceneChangeDetection).
>

Ebben a témakörben hello egyéni készletek hajtsa végre a következő kódolási feladatok hello egy.

## <a name="support-for-relative-sizes"></a>Relatív méretek támogatása

Miniatűrök létrehozásakor nem kell tooalways meg kimeneti szélessége és magassága képpontban megadva. Százalékos [1 %,..., 100 %-os] hello tartományban is adja meg azokat.

### <a name="json-preset"></a>JSON-készlet
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a>XML-készlet
    <Width>100%</Width>
    <Height>100%</Height>

## <a id="thumbnails"></a>Indexképének létrehozására

Ez a szakasz bemutatja, hogyan toocustomize egy készletet, amely hoz létre a miniatűrökön. hello előre definiált alább megadott információkat tartalmaz az tooencode módját a fájlt, valamint toogenerate miniatűrök szükséges információk. Bármelyik hello MES készletek dokumentált eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és adja hozzá a miniatűrök generáló kódot.  

> [!NOTE]
> Hello **SceneChangeDetection** a következő előre definiált hello-beállítás csak akkor állítható tootrue Ha kódolni kívánt tooa egyféle sávszélességű videó. Ha tooa többszörös sávszélességű kódolni kívánt videó és beállított **SceneChangeDetection** tootrue, a hello kódoló hibát ad vissza.  
>
>

Séma kapcsolatos információkért lásd: [ez](media-services-mes-schema.md) témakör.

Győződjön meg arról, hogy tooreview hello [szempontok](#considerations) szakasz.

### <a id="json"></a>JSON-készlet
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
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
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
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a id="xml"></a>XML-készlet
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
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

### <a name="considerations"></a>Megfontolandó szempontok

a következő szempontok hello vonatkoznak:

* Start/lépés/címtartomány explicit időbélyegeket hello használata feltételezi, hogy hello bemeneti forrás legalább 1 percnek hosszú.
* JPG vagy Png/BmpImage elemek elindítani, lépés, és karakterlánc-attribútumok között – ezek úgy:

  * Keret számát, ha azok nem negatív egész számokat, például "Start": "120"
  * Ha % utótaggal kifejezett relatív toosource időtartam például "Start": "15 %", vagy
  * Ha óó: pp: kifejezett időbélyeg... formázása, például "Start": "00: 01:00"

    Ön szabadon kombinálhatók jelölések, ha Ön adja.

    Emellett Start is támogatja a speciális makró: {ajánlott}, amely megpróbál toodetermine hello első "érdekes" keret hello tartalom Megjegyzés: (lépés, és a tartomány figyelmen kívül lesznek hagyva Start túl beállításakor {legjobb})
  * Alapértelmezett: Start: {legjobb}
* Kimeneti formátumot kell minden egyes képformátum explicit módon megadott toobe: Jpg vagy Png/BmpFormat. Ha létezik, a MES és így tovább JpgVideo tooJpgFormat megegyezik. OutputFormat bevezet egy új lemezkép-kodek adott makró: {Index}, amely kell toobe jelen (egyszer és csak egyszer) kép kimeneti formátum.

## <a id="trim_video"></a>Trim (vágás) videó
Ez a szakasz megbeszélések hello kódoló módosításával kapcsolatos tooclip készletet, vagy trim hello bemeneti videó hello bemenet esetén egy úgynevezett mezzazine-fájlt vagy a tárolt fájl. hello kódoló is lehet használt tooclip vagy egy eszköz, ez rögzített vagy egy élő adatfolyam archivált trim – hello részleteket a [ebben a blogban](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

tootrim a videók készíthet bármelyik hello MES készletek részletes ismertetését lásd [ez](media-services-mes-presets-overview.md) szakaszt, és módosítsa a hello **források** elem (mivel lásd alább). StartTime hello értékének kell toomatch hello abszolút időbélyegeket hello bemeneti videó. Például ha hello hello bemeneti videó első keret 12:00:10.000 időbélyeg, akkor az StartTime kell lennie, mint 12:00:10.000 és nagyobb. Hello az alábbi példában feltételezzük, hogy rendelkezik-e nulla kezdési időbélyeg hello bemeneti videóhoz. **Adatforrások** hello előre definiált hello elején kell elhelyezni.

### <a id="json"></a>JSON-készlet
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
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

### <a name="xml-preset"></a>XML-készlet
tootrim a videók készíthet hello MES készletek dokumentált bármelyikét [Itt](media-services-mes-presets-overview.md) , és módosítsa a hello **források** eleme (lásd alább).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
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
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

## <a id="overlay"></a>Hozzon létre egy átmeneti területre

Media Encoder Standard hello lehetővé teszi egy meglévő video egy képet toooverlay. Jelenleg a következő formátumok hello támogatottak: png, jpg, gif, bmp és. hello alább megadott készlet példája egy alapszintű egy videófelirat.

Továbbá toodefining egy előre definiált fájlt, is toolet Media Services tudja, melyik hello eszköz fájlban hello átfedő lemezképet, és mely fájl hello forrás videó, amelyre kívánja toooverlay hello lemezképet. hello videofájl rendelkezik toobe hello **elsődleges** fájlt.

Ha .NET használ, adja hozzá a két funkciók definiált toohello .NET típusú példát a következő hello [ez](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) témakör. Hello **UploadMediaFilesFromFolder** függvény fel a fájlokat egy mappából (például BigBuckBunny.mp4 és Image001.png) és a készletek hello mp4 toobe hello elsődleges fájl hello eszközt. Hello **EncodeWithOverlay** függvény hello egyéni előre definiált átadott fájl tooit használja (például hello készletet, hogy az alábbiak szerint) toocreate hello kódolási feladat.


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // hello following code assumes 
            // you have an input folder with one MP4 and one overlay image file.
            if (af.Name.Contains(".mp4"))
                af.IsPrimary = true;
            else
                af.IsPrimary = false;
    
            af.Update();
        }
    
        return asset;
    }

    static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("Media Encoder Standard Job");
        // Get a media processor reference, and pass tooit hello name of hello 
        // processor toouse for hello specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input assets toobe encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset toocontain hello results of hello job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> Aktuális korlátozások vonatkoznak:
>
> hello átfedő átlátszatlanság beállítás nem támogatott.
>
> A forrás videofájl és hello átfedő képfájl rendelkezik toobe a hello ugyanazon eszközre, és videofájl igények toobe set hello hello elsődleges fájlban található, ez az eszköz.
>
>

### <a name="json-preset"></a>JSON-készlet
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a>XML-készlet
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


## <a id="silent_audio"></a>Csendes hang nyomon beszúrni, ha bemeneti nincsenek hang
Alapértelmezés szerint ha egy bemeneti toohello kódoló csak videó, és nincsenek hang tartalmazó küld majd hello kimeneti adategységen tartalmaz csak videó adatokat tartalmazó fájlt. Néhány lejátszó nem lehet ilyen toohandle kimenetét. Ez a beállítás tooforce hello kódoló tooadd egy csendes hang követése toohello kimeneti forgatókönyv használható.

tooforce hello kódoló tooproduce Ha bemeneti nincsenek hang-, hang csendes nyomon tartalmazó hello "InsertSilenceIfNoAudio" értéket adjon meg.

Bármelyik MES készletek részletes ismertetését lásd: hello eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és ellenőrizze hello módosítását követően:

### <a name="json-preset"></a>JSON-készlet
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a>XML-készlet
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <a id="deinterlacing"></a>Automatikus deszerializálni rövidebbnek letiltása
Az ügyfelek nem kell toodo semmit, ha azok például hello váltott automatikusan deszerializálni váltakozó tartalma toobe. Ha be van kapcsolva (alapértelmezés) hello MES hello hello automatikus deszerializálni rövidebbnek automatikus észlelési váltakozó keretek, és csak deszerializálni interlaces keretek váltakozó megjelölve.

Kikapcsolhatja hello automatikus deszerializálni rövidebbnek. Ez a lehetőség nem ajánlott.

### <a name="json-preset"></a>JSON-készlet
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a>XML-készlet
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <a id="audio_only"></a>Csak hang-készletek
Ez a szakasz azt mutatja be két csak MES készletek: AAC hang- és AAC jó minőségű hang.

### <a name="aac-audio"></a>AAC hang
    {
      "Version": 1.0,
      "Codecs": [
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
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="aac-good-quality-audio"></a>AAC jó minőségű hang
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="concatenate"></a>ÖSSZEFŰZ két vagy több videofájlok

hello alábbi példa bemutatja, hogyan hozhat létre egy előre definiált tooconcatenate két vagy több videofájlok lejátszását. hello leggyakoribb eset az, amikor tooadd fejléc vagy pótkocsi toohello fő videó. hello szánt együtt szerkesztett hello videofájlok megosztás tulajdonságai (képernyőfelbontást képkockasebessége, zenei száma, stb.) használata esetén. Meg nem toomix videók különböző keret díjszabás, vagy eltérő számú zeneszámok gondoskodunk.

>[!NOTE]
>hello hello összefűzése funkció aktuális kialakítása vár, hogy hello bemeneti videó videóklipeket konzisztensek tekintetében megoldást képkockasebessége stb. 

### <a name="requirements-and-considerations"></a>Követelmények és szempontok

* A bemeneti videó csak kell rendelkeznie egy hang követése.
* Bemeneti videók kell összes hello azonos képkockasebessége.
* Kell tölteni a videók a különböző eszközök, és hello videók állítja be hello elsődleges fájl az egyes eszközökre.
* A videók tooknow hello időtartama van szüksége.
* hello előre definiált az alábbi példák azt feltételezi, hogy minden hello bemeneti videók indítsa el az időbélyegzőnek a nulla. Szüksége toomodify hello StartTime értékekre Ha hello videók különböző kezdési Timestamp típusú, mint általában hello élő archívummal rendelkező.
* hello JSON előre definiált teszi explicit hivatkozásokat hello bemeneti eszközök toohello AssetID értékét.
* hello példakód azt feltételezi, hogy a JSON-készlet hello tooa helyi fájl, például "C:\supportFiles\preset.json" mentése megtörtént. Azt is feltételezi, hogy létrejöttek-e a két eszközök két videofájlok feltöltésével, és arról, hogy hello eredő AssetID értékek.
* hello kódrészletet és JSON készletet két videofájlok hozzáfűzésével példáját mutatja be. Kiterjesztheti úgy, mint két videók által toomore:

  1. Hívása a feladatot. InputAssets.Add() ismételten tooadd további videók, sorrendben.
  2. Hello több bejegyzés hozzáadásával toohello "források" elemének hello JSON, így a megfelelő szerkesztése ugyanabban a sorrendben.

### <a name="net-code"></a>.NET-kódot

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass tooit hello name of the
    // processor toouse for hello specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load hello XML (or JSON) from hello local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify hello input videos toobe concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset toocontain hello results of hello job.
    // This output is specified as AssetCreationOptions.None, which
    // means hello output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a>JSON-készlet

Az azonosítók, amelyet az tooconcatenate hello eszközök, és hello megfelelő idő szegmens minden egyes videó az egyéni frissítés.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
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
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="crop"></a>A Media Encoder Standard körülvágása videók
Lásd: hello [a Media Encoder Standard videók körülvágása](media-services-crop-video.md) témakör.

## <a id="no_video"></a>Videó nyomon beszúrni, ha bemeneti nincs kép

Alapértelmezés szerint ha egy bemeneti toohello kódoló csak hang, és a nem kép tartalmazó küldi el majd hello kimeneti adategységen tartalmaz csak hang adatokat tartalmazó fájlt. Néhány szereplő, köztük az Azure Media Player (lásd: [ez](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) nem lehet képes toohandle ilyen adatfolyamokat. Használhatja ezt a beállítást tooforce hello kódoló tooadd egy fekete-fehér videó követése toohello kimeneti forgatókönyv.

> [!NOTE]
> Kényszerített hello kódoló tooinsert egy kimeneti videó követése növekszik hello méretét hello kimeneti eszköz, és ezáltal a kódolási feladat hello költsége hello. Végezzen tesztek tooverify, amelyek az eredő növelését csak mérsékelt hatása van a havi költségeket.
>

### <a name="inserting-video-at-only-hello-lowest-bitrate"></a>Videó: csak hello legalacsonyabb sávszélességű beszúrása

Tegyük fel, amelyek több sávszélességű kódolás használatával, mint a beállított ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode a teljes videofájlok lejátszását, és csak hang-fájlokat tartalmazó katalógus az adatfolyamként történő, adjon meg. Ebben az esetben ha hello bemeneti nincs videó-érdemes lehet tooforce hello kódoló tooinsert megakadályozását tooinserting videó minden kimeneti sávszélességű, mint a csak hello legalacsonyabb sávszélességű, nyomon egy videó egyszínűen. tooachieve, toouse hello kell **InsertBlackIfNoVideoBottomLayerOnly** jelzőt.

Bármelyik MES készletek részletes ismertetését lásd: hello eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és ellenőrizze hello módosítását követően:

#### <a name="json-preset"></a>JSON-készlet
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-készlet

XML használatakor használjon feltétel = "InsertBlackIfNoVideoBottomLayerOnly" egy attribútum toohello **H264Video** elem és a feltétel túl = a "InsertSilenceIfNoAudio" attribútumaként**AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideoBottomLayerOnly">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .
```

### <a name="inserting-video-at-all-output-bitrates"></a>A kimeneti bitrates videó minden beszúrása
Tegyük fel, amelyek több sávszélességű kódolás használatával, mint a beállított ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode a teljes videofájlok lejátszását, és csak hang-fájlokat tartalmazó katalógus az adatfolyamként történő, adjon meg. Ebben az esetben ha hello bemeneti nincs videó-érdemes lehet tooforce hello kódoló tooinsert fekete-fehér videó: az összes hello kimeneti bitrates nyomon. Ez biztosítja, hogy a kimeneti összes homogén tekintetben toonumber videó nyomon követi és zeneszámok az olyan eszközök. tooachieve, toospecify hello "InsertBlackIfNoVideo" jelző van szüksége.

Bármelyik MES készletek részletes ismertetését lásd: hello eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és ellenőrizze hello módosítását követően:

#### <a name="json-preset"></a>JSON-készlet
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-készlet

XML használatakor használjon feltétel = "InsertBlackIfNoVideo" egy attribútum toohello **H264Video** elem és a feltétel túl = a "InsertSilenceIfNoAudio" attribútumaként**AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideo">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .  
```

## <a id="rotate_video"></a>Videó elforgatása
Hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) elforgatási szög által 0/90/180 vagy 270 támogatja. hello alapértelmezés lesz az "Auto", ha megpróbál toodetect hello Elforgatás metaadatok hello bejövő videofájl és kártalanítja azt. Adja meg a következőket hello **források** elem tooone definiált hello készleteket [ez](media-services-mes-presets-overview.md) szakasz:

### <a name="json-preset"></a>JSON-készlet
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...
### <a name="xml-preset"></a>XML-készlet
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Lásd még [ez](media-services-mes-schema.md#PreserveResolutionAfterRotation) hello kódoló értelmezése hello szélességének és magasságának beállítások hello további információt a témakör az adott néven beállítás, elforgatás kompenzációs indítását.

Hello érték "0" tooindicate toohello kódoló tooignore Elforgatás metaadatok, ha jelen van, a bemeneti videó hello használhatja.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Media Services kódolási áttekintése](media-services-encode-asset.md)

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
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a><span data-ttu-id="3b473-103">Kódolásra speciális MES készletek testreszabásával.</span><span class="sxs-lookup"><span data-stu-id="3b473-103">Perform advanced encoding by customizing MES presets</span></span> 

## <a name="overview"></a><span data-ttu-id="3b473-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3b473-104">Overview</span></span>

<span data-ttu-id="3b473-105">Ez a témakör bemutatja, hogyan Media Encoder Standard toocustomize készleteket.</span><span class="sxs-lookup"><span data-stu-id="3b473-105">This topic shows how toocustomize Media Encoder Standard presets.</span></span> <span data-ttu-id="3b473-106">Hello [kódolása a Media Encoder Standard használatával egyéni készletek](media-services-custom-mes-presets-with-dotnet.md) a témakör bemutatja, hogyan toouse .NET toocreate kódolással feladat és egy feladatot, amely végrehajtja ezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="3b473-106">hello [Encoding with Media Encoder Standard using custom presets](media-services-custom-mes-presets-with-dotnet.md) topic shows how toouse .NET toocreate an encoding task and a job that executes this task.</span></span> <span data-ttu-id="3b473-107">Miután testre egy készletet, adjon meg hello egyéni készletek toohello kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="3b473-107">Once you customize a preset, supply hello custom presets toohello encoding task.</span></span> 

>[!NOTE]
><span data-ttu-id="3b473-108">Ha egy XML-készletet használ, győződjön meg arról, hogy toopreserve hello elemek sorrendjét, ahogy az alábbi XML-minták (például KeyFrameInterval előtt kell állnia SceneChangeDetection).</span><span class="sxs-lookup"><span data-stu-id="3b473-108">If using an XML preset, make sure toopreserve hello order of elements, as shown in XML samples below (for example, KeyFrameInterval should precede SceneChangeDetection).</span></span>
>

<span data-ttu-id="3b473-109">Ebben a témakörben hello egyéni készletek hajtsa végre a következő kódolási feladatok hello egy.</span><span class="sxs-lookup"><span data-stu-id="3b473-109">In this topic, hello custom presets that perform hello following encoding tasks are demonstrated.</span></span>

## <a name="support-for-relative-sizes"></a><span data-ttu-id="3b473-110">Relatív méretek támogatása</span><span class="sxs-lookup"><span data-stu-id="3b473-110">Support for relative sizes</span></span>

<span data-ttu-id="3b473-111">Miniatűrök létrehozásakor nem kell tooalways meg kimeneti szélessége és magassága képpontban megadva.</span><span class="sxs-lookup"><span data-stu-id="3b473-111">When generating thumbnails, you do not need tooalways specify output width and height in pixels.</span></span> <span data-ttu-id="3b473-112">Százalékos [1 %,..., 100 %-os] hello tartományban is adja meg azokat.</span><span class="sxs-lookup"><span data-stu-id="3b473-112">You can specify them in percentages, in hello range [1%, …, 100%].</span></span>

### <a name="json-preset"></a><span data-ttu-id="3b473-113">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-113">JSON preset</span></span>
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a><span data-ttu-id="3b473-114">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-114">XML preset</span></span>
    <Width>100%</Width>
    <Height>100%</Height>

## <span data-ttu-id="3b473-115"><a id="thumbnails"></a>Indexképének létrehozására</span><span class="sxs-lookup"><span data-stu-id="3b473-115"><a id="thumbnails"></a>Generate thumbnails</span></span>

<span data-ttu-id="3b473-116">Ez a szakasz bemutatja, hogyan toocustomize egy készletet, amely hoz létre a miniatűrökön.</span><span class="sxs-lookup"><span data-stu-id="3b473-116">This section shows how toocustomize a preset that generates thumbnails.</span></span> <span data-ttu-id="3b473-117">hello előre definiált alább megadott információkat tartalmaz az tooencode módját a fájlt, valamint toogenerate miniatűrök szükséges információk.</span><span class="sxs-lookup"><span data-stu-id="3b473-117">hello preset defined below contains information on how you want tooencode your file as well as information needed toogenerate thumbnails.</span></span> <span data-ttu-id="3b473-118">Bármelyik hello MES készletek dokumentált eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és adja hozzá a miniatűrök generáló kódot.</span><span class="sxs-lookup"><span data-stu-id="3b473-118">You can take any of hello MES presets documented [this](media-services-mes-presets-overview.md) section and add code that generates thumbnails.</span></span>  

> [!NOTE]
> <span data-ttu-id="3b473-119">Hello **SceneChangeDetection** a következő előre definiált hello-beállítás csak akkor állítható tootrue Ha kódolni kívánt tooa egyféle sávszélességű videó.</span><span class="sxs-lookup"><span data-stu-id="3b473-119">hello **SceneChangeDetection** setting in hello following preset can only be set tootrue if you are encoding tooa single  bitrate video.</span></span> <span data-ttu-id="3b473-120">Ha tooa többszörös sávszélességű kódolni kívánt videó és beállított **SceneChangeDetection** tootrue, a hello kódoló hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="3b473-120">If you are encoding tooa multi-bitrate video and set **SceneChangeDetection** tootrue, hello encoder returns an error.</span></span>  
>
>

<span data-ttu-id="3b473-121">Séma kapcsolatos információkért lásd: [ez](media-services-mes-schema.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="3b473-121">For information about schema, see [this](media-services-mes-schema.md) topic.</span></span>

<span data-ttu-id="3b473-122">Győződjön meg arról, hogy tooreview hello [szempontok](#considerations) szakasz.</span><span class="sxs-lookup"><span data-stu-id="3b473-122">Make sure tooreview hello [Considerations](#considerations) section.</span></span>

### <span data-ttu-id="3b473-123"><a id="json"></a>JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-123"><a id="json"></a>JSON preset</span></span>
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


### <span data-ttu-id="3b473-124"><a id="xml"></a>XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-124"><a id="xml"></a>XML preset</span></span>
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

### <a name="considerations"></a><span data-ttu-id="3b473-125">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="3b473-125">Considerations</span></span>

<span data-ttu-id="3b473-126">a következő szempontok hello vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="3b473-126">hello following considerations apply:</span></span>

* <span data-ttu-id="3b473-127">Start/lépés/címtartomány explicit időbélyegeket hello használata feltételezi, hogy hello bemeneti forrás legalább 1 percnek hosszú.</span><span class="sxs-lookup"><span data-stu-id="3b473-127">hello use of explicit timestamps for Start/Step/Range assumes that hello input source is at least 1 minute long.</span></span>
* <span data-ttu-id="3b473-128">JPG vagy Png/BmpImage elemek elindítani, lépés, és karakterlánc-attribútumok között – ezek úgy:</span><span class="sxs-lookup"><span data-stu-id="3b473-128">Jpg/Png/BmpImage elements have Start, Step, and Range string attributes – these can be interpreted as:</span></span>

  * <span data-ttu-id="3b473-129">Keret számát, ha azok nem negatív egész számokat, például "Start": "120"</span><span class="sxs-lookup"><span data-stu-id="3b473-129">Frame Number if they are non-negative integers, for example "Start": "120",</span></span>
  * <span data-ttu-id="3b473-130">Ha % utótaggal kifejezett relatív toosource időtartam például "Start": "15 %", vagy</span><span class="sxs-lookup"><span data-stu-id="3b473-130">Relative toosource duration if expressed as %-suffixed, for example "Start": "15%", OR</span></span>
  * <span data-ttu-id="3b473-131">Ha óó: pp: kifejezett időbélyeg...</span><span class="sxs-lookup"><span data-stu-id="3b473-131">Timestamp if expressed as HH:MM:SS…</span></span> <span data-ttu-id="3b473-132">formázása, például "Start": "00: 01:00"</span><span class="sxs-lookup"><span data-stu-id="3b473-132">format, for example "Start" : "00:01:00"</span></span>

    <span data-ttu-id="3b473-133">Ön szabadon kombinálhatók jelölések, ha Ön adja.</span><span class="sxs-lookup"><span data-stu-id="3b473-133">You can mix and match notations as you please.</span></span>

    <span data-ttu-id="3b473-134">Emellett Start is támogatja a speciális makró: {ajánlott}, amely megpróbál toodetermine hello első "érdekes" keret hello tartalom Megjegyzés: (lépés, és a tartomány figyelmen kívül lesznek hagyva Start túl beállításakor {legjobb})</span><span class="sxs-lookup"><span data-stu-id="3b473-134">Additionally, Start also supports a special Macro:{Best}, which attempts toodetermine hello first “interesting” frame of hello content NOTE: (Step and Range are ignored when Start is set too{Best})</span></span>
  * <span data-ttu-id="3b473-135">Alapértelmezett: Start: {legjobb}</span><span class="sxs-lookup"><span data-stu-id="3b473-135">Defaults: Start:{Best}</span></span>
* <span data-ttu-id="3b473-136">Kimeneti formátumot kell minden egyes képformátum explicit módon megadott toobe: Jpg vagy Png/BmpFormat.</span><span class="sxs-lookup"><span data-stu-id="3b473-136">Output format needs toobe explicitly provided for each Image format: Jpg/Png/BmpFormat.</span></span> <span data-ttu-id="3b473-137">Ha létezik, a MES és így tovább JpgVideo tooJpgFormat megegyezik.</span><span class="sxs-lookup"><span data-stu-id="3b473-137">When present, MES matches JpgVideo tooJpgFormat and so on.</span></span> <span data-ttu-id="3b473-138">OutputFormat bevezet egy új lemezkép-kodek adott makró: {Index}, amely kell toobe jelen (egyszer és csak egyszer) kép kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="3b473-138">OutputFormat introduces a new image-codec specific Macro: {Index}, which needs toobe present (once and only once) for image output formats.</span></span>

## <span data-ttu-id="3b473-139"><a id="trim_video"></a>Trim (vágás) videó</span><span class="sxs-lookup"><span data-stu-id="3b473-139"><a id="trim_video"></a>Trim a video (clipping)</span></span>
<span data-ttu-id="3b473-140">Ez a szakasz megbeszélések hello kódoló módosításával kapcsolatos tooclip készletet, vagy trim hello bemeneti videó hello bemenet esetén egy úgynevezett mezzazine-fájlt vagy a tárolt fájl.</span><span class="sxs-lookup"><span data-stu-id="3b473-140">This section talks about modifying hello encoder presets tooclip or trim hello input video where hello input is a so-called mezzanine file or on-demand file.</span></span> <span data-ttu-id="3b473-141">hello kódoló is lehet használt tooclip vagy egy eszköz, ez rögzített vagy egy élő adatfolyam archivált trim – hello részleteket a [ebben a blogban](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span><span class="sxs-lookup"><span data-stu-id="3b473-141">hello encoder can also be used tooclip or trim an asset, which is captured or archived from a live stream – hello details for this are available in [this blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).</span></span>

<span data-ttu-id="3b473-142">tootrim a videók készíthet bármelyik hello MES készletek részletes ismertetését lásd [ez](media-services-mes-presets-overview.md) szakaszt, és módosítsa a hello **források** elem (mivel lásd alább).</span><span class="sxs-lookup"><span data-stu-id="3b473-142">tootrim your videos, you can take any of hello MES presets documented [this](media-services-mes-presets-overview.md) section and modify hello **Sources** element (as shown below).</span></span> <span data-ttu-id="3b473-143">StartTime hello értékének kell toomatch hello abszolút időbélyegeket hello bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="3b473-143">hello value of StartTime needs toomatch hello absolute timestamps of hello input video.</span></span> <span data-ttu-id="3b473-144">Például ha hello hello bemeneti videó első keret 12:00:10.000 időbélyeg, akkor az StartTime kell lennie, mint 12:00:10.000 és nagyobb.</span><span class="sxs-lookup"><span data-stu-id="3b473-144">For example, if hello first frame of hello input video has a timestamp of 12:00:10.000, then StartTime should be at least 12:00:10.000 and greater.</span></span> <span data-ttu-id="3b473-145">Hello az alábbi példában feltételezzük, hogy rendelkezik-e nulla kezdési időbélyeg hello bemeneti videóhoz.</span><span class="sxs-lookup"><span data-stu-id="3b473-145">In hello example below, we assume that hello input video has a starting timestamp of zero.</span></span> <span data-ttu-id="3b473-146">**Adatforrások** hello előre definiált hello elején kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="3b473-146">**Sources** should be placed at hello beginning of hello preset.</span></span>

### <span data-ttu-id="3b473-147"><a id="json"></a>JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-147"><a id="json"></a>JSON preset</span></span>
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

### <a name="xml-preset"></a><span data-ttu-id="3b473-148">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-148">XML preset</span></span>
<span data-ttu-id="3b473-149">tootrim a videók készíthet hello MES készletek dokumentált bármelyikét [Itt](media-services-mes-presets-overview.md) , és módosítsa a hello **források** eleme (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="3b473-149">tootrim your videos, you can take any of hello MES presets documented [here](media-services-mes-presets-overview.md) and modify hello **Sources** element (as shown below).</span></span>

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

## <span data-ttu-id="3b473-150"><a id="overlay"></a>Hozzon létre egy átmeneti területre</span><span class="sxs-lookup"><span data-stu-id="3b473-150"><a id="overlay"></a>Create an overlay</span></span>

<span data-ttu-id="3b473-151">Media Encoder Standard hello lehetővé teszi egy meglévő video egy képet toooverlay.</span><span class="sxs-lookup"><span data-stu-id="3b473-151">hello Media Encoder Standard allows you toooverlay an image onto an existing video.</span></span> <span data-ttu-id="3b473-152">Jelenleg a következő formátumok hello támogatottak: png, jpg, gif, bmp és.</span><span class="sxs-lookup"><span data-stu-id="3b473-152">Currently, hello following formats are supported: png, jpg, gif, and bmp.</span></span> <span data-ttu-id="3b473-153">hello alább megadott készlet példája egy alapszintű egy videófelirat.</span><span class="sxs-lookup"><span data-stu-id="3b473-153">hello preset defined below is a basic example  of a video overlay.</span></span>

<span data-ttu-id="3b473-154">Továbbá toodefining egy előre definiált fájlt, is toolet Media Services tudja, melyik hello eszköz fájlban hello átfedő lemezképet, és mely fájl hello forrás videó, amelyre kívánja toooverlay hello lemezképet.</span><span class="sxs-lookup"><span data-stu-id="3b473-154">In addition toodefining a preset file, you also have toolet Media Services know which file in hello asset is hello overlay image and which file is hello source video onto which you want toooverlay hello image.</span></span> <span data-ttu-id="3b473-155">hello videofájl rendelkezik toobe hello **elsődleges** fájlt.</span><span class="sxs-lookup"><span data-stu-id="3b473-155">hello video file has toobe hello **primary** file.</span></span>

<span data-ttu-id="3b473-156">Ha .NET használ, adja hozzá a két funkciók definiált toohello .NET típusú példát a következő hello [ez](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) témakör.</span><span class="sxs-lookup"><span data-stu-id="3b473-156">If you are using .NET, add hello following two functions toohello .NET example defined in [this](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic.</span></span> <span data-ttu-id="3b473-157">Hello **UploadMediaFilesFromFolder** függvény fel a fájlokat egy mappából (például BigBuckBunny.mp4 és Image001.png) és a készletek hello mp4 toobe hello elsődleges fájl hello eszközt.</span><span class="sxs-lookup"><span data-stu-id="3b473-157">hello **UploadMediaFilesFromFolder** function uploads files from a folder (for example, BigBuckBunny.mp4 and Image001.png) and sets hello mp4 file toobe hello primary file in hello asset.</span></span> <span data-ttu-id="3b473-158">Hello **EncodeWithOverlay** függvény hello egyéni előre definiált átadott fájl tooit használja (például hello készletet, hogy az alábbiak szerint) toocreate hello kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="3b473-158">hello **EncodeWithOverlay** function uses hello custom preset file that was passed tooit (for example, hello preset that follows) toocreate hello encoding task.</span></span>


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
> <span data-ttu-id="3b473-159">Aktuális korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="3b473-159">Current limitations:</span></span>
>
> <span data-ttu-id="3b473-160">hello átfedő átlátszatlanság beállítás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="3b473-160">hello overlay opacity setting is not supported.</span></span>
>
> <span data-ttu-id="3b473-161">A forrás videofájl és hello átfedő képfájl rendelkezik toobe a hello ugyanazon eszközre, és videofájl igények toobe set hello hello elsődleges fájlban található, ez az eszköz.</span><span class="sxs-lookup"><span data-stu-id="3b473-161">Your source video file and hello overlay image file have toobe in hello same asset, and hello video file needs toobe set as hello primary file in this asset.</span></span>
>
>

### <a name="json-preset"></a><span data-ttu-id="3b473-162">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-162">JSON preset</span></span>
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


### <a name="xml-preset"></a><span data-ttu-id="3b473-163">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-163">XML preset</span></span>
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


## <span data-ttu-id="3b473-164"><a id="silent_audio"></a>Csendes hang nyomon beszúrni, ha bemeneti nincsenek hang</span><span class="sxs-lookup"><span data-stu-id="3b473-164"><a id="silent_audio"></a>Insert a silent audio track when input has no audio</span></span>
<span data-ttu-id="3b473-165">Alapértelmezés szerint ha egy bemeneti toohello kódoló csak videó, és nincsenek hang tartalmazó küld majd hello kimeneti adategységen tartalmaz csak videó adatokat tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="3b473-165">By default, if you send an input toohello encoder that contains only video, and no audio, then hello output asset contains files that contain only video data.</span></span> <span data-ttu-id="3b473-166">Néhány lejátszó nem lehet ilyen toohandle kimenetét.</span><span class="sxs-lookup"><span data-stu-id="3b473-166">Some players may not be able toohandle such output streams.</span></span> <span data-ttu-id="3b473-167">Ez a beállítás tooforce hello kódoló tooadd egy csendes hang követése toohello kimeneti forgatókönyv használható.</span><span class="sxs-lookup"><span data-stu-id="3b473-167">You can use this setting tooforce hello encoder tooadd a silent audio track toohello output in that scenario.</span></span>

<span data-ttu-id="3b473-168">tooforce hello kódoló tooproduce Ha bemeneti nincsenek hang-, hang csendes nyomon tartalmazó hello "InsertSilenceIfNoAudio" értéket adjon meg.</span><span class="sxs-lookup"><span data-stu-id="3b473-168">tooforce hello encoder tooproduce an asset that contains a silent audio track when input has no audio, specify hello "InsertSilenceIfNoAudio" value.</span></span>

<span data-ttu-id="3b473-169">Bármelyik MES készletek részletes ismertetését lásd: hello eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és ellenőrizze hello módosítását követően:</span><span class="sxs-lookup"><span data-stu-id="3b473-169">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

### <a name="json-preset"></a><span data-ttu-id="3b473-170">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-170">JSON preset</span></span>
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a><span data-ttu-id="3b473-171">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-171">XML preset</span></span>
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <span data-ttu-id="3b473-172"><a id="deinterlacing"></a>Automatikus deszerializálni rövidebbnek letiltása</span><span class="sxs-lookup"><span data-stu-id="3b473-172"><a id="deinterlacing"></a>Disable auto de-interlacing</span></span>
<span data-ttu-id="3b473-173">Az ügyfelek nem kell toodo semmit, ha azok például hello váltott automatikusan deszerializálni váltakozó tartalma toobe.</span><span class="sxs-lookup"><span data-stu-id="3b473-173">Customers don’t need toodo anything if they like hello interlace contents toobe automatically de-interlaced.</span></span> <span data-ttu-id="3b473-174">Ha be van kapcsolva (alapértelmezés) hello MES hello hello automatikus deszerializálni rövidebbnek automatikus észlelési váltakozó keretek, és csak deszerializálni interlaces keretek váltakozó megjelölve.</span><span class="sxs-lookup"><span data-stu-id="3b473-174">When hello auto de-interlacing is on (default) hello MES does hello auto detection of interlaced frames and only de-interlaces frames marked as interlaced.</span></span>

<span data-ttu-id="3b473-175">Kikapcsolhatja hello automatikus deszerializálni rövidebbnek.</span><span class="sxs-lookup"><span data-stu-id="3b473-175">You can turn hello auto de-interlacing off.</span></span> <span data-ttu-id="3b473-176">Ez a lehetőség nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="3b473-176">This option is not recommended.</span></span>

### <a name="json-preset"></a><span data-ttu-id="3b473-177">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-177">JSON preset</span></span>
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a><span data-ttu-id="3b473-178">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-178">XML preset</span></span>
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <span data-ttu-id="3b473-179"><a id="audio_only"></a>Csak hang-készletek</span><span class="sxs-lookup"><span data-stu-id="3b473-179"><a id="audio_only"></a>Audio-only presets</span></span>
<span data-ttu-id="3b473-180">Ez a szakasz azt mutatja be két csak MES készletek: AAC hang- és AAC jó minőségű hang.</span><span class="sxs-lookup"><span data-stu-id="3b473-180">This section demonstrates two audio-only MES presets: AAC Audio and AAC Good Quality Audio.</span></span>

### <a name="aac-audio"></a><span data-ttu-id="3b473-181">AAC hang</span><span class="sxs-lookup"><span data-stu-id="3b473-181">AAC Audio</span></span>
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

### <a name="aac-good-quality-audio"></a><span data-ttu-id="3b473-182">AAC jó minőségű hang</span><span class="sxs-lookup"><span data-stu-id="3b473-182">AAC Good Quality Audio</span></span>
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

## <span data-ttu-id="3b473-183"><a id="concatenate"></a>ÖSSZEFŰZ két vagy több videofájlok</span><span class="sxs-lookup"><span data-stu-id="3b473-183"><a id="concatenate"></a>Concatenate two or more video files</span></span>

<span data-ttu-id="3b473-184">hello alábbi példa bemutatja, hogyan hozhat létre egy előre definiált tooconcatenate két vagy több videofájlok lejátszását.</span><span class="sxs-lookup"><span data-stu-id="3b473-184">hello following example illustrates how you can generate a preset tooconcatenate two or more video files.</span></span> <span data-ttu-id="3b473-185">hello leggyakoribb eset az, amikor tooadd fejléc vagy pótkocsi toohello fő videó.</span><span class="sxs-lookup"><span data-stu-id="3b473-185">hello most common scenario is when you want tooadd a header or a trailer toohello main video.</span></span> <span data-ttu-id="3b473-186">hello szánt együtt szerkesztett hello videofájlok megosztás tulajdonságai (képernyőfelbontást képkockasebessége, zenei száma, stb.) használata esetén.</span><span class="sxs-lookup"><span data-stu-id="3b473-186">hello intended use is when hello video files being edited together share  properties (video resolution, frame rate, audio track count, etc.).</span></span> <span data-ttu-id="3b473-187">Meg nem toomix videók különböző keret díjszabás, vagy eltérő számú zeneszámok gondoskodunk.</span><span class="sxs-lookup"><span data-stu-id="3b473-187">You should take care not toomix videos of different frame rates, or with different number of audio tracks.</span></span>

>[!NOTE]
><span data-ttu-id="3b473-188">hello hello összefűzése funkció aktuális kialakítása vár, hogy hello bemeneti videó videóklipeket konzisztensek tekintetében megoldást képkockasebessége stb.</span><span class="sxs-lookup"><span data-stu-id="3b473-188">hello current design of hello concatenation feature expects that hello input video clips are consistent in terms of resolution, frame rate etc.</span></span> 

### <a name="requirements-and-considerations"></a><span data-ttu-id="3b473-189">Követelmények és szempontok</span><span class="sxs-lookup"><span data-stu-id="3b473-189">Requirements and considerations</span></span>

* <span data-ttu-id="3b473-190">A bemeneti videó csak kell rendelkeznie egy hang követése.</span><span class="sxs-lookup"><span data-stu-id="3b473-190">Input videos should only have one audio track.</span></span>
* <span data-ttu-id="3b473-191">Bemeneti videók kell összes hello azonos képkockasebessége.</span><span class="sxs-lookup"><span data-stu-id="3b473-191">Input videos should all have hello same frame rate.</span></span>
* <span data-ttu-id="3b473-192">Kell tölteni a videók a különböző eszközök, és hello videók állítja be hello elsődleges fájl az egyes eszközökre.</span><span class="sxs-lookup"><span data-stu-id="3b473-192">You must upload your videos into separate assets and set hello videos as hello primary file in each asset.</span></span>
* <span data-ttu-id="3b473-193">A videók tooknow hello időtartama van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3b473-193">You need tooknow hello duration of your videos.</span></span>
* <span data-ttu-id="3b473-194">hello előre definiált az alábbi példák azt feltételezi, hogy minden hello bemeneti videók indítsa el az időbélyegzőnek a nulla.</span><span class="sxs-lookup"><span data-stu-id="3b473-194">hello preset examples below assumes that all hello input videos start with a timestamp of zero.</span></span> <span data-ttu-id="3b473-195">Szüksége toomodify hello StartTime értékekre Ha hello videók különböző kezdési Timestamp típusú, mint általában hello élő archívummal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="3b473-195">You need toomodify hello StartTime values if hello videos have different starting timestamp, as is typically hello case with live archives.</span></span>
* <span data-ttu-id="3b473-196">hello JSON előre definiált teszi explicit hivatkozásokat hello bemeneti eszközök toohello AssetID értékét.</span><span class="sxs-lookup"><span data-stu-id="3b473-196">hello JSON preset makes explicit references toohello AssetID values of hello input assets.</span></span>
* <span data-ttu-id="3b473-197">hello példakód azt feltételezi, hogy a JSON-készlet hello tooa helyi fájl, például "C:\supportFiles\preset.json" mentése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="3b473-197">hello sample code assumes that hello JSON preset has been saved tooa local file, such as "C:\supportFiles\preset.json".</span></span> <span data-ttu-id="3b473-198">Azt is feltételezi, hogy létrejöttek-e a két eszközök két videofájlok feltöltésével, és arról, hogy hello eredő AssetID értékek.</span><span class="sxs-lookup"><span data-stu-id="3b473-198">It also assumes that two assets have been created by uploading two video files, and that you know hello resultant AssetID values.</span></span>
* <span data-ttu-id="3b473-199">hello kódrészletet és JSON készletet két videofájlok hozzáfűzésével példáját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="3b473-199">hello code snippet and JSON preset shows an example of concatenating two video files.</span></span> <span data-ttu-id="3b473-200">Kiterjesztheti úgy, mint két videók által toomore:</span><span class="sxs-lookup"><span data-stu-id="3b473-200">You can extend it toomore than two videos by:</span></span>

  1. <span data-ttu-id="3b473-201">Hívása a feladatot. InputAssets.Add() ismételten tooadd további videók, sorrendben.</span><span class="sxs-lookup"><span data-stu-id="3b473-201">Calling task.InputAssets.Add() repeatedly tooadd more videos, in order.</span></span>
  2. <span data-ttu-id="3b473-202">Hello több bejegyzés hozzáadásával toohello "források" elemének hello JSON, így a megfelelő szerkesztése ugyanabban a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="3b473-202">Making corresponding edits toohello "Sources" element in hello JSON, by adding more entries, in hello same order.</span></span>

### <a name="net-code"></a><span data-ttu-id="3b473-203">.NET-kódot</span><span class="sxs-lookup"><span data-stu-id="3b473-203">.NET code</span></span>

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

### <a name="json-preset"></a><span data-ttu-id="3b473-204">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-204">JSON preset</span></span>

<span data-ttu-id="3b473-205">Az azonosítók, amelyet az tooconcatenate hello eszközök, és hello megfelelő idő szegmens minden egyes videó az egyéni frissítés.</span><span class="sxs-lookup"><span data-stu-id="3b473-205">Update your custom preset with ids of hello assets that you want tooconcatenate, and with hello appropriate time segment for each video.</span></span>

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

## <span data-ttu-id="3b473-206"><a id="crop"></a>A Media Encoder Standard körülvágása videók</span><span class="sxs-lookup"><span data-stu-id="3b473-206"><a id="crop"></a>Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="3b473-207">Lásd: hello [a Media Encoder Standard videók körülvágása](media-services-crop-video.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="3b473-207">See hello [Crop videos with Media Encoder Standard](media-services-crop-video.md) topic.</span></span>

## <span data-ttu-id="3b473-208"><a id="no_video"></a>Videó nyomon beszúrni, ha bemeneti nincs kép</span><span class="sxs-lookup"><span data-stu-id="3b473-208"><a id="no_video"></a>Insert a video track when input has no video</span></span>

<span data-ttu-id="3b473-209">Alapértelmezés szerint ha egy bemeneti toohello kódoló csak hang, és a nem kép tartalmazó küldi el majd hello kimeneti adategységen tartalmaz csak hang adatokat tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="3b473-209">By default, if you send an input toohello encoder that contains only audio, and no video, then hello output asset contains files that contain only audio data.</span></span> <span data-ttu-id="3b473-210">Néhány szereplő, köztük az Azure Media Player (lásd: [ez](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) nem lehet képes toohandle ilyen adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="3b473-210">Some players, including Azure Media Player (see [this](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) may not be able toohandle such streams.</span></span> <span data-ttu-id="3b473-211">Használhatja ezt a beállítást tooforce hello kódoló tooadd egy fekete-fehér videó követése toohello kimeneti forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="3b473-211">You can use this setting tooforce hello encoder tooadd a monochrome video track toohello output in that scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="3b473-212">Kényszerített hello kódoló tooinsert egy kimeneti videó követése növekszik hello méretét hello kimeneti eszköz, és ezáltal a kódolási feladat hello költsége hello.</span><span class="sxs-lookup"><span data-stu-id="3b473-212">Forcing hello encoder tooinsert an output video track increases hello size of hello output Asset, and thereby hello cost incurred for hello encoding Task.</span></span> <span data-ttu-id="3b473-213">Végezzen tesztek tooverify, amelyek az eredő növelését csak mérsékelt hatása van a havi költségeket.</span><span class="sxs-lookup"><span data-stu-id="3b473-213">You should run tests tooverify that this resultant increase has only a modest impact on your monthly charges.</span></span>
>

### <a name="inserting-video-at-only-hello-lowest-bitrate"></a><span data-ttu-id="3b473-214">Videó: csak hello legalacsonyabb sávszélességű beszúrása</span><span class="sxs-lookup"><span data-stu-id="3b473-214">Inserting video at only hello lowest bitrate</span></span>

<span data-ttu-id="3b473-215">Tegyük fel, amelyek több sávszélességű kódolás használatával, mint a beállított ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode a teljes videofájlok lejátszását, és csak hang-fájlokat tartalmazó katalógus az adatfolyamként történő, adjon meg.</span><span class="sxs-lookup"><span data-stu-id="3b473-215">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="3b473-216">Ebben az esetben ha hello bemeneti nincs videó-érdemes lehet tooforce hello kódoló tooinsert megakadályozását tooinserting videó minden kimeneti sávszélességű, mint a csak hello legalacsonyabb sávszélességű, nyomon egy videó egyszínűen.</span><span class="sxs-lookup"><span data-stu-id="3b473-216">In this scenario, when hello input has no video, you may want tooforce hello encoder tooinsert a monochrome video track at just hello lowest bitrate, as opposed tooinserting video at every output bitrate.</span></span> <span data-ttu-id="3b473-217">tooachieve, toouse hello kell **InsertBlackIfNoVideoBottomLayerOnly** jelzőt.</span><span class="sxs-lookup"><span data-stu-id="3b473-217">tooachieve this, you need toouse hello **InsertBlackIfNoVideoBottomLayerOnly** flag.</span></span>

<span data-ttu-id="3b473-218">Bármelyik MES készletek részletes ismertetését lásd: hello eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és ellenőrizze hello módosítását követően:</span><span class="sxs-lookup"><span data-stu-id="3b473-218">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="3b473-219">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-219">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="3b473-220">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-220">XML preset</span></span>

<span data-ttu-id="3b473-221">XML használatakor használjon feltétel = "InsertBlackIfNoVideoBottomLayerOnly" egy attribútum toohello **H264Video** elem és a feltétel túl = a "InsertSilenceIfNoAudio" attribútumaként**AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="3b473-221">When using XML, use Condition="InsertBlackIfNoVideoBottomLayerOnly" as an attribute toohello **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute too**AACAudio**.</span></span>

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

### <a name="inserting-video-at-all-output-bitrates"></a><span data-ttu-id="3b473-222">A kimeneti bitrates videó minden beszúrása</span><span class="sxs-lookup"><span data-stu-id="3b473-222">Inserting video at all output bitrates</span></span>
<span data-ttu-id="3b473-223">Tegyük fel, amelyek több sávszélességű kódolás használatával, mint a beállított ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode a teljes videofájlok lejátszását, és csak hang-fájlokat tartalmazó katalógus az adatfolyamként történő, adjon meg.</span><span class="sxs-lookup"><span data-stu-id="3b473-223">Suppose you are using a multiple bitrate encoding preset such as ["H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode your entire input catalog for streaming, which contains a mix of video files and audio-only files.</span></span> <span data-ttu-id="3b473-224">Ebben az esetben ha hello bemeneti nincs videó-érdemes lehet tooforce hello kódoló tooinsert fekete-fehér videó: az összes hello kimeneti bitrates nyomon.</span><span class="sxs-lookup"><span data-stu-id="3b473-224">In this scenario, when hello input has no video, you may want tooforce hello encoder tooinsert a monochrome video track at all hello output bitrates.</span></span> <span data-ttu-id="3b473-225">Ez biztosítja, hogy a kimeneti összes homogén tekintetben toonumber videó nyomon követi és zeneszámok az olyan eszközök.</span><span class="sxs-lookup"><span data-stu-id="3b473-225">This ensures that your output Assets are all homogenous with respect toonumber of video tracks and audio tracks.</span></span> <span data-ttu-id="3b473-226">tooachieve, toospecify hello "InsertBlackIfNoVideo" jelző van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3b473-226">tooachieve this, you need toospecify hello "InsertBlackIfNoVideo" flag.</span></span>

<span data-ttu-id="3b473-227">Bármelyik MES készletek részletes ismertetését lásd: hello eltarthat [ez](media-services-mes-presets-overview.md) szakaszt, és ellenőrizze hello módosítását követően:</span><span class="sxs-lookup"><span data-stu-id="3b473-227">You can take any of hello MES presets documented in [this](media-services-mes-presets-overview.md) section, and make hello following modification:</span></span>

#### <a name="json-preset"></a><span data-ttu-id="3b473-228">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-228">JSON preset</span></span>
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a><span data-ttu-id="3b473-229">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-229">XML preset</span></span>

<span data-ttu-id="3b473-230">XML használatakor használjon feltétel = "InsertBlackIfNoVideo" egy attribútum toohello **H264Video** elem és a feltétel túl = a "InsertSilenceIfNoAudio" attribútumaként**AACAudio**.</span><span class="sxs-lookup"><span data-stu-id="3b473-230">When using XML, use Condition="InsertBlackIfNoVideo" as an attribute toohello **H264Video** element and  Condition="InsertSilenceIfNoAudio" as an attribute too**AACAudio**.</span></span>

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

## <span data-ttu-id="3b473-231"><a id="rotate_video"></a>Videó elforgatása</span><span class="sxs-lookup"><span data-stu-id="3b473-231"><a id="rotate_video"></a>Rotate a video</span></span>
<span data-ttu-id="3b473-232">Hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) elforgatási szög által 0/90/180 vagy 270 támogatja.</span><span class="sxs-lookup"><span data-stu-id="3b473-232">hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 0/90/180/270.</span></span> <span data-ttu-id="3b473-233">hello alapértelmezés lesz az "Auto", ha megpróbál toodetect hello Elforgatás metaadatok hello bejövő videofájl és kártalanítja azt.</span><span class="sxs-lookup"><span data-stu-id="3b473-233">hello default behavior is "Auto", where it tries toodetect hello rotation metadata in hello incoming video file and compensate for it.</span></span> <span data-ttu-id="3b473-234">Adja meg a következőket hello **források** elem tooone definiált hello készleteket [ez](media-services-mes-presets-overview.md) szakasz:</span><span class="sxs-lookup"><span data-stu-id="3b473-234">Include hello following **Sources** element tooone of hello presets defined in [this](media-services-mes-presets-overview.md) section:</span></span>

### <a name="json-preset"></a><span data-ttu-id="3b473-235">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-235">JSON preset</span></span>
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
### <a name="xml-preset"></a><span data-ttu-id="3b473-236">XML-készlet</span><span class="sxs-lookup"><span data-stu-id="3b473-236">XML preset</span></span>
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

<span data-ttu-id="3b473-237">Lásd még [ez](media-services-mes-schema.md#PreserveResolutionAfterRotation) hello kódoló értelmezése hello szélességének és magasságának beállítások hello további információt a témakör az adott néven beállítás, elforgatás kompenzációs indítását.</span><span class="sxs-lookup"><span data-stu-id="3b473-237">Also, see [this](media-services-mes-schema.md#PreserveResolutionAfterRotation) topic for more information on how hello encoder interprets hello Width and Height settings in hello preset, when rotation compensation is triggered.</span></span>

<span data-ttu-id="3b473-238">Hello érték "0" tooindicate toohello kódoló tooignore Elforgatás metaadatok, ha jelen van, a bemeneti videó hello használhatja.</span><span class="sxs-lookup"><span data-stu-id="3b473-238">You can use hello value "0" tooindicate toohello encoder tooignore rotation metadata, if present, in hello input video.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="3b473-239">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3b473-239">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3b473-240">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3b473-240">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="3b473-241">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3b473-241">See Also</span></span>
[<span data-ttu-id="3b473-242">Media Services kódolási áttekintése</span><span class="sxs-lookup"><span data-stu-id="3b473-242">Media Services Encoding Overview</span></span>](media-services-encode-asset.md)

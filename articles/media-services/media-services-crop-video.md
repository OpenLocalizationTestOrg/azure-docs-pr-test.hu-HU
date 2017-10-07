---
title: "a Media Encoder Standard - Azure aaaHow toocrop videók |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan a Media Encoder Standard toocrop videók."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 7628f674-2005-4531-8b61-d7a4f53e46ba
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/09/2017
ms.author: anilmur;juliako;
ms.openlocfilehash: 2b4ac3d96228b93c890a38c57c4913988de1e8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="crop-videos-with-media-encoder-standard"></a><span data-ttu-id="f2b68-103">Videók körülvágása a Media Encoder Standarddel</span><span class="sxs-lookup"><span data-stu-id="f2b68-103">Crop videos with Media Encoder Standard</span></span>
<span data-ttu-id="f2b68-104">Media Encoder Standard (MES) toocrop használhatja a bemeneti videóhoz.</span><span class="sxs-lookup"><span data-stu-id="f2b68-104">You can use Media Encoder Standard (MES) toocrop your input video.</span></span> <span data-ttu-id="f2b68-105">Vágás rendszer hello hello videó keretbe téglalap alakú ablak kiválasztásával, és csak az adott időszakban hello képpont kódolást.</span><span class="sxs-lookup"><span data-stu-id="f2b68-105">Cropping is hello process of selecting a rectangular window within hello video frame, and encoding just hello pixels within that window.</span></span> <span data-ttu-id="f2b68-106">a következő diagram hello segítségével hello folyamatot szemlélteti.</span><span class="sxs-lookup"><span data-stu-id="f2b68-106">hello following diagram helps illustrate hello process.</span></span>

![Körülvágása videó](./media/media-services-crop-video/media-services-crop-video01.png)

<span data-ttu-id="f2b68-108">Tegyük fel, bemeneti videó felbontása 1920 x 1080 képpont (16:9 oldalarány), de rendelkezik fekete sávok (pillar mezőkbe) hello bal és jobb oldali, hogy csak egy 4:3 ablakot vagy 1440 x 1080 képpont aktív videót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f2b68-108">Suppose you have as input a video that has a resolution of 1920x1080 pixels (16:9 aspect ratio), but has black bars (pillar boxes) at hello left and right, so that only a 4:3 window or 1440x1080 pixels contains active video.</span></span> <span data-ttu-id="f2b68-109">Használjon MES toocrop vagy fekete hello sávok kimenő, és hello 1440 x 1080 régió kódolása.</span><span class="sxs-lookup"><span data-stu-id="f2b68-109">You can use MES toocrop or edit out hello black bars, and encode hello 1440x1080 region.</span></span>

<span data-ttu-id="f2b68-110">Előre feldolgozásra szakasza alatt a MES vágás, így a hello vágási paraméterek a hello kódolási beállításkészlet toohello eredeti bemeneti videó alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="f2b68-110">Cropping in MES is a pre-processing stage, so hello cropping parameters in hello encoding preset apply toohello original input video.</span></span> <span data-ttu-id="f2b68-111">Kódolás későbbi, és hello szélességének és magasságának beállítások alkalmaznak toohello *előre feldolgozott* video-, és nem toohello eredeti videó.</span><span class="sxs-lookup"><span data-stu-id="f2b68-111">Encoding is a subsequent stage, and hello width/height settings apply toohello *pre-processed* video, and not toohello original video.</span></span> <span data-ttu-id="f2b68-112">Az előre definiált tervezésekor toodo hello következőkre lesz szüksége: (a) hello körülvágása paraméterek alapján hello eredeti bemeneti videó (b) válassza ki és a beállítások alapján levágja videó hello kódolása.</span><span class="sxs-lookup"><span data-stu-id="f2b68-112">When designing your preset you need toodo hello following: (a) select hello crop parameters based on hello original input video, and (b) select your encode settings based on hello cropped video.</span></span> <span data-ttu-id="f2b68-113">Ha nem egyeznek a beállítások toohello levágja videó kódolására, hello kimeneti nem lesz a várt módon.</span><span class="sxs-lookup"><span data-stu-id="f2b68-113">If you do not match your encode settings toohello cropped video, hello output will not be as you expect.</span></span>

<span data-ttu-id="f2b68-114">Hello [következő](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) a témakör bemutatja, hogyan toocreate MES egy kódolási feladat, és hogyan toospecify egyéni beállításkészlet hello kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="f2b68-114">hello [following](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) topic shows how toocreate an encoding job with MES and how toospecify a custom preset for hello encoding task.</span></span> 

## <a name="creating-a-custom-preset"></a><span data-ttu-id="f2b68-115">Egy készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2b68-115">Creating a custom preset</span></span>
<span data-ttu-id="f2b68-116">Hello példában hello ábrán is látható:</span><span class="sxs-lookup"><span data-stu-id="f2b68-116">In hello example shown in hello diagram:</span></span>

1. <span data-ttu-id="f2b68-117">Eredeti bemeneti érték 1920 x 1080</span><span class="sxs-lookup"><span data-stu-id="f2b68-117">Original input is 1920x1080</span></span>
2. <span data-ttu-id="f2b68-118">Hello bemeneti keretében középre 1440 x 1080 tooan kimenete levágja toobe kell</span><span class="sxs-lookup"><span data-stu-id="f2b68-118">It needs toobe cropped tooan output of 1440x1080, which is centered in hello input frame</span></span>
3. <span data-ttu-id="f2b68-119">Ez azt jelenti, hogy egy X eltolását (1920 – 1440) / 2 = 240, és egy Y eltolás: 0</span><span class="sxs-lookup"><span data-stu-id="f2b68-119">This means an X offset of (1920 – 1440)/2 = 240, and a Y offset of zero</span></span>
4. <span data-ttu-id="f2b68-120">hello szélessége és magassága hello körülvágása négyszög: 1440 és 1080, illetve</span><span class="sxs-lookup"><span data-stu-id="f2b68-120">hello Width and Height of hello Crop rectangle are 1440 and 1080, respectively</span></span>
5. <span data-ttu-id="f2b68-121">A hello kódolása lépésként hello kérdezze tooproduce három rétegek, a megoldások 1440 x 1080, 960 x 720 és 480 x 360, illetve</span><span class="sxs-lookup"><span data-stu-id="f2b68-121">In hello encode stage, hello ask is tooproduce three layers, are resolutions 1440x1080, 960x720 and 480x360, respectively</span></span>

### <a name="json-preset"></a><span data-ttu-id="f2b68-122">JSON-készlet</span><span class="sxs-lookup"><span data-stu-id="f2b68-122">JSON preset</span></span>
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
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
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
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
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
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


## <a name="restrictions-on-cropping"></a><span data-ttu-id="f2b68-123">Vágás korlátozásai</span><span class="sxs-lookup"><span data-stu-id="f2b68-123">Restrictions on cropping</span></span>
<span data-ttu-id="f2b68-124">hello szolgáltatás vágás toobe manuális célja.</span><span class="sxs-lookup"><span data-stu-id="f2b68-124">hello cropping feature is meant toobe manual.</span></span> <span data-ttu-id="f2b68-125">Kellene tooload a bemeneti videó olyan megfelelő szerkesztési eszközt, amely lehetővé teszi, hogy válassza ki az egyik fontos keretek, hello kurzor toodetermine eltolások hello vágás téglalap pozíciója, toodetermine hello kódolási beállításkészlet, amely van beállítva, az adott adott videó, stb. Ez a funkció nem böngészésre tooenable többek között: automatikus észlelése és eltávolítása a bemeneti videóhoz fekete postaláda/pillarbox szegélyek.</span><span class="sxs-lookup"><span data-stu-id="f2b68-125">You would need tooload your input video into a suitable editing tool that lets you select frames of interest, position hello cursor toodetermine offsets for hello cropping rectangle, toodetermine hello encoding preset that is tuned for that particular video, etc. This feature is not meant tooenable things like: automatic detection and removal of black letterbox/pillarbox borders in your input video.</span></span>

<span data-ttu-id="f2b68-126">Korlátozásokkal szolgáltatás vágás toohello alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="f2b68-126">Following constraints apply toohello cropping feature.</span></span> <span data-ttu-id="f2b68-127">Ha ezek nem teljesülnek, hello kódolása feladat is sikertelen, vagy egy nem várt eredménye.</span><span class="sxs-lookup"><span data-stu-id="f2b68-127">If these are not met, hello encode Task can fail, or produce an unexpected output.</span></span>

1. <span data-ttu-id="f2b68-128">hello fordítani száma és mérete hello körülvágása téglalap rendelkezik toofit belül hello bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="f2b68-128">hello co-ordinates and size of hello Crop rectangle have toofit within hello input video</span></span>
2. <span data-ttu-id="f2b68-129">Fent említett hello szélesség & hello magasságának kódolása beállítások levágja videó toocorrespond toohello rendelkezik</span><span class="sxs-lookup"><span data-stu-id="f2b68-129">As mentioned above, hello Width & Height in hello encode settings have toocorrespond toohello cropped video</span></span>
3. <span data-ttu-id="f2b68-130">A fekvő tájolás (azaz nem alkalmazható toovideos rögzített függőleges tárolt okostelefont vagy álló módban) rögzített toovideos vágás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="f2b68-130">Cropping applies toovideos captured in landscape mode (i.e. not applicable toovideos recorded with a smartphone held vertically or in portrait mode)</span></span>
4. <span data-ttu-id="f2b68-131">Működik a legjobban a négyzetes képpont rögzített fokozatos videó</span><span class="sxs-lookup"><span data-stu-id="f2b68-131">Works best with progressive video captured with square pixels</span></span>

## <a name="provide-feedback"></a><span data-ttu-id="f2b68-132">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="f2b68-132">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a><span data-ttu-id="f2b68-133">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="f2b68-133">Next step</span></span>
<span data-ttu-id="f2b68-134">Tekintse meg az Azure Media Services képzési elérési utak toohelp megismerkedhet a AMS kiváló szolgáltatásaira.</span><span class="sxs-lookup"><span data-stu-id="f2b68-134">See Azure Media Services learning paths toohelp you learn about great features offered by AMS.</span></span>  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

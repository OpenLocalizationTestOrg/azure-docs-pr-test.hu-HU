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
# <a name="crop-videos-with-media-encoder-standard"></a>Videók körülvágása a Media Encoder Standarddel
Media Encoder Standard (MES) toocrop használhatja a bemeneti videóhoz. Vágás rendszer hello hello videó keretbe téglalap alakú ablak kiválasztásával, és csak az adott időszakban hello képpont kódolást. a következő diagram hello segítségével hello folyamatot szemlélteti.

![Körülvágása videó](./media/media-services-crop-video/media-services-crop-video01.png)

Tegyük fel, bemeneti videó felbontása 1920 x 1080 képpont (16:9 oldalarány), de rendelkezik fekete sávok (pillar mezőkbe) hello bal és jobb oldali, hogy csak egy 4:3 ablakot vagy 1440 x 1080 képpont aktív videót tartalmaz. Használjon MES toocrop vagy fekete hello sávok kimenő, és hello 1440 x 1080 régió kódolása.

Előre feldolgozásra szakasza alatt a MES vágás, így a hello vágási paraméterek a hello kódolási beállításkészlet toohello eredeti bemeneti videó alkalmazni. Kódolás későbbi, és hello szélességének és magasságának beállítások alkalmaznak toohello *előre feldolgozott* video-, és nem toohello eredeti videó. Az előre definiált tervezésekor toodo hello következőkre lesz szüksége: (a) hello körülvágása paraméterek alapján hello eredeti bemeneti videó (b) válassza ki és a beállítások alapján levágja videó hello kódolása. Ha nem egyeznek a beállítások toohello levágja videó kódolására, hello kimeneti nem lesz a várt módon.

Hello [következő](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) a témakör bemutatja, hogyan toocreate MES egy kódolási feladat, és hogyan toospecify egyéni beállításkészlet hello kódolási feladat. 

## <a name="creating-a-custom-preset"></a>Egy készlet létrehozása
Hello példában hello ábrán is látható:

1. Eredeti bemeneti érték 1920 x 1080
2. Hello bemeneti keretében középre 1440 x 1080 tooan kimenete levágja toobe kell
3. Ez azt jelenti, hogy egy X eltolását (1920 – 1440) / 2 = 240, és egy Y eltolás: 0
4. hello szélessége és magassága hello körülvágása négyszög: 1440 és 1080, illetve
5. A hello kódolása lépésként hello kérdezze tooproduce három rétegek, a megoldások 1440 x 1080, 960 x 720 és 480 x 360, illetve

### <a name="json-preset"></a>JSON-készlet
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


## <a name="restrictions-on-cropping"></a>Vágás korlátozásai
hello szolgáltatás vágás toobe manuális célja. Kellene tooload a bemeneti videó olyan megfelelő szerkesztési eszközt, amely lehetővé teszi, hogy válassza ki az egyik fontos keretek, hello kurzor toodetermine eltolások hello vágás téglalap pozíciója, toodetermine hello kódolási beállításkészlet, amely van beállítva, az adott adott videó, stb. Ez a funkció nem böngészésre tooenable többek között: automatikus észlelése és eltávolítása a bemeneti videóhoz fekete postaláda/pillarbox szegélyek.

Korlátozásokkal szolgáltatás vágás toohello alkalmazni. Ha ezek nem teljesülnek, hello kódolása feladat is sikertelen, vagy egy nem várt eredménye.

1. hello fordítani száma és mérete hello körülvágása téglalap rendelkezik toofit belül hello bemeneti videó
2. Fent említett hello szélesség & hello magasságának kódolása beállítások levágja videó toocorrespond toohello rendelkezik
3. A fekvő tájolás (azaz nem alkalmazható toovideos rögzített függőleges tárolt okostelefont vagy álló módban) rögzített toovideos vágás vonatkozik.
4. Működik a legjobban a négyzetes képpont rögzített fokozatos videó

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Következő lépés
Tekintse meg az Azure Media Services képzési elérési utak toohelp megismerkedhet a AMS kiváló szolgáltatásaira.  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

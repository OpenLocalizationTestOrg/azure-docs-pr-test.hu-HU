---
title: "aaaH264 Multiple Bitrate 4 KB-os Media Encoder Standard előre definiált - Azure |} Microsoft Docs"
description: "hello a témakör áttekintést hello ** H264 Multiple Bitrate 4 KB -os ** feladat készletet."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: aba8e29e-d145-4f7b-814f-405f9c2a183b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: e22e0bd3bb110f54f7d624e099b5e34e8d4820a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="h264-multiple-bitrate-4k"></a><span data-ttu-id="ce738-103">H264 Multiple Bitrate 4 KB-os</span><span class="sxs-lookup"><span data-stu-id="ce738-103">H264 Multiple Bitrate 4K</span></span>
<span data-ttu-id="ce738-104">`Media Encoder Standard`Meghatározza a kódolási készletek kódolási feladatok létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="ce738-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="ce738-105">Használhatja a `preset name` mely formátumú fájlba szeretné tooencode toospecify a médiafájl.</span><span class="sxs-lookup"><span data-stu-id="ce738-105">You can either use a `preset name` toospecify into which format you would like tooencode your media file.</span></span> <span data-ttu-id="ce738-106">Másik lehetőségként létrehozhat saját JSON- vagy XML-alapú készletek (UTF-8 vagy UTF-16 kódolás használatával.</span><span class="sxs-lookup"><span data-stu-id="ce738-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="ce738-107">Majd át kellene hello egyéni előre definiált toohello kódoló.</span><span class="sxs-lookup"><span data-stu-id="ce738-107">You would then pass hello custom preset toohello encoder.</span></span> <span data-ttu-id="ce738-108">Minden hello hello listáját az adott néven beállítás által támogatott nevek `Media Encoder Standard` kódoló, lásd: [feladat készletek a Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ce738-108">For hello list of all hello preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="ce738-109">Ez a témakör bemutatja a hello `H264 Multiple Bitrate 4K` beállított XML és a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="ce738-109">This topic shows hello `H264 Multiple Bitrate 4K` preset in XML and JSON format.</span></span>  
  
 <span data-ttu-id="ce738-110">Az előre definiált létrejön egy 12 GOP igazított MP4-fájlok közötti 20000 kbps too1000 kbit/s és sztereó AAC hang.</span><span class="sxs-lookup"><span data-stu-id="ce738-110">This preset produces a set of 12 GOP-aligned MP4 files, ranging from 20000 kbps too1000 kbps, and stereo AAC audio.</span></span> <span data-ttu-id="ce738-111">Profillal kapcsolatos részletes információkért sávszélességű mintavételi arány stb. Ennek az adott néven beállítás esetében vizsgálja meg, XML vagy az alább megadott JSON hello.</span><span class="sxs-lookup"><span data-stu-id="ce738-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine hello XML or JSON defined below.</span></span> <span data-ttu-id="ce738-112">Az egyes milyen egyes elemei a készletek azt jelenti, és hello az érvényes értékek az egyes elemekhez, lásd: hello [Media Encoder Standard séma](media-services-mes-schema.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="ce738-112">For explanations of what each element in these presets means, and hello valid values for each element, see hello [Media Encoder Standard schema](media-services-mes-schema.md) topic.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="ce738-113">Hello szolgáltatás számára fenntartott prémium kapja meg a 4 KB-os egységtípus kódolja.</span><span class="sxs-lookup"><span data-stu-id="ce738-113">You should get hello Premium reserved unit type with 4K encodes.</span></span> <span data-ttu-id="ce738-114">További információkért lásd: [hogyan tooScale kódolás](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).</span><span class="sxs-lookup"><span data-stu-id="ce738-114">For more information, see [How tooScale Encoding](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="ce738-115">Ha módosítja a hello `Width` és `Height` értékek különböző rétegek, győződjön meg arról, hogy hello oldalarányának konzisztensek maradnak.</span><span class="sxs-lookup"><span data-stu-id="ce738-115">When modifying hello `Width` and `Height` values across layers, make sure that hello aspect ratio remains consistent.</span></span> <span data-ttu-id="ce738-116">Például: 1920 x 1080, 1280 x 720, 1080 x 576, 640 x 360.</span><span class="sxs-lookup"><span data-stu-id="ce738-116">For example: 1920x1080, 1280x720, 1080x576, 640x360.</span></span> <span data-ttu-id="ce738-117">Például ne használjon méretarányoknak megfelelően keverékével: 1280 x 720, 720 x 480, 640 x 360.</span><span class="sxs-lookup"><span data-stu-id="ce738-117">You should not use a mixture of aspect ratios, such as: 1280x720, 720x480, 640x360.</span></span>  
  
 <span data-ttu-id="ce738-118">XML</span><span class="sxs-lookup"><span data-stu-id="ce738-118">XML</span></span>  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>20000</Bitrate>  
          <Width>4096</Width>  
          <Height>2304</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>20000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>18000</Bitrate>  
          <Width>3840</Width>  
          <Height>2160</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>18000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>16000</Bitrate>  
          <Width>3840</Width>  
          <Height>2160</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>16000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>14000</Bitrate>  
          <Width>3840</Width>  
          <Height>2160</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>14000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>12000</Bitrate>  
          <Width>2560</Width>  
          <Height>1440</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>12000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>10000</Bitrate>  
          <Width>2560</Width>  
          <Height>1440</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>10000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>8000</Bitrate>  
          <Width>2560</Width>  
          <Height>1440</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>8000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>6000</Bitrate>  
          <Width>1920</Width>  
          <Height>1080</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>6000</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>4700</Bitrate>  
          <Width>1920</Width>  
          <Height>1080</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>4700</MaxBitrate>  
        </H264Layer>  
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
      </H264Layers>  
      <Chapters />  
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
```  
  
 <span data-ttu-id="ce738-119">JSON</span><span class="sxs-lookup"><span data-stu-id="ce738-119">JSON</span></span>  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 20000,  
          "MaxBitrate": 20000,  
          "BufferWindow": "00:00:05",  
          "Width": 4096,  
          "Height": 2304,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 18000,  
          "MaxBitrate": 18000,  
          "BufferWindow": "00:00:05",  
          "Width": 3840,  
          "Height": 2160,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 16000,  
          "MaxBitrate": 16000,  
          "BufferWindow": "00:00:05",  
          "Width": 3840,  
          "Height": 2160,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 14000,  
          "MaxBitrate": 14000,  
          "BufferWindow": "00:00:05",  
          "Width": 3840,  
          "Height": 2160,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 12000,  
          "MaxBitrate": 12000,  
          "BufferWindow": "00:00:05",  
          "Width": 2560,  
          "Height": 1440,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 10000,  
          "MaxBitrate": 10000,  
          "BufferWindow": "00:00:05",  
          "Width": 2560,  
          "Height": 1440,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 8000,  
          "MaxBitrate": 8000,  
          "BufferWindow": "00:00:05",  
          "Width": 2560,  
          "Height": 1440,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 6000,  
          "MaxBitrate": 6000,  
          "BufferWindow": "00:00:05",  
          "Width": 1920,  
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
          "Bitrate": 4700,  
          "MaxBitrate": 4700,  
          "BufferWindow": "00:00:05",  
          "Width": 1920,  
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
```

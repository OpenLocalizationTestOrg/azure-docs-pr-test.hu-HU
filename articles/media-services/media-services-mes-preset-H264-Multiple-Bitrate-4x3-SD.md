---
title: "H264 Több sávszélességű 4 x 3 SD |} Microsoft Docs"
description: "A témakör áttekintést a ** H264 Multiple Bitrate 4 x 3 SD ** feladat készletet."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 13e68b15-d090-4bf5-8e52-872eab025bc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: f8d342b0c317c31af0e02effbf8b8c1849e0bf1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="h264-multiple-bitrate-4x3-sd"></a><span data-ttu-id="9bdb8-103">H264 Több sávszélességű 4 x 3 SD</span><span class="sxs-lookup"><span data-stu-id="9bdb8-103">H264 Multiple Bitrate 4x3 SD</span></span>
<span data-ttu-id="9bdb8-104">`Media Encoder Standard`Meghatározza a kódolási készletek kódolási feladatok létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="9bdb8-105">Használhatja a `preset name` adhatja meg, melyik formátumba kódolja a médiafájl szeretné.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-105">You can either use a `preset name` to specify into which format you would like to encode your media file.</span></span> <span data-ttu-id="9bdb8-106">Másik lehetőségként létrehozhat saját JSON- vagy XML-alapú készletek (UTF-8 vagy UTF-16 kódolás használatával.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="9bdb8-107">Az egyéni készletet a kódoló majd át lesz.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-107">You would then pass the custom preset to the encoder.</span></span> <span data-ttu-id="9bdb8-108">Ez által támogatott összes előre definiált nevek listája `Media Encoder Standard` kódoló, lásd: [feladat készletek a Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9bdb8-108">For the list of all the preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="9bdb8-109">Ez a témakör bemutatja a `H264 Multiple Bitrate 4x3 SD` beállított XML és a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-109">This topic shows the `H264 Multiple Bitrate 4x3 SD` preset in XML and JSON format.</span></span>  
  
 <span data-ttu-id="9bdb8-110">Az előre definiált létrejön egy 5 GOP igazított MP4-fájlok 400 kbit/s és sztereó AAC hang 1600 kbps kezdve.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-110">This preset produces a set of 5 GOP-aligned MP4 files, ranging from 1600 kbps to 400 kbps, and stereo AAC audio.</span></span> <span data-ttu-id="9bdb8-111">Profillal kapcsolatos részletes információkért sávszélességű mintavételi arány stb. a készletet, vizsgálja meg az XML- vagy JSON-ban megadva.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine the XML or JSON defined below.</span></span> <span data-ttu-id="9bdb8-112">Az egyes milyen egyes elemei a készletek azt jelenti, és az érvényes értékek az egyes elemekhez, tekintse meg a [Media Encoder Standard séma](media-services-mes-schema.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-112">For explanations of what each element in these presets means, and the valid values for each element, see the [Media Encoder Standard schema](media-services-mes-schema.md) topic.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="9bdb8-113">Ha módosítja a `Width` és `Height` értékek különböző rétegek, győződjön meg arról, hogy az eredeti oldalarányok konzisztensek maradnak.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-113">When modifying the `Width` and `Height` values across layers, make sure that the aspect ratio remains consistent.</span></span> <span data-ttu-id="9bdb8-114">Például: 1920 x 1080, 1280 x 720, 1080 x 576, 640 x 360.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-114">For example: 1920x1080, 1280x720, 1080x576, 640x360.</span></span> <span data-ttu-id="9bdb8-115">Például ne használjon méretarányoknak megfelelően keverékével: 1280 x 720, 720 x 480, 640 x 360.</span><span class="sxs-lookup"><span data-stu-id="9bdb8-115">You should not use a mixture of aspect ratios, such as: 1280x720, 720x480, 640x360.</span></span>  
  
 <span data-ttu-id="9bdb8-116">XML</span><span class="sxs-lookup"><span data-stu-id="9bdb8-116">XML</span></span>  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>1600</Bitrate>  
          <Width>640</Width>  
          <Height>480</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>1600</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>1300</Bitrate>  
          <Width>640</Width>  
          <Height>480</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>1300</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>800</Bitrate>  
          <Width>480</Width>  
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
          <MaxBitrate>800</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>600</Bitrate>  
          <Width>480</Width>  
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
          <MaxBitrate>600</MaxBitrate>  
        </H264Layer>  
        <H264Layer>  
          <Bitrate>400</Bitrate>  
          <Width>360</Width>  
          <Height>240</Height>  
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
  
 <span data-ttu-id="9bdb8-117">JSON</span><span class="sxs-lookup"><span data-stu-id="9bdb8-117">JSON</span></span>  
  
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
          "Bitrate": 1600,  
          "MaxBitrate": 1600,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 480,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 1300,  
          "MaxBitrate": 1300,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 480,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        },  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 800,  
          "MaxBitrate": 800,  
          "BufferWindow": "00:00:05",  
          "Width": 480,  
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
          "Bitrate": 600,  
          "MaxBitrate": 600,  
          "BufferWindow": "00:00:05",  
          "Width": 480,  
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
          "Width": 360,  
          "Height": 240,  
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

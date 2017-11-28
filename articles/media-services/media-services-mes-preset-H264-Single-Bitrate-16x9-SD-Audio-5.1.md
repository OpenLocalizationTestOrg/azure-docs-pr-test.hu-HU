---
title: "aaaH264 egyféle sávszélességű 16 x 9 SD hang 5.1 |} Microsoft Docs"
description: "hello a témakör áttekintést hello ** H264 egyféle sávszélességű 16 x 9 SD hang 5.1* * feladat készletet."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d57cd39f-60e8-4248-a21d-c5b9f096b0ce
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: a1805112549668699bc17c56b7b8c869c30eaf90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="h264-single-bitrate-16x9-sd-audio-51"></a><span data-ttu-id="843c1-103">H264 Egyféle sávszélességű 16 x 9 SD hang 5.1</span><span class="sxs-lookup"><span data-stu-id="843c1-103">H264 Single Bitrate 16x9 SD Audio 5.1</span></span>
<span data-ttu-id="843c1-104">`Media Encoder Standard`Meghatározza a kódolási készletek kódolási feladatok létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="843c1-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="843c1-105">Használhatja a `preset name` mely formátumú fájlba szeretné tooencode toospecify a médiafájl.</span><span class="sxs-lookup"><span data-stu-id="843c1-105">You can either use a `preset name` toospecify into which format you would like tooencode your media file.</span></span> <span data-ttu-id="843c1-106">Másik lehetőségként létrehozhat saját JSON- vagy XML-alapú készletek (UTF-8 vagy UTF-16 kódolás használatával.</span><span class="sxs-lookup"><span data-stu-id="843c1-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="843c1-107">Majd át kellene hello egyéni előre definiált toohello kódoló.</span><span class="sxs-lookup"><span data-stu-id="843c1-107">You would then pass hello custom preset toohello encoder.</span></span> <span data-ttu-id="843c1-108">Minden hello hello listáját az adott néven beállítás által támogatott nevek `Media Encoder Standard` kódoló, lásd: [feladat készletek a Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="843c1-108">For hello list of all hello preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="843c1-109">Ez a témakör bemutatja a hello `H264 Single Bitrate 16x9 SD Audio 5.1` beállított XML és a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="843c1-109">This topic shows hello `H264 Single Bitrate 16x9 SD Audio 5.1` preset in XML and JSON format.</span></span>  
  
 <span data-ttu-id="843c1-110">Ezt a készletet hoz létre egyetlen MP4-fájl egy sávszélességű 2200 kbit/s és AAC 5.1 hang.</span><span class="sxs-lookup"><span data-stu-id="843c1-110">This preset produces a single MP4 file with a bitrate of 2200 kbps, and AAC 5.1 audio.</span></span> <span data-ttu-id="843c1-111">Profillal kapcsolatos részletes információkért sávszélességű mintavételi arány stb. Ennek az adott néven beállítás esetében vizsgálja meg, XML vagy az alább megadott JSON hello.</span><span class="sxs-lookup"><span data-stu-id="843c1-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine hello XML or JSON defined below.</span></span> <span data-ttu-id="843c1-112">További ismereteket szeretnének elsajátítani a azt jelenti, hogy milyen egyes elemei, és hello az érvényes értékek az egyes elemek: hello [Media Encoder Standard séma](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="843c1-112">For explanations of what each element means, and hello valid values for each element, see hello [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>  
  
 <span data-ttu-id="843c1-113">XML</span><span class="sxs-lookup"><span data-stu-id="843c1-113">XML</span></span>  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>2200</Bitrate>  
          <Width>848</Width>  
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
          <MaxBitrate>2200</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>6</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>384</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 <span data-ttu-id="843c1-114">JSON</span><span class="sxs-lookup"><span data-stu-id="843c1-114">JSON</span></span>  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 2200,  
          "MaxBitrate": 2200,  
          "BufferWindow": "00:00:05",  
          "Width": 848,  
          "Height": 480,  
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
      "Channels": 6,  
      "SamplingRate": 48000,  
      "Bitrate": 384,  
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

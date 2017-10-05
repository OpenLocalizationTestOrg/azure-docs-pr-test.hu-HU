---
title: "H264 egyféle sávszélességű Azure Media Encoder Standard beállított - 720p |} Microsoft Docs"
description: "A témakör áttekintést a ** H264 egyféle sávszélességű 720 p ** feladat készletet."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: f66da66c-9f21-441d-8038-51e3094e9307
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 5ca683db654a5adda6e15e1761579c7e81438417
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="h264-single-bitrate-720p"></a><span data-ttu-id="0b622-103">H264 Egyféle sávszélességű 720p</span><span class="sxs-lookup"><span data-stu-id="0b622-103">H264 Single Bitrate 720p</span></span>
<span data-ttu-id="0b622-104">`Media Encoder Standard`Meghatározza a kódolási készletek kódolási feladatok létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="0b622-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="0b622-105">Használhatja a `preset name` adhatja meg, melyik formátumba kódolja a médiafájl szeretné.</span><span class="sxs-lookup"><span data-stu-id="0b622-105">You can either use a `preset name` to specify into which format you would like to encode your media file.</span></span> <span data-ttu-id="0b622-106">Másik lehetőségként létrehozhat saját JSON- vagy XML-alapú készletek (UTF-8 vagy UTF-16 kódolás használatával.</span><span class="sxs-lookup"><span data-stu-id="0b622-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="0b622-107">Az egyéni készletet a kódoló majd át lesz.</span><span class="sxs-lookup"><span data-stu-id="0b622-107">You would then pass the custom preset to the encoder.</span></span> <span data-ttu-id="0b622-108">Ez által támogatott összes előre definiált nevek listája `Media Encoder Standard` kódoló, lásd: [feladat készletek a Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0b622-108">For the list of all the preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="0b622-109">Ez a témakör bemutatja a `H264 Single Bitrate 720p` beállított XML és a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="0b622-109">This topic shows the `H264 Single Bitrate 720p` preset in XML and JSON format.</span></span>  
  
 <span data-ttu-id="0b622-110">Ez egy sávszélességű 4500 kbit/s és sztereó AAC hang előre definiált előállított egyetlen MP4 fájl.</span><span class="sxs-lookup"><span data-stu-id="0b622-110">This preset produces a single MP4 file with a bitrate of 4500 kbps, and stereo AAC audio.</span></span> <span data-ttu-id="0b622-111">Profillal kapcsolatos részletes információkért sávszélességű mintavételi arány stb. a készletet, vizsgálja meg az XML- vagy JSON-ban megadva.</span><span class="sxs-lookup"><span data-stu-id="0b622-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine the XML or JSON defined below.</span></span> <span data-ttu-id="0b622-112">Az egyes milyen egyes elemei a készletek azt jelenti, és az érvényes értékek az egyes elemekhez, tekintse meg a [Media Encoder Standard séma](media-services-mes-schema.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="0b622-112">For explanations of what each element in these presets means, and the valid values for each element, see the [Media Encoder Standard schema](media-services-mes-schema.md) topic.</span></span>  
  
 <span data-ttu-id="0b622-113">XML</span><span class="sxs-lookup"><span data-stu-id="0b622-113">XML</span></span>  
  
```  
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
  
 <span data-ttu-id="0b622-114">JSON</span><span class="sxs-lookup"><span data-stu-id="0b622-114">JSON</span></span>  
  
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
          "Bitrate": 4500,  
          "MaxBitrate": 4500,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
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

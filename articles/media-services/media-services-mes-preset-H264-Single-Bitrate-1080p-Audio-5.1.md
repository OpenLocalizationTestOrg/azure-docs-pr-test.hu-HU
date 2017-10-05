---
title: H264 Egyetlen Bitrate 1080p hang 5.1 |} Microsoft Docs
description: "A témakör áttekintést a ** H264 egyetlen Bitrate 1080p hang 5.1* * feladat, előre definiált."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: b42238de-2a3c-4683-ae7f-7ce19ad5162e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako
ms.openlocfilehash: 07440d18afa83c571f1568a2e43fb6bca5e8b452
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="h264-single-bitrate-1080p-audio-51"></a><span data-ttu-id="be03e-103">H264 Egyetlen Bitrate 1080p hang 5.1</span><span class="sxs-lookup"><span data-stu-id="be03e-103">H264 Single Bitrate 1080p Audio 5.1</span></span>
<span data-ttu-id="be03e-104">`Media Encoder Standard`Meghatározza a kódolási készletek kódolási feladatok létrehozásakor használható.</span><span class="sxs-lookup"><span data-stu-id="be03e-104">`Media Encoder Standard` defines a set of encoding presets you can use when creating encoding jobs.</span></span> <span data-ttu-id="be03e-105">Használhatja a `preset name` adhatja meg, melyik formátumba kódolja a médiafájl szeretné.</span><span class="sxs-lookup"><span data-stu-id="be03e-105">You can either use a `preset name` to specify into which format you would like to encode your media file.</span></span> <span data-ttu-id="be03e-106">Másik lehetőségként létrehozhat saját JSON- vagy XML-alapú készletek (UTF-8 vagy UTF-16 kódolás használatával.</span><span class="sxs-lookup"><span data-stu-id="be03e-106">Or, you can create your own JSON or XML-based presets (using UTF-8 or UTF-16 encoding.</span></span> <span data-ttu-id="be03e-107">Az egyéni készletet a kódoló majd át lesz.</span><span class="sxs-lookup"><span data-stu-id="be03e-107">You would then pass the custom preset to the encoder.</span></span> <span data-ttu-id="be03e-108">Ez által támogatott összes előre definiált nevek listája `Media Encoder Standard` kódoló, lásd: [feladat készletek a Media Encoder Standard](media-services-mes-presets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="be03e-108">For the list of all the preset names supported by this `Media Encoder Standard` encoder, see [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md).</span></span>  
  
 <span data-ttu-id="be03e-109">Ez a témakör bemutatja a `H264 Single Bitrate 1080p Audio 5.1` beállított XML és a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="be03e-109">This topic shows the `H264 Single Bitrate 1080p Audio 5.1` preset in XML and JSON format..</span></span>  
  
 <span data-ttu-id="be03e-110">Ezt a készletet hoz létre egyetlen MP4-fájl egy sávszélességű 6750 kbit/s és AAC 5.1 hang.</span><span class="sxs-lookup"><span data-stu-id="be03e-110">This preset produces a single MP4 file with a bitrate of 6750 kbps, and AAC 5.1 audio.</span></span> <span data-ttu-id="be03e-111">Profillal kapcsolatos részletes információkért sávszélességű mintavételi arány stb. a készletet, vizsgálja meg az XML- vagy JSON-ban megadva.</span><span class="sxs-lookup"><span data-stu-id="be03e-111">For detailed information about profile, bitrate, sampling rate, etc. of this preset, examine the XML or JSON defined below.</span></span> <span data-ttu-id="be03e-112">Az egyes azt jelenti, hogy milyen egyes elemei, és az érvényes értékek az egyes elemekhez, tekintse meg a [Media Encoder Standard séma](media-services-mes-schema.md).</span><span class="sxs-lookup"><span data-stu-id="be03e-112">For explanations of what each element means, and the valid values for each element, see the [Media Encoder Standard schema](media-services-mes-schema.md).</span></span>  
  
 <span data-ttu-id="be03e-113">XML</span><span class="sxs-lookup"><span data-stu-id="be03e-113">XML</span></span>  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>6750</Bitrate>  
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
          <MaxBitrate>6750</MaxBitrate>  
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
  
 <span data-ttu-id="be03e-114">JSON</span><span class="sxs-lookup"><span data-stu-id="be03e-114">JSON</span></span>  
  
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
          "Bitrate": 6750,  
          "MaxBitrate": 6750,  
          "BufferWindow": "00:00:05",  
          "Width": 1920,  
          "Height": 1080,  
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

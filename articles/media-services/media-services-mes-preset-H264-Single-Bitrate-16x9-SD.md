---
title: "aaaH264 egyféle sávszélességű 16 x 9 SD Media Encoder Standard előre definiált - Azure |} Microsoft Docs"
description: "hello a témakör áttekintést hello ** H264 egyféle sávszélességű 16 x 9 SD ** feladat készletet."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: ce0efc07-3461-44f6-a7bc-c4877bc09529
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: e5fbf6b8accaf779014bee67ee4f5fd6910b2d25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="h264-single-bitrate-16x9-sd"></a>H264 Egyféle sávszélességű 16 x 9 SD
`Media Encoder Standard`Meghatározza a kódolási készletek kódolási feladatok létrehozásakor használható. Használhatja a `preset name` mely formátumú fájlba szeretné tooencode toospecify a médiafájl. Másik lehetőségként létrehozhat saját JSON- vagy XML-alapú készletek (UTF-8 vagy UTF-16 kódolás használatával. Majd át kellene hello egyéni előre definiált toohello kódoló. Minden hello hello listáját az adott néven beállítás által támogatott nevek `Media Encoder Standard` kódoló, lásd: [feladat készletek a Media Encoder Standard](media-services-mes-presets-overview.md).  
  
 Ez a témakör bemutatja a hello `H264 Single Bitrate 16x9 SD` beállított XML és a JSON formátumban.  
  
 Ez egy sávszélességű 2200 kbit/s és sztereó AAC hang előre definiált előállított egyetlen MP4 fájl. Profillal kapcsolatos részletes információkért sávszélességű mintavételi arány stb. Ennek az adott néven beállítás esetében vizsgálja meg, XML vagy az alább megadott JSON hello. Az egyes milyen egyes elemei a készletek azt jelenti, és hello az érvényes értékek az egyes elemekhez, lásd: hello [Media Encoder Standard séma](media-services-mes-schema.md) témakör.  
  
 XML  
  
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
  
 JSON  
  
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
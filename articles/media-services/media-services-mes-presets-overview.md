---
title: "a MES (Media Encoder Standard) aaaTask készletek |} Microsoft Docs"
description: "hello témakör által biztosított és MES (Media Encoder Standard) a feladat készletek áttekintése."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 56e098d6d8c8f84031421ec59f087f20370ba111
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="task-presets-for-mes-media-encoder-standard"></a>A feladat-készletek a MES (Media Encoder Standard)

**Media Encoder Standard** kódolási feladatok létrehozásakor használható készletek kódolási határozza meg. "Adaptív Streameléshez" készletet, ha azt szeretné, tooencode videó az adatfolyamként történő Media Services toouse hello ajánlott. Ha előre definiált, Media Encoder Standard ezzel a [automatikus létrehozása egy sávszélességű létra](media-services-autogen-bitrate-ladder-with-mes.md). 

Azonban ha egy kódolási beállításkészlet toocustomize van szüksége, gondolja hello kódolás meghatározva ebben a szakaszban egy sablont az egyéni konfigurációs készletek egyikét. Az egyes milyen egyes elemei a készletek azt jelenti, és hello az érvényes értékek az egyes elemekhez, lásd: hello [Media Encoder Standard séma](media-services-mes-schema.md) témakör.  
  
> [!NOTE]
>  Használatakor a készletet a 4 KB-os kódolja szerezheti be hello `S3` szolgáltatás számára fenntartott egység típusát. További információkért lásd: [hogyan tooScale kódolás](https://azure.microsoft.com/en-us/documentation/articles/media-services-portal-encoding-units).  
  
A Media Encoder Standard használatakor Elforgatás alapértelmezés szerint engedélyezve van. Ha a videó rögzítettük okostelefont vagy más mobileszköz álló üzemmódban, majd a készleteket lesz, alapértelmezés szerint forgathatók tooLandscape mód előzetes tooencoding (ellentétben, Azure Media Encoder, ahol videó elforgatása egy kézi művelet, használata esetén Ahogy [ez](http://azure.microsoft.com/blog/2014/08/21/advanced-encoding-features-in-azure-media-encoder/) blog, a "Videó Elforgatás").  
  
Rendelkezésre álló készletek:  
  
 [H264 Multiple Bitrate 1080p hang 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) létrejön egy 8 GOP igazított MP4-fájlok közötti 6000 kbps too400 kbit/s és AAC 5.1 hang.  
  
 [H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) létrejön egy 8 GOP igazított MP4-fájlok közötti 6000 kbps too400 kbit/s és sztereó AAC hang.  
  
 [H264 Multiple Bitrate 16 x 9 IOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) létrejön egy 8 GOP igazított MP4-fájlok közötti 8500 kbps too200 kbit/s és sztereó AAC hang.  
  
 [H264 Multiple Bitrate 16 x 9 SD hang 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) létrejön egy 5 GOP igazított MP4-fájlok közötti 1900 kbps too400 kbit/s és AAC 5.1 hang.  
  
 [H264 Multiple Bitrate 16 x 9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) létrejön egy 5 GOP igazított MP4-fájlok közötti 1900 kbps too400 kbit/s és sztereó AAC hang.  
  
 [5.1 H264 Multiple Bitrate 4 KB-os hang](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) létrejön egy 12 GOP igazított MP4-fájlok közötti 20000 kbps too1000 kbit/s, és AAC 5.1 hang.  
  
 [H264 Multiple Bitrate 4 KB-os](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) létrejön egy 12 GOP igazított MP4-fájlok közötti 20000 kbps too1000 kbit/s és sztereó AAC hang.  
  
 [H264 Multiple Bitrate 4 x 3 IOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) létrejön egy 8 GOP igazított MP4-fájlok közötti 8500 kbps too200 kbit/s és sztereó AAC hang.  
  
 [H264 Multiple Bitrate 4 x 3 SD hang 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) létrejön egy 5 GOP igazított MP4-fájlok közötti 1600 kbps too400 kbit/s és AAC 5.1 hang.  
  
 [H264 Multiple Bitrate 4 x 3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) létrejön egy 5 GOP igazított MP4-fájlok közötti 1600 kbps too400 kbit/s és sztereó AAC hang.  
  
 [H264 Multiple Bitrate 720p hang 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) létrejön egy 6 GOP igazított MP4-fájlok közötti 3400 kbps too400 kbit/s és AAC 5.1 hang.  
  
 [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) létrejön egy 6 GOP igazított MP4-fájlok közötti 3400 kbps too400 kbit/s és sztereó AAC hang.  
  
 [H264 egyetlen Bitrate 1080p hang 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) egyetlen MP4-fájl egy sávszélességű 6750 kbit/s és AAC 5.1 hang eredményez.  
  
 [H264 egyetlen Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) egyetlen MP4-fájl egy sávszélességű 6750 kbit/s és sztereó AAC hang eredményez.  
  
 [5.1 H264 egyféle sávszélességű 4 KB-os hang](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) egyetlen MP4-fájl egy sávszélességű 18000 kbit/s és AAC 5.1 hang eredményez.  
  
 [H264 egyféle sávszélességű 4 KB-os](media-services-mes-preset-H264-Single-Bitrate-4K.md) egyetlen MP4-fájl egy sávszélességű 18000 kbit/s és sztereó AAC hang eredményez.  
  
 [H264 egyféle sávszélességű 4 x 3 SD hang 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) egyetlen MP4-fájl egy 1800 kbit/s és AAC 5.1 hang sávszélességű eredményez.  
  
 [H264 egyféle sávszélességű 4 x 3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) egyetlen MP4-fájl egy 1800 kbit/s és sztereó AAC hang sávszélességű eredményez.  
  
 [H264 egyféle sávszélességű 16 x 9 SD hang 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) egyetlen MP4-fájl egy sávszélességű 2200 kbit/s és AAC 5.1 hang eredményez.  
  
 [H264 egyféle sávszélességű 16 x 9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) egyetlen MP4-fájl egy sávszélességű 2200 kbit/s és sztereó AAC hang eredményez.  
  
 [H264 egyféle sávszélességű 720p hang 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) egyetlen MP4-fájl egy sávszélességű 4500 kbit/s és AAC 5.1 hang eredményez.  
  
 [H264 egyféle sávszélességű Android 720p](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) készletet hoz létre egyetlen MP4-fájl egy sávszélességű 2000 kbit/s és sztereó AAC.  
  
 [H264 egyféle sávszélességű 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) egyetlen MP4-fájl egy sávszélességű 4500 kbit/s és sztereó AAC hang eredményez.  
  
 [H264 egyetlen sávszélességű kiváló minőségű SD Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) hozza létre egyetlen MP4-fájl egy sávszélességű 500 KB/s és sztereó AAC hang...  
  
 [H264 egyetlen sávszélességű alacsony minőségi SD Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) egyetlen MP4-fájl egy sávszélességű 56 kb/s és sztereó AAC hang eredményez.  
  
 További információ: kapcsolódó tooMedia szolgáltatások kódolók, [kódolás igény az Azure Media Services](https://azure.microsoft.com/en-us/documentation/articles/media-services-encode-asset/).

---
title: "aaaMedia kódoló prémium munkafolyamat formátumok és kodekek |} Microsoft Docs"
description: "Ez a témakör áttekintést nyújt a Media Encoder prémium munkafolyamat formátumok formátumok és kodekek"
services: media-services
documentationcenter: 
author: juliako
manager: erik43
editor: 
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: e781384ca8f08926f00c83b6710fd413ce2a3e1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder prémium szintű munkafolyamat-formátumok és kodekek
> [!NOTE]
> Prémium szintű kódolás kérdéseket tesz fel a Microsoft.com mepd e-mail.
> 
> A jelen témakörben bemutatott Media Encoder prémium munkafolyamat media processzor Kínában nem érhető el. 
> 
> 

Ez a dokumentum tartalmazza a bemeneti és kimeneti fájlformátumok és hello hello nyilvános előzetes verziója által támogatott kodekek listáját **Media Encoder prémium munkafolyamat** kódoló.

[Media Encoder prémium Worflow bemeneti formátumok és kodekek](#input_formats)

[Media Encoder prémium Worflow kimeneti formátumok és kodekek](#output_formats)

**Media Encoder prémium munkafolyamat** támogatja a kódolt feliratok a leírt [ez](#closed_captioning) szakasz. 

## <a id="input_formats"></a>Media Encoder prémium munkafolyamat bemeneti formátumok és kodekek
a következő szakasz hello hello kodekeket és a fájl formátuma, amely a media processzor támogatja a bemeneti sorolja fel.

### <a name="input-containerfile-formats"></a>Adjon meg tároló/fájlformátum
* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2 Transport adatfolyamok
* MPEG-2 Program adatfolyamok
* MPEG-4/MP4
* Windows Media/ASP
* AVI (tömörítetlen 8 bit/10 bit)

### <a name="input-video-codecs"></a>A bemeneti videó kodekek
* AVC 8 bit/10-bites too4:2:2, beleértve a AVCIntra mentése
* (A MXF) Avid DNxHD
* DVCPro/DVCProHD (a MXF)
* JPEG2000
* MPEG-2 (too422 profil és a magas szintű; például XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 Variant típusú adatok is beleértve)
* MPEG-1
* Windows Media videó/VC-1

### <a name="input-audio-codecs"></a>Bemeneti hang kodekek
* AES (SMPTE 331 M és 302 M, AES3-2003)
* E Dolby®
* Dolby® digitális (AC3)
* AAC (AAC-LC, AAC-HE és AAC-HEv2; too5.1 mentése)
* 2. réteg MPEG
* MP3 (MPEG-1 hang réteg 3)
* Windows Media hang
* WAV/PCM

## <a id="output_format"></a>Media Encoder prémium munkafolyamat kimeneti formátumok és kodekek
hello következő szakasz hello kodekeket és a fájl formátuma a media processzor kimeneteként támogatott.

### <a name="output-containerfile-formats"></a>Kimeneti tároló/fájlformátum
* Adobe® Flash® F4V
* MXF (OP1a, XDCAM és AS02)
* DPP (beleértve a AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASP
* AVI (tömörítetlen 8 bit/10 bit)
* Zökkenőmentes adatfolyam fájlformátum (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Kimeneti videó kodekek
* AVC (H.264; 8 bites up tooHigh profil, szint 5.2; 4 KB-os Ultra HD; AVC belüli)
* (A MXF) Avid DNxHD
* DVCPro/DVCProHD (a MXF)
* MPEG-2 (too422 profil és a magas szintű; például XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 Variant típusú adatok is beleértve)
* MPEG-1
* Windows Media videó/VC-1
* JPEG miniatűr létrehozása

### <a name="output-audio-codecs"></a>Kimeneti hang kodekek
* AES (SMPTE 331 M és 302 M, AES3-2003)
* Dolby® digitális (AC3)
* Dolby® digitális Plus (E-AC3) too7.1 mentése
* AAC (AAC-LC, AAC-HE és AAC-HEv2; too5.1 mentése)
* 2. réteg MPEG
* MP3 (MPEG-1 hang réteg 3)
* Windows Media hang

>[!NOTE]
>Ha tooDolby kódol® digitális (AC3) hello kimeneti csak írható ISO-MP4-fájlokat.

## <a id="closed_captioning"></a>Feliratok támogatása
A betöltési, **Media Encoder prémium munkafolyamat** támogatja:

1. SCC fájlok
2. Fájlok SMPTE-TT
3. CEA-608/CEA-708 – felhasználói adatként (SEI üzenetek H.264 elemi adatfolyam, ATSC/53, SCTE20) vagy sor MXF/GXF fájlok kiegészítő adatok
4. STL alcíme fájlok

A kimenetet hello alábbi beállítások érhetők el:

1. CEA-608 tooCEA-708 fordítás
2. CEA-608/CEA-708 továbbítása (SEI üzenetek H.264 elemi adatfolyam beágyazva, vagy végzik a kiegészítő adatok MXF-fájlokban szereplő)
3. SCC
4. SMPTE túllépte az időkorlátot szöveg (a forrás CEA-608 / SMPTE RP2052; DFXP fájl létrehozása is beleértve)
5. SRT alcíme fájl
6. DVB alcíme adatfolyamok

Megjegyzés: nem az összes fent kimeneti formátum hello támogatják keresztül Azure Media Services adatfolyamként történő kézbesítéshez.

## <a name="known-issues"></a>Ismert problémák
Ha a bemeneti videó nem tartalmaz a kódolt, a hello kimeneti eszköz továbbra is egy üres TTML fájlt fogja tartalmazni. 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


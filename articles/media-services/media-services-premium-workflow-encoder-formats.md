---
title: "Media Encoder prémium szintű munkafolyamat-formátumok és kodekek |} Microsoft Docs"
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
ms.openlocfilehash: e18de2adc9aac585d6890dd7b43a54f1a0ca177e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a><span data-ttu-id="dec17-103">Media Encoder prémium szintű munkafolyamat-formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-103">Media Encoder Premium Workflow formats and codecs</span></span>
> [!NOTE]
> <span data-ttu-id="dec17-104">Prémium szintű kódolás kérdéseket tesz fel a Microsoft.com mepd e-mail.</span><span class="sxs-lookup"><span data-stu-id="dec17-104">For premium encoder questions, email mepd at Microsoft.com.</span></span>
> 
> <span data-ttu-id="dec17-105">A jelen témakörben bemutatott Media Encoder prémium munkafolyamat media processzor Kínában nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="dec17-105">Media Encoder Premium Workflow media processor discussed in this topic is not available in China.</span></span> 
> 
> 

<span data-ttu-id="dec17-106">Ez a dokumentum a bemeneti és kimeneti fájlformátumokat és a nyilvános előzetes verziója által támogatott kodekek listáját tartalmazza a **Media Encoder prémium munkafolyamat** kódoló.</span><span class="sxs-lookup"><span data-stu-id="dec17-106">This document contains a list of input and output file formats and codecs that are supported by the public preview version of the **Media Encoder Premium Workflow** encoder.</span></span>

[<span data-ttu-id="dec17-107">Media Encoder prémium Worflow bemeneti formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-107">Media Encoder Premium Worflow Input Formats and Codecs</span></span>](#input_formats)

[<span data-ttu-id="dec17-108">Media Encoder prémium Worflow kimeneti formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-108">Media Encoder Premium Worflow Output Formats and Codecs</span></span>](#output_formats)

<span data-ttu-id="dec17-109">**Media Encoder prémium munkafolyamat** támogatja a kódolt feliratok a leírt [ez](#closed_captioning) szakasz.</span><span class="sxs-lookup"><span data-stu-id="dec17-109">**Media Encoder Premium Workflow** supports closed captioning described in [this](#closed_captioning) section.</span></span> 

## <span data-ttu-id="dec17-110"><a id="input_formats"></a>Media Encoder prémium munkafolyamat bemeneti formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-110"><a id="input_formats"></a>Media Encoder Premium Workflow Input Formats and Codecs</span></span>
<span data-ttu-id="dec17-111">A következő szakasz a kodekeket és fájlformátumot, amely a media processzor támogatja a bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="dec17-111">The following section lists the codecs and file formats that this media processor supports as input.</span></span>

### <a name="input-containerfile-formats"></a><span data-ttu-id="dec17-112">Adjon meg tároló/fájlformátum</span><span class="sxs-lookup"><span data-stu-id="dec17-112">Input Container/File Formats</span></span>
* <span data-ttu-id="dec17-113">Adobe® Flash® F4V</span><span class="sxs-lookup"><span data-stu-id="dec17-113">Adobe® Flash® F4V</span></span>
* <span data-ttu-id="dec17-114">MXF/SMPTE 377M</span><span class="sxs-lookup"><span data-stu-id="dec17-114">MXF/SMPTE 377M</span></span>
* <span data-ttu-id="dec17-115">GXF</span><span class="sxs-lookup"><span data-stu-id="dec17-115">GXF</span></span>
* <span data-ttu-id="dec17-116">MPEG-2 Transport adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="dec17-116">MPEG-2 Transport Streams</span></span>
* <span data-ttu-id="dec17-117">MPEG-2 Program adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="dec17-117">MPEG-2 Program Streams</span></span>
* <span data-ttu-id="dec17-118">MPEG-4/MP4</span><span class="sxs-lookup"><span data-stu-id="dec17-118">MPEG-4/MP4</span></span>
* <span data-ttu-id="dec17-119">Windows Media/ASP</span><span class="sxs-lookup"><span data-stu-id="dec17-119">Windows Media/ASF</span></span>
* <span data-ttu-id="dec17-120">AVI (tömörítetlen 8 bit/10 bit)</span><span class="sxs-lookup"><span data-stu-id="dec17-120">AVI (Uncompressed 8bit/10bit)</span></span>

### <a name="input-video-codecs"></a><span data-ttu-id="dec17-121">A bemeneti videó kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-121">Input Video Codecs</span></span>
* <span data-ttu-id="dec17-122">8 bit/10-bites, legfeljebb 4 AVC: 2:2, beleértve a AVCIntra</span><span class="sxs-lookup"><span data-stu-id="dec17-122">AVC 8-bit/10-bit, up to 4:2:2, including AVCIntra</span></span>
* <span data-ttu-id="dec17-123">(A MXF) Avid DNxHD</span><span class="sxs-lookup"><span data-stu-id="dec17-123">Avid DNxHD (in MXF)</span></span>
* <span data-ttu-id="dec17-124">DVCPro/DVCProHD (a MXF)</span><span class="sxs-lookup"><span data-stu-id="dec17-124">DVCPro/DVCProHD (in MXF)</span></span>
* <span data-ttu-id="dec17-125">JPEG2000</span><span class="sxs-lookup"><span data-stu-id="dec17-125">JPEG2000</span></span>
* <span data-ttu-id="dec17-126">MPEG-2 (legfeljebb 422 profil és a magas szintű; például XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 Variant típusú adatok is beleértve)</span><span class="sxs-lookup"><span data-stu-id="dec17-126">MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)</span></span>
* <span data-ttu-id="dec17-127">MPEG-1</span><span class="sxs-lookup"><span data-stu-id="dec17-127">MPEG-1</span></span>
* <span data-ttu-id="dec17-128">Windows Media videó/VC-1</span><span class="sxs-lookup"><span data-stu-id="dec17-128">Windows Media Video/VC-1</span></span>

### <a name="input-audio-codecs"></a><span data-ttu-id="dec17-129">Bemeneti hang kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-129">Input Audio Codecs</span></span>
* <span data-ttu-id="dec17-130">AES (SMPTE 331 M és 302 M, AES3-2003)</span><span class="sxs-lookup"><span data-stu-id="dec17-130">AES (SMPTE 331M and 302M, AES3-2003)</span></span>
* <span data-ttu-id="dec17-131">E Dolby®</span><span class="sxs-lookup"><span data-stu-id="dec17-131">Dolby® E</span></span>
* <span data-ttu-id="dec17-132">Dolby® digitális (AC3)</span><span class="sxs-lookup"><span data-stu-id="dec17-132">Dolby® Digital (AC3)</span></span>
* <span data-ttu-id="dec17-133">AAC (AAC-LC, AAC-HE és AAC-HEv2; akár 5.1)</span><span class="sxs-lookup"><span data-stu-id="dec17-133">AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)</span></span>
* <span data-ttu-id="dec17-134">2. réteg MPEG</span><span class="sxs-lookup"><span data-stu-id="dec17-134">MPEG Layer 2</span></span>
* <span data-ttu-id="dec17-135">MP3 (MPEG-1 hang réteg 3)</span><span class="sxs-lookup"><span data-stu-id="dec17-135">MP3 (MPEG-1 Audio Layer 3)</span></span>
* <span data-ttu-id="dec17-136">Windows Media hang</span><span class="sxs-lookup"><span data-stu-id="dec17-136">Windows Media Audio</span></span>
* <span data-ttu-id="dec17-137">WAV/PCM</span><span class="sxs-lookup"><span data-stu-id="dec17-137">WAV/PCM</span></span>

## <span data-ttu-id="dec17-138"><a id="output_format"></a>Media Encoder prémium munkafolyamat kimeneti formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-138"><a id="output_format"></a>Media Encoder Premium Workflow Output Formats and Codecs</span></span>
<span data-ttu-id="dec17-139">A következő szakasz a kodekeket és fájlformátumot, a media processzor kimeneteként támogatott.</span><span class="sxs-lookup"><span data-stu-id="dec17-139">The following section lists the codecs and file formats that are supported as output from this media processor.</span></span>

### <a name="output-containerfile-formats"></a><span data-ttu-id="dec17-140">Kimeneti tároló/fájlformátum</span><span class="sxs-lookup"><span data-stu-id="dec17-140">Output Container/File Formats</span></span>
* <span data-ttu-id="dec17-141">Adobe® Flash® F4V</span><span class="sxs-lookup"><span data-stu-id="dec17-141">Adobe® Flash® F4V</span></span>
* <span data-ttu-id="dec17-142">MXF (OP1a, XDCAM és AS02)</span><span class="sxs-lookup"><span data-stu-id="dec17-142">MXF (OP1a, XDCAM and AS02)</span></span>
* <span data-ttu-id="dec17-143">DPP (beleértve a AS11)</span><span class="sxs-lookup"><span data-stu-id="dec17-143">DPP (including AS11)</span></span>
* <span data-ttu-id="dec17-144">GXF</span><span class="sxs-lookup"><span data-stu-id="dec17-144">GXF</span></span>
* <span data-ttu-id="dec17-145">MPEG-4/MP4</span><span class="sxs-lookup"><span data-stu-id="dec17-145">MPEG-4/MP4</span></span>
* <span data-ttu-id="dec17-146">Windows Media/ASP</span><span class="sxs-lookup"><span data-stu-id="dec17-146">Windows Media/ASF</span></span>
* <span data-ttu-id="dec17-147">AVI (tömörítetlen 8 bit/10 bit)</span><span class="sxs-lookup"><span data-stu-id="dec17-147">AVI (Uncompressed 8bit/10bit)</span></span>
* <span data-ttu-id="dec17-148">Zökkenőmentes adatfolyam fájlformátum (PIFF 1.3)</span><span class="sxs-lookup"><span data-stu-id="dec17-148">Smooth Streaming File Format (PIFF 1.3)</span></span>
* <span data-ttu-id="dec17-149">MPEG-TS</span><span class="sxs-lookup"><span data-stu-id="dec17-149">MPEG-TS</span></span> 

### <a name="output-video-codecs"></a><span data-ttu-id="dec17-150">Kimeneti videó kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-150">Output Video Codecs</span></span>
* <span data-ttu-id="dec17-151">AVC (H.264; 8 bites; nagy profil, akár szinten 5.2-es; 4 KB-os Ultra HD; AVC belüli)</span><span class="sxs-lookup"><span data-stu-id="dec17-151">AVC (H.264; 8-bit; up to High Profile, Level 5.2; 4K Ultra HD; AVC Intra)</span></span>
* <span data-ttu-id="dec17-152">(A MXF) Avid DNxHD</span><span class="sxs-lookup"><span data-stu-id="dec17-152">Avid DNxHD (in MXF)</span></span>
* <span data-ttu-id="dec17-153">DVCPro/DVCProHD (a MXF)</span><span class="sxs-lookup"><span data-stu-id="dec17-153">DVCPro/DVCProHD (in MXF)</span></span>
* <span data-ttu-id="dec17-154">MPEG-2 (legfeljebb 422 profil és a magas szintű; például XDCAM, XDCAM HD, XDCAM IMX, CableLabs® és D10 Variant típusú adatok is beleértve)</span><span class="sxs-lookup"><span data-stu-id="dec17-154">MPEG-2 (up to 422 Profile and High Level; including variants such as XDCAM, XDCAM HD, XDCAM IMX, CableLabs® and D10)</span></span>
* <span data-ttu-id="dec17-155">MPEG-1</span><span class="sxs-lookup"><span data-stu-id="dec17-155">MPEG-1</span></span>
* <span data-ttu-id="dec17-156">Windows Media videó/VC-1</span><span class="sxs-lookup"><span data-stu-id="dec17-156">Windows Media Video/VC-1</span></span>
* <span data-ttu-id="dec17-157">JPEG miniatűr létrehozása</span><span class="sxs-lookup"><span data-stu-id="dec17-157">JPEG thumbnail creation</span></span>

### <a name="output-audio-codecs"></a><span data-ttu-id="dec17-158">Kimeneti hang kodekek</span><span class="sxs-lookup"><span data-stu-id="dec17-158">Output Audio Codecs</span></span>
* <span data-ttu-id="dec17-159">AES (SMPTE 331 M és 302 M, AES3-2003)</span><span class="sxs-lookup"><span data-stu-id="dec17-159">AES (SMPTE 331M and 302M, AES3-2003)</span></span>
* <span data-ttu-id="dec17-160">Dolby® digitális (AC3)</span><span class="sxs-lookup"><span data-stu-id="dec17-160">Dolby® Digital (AC3)</span></span>
* <span data-ttu-id="dec17-161">Dolby® digitális Plus (E-AC3) legfeljebb 7.1</span><span class="sxs-lookup"><span data-stu-id="dec17-161">Dolby® Digital Plus (E-AC3) up to 7.1</span></span>
* <span data-ttu-id="dec17-162">AAC (AAC-LC, AAC-HE és AAC-HEv2; akár 5.1)</span><span class="sxs-lookup"><span data-stu-id="dec17-162">AAC (AAC-LC, AAC-HE, and AAC-HEv2; up to 5.1)</span></span>
* <span data-ttu-id="dec17-163">2. réteg MPEG</span><span class="sxs-lookup"><span data-stu-id="dec17-163">MPEG Layer 2</span></span>
* <span data-ttu-id="dec17-164">MP3 (MPEG-1 hang réteg 3)</span><span class="sxs-lookup"><span data-stu-id="dec17-164">MP3 (MPEG-1 Audio Layer 3)</span></span>
* <span data-ttu-id="dec17-165">Windows Media hang</span><span class="sxs-lookup"><span data-stu-id="dec17-165">Windows Media Audio</span></span>

>[!NOTE]
><span data-ttu-id="dec17-166">Ha Ön a kódolás kimenete Dolby® digitális (AC3), a kimeneti csak írható ISO-MP4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="dec17-166">If you encode to Dolby® Digital (AC3), the output can only be written into an ISO MP4 file.</span></span>

## <span data-ttu-id="dec17-167"><a id="closed_captioning"></a>Feliratok támogatása</span><span class="sxs-lookup"><span data-stu-id="dec17-167"><a id="closed_captioning"></a>Support for Closed Captioning</span></span>
<span data-ttu-id="dec17-168">A betöltési, **Media Encoder prémium munkafolyamat** támogatja:</span><span class="sxs-lookup"><span data-stu-id="dec17-168">On ingest, **Media Encoder Premium Workflow** supports:</span></span>

1. <span data-ttu-id="dec17-169">SCC fájlok</span><span class="sxs-lookup"><span data-stu-id="dec17-169">SCC files</span></span>
2. <span data-ttu-id="dec17-170">Fájlok SMPTE-TT</span><span class="sxs-lookup"><span data-stu-id="dec17-170">SMPTE-TT files</span></span>
3. <span data-ttu-id="dec17-171">CEA-608/CEA-708 – felhasználói adatként (SEI üzenetek H.264 elemi adatfolyam, ATSC/53, SCTE20) vagy sor MXF/GXF fájlok kiegészítő adatok</span><span class="sxs-lookup"><span data-stu-id="dec17-171">CEA-608/CEA-708 – carried as user data (SEI messages of H.264 elementary streams, ATSC/53, SCTE20) or carried as ancillary data in MXF/GXF files</span></span>
4. <span data-ttu-id="dec17-172">STL alcíme fájlok</span><span class="sxs-lookup"><span data-stu-id="dec17-172">STL subtitle files</span></span>

<span data-ttu-id="dec17-173">A kimenetet az alábbi beállítások érhetők el:</span><span class="sxs-lookup"><span data-stu-id="dec17-173">On output, the following options are available:</span></span>

1. <span data-ttu-id="dec17-174">CEA-608 való CEA-708 fordítás</span><span class="sxs-lookup"><span data-stu-id="dec17-174">CEA-608 to CEA-708 translation</span></span>
2. <span data-ttu-id="dec17-175">CEA-608/CEA-708 továbbítása (SEI üzenetek H.264 elemi adatfolyam beágyazva, vagy végzik a kiegészítő adatok MXF-fájlokban szereplő)</span><span class="sxs-lookup"><span data-stu-id="dec17-175">CEA-608/CEA-708 pass through (embedded in SEI messages of H.264 elementary streams, or carried as ancillary data in MXF files)</span></span>
3. <span data-ttu-id="dec17-176">SCC</span><span class="sxs-lookup"><span data-stu-id="dec17-176">SCC</span></span>
4. <span data-ttu-id="dec17-177">SMPTE túllépte az időkorlátot szöveg (a forrás CEA-608 / SMPTE RP2052; DFXP fájl létrehozása is beleértve)</span><span class="sxs-lookup"><span data-stu-id="dec17-177">SMPTE Timed Text (from source CEA-608 per SMPTE RP2052; including DFXP file creation)</span></span>
5. <span data-ttu-id="dec17-178">SRT alcíme fájl</span><span class="sxs-lookup"><span data-stu-id="dec17-178">SRT Subtitle file</span></span>
6. <span data-ttu-id="dec17-179">DVB alcíme adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="dec17-179">DVB subtitle streams</span></span>

<span data-ttu-id="dec17-180">Megjegyzés: nem a fenti formátumokban használható összes keresztül Azure Media Services adatfolyamként történő kézbesítéshez.</span><span class="sxs-lookup"><span data-stu-id="dec17-180">Note: not all of the above output formats are supported for delivery via streaming in Azure Media Services.</span></span>

## <a name="known-issues"></a><span data-ttu-id="dec17-181">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="dec17-181">Known issues</span></span>
<span data-ttu-id="dec17-182">Ha a bemeneti videó nem tartalmaz kódolt a kimeneti eszköz továbbra is tartalmazni fog egy üres TTML fájlt.</span><span class="sxs-lookup"><span data-stu-id="dec17-182">If your input video does not contain closed captioning, the output Asset will still contain an empty TTML file.</span></span> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="dec17-183">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="dec17-183">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="dec17-184">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="dec17-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


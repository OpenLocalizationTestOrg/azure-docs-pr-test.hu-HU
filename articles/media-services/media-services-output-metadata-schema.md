---
title: "aaaAzure Media Services kimeneti metaadatok séma |} Microsoft Docs"
description: "hello a témakör áttekintést nyújt az Azure Media Services kimeneti metaadatok séma."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 1ed84c88-eea5-4a24-9c4f-f2428157d08a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 7f07d6accbe0b171d0408b15d5e1e6b5afd6c367
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="output-metadata"></a><span data-ttu-id="cedb2-103">Kimeneti metaadatok</span><span class="sxs-lookup"><span data-stu-id="cedb2-103">Output Metadata</span></span>
## <a name="overview"></a><span data-ttu-id="cedb2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="cedb2-104">Overview</span></span>
<span data-ttu-id="cedb2-105">A kódolási feladat vagy társítva egy bemeneti eszköz (eszközök) kívánja tooperform egyes kódolási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="cedb2-105">An encoding job is associated with an input asset (or assets) on which you want tooperform some encoding tasks.</span></span> <span data-ttu-id="cedb2-106">Például kódolása egy MP4 fájl tooH.264 MP4 adaptív sávszélességű beállítja; Hozzon létre egy miniatűrre. átfedések létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cedb2-106">For example, encode an MP4 file tooH.264 MP4 adaptive bitrate sets; create a thumbnail; create overlays.</span></span> <span data-ttu-id="cedb2-107">Létrehozása után egy feladatot kimeneti eszközként hozott létre.</span><span class="sxs-lookup"><span data-stu-id="cedb2-107">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="cedb2-108">hello kimeneti adategységen videót, hangot, miniatűrök, a stb hello kimeneti adategységen is hello kimeneti adategységen vonatkozó metaadatok fájlt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cedb2-108">hello output asset contains video, audio, thumbnails, etc. hello output asset also contains a file with metadata about hello output asset.</span></span> <span data-ttu-id="cedb2-109">hello hello metaadatok XML-fájl neve van hello a következő formátumban: &lt;source_file_name&gt;_manifest.xml (például BigBuckBunny_manifest.xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-109">hello name of hello metadata XML file has hello following format: &lt;source_file_name&gt;_manifest.xml (for example, BigBuckBunny_manifest.xml).</span></span>  

<span data-ttu-id="cedb2-110">Ha azt szeretné, hogy tooexamine hello metaadatfájl, létrehozhat egy **SAS** lokátor és a letöltési hello fájl tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cedb2-110">If you want tooexamine hello metadata file, you can create a **SAS** locator and download hello file tooyour local computer.</span></span>  

<span data-ttu-id="cedb2-111">Ez a témakör ismerteti a mely hello kimeneti metada hello elemek és hello XML-séma típusú (&lt;source_file_name&gt;_manifest.xml) alapul.</span><span class="sxs-lookup"><span data-stu-id="cedb2-111">This topic discusses hello elements and types of hello XML schema on which hello output metada (&lt;source_file_name&gt;_manifest.xml) is based.</span></span> <span data-ttu-id="cedb2-112">Hello bemeneti eszköz metaadatait tartalmazó fájl hello kapcsolatos információkért lásd: [bemeneti metaadatok](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cedb2-112">For information about hello file that contains metadata about hello input asset, see [Input Metadata](media-services-input-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="cedb2-113">Hello teljes séma kódot és az XML-példa hello Ez a témakör végén található.</span><span class="sxs-lookup"><span data-stu-id="cedb2-113">You can find hello complete schema code and XML example at hello end of this topic.</span></span>  
>
>

## <span data-ttu-id="cedb2-114"><a name="AssetFiles "></a>AssetFiles gyökérelem</span><span class="sxs-lookup"><span data-stu-id="cedb2-114"><a name="AssetFiles "></a> AssetFiles root element</span></span>
<span data-ttu-id="cedb2-115">Hello kódolási feladat AssetFile bejegyzések gyűjteményét.</span><span class="sxs-lookup"><span data-stu-id="cedb2-115">Collection of AssetFile entries for hello encoding job.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="cedb2-116">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="cedb2-116">Child elements</span></span>
| <span data-ttu-id="cedb2-117">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-117">Name</span></span> | <span data-ttu-id="cedb2-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-118">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cedb2-119">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="cedb2-119">**AssetFile**</span></span><br/><br/> <span data-ttu-id="cedb2-120">minOccurs = "0" maxOccurs = "1"</span><span class="sxs-lookup"><span data-stu-id="cedb2-120">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="cedb2-121">Egy [AssetFile elem](media-services-output-metadata-schema.md) részét képező hello AssetFiles gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="cedb2-121">An [AssetFile element](media-services-output-metadata-schema.md) that is part of hello AssetFiles collection.</span></span> |

## <span data-ttu-id="cedb2-122"><a name="AssetFile "></a>AssetFile elem</span><span class="sxs-lookup"><span data-stu-id="cedb2-122"><a name="AssetFile "></a> AssetFile element</span></span>
<span data-ttu-id="cedb2-123">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-123">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="cedb2-124">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="cedb2-124">Attributes</span></span>
| <span data-ttu-id="cedb2-125">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-125">Name</span></span> | <span data-ttu-id="cedb2-126">Típus</span><span class="sxs-lookup"><span data-stu-id="cedb2-126">Type</span></span> | <span data-ttu-id="cedb2-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cedb2-128">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="cedb2-128">**Name**</span></span><br/><br/> <span data-ttu-id="cedb2-129">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-129">Required</span></span> |<span data-ttu-id="cedb2-130">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-130">**xs:string**</span></span> |<span data-ttu-id="cedb2-131">hello media eszköz fájl neve.</span><span class="sxs-lookup"><span data-stu-id="cedb2-131">hello media asset file name.</span></span> |
| <span data-ttu-id="cedb2-132">**Méret**</span><span class="sxs-lookup"><span data-stu-id="cedb2-132">**Size**</span></span><br/><br/> <span data-ttu-id="cedb2-133">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-133">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-134">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-134">Required</span></span> |<span data-ttu-id="cedb2-135">**xs:Long**</span><span class="sxs-lookup"><span data-stu-id="cedb2-135">**xs:long**</span></span> |<span data-ttu-id="cedb2-136">Mérete bájtban hello objektumfájlt.</span><span class="sxs-lookup"><span data-stu-id="cedb2-136">Size of hello asset file in bytes.</span></span> |
| <span data-ttu-id="cedb2-137">**Időtartam**</span><span class="sxs-lookup"><span data-stu-id="cedb2-137">**Duration**</span></span><br/><br/> <span data-ttu-id="cedb2-138">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-138">Required</span></span> |<span data-ttu-id="cedb2-139">**DURATION típusú**</span><span class="sxs-lookup"><span data-stu-id="cedb2-139">**xs:duration**</span></span> |<span data-ttu-id="cedb2-140">Tartalom play hátsó időtartama.</span><span class="sxs-lookup"><span data-stu-id="cedb2-140">Content play back duration.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="cedb2-141">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="cedb2-141">Child elements</span></span>
| <span data-ttu-id="cedb2-142">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-142">Name</span></span> | <span data-ttu-id="cedb2-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-143">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cedb2-144">**Adatforrások**</span><span class="sxs-lookup"><span data-stu-id="cedb2-144">**Sources**</span></span> |<span data-ttu-id="cedb2-145">Feldolgozott gyűjtemény bemeneti/media forrásfájljainak, a rendezés tooproduce a AssetFile.</span><span class="sxs-lookup"><span data-stu-id="cedb2-145">Collection of input/source media files, that was processed in order tooproduce this AssetFile.</span></span> <span data-ttu-id="cedb2-146">További információkért lásd: [Forráselem](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cedb2-146">For more information, see [Source element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="cedb2-147">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="cedb2-147">**VideoTracks**</span></span><br/><br/> <span data-ttu-id="cedb2-148">minOccurs = "0" maxOccurs = "1"</span><span class="sxs-lookup"><span data-stu-id="cedb2-148">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="cedb2-149">Minden egyes fizikai AssetFile nulla vagy több videó nyomon követi a megfelelő tárolót formátumra időosztásos azt is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cedb2-149">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="cedb2-150">Ez az adott videó nyomon követi hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="cedb2-150">This is hello collection of all those video tracks.</span></span> <span data-ttu-id="cedb2-151">További információkért lásd: [VideoTracks elem](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cedb2-151">For more information, see [VideoTracks element](media-services-output-metadata-schema.md).</span></span> |
| <span data-ttu-id="cedb2-152">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="cedb2-152">**AudioTracks**</span></span><br/><br/> <span data-ttu-id="cedb2-153">minOccurs = "0" maxOccurs = "1"</span><span class="sxs-lookup"><span data-stu-id="cedb2-153">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="cedb2-154">Minden fizikai AssetFile azt egy megfelelő tárolót formátumra időosztásos nulla vagy több zeneszámok tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cedb2-154">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="cedb2-155">Ez az összes adott zeneszámok hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="cedb2-155">This is hello collection of all those audio tracks.</span></span> <span data-ttu-id="cedb2-156">További információkért lásd: [AudioTracks elem](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cedb2-156">For more information, see [AudioTracks element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="cedb2-157"><a name="Sources "></a>Adatforrások elem</span><span class="sxs-lookup"><span data-stu-id="cedb2-157"><a name="Sources "></a> Sources element</span></span>
<span data-ttu-id="cedb2-158">Feldolgozott gyűjtemény bemeneti/media forrásfájljainak, a rendezés tooproduce a AssetFile.</span><span class="sxs-lookup"><span data-stu-id="cedb2-158">Collection of input/source media files, that was processed in order tooproduce this AssetFile.</span></span>  

<span data-ttu-id="cedb2-159">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-159">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="cedb2-160">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="cedb2-160">Child elements</span></span>
| <span data-ttu-id="cedb2-161">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-161">Name</span></span> | <span data-ttu-id="cedb2-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cedb2-163">**Forrás**</span><span class="sxs-lookup"><span data-stu-id="cedb2-163">**Source**</span></span><br/><br/> <span data-ttu-id="cedb2-164">minOccurs = "1" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="cedb2-164">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="cedb2-165">Egy bemeneti/forrásfájl Ez az eszköz létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="cedb2-165">An input/source file used when generating this asset.</span></span> <span data-ttu-id="cedb2-166">További információ: [Forráselem](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cedb2-166">For more information see [Source element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="cedb2-167"><a name="Source "></a>Adatforrás</span><span class="sxs-lookup"><span data-stu-id="cedb2-167"><a name="Source "></a> Source element</span></span>
<span data-ttu-id="cedb2-168">Egy bemeneti/forrásfájl Ez az eszköz létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="cedb2-168">An input/source file used when generating this asset.</span></span>  

<span data-ttu-id="cedb2-169">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-169">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="cedb2-170">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="cedb2-170">Attributes</span></span>
| <span data-ttu-id="cedb2-171">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-171">Name</span></span> | <span data-ttu-id="cedb2-172">Típus</span><span class="sxs-lookup"><span data-stu-id="cedb2-172">Type</span></span> | <span data-ttu-id="cedb2-173">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-173">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cedb2-174">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="cedb2-174">**Name**</span></span><br/><br/> <span data-ttu-id="cedb2-175">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-175">Required</span></span> |<span data-ttu-id="cedb2-176">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-176">**xs:string**</span></span> |<span data-ttu-id="cedb2-177">Bemeneti forrásfájl neve.</span><span class="sxs-lookup"><span data-stu-id="cedb2-177">Input source file name.</span></span> |

## <span data-ttu-id="cedb2-178"><a name="VideoTracks "></a>VideoTracks elem</span><span class="sxs-lookup"><span data-stu-id="cedb2-178"><a name="VideoTracks "></a> VideoTracks element</span></span>
<span data-ttu-id="cedb2-179">Minden egyes fizikai AssetFile nulla vagy több videó nyomon követi a megfelelő tárolót formátumra időosztásos azt is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cedb2-179">Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="cedb2-180">Ez az adott videó nyomon követi hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="cedb2-180">This is hello collection of all those video tracks.</span></span>  

<span data-ttu-id="cedb2-181">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-181">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="cedb2-182">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="cedb2-182">Child elements</span></span>
| <span data-ttu-id="cedb2-183">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-183">Name</span></span> | <span data-ttu-id="cedb2-184">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-184">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cedb2-185">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="cedb2-185">**VideoTrack**</span></span><br/><br/> <span data-ttu-id="cedb2-186">minOccurs = "1" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="cedb2-186">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="cedb2-187">Egy adott videó hello szülő AssetFile nyomon.</span><span class="sxs-lookup"><span data-stu-id="cedb2-187">A specific video track in hello parent AssetFile.</span></span> <span data-ttu-id="cedb2-188">További információkért lásd: [VideoTrack elem](media-services-output-metadata-schema.md#VideoTrack).</span><span class="sxs-lookup"><span data-stu-id="cedb2-188">For more information, see [VideoTrack element](media-services-output-metadata-schema.md#VideoTrack).</span></span> |

## <span data-ttu-id="cedb2-189"><a name="VideoTrack"></a>VideoTrack elem</span><span class="sxs-lookup"><span data-stu-id="cedb2-189"><a name="VideoTrack"></a> VideoTrack element</span></span>
<span data-ttu-id="cedb2-190">Egy adott videó hello szülő AssetFile nyomon.</span><span class="sxs-lookup"><span data-stu-id="cedb2-190">A specific video track in hello parent AssetFile.</span></span>  

<span data-ttu-id="cedb2-191">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-191">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="cedb2-192">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="cedb2-192">Attributes</span></span>
| <span data-ttu-id="cedb2-193">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-193">Name</span></span> | <span data-ttu-id="cedb2-194">Típus</span><span class="sxs-lookup"><span data-stu-id="cedb2-194">Type</span></span> | <span data-ttu-id="cedb2-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-195">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cedb2-196">**Azonosítója**</span><span class="sxs-lookup"><span data-stu-id="cedb2-196">**Id**</span></span><br/><br/> <span data-ttu-id="cedb2-197">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-197">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-198">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-198">Required</span></span> |<span data-ttu-id="cedb2-199">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-199">**xs:int**</span></span> |<span data-ttu-id="cedb2-200">Ez a videó track nulla alapú indexét. **Megjegyzés:** ez nem szükségszerűen hello TrackID használt MP4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="cedb2-200">Zero-based index of this video track. **Note:**  This is not necessarily hello TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="cedb2-201">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="cedb2-201">**FourCC**</span></span><br/><br/> <span data-ttu-id="cedb2-202">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-202">Required</span></span> |<span data-ttu-id="cedb2-203">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-203">**xs:string**</span></span> |<span data-ttu-id="cedb2-204">Videó kodek FourCC kódot.</span><span class="sxs-lookup"><span data-stu-id="cedb2-204">Video codec FourCC code.</span></span> |
| <span data-ttu-id="cedb2-205">**Profil**</span><span class="sxs-lookup"><span data-stu-id="cedb2-205">**Profile**</span></span> |<span data-ttu-id="cedb2-206">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-206">**xs:string**</span></span> |<span data-ttu-id="cedb2-207">H264 profil (csak a megfelelő tooH264 kodek).</span><span class="sxs-lookup"><span data-stu-id="cedb2-207">H264 profile (only applicable tooH264 codec).</span></span> |
| <span data-ttu-id="cedb2-208">**Szint**</span><span class="sxs-lookup"><span data-stu-id="cedb2-208">**Level**</span></span> |<span data-ttu-id="cedb2-209">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-209">**xs:string**</span></span> |<span data-ttu-id="cedb2-210">H264 szint (csak a megfelelő tooH264 kodek).</span><span class="sxs-lookup"><span data-stu-id="cedb2-210">H264 level (only applicable tooH264 codec).</span></span> |
| <span data-ttu-id="cedb2-211">**Szélessége**</span><span class="sxs-lookup"><span data-stu-id="cedb2-211">**Width**</span></span><br/><br/> <span data-ttu-id="cedb2-212">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-212">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-213">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-213">Required</span></span> |<span data-ttu-id="cedb2-214">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-214">**xs:int**</span></span> |<span data-ttu-id="cedb2-215">Kódolt videó szélessége képpontban megadva.</span><span class="sxs-lookup"><span data-stu-id="cedb2-215">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="cedb2-216">**Magassága**</span><span class="sxs-lookup"><span data-stu-id="cedb2-216">**Height**</span></span><br/><br/> <span data-ttu-id="cedb2-217">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-217">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-218">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-218">Required</span></span> |<span data-ttu-id="cedb2-219">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-219">**xs:int**</span></span> |<span data-ttu-id="cedb2-220">Videó magassága képpontban kódolva.</span><span class="sxs-lookup"><span data-stu-id="cedb2-220">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="cedb2-221">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="cedb2-221">**DisplayAspectRatioNumerator**</span></span><br/><br/> <span data-ttu-id="cedb2-222">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-222">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-223">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-223">Required</span></span> |<span data-ttu-id="cedb2-224">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="cedb2-224">**xs:double**</span></span> |<span data-ttu-id="cedb2-225">Képmegjelenítő oldalarányának számlálójának.</span><span class="sxs-lookup"><span data-stu-id="cedb2-225">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="cedb2-226">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="cedb2-226">**DisplayAspectRatioDenominator**</span></span><br/><br/> <span data-ttu-id="cedb2-227">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-227">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-228">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-228">Required</span></span> |<span data-ttu-id="cedb2-229">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="cedb2-229">**xs:double**</span></span> |<span data-ttu-id="cedb2-230">Képmegjelenítő oldalarányának nevező.</span><span class="sxs-lookup"><span data-stu-id="cedb2-230">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="cedb2-231">**Képkockasebességhez**</span><span class="sxs-lookup"><span data-stu-id="cedb2-231">**Framerate**</span></span><br/><br/> <span data-ttu-id="cedb2-232">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-232">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-233">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-233">Required</span></span> |<span data-ttu-id="cedb2-234">**xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="cedb2-234">**xs:decimal**</span></span> |<span data-ttu-id="cedb2-235">Mért videó képkockasebessége .3f formátumban.</span><span class="sxs-lookup"><span data-stu-id="cedb2-235">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="cedb2-236">**TargetFramerate**</span><span class="sxs-lookup"><span data-stu-id="cedb2-236">**TargetFramerate**</span></span><br/><br/> <span data-ttu-id="cedb2-237">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-237">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-238">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-238">Required</span></span> |<span data-ttu-id="cedb2-239">**xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="cedb2-239">**xs:decimal**</span></span> |<span data-ttu-id="cedb2-240">Előre definiált cél videó képkockasebessége .3f formátumban.</span><span class="sxs-lookup"><span data-stu-id="cedb2-240">Preset target video frame rate in .3f format.</span></span> |
| <span data-ttu-id="cedb2-241">**Átviteli sebesség**</span><span class="sxs-lookup"><span data-stu-id="cedb2-241">**Bitrate**</span></span><br/><br/> <span data-ttu-id="cedb2-242">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-242">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-243">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-243">Required</span></span> |<span data-ttu-id="cedb2-244">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-244">**xs:int**</span></span> |<span data-ttu-id="cedb2-245">Kilobit / másodperc, a hello AssetFile kiszámított videó átlagos átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="cedb2-245">Average video bit rate in kilobits per second, as calculated from hello AssetFile.</span></span> <span data-ttu-id="cedb2-246">Csak a hello elemi adatfolyam tartalmat megjeleníti, és nem tartalmaz hello csomagolás terhelés.</span><span class="sxs-lookup"><span data-stu-id="cedb2-246">Counts only hello elementary stream payload, and does not include hello packaging overhead.</span></span> |
| <span data-ttu-id="cedb2-247">**TargetBitrate**</span><span class="sxs-lookup"><span data-stu-id="cedb2-247">**TargetBitrate**</span></span><br/><br/> <span data-ttu-id="cedb2-248">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-248">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-249">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-249">Required</span></span> |<span data-ttu-id="cedb2-250">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-250">**xs:int**</span></span> |<span data-ttu-id="cedb2-251">A videó nyomon követése, az átlagos sávszélességű TARGET kért keresztül hello kódolási beállításkészlet a kilobit / másodperc.</span><span class="sxs-lookup"><span data-stu-id="cedb2-251">Target average bitrate for this video track, as requested via hello encoding preset, in kilobits per second.</span></span> |
| <span data-ttu-id="cedb2-252">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="cedb2-252">**MaxGOPBitrate**</span></span><br/><br/> <span data-ttu-id="cedb2-253">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-253">minInclusive ="0"</span></span> |<span data-ttu-id="cedb2-254">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-254">**xs:int**</span></span> |<span data-ttu-id="cedb2-255">Maximális GOP a videó nyomon követése, a kilobit / másodperc átlagos sávszélességű.</span><span class="sxs-lookup"><span data-stu-id="cedb2-255">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |

## <span data-ttu-id="cedb2-256"><a name="AudioTracks "></a>AudioTracks elem</span><span class="sxs-lookup"><span data-stu-id="cedb2-256"><a name="AudioTracks "></a> AudioTracks element</span></span>
<span data-ttu-id="cedb2-257">Minden fizikai AssetFile azt egy megfelelő tárolót formátumra időosztásos nulla vagy több zeneszámok tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cedb2-257">Each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="cedb2-258">Ez az összes adott zeneszámok hello gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="cedb2-258">This is hello collection of all those audio tracks.</span></span>  

<span data-ttu-id="cedb2-259">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-259">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="cedb2-260">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="cedb2-260">Child elements</span></span>
| <span data-ttu-id="cedb2-261">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-261">Name</span></span> | <span data-ttu-id="cedb2-262">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-262">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cedb2-263">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="cedb2-263">**AudioTrack**</span></span><br/><br/> <span data-ttu-id="cedb2-264">minOccurs = "1" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="cedb2-264">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="cedb2-265">Egy adott hang hello szülő AssetFile nyomon.</span><span class="sxs-lookup"><span data-stu-id="cedb2-265">A specific audio track in hello parent AssetFile.</span></span> <span data-ttu-id="cedb2-266">További információkért lásd: [AudioTrack elem](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cedb2-266">For more information, see [AudioTrack element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="cedb2-267"><a name="AudioTrack "></a>AudioTrack elem</span><span class="sxs-lookup"><span data-stu-id="cedb2-267"><a name="AudioTrack "></a> AudioTrack element</span></span>
<span data-ttu-id="cedb2-268">Egy adott hang hello szülő AssetFile nyomon.</span><span class="sxs-lookup"><span data-stu-id="cedb2-268">A specific audio track in hello parent AssetFile.</span></span>  

<span data-ttu-id="cedb2-269">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-269">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="cedb2-270">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="cedb2-270">Attributes</span></span>
| <span data-ttu-id="cedb2-271">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-271">Name</span></span> | <span data-ttu-id="cedb2-272">Típus</span><span class="sxs-lookup"><span data-stu-id="cedb2-272">Type</span></span> | <span data-ttu-id="cedb2-273">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-273">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cedb2-274">**Azonosítója**</span><span class="sxs-lookup"><span data-stu-id="cedb2-274">**Id**</span></span><br/><br/> <span data-ttu-id="cedb2-275">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-275">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-276">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-276">Required</span></span> |<span data-ttu-id="cedb2-277">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-277">**xs:int**</span></span> |<span data-ttu-id="cedb2-278">A hang követése nulla alapú indexét. **Megjegyzés:** ez nem szükségszerűen hello TrackID használt MP4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="cedb2-278">Zero-based index of this audio track. **Note:**  This is not necessarily hello TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="cedb2-279">**Kodek**</span><span class="sxs-lookup"><span data-stu-id="cedb2-279">**Codec**</span></span> |<span data-ttu-id="cedb2-280">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-280">**xs:string**</span></span> |<span data-ttu-id="cedb2-281">Zenei kodek karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="cedb2-281">Audio track codec string.</span></span> |
| <span data-ttu-id="cedb2-282">**EncoderVersion**</span><span class="sxs-lookup"><span data-stu-id="cedb2-282">**EncoderVersion**</span></span> |<span data-ttu-id="cedb2-283">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-283">**xs:string**</span></span> |<span data-ttu-id="cedb2-284">Nem kötelező kódoló verzió-karakterláncnak, EAC3 szükséges.</span><span class="sxs-lookup"><span data-stu-id="cedb2-284">Optional encoder version string, required for EAC3.</span></span> |
| <span data-ttu-id="cedb2-285">**Csatornák**</span><span class="sxs-lookup"><span data-stu-id="cedb2-285">**Channels**</span></span><br/><br/> <span data-ttu-id="cedb2-286">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-286">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-287">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-287">Required</span></span> |<span data-ttu-id="cedb2-288">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-288">**xs:int**</span></span> |<span data-ttu-id="cedb2-289">A hang csatornák számát.</span><span class="sxs-lookup"><span data-stu-id="cedb2-289">Number of audio channels.</span></span> |
| <span data-ttu-id="cedb2-290">**Érvénytelen a SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="cedb2-290">**SamplingRate**</span></span><br/><br/> <span data-ttu-id="cedb2-291">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-291">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-292">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-292">Required</span></span> |<span data-ttu-id="cedb2-293">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-293">**xs:int**</span></span> |<span data-ttu-id="cedb2-294">Hang mintavételi ráta minták másodpercenkénti vagy Hz.</span><span class="sxs-lookup"><span data-stu-id="cedb2-294">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="cedb2-295">**Átviteli sebesség**</span><span class="sxs-lookup"><span data-stu-id="cedb2-295">**Bitrate**</span></span><br/><br/> <span data-ttu-id="cedb2-296">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-296">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-297">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-297">Required</span></span> |<span data-ttu-id="cedb2-298">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-298">**xs:int**</span></span> |<span data-ttu-id="cedb2-299">Bit / másodperc, a hello AssetFile kiszámított átlagos átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="cedb2-299">Average audio bit rate in bits per second, as calculated from hello AssetFile.</span></span> <span data-ttu-id="cedb2-300">Csak a hello elemi adatfolyam tartalmat megjeleníti, és nem tartalmaz hello csomagolás terhelés.</span><span class="sxs-lookup"><span data-stu-id="cedb2-300">Counts only hello elementary stream payload, and does not include hello packaging overhead.</span></span> |
| <span data-ttu-id="cedb2-301">**A BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="cedb2-301">**BitsPerSample**</span></span><br/><br/> <span data-ttu-id="cedb2-302">minInclusive = "0"</span><span class="sxs-lookup"><span data-stu-id="cedb2-302">minInclusive ="0"</span></span><br/><br/> <span data-ttu-id="cedb2-303">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-303">Required</span></span> |<span data-ttu-id="cedb2-304">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-304">**xs:int**</span></span> |<span data-ttu-id="cedb2-305">Bit / minta hello wFormatTag formátumban írja be.</span><span class="sxs-lookup"><span data-stu-id="cedb2-305">Bits per sample for hello wFormatTag format type.</span></span> |

### <a name="child-elements"></a><span data-ttu-id="cedb2-306">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="cedb2-306">Child elements</span></span>
| <span data-ttu-id="cedb2-307">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-307">Name</span></span> | <span data-ttu-id="cedb2-308">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-308">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cedb2-309">**LoudnessMeteringResultParameters**</span><span class="sxs-lookup"><span data-stu-id="cedb2-309">**LoudnessMeteringResultParameters**</span></span><br/><br/> <span data-ttu-id="cedb2-310">minOccurs = "0" maxOccurs = "1"</span><span class="sxs-lookup"><span data-stu-id="cedb2-310">minOccurs="0" maxOccurs="1"</span></span> |<span data-ttu-id="cedb2-311">Hangerő mérési eredmények paraméterek.</span><span class="sxs-lookup"><span data-stu-id="cedb2-311">Loudness metering result parameters.</span></span> <span data-ttu-id="cedb2-312">További információkért lásd: [LoudnessMeteringResultParameters elem](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="cedb2-312">For more information, see [LoudnessMeteringResultParameters element](media-services-output-metadata-schema.md).</span></span> |

## <span data-ttu-id="cedb2-313"><a name="LoudnessMeteringResultParameters "></a>LoudnessMeteringResultParameters elem</span><span class="sxs-lookup"><span data-stu-id="cedb2-313"><a name="LoudnessMeteringResultParameters "></a> LoudnessMeteringResultParameters element</span></span>
<span data-ttu-id="cedb2-314">Hangerő mérési eredmények paraméterek.</span><span class="sxs-lookup"><span data-stu-id="cedb2-314">Loudness metering result parameters.</span></span>  

<span data-ttu-id="cedb2-315">Az XML-példa található [XML-példa](media-services-output-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="cedb2-315">You can find an XML example [XML example](media-services-output-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="cedb2-316">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="cedb2-316">Attributes</span></span>
| <span data-ttu-id="cedb2-317">Név</span><span class="sxs-lookup"><span data-stu-id="cedb2-317">Name</span></span> | <span data-ttu-id="cedb2-318">Típus</span><span class="sxs-lookup"><span data-stu-id="cedb2-318">Type</span></span> | <span data-ttu-id="cedb2-319">Leírás</span><span class="sxs-lookup"><span data-stu-id="cedb2-319">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cedb2-320">**DPLMVersionInformation**</span><span class="sxs-lookup"><span data-stu-id="cedb2-320">**DPLMVersionInformation**</span></span> |<span data-ttu-id="cedb2-321">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-321">**xs:string**</span></span> |<span data-ttu-id="cedb2-322">**Dolby** szakmai hangerő mérési fejlesztési csomag verzióját.</span><span class="sxs-lookup"><span data-stu-id="cedb2-322">**Dolby** professional loudness metering development kit version.</span></span> |
| <span data-ttu-id="cedb2-323">**DialogNormalization**</span><span class="sxs-lookup"><span data-stu-id="cedb2-323">**DialogNormalization**</span></span><br/><br/> <span data-ttu-id="cedb2-324">minInclusive = "-31" maxInclusive "-1" =</span><span class="sxs-lookup"><span data-stu-id="cedb2-324">minInclusive="-31" maxInclusive="-1"</span></span><br/><br/> <span data-ttu-id="cedb2-325">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-325">Required</span></span> |<span data-ttu-id="cedb2-326">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="cedb2-326">**xs:int**</span></span> |<span data-ttu-id="cedb2-327">DPLM, kötelező, ha a LoudnessMetering beállítása során generált DialogNormalization</span><span class="sxs-lookup"><span data-stu-id="cedb2-327">DialogNormalization generated through DPLM, required when LoudnessMetering is set</span></span> |
| <span data-ttu-id="cedb2-328">**IntegratedLoudness**</span><span class="sxs-lookup"><span data-stu-id="cedb2-328">**IntegratedLoudness**</span></span><br/><br/> <span data-ttu-id="cedb2-329">minInclusive = "-70" maxInclusive "10" =</span><span class="sxs-lookup"><span data-stu-id="cedb2-329">minInclusive="-70" maxInclusive="10"</span></span><br/><br/> <span data-ttu-id="cedb2-330">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-330">Required</span></span> |<span data-ttu-id="cedb2-331">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="cedb2-331">**xs:float**</span></span> |<span data-ttu-id="cedb2-332">Integrált hangerő</span><span class="sxs-lookup"><span data-stu-id="cedb2-332">Integrated loudness</span></span> |
| <span data-ttu-id="cedb2-333">**IntegratedLoudnessUnit**</span><span class="sxs-lookup"><span data-stu-id="cedb2-333">**IntegratedLoudnessUnit**</span></span><br/><br/> <span data-ttu-id="cedb2-334">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-334">Required</span></span> |<span data-ttu-id="cedb2-335">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-335">**xs:string**</span></span> |<span data-ttu-id="cedb2-336">Integrált hangerő egység.</span><span class="sxs-lookup"><span data-stu-id="cedb2-336">Integrated loudness unit.</span></span> |
| <span data-ttu-id="cedb2-337">**IntegratedLoudnessGatingMethod**</span><span class="sxs-lookup"><span data-stu-id="cedb2-337">**IntegratedLoudnessGatingMethod**</span></span><br/><br/> <span data-ttu-id="cedb2-338">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-338">Required</span></span> |<span data-ttu-id="cedb2-339">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="cedb2-339">**xs:string**</span></span> |<span data-ttu-id="cedb2-340">Átjáró azonosítója</span><span class="sxs-lookup"><span data-stu-id="cedb2-340">Gating identifier</span></span> |
| <span data-ttu-id="cedb2-341">**IntegratedLoudnessSpeechPercentage**</span><span class="sxs-lookup"><span data-stu-id="cedb2-341">**IntegratedLoudnessSpeechPercentage**</span></span><br/><br/> <span data-ttu-id="cedb2-342">minInclusive = "0" maxInclusive "100" =</span><span class="sxs-lookup"><span data-stu-id="cedb2-342">minInclusive ="0" maxInclusive="100"</span></span> |<span data-ttu-id="cedb2-343">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="cedb2-343">**xs:float**</span></span> |<span data-ttu-id="cedb2-344">Beszéd tartalom hello program százalékos keresztül.</span><span class="sxs-lookup"><span data-stu-id="cedb2-344">Speech content over hello program, as a percentage.</span></span> |
| <span data-ttu-id="cedb2-345">**SamplePeak**</span><span class="sxs-lookup"><span data-stu-id="cedb2-345">**SamplePeak**</span></span><br/><br/> <span data-ttu-id="cedb2-346">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-346">Required</span></span> |<span data-ttu-id="cedb2-347">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="cedb2-347">**xs:float**</span></span> |<span data-ttu-id="cedb2-348">A minta abszolút értéke, visszaállítás óta, vagy utolsó óta törölve lett, csatornánként.</span><span class="sxs-lookup"><span data-stu-id="cedb2-348">Peak absolute sample value, since reset or since it was last cleared, per channel.</span></span>  <span data-ttu-id="cedb2-349">Egységek dBFS.</span><span class="sxs-lookup"><span data-stu-id="cedb2-349">Units are dBFS.</span></span> |
| <span data-ttu-id="cedb2-350">**SamplePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="cedb2-350">**SamplePeakUnit**</span></span><br/><br/> <span data-ttu-id="cedb2-351">rögzített = "dBFS"</span><span class="sxs-lookup"><span data-stu-id="cedb2-351">fixed="dBFS"</span></span><br/><br/> <span data-ttu-id="cedb2-352">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-352">Required</span></span> |<span data-ttu-id="cedb2-353">**xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="cedb2-353">**xs:anySimpleType**</span></span> |<span data-ttu-id="cedb2-354">A minta csúcs egység.</span><span class="sxs-lookup"><span data-stu-id="cedb2-354">Sample peak unit.</span></span> |
| <span data-ttu-id="cedb2-355">**TruePeak**</span><span class="sxs-lookup"><span data-stu-id="cedb2-355">**TruePeak**</span></span><br/><br/> <span data-ttu-id="cedb2-356">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-356">Required</span></span> |<span data-ttu-id="cedb2-357">**xs:float**</span><span class="sxs-lookup"><span data-stu-id="cedb2-357">**xs:float**</span></span> |<span data-ttu-id="cedb2-358">Maximális true értéke, szerint ITU-R BS.1770-2, visszaállítás óta, vagy utolsó óta csatornánként bejelölve.</span><span class="sxs-lookup"><span data-stu-id="cedb2-358">Maximum true peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.</span></span> <span data-ttu-id="cedb2-359">Egységek dBTP.</span><span class="sxs-lookup"><span data-stu-id="cedb2-359">Units are dBTP.</span></span> |
| <span data-ttu-id="cedb2-360">**TruePeakUnit**</span><span class="sxs-lookup"><span data-stu-id="cedb2-360">**TruePeakUnit**</span></span><br/><br/> <span data-ttu-id="cedb2-361">rögzített = "dBTP"</span><span class="sxs-lookup"><span data-stu-id="cedb2-361">fixed="dBTP"</span></span><br/><br/> <span data-ttu-id="cedb2-362">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cedb2-362">Required</span></span> |<span data-ttu-id="cedb2-363">**xs:anySimpleType**</span><span class="sxs-lookup"><span data-stu-id="cedb2-363">**xs:anySimpleType**</span></span> |<span data-ttu-id="cedb2-364">Igaz csúcs egység.</span><span class="sxs-lookup"><span data-stu-id="cedb2-364">True peak unit.</span></span> |

## <a name="schema-code"></a><span data-ttu-id="cedb2-365">Séma kódot</span><span class="sxs-lookup"><span data-stu-id="cedb2-365">Schema Code</span></span>
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.2"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata"  
               elementFormDefault="qualified">  
      <xs:element name="AssetFiles">  
        <xs:annotation>  
          <xs:documentation>Collection of AssetFile entries for hello encoding job</xs:documentation>  
        </xs:annotation>  
        <xs:complexType>  
          <xs:sequence>  
            <xs:element name="AssetFile" minOccurs="1" maxOccurs="unbounded">  
              <xs:annotation>  
                <xs:documentation>asset file</xs:documentation>  
              </xs:annotation>  
              <xs:complexType>  
                <xs:sequence>  
                  <xs:element name="Sources">  
                    <xs:annotation>  
                      <xs:documentation>Collection of input/source media files, that was processed in order tooproduce this AssetFile</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Source" minOccurs="1" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>An input/source file used when generating this asset</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Name" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>input source file name</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is hello collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>A specific video track in hello parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this video track. Note: this is not necessarily hello TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="FourCC" type="xs:string" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video codec FourCC code</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Profile" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 profile (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Level" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>H264 level (only appliable for H264 codec)</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Width" use="required">  
                              <xs:annotation>  
                                <xs:documentation>encoded video width in pixels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Height" use="required">  
                              <xs:annotation>  
                                <xs:documentation>encoded video height in pixels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="DisplayAspectRatioNumerator" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video display aspect ratio numerator</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:double">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="DisplayAspectRatioDenominator" use="required">  
                              <xs:annotation>  
                                <xs:documentation>video display aspect ratio denominator</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:double">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Framerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>measured video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetFramerate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>preset target video frame rate in .3f format</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:decimal">  
                                  <xs:minInclusive value="0"/>  
                                  <xs:fractionDigits value="3"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average video bit rate in kilobits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="TargetBitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>target average bitrate for this video track, as requested via hello encoding preset, in kilobits per second</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="MaxGOPBitrate">  
                              <xs:annotation>  
                                <xs:documentation>Max GOP average bitrate for this video track, in kilobits per second</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is hello collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" maxOccurs="unbounded">  
                          <xs:annotation>  
                            <xs:documentation>a specific audio track in hello parent AssetFile</xs:documentation>  
                          </xs:annotation>  
                          <xs:complexType>  
                            <xs:sequence>  
                              <xs:element name="LoudnessMeteringResultParameters" minOccurs="0" maxOccurs="1">  
                                <xs:annotation>  
                                  <xs:documentation>Loudness Metering Result Parameters</xs:documentation>  
                                </xs:annotation>  
                                <xs:complexType>  
                                  <xs:attribute name="DPLMVersionInformation" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Dolby Professional Loudness Metering Development Kit Version</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="DialogNormalization" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation> DialogNormalization generated through DPLM, required when LoudnessMetering is set</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:int">  
                                        <xs:minInclusive value="-31"/>  
                                        <xs:maxInclusive value="-1"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudness" use="required">  
                                    <xs:annotation>  
                                      <xs:documentation>Integrated loudness</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="-70"/>  
                                        <xs:maxInclusive value="10"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessUnit" use="required" type="xs:string">  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessGatingMethod" use="required" type="xs:string">  
                                    <xs:annotation>  
                                      <xs:documentation>Gating identifier</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="IntegratedLoudnessSpeechPercentage">  
                                    <xs:annotation>  
                                      <xs:documentation>Speech content over hello program, as a percentage.</xs:documentation>  
                                    </xs:annotation>  
                                    <xs:simpleType>  
                                      <xs:restriction base="xs:float">  
                                        <xs:minInclusive value="0"/>  
                                        <xs:maxInclusive value="100"/>  
                                      </xs:restriction>  
                                    </xs:simpleType>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Peak absolute sample value, since reset or since it was last cleared, per channel.  Units are dBFS.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="SamplePeakUnit" use="required" fixed="dBFS">  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeak" use="required" type="xs:float">  
                                    <xs:annotation>  
                                      <xs:documentation>Maximum True Peak value, as per ITU-R BS.1770-2, since reset or since it was last cleared, per channel.  Units are dBTP.</xs:documentation>  
                                    </xs:annotation>  
                                  </xs:attribute>  
                                  <xs:attribute name="TruePeakUnit" use="required" fixed="dBTP">  
                                  </xs:attribute>  
                                </xs:complexType>  
                              </xs:element>  
                            </xs:sequence>  
                            <xs:attribute name="Id" use="required">  
                              <xs:annotation>  
                                <xs:documentation>zero-based index of this audio track. Note: this is not necessarily hello TrackID as used in an MP4 file</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Codec" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>audio track codec string</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="EncoderVersion" type="xs:string">  
                              <xs:annotation>  
                                <xs:documentation>optional encoder version string, required for EAC3</xs:documentation>  
                              </xs:annotation>  
                            </xs:attribute>  
                            <xs:attribute name="Channels" use="required">  
                              <xs:annotation>  
                                <xs:documentation>number of audio channels</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="SamplingRate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>audio sampling rate in samples/sec or Hz</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="Bitrate" use="required">  
                              <xs:annotation>  
                                <xs:documentation>average audio bit rate in bits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                            <xs:attribute name="BitsPerSample" use="required">  
                              <xs:annotation>  
                                <xs:documentation>Bits per sample for hello wFormatTag format type</xs:documentation>  
                              </xs:annotation>  
                              <xs:simpleType>  
                                <xs:restriction base="xs:int">  
                                  <xs:minInclusive value="0"/>  
                                </xs:restriction>  
                              </xs:simpleType>  
                            </xs:attribute>  
                          </xs:complexType>  
                        </xs:element>  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                </xs:sequence>  
                <xs:attribute name="Name" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>hello media asset file name</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="Size" use="required">  
                  <xs:annotation>  
                    <xs:documentation>size of file in bytes</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:long">  
                      <xs:minInclusive value="0"/>  
                    </xs:restriction>  
                  </xs:simpleType>  
                </xs:attribute>  
                <xs:attribute name="Duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration</xs:documentation>  
                  </xs:annotation>  
                  <xs:simpleType>  
                    <xs:restriction base="xs:duration"/>  
                  </xs:simpleType>  
                </xs:attribute>  
              </xs:complexType>  
            </xs:element>  
          </xs:sequence>  
        </xs:complexType>  
      </xs:element>  
    </xs:schema>  



## <span data-ttu-id="cedb2-366"><a name="xml"></a>XML-példa</span><span class="sxs-lookup"><span data-stu-id="cedb2-366"><a name="xml"></a> XML example</span></span>
 <span data-ttu-id="cedb2-367">hello hello kimeneti metaadatait tartalmazó fájl egy példa látható.</span><span class="sxs-lookup"><span data-stu-id="cedb2-367">hello following is an example of hello Output metadata file.</span></span>  

    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
                xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2013/05/mediaencoder/metadata">  
      <AssetFile Name="BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4" Size="4646283" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.2" Width="1280" Height="720" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="4250" TargetBitrate="3400" MaxGOPBitrate="5514"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4" Size="3166728" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="2846" TargetBitrate="2250" MaxGOPBitrate="3630"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4" Size="2205095" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.1" Width="960" Height="540" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1932" TargetBitrate="1500" MaxGOPBitrate="2513"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4" Size="1508567" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="1271" TargetBitrate="1000" MaxGOPBitrate="1527"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4" Size="1057155" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="3.0" Width="640" Height="360" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="843" TargetBitrate="650" MaxGOPBitrate="1086"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4" Size="699262" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <VideoTracks>  
          <VideoTrack Id="0" FourCC="AVC1" Profile="Main" Level="1.3" Width="320" Height="180" DisplayAspectRatioNumerator="16" DisplayAspectRatioDenominator="9" Framerate="23.974" TargetFramerate="23.974" Bitrate="503" TargetBitrate="400" MaxGOPBitrate="661"/>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_96kbps.mp4" Size="166780" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="93" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
      <AssetFile Name="BigBuckBunny_AAC_und_ch2_56kbps.mp4" Size="124576" Duration="PT8.4288444S">  
        <Sources>  
          <Source Name="BigBuckBunny.mp4"/>  
        </Sources>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="AacLc" Channels="2" SamplingRate="44100" Bitrate="53" BitsPerSample="16"/>  
        </AudioTracks>  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a><span data-ttu-id="cedb2-368">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cedb2-368">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="cedb2-369">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="cedb2-369">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

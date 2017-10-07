---
title: "aaaAzure Media Services bemeneti metaadatok séma |} Microsoft Docs"
description: "hello a témakör áttekintést nyújt az Azure Media Services bemeneti metaadatok séma."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: d72848e2-4b65-4c84-94bc-e2a90a6e7f47
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 9b72c6ff317aa98451ea75548465dc6023b44a55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="input-metadata"></a><span data-ttu-id="7645f-103">Bemeneti metaadatok</span><span class="sxs-lookup"><span data-stu-id="7645f-103">Input Metadata</span></span>
<span data-ttu-id="7645f-104">A kódolási feladat vagy társítva egy bemeneti eszköz (eszközök) kívánja tooperform egyes kódolási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="7645f-104">An encoding job is associated with an input asset (or assets) on which you want tooperform some encoding tasks.</span></span>  <span data-ttu-id="7645f-105">Létrehozása után egy feladatot kimeneti eszközként hozott létre.</span><span class="sxs-lookup"><span data-stu-id="7645f-105">Upon completion of a task, an output asset is produced.</span></span>  <span data-ttu-id="7645f-106">hello kimeneti eszköz tartalmaz, illetve hang-, videoindexképek, jegyzék, stb. hello kimeneti adategységen is tartalmaz hello bemeneti eszköz metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="7645f-106">hello output asset contains video, audio, thumbnails, manifest, etc. hello output asset also contains a file with metadata about hello input asset.</span></span> <span data-ttu-id="7645f-107">hello hello metaadatok XML-fájl neve van hello a következő formátumban: &lt;asset_id&gt;_metadata.xml (például 41114ad3 eb5e - 4 - c 57-8d 92-5354e2b7d4a4_metadata.xml), ahol &lt;asset_id&gt; hello AssetId hello bemeneti eszköz értéke.</span><span class="sxs-lookup"><span data-stu-id="7645f-107">hello name of hello metadata XML file has hello following format: &lt;asset_id&gt;_metadata.xml (for example, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), where &lt;asset_id&gt; is hello AssetId value of hello input asset.</span></span>  

<span data-ttu-id="7645f-108">Ha azt szeretné, hogy tooexamine hello metaadatfájl, létrehozhat egy **SAS** lokátor és a letöltési hello fájl tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7645f-108">If you want tooexamine hello metadata file, you can create a **SAS** locator and download hello file tooyour local computer.</span></span> <span data-ttu-id="7645f-109">Egy példa található hogyan toocreate egy SAS-kereső és töltse le a fájlt [hello Media Services .NET SDK-bővítmények használatával](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7645f-109">You can find an example on how toocreate a SAS locator and download a file  [Using hello Media Services .NET SDK Extensions](media-services-dotnet-get-started.md).</span></span>  

<span data-ttu-id="7645f-110">Ez a témakör ismerteti a mely hello bemeneti metada hello elemek és hello XML-séma típusú (&lt;asset_id&gt;_metadata.xml) alapul.</span><span class="sxs-lookup"><span data-stu-id="7645f-110">This topic discusses hello elements and types of hello XML schema on which hello input metada (&lt;asset_id&gt;_metadata.xml) is based.</span></span>  <span data-ttu-id="7645f-111">Hello kimeneti adategységen vonatkozó metaadatok tartalmazó hello fájllal kapcsolatos információkért lásd: [kimeneti metaadatok](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="7645f-111">For information about hello file that contains metadata about hello output asset, see [Output Metadata](media-services-output-metadata-schema.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="7645f-112">Hello található [séma kód](media-services-input-metadata-schema.md#code) egy [XML-példa](media-services-input-metadata-schema.md#xml) hello Ez a témakör végén.</span><span class="sxs-lookup"><span data-stu-id="7645f-112">You can find hello [Schema Code](media-services-input-metadata-schema.md#code) an [XML example](media-services-input-metadata-schema.md#xml) at hello end of this topic.</span></span>  
> 
> 

## <span data-ttu-id="7645f-113"><a name="AssetFiles"></a>AssetFiles elem (legfelső szintű elem)</span><span class="sxs-lookup"><span data-stu-id="7645f-113"><a name="AssetFiles"></a> AssetFiles element (root element)</span></span>
<span data-ttu-id="7645f-114">Gyűjteményét tartalmazza [AssetFile elem](media-services-input-metadata-schema.md#AssetFile)s hello kódolási feladat.</span><span class="sxs-lookup"><span data-stu-id="7645f-114">Contains a collection of [AssetFile element](media-services-input-metadata-schema.md#AssetFile)s for hello encoding job.</span></span>  

<span data-ttu-id="7645f-115">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-115">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

| <span data-ttu-id="7645f-116">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-116">Name</span></span> | <span data-ttu-id="7645f-117">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-117">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7645f-118">**AssetFile**</span><span class="sxs-lookup"><span data-stu-id="7645f-118">**AssetFile**</span></span><br /><br /> <span data-ttu-id="7645f-119">minOccurs = "1" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="7645f-119">minOccurs="1" maxOccurs="unbounded"</span></span> |<span data-ttu-id="7645f-120">Egyetlen gyermekelemet.</span><span class="sxs-lookup"><span data-stu-id="7645f-120">A single child element.</span></span> <span data-ttu-id="7645f-121">További információkért lásd: [AssetFile elem](media-services-input-metadata-schema.md#AssetFile).</span><span class="sxs-lookup"><span data-stu-id="7645f-121">For more information, see [AssetFile element](media-services-input-metadata-schema.md#AssetFile).</span></span> |

## <span data-ttu-id="7645f-122"><a name="AssetFile"></a>AssetFile elem</span><span class="sxs-lookup"><span data-stu-id="7645f-122"><a name="AssetFile"></a> AssetFile element</span></span>
 <span data-ttu-id="7645f-123">Attribútumok és elemek leíró egy eszköz-fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7645f-123">Contains attributes and elements that describe an asset file.</span></span>  

 <span data-ttu-id="7645f-124">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-124">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="7645f-125">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="7645f-125">Attributes</span></span>
| <span data-ttu-id="7645f-126">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-126">Name</span></span> | <span data-ttu-id="7645f-127">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-127">Type</span></span> | <span data-ttu-id="7645f-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-128">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-129">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="7645f-129">**Name**</span></span><br /><br /> <span data-ttu-id="7645f-130">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-130">Required</span></span> |<span data-ttu-id="7645f-131">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-131">**xs:string**</span></span> |<span data-ttu-id="7645f-132">Eszköz neve.</span><span class="sxs-lookup"><span data-stu-id="7645f-132">Asset file name.</span></span> |
| <span data-ttu-id="7645f-133">**Méret**</span><span class="sxs-lookup"><span data-stu-id="7645f-133">**Size**</span></span><br /><br /> <span data-ttu-id="7645f-134">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-134">Required</span></span> |<span data-ttu-id="7645f-135">**xs:Long**</span><span class="sxs-lookup"><span data-stu-id="7645f-135">**xs:long**</span></span> |<span data-ttu-id="7645f-136">Mérete bájtban hello objektumfájlt.</span><span class="sxs-lookup"><span data-stu-id="7645f-136">Size of hello asset file in bytes.</span></span> |
| <span data-ttu-id="7645f-137">**Időtartam**</span><span class="sxs-lookup"><span data-stu-id="7645f-137">**Duration**</span></span><br /><br /> <span data-ttu-id="7645f-138">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-138">Required</span></span> |<span data-ttu-id="7645f-139">**DURATION típusú**</span><span class="sxs-lookup"><span data-stu-id="7645f-139">**xs:duration**</span></span> |<span data-ttu-id="7645f-140">Tartalom play hátsó időtartama.</span><span class="sxs-lookup"><span data-stu-id="7645f-140">Content play back duration.</span></span> <span data-ttu-id="7645f-141">Példa: Duration = "PT25M37.757S".</span><span class="sxs-lookup"><span data-stu-id="7645f-141">Example: Duration="PT25M37.757S".</span></span> |
| <span data-ttu-id="7645f-142">**NumberOfStreams**</span><span class="sxs-lookup"><span data-stu-id="7645f-142">**NumberOfStreams**</span></span><br /><br /> <span data-ttu-id="7645f-143">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-143">Required</span></span> |<span data-ttu-id="7645f-144">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-144">**xs:int**</span></span> |<span data-ttu-id="7645f-145">Hello objektumfájlt adatfolyamok száma.</span><span class="sxs-lookup"><span data-stu-id="7645f-145">Number of streams in hello asset file.</span></span> |
| <span data-ttu-id="7645f-146">**FormatNames**</span><span class="sxs-lookup"><span data-stu-id="7645f-146">**FormatNames**</span></span><br /><br /> <span data-ttu-id="7645f-147">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-147">Required</span></span> |<span data-ttu-id="7645f-148">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-148">**xs:string**</span></span> |<span data-ttu-id="7645f-149">Formátumnevek.</span><span class="sxs-lookup"><span data-stu-id="7645f-149">Format names.</span></span> |
| <span data-ttu-id="7645f-150">**FormatVerboseNames**</span><span class="sxs-lookup"><span data-stu-id="7645f-150">**FormatVerboseNames**</span></span><br /><br /> <span data-ttu-id="7645f-151">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-151">Required</span></span> |<span data-ttu-id="7645f-152">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-152">**xs:string**</span></span> |<span data-ttu-id="7645f-153">Részletes formátumnevek.</span><span class="sxs-lookup"><span data-stu-id="7645f-153">Format verbose names.</span></span> |
| <span data-ttu-id="7645f-154">**Kezdő időpont**</span><span class="sxs-lookup"><span data-stu-id="7645f-154">**StartTime**</span></span> |<span data-ttu-id="7645f-155">**DURATION típusú**</span><span class="sxs-lookup"><span data-stu-id="7645f-155">**xs:duration**</span></span> |<span data-ttu-id="7645f-156">Tartalom kezdete.</span><span class="sxs-lookup"><span data-stu-id="7645f-156">Content start time.</span></span> <span data-ttu-id="7645f-157">Példa: StartTime = "PT2.669S".</span><span class="sxs-lookup"><span data-stu-id="7645f-157">Example: StartTime="PT2.669S".</span></span> |
| <span data-ttu-id="7645f-158">**OverallBitRate**</span><span class="sxs-lookup"><span data-stu-id="7645f-158">**OverallBitRate**</span></span> |<span data-ttu-id="7645f-159">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-159">**xs:int**</span></span> |<span data-ttu-id="7645f-160">Hello eszköz fájl kbit/s átlagos átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="7645f-160">Average bitrate of hello asset file in kbps.</span></span> |

> [!NOTE]
> <span data-ttu-id="7645f-161">a következő 4 gyermekelemek hello sorrendben kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="7645f-161">hello following 4 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="7645f-162">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="7645f-162">Child elements</span></span>
| <span data-ttu-id="7645f-163">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-163">Name</span></span> | <span data-ttu-id="7645f-164">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-164">Type</span></span> | <span data-ttu-id="7645f-165">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-165">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-166">**Programok**</span><span class="sxs-lookup"><span data-stu-id="7645f-166">**Programs**</span></span><br /><br /> <span data-ttu-id="7645f-167">minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="7645f-167">minOccurs="0"</span></span> | |<span data-ttu-id="7645f-168">Az összes gyűjtemény [programok elem](media-services-input-metadata-schema.md#Programs) hello objektumfájlt esetén MPEG-TS formátumban.</span><span class="sxs-lookup"><span data-stu-id="7645f-168">Collection of all [Programs element](media-services-input-metadata-schema.md#Programs) when hello asset file is in MPEG-TS format.</span></span> |
| <span data-ttu-id="7645f-169">**VideoTracks**</span><span class="sxs-lookup"><span data-stu-id="7645f-169">**VideoTracks**</span></span><br /><br /> <span data-ttu-id="7645f-170">minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="7645f-170">minOccurs="0"</span></span> | |<span data-ttu-id="7645f-171">Minden egyes fizikai eszköz fájl tartalmazhat nulla vagy több videó nyomon követi a megfelelő tárolót formátumra időosztásos.</span><span class="sxs-lookup"><span data-stu-id="7645f-171">Each physical asset file can contain zero or more video tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="7645f-172">Ez az elem tartalmazza az összes [VideoTracks elem](media-services-input-metadata-schema.md#VideoTracks) hello objektumfájlt részét képező.</span><span class="sxs-lookup"><span data-stu-id="7645f-172">This element contains a collection of all [VideoTracks element](media-services-input-metadata-schema.md#VideoTracks) that are part of hello asset file.</span></span> |
| <span data-ttu-id="7645f-173">**AudioTracks**</span><span class="sxs-lookup"><span data-stu-id="7645f-173">**AudioTracks**</span></span><br /><br /> <span data-ttu-id="7645f-174">minOccurs = "0"</span><span class="sxs-lookup"><span data-stu-id="7645f-174">minOccurs="0"</span></span> | |<span data-ttu-id="7645f-175">Minden egyes fizikai eszköz fájl tartalmazhat nulla vagy több zeneszámok egy megfelelő tárolót formátumra időosztásos.</span><span class="sxs-lookup"><span data-stu-id="7645f-175">Each physical asset file can contain zero or more audio tracks interleaved into an appropriate container format.</span></span> <span data-ttu-id="7645f-176">Ez az elem tartalmazza az összes [AudioTracks elem](media-services-input-metadata-schema.md#AudioTracks) hello objektumfájlt részét képező.</span><span class="sxs-lookup"><span data-stu-id="7645f-176">This element contains a collection of all [AudioTracks element](media-services-input-metadata-schema.md#AudioTracks) that are part of hello asset file.</span></span> |
| <span data-ttu-id="7645f-177">**Metaadatok**</span><span class="sxs-lookup"><span data-stu-id="7645f-177">**Metadata**</span></span><br /><br /> <span data-ttu-id="7645f-178">minOccurs = "0" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="7645f-178">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="7645f-179">MetadataType</span><span class="sxs-lookup"><span data-stu-id="7645f-179">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="7645f-180">Eszköz fájl metaadat-ként key\value karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="7645f-180">Asset file’s metadata represented as key\value strings.</span></span> <span data-ttu-id="7645f-181">Példa:</span><span class="sxs-lookup"><span data-stu-id="7645f-181">For example:</span></span><br /><br /> <span data-ttu-id="7645f-182">**&lt;A metaadat kulcsként = "nyelv" value = "hun" /&gt;**</span><span class="sxs-lookup"><span data-stu-id="7645f-182">**&lt;Metadata key="language" value="eng" /&gt;**</span></span> |

## <span data-ttu-id="7645f-183"><a name="TrackType"></a>TrackType</span><span class="sxs-lookup"><span data-stu-id="7645f-183"><a name="TrackType"></a> TrackType</span></span>
<span data-ttu-id="7645f-184">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-184">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="7645f-185">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="7645f-185">Attributes</span></span>
| <span data-ttu-id="7645f-186">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-186">Name</span></span> | <span data-ttu-id="7645f-187">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-187">Type</span></span> | <span data-ttu-id="7645f-188">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-188">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-189">**Azonosítója**</span><span class="sxs-lookup"><span data-stu-id="7645f-189">**Id**</span></span><br /><br /> <span data-ttu-id="7645f-190">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-190">Required</span></span> |<span data-ttu-id="7645f-191">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-191">**xs:int**</span></span> |<span data-ttu-id="7645f-192">A hang- vagy követése nulla alapú indexét.</span><span class="sxs-lookup"><span data-stu-id="7645f-192">Zero-based index of this audio or video track.</span></span><br /><br /> <span data-ttu-id="7645f-193">Ez nem szükségszerűen adott hello TrackID használt MP4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7645f-193">This is not necessarily that hello TrackID as used in an MP4 file.</span></span> |
| <span data-ttu-id="7645f-194">**Kodek**</span><span class="sxs-lookup"><span data-stu-id="7645f-194">**Codec**</span></span> |<span data-ttu-id="7645f-195">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-195">**xs:string**</span></span> |<span data-ttu-id="7645f-196">Videó követése kodek karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="7645f-196">Video track codec string.</span></span> |
| <span data-ttu-id="7645f-197">**CodecLongName**</span><span class="sxs-lookup"><span data-stu-id="7645f-197">**CodecLongName**</span></span> |<span data-ttu-id="7645f-198">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-198">**xs:string**</span></span> |<span data-ttu-id="7645f-199">Hang- vagy követése kodek hosszú neve.</span><span class="sxs-lookup"><span data-stu-id="7645f-199">Audio or video track codec long name.</span></span> |
| <span data-ttu-id="7645f-200">**TimeBase**</span><span class="sxs-lookup"><span data-stu-id="7645f-200">**TimeBase**</span></span><br /><br /> <span data-ttu-id="7645f-201">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-201">Required</span></span> |<span data-ttu-id="7645f-202">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-202">**xs:string**</span></span> |<span data-ttu-id="7645f-203">Idejének alapja.</span><span class="sxs-lookup"><span data-stu-id="7645f-203">Time base.</span></span> <span data-ttu-id="7645f-204">Példa: TimeBase = "1/48000"</span><span class="sxs-lookup"><span data-stu-id="7645f-204">Example: TimeBase="1/48000"</span></span> |
| <span data-ttu-id="7645f-205">**NumberOfFrames**</span><span class="sxs-lookup"><span data-stu-id="7645f-205">**NumberOfFrames**</span></span> |<span data-ttu-id="7645f-206">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-206">**xs:int**</span></span> |<span data-ttu-id="7645f-207">Keretek (videó nyomon követi a jelen) száma.</span><span class="sxs-lookup"><span data-stu-id="7645f-207">Number of frames (present for video tracks).</span></span> |
| <span data-ttu-id="7645f-208">**Kezdő időpont**</span><span class="sxs-lookup"><span data-stu-id="7645f-208">**StartTime**</span></span> |<span data-ttu-id="7645f-209">**DURATION típusú**</span><span class="sxs-lookup"><span data-stu-id="7645f-209">**xs:duration**</span></span> |<span data-ttu-id="7645f-210">Kezdési idő nyomon követése.</span><span class="sxs-lookup"><span data-stu-id="7645f-210">Track start time.</span></span> <span data-ttu-id="7645f-211">Példa: StartTime = "PT2.669S"</span><span class="sxs-lookup"><span data-stu-id="7645f-211">Example: StartTime="PT2.669S"</span></span> |
| <span data-ttu-id="7645f-212">**Időtartam**</span><span class="sxs-lookup"><span data-stu-id="7645f-212">**Duration**</span></span> |<span data-ttu-id="7645f-213">**DURATION típusú**</span><span class="sxs-lookup"><span data-stu-id="7645f-213">**xs:duration**</span></span> |<span data-ttu-id="7645f-214">Időtartam nyomon.</span><span class="sxs-lookup"><span data-stu-id="7645f-214">Track duration.</span></span> <span data-ttu-id="7645f-215">Példa: Duration = "PTSampleFormat M37.757S".</span><span class="sxs-lookup"><span data-stu-id="7645f-215">Example: Duration="PTSampleFormat M37.757S".</span></span> |

> [!NOTE]
> <span data-ttu-id="7645f-216">a következő 2 gyermekelemek hello sorrendben kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="7645f-216">hello following 2 child elements must appear in a sequence.</span></span>  
> 
> 

### <a name="child-elements"></a><span data-ttu-id="7645f-217">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="7645f-217">Child elements</span></span>
| <span data-ttu-id="7645f-218">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-218">Name</span></span> | <span data-ttu-id="7645f-219">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-219">Type</span></span> | <span data-ttu-id="7645f-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-220">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-221">**Törlése**</span><span class="sxs-lookup"><span data-stu-id="7645f-221">**Disposition**</span></span><br /><br /> <span data-ttu-id="7645f-222">minOccurs = "0" maxOccurs = "1"</span><span class="sxs-lookup"><span data-stu-id="7645f-222">minOccurs="0" maxOccurs="1"</span></span> |[<span data-ttu-id="7645f-223">StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="7645f-223">StreamDispositionType</span></span>](media-services-input-metadata-schema.md#StreamDispositionType) |<span data-ttu-id="7645f-224">Megjelenítési adatok (például, hogy egy adott hang nyomon követése a gyengén látó felhasználóknak) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7645f-224">Contains presentation information (for example, whether a particular audio track is for visually impaired viewers).</span></span> |
| <span data-ttu-id="7645f-225">**Metaadatok**</span><span class="sxs-lookup"><span data-stu-id="7645f-225">**Metadata**</span></span><br /><br /> <span data-ttu-id="7645f-226">minOccurs = "0" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="7645f-226">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="7645f-227">MetadataType</span><span class="sxs-lookup"><span data-stu-id="7645f-227">MetadataType</span></span>](media-services-input-metadata-schema.md#MetadataType) |<span data-ttu-id="7645f-228">Általános kulcs/érték karakterláncok, amelyek használt toohold számos információs lehetnek.</span><span class="sxs-lookup"><span data-stu-id="7645f-228">Generic key/value strings that can be used toohold a variety of information.</span></span> <span data-ttu-id="7645f-229">Például a kulcs = "nyelv", és az érték = "hun".</span><span class="sxs-lookup"><span data-stu-id="7645f-229">For example, key=”language”, and value=”eng”.</span></span> |

## <span data-ttu-id="7645f-230"><a name="AudioTrackType"></a>AudioTrackType (TrackType örököl)</span><span class="sxs-lookup"><span data-stu-id="7645f-230"><a name="AudioTrackType"></a> AudioTrackType (inherits from TrackType)</span></span>
 <span data-ttu-id="7645f-231">**AudioTrackType** származó globális összetett típus [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="7645f-231">**AudioTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

 <span data-ttu-id="7645f-232">hello típus hello eszköz fájlban meghatározott hang nyomon jelképezi.</span><span class="sxs-lookup"><span data-stu-id="7645f-232">hello type represents a specific audio track in hello asset file.</span></span>  

 <span data-ttu-id="7645f-233">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-233">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="7645f-234">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="7645f-234">Attributes</span></span>
| <span data-ttu-id="7645f-235">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-235">Name</span></span> | <span data-ttu-id="7645f-236">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-236">Type</span></span> | <span data-ttu-id="7645f-237">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-237">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-238">**SampleFormat**</span><span class="sxs-lookup"><span data-stu-id="7645f-238">**SampleFormat**</span></span> |<span data-ttu-id="7645f-239">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-239">**xs:string**</span></span> |<span data-ttu-id="7645f-240">A minta-formátum.</span><span class="sxs-lookup"><span data-stu-id="7645f-240">Sample format.</span></span> |
| <span data-ttu-id="7645f-241">**ChannelLayout**</span><span class="sxs-lookup"><span data-stu-id="7645f-241">**ChannelLayout**</span></span> |<span data-ttu-id="7645f-242">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-242">**xs:string**</span></span> |<span data-ttu-id="7645f-243">Csatorna elrendezés.</span><span class="sxs-lookup"><span data-stu-id="7645f-243">Channel layout.</span></span> |
| <span data-ttu-id="7645f-244">**Csatornák**</span><span class="sxs-lookup"><span data-stu-id="7645f-244">**Channels**</span></span><br /><br /> <span data-ttu-id="7645f-245">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-245">Required</span></span> |<span data-ttu-id="7645f-246">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-246">**xs:int**</span></span> |<span data-ttu-id="7645f-247">(0 vagy több) hang csatornák száma.</span><span class="sxs-lookup"><span data-stu-id="7645f-247">Number (0 or more) of audio channels.</span></span> |
| <span data-ttu-id="7645f-248">**Érvénytelen a SamplingRate**</span><span class="sxs-lookup"><span data-stu-id="7645f-248">**SamplingRate**</span></span><br /><br /> <span data-ttu-id="7645f-249">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-249">Required</span></span> |<span data-ttu-id="7645f-250">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-250">**xs:int**</span></span> |<span data-ttu-id="7645f-251">Hang mintavételi ráta minták másodpercenkénti vagy Hz.</span><span class="sxs-lookup"><span data-stu-id="7645f-251">Audio sampling rate in samples/sec or Hz.</span></span> |
| <span data-ttu-id="7645f-252">**Átviteli sebesség**</span><span class="sxs-lookup"><span data-stu-id="7645f-252">**Bitrate**</span></span> |<span data-ttu-id="7645f-253">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-253">**xs:int**</span></span> |<span data-ttu-id="7645f-254">Bit / másodperc, a kiszámított hello eszköz fájlból átlagos átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="7645f-254">Average audio bit rate in bits per second, as calculated from hello asset file.</span></span> <span data-ttu-id="7645f-255">Csak a hello elemi adatfolyam hasznos számít, és hello csomagolás terhelést nem szerepel a számláló értéke.</span><span class="sxs-lookup"><span data-stu-id="7645f-255">Only hello elementary stream payload is counted, and hello packaging overhead is not included in this count.</span></span> |
| <span data-ttu-id="7645f-256">**A BitsPerSample**</span><span class="sxs-lookup"><span data-stu-id="7645f-256">**BitsPerSample**</span></span> |<span data-ttu-id="7645f-257">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-257">**xs:int**</span></span> |<span data-ttu-id="7645f-258">Bit / minta hello wFormatTag formátumban írja be.</span><span class="sxs-lookup"><span data-stu-id="7645f-258">Bits per sample for hello wFormatTag format type.</span></span> |

## <span data-ttu-id="7645f-259"><a name="VideoTrackType"></a>VideoTrackType (TrackType örököl)</span><span class="sxs-lookup"><span data-stu-id="7645f-259"><a name="VideoTrackType"></a> VideoTrackType (inherits from TrackType)</span></span>
<span data-ttu-id="7645f-260">**VideoTrackType** származó globális összetett típus [TrackType](media-services-input-metadata-schema.md#TrackType).</span><span class="sxs-lookup"><span data-stu-id="7645f-260">**VideoTrackType** is a global complex type that inherits from [TrackType](media-services-input-metadata-schema.md#TrackType).</span></span>  

<span data-ttu-id="7645f-261">hello típus hello eszköz fájlban meghatározott videó nyomon jelképezi.</span><span class="sxs-lookup"><span data-stu-id="7645f-261">hello type represents a specific video track in hello asset file.</span></span>  

<span data-ttu-id="7645f-262">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-262">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="7645f-263">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="7645f-263">Attributes</span></span>
| <span data-ttu-id="7645f-264">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-264">Name</span></span> | <span data-ttu-id="7645f-265">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-265">Type</span></span> | <span data-ttu-id="7645f-266">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-266">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-267">**FourCC**</span><span class="sxs-lookup"><span data-stu-id="7645f-267">**FourCC**</span></span><br /><br /> <span data-ttu-id="7645f-268">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-268">Required</span></span> |<span data-ttu-id="7645f-269">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-269">**xs:string**</span></span> |<span data-ttu-id="7645f-270">Videó kodek FourCC kódot.</span><span class="sxs-lookup"><span data-stu-id="7645f-270">Video codec FourCC code.</span></span> |
| <span data-ttu-id="7645f-271">**Profil**</span><span class="sxs-lookup"><span data-stu-id="7645f-271">**Profile**</span></span> |<span data-ttu-id="7645f-272">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-272">**xs:string**</span></span> |<span data-ttu-id="7645f-273">Videó követése profil.</span><span class="sxs-lookup"><span data-stu-id="7645f-273">Video track's profile.</span></span> |
| <span data-ttu-id="7645f-274">**Szint**</span><span class="sxs-lookup"><span data-stu-id="7645f-274">**Level**</span></span> |<span data-ttu-id="7645f-275">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-275">**xs:string**</span></span> |<span data-ttu-id="7645f-276">Videó követése szintje.</span><span class="sxs-lookup"><span data-stu-id="7645f-276">Video track's level.</span></span> |
| <span data-ttu-id="7645f-277">**PixelFormat**</span><span class="sxs-lookup"><span data-stu-id="7645f-277">**PixelFormat**</span></span> |<span data-ttu-id="7645f-278">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-278">**xs:string**</span></span> |<span data-ttu-id="7645f-279">Videó követése képpontformátum.</span><span class="sxs-lookup"><span data-stu-id="7645f-279">Video track's pixel format.</span></span> |
| <span data-ttu-id="7645f-280">**Szélessége**</span><span class="sxs-lookup"><span data-stu-id="7645f-280">**Width**</span></span><br /><br /> <span data-ttu-id="7645f-281">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-281">Required</span></span> |<span data-ttu-id="7645f-282">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-282">**xs:int**</span></span> |<span data-ttu-id="7645f-283">Kódolt videó szélessége képpontban megadva.</span><span class="sxs-lookup"><span data-stu-id="7645f-283">Encoded video width in pixels.</span></span> |
| <span data-ttu-id="7645f-284">**Magassága**</span><span class="sxs-lookup"><span data-stu-id="7645f-284">**Height**</span></span><br /><br /> <span data-ttu-id="7645f-285">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-285">Required</span></span> |<span data-ttu-id="7645f-286">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-286">**xs:int**</span></span> |<span data-ttu-id="7645f-287">Videó magassága képpontban kódolva.</span><span class="sxs-lookup"><span data-stu-id="7645f-287">Encoded video height in pixels.</span></span> |
| <span data-ttu-id="7645f-288">**DisplayAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="7645f-288">**DisplayAspectRatioNumerator**</span></span><br /><br /> <span data-ttu-id="7645f-289">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-289">Required</span></span> |<span data-ttu-id="7645f-290">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="7645f-290">**xs:double**</span></span> |<span data-ttu-id="7645f-291">Képmegjelenítő oldalarányának számlálójának.</span><span class="sxs-lookup"><span data-stu-id="7645f-291">Video display aspect ratio numerator.</span></span> |
| <span data-ttu-id="7645f-292">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="7645f-292">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="7645f-293">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-293">Required</span></span> |<span data-ttu-id="7645f-294">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="7645f-294">**xs:double**</span></span> |<span data-ttu-id="7645f-295">Képmegjelenítő oldalarányának nevező.</span><span class="sxs-lookup"><span data-stu-id="7645f-295">Video display aspect ratio denominator.</span></span> |
| <span data-ttu-id="7645f-296">**DisplayAspectRatioDenominator**</span><span class="sxs-lookup"><span data-stu-id="7645f-296">**DisplayAspectRatioDenominator**</span></span><br /><br /> <span data-ttu-id="7645f-297">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-297">Required</span></span> |<span data-ttu-id="7645f-298">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="7645f-298">**xs:double**</span></span> |<span data-ttu-id="7645f-299">Videó minta oldalarányának számlálójának.</span><span class="sxs-lookup"><span data-stu-id="7645f-299">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="7645f-300">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="7645f-300">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="7645f-301">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="7645f-301">**xs:double**</span></span> |<span data-ttu-id="7645f-302">Videó minta oldalarányának számlálójának.</span><span class="sxs-lookup"><span data-stu-id="7645f-302">Video sample aspect ratio numerator.</span></span> |
| <span data-ttu-id="7645f-303">**SampleAspectRatioNumerator**</span><span class="sxs-lookup"><span data-stu-id="7645f-303">**SampleAspectRatioNumerator**</span></span> |<span data-ttu-id="7645f-304">**xs:Double**</span><span class="sxs-lookup"><span data-stu-id="7645f-304">**xs:double**</span></span> |<span data-ttu-id="7645f-305">Videó minta oldalarányának nevező.</span><span class="sxs-lookup"><span data-stu-id="7645f-305">Video sample aspect ratio denominator.</span></span> |
| <span data-ttu-id="7645f-306">**Képkockasebességhez**</span><span class="sxs-lookup"><span data-stu-id="7645f-306">**FrameRate**</span></span><br /><br /> <span data-ttu-id="7645f-307">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-307">Required</span></span> |<span data-ttu-id="7645f-308">**xs:decimal**</span><span class="sxs-lookup"><span data-stu-id="7645f-308">**xs:decimal**</span></span> |<span data-ttu-id="7645f-309">Mért videó képkockasebessége .3f formátumban.</span><span class="sxs-lookup"><span data-stu-id="7645f-309">Measured video frame rate in .3f format.</span></span> |
| <span data-ttu-id="7645f-310">**Átviteli sebesség**</span><span class="sxs-lookup"><span data-stu-id="7645f-310">**Bitrate**</span></span> |<span data-ttu-id="7645f-311">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-311">**xs:int**</span></span> |<span data-ttu-id="7645f-312">Kilobit / másodperc, a kiszámított hello eszköz fájlból videó átlagos átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="7645f-312">Average video bit rate in kilobits per second, as calculated from hello asset file.</span></span> <span data-ttu-id="7645f-313">Csak a hello elemi adatfolyam hasznos számít, és hello csomagolás terhelés nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="7645f-313">Only hello elementary stream payload is counted, and hello packaging overhead is not included.</span></span> |
| <span data-ttu-id="7645f-314">**MaxGOPBitrate**</span><span class="sxs-lookup"><span data-stu-id="7645f-314">**MaxGOPBitrate**</span></span> |<span data-ttu-id="7645f-315">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-315">**xs:int**</span></span> |<span data-ttu-id="7645f-316">Maximális GOP a videó nyomon követése, a kilobit / másodperc átlagos sávszélességű.</span><span class="sxs-lookup"><span data-stu-id="7645f-316">Max GOP average bitrate for this video track, in kilobits per second.</span></span> |
| <span data-ttu-id="7645f-317">**HasBFrames**</span><span class="sxs-lookup"><span data-stu-id="7645f-317">**HasBFrames**</span></span> |<span data-ttu-id="7645f-318">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-318">**xs:int**</span></span> |<span data-ttu-id="7645f-319">B keretek videó követése száma.</span><span class="sxs-lookup"><span data-stu-id="7645f-319">Video track number of B frames.</span></span> |

## <span data-ttu-id="7645f-320"><a name="MetadataType"></a>MetadataType</span><span class="sxs-lookup"><span data-stu-id="7645f-320"><a name="MetadataType"></a> MetadataType</span></span>
<span data-ttu-id="7645f-321">**MetadataType** leíró metaadatok eszköz fájlok kulcs/érték karakterláncként globális összetett típus.</span><span class="sxs-lookup"><span data-stu-id="7645f-321">**MetadataType** is a global complex type that describes metadata of an asset file as key/value strings.</span></span> <span data-ttu-id="7645f-322">Például a kulcs = "nyelv", és az érték = "hun".</span><span class="sxs-lookup"><span data-stu-id="7645f-322">For example, key=”language”, and value=”eng”.</span></span>  

<span data-ttu-id="7645f-323">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-323">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="7645f-324">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="7645f-324">Attributes</span></span>
| <span data-ttu-id="7645f-325">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-325">Name</span></span> | <span data-ttu-id="7645f-326">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-326">Type</span></span> | <span data-ttu-id="7645f-327">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-327">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-328">**kulcs**</span><span class="sxs-lookup"><span data-stu-id="7645f-328">**key**</span></span><br /><br /> <span data-ttu-id="7645f-329">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-329">Required</span></span> |<span data-ttu-id="7645f-330">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-330">**xs:string**</span></span> |<span data-ttu-id="7645f-331">hello kulcs/érték pár hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="7645f-331">hello key in hello key/value pair.</span></span> |
| <span data-ttu-id="7645f-332">**érték**</span><span class="sxs-lookup"><span data-stu-id="7645f-332">**value**</span></span><br /><br /> <span data-ttu-id="7645f-333">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-333">Required</span></span> |<span data-ttu-id="7645f-334">**xs:String**</span><span class="sxs-lookup"><span data-stu-id="7645f-334">**xs:string**</span></span> |<span data-ttu-id="7645f-335">hello kulcs/érték pár hello érték.</span><span class="sxs-lookup"><span data-stu-id="7645f-335">hello value in hello key/value pair.</span></span> |

## <span data-ttu-id="7645f-336"><a name="ProgramType"></a>ProgramType</span><span class="sxs-lookup"><span data-stu-id="7645f-336"><a name="ProgramType"></a> ProgramType</span></span>
<span data-ttu-id="7645f-337">**ProgramType** globális összetett típus, amely leírja a program.</span><span class="sxs-lookup"><span data-stu-id="7645f-337">**ProgramType** is a global complex type that describes a program.</span></span>  

### <a name="attributes"></a><span data-ttu-id="7645f-338">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="7645f-338">Attributes</span></span>
| <span data-ttu-id="7645f-339">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-339">Name</span></span> | <span data-ttu-id="7645f-340">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-340">Type</span></span> | <span data-ttu-id="7645f-341">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-341">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-342">**ProgramId**</span><span class="sxs-lookup"><span data-stu-id="7645f-342">**ProgramId**</span></span><br /><br /> <span data-ttu-id="7645f-343">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-343">Required</span></span> |<span data-ttu-id="7645f-344">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-344">**xs:int**</span></span> |<span data-ttu-id="7645f-345">Program azonosítója</span><span class="sxs-lookup"><span data-stu-id="7645f-345">Program Id</span></span> |
| <span data-ttu-id="7645f-346">**NumberOfPrograms**</span><span class="sxs-lookup"><span data-stu-id="7645f-346">**NumberOfPrograms**</span></span><br /><br /> <span data-ttu-id="7645f-347">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-347">Required</span></span> |<span data-ttu-id="7645f-348">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-348">**xs:int**</span></span> |<span data-ttu-id="7645f-349">Programok száma.</span><span class="sxs-lookup"><span data-stu-id="7645f-349">Number of programs.</span></span> |
| <span data-ttu-id="7645f-350">**PmtPid**</span><span class="sxs-lookup"><span data-stu-id="7645f-350">**PmtPid**</span></span><br /><br /> <span data-ttu-id="7645f-351">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-351">Required</span></span> |<span data-ttu-id="7645f-352">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-352">**xs:int**</span></span> |<span data-ttu-id="7645f-353">Program térkép táblák (PMTs) a programok információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="7645f-353">Program Map Tables (PMTs) contain information about programs.</span></span>  <span data-ttu-id="7645f-354">További információkért lásd: [részlet](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span><span class="sxs-lookup"><span data-stu-id="7645f-354">For more information, see [PMt](http://en.wikipedia.org/wiki/MPEG_transport_stream#PMT).</span></span> |
| <span data-ttu-id="7645f-355">**PcrPid**</span><span class="sxs-lookup"><span data-stu-id="7645f-355">**PcrPid**</span></span><br /><br /> <span data-ttu-id="7645f-356">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-356">Required</span></span> |<span data-ttu-id="7645f-357">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-357">**xs:int**</span></span> |<span data-ttu-id="7645f-358">A dekódoló használják.</span><span class="sxs-lookup"><span data-stu-id="7645f-358">Used by decoder.</span></span> <span data-ttu-id="7645f-359">További információkért lásd: [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span><span class="sxs-lookup"><span data-stu-id="7645f-359">For more information, see [PCR](http://en.wikipedia.org/wiki/MPEG_transport_stream#PCR)</span></span> |
| <span data-ttu-id="7645f-360">**StartPTS**</span><span class="sxs-lookup"><span data-stu-id="7645f-360">**StartPTS**</span></span> |<span data-ttu-id="7645f-361">**xs: hosszú**</span><span class="sxs-lookup"><span data-stu-id="7645f-361">**xs: long**</span></span> |<span data-ttu-id="7645f-362">Kezdő bemutató időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="7645f-362">Starting presentation time stamp.</span></span> |
| <span data-ttu-id="7645f-363">**EndPTS**</span><span class="sxs-lookup"><span data-stu-id="7645f-363">**EndPTS**</span></span> |<span data-ttu-id="7645f-364">**xs: hosszú**</span><span class="sxs-lookup"><span data-stu-id="7645f-364">**xs: long**</span></span> |<span data-ttu-id="7645f-365">Befejezési bemutató időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="7645f-365">Ending presentation time stamp.</span></span> |

## <span data-ttu-id="7645f-366"><a name="StreamDispositionType"></a>StreamDispositionType</span><span class="sxs-lookup"><span data-stu-id="7645f-366"><a name="StreamDispositionType"></a> StreamDispositionType</span></span>
<span data-ttu-id="7645f-367">**StreamDispositionType** hello adatfolyam leíró globális összetett típus.</span><span class="sxs-lookup"><span data-stu-id="7645f-367">**StreamDispositionType** is a global complex type that describes hello stream.</span></span>  

<span data-ttu-id="7645f-368">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-368">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="attributes"></a><span data-ttu-id="7645f-369">Attribútumok</span><span class="sxs-lookup"><span data-stu-id="7645f-369">Attributes</span></span>
| <span data-ttu-id="7645f-370">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-370">Name</span></span> | <span data-ttu-id="7645f-371">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-371">Type</span></span> | <span data-ttu-id="7645f-372">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-372">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-373">**Alapértelmezett**</span><span class="sxs-lookup"><span data-stu-id="7645f-373">**Default**</span></span><br /><br /> <span data-ttu-id="7645f-374">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-374">Required</span></span> |<span data-ttu-id="7645f-375">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-375">**xs:int**</span></span> |<span data-ttu-id="7645f-376">Ez ez az attribútum too1 tooindicate hello alapértelmezett bemutató beállítása.</span><span class="sxs-lookup"><span data-stu-id="7645f-376">Set this attribute too1 tooindicate this is hello default presentation.</span></span> |
| <span data-ttu-id="7645f-377">**Dub**</span><span class="sxs-lookup"><span data-stu-id="7645f-377">**Dub**</span></span><br /><br /> <span data-ttu-id="7645f-378">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-378">Required</span></span> |<span data-ttu-id="7645f-379">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-379">**xs:int**</span></span> |<span data-ttu-id="7645f-380">Állítsa a attribútum too1 tooindicate van hello szinkronizált bemutató.</span><span class="sxs-lookup"><span data-stu-id="7645f-380">Set this attribute too1 tooindicate this is hello dubbed presentation.</span></span> |
| <span data-ttu-id="7645f-381">**Eredeti**</span><span class="sxs-lookup"><span data-stu-id="7645f-381">**Original**</span></span><br /><br /> <span data-ttu-id="7645f-382">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-382">Required</span></span> |<span data-ttu-id="7645f-383">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-383">**xs:int**</span></span> |<span data-ttu-id="7645f-384">Ez ez az attribútum too1 tooindicate hello eredeti bemutató beállítása.</span><span class="sxs-lookup"><span data-stu-id="7645f-384">Set this attribute too1 tooindicate this is hello original presentation.</span></span> |
| <span data-ttu-id="7645f-385">**Megjegyzés**</span><span class="sxs-lookup"><span data-stu-id="7645f-385">**Comment**</span></span><br /><br /> <span data-ttu-id="7645f-386">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-386">Required</span></span> |<span data-ttu-id="7645f-387">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-387">**xs:int**</span></span> |<span data-ttu-id="7645f-388">Állítsa a attribútum too1 tooindicate követése magyarázatokkal tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7645f-388">Set this attribute too1 tooindicate this track contains commentary.</span></span> |
| <span data-ttu-id="7645f-389">**Szöveg**</span><span class="sxs-lookup"><span data-stu-id="7645f-389">**Lyrics**</span></span><br /><br /> <span data-ttu-id="7645f-390">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-390">Required</span></span> |<span data-ttu-id="7645f-391">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-391">**xs:int**</span></span> |<span data-ttu-id="7645f-392">Állítsa a attribútum too1 tooindicate követése szöveget tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7645f-392">Set this attribute too1 tooindicate this track contains lyrics.</span></span> |
| <span data-ttu-id="7645f-393">**Karaoke**</span><span class="sxs-lookup"><span data-stu-id="7645f-393">**Karaoke**</span></span><br /><br /> <span data-ttu-id="7645f-394">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-394">Required</span></span> |<span data-ttu-id="7645f-395">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-395">**xs:int**</span></span> |<span data-ttu-id="7645f-396">Ez az attribútum too1 tooindicate beállítása jelöli hello karaoke nyomon követése (háttér zene, nincs énekhez).</span><span class="sxs-lookup"><span data-stu-id="7645f-396">Set this attribute too1 tooindicate this represents hello karaoke track (background music, no vocals).</span></span> |
| <span data-ttu-id="7645f-397">**A kényszerített**</span><span class="sxs-lookup"><span data-stu-id="7645f-397">**Forced**</span></span><br /><br /> <span data-ttu-id="7645f-398">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-398">Required</span></span> |<span data-ttu-id="7645f-399">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-399">**xs:int**</span></span> |<span data-ttu-id="7645f-400">Ez ez az attribútum too1 tooindicate kényszerített hello bemutató beállítása.</span><span class="sxs-lookup"><span data-stu-id="7645f-400">Set this attribute too1 tooindicate this is hello forced presentation.</span></span> |
| <span data-ttu-id="7645f-401">**HearingImpaired**</span><span class="sxs-lookup"><span data-stu-id="7645f-401">**HearingImpaired**</span></span><br /><br /> <span data-ttu-id="7645f-402">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-402">Required</span></span> |<span data-ttu-id="7645f-403">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-403">**xs:int**</span></span> |<span data-ttu-id="7645f-404">Az attribútum too1 tooindicate sérült hello nyújtanak segítséget a szám van beállítva.</span><span class="sxs-lookup"><span data-stu-id="7645f-404">Set this attribute too1 tooindicate this track is for hello hearing impaired.</span></span> |
| <span data-ttu-id="7645f-405">**VisualImpaired**</span><span class="sxs-lookup"><span data-stu-id="7645f-405">**VisualImpaired**</span></span><br /><br /> <span data-ttu-id="7645f-406">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-406">Required</span></span> |<span data-ttu-id="7645f-407">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-407">**xs:int**</span></span> |<span data-ttu-id="7645f-408">Ez a szám a gyengén látó hello van attribútum too1 tooindicate beállítása.</span><span class="sxs-lookup"><span data-stu-id="7645f-408">Set this attribute too1 tooindicate this track is for hello visually impaired.</span></span> |
| <span data-ttu-id="7645f-409">**CleanEffects**</span><span class="sxs-lookup"><span data-stu-id="7645f-409">**CleanEffects**</span></span><br /><br /> <span data-ttu-id="7645f-410">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-410">Required</span></span> |<span data-ttu-id="7645f-411">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-411">**xs:int**</span></span> |<span data-ttu-id="7645f-412">Ez a szám rendelkezik attribútum too1 tooindicate tiszta hatások beállítása.</span><span class="sxs-lookup"><span data-stu-id="7645f-412">Set this attribute too1 tooindicate this track has clean effects.</span></span> |
| <span data-ttu-id="7645f-413">**AttachedPic**</span><span class="sxs-lookup"><span data-stu-id="7645f-413">**AttachedPic**</span></span><br /><br /> <span data-ttu-id="7645f-414">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7645f-414">Required</span></span> |<span data-ttu-id="7645f-415">**xs:INT**</span><span class="sxs-lookup"><span data-stu-id="7645f-415">**xs:int**</span></span> |<span data-ttu-id="7645f-416">Ez a szám rendelkezik attribútum too1 tooindicate képek beállítása.</span><span class="sxs-lookup"><span data-stu-id="7645f-416">Set this attribute too1 tooindicate this track has pictures.</span></span> |

## <span data-ttu-id="7645f-417"><a name="Programs"></a>Programok elem</span><span class="sxs-lookup"><span data-stu-id="7645f-417"><a name="Programs"></a> Programs element</span></span>
<span data-ttu-id="7645f-418">Több okozó burkolóelemet **Program** elemek.</span><span class="sxs-lookup"><span data-stu-id="7645f-418">Wrapper element holding multiple **Program** elements.</span></span>  

### <a name="child-elements"></a><span data-ttu-id="7645f-419">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="7645f-419">Child elements</span></span>
| <span data-ttu-id="7645f-420">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-420">Name</span></span> | <span data-ttu-id="7645f-421">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-421">Type</span></span> | <span data-ttu-id="7645f-422">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-422">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-423">**Program**</span><span class="sxs-lookup"><span data-stu-id="7645f-423">**Program**</span></span><br /><br /> <span data-ttu-id="7645f-424">minOccurs = "0" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="7645f-424">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="7645f-425">ProgramType</span><span class="sxs-lookup"><span data-stu-id="7645f-425">ProgramType</span></span>](media-services-input-metadata-schema.md#ProgramType) |<span data-ttu-id="7645f-426">Eszköz MPEG-TS formátumú fájlok esetében hello objektumfájlt programokkal kapcsolatos információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7645f-426">For asset files that are in MPEG-TS format, contains information about programs in hello asset file.</span></span> |

## <span data-ttu-id="7645f-427"><a name="VideoTracks"></a>VideoTracks elem</span><span class="sxs-lookup"><span data-stu-id="7645f-427"><a name="VideoTracks"></a> VideoTracks element</span></span>
 <span data-ttu-id="7645f-428">Több okozó burkolóelemet **VideoTrack** elemek.</span><span class="sxs-lookup"><span data-stu-id="7645f-428">Wrapper element holding multiple **VideoTrack** elements.</span></span>  

 <span data-ttu-id="7645f-429">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-429">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="child-elements"></a><span data-ttu-id="7645f-430">Gyermekelemek</span><span class="sxs-lookup"><span data-stu-id="7645f-430">Child elements</span></span>
| <span data-ttu-id="7645f-431">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-431">Name</span></span> | <span data-ttu-id="7645f-432">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-432">Type</span></span> | <span data-ttu-id="7645f-433">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-433">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-434">**VideoTrack**</span><span class="sxs-lookup"><span data-stu-id="7645f-434">**VideoTrack**</span></span><br /><br /> <span data-ttu-id="7645f-435">minOccurs = "0" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="7645f-435">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="7645f-436">VideoTrackType (TrackType örököl)</span><span class="sxs-lookup"><span data-stu-id="7645f-436">VideoTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#VideoTrackType) |<span data-ttu-id="7645f-437">Videó nyomon követi a hello eszköz fájl adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7645f-437">Contains information about video tracks in hello asset file.</span></span> |

## <span data-ttu-id="7645f-438"><a name="AudioTracks"></a>AudioTracks elem</span><span class="sxs-lookup"><span data-stu-id="7645f-438"><a name="AudioTracks"></a> AudioTracks element</span></span>
 <span data-ttu-id="7645f-439">Több okozó burkolóelemet **AudioTrack** elemek.</span><span class="sxs-lookup"><span data-stu-id="7645f-439">Wrapper element holding multiple **AudioTrack** elements.</span></span>  

 <span data-ttu-id="7645f-440">Ez a témakör végén hello XML példát: [XML-példa](media-services-input-metadata-schema.md#xml).</span><span class="sxs-lookup"><span data-stu-id="7645f-440">See an XML example at hello end of this topic: [XML example](media-services-input-metadata-schema.md#xml).</span></span>  

### <a name="elements"></a><span data-ttu-id="7645f-441">Elemek</span><span class="sxs-lookup"><span data-stu-id="7645f-441">elements</span></span>
| <span data-ttu-id="7645f-442">Név</span><span class="sxs-lookup"><span data-stu-id="7645f-442">Name</span></span> | <span data-ttu-id="7645f-443">Típus</span><span class="sxs-lookup"><span data-stu-id="7645f-443">Type</span></span> | <span data-ttu-id="7645f-444">Leírás</span><span class="sxs-lookup"><span data-stu-id="7645f-444">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="7645f-445">**AudioTrack**</span><span class="sxs-lookup"><span data-stu-id="7645f-445">**AudioTrack**</span></span><br /><br /> <span data-ttu-id="7645f-446">minOccurs = "0" maxOccurs = "unbounded"</span><span class="sxs-lookup"><span data-stu-id="7645f-446">minOccurs="0" maxOccurs="unbounded"</span></span> |[<span data-ttu-id="7645f-447">AudioTrackType (TrackType örököl)</span><span class="sxs-lookup"><span data-stu-id="7645f-447">AudioTrackType (inherits from TrackType)</span></span>](media-services-input-metadata-schema.md#AudioTrackType) |<span data-ttu-id="7645f-448">Hello eszköz fájlban zeneszámok adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7645f-448">Contains information about audio tracks in hello asset file.</span></span> |

## <span data-ttu-id="7645f-449"><a name="code"></a>Séma kódot</span><span class="sxs-lookup"><span data-stu-id="7645f-449"><a name="code"></a> Schema Code</span></span>
    <?xml version="1.0" encoding="utf-8"?>  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" version="1.0"  
               xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               targetNamespace="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata"  
               elementFormDefault="qualified">  

      <xs:complexType name="MetadataType">  
        <xs:attribute name="key"   type="xs:string" use="required"/>  
        <xs:attribute name="value" type="xs:string" use="required"/>  
      </xs:complexType>  

      <xs:complexType name="ProgramType">  
        <xs:attribute name="ProgramId" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Program Id</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfPrograms" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>Number of programs</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PmtPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pmt pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="PcrPid" type="xs:int" use="required">  
          <xs:annotation>  
            <xs:documentation>pcr pid</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="StartPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>start pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="EndPTS" type="xs:long">  
          <xs:annotation>  
            <xs:documentation>end pts</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="StreamDispositionType">  
        <xs:attribute name="Default"          type="xs:int" use="required" />  
        <xs:attribute name="Dub"              type="xs:int" use="required" />  
        <xs:attribute name="Original"         type="xs:int" use="required" />  
        <xs:attribute name="Comment"          type="xs:int" use="required" />  
        <xs:attribute name="Lyrics"           type="xs:int" use="required" />  
        <xs:attribute name="Karaoke"          type="xs:int" use="required" />  
        <xs:attribute name="Forced"           type="xs:int" use="required" />  
        <xs:attribute name="HearingImpaired"  type="xs:int" use="required" />  
        <xs:attribute name="VisualImpaired"   type="xs:int" use="required" />  
        <xs:attribute name="CleanEffects"     type="xs:int" use="required" />  
        <xs:attribute name="AttachedPic"      type="xs:int" use="required" />  
      </xs:complexType>  

      <xs:complexType name="TrackType" abstract="true">  
        <xs:sequence>  
          <xs:element name="Disposition" type="StreamDispositionType" minOccurs="0" maxOccurs="1"/>  
          <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded"/>  
        </xs:sequence>  
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
        <xs:attribute name="Codec" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec string</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="CodecLongName" type="xs:string">  
          <xs:annotation>  
            <xs:documentation>video track codec long name</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="TimeBase"  type="xs:string" use="required">  
          <xs:annotation>  
            <xs:documentation>Time base. Example: TimeBase="1/48000"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="NumberOfFrames">  
          <xs:annotation>  
            <xs:documentation>number of frames</xs:documentation>  
          </xs:annotation>  
          <xs:simpleType>  
            <xs:restriction base="xs:int">  
              <xs:minInclusive value="0"/>  
            </xs:restriction>  
          </xs:simpleType>  
        </xs:attribute>  
        <xs:attribute name="StartTime" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track start time. Example: StartTime="PT2.669S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
        <xs:attribute name="Duration" type="xs:duration">  
          <xs:annotation>  
            <xs:documentation>Track duration. Example: Duration="PT25M37.757S"</xs:documentation>  
          </xs:annotation>  
        </xs:attribute>  
      </xs:complexType>  

      <xs:complexType name="VideoTrackType">  
        <xs:annotation>  
          <xs:documentation>A specific video track in hello parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="FourCC" type="xs:string" use="required">  
              <xs:annotation>  
                <xs:documentation>video codec FourCC code</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Profile" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>profile</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="Level" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>level</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="PixelFormat" type="xs:string">  
              <xs:annotation>  
                <xs:documentation>Video track's pixel format</xs:documentation>  
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
            <xs:attribute name="SampleAspectRatioNumerator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio numerator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="SampleAspectRatioDenominator">  
              <xs:annotation>  
                <xs:documentation>video sample aspect ratio denominator</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:double">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="FrameRate" use="required">  
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
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average video bit rate in kilobits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
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
            <xs:attribute name="HasBFrames" type="xs:int">  
              <xs:annotation>  
                <xs:documentation>video track number of B frames</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

      <xs:complexType name="AudioTrackType">  
        <xs:annotation>  
          <xs:documentation>a specific audio track in hello parent AssetFile</xs:documentation>  
        </xs:annotation>  
        <xs:complexContent>  
          <xs:extension base="TrackType">  
            <xs:attribute name="SampleFormat"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>sample format</xs:documentation>  
              </xs:annotation>  
            </xs:attribute>  
            <xs:attribute name="ChannelLayout"  type="xs:string">  
              <xs:annotation>  
                <xs:documentation>channel layout</xs:documentation>  
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
            <xs:attribute name="Bitrate">  
              <xs:annotation>  
                <xs:documentation>average audio bit rate in bits per second, as calculated from hello AssetFile. Counts only hello elementary stream payload, and does not include hello packaging overhead</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
            <xs:attribute name="BitsPerSample">  
              <xs:annotation>  
                <xs:documentation>Bits per sample for hello wFormatTag format type</xs:documentation>  
              </xs:annotation>  
              <xs:simpleType>  
                <xs:restriction base="xs:int">  
                  <xs:minInclusive value="0"/>  
                </xs:restriction>  
              </xs:simpleType>  
            </xs:attribute>  
          </xs:extension>  
        </xs:complexContent>  
      </xs:complexType>  

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
                  <xs:element name="Programs" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>This is hello collection of all programs when file is MPEG-TS</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="Program" type="ProgramType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="VideoTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>Each physical AssetFile can contain in it zero or more video tracks interleaved into an appropriate container format. This is hello collection of all those video tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="VideoTrack" type="VideoTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="AudioTracks" minOccurs="0">  
                    <xs:annotation>  
                      <xs:documentation>each physical AssetFile can contain in it zero or more audio tracks interleaved into an appropriate container format. This is hello collection of all those audio tracks</xs:documentation>  
                    </xs:annotation>  
                    <xs:complexType>  
                      <xs:sequence>  
                        <xs:element name="AudioTrack" type="AudioTrackType" minOccurs="0" maxOccurs="unbounded" />  
                      </xs:sequence>  
                    </xs:complexType>  
                  </xs:element>  
                  <xs:element name="Metadata" type="MetadataType" minOccurs="0" maxOccurs="unbounded" />  
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
                <xs:attribute name="Duration" type="xs:duration" use="required">  
                  <xs:annotation>  
                    <xs:documentation>content play back duration. Example: Duration="PT25M37.757S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="NumberOfStreams" type="xs:int" use="required">  
                  <xs:annotation>  
                    <xs:documentation>number of streams in asset file</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatNames" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="FormatVerboseName" type="xs:string" use="required">  
                  <xs:annotation>  
                    <xs:documentation>format verbose names</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="StartTime" type="xs:duration">  
                  <xs:annotation>  
                    <xs:documentation>content start time. Example: StartTime="PT2.669S"</xs:documentation>  
                  </xs:annotation>  
                </xs:attribute>  
                <xs:attribute name="OverallBitRate">  
                  <xs:annotation>  
                    <xs:documentation>average bitrate of hello asset file in kbps</xs:documentation>  
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
    </xs:schema>  


## <span data-ttu-id="7645f-450"><a name="xml"></a>XML-példa</span><span class="sxs-lookup"><span data-stu-id="7645f-450"><a name="xml"></a> XML example</span></span>
<span data-ttu-id="7645f-451">hello hello bemeneti metaadatait tartalmazó fájl egy példa látható.</span><span class="sxs-lookup"><span data-stu-id="7645f-451">hello following is an example of hello Input metadata file.</span></span>  

    <?xml version="1.0" encoding="utf-8"?>  
    <AssetFiles xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/windowsazure/mediaservices/2014/07/mediaencoder/inputmetadata">  
      <AssetFile Name="bear.mp4" Size="1973733" Duration="PT12.678S" NumberOfStreams="2" FormatNames="mov,mp4,m4a,3gp,3g2,mj2" FormatVerboseName="QuickTime / MOV" StartTime="PT0S" OverallBitRate="1245">  
        <VideoTracks>  
          <VideoTrack Id="1" Codec="h264" CodecLongName="H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10" TimeBase="1/29970" NumberOfFrames="375" StartTime="PT0.034S" Duration="PT12.645S" FourCC="avc1" Profile="High" Level="4.1" PixelFormat="yuv420p" Width="512" Height="384" DisplayAspectRatioNumerator="4" DisplayAspectRatioDenominator="3" SampleAspectRatioNumerator="1" SampleAspectRatioDenominator="1" FrameRate="29.656" Bitrate="1043" HasBFrames="1">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Video Media Handler" />  
          </VideoTrack>  
        </VideoTracks>  
        <AudioTracks>  
          <AudioTrack Id="0" Codec="aac" CodecLongName="AAC (Advanced Audio Coding)" TimeBase="1/44100" NumberOfFrames="546" StartTime="PT0S" Duration="PT12.678S" SampleFormat="fltp" ChannelLayout="stereo" Channels="2" SamplingRate="44100" Bitrate="156" BitsPerSample="0">  
            <Disposition Default="1" Dub="0" Original="0" Comment="0" Lyrics="0" Karaoke="0" Forced="0" HearingImpaired="0" VisualImpaired="0" CleanEffects="0" AttachedPic="0" />  
            <Metadata key="creation_time" value="2010-03-10 16:11:56" />  
            <Metadata key="language" value="eng" />  
            <Metadata key="handler_name" value="Mainconcept MP4 Sound Media Handler" />  
          </AudioTrack>  
        </AudioTracks>  
        <Metadata key="major_brand" value="mp42" />  
        <Metadata key="minor_version" value="0" />  
        <Metadata key="compatible_brands" value="mp42mp41" />  
        <Metadata key="creation_time" value="2010-03-10 16:11:53" />  
        <Metadata key="comment" value="Courtesy of National Geographic.  Used by Permission." />  
      </AssetFile>  
    </AssetFiles>  

## <a name="next-steps"></a><span data-ttu-id="7645f-452">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7645f-452">Next steps</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7645f-453">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7645f-453">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


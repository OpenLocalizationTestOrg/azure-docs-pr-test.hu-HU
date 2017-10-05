---
title: "A média az Azure portál használatával elemezheti |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan kell feldolgozni a media Médiaelemzés media processzorral (felügyeleti csomagok) az Azure portál használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: juliako
ms.openlocfilehash: 22032aef0cc8b7b015503043028873e70c21ee85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="analyze-your-media-using-the-azure-portal"></a><span data-ttu-id="b8b81-103">Médiatartalmak elemzése az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="b8b81-103">Analyze your media using the Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="b8b81-104">Az oktatóanyag elvégzéséhez egy Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="b8b81-104">To complete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="b8b81-105">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b8b81-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

## <a name="overview"></a><span data-ttu-id="b8b81-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b8b81-106">Overview</span></span>
<span data-ttu-id="b8b81-107">Az Azure Media Services Analytics beszéd- és vizuális összetevők (a vállalati méretű, megfelelőség, biztonsági és globális reach), amelyek megkönnyítik a szervezetek és vállalatok számára, hogy készítsenek gyakorlatban használható elemzések gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="b8b81-107">Azure Media Services Analytics is a collection of speech and vision components (at enterprise scale, compliance, security and global reach) that make it easier for organizations and enterprises to derive actionable insights from their video files.</span></span> <span data-ttu-id="b8b81-108">További részletes Azure Media Services Analytics áttekintése: [ez](media-services-analytics-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="b8b81-108">For more detailed overview of Azure Media Services Analytics see [this](media-services-analytics-overview.md) topic.</span></span> 

<span data-ttu-id="b8b81-109">Ez a témakör ismerteti, hogyan kell feldolgozni a media Médiaelemzés media processzorral (felügyeleti csomagok) az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="b8b81-109">This topic discusses how to process your media with Media Analytics media processors (MPs) using the Azure portal.</span></span> <span data-ttu-id="b8b81-110">Media Analytics felügyeleti csomagok MP4- vagy JSON-fájlok előállításához.</span><span class="sxs-lookup"><span data-stu-id="b8b81-110">Media Analytics MPs produce MP4 files or JSON files.</span></span> <span data-ttu-id="b8b81-111">A médiafeldolgozók által létrehozott MP4-fájlokat fokozatosan lehet letölteni.</span><span class="sxs-lookup"><span data-stu-id="b8b81-111">If a media processor produced an MP4 file, you can progressively download the file.</span></span> <span data-ttu-id="b8b81-112">A médiafeldolgozók által létrehozott JSON-fájlokat az Azure-blobtárolóból lehet letölteni.</span><span class="sxs-lookup"><span data-stu-id="b8b81-112">If a media processor produced a JSON file, you can download the file from the Azure blob storage.</span></span> 

## <a name="choose-an-asset-that-you-want-to-analyze"></a><span data-ttu-id="b8b81-113">Válassza ki az elemezni kívánt eszköz</span><span class="sxs-lookup"><span data-stu-id="b8b81-113">Choose an asset that you want to analyze</span></span>
1. <span data-ttu-id="b8b81-114">Az [Azure-portálon](https://portal.azure.com/) válassza ki Azure Media Services-fiókját.</span><span class="sxs-lookup"><span data-stu-id="b8b81-114">In the [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="b8b81-115">A **Settings** (Beállítások) ablakban válassza az **Assets** (Objektumok) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b8b81-115">In the **Settings** window, select **Assets**.</span></span>  
   <span data-ttu-id="b8b81-116">.</span><span class="sxs-lookup"><span data-stu-id="b8b81-116">.</span></span>
    <span data-ttu-id="b8b81-117">![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span><span class="sxs-lookup"><span data-stu-id="b8b81-117">![Analyze videos](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span></span>
3. <span data-ttu-id="b8b81-118">Válassza ki az objektumot, amelyeket szeretne elemzéséhez, és nyomja le az **elemzés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b8b81-118">Select the asset that you would like to analyze and press the **Analyze** button.</span></span>
   
    ![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. <span data-ttu-id="b8b81-120">Az a **folyamat media eszköz Media Analytics** ablakban válassza ki a processzor.</span><span class="sxs-lookup"><span data-stu-id="b8b81-120">In the **Process media asset with  Media Analytics** window, select the processor.</span></span> 
   
    <span data-ttu-id="b8b81-121">A többi a cikk azt ismerteti, miért és hogyan lehet minden processzor.</span><span class="sxs-lookup"><span data-stu-id="b8b81-121">The rest of the article explains why and how to use each processor.</span></span> 
5. <span data-ttu-id="b8b81-122">Nyomja le az **létrehozása** számára a feladat elindítása.</span><span class="sxs-lookup"><span data-stu-id="b8b81-122">Press **Create** to the start a job.</span></span>

## <a name="azure-media-indexer"></a><span data-ttu-id="b8b81-123">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="b8b81-123">Azure Media Indexer</span></span>
<span data-ttu-id="b8b81-124">A **Azure Media Indexer** media processzor teszi lehetővé tegye médiafájlok és a tartalom kereshető, valamint a készítése lezárt feliratok követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="b8b81-124">The **Azure Media Indexer** media processor enables you to make media files and content searchable, as well as generate closed captioning tracks.</span></span> <span data-ttu-id="b8b81-125">Ez a szakasz tájékoztatást nyújt a néhány, a felügyeleti csomag megadható beállítások.</span><span class="sxs-lookup"><span data-stu-id="b8b81-125">This sections gives some details about options that you can specify for this MP.</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a><span data-ttu-id="b8b81-127">Nyelv</span><span class="sxs-lookup"><span data-stu-id="b8b81-127">Language</span></span>
<span data-ttu-id="b8b81-128">A természetes nyelvű, hogy felismerje a multimédia fájlban.</span><span class="sxs-lookup"><span data-stu-id="b8b81-128">The natural language to be recognized in the multimedia file.</span></span> <span data-ttu-id="b8b81-129">Például angol és spanyol.</span><span class="sxs-lookup"><span data-stu-id="b8b81-129">For example, English or Spanish.</span></span> 

### <a name="captions"></a><span data-ttu-id="b8b81-130">Feliratok</span><span class="sxs-lookup"><span data-stu-id="b8b81-130">Captions</span></span>
<span data-ttu-id="b8b81-131">Kiválaszthatja a fejléc formátuma nem hoz létre a tartalomról.</span><span class="sxs-lookup"><span data-stu-id="b8b81-131">You can choose a caption format that will be generated from your content.</span></span> <span data-ttu-id="b8b81-132">Az indexelő feladat feliratfájlokat fájlok hozhat létre a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="b8b81-132">An indexing job can generate closed caption files in the following formats:</span></span>  

* <span data-ttu-id="b8b81-133">**SZÁMI**</span><span class="sxs-lookup"><span data-stu-id="b8b81-133">**SAMI**</span></span>
* <span data-ttu-id="b8b81-134">**TTML**</span><span class="sxs-lookup"><span data-stu-id="b8b81-134">**TTML**</span></span>
* <span data-ttu-id="b8b81-135">**WebVTT**</span><span class="sxs-lookup"><span data-stu-id="b8b81-135">**WebVTT**</span></span>

<span data-ttu-id="b8b81-136">Lezárt felirat (CC) az alábbi formátumokban fájlok használhatók hang- és fájlok elérhetővé a fogyatékkal élők nyújtanak segítséget.</span><span class="sxs-lookup"><span data-stu-id="b8b81-136">Closed Caption (CC) files in these formats can be used to make audio and video files accessible to people with hearing disability.</span></span>

### <a name="aib-file"></a><span data-ttu-id="b8b81-137">AIB fájl</span><span class="sxs-lookup"><span data-stu-id="b8b81-137">AIB file</span></span>
<span data-ttu-id="b8b81-138">Válassza ezt a lehetőséget, ha azt szeretné, hozza létre a hang Index Blob fájlt az egyéni SQL Server IFilter való használatra.</span><span class="sxs-lookup"><span data-stu-id="b8b81-138">Select this option if you would like to generate the Audio Index Blob file for use with the custom SQL Server IFilter.</span></span> <span data-ttu-id="b8b81-139">További információkért lásd: [ez](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span><span class="sxs-lookup"><span data-stu-id="b8b81-139">For more information, see [this](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span></span>

### <a name="keywords"></a><span data-ttu-id="b8b81-140">Kulcsszavak</span><span class="sxs-lookup"><span data-stu-id="b8b81-140">Keywords</span></span>
<span data-ttu-id="b8b81-141">Válassza ezt a lehetőséget, ha azt szeretné, a kulcsszavak XML-fájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b8b81-141">Select this option if you would like to generate a keywords XML file.</span></span> <span data-ttu-id="b8b81-142">Ez a fájl tartalmazza a beszédfelismerés tartalom kinyert gyakorisággal és oszlopeltolási információ kulcsszavak.</span><span class="sxs-lookup"><span data-stu-id="b8b81-142">This file contains keywords extracted from the speech content, with frequency and offset information.</span></span>

### <a name="job-name"></a><span data-ttu-id="b8b81-143">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="b8b81-143">Job name</span></span>
<span data-ttu-id="b8b81-144">Egy rövid nevet, amely lehetővé teszi, hogy a feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b8b81-144">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="b8b81-145">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="b8b81-145">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="b8b81-146">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="b8b81-146">Output file</span></span>
<span data-ttu-id="b8b81-147">Egy rövid nevet, amely lehetővé teszi a kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="b8b81-147">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-hyperlapse"></a><span data-ttu-id="b8b81-148">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="b8b81-148">Azure Media Hyperlapse</span></span>
<span data-ttu-id="b8b81-149">Az Azure Media Hyperlapse egy felügyeleti csomag által létrehozott zökkenőmentes idő letelt videók első, aki vagy művelet – kamera tartalomról.</span><span class="sxs-lookup"><span data-stu-id="b8b81-149">Azure Media Hyperlapse is an MP that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="b8b81-150">További információ [ebben](media-services-hyperlapse-content.md) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="b8b81-150">For more information, see [this](media-services-hyperlapse-content.md) topic.</span></span> <span data-ttu-id="b8b81-151">Ez a szakasz tájékoztatást nyújt a néhány, a felügyeleti csomag megadható beállítások.</span><span class="sxs-lookup"><span data-stu-id="b8b81-151">This sections gives some details about options that you can specify for this MP.</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a><span data-ttu-id="b8b81-153">Gyorsaság</span><span class="sxs-lookup"><span data-stu-id="b8b81-153">Speed</span></span>
<span data-ttu-id="b8b81-154">Adja meg a sebesség, amellyel a bemeneti videó felgyorsítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="b8b81-154">Specify the speed with which to speed up the input video.</span></span> <span data-ttu-id="b8b81-155">Az eredménye egy stabil és az idő letelt verzióinak a bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="b8b81-155">The output is a stabilized and time-lapsed rendition of the input video.</span></span>

### <a name="job-name"></a><span data-ttu-id="b8b81-156">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="b8b81-156">Job name</span></span>
<span data-ttu-id="b8b81-157">Egy rövid nevet, amely lehetővé teszi, hogy a feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b8b81-157">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="b8b81-158">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="b8b81-158">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="b8b81-159">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="b8b81-159">Output file</span></span>
<span data-ttu-id="b8b81-160">Egy rövid nevet, amely lehetővé teszi a kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="b8b81-160">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-face-detector"></a><span data-ttu-id="b8b81-161">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="b8b81-161">Azure Media Face Detector</span></span>
<span data-ttu-id="b8b81-162">A **Azure Media Arcfelismerési érzékelő** media processzor (MP) lehetővé teszi a száma, nyomon követésére típusú áthelyezések, és akkor is fel tudja mérni a célközönség részvételét és reakciót arcfelismerést kifejezések keresztül.</span><span class="sxs-lookup"><span data-stu-id="b8b81-162">The **Azure Media Face Detector** media processor (MP) enables you to count, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="b8b81-163">Ez a szolgáltatás két funkciókat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b8b81-163">This service contains two features:</span></span> 

* <span data-ttu-id="b8b81-164">**Arcfelismerési észlelése**</span><span class="sxs-lookup"><span data-stu-id="b8b81-164">**Face detection**</span></span>
  
    <span data-ttu-id="b8b81-165">Arcfelismerési észlelési talál, és nyomon követi a videó emberi lapjaira.</span><span class="sxs-lookup"><span data-stu-id="b8b81-165">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="b8b81-166">Több lapokat észlelhető, és ezt követően nyomon követheti az adott vissza egy JSON-fájlban ideje és helye metaadatokkal körül, mozgás.</span><span class="sxs-lookup"><span data-stu-id="b8b81-166">Multiple faces can be detected and subsequently be tracked as they move around, with the time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="b8b81-167">Nyomon követése, során a program megpróbálja adjon egységes azonosító ugyanazon felületére, amíg a személy van Navigálás a képernyőn, még akkor is, ha kényszerítő vagy röviden hagyja a keret.</span><span class="sxs-lookup"><span data-stu-id="b8b81-167">During tracking, it will attempt to give a consistent ID to the same face while the person is moving around on screen, even if they are obstructed or briefly leave the frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="b8b81-168">A szolgáltatások nem végez arcfelismerést.</span><span class="sxs-lookup"><span data-stu-id="b8b81-168">This services does not perform facial recognition.</span></span> <span data-ttu-id="b8b81-169">Személy elhagyja a keret vagy a válik fedhetik túl hosszú kap egy új Azonosítót amikor azok tér vissza.</span><span class="sxs-lookup"><span data-stu-id="b8b81-169">An individual who leaves the frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="b8b81-170">**Érzelemfelismerés**</span><span class="sxs-lookup"><span data-stu-id="b8b81-170">**Emotion detection**</span></span>
  
    <span data-ttu-id="b8b81-171">Érzelemfelismerés Arcfelismerési észlelési adathordozó-processzor elemzés több érzelmi attribútum vissza a lapok észlel, például Boldogsága, sadness, félelem, utasítás és egyéb választható összetevőként.</span><span class="sxs-lookup"><span data-stu-id="b8b81-171">Emotion Detection is an optional component of the Face Detection Media Processor that returns analysis on multiple emotional attributes from the faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a><span data-ttu-id="b8b81-173">Észlelési mód</span><span class="sxs-lookup"><span data-stu-id="b8b81-173">Detection mode</span></span>
<span data-ttu-id="b8b81-174">A következő módok egyikét használhatja a processzor:</span><span class="sxs-lookup"><span data-stu-id="b8b81-174">One of the following modes can be used by the processor:</span></span>

* <span data-ttu-id="b8b81-175">arcfelismerési észlelése</span><span class="sxs-lookup"><span data-stu-id="b8b81-175">face detection</span></span>
* <span data-ttu-id="b8b81-176">egy oldallal érzelemfelismerés</span><span class="sxs-lookup"><span data-stu-id="b8b81-176">per face emotion detection</span></span>
* <span data-ttu-id="b8b81-177">összesített érzelemfelismerés</span><span class="sxs-lookup"><span data-stu-id="b8b81-177">aggregate emotion detection</span></span>

### <a name="job-name"></a><span data-ttu-id="b8b81-178">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="b8b81-178">Job name</span></span>
<span data-ttu-id="b8b81-179">Egy rövid nevet, amely lehetővé teszi, hogy a feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b8b81-179">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="b8b81-180">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="b8b81-180">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="b8b81-181">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="b8b81-181">Output file</span></span>
<span data-ttu-id="b8b81-182">Egy rövid nevet, amely lehetővé teszi a kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="b8b81-182">A friendly name that lets you identify the output content.</span></span> 

## <a name="azure-media-motion-detector"></a><span data-ttu-id="b8b81-183">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="b8b81-183">Azure Media Motion Detector</span></span>
<span data-ttu-id="b8b81-184">A **Azure Media mozgásérzékelő** media processzor (MP) lehetővé teszi a szakaszok egy egyébként hosszú és Eseménytelen video házirendsablonokkal hatékonyan azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b8b81-184">The **Azure Media Motion Detector** media processor (MP) enables you to efficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="b8b81-185">Mozgásérzékelés statikus kamerák felvételei szakaszok a videó hol mozgásérzékelési jelentkezik azonosításához használhatja.</span><span class="sxs-lookup"><span data-stu-id="b8b81-185">Motion detection can be used on static camera footage to identify sections of the video where motion occurs.</span></span> <span data-ttu-id="b8b81-186">Az időbélyegzőket és a határoló régió, ahol az esemény történt a metaadatokat tartalmazó JSON-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b8b81-186">It generates a JSON file containing a metadata with timestamps and the bounding region where the event occurred.</span></span>

<span data-ttu-id="b8b81-187">Cél biztonsági videó hírcsatornák, ez a technológia el tudja mozgásérzékelési kategorizálása kapcsolódó eseményeket és vakriasztások például árnyékok és megvilágítási módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b8b81-187">Targeted towards security video feeds, this technology is able to categorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="b8b81-188">Ez lehetővé teszi, hogy a biztonsági riasztások generálása kamera hírcsatornák közben igényt érdeklő pillanat kinyerése rendkívül hosszú felügyeleti videók végtelen irreleváns események, éppen címünkre nélkül.</span><span class="sxs-lookup"><span data-stu-id="b8b81-188">This allows you to generate security alerts from camera feeds without being spammed with endless irrelevant events, while being able to extract moments of interest from extremely long surveillance videos.</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a><span data-ttu-id="b8b81-190">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="b8b81-190">Azure Media Video Thumbnails</span></span>
<span data-ttu-id="b8b81-191">A processzor segítségével létre videók kijelölésével automatikusan érdekes kódtöredékek a forrás-videót.</span><span class="sxs-lookup"><span data-stu-id="b8b81-191">This processor can help you create summaries of long videos by automatically selecting interesting snippets from the source video.</span></span> <span data-ttu-id="b8b81-192">Ez akkor hasznos, ha lehetővé szeretné tenni, mi történik, a hosszú videó gyors áttekintést.</span><span class="sxs-lookup"><span data-stu-id="b8b81-192">This is useful when you want to provide a quick overview of what to expect in a long video.</span></span> <span data-ttu-id="b8b81-193">Részletes útmutatást és példákat, lásd: [használata Azure Media Videoindexképek használatával egy videó összegzésének létrehozása](media-services-video-summarization.md)</span><span class="sxs-lookup"><span data-stu-id="b8b81-193">For detailed information and examples, see [Use Azure Media Video Thumbnails to Create a Video Summarization](media-services-video-summarization.md)</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a><span data-ttu-id="b8b81-195">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="b8b81-195">Job name</span></span>
<span data-ttu-id="b8b81-196">Egy rövid nevet, amely lehetővé teszi, hogy a feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="b8b81-196">A friendly name that lets you identify the job.</span></span> <span data-ttu-id="b8b81-197">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="b8b81-197">[This](media-services-portal-check-job-progress.md) article describes how you can monitor the progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="b8b81-198">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="b8b81-198">Output file</span></span>
<span data-ttu-id="b8b81-199">Egy rövid nevet, amely lehetővé teszi a kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="b8b81-199">A friendly name that lets you identify the output content.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="b8b81-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8b81-200">Next steps</span></span>
<span data-ttu-id="b8b81-201">Nézet Media Services tanulási útvonalai.</span><span class="sxs-lookup"><span data-stu-id="b8b81-201">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b8b81-202">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b8b81-202">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


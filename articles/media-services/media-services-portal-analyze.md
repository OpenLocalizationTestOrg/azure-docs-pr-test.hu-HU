---
title: "aaaAnalyze az adathordozó használatával hello Azure portálon |} Microsoft Docs"
description: "Ez a témakör ismerteti hogyan tooprocess a Médiaelemzés media processzor (felügyeleti csomagok) használatával médiatartalmakat hello Azure-portálon."
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
ms.openlocfilehash: d549c0315cd58c3771420379316b4f2ad3c2cea6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-your-media-using-hello-azure-portal"></a><span data-ttu-id="0abd4-103">Elemezze az adathordozó hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="0abd4-103">Analyze your media using hello Azure portal</span></span>
> [!NOTE]
> <span data-ttu-id="0abd4-104">toocomplete ebben az oktatóanyagban egy Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0abd4-104">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="0abd4-105">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0abd4-105">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
> 
> 

## <a name="overview"></a><span data-ttu-id="0abd4-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0abd4-106">Overview</span></span>
<span data-ttu-id="0abd4-107">Azure Services Médiaelemzés beszéd- és vizuális összetevők (a vállalati méretű, megfelelőség, biztonsági és globális reach), amelyek megkönnyítik a szervezetek és vállalatok számára tooderive gyakorlatban használható elemzések készítsenek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="0abd4-107">Azure Media Services Analytics is a collection of speech and vision components (at enterprise scale, compliance, security and global reach) that make it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="0abd4-108">További részletes Azure Media Services Analytics áttekintése: [ez](media-services-analytics-overview.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="0abd4-108">For more detailed overview of Azure Media Services Analytics see [this](media-services-analytics-overview.md) topic.</span></span> 

<span data-ttu-id="0abd4-109">Ez a témakör ismerteti hogyan tooprocess a Médiaelemzés media processzor (felügyeleti csomagok) használatával médiatartalmakat hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0abd4-109">This topic discusses how tooprocess your media with Media Analytics media processors (MPs) using hello Azure portal.</span></span> <span data-ttu-id="0abd4-110">Media Analytics felügyeleti csomagok MP4- vagy JSON-fájlok előállításához.</span><span class="sxs-lookup"><span data-stu-id="0abd4-110">Media Analytics MPs produce MP4 files or JSON files.</span></span> <span data-ttu-id="0abd4-111">Ha egy media processzor MP4-fájlokat, hello fájl fokozatosan lehet letölteni.</span><span class="sxs-lookup"><span data-stu-id="0abd4-111">If a media processor produced an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="0abd4-112">Ha egy media processzor egy JSON-fájl, hello fájlt letöltheti hello Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="0abd4-112">If a media processor produced a JSON file, you can download hello file from hello Azure blob storage.</span></span> 

## <a name="choose-an-asset-that-you-want-tooanalyze"></a><span data-ttu-id="0abd4-113">Válasszon egy eszköz, amelyet az tooanalyze</span><span class="sxs-lookup"><span data-stu-id="0abd4-113">Choose an asset that you want tooanalyze</span></span>
1. <span data-ttu-id="0abd4-114">A hello [Azure-portálon](https://portal.azure.com/), válassza ki az Azure Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="0abd4-114">In hello [Azure portal](https://portal.azure.com/), select your Azure Media Services account.</span></span>
2. <span data-ttu-id="0abd4-115">A hello **beállítások** ablakban válassza ki **eszközök**.</span><span class="sxs-lookup"><span data-stu-id="0abd4-115">In hello **Settings** window, select **Assets**.</span></span>  
   <span data-ttu-id="0abd4-116">.</span><span class="sxs-lookup"><span data-stu-id="0abd4-116">.</span></span>
    <span data-ttu-id="0abd4-117">![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span><span class="sxs-lookup"><span data-stu-id="0abd4-117">![Analyze videos](./media/media-services-portal-analyze/media-services-portal-analyze001.png)</span></span>
3. <span data-ttu-id="0abd4-118">Jelölje be hello eszköz, amelyet tooanalyze, és nyomja le az hello **elemzés** gombra.</span><span class="sxs-lookup"><span data-stu-id="0abd4-118">Select hello asset that you would like tooanalyze and press hello **Analyze** button.</span></span>
   
    ![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze002.png)
4. <span data-ttu-id="0abd4-120">A hello **folyamat media eszköz Media Analytics** ablakban, a select hello processzor.</span><span class="sxs-lookup"><span data-stu-id="0abd4-120">In hello **Process media asset with  Media Analytics** window, select hello processor.</span></span> 
   
    <span data-ttu-id="0abd4-121">hello rest hello cikk azt ismerteti, miért és hogyan toouse minden processzor.</span><span class="sxs-lookup"><span data-stu-id="0abd4-121">hello rest of hello article explains why and how toouse each processor.</span></span> 
5. <span data-ttu-id="0abd4-122">Nyomja le az **létrehozása** toohello feladat indítása.</span><span class="sxs-lookup"><span data-stu-id="0abd4-122">Press **Create** toohello start a job.</span></span>

## <a name="azure-media-indexer"></a><span data-ttu-id="0abd4-123">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="0abd4-123">Azure Media Indexer</span></span>
<span data-ttu-id="0abd4-124">Hello **Azure Media Indexer** media processzor lehetővé teszi toomake médiafájlok és a tartalom kereshető, valamint készítése lezárt feliratok nyomon követi.</span><span class="sxs-lookup"><span data-stu-id="0abd4-124">hello **Azure Media Indexer** media processor enables you toomake media files and content searchable, as well as generate closed captioning tracks.</span></span> <span data-ttu-id="0abd4-125">Ez a szakasz tájékoztatást nyújt a néhány, a felügyeleti csomag megadható beállítások.</span><span class="sxs-lookup"><span data-stu-id="0abd4-125">This sections gives some details about options that you can specify for this MP.</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a><span data-ttu-id="0abd4-127">Nyelv</span><span class="sxs-lookup"><span data-stu-id="0abd4-127">Language</span></span>
<span data-ttu-id="0abd4-128">hello természetes nyelvű toobe hello multimédiás fájlban ismerhető fel.</span><span class="sxs-lookup"><span data-stu-id="0abd4-128">hello natural language toobe recognized in hello multimedia file.</span></span> <span data-ttu-id="0abd4-129">Például angol és spanyol.</span><span class="sxs-lookup"><span data-stu-id="0abd4-129">For example, English or Spanish.</span></span> 

### <a name="captions"></a><span data-ttu-id="0abd4-130">Feliratok</span><span class="sxs-lookup"><span data-stu-id="0abd4-130">Captions</span></span>
<span data-ttu-id="0abd4-131">Kiválaszthatja a fejléc formátuma nem hoz létre a tartalomról.</span><span class="sxs-lookup"><span data-stu-id="0abd4-131">You can choose a caption format that will be generated from your content.</span></span> <span data-ttu-id="0abd4-132">Az indexelő feladat feliratfájlokat fájlok hozhat létre a következő formátumok hello:</span><span class="sxs-lookup"><span data-stu-id="0abd4-132">An indexing job can generate closed caption files in hello following formats:</span></span>  

* <span data-ttu-id="0abd4-133">**SZÁMI**</span><span class="sxs-lookup"><span data-stu-id="0abd4-133">**SAMI**</span></span>
* <span data-ttu-id="0abd4-134">**TTML**</span><span class="sxs-lookup"><span data-stu-id="0abd4-134">**TTML**</span></span>
* <span data-ttu-id="0abd4-135">**WebVTT**</span><span class="sxs-lookup"><span data-stu-id="0abd4-135">**WebVTT**</span></span>

<span data-ttu-id="0abd4-136">Bezárt felirat (CC) fájlok az alábbi formátumokban lehet használt toomake hang, és a videó elérhető toopeople nyújtanak segítséget fogyatékkal fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0abd4-136">Closed Caption (CC) files in these formats can be used toomake audio and video files accessible toopeople with hearing disability.</span></span>

### <a name="aib-file"></a><span data-ttu-id="0abd4-137">AIB fájl</span><span class="sxs-lookup"><span data-stu-id="0abd4-137">AIB file</span></span>
<span data-ttu-id="0abd4-138">Válassza ezt a lehetőséget, ha meg szeretné toogenerate hello hang Index Blob fájlt az például hello egyéni SQL Server IFilter.</span><span class="sxs-lookup"><span data-stu-id="0abd4-138">Select this option if you would like toogenerate hello Audio Index Blob file for use with hello custom SQL Server IFilter.</span></span> <span data-ttu-id="0abd4-139">További információkért lásd: [ez](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span><span class="sxs-lookup"><span data-stu-id="0abd4-139">For more information, see [this](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blog.</span></span>

### <a name="keywords"></a><span data-ttu-id="0abd4-140">Kulcsszavak</span><span class="sxs-lookup"><span data-stu-id="0abd4-140">Keywords</span></span>
<span data-ttu-id="0abd4-141">Válassza ezt a lehetőséget, ha azt szeretné, hogy toogenerate egy kulcsszavak XML-fájlt.</span><span class="sxs-lookup"><span data-stu-id="0abd4-141">Select this option if you would like toogenerate a keywords XML file.</span></span> <span data-ttu-id="0abd4-142">Ez a fájl kulcsszavak hello beszéd tartalom, kinyert gyakorisággal és oszlopeltolási információ tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0abd4-142">This file contains keywords extracted from hello speech content, with frequency and offset information.</span></span>

### <a name="job-name"></a><span data-ttu-id="0abd4-143">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="0abd4-143">Job name</span></span>
<span data-ttu-id="0abd4-144">Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="0abd4-144">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="0abd4-145">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello.</span><span class="sxs-lookup"><span data-stu-id="0abd4-145">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="0abd4-146">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="0abd4-146">Output file</span></span>
<span data-ttu-id="0abd4-147">Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="0abd4-147">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-hyperlapse"></a><span data-ttu-id="0abd4-148">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="0abd4-148">Azure Media Hyperlapse</span></span>
<span data-ttu-id="0abd4-149">Az Azure Media Hyperlapse egy felügyeleti csomag által létrehozott zökkenőmentes idő letelt videók első, aki vagy művelet – kamera tartalomról.</span><span class="sxs-lookup"><span data-stu-id="0abd4-149">Azure Media Hyperlapse is an MP that creates smooth time-lapsed videos from first-person or action-camera content.</span></span>  <span data-ttu-id="0abd4-150">További információ [ebben](media-services-hyperlapse-content.md) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="0abd4-150">For more information, see [this](media-services-hyperlapse-content.md) topic.</span></span> <span data-ttu-id="0abd4-151">Ez a szakasz tájékoztatást nyújt a néhány, a felügyeleti csomag megadható beállítások.</span><span class="sxs-lookup"><span data-stu-id="0abd4-151">This sections gives some details about options that you can specify for this MP.</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a><span data-ttu-id="0abd4-153">Gyorsaság</span><span class="sxs-lookup"><span data-stu-id="0abd4-153">Speed</span></span>
<span data-ttu-id="0abd4-154">Adja meg hello sebesség mely toospeed hello bemeneti videó fel.</span><span class="sxs-lookup"><span data-stu-id="0abd4-154">Specify hello speed with which toospeed up hello input video.</span></span> <span data-ttu-id="0abd4-155">hello eredménye egy stabil és az idő letelt verzióinak hello bemeneti videó.</span><span class="sxs-lookup"><span data-stu-id="0abd4-155">hello output is a stabilized and time-lapsed rendition of hello input video.</span></span>

### <a name="job-name"></a><span data-ttu-id="0abd4-156">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="0abd4-156">Job name</span></span>
<span data-ttu-id="0abd4-157">Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="0abd4-157">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="0abd4-158">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello.</span><span class="sxs-lookup"><span data-stu-id="0abd4-158">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="0abd4-159">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="0abd4-159">Output file</span></span>
<span data-ttu-id="0abd4-160">Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="0abd4-160">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-face-detector"></a><span data-ttu-id="0abd4-161">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="0abd4-161">Azure Media Face Detector</span></span>
<span data-ttu-id="0abd4-162">Hello **Azure Media Arcfelismerési érzékelő** media processzor (MP) lehetővé teszi a toocount, nyomon követése típusú áthelyezések, és még a mérőműszer célközönség részvételét és reakciót arcfelismerést kifejezések keresztül.</span><span class="sxs-lookup"><span data-stu-id="0abd4-162">hello **Azure Media Face Detector** media processor (MP) enables you toocount, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="0abd4-163">Ez a szolgáltatás két funkciókat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="0abd4-163">This service contains two features:</span></span> 

* <span data-ttu-id="0abd4-164">**Arcfelismerési észlelése**</span><span class="sxs-lookup"><span data-stu-id="0abd4-164">**Face detection**</span></span>
  
    <span data-ttu-id="0abd4-165">Arcfelismerési észlelési talál, és nyomon követi a videó emberi lapjaira.</span><span class="sxs-lookup"><span data-stu-id="0abd4-165">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="0abd4-166">Több lapokat észlelhető, és ezt követően nyomon követhetők egy JSON-fájl által visszaadott hello ideje és helye metaadatokkal körül, mozgás.</span><span class="sxs-lookup"><span data-stu-id="0abd4-166">Multiple faces can be detected and subsequently be tracked as they move around, with hello time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="0abd4-167">Nyomon követése, során a program megpróbálja toogive egy egységes azonosító toohello azonos szembesülhetnek, amíg hello személy van Navigálás a képernyőn, még akkor is, ha kényszerítő vagy röviden hagyja hello keret.</span><span class="sxs-lookup"><span data-stu-id="0abd4-167">During tracking, it will attempt toogive a consistent ID toohello same face while hello person is moving around on screen, even if they are obstructed or briefly leave hello frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="0abd4-168">A szolgáltatások nem végez arcfelismerést.</span><span class="sxs-lookup"><span data-stu-id="0abd4-168">This services does not perform facial recognition.</span></span> <span data-ttu-id="0abd4-169">Személy hello keret hagyja, vagy a válik fedhetik túl hosszú kap egy új Azonosítót amikor azok tér vissza.</span><span class="sxs-lookup"><span data-stu-id="0abd4-169">An individual who leaves hello frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="0abd4-170">**Érzelemfelismerés**</span><span class="sxs-lookup"><span data-stu-id="0abd4-170">**Emotion detection**</span></span>
  
    <span data-ttu-id="0abd4-171">Érzelemfelismerés hello Arcfelismerési észlelési Media processzor ad vissza elemzés több érzelmi attribútum hello lapok észlel, például Boldogsága, sadness, félelem, utasítás és egyéb választható összetevőként.</span><span class="sxs-lookup"><span data-stu-id="0abd4-171">Emotion Detection is an optional component of hello Face Detection Media Processor that returns analysis on multiple emotional attributes from hello faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a><span data-ttu-id="0abd4-173">Észlelési mód</span><span class="sxs-lookup"><span data-stu-id="0abd4-173">Detection mode</span></span>
<span data-ttu-id="0abd4-174">A következő módok hello egyik hello processzor használhatja:</span><span class="sxs-lookup"><span data-stu-id="0abd4-174">One of hello following modes can be used by hello processor:</span></span>

* <span data-ttu-id="0abd4-175">arcfelismerési észlelése</span><span class="sxs-lookup"><span data-stu-id="0abd4-175">face detection</span></span>
* <span data-ttu-id="0abd4-176">egy oldallal érzelemfelismerés</span><span class="sxs-lookup"><span data-stu-id="0abd4-176">per face emotion detection</span></span>
* <span data-ttu-id="0abd4-177">összesített érzelemfelismerés</span><span class="sxs-lookup"><span data-stu-id="0abd4-177">aggregate emotion detection</span></span>

### <a name="job-name"></a><span data-ttu-id="0abd4-178">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="0abd4-178">Job name</span></span>
<span data-ttu-id="0abd4-179">Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="0abd4-179">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="0abd4-180">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello.</span><span class="sxs-lookup"><span data-stu-id="0abd4-180">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="0abd4-181">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="0abd4-181">Output file</span></span>
<span data-ttu-id="0abd4-182">Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="0abd4-182">A friendly name that lets you identify hello output content.</span></span> 

## <a name="azure-media-motion-detector"></a><span data-ttu-id="0abd4-183">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="0abd4-183">Azure Media Motion Detector</span></span>
<span data-ttu-id="0abd4-184">Hello **Azure Media mozgásérzékelő** media processzor (MP) lehetővé teszi, hogy Ön tooefficiently azonosítani egy egyébként hosszú és Eseménytelen video házirendsablonokkal szakaszait.</span><span class="sxs-lookup"><span data-stu-id="0abd4-184">hello **Azure Media Motion Detector** media processor (MP) enables you tooefficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="0abd4-185">Mozgásérzékelés statikus kamera felvételei tooidentify szakaszok hello videó hol mozgásérzékelési jelentkezik is használhatók.</span><span class="sxs-lookup"><span data-stu-id="0abd4-185">Motion detection can be used on static camera footage tooidentify sections of hello video where motion occurs.</span></span> <span data-ttu-id="0abd4-186">A metaadatok időbélyegeket és a régió, ahol hello esemény történt határolókeret hello tartalmazó JSON-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0abd4-186">It generates a JSON file containing a metadata with timestamps and hello bounding region where hello event occurred.</span></span>

<span data-ttu-id="0abd4-187">Cél biztonsági videó hírcsatornák, ez a technológia képes toocategorize mozgásérzékelési kapcsolódó eseményeket és vakriasztások például árnyékok és megvilágítási módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0abd4-187">Targeted towards security video feeds, this technology is able toocategorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="0abd4-188">Ez lehetővé teszi a kamera hírcsatornák toogenerate biztonsági riasztások alatt címünkre végtelen irreleváns események, ugyanakkor nem képes tooextract perc múlva a rendkívül hosszú felügyeleti videók érdeklő nélkül.</span><span class="sxs-lookup"><span data-stu-id="0abd4-188">This allows you toogenerate security alerts from camera feeds without being spammed with endless irrelevant events, while being able tooextract moments of interest from extremely long surveillance videos.</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a><span data-ttu-id="0abd4-190">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="0abd4-190">Azure Media Video Thumbnails</span></span>
<span data-ttu-id="0abd4-191">A processzor segítségével létre videók kijelölésével automatikusan érdekes kódtöredékek hello forrás videó.</span><span class="sxs-lookup"><span data-stu-id="0abd4-191">This processor can help you create summaries of long videos by automatically selecting interesting snippets from hello source video.</span></span> <span data-ttu-id="0abd4-192">Ez akkor hasznos, ha azt szeretné, hogy milyen hosszú videó tooexpect gyors áttekintést tooprovide.</span><span class="sxs-lookup"><span data-stu-id="0abd4-192">This is useful when you want tooprovide a quick overview of what tooexpect in a long video.</span></span> <span data-ttu-id="0abd4-193">Részletes útmutatást és példákat, lásd: [használata Azure Media Videoindexképek tooCreate egy videó összegzésének](media-services-video-summarization.md)</span><span class="sxs-lookup"><span data-stu-id="0abd4-193">For detailed information and examples, see [Use Azure Media Video Thumbnails tooCreate a Video Summarization](media-services-video-summarization.md)</span></span>

![Videók elemzése](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a><span data-ttu-id="0abd4-195">Feladat neve</span><span class="sxs-lookup"><span data-stu-id="0abd4-195">Job name</span></span>
<span data-ttu-id="0abd4-196">Egy rövid nevet, amely lehetővé teszi a hello feladat azonosításához.</span><span class="sxs-lookup"><span data-stu-id="0abd4-196">A friendly name that lets you identify hello job.</span></span> <span data-ttu-id="0abd4-197">[Ez](media-services-portal-check-job-progress.md) a cikk ismerteti, hogyan figyelheti a feladat előrehaladását hello.</span><span class="sxs-lookup"><span data-stu-id="0abd4-197">[This](media-services-portal-check-job-progress.md) article describes how you can monitor hello progress of a job.</span></span> 

### <a name="output-file"></a><span data-ttu-id="0abd4-198">Kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="0abd4-198">Output file</span></span>
<span data-ttu-id="0abd4-199">Egy rövid nevet, amely lehetővé teszi a hello kimeneti tartalmat.</span><span class="sxs-lookup"><span data-stu-id="0abd4-199">A friendly name that lets you identify hello output content.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="0abd4-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0abd4-200">Next steps</span></span>
<span data-ttu-id="0abd4-201">Nézet Media Services tanulási útvonalai.</span><span class="sxs-lookup"><span data-stu-id="0abd4-201">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0abd4-202">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0abd4-202">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


---
title: "az Azure Médiaelemzés használatával forgatókönyv aaaRedact lapok |} Microsoft Docs"
description: "Ez a témakör ismerteti részletesen hogyan toorun egy teljes kivonási munkafolyamat Azure Media Services Explorer (AMSE) és az Azure Media Redactor Vizualizálója (nyílt forráskódú eszköz) használatával."
services: media-services
documentationcenter: 
author: Lichard
manager: cfowler
editor: 
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/03/2017
ms.author: rli; juliako;
ms.openlocfilehash: ab28f4052b73fdb74fcd5766235eab35402a0c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a><span data-ttu-id="84c87-103">Az Azure Médiaelemzés használatával forgatókönyv lapok kivonása</span><span class="sxs-lookup"><span data-stu-id="84c87-103">Redact faces with Azure Media Analytics walkthrough</span></span>

## <a name="overview"></a><span data-ttu-id="84c87-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="84c87-104">Overview</span></span>

<span data-ttu-id="84c87-105">**Az Azure Media Redactor** van egy [Azure Médiaelemzés használatával](media-services-analytics-overview.md) media processzor (MP), amely méretezhető arcfelismerési kivonási hello felhőben nyújt.</span><span class="sxs-lookup"><span data-stu-id="84c87-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="84c87-106">Arcfelismerési kivonási lehetővé teszi, hogy Ön toomodify a rendelés tooblur felületei kijelölt személyek a videó.</span><span class="sxs-lookup"><span data-stu-id="84c87-106">Face redaction enables you toomodify your video in order tooblur faces of selected individuals.</span></span> <span data-ttu-id="84c87-107">Érdemes lehet toouse hello arcfelismerési kivonási szolgáltatás nyilvános biztonsági és hírek media forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="84c87-107">You may want toouse hello face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="84c87-108">Több lapokat tartalmazó felvételei, néhány percet is igénybe vehet óra tooredact manuálisan, de a szolgáltatás hello arcfelismerési a kivonási folyamat néhány egyszerű lépésben szükséges.</span><span class="sxs-lookup"><span data-stu-id="84c87-108">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service hello face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="84c87-109">További információkért lásd: [ez](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span><span class="sxs-lookup"><span data-stu-id="84c87-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="84c87-110">Vonatkozó további információért **Azure Media Redactor**, lásd: hello [Arcfelismerési kivonási áttekintése](media-services-face-redaction.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="84c87-110">For details about  **Azure Media Redactor**, see hello [Face redaction overview](media-services-face-redaction.md) topic.</span></span>

<span data-ttu-id="84c87-111">Ez a témakör ismerteti részletesen hogyan toorun egy teljes kivonási munkafolyamat Azure Media Services Explorer (AMSE) és az Azure Media Redactor Vizualizálója (nyílt forráskódú eszköz) használatával.</span><span class="sxs-lookup"><span data-stu-id="84c87-111">This topic shows step by step instructions on how toorun a full redaction workflow using Azure Media Services Explorer (AMSE) and Azure Media Redactor Visualizer (open source tool).</span></span>

<span data-ttu-id="84c87-112">Hello **Azure Media Redactor** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="84c87-112">hello **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="84c87-113">Érhető el az összes Azure-régiók, valamint Amerikai Egyesült államokbeli kormányzati és Kína adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="84c87-113">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="84c87-114">Ez az előnézet jelenleg díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="84c87-114">This preview is currently free of charge.</span></span> <span data-ttu-id="84c87-115">Hello a jelenlegi kiadásban 10 perces korlátozva van a feldolgozott videó hossza.</span><span class="sxs-lookup"><span data-stu-id="84c87-115">In hello current release, there is a 10 minute limit on processed video length.</span></span>

<span data-ttu-id="84c87-116">További információkért lásd: [ez](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.</span><span class="sxs-lookup"><span data-stu-id="84c87-116">For more information, see [this](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.</span></span>

## <a name="azure-media-services-explorer-workflow"></a><span data-ttu-id="84c87-117">Az Azure Media Services Explorer munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="84c87-117">Azure Media Services Explorer workflow</span></span>

<span data-ttu-id="84c87-118">hello legegyszerűbb módja tooget használatába Redactor toouse hello nyílt forráskódú AMSE eszköz a githubon.</span><span class="sxs-lookup"><span data-stu-id="84c87-118">hello easiest way tooget started with Redactor is toouse hello open source AMSE tool on github.</span></span> <span data-ttu-id="84c87-119">Keresztül egyszerűsített munkafolyamat futtatása **kombinált** mód, ha már nem kell használni toohello jegyzet json vagy hello arcfelismerési jpg lemezképet.</span><span class="sxs-lookup"><span data-stu-id="84c87-119">You can run a simplified workflow via **combined** mode if you don’t need access toohello annotation json or hello face jpg images.</span></span>

### <a name="download-and-setup"></a><span data-ttu-id="84c87-120">A letöltés és telepítés</span><span class="sxs-lookup"><span data-stu-id="84c87-120">Download and setup</span></span>

1. <span data-ttu-id="84c87-121">Töltse le az AMSE eszköz hello [Itt](https://github.com/Azure/Azure-Media-Services-Explorer).</span><span class="sxs-lookup"><span data-stu-id="84c87-121">Download hello AMSE tool from [here](https://github.com/Azure/Azure-Media-Services-Explorer).</span></span>
1. <span data-ttu-id="84c87-122">Jelentkezzen be a szolgáltatás kulccsal Media Services-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="84c87-122">Log in tooyour Media Services account using your service key.</span></span>

    <span data-ttu-id="84c87-123">tooobtain hello fióknevet és a kulcsadatokat, nyissa meg toohello [Azure-portálon](https://portal.azure.com/) válassza ki az AMS-fiók.</span><span class="sxs-lookup"><span data-stu-id="84c87-123">tooobtain hello account name and key information, go toohello [Azure portal](https://portal.azure.com/) and select your AMS account.</span></span> <span data-ttu-id="84c87-124">Válassza a beállítások > kulcsok.</span><span class="sxs-lookup"><span data-stu-id="84c87-124">Then, select Settings > Keys.</span></span> <span data-ttu-id="84c87-125">hello kezelése kulcsok windows hello-fiók nevét jeleníti meg, és hello elsődleges és másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="84c87-125">hello Manage keys windows shows hello account name and hello primary and secondary keys is displayed.</span></span> <span data-ttu-id="84c87-126">Másolja a hello fióknevet és a hello elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="84c87-126">Copy values of hello account name and hello primary key.</span></span>

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a><span data-ttu-id="84c87-128">Először adja át – mód elemzése</span><span class="sxs-lookup"><span data-stu-id="84c87-128">First pass – analyze mode</span></span>

1. <span data-ttu-id="84c87-129">A médiafájl keresztül eszköz feltöltés –> feltöltési, sem a fogd és vidd segítségével.</span><span class="sxs-lookup"><span data-stu-id="84c87-129">Upload your media file through Asset –> Upload, or via drag and drop.</span></span> 
1. <span data-ttu-id="84c87-130">Kattintson a jobb gombbal, és dolgozza fel a médiafájl Médiaelemzés használatával Azure Media Redactor –> –> elemzési módot.</span><span class="sxs-lookup"><span data-stu-id="84c87-130">Right click and process your media file using Media Analytics –> Azure Media Redactor –> Analyze mode.</span></span> 


![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

<span data-ttu-id="84c87-133">hello kimeneti egy jegyzetek json-fájl, arc helyadatok, valamint minden észlelt felületen jpg tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="84c87-133">hello output will include an annotations json file with face location data, as well as a jpg of each detected face.</span></span> 

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a><span data-ttu-id="84c87-135">Második fázis – mód kivonása</span><span class="sxs-lookup"><span data-stu-id="84c87-135">Second pass – redact mode</span></span>

1. <span data-ttu-id="84c87-136">Töltse fel az eredeti video asset toohello hello első fázis kimenetét, és állítsa be elsődleges eszközként.</span><span class="sxs-lookup"><span data-stu-id="84c87-136">Upload your original video asset toohello output from hello first pass and set as a primary asset.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. <span data-ttu-id="84c87-138">(Választható) Töltse fel egy "Dance_idlist.txt" fájlt, amely hello tooredact kívánja azonosítók Sortöréssel elválasztott listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="84c87-138">(Optional) Upload a 'Dance_idlist.txt' file which includes a newline delimited list of hello IDs you wish tooredact.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. <span data-ttu-id="84c87-140">(Választható) Hogy bármely módosításokat toohello annotations.json fájl például növelése hello határoló mező határait.</span><span class="sxs-lookup"><span data-stu-id="84c87-140">(Optional) Make any edits toohello annotations.json file such as increasing hello bounding box boundaries.</span></span> 
4. <span data-ttu-id="84c87-141">Hello kimeneti eszköz az első fázis hello kattintson a jobb gombbal, válassza ki a hello Redactor, futtassa a hello **Redact** mód.</span><span class="sxs-lookup"><span data-stu-id="84c87-141">Right click hello output asset from hello first pass, select hello Redactor, and run with hello **Redact** mode.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. <span data-ttu-id="84c87-143">Töltse le, vagy a megosztási hello végső kivont kimenetet eszköz.</span><span class="sxs-lookup"><span data-stu-id="84c87-143">Download or share hello final redacted output asset.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a><span data-ttu-id="84c87-145">Azure Media Redactor Vizualizálója nyílt forráskódú eszköz</span><span class="sxs-lookup"><span data-stu-id="84c87-145">Azure Media Redactor Visualizer open source tool</span></span>

<span data-ttu-id="84c87-146">Egy nyílt forráskódú [vizualizálója eszköz](https://github.com/Microsoft/azure-media-redactor-visualizer) nemrég kezdte hello jegyzetek formátumú elemzése, és hello kimeneti használatával kialakított toohelp fejlesztők.</span><span class="sxs-lookup"><span data-stu-id="84c87-146">An open source [visualizer tool](https://github.com/Microsoft/azure-media-redactor-visualizer) is designed toohelp developers just starting with hello annotations format with parsing and using hello output.</span></span>

<span data-ttu-id="84c87-147">Hello tárház rendelés toorun hello projektben klónozását kell toodownload FFMPEG a saját [hivatalos webhely](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="84c87-147">After you clone hello repo, in order toorun hello project, you will need toodownload FFMPEG from their [official site](https://ffmpeg.org/download.html).</span></span>

<span data-ttu-id="84c87-148">Ha egy fejlesztő tooparse hello JSON jegyzet adatokat próbált, nyissa meg Models.MetaData minta kód példákat.</span><span class="sxs-lookup"><span data-stu-id="84c87-148">If you are a developer trying tooparse hello JSON annotation data, look inside Models.MetaData for sample code examples.</span></span>

### <a name="set-up-hello-tool"></a><span data-ttu-id="84c87-149">Hello eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="84c87-149">Set up hello tool</span></span>

1.  <span data-ttu-id="84c87-150">Töltse le és hello teljes megoldás felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="84c87-150">Download and build hello entire solution.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  <span data-ttu-id="84c87-152">Töltse le a FFMPEG [Itt](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="84c87-152">Download FFMPEG from [here](https://ffmpeg.org/download.html).</span></span> <span data-ttu-id="84c87-153">Ez a projekt eredetileg fejlesztettek verzió be1d324 (2016-10-04) a statikus hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="84c87-153">This project was originally developed with version be1d324 (2016-10-04) with static linking.</span></span> 
3.  <span data-ttu-id="84c87-154">Másolja a ffmpeg.exe és ffprobe.exe toohello AzureMediaRedactor.exe megegyező kimeneti mappában.</span><span class="sxs-lookup"><span data-stu-id="84c87-154">Copy ffmpeg.exe and ffprobe.exe toohello same output folder as AzureMediaRedactor.exe.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. <span data-ttu-id="84c87-156">Futtassa a AzureMediaRedactor.exe.</span><span class="sxs-lookup"><span data-stu-id="84c87-156">Run AzureMediaRedactor.exe.</span></span> 

### <a name="use-hello-tool"></a><span data-ttu-id="84c87-157">Hello eszközzel</span><span class="sxs-lookup"><span data-stu-id="84c87-157">Use hello tool</span></span>

1. <span data-ttu-id="84c87-158">A videó az Azure Media Services-fiókban hello Redactor felügyeleti csomag az elemzési módot a feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="84c87-158">Process your video in your Azure Media Services account with hello Redactor MP on Analyze mode.</span></span> 
2. <span data-ttu-id="84c87-159">Töltse le a hello eredeti videofájl és a kivonási hello hello kimenete, mert a feladat elemzése.</span><span class="sxs-lookup"><span data-stu-id="84c87-159">Download both hello original video file and hello output of hello Redaction - Analyze job.</span></span> 
3. <span data-ttu-id="84c87-160">Hello vizualizálója alkalmazás futtatásához, és válassza ki a fenti hello fájlok.</span><span class="sxs-lookup"><span data-stu-id="84c87-160">Run hello visualizer application and choose hello files above.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. <span data-ttu-id="84c87-162">Tekintse meg a fájlt.</span><span class="sxs-lookup"><span data-stu-id="84c87-162">Preview your file.</span></span> <span data-ttu-id="84c87-163">Válassza ki azt az oldal akkor tooblur hello oldalsávon a hello keresztül szeretné jobb.</span><span class="sxs-lookup"><span data-stu-id="84c87-163">Select which faces you'd like tooblur via hello sidebar on hello right.</span></span> 
    
    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  <span data-ttu-id="84c87-165">hello alsó szövegmező hello arcfelismerési azonosítók frissíteni fogja.</span><span class="sxs-lookup"><span data-stu-id="84c87-165">hello bottom text field will update with hello face IDs.</span></span> <span data-ttu-id="84c87-166">Ezek az azonosítók "idlist.txt" nevű fájl létrehozása Sortöréssel elválasztott listáját.</span><span class="sxs-lookup"><span data-stu-id="84c87-166">Create a file called "idlist.txt" with these IDs as a newline delimited list.</span></span> 

    >[!NOTE]
    > <span data-ttu-id="84c87-167">ANSI hello idlist.txt kell menteni.</span><span class="sxs-lookup"><span data-stu-id="84c87-167">hello idlist.txt should be saved in ANSI.</span></span> <span data-ttu-id="84c87-168">A Jegyzettömb toosave ANSI használható.</span><span class="sxs-lookup"><span data-stu-id="84c87-168">You can use notepad toosave in ANSI.</span></span>
    
6.  <span data-ttu-id="84c87-169">Töltse fel a fájl toohello kimeneti adategységen 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="84c87-169">Upload this file toohello output asset from step 1.</span></span> <span data-ttu-id="84c87-170">Töltse fel a hello eredeti videó toothis eszköz is, és állítsa be elsődleges eszközként.</span><span class="sxs-lookup"><span data-stu-id="84c87-170">Upload hello original video toothis asset as well and set as primary asset.</span></span> 
7.  <span data-ttu-id="84c87-171">Ez az eszköz "Redact" mód tooget hello végső kivont videó kivonási feladat fut.</span><span class="sxs-lookup"><span data-stu-id="84c87-171">Run Redaction job on this asset with "Redact" mode tooget hello final redacted video.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="84c87-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84c87-172">Next steps</span></span> 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="84c87-173">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="84c87-173">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="84c87-174">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="84c87-174">Related links</span></span>
[<span data-ttu-id="84c87-175">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="84c87-175">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="84c87-176">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="84c87-176">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[<span data-ttu-id="84c87-177">Az Azure Médiaelemzés használatával Arcfelismerési kivonási bejelentése</span><span class="sxs-lookup"><span data-stu-id="84c87-177">Announcing Face Redaction for Azure Media Analytics</span></span>](https://azure.microsoft.com/blog/azure-media-redactor/)

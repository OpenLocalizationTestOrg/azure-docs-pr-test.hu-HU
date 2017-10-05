---
title: "Az Azure Médiaelemzés használatával forgatókönyv lapok kivonás |} Microsoft Docs"
description: "Ez a témakör bemutatja a részletes útmutatást tartalmaz egy Azure Media Services Explorer (AMSE) és az Azure Media Redactor Vizualizálója (nyílt forráskódú eszköz) használatával teljes kivonási munkafolyamat futtatásához."
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
ms.openlocfilehash: c0c622237f8cdca65fb6933f14cc21e9eb9ac036
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a><span data-ttu-id="1d352-103">Az Azure Médiaelemzés használatával forgatókönyv lapok kivonása</span><span class="sxs-lookup"><span data-stu-id="1d352-103">Redact faces with Azure Media Analytics walkthrough</span></span>

## <a name="overview"></a><span data-ttu-id="1d352-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1d352-104">Overview</span></span>

<span data-ttu-id="1d352-105">**Az Azure Media Redactor** van egy [Azure Médiaelemzés használatával](media-services-analytics-overview.md) media processzor (MP), amely a felhőben méretezhető arcfelismerési kivonási nyújt.</span><span class="sxs-lookup"><span data-stu-id="1d352-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in the cloud.</span></span> <span data-ttu-id="1d352-106">Arcfelismerési kivonási lehetővé teszi, hogy a videó ahhoz, hogy a kijelölt személyeket felületei életlenítés módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="1d352-106">Face redaction enables you to modify your video in order to blur faces of selected individuals.</span></span> <span data-ttu-id="1d352-107">Érdemes lehet nyilvános biztonsági és hírek media helyzetekben használhatja a tapasztalt kivonási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d352-107">You may want to use the face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="1d352-108">Több lapokat tartalmazó felvételei, néhány perc múlva a kivonás a manuálisan órát is igénybe vehet, de ezzel a szolgáltatással a tapasztalt kivonási folyamat néhány egyszerű lépésben szükséges.</span><span class="sxs-lookup"><span data-stu-id="1d352-108">A few minutes of footage that contains multiple faces can take hours to redact manually, but with this service the face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="1d352-109">További információkért lásd: [ez](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span><span class="sxs-lookup"><span data-stu-id="1d352-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="1d352-110">Vonatkozó további információért **Azure Media Redactor**, tekintse meg a [Arcfelismerési kivonási áttekintése](media-services-face-redaction.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="1d352-110">For details about  **Azure Media Redactor**, see the [Face redaction overview](media-services-face-redaction.md) topic.</span></span>

<span data-ttu-id="1d352-111">Ez a témakör bemutatja a részletes útmutatást tartalmaz egy Azure Media Services Explorer (AMSE) és az Azure Media Redactor Vizualizálója (nyílt forráskódú eszköz) használatával teljes kivonási munkafolyamat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1d352-111">This topic shows step by step instructions on how to run a full redaction workflow using Azure Media Services Explorer (AMSE) and Azure Media Redactor Visualizer (open source tool).</span></span>

<span data-ttu-id="1d352-112">A **Azure Media Redactor** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="1d352-112">The **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="1d352-113">Érhető el az összes Azure-régiók, valamint Amerikai Egyesült államokbeli kormányzati és Kína adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="1d352-113">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="1d352-114">Ez az előnézet jelenleg díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="1d352-114">This preview is currently free of charge.</span></span> <span data-ttu-id="1d352-115">A jelenlegi kiadásban 10 perces korlátozva van a feldolgozott videó hossza.</span><span class="sxs-lookup"><span data-stu-id="1d352-115">In the current release, there is a 10 minute limit on processed video length.</span></span>

<span data-ttu-id="1d352-116">További információkért lásd: [ez](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.</span><span class="sxs-lookup"><span data-stu-id="1d352-116">For more information, see [this](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.</span></span>

## <a name="azure-media-services-explorer-workflow"></a><span data-ttu-id="1d352-117">Az Azure Media Services Explorer munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="1d352-117">Azure Media Services Explorer workflow</span></span>

<span data-ttu-id="1d352-118">A legegyszerűbb Ismerkedés a Redactor módja a nyílt forráskódú AMSE eszköz használata a githubon.</span><span class="sxs-lookup"><span data-stu-id="1d352-118">The easiest way to get started with Redactor is to use the open source AMSE tool on github.</span></span> <span data-ttu-id="1d352-119">Keresztül egyszerűsített munkafolyamat futtatása **kombinált** mód, ha nincs szüksége a jegyzet json és a tapasztalt jpg képek a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="1d352-119">You can run a simplified workflow via **combined** mode if you don’t need access to the annotation json or the face jpg images.</span></span>

### <a name="download-and-setup"></a><span data-ttu-id="1d352-120">A letöltés és telepítés</span><span class="sxs-lookup"><span data-stu-id="1d352-120">Download and setup</span></span>

1. <span data-ttu-id="1d352-121">Töltse le az AMSE eszköz [Itt](https://github.com/Azure/Azure-Media-Services-Explorer).</span><span class="sxs-lookup"><span data-stu-id="1d352-121">Download the AMSE tool from [here](https://github.com/Azure/Azure-Media-Services-Explorer).</span></span>
1. <span data-ttu-id="1d352-122">Jelentkezzen be a Media Services-fiók, a szolgáltatás-kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="1d352-122">Log in to your Media Services account using your service key.</span></span>

    <span data-ttu-id="1d352-123">A fiók neve és a legfontosabb információk beszerzéséhez látogasson el az [Azure-portálra](https://portal.azure.com/), és válassza ki AMS-fiókját.</span><span class="sxs-lookup"><span data-stu-id="1d352-123">To obtain the account name and key information, go to the [Azure portal](https://portal.azure.com/) and select your AMS account.</span></span> <span data-ttu-id="1d352-124">Válassza a beállítások > kulcsok.</span><span class="sxs-lookup"><span data-stu-id="1d352-124">Then, select Settings > Keys.</span></span> <span data-ttu-id="1d352-125">A Kulcsok kezelése ablakban megtalálja a fiók nevét, valamint az elsődleges és másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="1d352-125">The Manage keys windows shows the account name and the primary and secondary keys is displayed.</span></span> <span data-ttu-id="1d352-126">Másolja ki a fióknév és az elsődleges kulcs értékeit.</span><span class="sxs-lookup"><span data-stu-id="1d352-126">Copy values of the account name and the primary key.</span></span>

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a><span data-ttu-id="1d352-128">Először adja át – mód elemzése</span><span class="sxs-lookup"><span data-stu-id="1d352-128">First pass – analyze mode</span></span>

1. <span data-ttu-id="1d352-129">A médiafájl keresztül eszköz feltöltés –> feltöltési, sem a fogd és vidd segítségével.</span><span class="sxs-lookup"><span data-stu-id="1d352-129">Upload your media file through Asset –> Upload, or via drag and drop.</span></span> 
1. <span data-ttu-id="1d352-130">Kattintson a jobb gombbal, és dolgozza fel a médiafájl Médiaelemzés használatával Azure Media Redactor –> –> elemzési módot.</span><span class="sxs-lookup"><span data-stu-id="1d352-130">Right click and process your media file using Media Analytics –> Azure Media Redactor –> Analyze mode.</span></span> 


![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

<span data-ttu-id="1d352-133">A kimenet egy jegyzetek json-fájl, arc helyadatok, valamint minden észlelt felületen jpg tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1d352-133">The output will include an annotations json file with face location data, as well as a jpg of each detected face.</span></span> 

![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a><span data-ttu-id="1d352-135">Második fázis – mód kivonása</span><span class="sxs-lookup"><span data-stu-id="1d352-135">Second pass – redact mode</span></span>

1. <span data-ttu-id="1d352-136">Az első fázisban a töltse fel az eredeti video asset a kimeneti, és állítsa be elsődleges eszközként.</span><span class="sxs-lookup"><span data-stu-id="1d352-136">Upload your original video asset to the output from the first pass and set as a primary asset.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. <span data-ttu-id="1d352-138">(Választható) A kivonás kívánt azonosító Sortöréssel elválasztott listáját tartalmazó "Dance_idlist.txt" fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="1d352-138">(Optional) Upload a 'Dance_idlist.txt' file which includes a newline delimited list of the IDs you wish to redact.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. <span data-ttu-id="1d352-140">(Választható) Végezze el a módosításokat a annotations.json fájlba, például a határoló mező határait növelését.</span><span class="sxs-lookup"><span data-stu-id="1d352-140">(Optional) Make any edits to the annotations.json file such as increasing the bounding box boundaries.</span></span> 
4. <span data-ttu-id="1d352-141">A kimeneti adategységen az első fázisban a kattintson a jobb gombbal, válassza ki a Redactor, futtassa a **Redact** mód.</span><span class="sxs-lookup"><span data-stu-id="1d352-141">Right click the output asset from the first pass, select the Redactor, and run with the **Redact** mode.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. <span data-ttu-id="1d352-143">Töltse le, vagy a végső kivont kimeneti adategységen fájlmegosztás.</span><span class="sxs-lookup"><span data-stu-id="1d352-143">Download or share the final redacted output asset.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a><span data-ttu-id="1d352-145">Azure Media Redactor Vizualizálója nyílt forráskódú eszköz</span><span class="sxs-lookup"><span data-stu-id="1d352-145">Azure Media Redactor Visualizer open source tool</span></span>

<span data-ttu-id="1d352-146">Egy nyílt forráskódú [vizualizálója eszköz](https://github.com/Microsoft/azure-media-redactor-visualizer) célja, hogy segítségével a fejlesztők a jegyzetek formátumú elemzése, és a kimeneti használatával indítása.</span><span class="sxs-lookup"><span data-stu-id="1d352-146">An open source [visualizer tool](https://github.com/Microsoft/azure-media-redactor-visualizer) is designed to help developers just starting with the annotations format with parsing and using the output.</span></span>

<span data-ttu-id="1d352-147">Után a projekt futtatásához a tárházban klónozását, akkor töltse le a FFMPEG a [hivatalos webhely](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="1d352-147">After you clone the repo, in order to run the project, you will need to download FFMPEG from their [official site](https://ffmpeg.org/download.html).</span></span>

<span data-ttu-id="1d352-148">Ha egy fejlesztő a jegyzet JSON-adatok elemzése, nyissa meg Models.MetaData minta kód példákat.</span><span class="sxs-lookup"><span data-stu-id="1d352-148">If you are a developer trying to parse the JSON annotation data, look inside Models.MetaData for sample code examples.</span></span>

### <a name="set-up-the-tool"></a><span data-ttu-id="1d352-149">Az eszköz beállítása</span><span class="sxs-lookup"><span data-stu-id="1d352-149">Set up the tool</span></span>

1.  <span data-ttu-id="1d352-150">Töltse le, és a teljes megoldás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1d352-150">Download and build the entire solution.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  <span data-ttu-id="1d352-152">Töltse le a FFMPEG [Itt](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="1d352-152">Download FFMPEG from [here](https://ffmpeg.org/download.html).</span></span> <span data-ttu-id="1d352-153">Ez a projekt eredetileg fejlesztettek verzió be1d324 (2016-10-04) a statikus hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="1d352-153">This project was originally developed with version be1d324 (2016-10-04) with static linking.</span></span> 
3.  <span data-ttu-id="1d352-154">A kimeneti mappában, amelyben AzureMediaRedactor.exe ffmpeg.exe és ffprobe.exe másolja.</span><span class="sxs-lookup"><span data-stu-id="1d352-154">Copy ffmpeg.exe and ffprobe.exe to the same output folder as AzureMediaRedactor.exe.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. <span data-ttu-id="1d352-156">Futtassa a AzureMediaRedactor.exe.</span><span class="sxs-lookup"><span data-stu-id="1d352-156">Run AzureMediaRedactor.exe.</span></span> 

### <a name="use-the-tool"></a><span data-ttu-id="1d352-157">Az eszköz használatával</span><span class="sxs-lookup"><span data-stu-id="1d352-157">Use the tool</span></span>

1. <span data-ttu-id="1d352-158">A videó az Azure Media Services-fiókban Redactor felügyeleti csomag az elemzési módot a feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="1d352-158">Process your video in your Azure Media Services account with the Redactor MP on Analyze mode.</span></span> 
2. <span data-ttu-id="1d352-159">Töltse le az eredeti videó fájl- és a kivonási kimenetét, mert a feladat elemzése.</span><span class="sxs-lookup"><span data-stu-id="1d352-159">Download both the original video file and the output of the Redaction - Analyze job.</span></span> 
3. <span data-ttu-id="1d352-160">Futtassa a vizualizálója alkalmazást, és válassza ki a fenti fájlok.</span><span class="sxs-lookup"><span data-stu-id="1d352-160">Run the visualizer application and choose the files above.</span></span> 

    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. <span data-ttu-id="1d352-162">Tekintse meg a fájlt.</span><span class="sxs-lookup"><span data-stu-id="1d352-162">Preview your file.</span></span> <span data-ttu-id="1d352-163">Válassza ki, mely lapok lehetőséget a jobb oldali az oldalsávon keresztül szeretné.</span><span class="sxs-lookup"><span data-stu-id="1d352-163">Select which faces you'd like to blur via the sidebar on the right.</span></span> 
    
    ![Arcszerkesztés](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  <span data-ttu-id="1d352-165">Az alsó szövegmező frissíteni fogja a tapasztalt azonosítók.</span><span class="sxs-lookup"><span data-stu-id="1d352-165">The bottom text field will update with the face IDs.</span></span> <span data-ttu-id="1d352-166">Ezek az azonosítók "idlist.txt" nevű fájl létrehozása Sortöréssel elválasztott listáját.</span><span class="sxs-lookup"><span data-stu-id="1d352-166">Create a file called "idlist.txt" with these IDs as a newline delimited list.</span></span> 

    >[!NOTE]
    > <span data-ttu-id="1d352-167">A idlist.txt ANSI kell menteni.</span><span class="sxs-lookup"><span data-stu-id="1d352-167">The idlist.txt should be saved in ANSI.</span></span> <span data-ttu-id="1d352-168">ANSI mentéséhez használja a Jegyzettömböt.</span><span class="sxs-lookup"><span data-stu-id="1d352-168">You can use notepad to save in ANSI.</span></span>
    
6.  <span data-ttu-id="1d352-169">A feltöltés a kimeneti adategységen az 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="1d352-169">Upload this file to the output asset from step 1.</span></span> <span data-ttu-id="1d352-170">Az eredeti videó feltöltése, valamint az ehhez az eszközhöz, és állítsa be elsődleges eszközként.</span><span class="sxs-lookup"><span data-stu-id="1d352-170">Upload the original video to this asset as well and set as primary asset.</span></span> 
7.  <span data-ttu-id="1d352-171">Ez az eszköz "Redact" módban fusson kivonási feladat végső beolvasandó kivont videó.</span><span class="sxs-lookup"><span data-stu-id="1d352-171">Run Redaction job on this asset with "Redact" mode to get the final redacted video.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1d352-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d352-172">Next steps</span></span> 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1d352-173">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="1d352-173">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="1d352-174">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="1d352-174">Related links</span></span>
[<span data-ttu-id="1d352-175">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="1d352-175">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="1d352-176">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="1d352-176">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[<span data-ttu-id="1d352-177">Az Azure Médiaelemzés használatával Arcfelismerési kivonási bejelentése</span><span class="sxs-lookup"><span data-stu-id="1d352-177">Announcing Face Redaction for Azure Media Analytics</span></span>](https://azure.microsoft.com/blog/azure-media-redactor/)

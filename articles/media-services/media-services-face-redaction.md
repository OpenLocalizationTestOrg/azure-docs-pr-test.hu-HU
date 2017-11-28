---
title: "Az Azure Media Analytics lapok kivonása |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan kivonása az Azure media analytics lapokat."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5b6d8b8c-5f4d-4fef-b3d6-dc22c6b5a0f5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;
ms.openlocfilehash: 747f3ae1a7484515083c590942de3da22568cd39
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a><span data-ttu-id="6ca69-103">Az Azure Media Analytics lapok kivonása</span><span class="sxs-lookup"><span data-stu-id="6ca69-103">Redact faces with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="6ca69-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6ca69-104">Overview</span></span>
<span data-ttu-id="6ca69-105">**Az Azure Media Redactor** van egy [Azure Médiaelemzés használatával](media-services-analytics-overview.md) media processzor (MP), amely a felhőben méretezhető arcfelismerési kivonási nyújt.</span><span class="sxs-lookup"><span data-stu-id="6ca69-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in the cloud.</span></span> <span data-ttu-id="6ca69-106">Arcfelismerési kivonási lehetővé teszi, hogy a videó ahhoz, hogy a kijelölt személyeket felületei életlenítés módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6ca69-106">Face redaction enables you to modify your video in order to blur faces of selected individuals.</span></span> <span data-ttu-id="6ca69-107">Érdemes lehet nyilvános biztonsági és hírek media helyzetekben használhatja a tapasztalt kivonási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ca69-107">You may want to use the face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="6ca69-108">Több lapokat tartalmazó felvételei, néhány perc múlva a kivonás a manuálisan órát is igénybe vehet, de ezzel a szolgáltatással a tapasztalt kivonási folyamat néhány egyszerű lépésben szükséges.</span><span class="sxs-lookup"><span data-stu-id="6ca69-108">A few minutes of footage that contains multiple faces can take hours to redact manually, but with this service the face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="6ca69-109">További információkért lásd: [ez](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span><span class="sxs-lookup"><span data-stu-id="6ca69-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="6ca69-110">Ez a témakör kapcsolatos részleteket nyújt **Azure Media Redactor** és a .NET-keretrendszerhez készült Media Services SDK-val való használatát ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6ca69-110">This topic gives details about **Azure Media Redactor** and shows how to use it with Media Services SDK for .NET.</span></span>

<span data-ttu-id="6ca69-111">A **Azure Media Redactor** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="6ca69-111">The **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="6ca69-112">Érhető el az összes Azure-régiók, valamint Amerikai Egyesült államokbeli kormányzati és Kína adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="6ca69-112">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="6ca69-113">Ez az előnézet jelenleg díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="6ca69-113">This preview is currently free of charge.</span></span> 

## <a name="face-redaction-modes"></a><span data-ttu-id="6ca69-114">Arcfelismerési kivonási módok</span><span class="sxs-lookup"><span data-stu-id="6ca69-114">Face redaction modes</span></span>
<span data-ttu-id="6ca69-115">Arcfelismerést kivonási működik található videó lapok észlelésére és nyomon követni a tapasztalt objektum mindkét előre és hátra időben, ugyanabból is homályos, a más szögek is.</span><span class="sxs-lookup"><span data-stu-id="6ca69-115">Facial redaction works by detecting faces in every frame of video and tracking the face object both forwards and backwards in time, so that the same individual can be blurred from other angles as well.</span></span> <span data-ttu-id="6ca69-116">Automatizált kivonási folyamata bonyolult, és nem nem mindig által előállított 100 %-a kívánt kimeneti, ezért Media Analytics tartalmazza a több módon módosítani a végső kimenetet.</span><span class="sxs-lookup"><span data-stu-id="6ca69-116">The automated redaction process is very complex and does not always produce 100% of desired output, for this reason Media Analytics provides you with a couple of ways to modify the final output.</span></span>

<span data-ttu-id="6ca69-117">Mellett a teljesen automatikus üzemmódban van, amely lehetővé teszi, hogy a kijelölés/inaktiválása-selection talált lapok keresztül azonosítók listáját a két-fázis munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="6ca69-117">In addition to a fully automatic mode, there is a two-pass workflow which allows the selection/de-selection of found faces via a list of IDs.</span></span> <span data-ttu-id="6ca69-118">Ellenőrizze a keret módosításának a felügyeleti csomag egy tetszőleges is, használja a metaadatfájl JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="6ca69-118">Also, to make arbitrary per frame adjustments the MP uses a metadata file in JSON format.</span></span> <span data-ttu-id="6ca69-119">Ez a munkafolyamat oszlik **elemzés** és **Redact** módot.</span><span class="sxs-lookup"><span data-stu-id="6ca69-119">This workflow is split into **Analyze** and **Redact** modes.</span></span> <span data-ttu-id="6ca69-120">A két mód, amely mindkét feladat fut egy feladat; egyetlen menetben kombinálva Ebben a módban nevezik **kombinált**.</span><span class="sxs-lookup"><span data-stu-id="6ca69-120">You can combine the two modes in a single pass that runs both tasks in one job; this mode is called **Combined**.</span></span>

### <a name="combined-mode"></a><span data-ttu-id="6ca69-121">Kombinált mód</span><span class="sxs-lookup"><span data-stu-id="6ca69-121">Combined mode</span></span>
<span data-ttu-id="6ca69-122">Ez a művelet létrehoz egy kivont mp4 automatikusan szükséges bemeneti manuális nélkül.</span><span class="sxs-lookup"><span data-stu-id="6ca69-122">This will produce a redacted mp4 automatically without any manual input.</span></span>

| <span data-ttu-id="6ca69-123">Fázis</span><span class="sxs-lookup"><span data-stu-id="6ca69-123">Stage</span></span> | <span data-ttu-id="6ca69-124">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="6ca69-124">File Name</span></span> | <span data-ttu-id="6ca69-125">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6ca69-125">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6ca69-126">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-126">Input asset</span></span> |<span data-ttu-id="6ca69-127">foo.bar</span><span class="sxs-lookup"><span data-stu-id="6ca69-127">foo.bar</span></span> |<span data-ttu-id="6ca69-128">Videó WMV, MOV vagy MP4 formátumban</span><span class="sxs-lookup"><span data-stu-id="6ca69-128">Video in WMV, MOV, or MP4 format</span></span> |
| <span data-ttu-id="6ca69-129">Bemeneti config</span><span class="sxs-lookup"><span data-stu-id="6ca69-129">Input config</span></span> |<span data-ttu-id="6ca69-130">Konfigurációs feladat az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="6ca69-130">Job configuration preset</span></span> |<span data-ttu-id="6ca69-131">{"version": "1.0', a"beállítások": {"mode":"kombinált"}}</span><span class="sxs-lookup"><span data-stu-id="6ca69-131">{'version':'1.0', 'options': {'mode':'combined'}}</span></span> |
| <span data-ttu-id="6ca69-132">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-132">Output asset</span></span> |<span data-ttu-id="6ca69-133">foo_redacted.mp4</span><span class="sxs-lookup"><span data-stu-id="6ca69-133">foo_redacted.mp4</span></span> |<span data-ttu-id="6ca69-134">Az alkalmazott fellazítja videó</span><span class="sxs-lookup"><span data-stu-id="6ca69-134">Video with blurring applied</span></span> |

#### <a name="input-example"></a><span data-ttu-id="6ca69-135">Bemeneti. példa:</span><span class="sxs-lookup"><span data-stu-id="6ca69-135">Input example:</span></span>
[<span data-ttu-id="6ca69-136">Ez a videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="6ca69-136">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a><span data-ttu-id="6ca69-137">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6ca69-137">Output example:</span></span>
[<span data-ttu-id="6ca69-138">Ez a videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="6ca69-138">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a><span data-ttu-id="6ca69-139">Mód elemzése</span><span class="sxs-lookup"><span data-stu-id="6ca69-139">Analyze mode</span></span>
<span data-ttu-id="6ca69-140">A **elemzése** fázis a két-fázis munkafolyamat videó bemenetből fogad adatokat, és arcfelismerési helyek JSON fájlt hoz létre, és jpg lemezképek az egyes észlelt arcfelismerési.</span><span class="sxs-lookup"><span data-stu-id="6ca69-140">The **analyze** pass of the two-pass workflow takes a video input and produces a JSON file of face locations, and jpg images of each detected face.</span></span>

| <span data-ttu-id="6ca69-141">Fázis</span><span class="sxs-lookup"><span data-stu-id="6ca69-141">Stage</span></span> | <span data-ttu-id="6ca69-142">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="6ca69-142">File Name</span></span> | <span data-ttu-id="6ca69-143">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6ca69-143">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6ca69-144">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-144">Input asset</span></span> |<span data-ttu-id="6ca69-145">foo.bar</span><span class="sxs-lookup"><span data-stu-id="6ca69-145">foo.bar</span></span> |<span data-ttu-id="6ca69-146">Videó WMV, MPV vagy MP4 formátumban</span><span class="sxs-lookup"><span data-stu-id="6ca69-146">Video in WMV, MPV, or MP4 format</span></span> |
| <span data-ttu-id="6ca69-147">Bemeneti config</span><span class="sxs-lookup"><span data-stu-id="6ca69-147">Input config</span></span> |<span data-ttu-id="6ca69-148">Konfigurációs feladat az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="6ca69-148">Job configuration preset</span></span> |<span data-ttu-id="6ca69-149">{"version": "1.0', a"beállítások": {"mode":"elemezni"}}</span><span class="sxs-lookup"><span data-stu-id="6ca69-149">{'version':'1.0', 'options': {'mode':'analyze'}}</span></span> |
| <span data-ttu-id="6ca69-150">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-150">Output asset</span></span> |<span data-ttu-id="6ca69-151">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="6ca69-151">foo_annotations.json</span></span> |<span data-ttu-id="6ca69-152">A jegyzet adatok arcfelismerési helyek JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="6ca69-152">Annotation data of face locations in JSON format.</span></span> <span data-ttu-id="6ca69-153">Ez a felhasználó módosítása a mezőkbe határolókeret fellazítja szerkeszthető.</span><span class="sxs-lookup"><span data-stu-id="6ca69-153">This can be edited by the user to modify the blurring bounding boxes.</span></span> <span data-ttu-id="6ca69-154">Lásd az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="6ca69-154">See sample below.</span></span> |
| <span data-ttu-id="6ca69-155">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-155">Output asset</span></span> |<span data-ttu-id="6ca69-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span><span class="sxs-lookup"><span data-stu-id="6ca69-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span></span> |<span data-ttu-id="6ca69-157">Az egyes levágott jpg arc, ahol a számot jelzi a felületen címkeazonosító észlelt</span><span class="sxs-lookup"><span data-stu-id="6ca69-157">A cropped jpg of each detected face, where the number indicates the labelId of the face</span></span> |

#### <a name="output-example"></a><span data-ttu-id="6ca69-158">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6ca69-158">Output example:</span></span>

    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated

### <a name="redact-mode"></a><span data-ttu-id="6ca69-159">Mód kivonása</span><span class="sxs-lookup"><span data-stu-id="6ca69-159">Redact mode</span></span>
<span data-ttu-id="6ca69-160">A második fázis a munkafolyamat bemenetei, egyetlen eszköz kombinálni kell nagyobb számú vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6ca69-160">The second pass of the workflow takes a larger number of inputs that must be combined into a single asset.</span></span>

<span data-ttu-id="6ca69-161">Ez magában foglalja a azonosítók lehetőséget, az eredeti video-, és a jegyzetek JSON listáját.</span><span class="sxs-lookup"><span data-stu-id="6ca69-161">This includes a list of IDs to blur, the original video, and the annotations JSON.</span></span> <span data-ttu-id="6ca69-162">Ebben a módban a jegyzetek a segítségével a bemeneti videóhoz a fellazítja.</span><span class="sxs-lookup"><span data-stu-id="6ca69-162">This mode uses the annotations to apply blurring on the input video.</span></span>

<span data-ttu-id="6ca69-163">Az elemzési fázis kimenete nem tartalmazza az eredeti videó.</span><span class="sxs-lookup"><span data-stu-id="6ca69-163">The output from the Analyze pass does not include the original video.</span></span> <span data-ttu-id="6ca69-164">A videó kell a Redact mód tevékenység bemeneti objektumba feltölthetők és az elsődleges fájl választotta.</span><span class="sxs-lookup"><span data-stu-id="6ca69-164">The video needs to be uploaded into the input asset for the Redact mode task and selected as the primary file.</span></span>

| <span data-ttu-id="6ca69-165">Fázis</span><span class="sxs-lookup"><span data-stu-id="6ca69-165">Stage</span></span> | <span data-ttu-id="6ca69-166">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="6ca69-166">File Name</span></span> | <span data-ttu-id="6ca69-167">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6ca69-167">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6ca69-168">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-168">Input asset</span></span> |<span data-ttu-id="6ca69-169">foo.bar</span><span class="sxs-lookup"><span data-stu-id="6ca69-169">foo.bar</span></span> |<span data-ttu-id="6ca69-170">Videó WMV, MPV vagy MP4 formátumban.</span><span class="sxs-lookup"><span data-stu-id="6ca69-170">Video in WMV, MPV, or MP4 format.</span></span> <span data-ttu-id="6ca69-171">Ugyanaz, mint 1. lépés videó.</span><span class="sxs-lookup"><span data-stu-id="6ca69-171">Same video as in step 1.</span></span> |
| <span data-ttu-id="6ca69-172">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-172">Input asset</span></span> |<span data-ttu-id="6ca69-173">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="6ca69-173">foo_annotations.json</span></span> |<span data-ttu-id="6ca69-174">első lépése, opcionális módosításokkal jegyzetek metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="6ca69-174">annotations metadata file from phase one, with optional modifications.</span></span> |
| <span data-ttu-id="6ca69-175">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-175">Input asset</span></span> |<span data-ttu-id="6ca69-176">foo_IDList.txt (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="6ca69-176">foo_IDList.txt (Optional)</span></span> |<span data-ttu-id="6ca69-177">Választható új sor tagolt arcfelismerési kivonás azonosítók listája.</span><span class="sxs-lookup"><span data-stu-id="6ca69-177">Optional new line separated list of face IDs to redact.</span></span> <span data-ttu-id="6ca69-178">Ha üresen hagyja a mezőt, ez minden életleníti.</span><span class="sxs-lookup"><span data-stu-id="6ca69-178">If left blank, this blurs all faces.</span></span> |
| <span data-ttu-id="6ca69-179">Bemeneti config</span><span class="sxs-lookup"><span data-stu-id="6ca69-179">Input config</span></span> |<span data-ttu-id="6ca69-180">Konfigurációs feladat az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="6ca69-180">Job configuration preset</span></span> |<span data-ttu-id="6ca69-181">{"version": "1.0', a"beállítások": {"mode":"Kihagyás"}}</span><span class="sxs-lookup"><span data-stu-id="6ca69-181">{'version':'1.0', 'options': {'mode':'redact'}}</span></span> |
| <span data-ttu-id="6ca69-182">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="6ca69-182">Output asset</span></span> |<span data-ttu-id="6ca69-183">foo_redacted.mp4</span><span class="sxs-lookup"><span data-stu-id="6ca69-183">foo_redacted.mp4</span></span> |<span data-ttu-id="6ca69-184">Videó fellazítja alkalmazott jegyzetek alapján</span><span class="sxs-lookup"><span data-stu-id="6ca69-184">Video with blurring applied based on annotations</span></span> |

#### <a name="example-output"></a><span data-ttu-id="6ca69-185">Példa a kimenetre</span><span class="sxs-lookup"><span data-stu-id="6ca69-185">Example output</span></span>
<span data-ttu-id="6ca69-186">Ez a kiválasztott egy azonosítójú egy IDList kimenetét.</span><span class="sxs-lookup"><span data-stu-id="6ca69-186">This is the output from an IDList with one ID selected.</span></span>

[<span data-ttu-id="6ca69-187">Ez a videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="6ca69-187">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

<span data-ttu-id="6ca69-188">Példa foo_IDList.txt</span><span class="sxs-lookup"><span data-stu-id="6ca69-188">Example foo_IDList.txt</span></span>
 
     1
     2
     3

## <a name="blur-types"></a><span data-ttu-id="6ca69-189">Életlenítés típusok</span><span class="sxs-lookup"><span data-stu-id="6ca69-189">Blur types</span></span>

<span data-ttu-id="6ca69-190">A a **kombinált** vagy **Redact** mód közül választhat a bemeneti JSON-konfigurációs keresztül 5 különböző életlenítés módot: **alacsony**, **Med**, **magas**, **Debug**, és **fekete**.</span><span class="sxs-lookup"><span data-stu-id="6ca69-190">In the **Combined** or **Redact** mode, there are 5 different blur modes you can choose from via the JSON input configuration: **Low**, **Med**, **High**, **Debug**, and **Black**.</span></span> <span data-ttu-id="6ca69-191">Alapértelmezés szerint **Med** szolgál.</span><span class="sxs-lookup"><span data-stu-id="6ca69-191">By default **Med** is used.</span></span>

<span data-ttu-id="6ca69-192">A életlenítés típusú mintában találja.</span><span class="sxs-lookup"><span data-stu-id="6ca69-192">You can find samples of the blur types below.</span></span>

### <a name="example-json"></a><span data-ttu-id="6ca69-193">JSON-NÁ. példa:</span><span class="sxs-lookup"><span data-stu-id="6ca69-193">Example JSON:</span></span>

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a><span data-ttu-id="6ca69-194">Alacsony</span><span class="sxs-lookup"><span data-stu-id="6ca69-194">Low</span></span>

![Alacsony](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a><span data-ttu-id="6ca69-196">Med</span><span class="sxs-lookup"><span data-stu-id="6ca69-196">Med</span></span>

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a><span data-ttu-id="6ca69-198">Magas</span><span class="sxs-lookup"><span data-stu-id="6ca69-198">High</span></span>

![Magas](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a><span data-ttu-id="6ca69-200">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="6ca69-200">Debug</span></span>

![Hibakeresés](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a><span data-ttu-id="6ca69-202">Fekete</span><span class="sxs-lookup"><span data-stu-id="6ca69-202">Black</span></span>

![Fekete](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-the-output-json-file"></a><span data-ttu-id="6ca69-204">A kimeneti JSON-fájl elemeinek</span><span class="sxs-lookup"><span data-stu-id="6ca69-204">Elements of the output JSON file</span></span>

<span data-ttu-id="6ca69-205">A kivonási MP magas pontosság arcfelismerési hely észleli és nyomon követése, mely legfeljebb 64 emberi lapok észlelheti a képkockák.</span><span class="sxs-lookup"><span data-stu-id="6ca69-205">The Redaction MP provides high precision face location detection and tracking that can detect up to 64 human faces in a video frame.</span></span> <span data-ttu-id="6ca69-206">Elülső lapok nyújtanak a legjobb eredmények elérése érdekében, közben ügyféloldali lapok és a kis (legfeljebb 24 x 24 képpont) lapok kihívást.</span><span class="sxs-lookup"><span data-stu-id="6ca69-206">Frontal faces provide the best results, while side faces and small faces (less than or equal to 24x24 pixels) are challenging.</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a><span data-ttu-id="6ca69-207">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="6ca69-207">.NET sample code</span></span>

<span data-ttu-id="6ca69-208">A következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="6ca69-208">The following program shows how to:</span></span>

1. <span data-ttu-id="6ca69-209">Hozzon létre egy eszközt, és adathordozó-fájl feltöltése az objektumba.</span><span class="sxs-lookup"><span data-stu-id="6ca69-209">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="6ca69-210">Hozzon létre egy feladatot a következő json-készletet tartalmazó konfigurációs fájl alapján arcfelismerési kivonási feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="6ca69-210">Create a job with a face redaction task based on a configuration file that contains the following json preset.</span></span> 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
3. <span data-ttu-id="6ca69-211">A kimeneti JSON-fájlok letöltésére.</span><span class="sxs-lookup"><span data-stu-id="6ca69-211">Download the output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="6ca69-212">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6ca69-212">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="6ca69-213">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6ca69-213">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="6ca69-214">Példa</span><span class="sxs-lookup"><span data-stu-id="6ca69-214">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace FaceRedaction
    {
        class Program
        {
        // Read values from the App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Run the FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference to Azure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

            // Use the following event handler to check job progress.  
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

            // Launch the job.
            job.Submit();

            // Check job execution and wait for job to finish.
            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
            ErrorDetail error = job.Tasks.First().ErrorDetails.First();
            Console.WriteLine(string.Format("Error: {0}. {1}",
                            error.Code,
                            error.Message));
            return null;
            }

            return job.OutputMediaAssets[0];
        }

        static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
        {
            IAsset asset = _context.Assets.Create(assetName, options);

            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);

            return asset;
        }

        static void DownloadAsset(IAsset asset, string outputDirectory)
        {
            foreach (IAssetFile file in asset.AssetFiles)
            {
            file.Download(Path.Combine(outputDirectory, file.Name));
            }
        }

        static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

            if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                   mediaProcessorName));

            return processor;
        }

        static private void StateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);

            switch (e.CurrentState)
            {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job is finished.");
                Console.WriteLine();
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
            case JobState.Error:
                // Cast sender as a job.
                IJob job = (IJob)sender;
                // Display or log error details as needed.
                // LogJobStop(job.Id);
                break;
            default:
                break;
            }
        }
        }
    }

## <a name="next-steps"></a><span data-ttu-id="6ca69-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ca69-215">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="6ca69-216">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="6ca69-216">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="6ca69-217">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="6ca69-217">Related links</span></span>
[<span data-ttu-id="6ca69-218">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="6ca69-218">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="6ca69-219">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="6ca69-219">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


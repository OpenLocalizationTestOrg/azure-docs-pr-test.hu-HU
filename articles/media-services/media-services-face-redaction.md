---
title: az Azure Media Analytics aaaRedact lapok |} Microsoft Docs
description: "Ez a témakör bemutatja, hogyan tooredact előtt álló az Azure médiaelemzés használatával."
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
ms.openlocfilehash: 1f5688a8c6374151c526a9c702b904d8c3e46164
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a><span data-ttu-id="dccea-103">Az Azure Media Analytics lapok kivonása</span><span class="sxs-lookup"><span data-stu-id="dccea-103">Redact faces with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="dccea-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="dccea-104">Overview</span></span>
<span data-ttu-id="dccea-105">**Az Azure Media Redactor** van egy [Azure Médiaelemzés használatával](media-services-analytics-overview.md) media processzor (MP), amely méretezhető arcfelismerési kivonási hello felhőben nyújt.</span><span class="sxs-lookup"><span data-stu-id="dccea-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="dccea-106">Arcfelismerési kivonási lehetővé teszi, hogy Ön toomodify a rendelés tooblur felületei kijelölt személyek a videó.</span><span class="sxs-lookup"><span data-stu-id="dccea-106">Face redaction enables you toomodify your video in order tooblur faces of selected individuals.</span></span> <span data-ttu-id="dccea-107">Érdemes lehet toouse hello arcfelismerési kivonási szolgáltatás nyilvános biztonsági és hírek media forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="dccea-107">You may want toouse hello face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="dccea-108">Több lapokat tartalmazó felvételei, néhány percet is igénybe vehet óra tooredact manuálisan, de a szolgáltatás hello arcfelismerési a kivonási folyamat néhány egyszerű lépésben szükséges.</span><span class="sxs-lookup"><span data-stu-id="dccea-108">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service hello face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="dccea-109">További információkért lásd: [ez](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span><span class="sxs-lookup"><span data-stu-id="dccea-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="dccea-110">Ez a témakör kapcsolatos részleteket nyújt **Azure Media Redactor** és bemutatja, hogyan toouse a Media Services SDK for .NET.</span><span class="sxs-lookup"><span data-stu-id="dccea-110">This topic gives details about **Azure Media Redactor** and shows how toouse it with Media Services SDK for .NET.</span></span>

<span data-ttu-id="dccea-111">Hello **Azure Media Redactor** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="dccea-111">hello **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="dccea-112">Érhető el az összes Azure-régiók, valamint Amerikai Egyesült államokbeli kormányzati és Kína adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="dccea-112">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="dccea-113">Ez az előnézet jelenleg díjmentesen.</span><span class="sxs-lookup"><span data-stu-id="dccea-113">This preview is currently free of charge.</span></span> 

## <a name="face-redaction-modes"></a><span data-ttu-id="dccea-114">Arcfelismerési kivonási módok</span><span class="sxs-lookup"><span data-stu-id="dccea-114">Face redaction modes</span></span>
<span data-ttu-id="dccea-115">Arcfelismerést kivonási works található videó lapok észlelésére, és nyomon követni a hello arcfelismerési objektumot is előre és hátra időben, hogy hello azonos egyéni is lehet homályos más szögek, valamint a.</span><span class="sxs-lookup"><span data-stu-id="dccea-115">Facial redaction works by detecting faces in every frame of video and tracking hello face object both forwards and backwards in time, so that hello same individual can be blurred from other angles as well.</span></span> <span data-ttu-id="dccea-116">hello automatizált kivonási folyamat nagyon összetett és nem mindig készít 100 %-a kívánt kimeneti, emiatt Media Analytics tartalmazza a több módon toomodify hello végső kimenetet.</span><span class="sxs-lookup"><span data-stu-id="dccea-116">hello automated redaction process is very complex and does not always produce 100% of desired output, for this reason Media Analytics provides you with a couple of ways toomodify hello final output.</span></span>

<span data-ttu-id="dccea-117">Továbbá tooa teljesen automatikus üzemmódban van; nincs lehetővé teszi, hogy hello kijelölés/inaktiválása-selection talált lapok keresztül azonosítók listáját, amelyek a két-fázis munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="dccea-117">In addition tooa fully automatic mode, there is a two-pass workflow which allows hello selection/de-selection of found faces via a list of IDs.</span></span> <span data-ttu-id="dccea-118">Toomake keret módosításának hello felügyeleti csomag egy tetszőleges ugyancsak a metaadatfájl JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="dccea-118">Also, toomake arbitrary per frame adjustments hello MP uses a metadata file in JSON format.</span></span> <span data-ttu-id="dccea-119">Ez a munkafolyamat oszlik **elemzés** és **Redact** módot.</span><span class="sxs-lookup"><span data-stu-id="dccea-119">This workflow is split into **Analyze** and **Redact** modes.</span></span> <span data-ttu-id="dccea-120">Kombinálhatja a két mód hello egyetlen menetben, amely mindkét feladat fut egy feladat; Ebben a módban nevezik **kombinált**.</span><span class="sxs-lookup"><span data-stu-id="dccea-120">You can combine hello two modes in a single pass that runs both tasks in one job; this mode is called **Combined**.</span></span>

### <a name="combined-mode"></a><span data-ttu-id="dccea-121">Kombinált mód</span><span class="sxs-lookup"><span data-stu-id="dccea-121">Combined mode</span></span>
<span data-ttu-id="dccea-122">Ez a művelet létrehoz egy kivont mp4 automatikusan szükséges bemeneti manuális nélkül.</span><span class="sxs-lookup"><span data-stu-id="dccea-122">This will produce a redacted mp4 automatically without any manual input.</span></span>

| <span data-ttu-id="dccea-123">Fázis</span><span class="sxs-lookup"><span data-stu-id="dccea-123">Stage</span></span> | <span data-ttu-id="dccea-124">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="dccea-124">File Name</span></span> | <span data-ttu-id="dccea-125">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="dccea-125">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dccea-126">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-126">Input asset</span></span> |<span data-ttu-id="dccea-127">foo.bar</span><span class="sxs-lookup"><span data-stu-id="dccea-127">foo.bar</span></span> |<span data-ttu-id="dccea-128">Videó WMV, MOV vagy MP4 formátumban</span><span class="sxs-lookup"><span data-stu-id="dccea-128">Video in WMV, MOV, or MP4 format</span></span> |
| <span data-ttu-id="dccea-129">Bemeneti config</span><span class="sxs-lookup"><span data-stu-id="dccea-129">Input config</span></span> |<span data-ttu-id="dccea-130">Konfigurációs feladat az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="dccea-130">Job configuration preset</span></span> |<span data-ttu-id="dccea-131">{"version": "1.0', a"beállítások": {"mode":"kombinált"}}</span><span class="sxs-lookup"><span data-stu-id="dccea-131">{'version':'1.0', 'options': {'mode':'combined'}}</span></span> |
| <span data-ttu-id="dccea-132">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-132">Output asset</span></span> |<span data-ttu-id="dccea-133">foo_redacted.mp4</span><span class="sxs-lookup"><span data-stu-id="dccea-133">foo_redacted.mp4</span></span> |<span data-ttu-id="dccea-134">Az alkalmazott fellazítja videó</span><span class="sxs-lookup"><span data-stu-id="dccea-134">Video with blurring applied</span></span> |

#### <a name="input-example"></a><span data-ttu-id="dccea-135">Bemeneti. példa:</span><span class="sxs-lookup"><span data-stu-id="dccea-135">Input example:</span></span>
[<span data-ttu-id="dccea-136">Ez a videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="dccea-136">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a><span data-ttu-id="dccea-137">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="dccea-137">Output example:</span></span>
[<span data-ttu-id="dccea-138">Ez a videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="dccea-138">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a><span data-ttu-id="dccea-139">Mód elemzése</span><span class="sxs-lookup"><span data-stu-id="dccea-139">Analyze mode</span></span>
<span data-ttu-id="dccea-140">Hello **elemzése** hello két-fázis munkafolyamat menete videó bemenetből fogad adatokat, és arcfelismerési helyek JSON fájlt hoz létre, és jpg lemezképek az egyes észlelt arcfelismerési.</span><span class="sxs-lookup"><span data-stu-id="dccea-140">hello **analyze** pass of hello two-pass workflow takes a video input and produces a JSON file of face locations, and jpg images of each detected face.</span></span>

| <span data-ttu-id="dccea-141">Fázis</span><span class="sxs-lookup"><span data-stu-id="dccea-141">Stage</span></span> | <span data-ttu-id="dccea-142">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="dccea-142">File Name</span></span> | <span data-ttu-id="dccea-143">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="dccea-143">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dccea-144">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-144">Input asset</span></span> |<span data-ttu-id="dccea-145">foo.bar</span><span class="sxs-lookup"><span data-stu-id="dccea-145">foo.bar</span></span> |<span data-ttu-id="dccea-146">Videó WMV, MPV vagy MP4 formátumban</span><span class="sxs-lookup"><span data-stu-id="dccea-146">Video in WMV, MPV, or MP4 format</span></span> |
| <span data-ttu-id="dccea-147">Bemeneti config</span><span class="sxs-lookup"><span data-stu-id="dccea-147">Input config</span></span> |<span data-ttu-id="dccea-148">Konfigurációs feladat az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="dccea-148">Job configuration preset</span></span> |<span data-ttu-id="dccea-149">{"version": "1.0', a"beállítások": {"mode":"elemezni"}}</span><span class="sxs-lookup"><span data-stu-id="dccea-149">{'version':'1.0', 'options': {'mode':'analyze'}}</span></span> |
| <span data-ttu-id="dccea-150">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-150">Output asset</span></span> |<span data-ttu-id="dccea-151">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="dccea-151">foo_annotations.json</span></span> |<span data-ttu-id="dccea-152">A jegyzet adatok arcfelismerési helyek JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="dccea-152">Annotation data of face locations in JSON format.</span></span> <span data-ttu-id="dccea-153">Ez módosítható használatával hello felhasználói toomodify hello fellazítja határolókeret jelölőnégyzetéből.</span><span class="sxs-lookup"><span data-stu-id="dccea-153">This can be edited by hello user toomodify hello blurring bounding boxes.</span></span> <span data-ttu-id="dccea-154">Lásd az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="dccea-154">See sample below.</span></span> |
| <span data-ttu-id="dccea-155">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-155">Output asset</span></span> |<span data-ttu-id="dccea-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span><span class="sxs-lookup"><span data-stu-id="dccea-156">foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]</span></span> |<span data-ttu-id="dccea-157">Az egyes levágott jpg arc, ahol hello számot jelzi hello címkeazonosító hello felületen észlelt</span><span class="sxs-lookup"><span data-stu-id="dccea-157">A cropped jpg of each detected face, where hello number indicates hello labelId of hello face</span></span> |

#### <a name="output-example"></a><span data-ttu-id="dccea-158">Példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="dccea-158">Output example:</span></span>

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

### <a name="redact-mode"></a><span data-ttu-id="dccea-159">Mód kivonása</span><span class="sxs-lookup"><span data-stu-id="dccea-159">Redact mode</span></span>
<span data-ttu-id="dccea-160">második fázisában hello hello munkafolyamat egyetlen eszköz kombinálni kell felhasználandó nagyobb számú vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="dccea-160">hello second pass of hello workflow takes a larger number of inputs that must be combined into a single asset.</span></span>

<span data-ttu-id="dccea-161">Ez magában foglalja a azonosítók tooblur, hello eredeti videó és hello jegyzetek JSON listáját.</span><span class="sxs-lookup"><span data-stu-id="dccea-161">This includes a list of IDs tooblur, hello original video, and hello annotations JSON.</span></span> <span data-ttu-id="dccea-162">Ebben a módban a hello jegyzetek tooapply fellazítja hello bemeneti videóhoz használja.</span><span class="sxs-lookup"><span data-stu-id="dccea-162">This mode uses hello annotations tooapply blurring on hello input video.</span></span>

<span data-ttu-id="dccea-163">hello hello elemzési fázis kimenete nem tartalmaz hello eredeti videó.</span><span class="sxs-lookup"><span data-stu-id="dccea-163">hello output from hello Analyze pass does not include hello original video.</span></span> <span data-ttu-id="dccea-164">videó hello kell toobe hello Redact mód feladat hello bemeneti objektumba feltöltött és hello elsődleges fájl választotta.</span><span class="sxs-lookup"><span data-stu-id="dccea-164">hello video needs toobe uploaded into hello input asset for hello Redact mode task and selected as hello primary file.</span></span>

| <span data-ttu-id="dccea-165">Fázis</span><span class="sxs-lookup"><span data-stu-id="dccea-165">Stage</span></span> | <span data-ttu-id="dccea-166">Fájlnév</span><span class="sxs-lookup"><span data-stu-id="dccea-166">File Name</span></span> | <span data-ttu-id="dccea-167">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="dccea-167">Notes</span></span> |
| --- | --- | --- |
| <span data-ttu-id="dccea-168">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-168">Input asset</span></span> |<span data-ttu-id="dccea-169">foo.bar</span><span class="sxs-lookup"><span data-stu-id="dccea-169">foo.bar</span></span> |<span data-ttu-id="dccea-170">Videó WMV, MPV vagy MP4 formátumban.</span><span class="sxs-lookup"><span data-stu-id="dccea-170">Video in WMV, MPV, or MP4 format.</span></span> <span data-ttu-id="dccea-171">Ugyanaz, mint 1. lépés videó.</span><span class="sxs-lookup"><span data-stu-id="dccea-171">Same video as in step 1.</span></span> |
| <span data-ttu-id="dccea-172">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-172">Input asset</span></span> |<span data-ttu-id="dccea-173">foo_annotations.JSON</span><span class="sxs-lookup"><span data-stu-id="dccea-173">foo_annotations.json</span></span> |<span data-ttu-id="dccea-174">első lépése, opcionális módosításokkal jegyzetek metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="dccea-174">annotations metadata file from phase one, with optional modifications.</span></span> |
| <span data-ttu-id="dccea-175">Bemeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-175">Input asset</span></span> |<span data-ttu-id="dccea-176">foo_IDList.txt (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="dccea-176">foo_IDList.txt (Optional)</span></span> |<span data-ttu-id="dccea-177">Választható Sortöréssel elválasztott arcfelismerési azonosítók tooredact.</span><span class="sxs-lookup"><span data-stu-id="dccea-177">Optional new line separated list of face IDs tooredact.</span></span> <span data-ttu-id="dccea-178">Ha üresen hagyja a mezőt, ez minden életleníti.</span><span class="sxs-lookup"><span data-stu-id="dccea-178">If left blank, this blurs all faces.</span></span> |
| <span data-ttu-id="dccea-179">Bemeneti config</span><span class="sxs-lookup"><span data-stu-id="dccea-179">Input config</span></span> |<span data-ttu-id="dccea-180">Konfigurációs feladat az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="dccea-180">Job configuration preset</span></span> |<span data-ttu-id="dccea-181">{"version": "1.0', a"beállítások": {"mode":"Kihagyás"}}</span><span class="sxs-lookup"><span data-stu-id="dccea-181">{'version':'1.0', 'options': {'mode':'redact'}}</span></span> |
| <span data-ttu-id="dccea-182">Kimeneti eszköz</span><span class="sxs-lookup"><span data-stu-id="dccea-182">Output asset</span></span> |<span data-ttu-id="dccea-183">foo_redacted.mp4</span><span class="sxs-lookup"><span data-stu-id="dccea-183">foo_redacted.mp4</span></span> |<span data-ttu-id="dccea-184">Videó fellazítja alkalmazott jegyzetek alapján</span><span class="sxs-lookup"><span data-stu-id="dccea-184">Video with blurring applied based on annotations</span></span> |

#### <a name="example-output"></a><span data-ttu-id="dccea-185">Példa a kimenetre</span><span class="sxs-lookup"><span data-stu-id="dccea-185">Example output</span></span>
<span data-ttu-id="dccea-186">Ez a kiválasztott egy azonosítójú egy IDList hello kimenete.</span><span class="sxs-lookup"><span data-stu-id="dccea-186">This is hello output from an IDList with one ID selected.</span></span>

[<span data-ttu-id="dccea-187">Ez a videó megtekintése</span><span class="sxs-lookup"><span data-stu-id="dccea-187">view this video</span></span>](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

<span data-ttu-id="dccea-188">Példa foo_IDList.txt</span><span class="sxs-lookup"><span data-stu-id="dccea-188">Example foo_IDList.txt</span></span>
 
     1
     2
     3

## <a name="blur-types"></a><span data-ttu-id="dccea-189">Életlenítés típusok</span><span class="sxs-lookup"><span data-stu-id="dccea-189">Blur types</span></span>

<span data-ttu-id="dccea-190">A hello **kombinált** vagy **Redact** módban keresztül hello JSON bemeneti konfigurációs választhat 5 különböző életlenítés módot: **alacsony**, **Med**, **Magas**, **Debug**, és **fekete**.</span><span class="sxs-lookup"><span data-stu-id="dccea-190">In hello **Combined** or **Redact** mode, there are 5 different blur modes you can choose from via hello JSON input configuration: **Low**, **Med**, **High**, **Debug**, and **Black**.</span></span> <span data-ttu-id="dccea-191">Alapértelmezés szerint **Med** szolgál.</span><span class="sxs-lookup"><span data-stu-id="dccea-191">By default **Med** is used.</span></span>

<span data-ttu-id="dccea-192">Hello mintái az alábbi típusok életlenítés találja.</span><span class="sxs-lookup"><span data-stu-id="dccea-192">You can find samples of hello blur types below.</span></span>

### <a name="example-json"></a><span data-ttu-id="dccea-193">JSON-NÁ. példa:</span><span class="sxs-lookup"><span data-stu-id="dccea-193">Example JSON:</span></span>

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

#### <a name="low"></a><span data-ttu-id="dccea-194">Alacsony</span><span class="sxs-lookup"><span data-stu-id="dccea-194">Low</span></span>

![Alacsony](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a><span data-ttu-id="dccea-196">Med</span><span class="sxs-lookup"><span data-stu-id="dccea-196">Med</span></span>

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a><span data-ttu-id="dccea-198">Magas</span><span class="sxs-lookup"><span data-stu-id="dccea-198">High</span></span>

![Magas](./media/media-services-face-redaction/blur3.png)

#### <a name="debug"></a><span data-ttu-id="dccea-200">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="dccea-200">Debug</span></span>

![Hibakeresés](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a><span data-ttu-id="dccea-202">Fekete</span><span class="sxs-lookup"><span data-stu-id="dccea-202">Black</span></span>

![Fekete](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-hello-output-json-file"></a><span data-ttu-id="dccea-204">Hello kimeneti JSON-fájl elemeinek</span><span class="sxs-lookup"><span data-stu-id="dccea-204">Elements of hello output JSON file</span></span>

<span data-ttu-id="dccea-205">hello kivonási felügyeleti csomag biztosít magas pontosság arcfelismerési hely felderítését és a nyomkövetési, amely észlelni tudja a too64 emberi lapok videó keret mentése.</span><span class="sxs-lookup"><span data-stu-id="dccea-205">hello Redaction MP provides high precision face location detection and tracking that can detect up too64 human faces in a video frame.</span></span> <span data-ttu-id="dccea-206">Elülső lapok hello legjobb eredmények elérése érdekében ügyféloldali lapokat és kis lapok közben adja meg (kevesebb mint vagy egyenlő too24x24 képpont) kihívást is.</span><span class="sxs-lookup"><span data-stu-id="dccea-206">Frontal faces provide hello best results, while side faces and small faces (less than or equal too24x24 pixels) are challenging.</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a><span data-ttu-id="dccea-207">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="dccea-207">.NET sample code</span></span>

<span data-ttu-id="dccea-208">hello következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="dccea-208">hello following program shows how to:</span></span>

1. <span data-ttu-id="dccea-209">Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.</span><span class="sxs-lookup"><span data-stu-id="dccea-209">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="dccea-210">Hozzon létre egy feladatot a json-készlet a következő hello tartalmazó konfigurációs fájl alapján arcfelismerési kivonási feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="dccea-210">Create a job with a face redaction task based on a configuration file that contains hello following json preset.</span></span> 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
3. <span data-ttu-id="dccea-211">Hello kimeneti JSON-fájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="dccea-211">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="dccea-212">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dccea-212">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="dccea-213">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="dccea-213">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="dccea-214">Példa</span><span class="sxs-lookup"><span data-stu-id="dccea-214">Example</span></span>

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
        // Read values from hello App.config file.
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

            // Run hello FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download hello job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload hello input media file toostorage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference tooAzure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from hello specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with hello encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify hello input asset.
            task.InputAssets.Add(asset);

            // Add an output asset toocontain hello results of hello job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

            // Use hello following event handler toocheck job progress.  
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

            // Launch hello job.
            job.Submit();

            // Check job execution and wait for job toofinish.
            Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

            progressJobTask.Wait();

            // If job state is Error, hello event handling
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

## <a name="next-steps"></a><span data-ttu-id="dccea-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dccea-215">Next steps</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="dccea-216">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="dccea-216">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="dccea-217">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="dccea-217">Related links</span></span>
[<span data-ttu-id="dccea-218">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="dccea-218">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="dccea-219">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="dccea-219">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


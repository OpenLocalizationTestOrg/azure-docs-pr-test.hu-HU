---
title: "aaaAvanced Media Encoder prémium munkafolyamat oktatóprogramok"
description: "Ez a dokumentum tartalmaz, hogyan tooperform speciális Media Encoder prémium munkafolyamat rendelkező tevékenységek megjelenítése forgatókönyvek és hogyan is toocreate bonyolult munkafolyamatok a munkafolyamat-tervezővel."
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 641bd1be3bd795b5e138fa7ffdf299ff6710904e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a><span data-ttu-id="2f7bc-103">Speciális Media Encoder prémium munkafolyamat oktatóprogramok</span><span class="sxs-lookup"><span data-stu-id="2f7bc-103">Advanced Media Encoder Premium Workflow tutorials</span></span>
## <a name="overview"></a><span data-ttu-id="2f7bc-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2f7bc-104">Overview</span></span>
<span data-ttu-id="2f7bc-105">Ez a dokumentum tartalmaz, amelyek megjelenítik forgatókönyvek hogyan toocustomize munkafolyamatok **munkafolyamat-Tervező**.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-105">This document contains walkthroughs that show how toocustomize workflows with  **Workflow Designer**.</span></span> <span data-ttu-id="2f7bc-106">Található hello tényleges munkafolyamatfájlokat [Itt](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-106">You can find hello actual workflow files [here](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).</span></span>  

## <a name="toc"></a><span data-ttu-id="2f7bc-107">TARTALOMJEGYZÉK</span><span class="sxs-lookup"><span data-stu-id="2f7bc-107">TOC</span></span>
<span data-ttu-id="2f7bc-108">hello a következő témakörök ismertetnek:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-108">hello following topics are covered:</span></span>

* [<span data-ttu-id="2f7bc-109">Az egyszeres sávszélességű MP4 kódolási MXF</span><span class="sxs-lookup"><span data-stu-id="2f7bc-109">Encoding MXF into a single bitrate MP4</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [<span data-ttu-id="2f7bc-110">Új munkafolyamat indítása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-110">Starting a new workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [<span data-ttu-id="2f7bc-111">Media fájl bemeneti hello használata</span><span class="sxs-lookup"><span data-stu-id="2f7bc-111">Using hello Media File Input</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [<span data-ttu-id="2f7bc-112">Tanulmányozza az adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="2f7bc-112">Inspecting media streams</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [<span data-ttu-id="2f7bc-113">Az egy videókódoló hozzáadása. MP4-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-113">Adding a video encoder for .MP4 file generation</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [<span data-ttu-id="2f7bc-114">Kódolási hello hangadatfolyam</span><span class="sxs-lookup"><span data-stu-id="2f7bc-114">Encoding hello audio stream</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [<span data-ttu-id="2f7bc-115">Audió és videó-adatfolyamokat multiplexáló egy MP4-tárolóba</span><span class="sxs-lookup"><span data-stu-id="2f7bc-115">Multiplexing Audio and Video streams into an MP4 container</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [<span data-ttu-id="2f7bc-116">Hello MP4-fájl írása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-116">Writing hello MP4 file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [<span data-ttu-id="2f7bc-117">Egy Media Services eszköz hello kimeneti fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-117">Creating a Media Services Asset from hello output file</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [<span data-ttu-id="2f7bc-118">Teszt hello befejeződött helyben munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="2f7bc-118">Test hello finished workflow locally</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [<span data-ttu-id="2f7bc-119">Kódolás MXF multibitrate MP4 - a dinamikus becsomagolás engedélyezve</span><span class="sxs-lookup"><span data-stu-id="2f7bc-119">Encoding MXF into multibitrate MP4s - dynamic packaging enabled</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [<span data-ttu-id="2f7bc-120">Egy vagy több további MP4 kimenetek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-120">Adding one or more additional MP4 outputs</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [<span data-ttu-id="2f7bc-121">Kimeneti nevek konfigurálása hello fájl</span><span class="sxs-lookup"><span data-stu-id="2f7bc-121">Configuring hello file output names</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [<span data-ttu-id="2f7bc-122">Egy külön lejátszása hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-122">Adding a separate Audio Track</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [<span data-ttu-id="2f7bc-123">Hello hozzáadása. ISM SMIL fájl</span><span class="sxs-lookup"><span data-stu-id="2f7bc-123">Adding hello .ISM SMIL File</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [<span data-ttu-id="2f7bc-124">Kódolási MXF multibitrate MP4 - továbbfejlesztett tervezetének be</span><span class="sxs-lookup"><span data-stu-id="2f7bc-124">Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [<span data-ttu-id="2f7bc-125">Munkafolyamat áttekintése tooenhance</span><span class="sxs-lookup"><span data-stu-id="2f7bc-125">Workflow overview tooenhance</span></span>](#workflow-overview-to-enhance)
  * [<span data-ttu-id="2f7bc-126">A fájlok elnevezési konvenciók</span><span class="sxs-lookup"><span data-stu-id="2f7bc-126">File Naming Conventions</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [<span data-ttu-id="2f7bc-127">Hello munkafolyamat legfelső szintű alakzatot közzétételi összetevő tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="2f7bc-127">Publishing component properties onto hello workflow root</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [<span data-ttu-id="2f7bc-128">Kimeneti fájl nevét közzétett tulajdonságértékek támaszkodnak hozta létre.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-128">Have generated output file names rely on published property values</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [<span data-ttu-id="2f7bc-129">Miniatűrök toomultibitrate MP4 kimenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-129">Adding thumbnails toomultibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [<span data-ttu-id="2f7bc-130">Munkafolyamat áttekintése tooadd miniatűrök</span><span class="sxs-lookup"><span data-stu-id="2f7bc-130">Workflow overview tooadd thumbnails to</span></span>](#workflow-overview-to-add-thumbnails-to)
  * [<span data-ttu-id="2f7bc-131">JPG kódolás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-131">Adding JPG Encoding</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [<span data-ttu-id="2f7bc-132">A szín terület átalakítás foglalkozó</span><span class="sxs-lookup"><span data-stu-id="2f7bc-132">Dealing with Color Space conversion</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [<span data-ttu-id="2f7bc-133">Írás hello miniatűrök</span><span class="sxs-lookup"><span data-stu-id="2f7bc-133">Writing hello thumbnails</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [<span data-ttu-id="2f7bc-134">A munkafolyamat hibáinak észleléséhez</span><span class="sxs-lookup"><span data-stu-id="2f7bc-134">Detecting errors in a workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [<span data-ttu-id="2f7bc-135">Befejezett munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="2f7bc-135">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [<span data-ttu-id="2f7bc-136">Tisztítás időalapú multibitrate MP4 kimenet</span><span class="sxs-lookup"><span data-stu-id="2f7bc-136">Time-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [<span data-ttu-id="2f7bc-137">Munkafolyamat áttekintése toostart levágási történő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-137">Workflow overview toostart adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [<span data-ttu-id="2f7bc-138">Az adatfolyam vágó hello használata</span><span class="sxs-lookup"><span data-stu-id="2f7bc-138">Using hello Stream Trimmer</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [<span data-ttu-id="2f7bc-139">Befejezett munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="2f7bc-139">Finished Workflow</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [<span data-ttu-id="2f7bc-140">Introducing hello összetevő parancsprogrammal létrehozva</span><span class="sxs-lookup"><span data-stu-id="2f7bc-140">Introducing hello Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [<span data-ttu-id="2f7bc-141">A munkafolyamaton belül Scripting: hello world</span><span class="sxs-lookup"><span data-stu-id="2f7bc-141">Scripting within a workflow: hello world</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [<span data-ttu-id="2f7bc-142">Keret-alapú levágási multibitrate MP4 kimenet</span><span class="sxs-lookup"><span data-stu-id="2f7bc-142">Frame-based trimming of multibitrate MP4 output</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [<span data-ttu-id="2f7bc-143">Tervezetének áttekintése toostart levágási történő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-143">Blueprint overview toostart adding trimming to</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [<span data-ttu-id="2f7bc-144">Hello klip lista XML használatával</span><span class="sxs-lookup"><span data-stu-id="2f7bc-144">Using hello Clip List XML</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [<span data-ttu-id="2f7bc-145">Egy parancsprogram összetevő hello klip listájának módosítása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-145">Modifying hello clip list from a Scripted Component</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [<span data-ttu-id="2f7bc-146">Egy ClippingEnabled kényelmi tulajdonság hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-146">Adding a ClippingEnabled convenience property</span></span>](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <span data-ttu-id="2f7bc-147"><a id="MXF_to_MP4"></a>Az egyszeres sávszélességű MP4 kódolási MXF</span><span class="sxs-lookup"><span data-stu-id="2f7bc-147"><a id="MXF_to_MP4"></a>Encoding MXF into a single bitrate MP4</span></span>
<span data-ttu-id="2f7bc-148">A forgatókönyv létrehozunk egy egyféle sávszélességű. MP4-fájlokat a AAC-HE kódolású hangja egy. MXF bemeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-148">In this walkthrough we'll create a single bitrate .MP4 file with AAC-HE encoded audio from an .MXF input file.</span></span>

### <span data-ttu-id="2f7bc-149"><a id="MXF_to_MP4_start_new"></a>Új munkafolyamat indítása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-149"><a id="MXF_to_MP4_start_new"></a>Starting a new workflow</span></span>
<span data-ttu-id="2f7bc-150">Nyissa meg a munkafolyamat-tervezőben, és válassza ki a "Fájl"-"új munkaterület"-"átkódolására tervezetének"</span><span class="sxs-lookup"><span data-stu-id="2f7bc-150">Open Workflow Designer and select "File"-"New Workspace"-"Transcode Blueprint"</span></span>

<span data-ttu-id="2f7bc-151">Új munkafolyamat hello 3 elemek jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-151">hello new workflow will show 3 elements:</span></span>

* <span data-ttu-id="2f7bc-152">Elsődleges forrásfájl</span><span class="sxs-lookup"><span data-stu-id="2f7bc-152">Primary Source File</span></span>
* <span data-ttu-id="2f7bc-153">XML klip listázása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-153">Clip List XML</span></span>
* <span data-ttu-id="2f7bc-154">Kimeneti fájl vagy eszköz</span><span class="sxs-lookup"><span data-stu-id="2f7bc-154">Output File/Asset</span></span>  

![Új kódolási munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

<span data-ttu-id="2f7bc-156">*Új kódolási munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-156">*New Encoding Workflow*</span></span>

### <span data-ttu-id="2f7bc-157"><a id="MXF_to_MP4_with_file_input"></a>Media fájl bemeneti hello használata</span><span class="sxs-lookup"><span data-stu-id="2f7bc-157"><a id="MXF_to_MP4_with_file_input"></a>Using hello Media File Input</span></span>
<span data-ttu-id="2f7bc-158">A sorrend tooaccept a bemeneti médiafájl egy kezdődik-e Media fájl bemeneti összetevő hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-158">In order tooaccept our input media file, one starts with adding a Media File Input component.</span></span> <span data-ttu-id="2f7bc-159">tooadd összetevő toohello munkafolyamat keressen hello tárház keresési mezőbe, majd szükséges hello bejegyzés húzza hello Tervező ablak.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-159">tooadd a component toohello workflow, look for it in hello Repository search box and drag hello desired entry onto hello designer pane.</span></span> <span data-ttu-id="2f7bc-160">Ezt a lépést a hello Media fájl bemeneti, és csatlakoztassa hello elsődleges forrásfájl összetevő toohello fájlnév bemeneti PIN-kód Media fájl bemeneti hello.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-160">Do this for hello Media File Input and connect hello Primary Source File component toohello Filename input pin from hello Media File Input.</span></span>

![Csatlakoztatott médiafájl bemeneti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

<span data-ttu-id="2f7bc-162">*Csatlakoztatott médiafájl bemeneti*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-162">*Connected Media File Input*</span></span>

<span data-ttu-id="2f7bc-163">Mielőtt tehetünk ennek sok más, először kell tooindicate toohello munkafolyamat-Tervező milyen mintafájl toouse toodesign tapasztalatairól a munkafolyamatot.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-163">Before we can do much else, we'll first need tooindicate toohello workflow designer what sample file we'd like toouse toodesign our workflow with.</span></span> <span data-ttu-id="2f7bc-164">toodo tehát hello Tervező ablak háttér kattintson, és keresse meg hello elsődleges forrásfájl tulajdonság a hello tulajdonság jobb oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-164">toodo so, click hello designer pane background and look for hello Primary Source File property on hello right-hand property pane.</span></span> <span data-ttu-id="2f7bc-165">Kattintson a hello ikonja, és válassza hello szükséges fájl tootest hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-165">Click hello folder icon and select hello desired file tootest hello workflow with.</span></span> <span data-ttu-id="2f7bc-166">Amint ez történik, hello Media fájl bemeneti összetevő hello fájl vizsgálata, és a PIN-kódok tooreflect hello kimenetfájlba ellenőrizni azt tölteni.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-166">As soon as this is done, hello Media File Input component will inspect hello file and populate its output pins tooreflect hello file it inspected.</span></span>

![Ki van töltve médiafájl bemeneti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

<span data-ttu-id="2f7bc-168">*Ki van töltve médiafájl bemeneti*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-168">*Populated Media File Input*</span></span>

<span data-ttu-id="2f7bc-169">Azt határozza meg, mi adjon meg szeretnénk az toowork rendelkező, amíg nem arról, hogy még ahol kódolású hello kimeneti kell lépjen.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-169">While this specifies with what input we'd like toowork with, it doesn't tell yet where hello encoded output should go to.</span></span> <span data-ttu-id="2f7bc-170">Elsődleges forrásfájl be lett állítva, hasonló toohow hello hello kimeneti mappa változóját megadó tulajdonság, akkor alatt konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-170">Similar toohow hello Primary Source File was configured, now configure hello Output Folder Variable property, just below it.</span></span>

![Konfigurált bemeneti és kimeneti tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

<span data-ttu-id="2f7bc-172">*Konfigurált bemeneti és kimeneti tulajdonságai*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-172">*Configured Input and Output properties*</span></span>

### <span data-ttu-id="2f7bc-173"><a id="MXF_to_MP4_streams"></a>Tanulmányozza az adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="2f7bc-173"><a id="MXF_to_MP4_streams"></a>Inspecting media streams</span></span>
<span data-ttu-id="2f7bc-174">Gyakran kívánt hello adatfolyam megjelenésének hasonlóan tooknow áthaladó hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-174">Often it's desired tooknow how hello stream looks like that flows through hello workflow.</span></span> <span data-ttu-id="2f7bc-175">hello munkafolyamat bármely pontján adatfolyam tooinspect kattintson egy kimeneti vagy bemeneti PIN-kódjának hello az összetevőket.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-175">tooinspect a stream at any point in hello workflow, just click an output or input pin on any of hello components.</span></span> <span data-ttu-id="2f7bc-176">Ebben az esetben próbálkozzon hello tömörítetlen videó kimeneti PIN-kód az adathordozó fájl bemeneti kattint.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-176">In this case, try clicking on hello Uncompressed Video output pin from our Media File Input.</span></span> <span data-ttu-id="2f7bc-177">A párbeszédpanel ekkor megnyílik, amely lehetővé teszi a tooinspect hello kimenő videó.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-177">A dialog will open up that allows tooinspect hello outbound video.</span></span>

![Ellenőrző hello tömörítetlen videó kimeneti PIN-kód](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

<span data-ttu-id="2f7bc-179">*Ellenőrző hello tömörítetlen videó kimeneti PIN-kód*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-179">*Inspecting hello Uncompressed Video output pin*</span></span>

<span data-ttu-id="2f7bc-180">Ebben az esetben közli a gép például, hogy azt még foglalkozó egy 1920 x 1080 ráfordítás 24 képkockák másodpercenkénti 4:2:2 mintavételi szinte 2 perces videót.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-180">In our case, it tells us for example that we're dealing with a 1920x1080 input at 24 frames per second in 4:2:2 sampling for a video of almost 2 minutes.</span></span>

### <span data-ttu-id="2f7bc-181"><a id="MXF_to_MP4_file_generation"></a>Az egy videókódoló hozzáadása. MP4-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-181"><a id="MXF_to_MP4_file_generation"></a>Adding a video encoder for .MP4 file generation</span></span>
<span data-ttu-id="2f7bc-182">Vegye figyelembe, hogy most már, egy tömörítetlen videó és a több tömörítetlen hang kimeneti PIN-kód az adathordozó fájl bemeneti használhatók.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-182">Note that now, an Uncompressed Video and multiple Uncompressed Audio output pins are available for use on our Media File Input.</span></span> <span data-ttu-id="2f7bc-183">A sorrend tooencode hello bejövő videó, igazolnia kell egy kódolási összetevő - ebben az esetben előállítása érdekében. MP4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-183">In order tooencode hello inbound video, we need an encoding component - in this case for generating .MP4 files.</span></span>

<span data-ttu-id="2f7bc-184">tooencode hello video-adatfolyamot tooH.264, vegye fel a hello AVC Videókódoló összetevő toohello Tervező felületére.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-184">tooencode hello video stream tooH.264, add hello AVC Video Encoder component toohello designer surface.</span></span> <span data-ttu-id="2f7bc-185">Ez az összetevő egy Kibontás video-adatfolyamot bemenetként veszi, és kézbesíti az AVC tömörített video-adatfolyamot a kimeneti PIN-kód a.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-185">This component takes an uncompress video stream as input and delivers an AVC compressed video stream on its output pin.</span></span>

![Frissíthető AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

<span data-ttu-id="2f7bc-187">*Frissíthető AVC kódoló*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-187">*Unconnected AVC Encoder*</span></span>

<span data-ttu-id="2f7bc-188">A tulajdonság határozza meg, hogyan hello kódolás pontosan történik.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-188">Its properties determine how hello encoding exactly happens.</span></span> <span data-ttu-id="2f7bc-189">Most rá egy pillantást, hello néhány fontosabb beállítások:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-189">Let's have a look at some of hello more important settings:</span></span>

* <span data-ttu-id="2f7bc-190">Kimeneti szélességének és magasságának kimeneti: ezek határozzák meg a kódolt hello videó hello feloldását.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-190">Output width and Output height: these determine hello resolution of hello encoded video.</span></span> <span data-ttu-id="2f7bc-191">Abban az esetben, ha pedig ugorjunk az 640 x 360</span><span class="sxs-lookup"><span data-stu-id="2f7bc-191">In our case let's go with 640x360</span></span>
* <span data-ttu-id="2f7bc-192">Képkockasebessége: set toopassthrough azt fogja csak hello forrás képkockasebessége elfogadják, esetén lehetséges toooverride azonban ebben.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-192">Frame Rate: when set toopassthrough it will just adopt hello source frame rate, it's possible toooverride this though.</span></span> <span data-ttu-id="2f7bc-193">Vegye figyelembe, hogy ilyen képkockasebességhez átalakítás nem mozgásérzékelő – a kompenzációt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-193">Note that such framerate conversion is not motion-compensated.</span></span>
* <span data-ttu-id="2f7bc-194">Profil és szint: ezek határozzák meg, hello AVC-profil és szintjét.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-194">Profile and Level: these determine hello AVC profile and level.</span></span> <span data-ttu-id="2f7bc-195">További információ a feladatnaplókban tooconveniently hello különböző szintű és profilok, kattintson a hello kérdőjel ikon hello AVC videó kódoló összetevő, és hello súgólap megjeleníti az egyes hello szintek további információkra.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-195">tooconveniently get more information about hello different levels and profiles, click hello question mark icon on hello AVC Video Encoder component and hello help page will show more detail about each of hello levels.</span></span> <span data-ttu-id="2f7bc-196">A minta ugorjunk fő profillal 3.2 (hello alapértelmezett) szinten.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-196">For our sample, let's go with Main Profile at level 3.2 (hello default).</span></span>
* <span data-ttu-id="2f7bc-197">Értékelje a mód és átviteli sebesség (KB/s): a mi esetünkben azt egy állandó átviteli sebesség (CBR), 1200-as kbps kimeneti választhat</span><span class="sxs-lookup"><span data-stu-id="2f7bc-197">Rate Control Mode and Bitrate (kbps): in our scenario we opt for a constant bitrate (CBR) output at 1200 kbps</span></span>
* <span data-ttu-id="2f7bc-198">: Videó ez formátuma kapcsolatos hello VUI (videó használhatóság adatokat), amely lekérdezi hello H.264 adatfolyamba való írása (ügyféloldali információt, amelyik alkalmas lehet a dekóder tooenhance hello megjelenítési, de nem feltétlenül toocorrectly dekódolása):</span><span class="sxs-lookup"><span data-stu-id="2f7bc-198">Video Format: this is about hello VUI (Video Usability Information) that gets written into hello H.264 stream (side information that might be used by a decoder tooenhance hello display but not essential toocorrectly decode):</span></span>
* <span data-ttu-id="2f7bc-199">NTSC (Egyesült Államok vagy japán, 30 fps használatával a jellemző)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-199">NTSC (typical for US or Japan, using 30 fps)</span></span>
* <span data-ttu-id="2f7bc-200">PAL (Európa, 25 fps használatával a jellemző)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-200">PAL (typical for Europe, using 25 fps)</span></span>
* <span data-ttu-id="2f7bc-201">GOP méretezési módját: konfigurálását végezzük el GOP rögzített méretű a lezárt GOPs 2 másodperc kulcs időközt a célokra.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-201">GOP Size Mode: we'll configure Fixed GOP Size for our purposes with a Key Interval of 2 seconds with Closed GOPs.</span></span> <span data-ttu-id="2f7bc-202">Ez biztosítja a kompatibilitást a hello Azure-Media Services dinamikus becsomagolást biztosít.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-202">This ensures compatibility with hello dynamic packaging Azure Media Services provides.</span></span>

<span data-ttu-id="2f7bc-203">toofeed az AVC kódoló hello tömörítetlen videó kimeneti PIN-kód csatlakoztatja a hello Media fájl bemeneti összetevő toohello tömörítetlen videó bemeneti PIN-kódot hello AVC kódoló.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-203">toofeed our AVC encoder, connect hello Uncompressed Video output pin from hello Media File Input component toohello Uncompressed Video input pin from hello AVC encoder.</span></span>

![Csatlakoztatott AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

<span data-ttu-id="2f7bc-205">*Csatlakoztatott AVC fő kódoló*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-205">*Connected AVC Main encoder*</span></span>

### <span data-ttu-id="2f7bc-206"><a id="MXF_to_MP4_audio"></a>Kódolási hello hangadatfolyam</span><span class="sxs-lookup"><span data-stu-id="2f7bc-206"><a id="MXF_to_MP4_audio"></a>Encoding hello audio stream</span></span>
<span data-ttu-id="2f7bc-207">Ezen a ponton azt videó van kódolva, de hello eredeti tömörítetlen hangadatfolyam továbbra is hozzá kell tömörített toobe.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-207">At this point, we have encoded video but hello original uncompressed audio stream still needs toobe compressed.</span></span> <span data-ttu-id="2f7bc-208">Ez azt fogja látogassa meg AAC kódolással hello AAC kódoló (Dolby) összetevő.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-208">For this we'll go with AAC encoding by hello AAC Encoder (Dolby) component.</span></span> <span data-ttu-id="2f7bc-209">Toohello munkafolyamat adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-209">Add it toohello workflow.</span></span>

![Frissíthető AVC kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

<span data-ttu-id="2f7bc-211">*Frissíthető AAC kódoló*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-211">*Unconnected AAC encoder*</span></span>

<span data-ttu-id="2f7bc-212">Most nincs inkompatibilitás: nincs csak egyetlen tömörítetlen hang bemeneti PIN-kódot a hello AAC kódoló, amíg több mint valószínű hello Media fájl bemeneti kell két különböző tömörítetlen hangadatfolyam érhető el: egy a bal oldali hang csatorna és az egyik az hello hello jogosultság.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-212">Now there's an incompatibility: there's only a single uncompressed audio input pin from hello AAC Encoder while more than likely hello Media File Input will have two different uncompressed audio stream available: one for hello left audio channel and one for hello right.</span></span> <span data-ttu-id="2f7bc-213">(Ha között legyen hang foglalkozó, ez 6 csatornák.) Ezért nem lehetséges toodirectly hello Media fájl bemeneti forrás hello hang kapcsolódnak hello AAC hang kódoló.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-213">(If you're dealing with surround sound, that's 6 channels.) So it's not possible toodirectly connect hello audio from hello Media File Input source into hello AAC audio encoder.</span></span> <span data-ttu-id="2f7bc-214">hello AAC összetevő vár egy úgynevezett "kihagyásos" hangadatfolyam:, amelyek mindegyikét egy adatfolyam hello balra és hello jobb csatornák időosztásos egymással.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-214">hello AAC component expects a so-called "interleaved" audio stream: a single stream that has both hello left and hello right channels interleaved with each other.</span></span> <span data-ttu-id="2f7bc-215">Után tudjuk a forrás-adathordozó fájlból, mely zeneszámok hello forrás milyen pozíciója a azt hozhat létre a hello ilyen kihagyásos hangadatfolyam megfelelően rendelt hangalapú pozíciók bal és jobb.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-215">Once we know from our source media file which audio tracks are on what position in hello source, we can generate such interleaved audio stream with hello correctly assigned speaker positions for left and right.</span></span>

<span data-ttu-id="2f7bc-216">Először egy olyan kihagyásos adatfolyam a szükséges hello forrás hang csatornák toogenerated érdemes.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-216">First one will want toogenerated an interleaved stream from hello required source audio channels.</span></span> <span data-ttu-id="2f7bc-217">hello hang adatfolyam Interleaver összetevő kezelnek Ez az USA.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-217">hello Audio Stream Interleaver component will handle this for us.</span></span> <span data-ttu-id="2f7bc-218">Toohello munkafolyamat hozzáadása, és csatlakoztassa hello hang kimenetek hello Media fájl bemeneti bele.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-218">Add it toohello workflow and connect hello audio outputs from hello Media File Input into it.</span></span>

![Csatlakoztatott hangadatfolyam Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

<span data-ttu-id="2f7bc-220">*Csatlakoztatott hangadatfolyam Interleaver*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-220">*Connected Audio Stream Interleaver*</span></span>

<span data-ttu-id="2f7bc-221">Most, hogy egy kihagyásos hangadatfolyam, azt még nem adott meg ahol tooassign hello balra vagy jobbra hangalapú pozíciót szeretnénk.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-221">Now that we have an interleaved audio stream, we still didn't specify where tooassign hello left or right speaker positions to.</span></span> <span data-ttu-id="2f7bc-222">Ennek rendelés toospecify, azt hello hangalapú pozíció Assigner használhatják fel.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-222">In order toospecify this, we can leverage hello Speaker Position Assigner.</span></span>

![Egy hangalapú pozíció Assigner hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

<span data-ttu-id="2f7bc-224">*Egy hangalapú pozíció Assigner hozzáadása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-224">*Adding a Speaker Position Assigner*</span></span>

<span data-ttu-id="2f7bc-225">Hello hangalapú pozíció Assigner használatra sztereó bemeneti adatfolyam keresztül egy kódoló beállított szűrő az "Egyéni" konfigurálásához, valamint hello csatorna beállított "(L, R) 2.0" nevezik.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-225">Configure hello Speaker Position Assigner for use with a stereo input stream through an Encoder Preset Filter of "Custom" and hello Channel Preset called "2.0 (L,R)".</span></span> <span data-ttu-id="2f7bc-226">(Ez rendeli hello hangalapú bal oldali pozíciója toochannel 1 és hello jobb hangalapú pozíció toochannel 2.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-226">(This will assign hello left speaker position toochannel 1 and hello right speaker position toochannel 2.)</span></span>

<span data-ttu-id="2f7bc-227">Csatlakozás hello hangalapú pozíció Assigner toohello bemeneti hello AAC kódoló hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-227">Connect hello output of hello Speaker Position Assigner toohello input of hello AAC Encoder.</span></span> <span data-ttu-id="2f7bc-228">Ezt követően adja a hello AAC kódoló toowork egy "2.0 (L, R)" csatorna-készletet, így az tudni fogja, hogy a bemeneti sztereó hang toodeal.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-228">Then, tell hello AAC Encoder toowork with a "2.0 (L,R)" Channel Preset, so it knows toodeal with stereo audio as input.</span></span>

### <span data-ttu-id="2f7bc-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Audió és videó-adatfolyamokat multiplexáló egy MP4-tárolóba</span><span class="sxs-lookup"><span data-stu-id="2f7bc-229"><a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexing Audio and Video streams into an MP4 container</span></span>
<span data-ttu-id="2f7bc-230">Az AVC megadott kódolt video-adatfolyamot és a AAC kódolású hangadatfolyam, azt is rögzítheti, mindkettő egy. MP4-tároló.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-230">Given our AVC encoded video stream and our AAC encoded audio stream, we can capture both into an .MP4 container.</span></span> <span data-ttu-id="2f7bc-231">hello különböző adatfolyamokba keverési be egyetlen egy folyamathoz "multiplexáló" (vagy a "muxing").</span><span class="sxs-lookup"><span data-stu-id="2f7bc-231">hello process of mixing different streams into a single one is called "multiplexing" (or "muxing").</span></span> <span data-ttu-id="2f7bc-232">Ebben az esetben azt még kihagyásos hello hang- és hello video-adatfolyamot összefüggő egyetlen. MP4-csomag.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-232">In this case we're interleaving hello audio and hello video streams in a single coherent .MP4 package.</span></span> <span data-ttu-id="2f7bc-233">hello összetevő koordinálására ezt egy. MP4-tárolónak a neve hello ISO MPEG-4 Multiplexer.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-233">hello component that coordinates this for an .MP4 container is called hello ISO MPEG-4 Multiplexer.</span></span> <span data-ttu-id="2f7bc-234">Adja hozzá egy toohello Tervező felületére, és csatlakoztassa a hello AVC videó kódoló és a hello AAC kódoló tooits bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-234">Add one toohello designer surface and connect both hello AVC Video Encoder and hello AAC Encoder tooits inputs.</span></span>

![Csatlakoztatott MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

<span data-ttu-id="2f7bc-236">*Csatlakoztatott MPEG4 Multiplexer*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-236">*Connected MPEG4 Multiplexer*</span></span>

### <span data-ttu-id="2f7bc-237"><a id="MXF_to_MP4_writing_mp4"></a>Hello MP4-fájl írása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-237"><a id="MXF_to_MP4_writing_mp4"></a>Writing hello MP4 file</span></span>
<span data-ttu-id="2f7bc-238">Kimeneti fájl írásakor hello kimeneti fájl összetevő szolgál.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-238">When writing an output file, hello File Output component is used.</span></span> <span data-ttu-id="2f7bc-239">A toohello kimenetét ISO MPEG-4 Multiplexer hello azt is elérheti, így az a kimeneti lekérdezi toodisk.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-239">We can connect this toohello output of hello ISO MPEG-4 Multiplexer so that its output gets written toodisk.</span></span> <span data-ttu-id="2f7bc-240">toodo, csatlakozás hello (MPEG-4) tároló kimeneti PIN-kód toohello írási bemeneti PIN-kódja hello kimeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-240">toodo this, connect hello Container (MPEG-4) output pin toohello Write input pin of hello File Output.</span></span>

![Kimeneti fájl csatlakoztatva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

<span data-ttu-id="2f7bc-242">*Kimeneti fájl csatlakoztatva*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-242">*Connected File Output*</span></span>

<span data-ttu-id="2f7bc-243">hello fájlnév használandó hello fájl tulajdonság határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-243">hello filename that will be used is determined by hello File property.</span></span> <span data-ttu-id="2f7bc-244">Tulajdonság értéke szoftveresen kötött tooa lehetnek, nagy valószínűséggel tooset érdemes azt a kifejezést helyette.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-244">While that property can be hardcoded tooa given value, most likely one will want tooset it through an expression instead.</span></span>

<span data-ttu-id="2f7bc-245">toohave hello munkafolyamat automatikusan meghatározni a hello kimeneti fájlt a name tulajdonság kifejezésből, kattintson a hello buton következő toohello fájl neve (következő toohello ikonja).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-245">toohave hello workflow automatically determine hello output File name property from an expression, click hello buton next toohello File name (next toohello folder icon).</span></span> <span data-ttu-id="2f7bc-246">Hello a legördülő menüből, majd válassza a "Kifejezése".</span><span class="sxs-lookup"><span data-stu-id="2f7bc-246">From hello drop down menu then select "Expression".</span></span> <span data-ttu-id="2f7bc-247">Megjelenik a hello Kifejezésszerkesztő.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-247">This will bring up hello expression editor.</span></span> <span data-ttu-id="2f7bc-248">Először törölje a hello szerkesztő hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-248">Clear hello contents of hello editor first.</span></span>

![Üres kifejezés-szerkesztő](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

<span data-ttu-id="2f7bc-250">*Üres kifejezés-szerkesztő*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-250">*Empty Expression Editor*</span></span>

<span data-ttu-id="2f7bc-251">hello kifejezés szerkesztő segítségével tooenter szöveges értéket, és egy vagy több változót.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-251">hello expression editor allows tooenter any literal value, mixed with one or more variables.</span></span> <span data-ttu-id="2f7bc-252">Változók dollárjelet kezdődik.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-252">Variables start with a dollar sign.</span></span> <span data-ttu-id="2f7bc-253">Találati hello $ kulcs, mert a hello szerkesztő változók közül választhat a legördülő listában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-253">As you hit hello $ key, hello editor will show a dropdown box with a choice of available variables.</span></span> <span data-ttu-id="2f7bc-254">A mi esetünkben hello kimeneti könyvtár változó és a hello alap bemeneti fájlnév változó fogjuk használni:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-254">In our case we'll use a combination of hello output directory variable and hello base input file name variable:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Kitöltött kimenő kifejezés-szerkesztő](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

<span data-ttu-id="2f7bc-256">*Kitöltött kimenő kifejezés-szerkesztő*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-256">*Filled out Expression Editor*</span></span>

> [!NOTE]
> <span data-ttu-id="2f7bc-257">Ahhoz, toosee az Azure-ban a kódolási feladat kimeneti fájl megtekintéséhez hello kifejezés szerkesztőben értéket kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-257">In order toosee see an output file of your encoding job in Azure, you must provide a value in hello expression editor.</span></span>
>
>

<span data-ttu-id="2f7bc-258">Ha meggyőződött róla hello kifejezés által elérte-e az OK gombra, hello tulajdonság ablakban ezen a ponton a időben történő toowhat érték hello fájl tulajdonság oldja fel a rendszer előzetes megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-258">When you confirm hello expression by hitting ok, hello property window will preview toowhat value hello File property resolves at this point in time.</span></span>

![Fájl kifejezés kimeneti dir oldja fel.](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

<span data-ttu-id="2f7bc-260">*Fájl kifejezés kimeneti dir oldja fel.*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-260">*File Expression resolves output dir*</span></span>

### <span data-ttu-id="2f7bc-261"><a id="MXF_to_MP4_asset_from_output"></a>Egy Media Services eszköz hello kimeneti fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-261"><a id="MXF_to_MP4_asset_from_output"></a>Creating a Media Services Asset from hello output file</span></span>
<span data-ttu-id="2f7bc-262">Amíg azt írt kimeneti MP4-fájlokat, továbbra is szükséges tooindicate, hogy a fájl tartozik toohello kimeneti eszköz mely media services hoz létre a munkafolyamat végrehajtása miatt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-262">While we have written an MP4 output file, we still need tooindicate that this file belongs toohello output asset which media services will generate as a result of executing this workflow.</span></span> <span data-ttu-id="2f7bc-263">toothis célból hello kimeneti fájl/eszköz csomópontot a vásznon a hello munkafolyamat szolgál.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-263">toothis end, hello Output File/Asset node on hello workflow canvas is used.</span></span> <span data-ttu-id="2f7bc-264">Ez a csomópont minden bejövő fájlok megkönnyítő hello eredményül kapott Azure Media Services eszközt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-264">All incoming files into this node will make part of hello resulting Azure Media Services asset.</span></span>

<span data-ttu-id="2f7bc-265">Csatlakozás hello kimeneti fájl összetevő toohello kimeneti fájl/eszköz összetevő toofinish hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-265">Connect hello File Output component toohello Output File/Asset component toofinish hello workflow.</span></span>

![Befejezett munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

<span data-ttu-id="2f7bc-267">*Befejezett munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-267">*Finished Workflow*</span></span>

### <span data-ttu-id="2f7bc-268"><a id="MXF_to_MP4_test"></a>Teszt hello befejeződött helyben munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="2f7bc-268"><a id="MXF_to_MP4_test"></a>Test hello finished workflow locally</span></span>
<span data-ttu-id="2f7bc-269">helyileg, a tootest hello munkafolyamat találati hello eszköztár hello felső hello lejátszás gombra.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-269">tootest hello workflow locally, hit hello play button in hello toolbar at hello top.</span></span> <span data-ttu-id="2f7bc-270">Befejezésekor hello munkafolyamat végrehajtása vizsgálhatja hello kimeneti konfigurált hello kimeneti mappában jön létre.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-270">When hello workflow finished executing, inspect hello output generated in hello configured output folder.</span></span> <span data-ttu-id="2f7bc-271">Láthatja, hogy hello hello MXF bemeneti forrás fájlból kódolt MP4 kimeneti fájlok befejeződött.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-271">You'll see hello finished MP4 output file that was encoded from hello MXF input source file.</span></span>

## <span data-ttu-id="2f7bc-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Kódolás MXF MP4 - multibitrate a dinamikus becsomagolás engedélyezve</span><span class="sxs-lookup"><span data-stu-id="2f7bc-272"><a id="MXF_to_MP4_with_dyn_packaging"></a>Encoding MXF into MP4 - multibitrate dynamic packaging enabled</span></span>
<span data-ttu-id="2f7bc-273">A forgatókönyv nem fogja létrehozni a több sávszélességű MP4-fájlokat kódolású AAC hang egyetlen. MXF bemeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-273">In this walkthrough we'll create a set of multiple bitrate MP4 files with AAC encoded audio from a single .MXF input file.</span></span>

<span data-ttu-id="2f7bc-274">Ha egy többszörös sávszélességű eszköz kimeneti van szükség együttesen használják az Azure Media Services szolgáltatásban több GOP igazított MP4-fájlok az egyes egy másik átviteli sebesség és a feloldási kell generált toobe által kínált hello dinamikus becsomagolás funkcióival.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-274">When a multi-bitrate asset output is desired for use in combination with hello Dynamic Packaging features offered by Azure Media Services, multiple GOP-aligned MP4 files of each a different bitrate and resolution will need toobe generated.</span></span> <span data-ttu-id="2f7bc-275">Igen, hello toodo [kódolás MXF be egy egyféle sávszélességű MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) útmutatóul szolgál, az jó kiindulási pont.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-275">toodo so, hello [Encoding MXF into a single bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) walkthrough provides us with a good starting point.</span></span>

![Munkafolyamat indítása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

<span data-ttu-id="2f7bc-277">*Munkafolyamat indítása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-277">*Starting Workflow*</span></span>

### <span data-ttu-id="2f7bc-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Egy vagy több további MP4 kimenetek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-278"><a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Adding one or more additional MP4 outputs</span></span>
<span data-ttu-id="2f7bc-279">Az eredményül kapott Azure Media Services eszközt minden MP4-fájlokat egy másik átviteli sebesség és a feloldási fogja támogatni.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-279">Every MP4 file in our resulting Azure Media Services asset will support a different bitrate and resolution.</span></span> <span data-ttu-id="2f7bc-280">Egy vagy több MP4 kimeneti fájlok toohello munkafolyamat adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-280">Let's add one or more MP4 output files toohello workflow.</span></span>

<span data-ttu-id="2f7bc-281">toomake meg arról, hogy tudunk a videó kódolók létrehozott összes hello ugyanazokat a beállításokat, fennállt legkényelmesebben tooduplicate már meglévő AVC Videókódoló hello, és állítsa be névfeloldási és sávszélességű egy másik kombinációja (adjuk hozzá 960 x 540 egyikét: 25 képkockák másodpercenkénti: 2,5 MB/s).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-281">toomake sure we have all our video encoders created with hello same settings, it's most convenient tooduplicate hello already existing AVC Video Encoder and configure another combination of resolution and bitrate (let's add one of 960 x 540 at 25 frames per second at 2,5 Mbps).</span></span> <span data-ttu-id="2f7bc-282">tooduplicate hello meglévő kódoló másolási illessze be a hello Tervező felületére.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-282">tooduplicate hello existing encoder, copy paste it on hello designer surface.</span></span>

<span data-ttu-id="2f7bc-283">Csatlakozás hello tömörítetlen videó kimeneti hello Media fájl bemeneti az új AVC összetevő be PIN kódját.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-283">Connect hello Uncompressed Video output pin of hello Media File Input into our new AVC component.</span></span>

![Második AVC kódoló csatlakoztatva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

<span data-ttu-id="2f7bc-285">*Második AVC kódoló csatlakoztatva*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-285">*Second AVC encoder connected*</span></span>

<span data-ttu-id="2f7bc-286">Most alkalmazkodnak az új AVC kódoló toooutput 960 x 540 2,5 Mbit/s hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-286">Now adapt hello configuration for our new AVC encoder toooutput 960x540 at 2,5 Mbps.</span></span> <span data-ttu-id="2f7bc-287">(Használható tulajdonságok "kimeneti szélességének", "Kimeneti magasság" és "Átviteli sebesség (KB/s)" a ez.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-287">(Use its properties "Output width", "Output height", and "Bitrate (kbps)" for this.)</span></span>

<span data-ttu-id="2f7bc-288">Megadott toouse hello eredményül kapott eszköz együtt az Azure Media Services dinamikus becsomagolást szeretnénk, hello adatfolyam-továbbítási végpontra kell alkalmas MP4 fájlokat, amelyek pontosan igazított tooeach más módon HLS/Fragmented MP4/DASH-töredék toobe hogy az ügyfelek, amelyek között különböző bitrates vált lekérni egy egyetlen zökkenőmentes folyamatos video- és felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-288">Given we want toouse hello resulting asset together with Azure Media Services' dynamic packaging, hello streaming endpoint needs toobe capable of generating from these MP4 files HLS/Fragmented MP4/DASH fragments that are exactly aligned tooeach other in a way that clients that are switching between different bitrates get a single smooth continuous video and audio experience.</span></span> <span data-ttu-id="2f7bc-289">amely fordulhat elő, igazolnia kell, hogy mindkét AVC kódolók hello tulajdonságaiban hello mindkét MP4-fájlok mérete GOP ("csoport képek") értéke tooensure toomake too2 másodpercen belül végezhető el:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-289">toomake that happen, we need tooensure that, in hello properties of both AVC encoders hello GOP ("group of pictures") size for both MP4 files is set too2 seconds, which can be done by:</span></span>

* <span data-ttu-id="2f7bc-290">hello GOP méretezési módját tooFixed GOP méret a következőre és</span><span class="sxs-lookup"><span data-stu-id="2f7bc-290">setting hello GOP Size Mode tooFixed GOP size and</span></span>
* <span data-ttu-id="2f7bc-291">hello kulcs keret időköz tootwo (másodperc).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-291">hello Key Frame Interval tootwo seconds.</span></span>
* <span data-ttu-id="2f7bc-292">hello GOP IDR vezérlő tooClosed GOP tooensure összes GOP vannak állandó is be a saját nélkül függőségek</span><span class="sxs-lookup"><span data-stu-id="2f7bc-292">also set hello GOP IDR Control tooClosed GOP tooensure all GOP's are standing on their own without dependencies</span></span>

<span data-ttu-id="2f7bc-293">toomake a munkafolyamat kényelmes toounderstand hello első AVC kódoló túl átnevezése "AVC videó kódoló 640 x 360 1200-as kbps" és a második AVC kódoló hello "AVC videó kódoló 960 x 540 2500 kbit/s".</span><span class="sxs-lookup"><span data-stu-id="2f7bc-293">toomake our workflow convenient toounderstand, rename hello first AVC encoder too"AVC Video Encoder 640x360 1200kbps" and hello second AVC encoder "AVC Video Encoder 960x540 2500 kbps".</span></span>

<span data-ttu-id="2f7bc-294">Ezután adja hozzá a második ISO MPEG-4 Multiplexer és egy második fájl kimenet.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-294">Now add a second ISO MPEG-4 Multiplexer and a second File Output.</span></span> <span data-ttu-id="2f7bc-295">Csatlakozás hello multiplexer toohello új AVC kódoló, és győződjön meg arról, annak kimenetét a kimeneti fájl hello van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-295">Connect hello multiplexer toohello new AVC encoder and make sure its output is directed into hello File Output.</span></span> <span data-ttu-id="2f7bc-296">Ezután is csatlakoznak hello AAC hang kódoló kimeneti toohello új multiplexer tartozó bemeneti.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-296">Then also connect hello AAC audio encoder output toohello new multiplexer's input.</span></span> <span data-ttu-id="2f7bc-297">hello kimeneti fájl pedig majd lehet csatlakoztatott toohello kimeneti fájl/eszköz csomópont tooadd azt toohello Media Services eszköz, amely jön létre.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-297">hello File Output in turn can then be connected toohello Output File/Asset node tooadd it toohello Media Services Asset that will be created.</span></span>

![Második Muxer és a kimeneti fájl csatlakoztatva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

<span data-ttu-id="2f7bc-299">*Második Muxer és a kimeneti fájl csatlakoztatva*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-299">*Second Muxer and File Output connected*</span></span>

<span data-ttu-id="2f7bc-300">Azure Media Services dinamikus becsomagolást is kompatibilisek, a multiplexer tartozó hello adatrészlet mód tooGOP számát vagy a duration konfigurálása, valamint hello GOPs adatrészlet too1 / beállítása.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-300">For compatibility with Azure Media Services dynamic packaging, configure hello multiplexer's Chunk Mode tooGOP count or duration and set hello GOPs per chunk too1.</span></span> <span data-ttu-id="2f7bc-301">(Az alapértelmezett hello ennek kell lennie.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-301">(This should be hello default.)</span></span>

![Muxer adatrészlet módok](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

<span data-ttu-id="2f7bc-303">*Muxer adatrészlet módok*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-303">*Muxer Chunk Modes*</span></span>

<span data-ttu-id="2f7bc-304">Megjegyzés: érdemes lehet toorepeat ezt a folyamatot az átviteli sebesség és feloldási toohave kívánt kombinációk toohello eszköz kimenet hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-304">Note: you may want toorepeat this process for any other bitrate and resolution combinations you want toohave added toohello asset output.</span></span>

### <span data-ttu-id="2f7bc-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Kimeneti nevek konfigurálása hello fájl</span><span class="sxs-lookup"><span data-stu-id="2f7bc-305"><a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Configuring hello file output names</span></span>
<span data-ttu-id="2f7bc-306">Egynél több egyfájlos hozzáadott toohello kimeneti adategységen van.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-306">We have more than one single file added toohello output asset.</span></span> <span data-ttu-id="2f7bc-307">Ez lehetővé teszi egy szükséges toomake meg arról, hogy hello az egyes hello fájlnevek kimeneti fájlok különbözik egymástól, és lehet, hogy még érvényes a fájl-elnevezési konvenció így világossá válik, a fájl nevéből hello a most foglalkozó.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-307">This provides a need toomake sure hello filenames for each of hello output files are different from each other and maybe even apply a file-naming convention so it becomes clear from hello file name what you're dealing with.</span></span>

<span data-ttu-id="2f7bc-308">Kimeneti fájlelnevezésnél szabályozható kifejezések hello tervezőben.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-308">File output naming can be controlled through expressions in hello designer.</span></span> <span data-ttu-id="2f7bc-309">Nyissa meg a kimeneti fájl összetevői hello tulajdonság ablaktábla hello, és nyissa meg a kifejezés objektumszerkesztője hello hello fájl tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-309">Open hello property pane for one of hello File Output components and open hello expression editor for hello File property.</span></span> <span data-ttu-id="2f7bc-310">Az első kimeneti fájl lett konfigurálva, a következő kifejezés hello keresztül (lásd: hello útmutató át [MXF tooa egyféle sávszélességű MP4 kimeneti](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span><span class="sxs-lookup"><span data-stu-id="2f7bc-310">Our first output file was configured through hello following expression (see hello tutorial for going from [MXF tooa single bitrate MP4 output](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

<span data-ttu-id="2f7bc-311">Ez azt jelenti, hogy a fájlnév határozza meg két változót: hello kimeneti könyvtár toowrite a és hello adatforrás fájl neve.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-311">This means that our filename is determined by two variables: hello output directory toowrite in and hello source file base name.</span></span> <span data-ttu-id="2f7bc-312">hello volt hello munkafolyamat legfelső szintű tulajdonságként van közzétéve, és ez utóbbi hello hello bejövő fájl határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-312">hello former is exposed as a property on hello workflow root and hello latter is determined by hello incoming file.</span></span> <span data-ttu-id="2f7bc-313">Vegye figyelembe, hogy hello kimeneti könyvtár helyi tesztelési; használata Ez a tulajdonság felülbírálja hello munkafolyamat-motor hello munkafolyamat hello felhőalapú media feldolgozó Azure Media Services eljárás végrehajtásakor.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-313">Note that hello output directory is what you use for local testing; this property will be overridden by hello workflow engine when hello workflow is executed by hello cloud-based media processor in Azure Media Services.</span></span>
<span data-ttu-id="2f7bc-314">toogive mind a kimeneti fájlok elnevezési konzisztens kimeneti, a módosítás hello először fájl elnevezési kifejezés:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-314">toogive both our output files a consistent output naming, change hello first file naming expression to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="2f7bc-315">és a második hello:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-315">and hello second to:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="2f7bc-316">Hajtsa végre egy köztes teszt futtatása toomake meg arról, hogy mindkét MP4 kimeneti fájlok megfelelően jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-316">Execute an intermediate test run toomake sure both MP4 output files are properly generated.</span></span>

### <span data-ttu-id="2f7bc-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Egy külön lejátszása hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-317"><a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Adding a separate Audio Track</span></span>
<span data-ttu-id="2f7bc-318">Megtanulhatja, később generálásakor azt egy .ism-fájlt toogo az MP4 kimeneti fájlokat, mivel azt is szüksége van egy csak MP4-fájlokat hello hang nyomon követése, a az adaptív streameléshez.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-318">As we'll see later when we generate an .ism file toogo with our MP4 output files, we will also require a audio-only MP4 file as hello audio track for our adaptive streaming.</span></span> <span data-ttu-id="2f7bc-319">toocreate a fájl, adja hozzá a további muxer toohello munkafolyamat (Multiplexer ISO-MPEG-4), és csatlakozzon a hello AAC kódoló kimeneti PIN-kód és a bemeneti PIN-kód követése 1.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-319">toocreate this file, add an additional muxer toohello workflow (ISO-MPEG-4 Multiplexer) and connect hello AAC encoder's output pin with its input pin for Track 1.</span></span>

![Hang Muxer hozzáadva](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

<span data-ttu-id="2f7bc-321">*Hang Muxer hozzáadva*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-321">*Audio Muxer Added*</span></span>

<span data-ttu-id="2f7bc-322">Kimeneti fájl összetevő toooutput hello kimenő adatfolyam harmadik alapján hello muxer és hello fájl elnevezési kifejezés konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-322">Create a third File Output component toooutput hello outbound stream from hello muxer and configure hello file naming expression as:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Hang Muxer kimeneti fájl létrehozása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

<span data-ttu-id="2f7bc-324">*Hang Muxer kimeneti fájl létrehozása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-324">*Audio Muxer creating File Output*</span></span>

### <span data-ttu-id="2f7bc-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Hello hozzáadása. ISM SMIL fájl</span><span class="sxs-lookup"><span data-stu-id="2f7bc-325"><a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Adding hello .ISM SMIL File</span></span>
<span data-ttu-id="2f7bc-326">A hello dinamikus becsomagolás toowork együtt mindkét MP4-fájlokat (és hello csak MP4) a Media Services eszközt is kell a jegyzékfájlt (más néven "SMIL" fájlba: multimédia integrációs nyelvi szinkronizálva).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-326">For hello dynamic packaging toowork in combination with both MP4 files (and hello audio-only MP4) in our Media Services asset, we also need a manifest file (also called a "SMIL" file: Synchronized Multimedia Integration Language).</span></span> <span data-ttu-id="2f7bc-327">Ez a fájl milyen MP4-fájlok érhetők el a dinamikus csomagolás és amely ezeket az hello hang adatfolyamként történő tooconsider tooAzure Media Services jelzi.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-327">This file indicates tooAzure Media Services what MP4 files are available for dynamic packaging and which of those tooconsider for hello audio streaming.</span></span> <span data-ttu-id="2f7bc-328">Egy tipikus jegyzékfájl állítja be a MP4 meg egyetlen hangadatfolyam a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-328">A typical manifest file for a set of MP4's with a single audio stream looks like this:</span></span>

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

<span data-ttu-id="2f7bc-329">hello .ism-fájlt tartalmaz egy switch utasításban, egy hivatkozás tooeach hello egyedi MP4 videó fájlok belül, és emellett toothose is egy (vagy több) hangfájl hivatkozik tooan hello hang tartalmazó MP4.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-329">hello .ism file contains within a switch statement, a reference tooeach of hello individual MP4 video files and in addition toothose also one (or more) audio file references tooan MP4 that only contains hello audio.</span></span>

<span data-ttu-id="2f7bc-330">Hello jegyzékfájl létrehozása az MP4 tartozó számú végezhető el hello "AMS Manifest író" nevű összetevőt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-330">Generating hello manifest file for our set of MP4's can be done through a component called hello "AMS Manifest Writer".</span></span> <span data-ttu-id="2f7bc-331">toouse, húzza hello felületet, és csatlakoztassa hello "Írási kész" kimeneti PIN-kódok hello három kimeneti fájl összetevők toohello AMS Manifest író adjon meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-331">toouse it, drag it onto hello surface and connect hello "Write Complete" output pins from hello three File Output components toohello AMS Manifest Writer input.</span></span> <span data-ttu-id="2f7bc-332">Győződjön meg arról, hogy tooconnect hello kimeneti hello AMS Manifest író toohello kimeneti fájl vagy eszköz.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-332">Then make sure tooconnect hello output of hello AMS Manifest Writer toohello Output File/Asset.</span></span>

<span data-ttu-id="2f7bc-333">A többi fájl kimeneti összetevők konfigurálását a hello .ism kimeneti fájlnév kifejezéssel:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-333">As with our other file output components, configure hello .ism file output name with an expression:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

<span data-ttu-id="2f7bc-334">A befejezett munkafolyamat alábbi hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-334">Our finished workflow looks like hello below:</span></span>

![Befejezett MXF toomultibitrate MP4 munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

<span data-ttu-id="2f7bc-336">*Befejezett MXF toomultibitrate MP4 munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-336">*Finished MXF toomultibitrate MP4 workflow*</span></span>

## <span data-ttu-id="2f7bc-337"><a id="MXF_to__multibitrate_MP4"></a>Kódolási MXF multibitrate MP4 - továbbfejlesztett tervezetének be</span><span class="sxs-lookup"><span data-stu-id="2f7bc-337"><a id="MXF_to__multibitrate_MP4"></a>Encoding MXF into multibitrate MP4 - enhanced blueprint</span></span>
<span data-ttu-id="2f7bc-338">A hello [előző munkafolyamat forgatókönyv](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) azt láthatta, hogyan MXF egyetlen bemeneti eszköz létrehozható egy kimeneti eszköz többszörös sávszélességű MP4-fájlok, a csak MP4-fájlokat és a jegyzékfájlt, az Azure Media együtt használható Services dinamikus becsomagolást.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-338">In hello [previous workflow walkthrough](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) we've seen how a single MXF input asset can be converted into an output asset with multi-bitrate MP4 files, an audio-only MP4 file and a manifest file for use in conjunction with Azure Media Services dynamic packaging.</span></span>

<span data-ttu-id="2f7bc-339">Ez a forgatókönyv hogyan néhány hello szempontok melyek fejlesztése és kényelmesebb végrehajtott jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-339">This walkthrough will show how some of hello aspects can be enhanced and made more convenient.</span></span>

### <span data-ttu-id="2f7bc-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Munkafolyamat áttekintése tooenhance</span><span class="sxs-lookup"><span data-stu-id="2f7bc-340"><a id="MXF_to_multibitrate_MP4_overview"></a>Workflow overview tooenhance</span></span>
![Multibitrate MP4 munkafolyamat tooenhance](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

<span data-ttu-id="2f7bc-342">*Multibitrate MP4 munkafolyamat tooenhance*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-342">*Multibitrate MP4 workflow tooenhance*</span></span>

### <span data-ttu-id="2f7bc-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>A fájlok elnevezési konvenciók</span><span class="sxs-lookup"><span data-stu-id="2f7bc-343"><a id="MXF_to__multibitrate_MP4_file_naming"></a>File Naming Conventions</span></span>
<span data-ttu-id="2f7bc-344">Hello előző munkafolyamat létrehozásának kimeneti fájl nevének hello alapjául azt meg egyetlen egyszerű kifejezésbe.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-344">In hello previous workflow we specified a simple expression as hello basis for generating output file names.</span></span> <span data-ttu-id="2f7bc-345">Néhány duplikálva lettek-e, ha van: hello hello egyes kimeneti fájl összetevőket megadott ilyen kifejezés.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-345">We have some duplication though: all of hello hello individual output file components specified such expression.</span></span>

<span data-ttu-id="2f7bc-346">Például a fájl kimeneti összetevőjének hello első videofájl ebben a kifejezésben van konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-346">For example, our file output component for hello first video file is configured with this expression:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

<span data-ttu-id="2f7bc-347">A hello második kimeneti videó, például a kifejezés vezetünk be:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-347">While for hello second output video, we have an expression like:</span></span>

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

<span data-ttu-id="2f7bc-348">Lenne kisebb hiba nagyon eséllyel fordulnak elő, és több akkor hasznos, ha azt nem sikerült távolítson el néhányat a ismétlődést és tétele több konfigurálható helyette tisztító?</span><span class="sxs-lookup"><span data-stu-id="2f7bc-348">Wouldn't it be cleaner, less error prone and more convenient if we could remove some of this duplication and make things more configurable instead?</span></span> <span data-ttu-id="2f7bc-349">Szerencsére alábbiakat tehetjük: hello designer kifejezés képességek hello képességét toocreate egyéni tulajdonságok a munkafolyamat legfelső szintű együtt fog biztosítják egy hozzáadott réteget kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-349">Luckily we can: hello designer's expression capabilities in combination with hello ability toocreate custom properties on our workflow root will give us an added layer of convenience.</span></span>

<span data-ttu-id="2f7bc-350">Tegyük fel, azt fogja hello bitrates hello egyedi MP4-fájlokat a fájlnév konfigurációs meghajtó.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-350">Let's assume we'll drive filename configuration from hello bitrates of hello individual MP4 files.</span></span> <span data-ttu-id="2f7bc-351">Ezek bitrates lesz igyekszünk tooconfigure a egy központi helyezze (hello gyökere a diagramhoz), a ahol azok lesz használt tooconfigure és a meghajtó fájlnév létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-351">These bitrates we'll aim tooconfigure in one central place (on hello root of our graph), from where they'll be accessed tooconfigure and drive file name generation.</span></span> <span data-ttu-id="2f7bc-352">toodo, először hello sávszélességű tulajdonság mindkét AVC kódolók toohello legfelső szintű a munkafolyamat közzétételével, hogy az hello AVC kódolók maga mindkét hello legfelső szintű is elérhető.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-352">toodo this, we start by publishing hello bitrate property from both AVC encoders toohello root of our workflow, so that it becomes accessible from both hello root as well as from hello AVC encoders.</span></span> <span data-ttu-id="2f7bc-353">(Még akkor is, ha két különböző tesztüzeméhez jelenik meg, nincs csak egy alapul szolgáló érték.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-353">(Even if displayed in two different spots, there's only one underlying value.)</span></span>

### <span data-ttu-id="2f7bc-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Hello munkafolyamat legfelső szintű alakzatot közzétételi összetevő tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="2f7bc-354"><a id="MXF_to__multibitrate_MP4_publishing"></a>Publishing component properties onto hello workflow root</span></span>
<span data-ttu-id="2f7bc-355">Nyissa meg a hello első AVC kódoló toohello sávszélességű (kbps) tulajdonság nyissa és hello legördülő menüből válassza a Publish.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-355">Open hello first AVC encoder, go toohello Bitrate (kbps) property and from hello dropdown choose Publish.</span></span>

![Közzétételi hello sávszélességű tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

<span data-ttu-id="2f7bc-357">*Közzétételi hello sávszélességű tulajdonság*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-357">*Publishing hello bitrate property*</span></span>

<span data-ttu-id="2f7bc-358">Hello konfigurálása közzététele párbeszédpanelen toopublish toohello legfelső szintű a munkafolyamat gráf egy közzétett neve "video1bitrate" és "Videó 1 sávszélességű" olvasható megjelenítendő neve.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-358">Configure hello publish dialog toopublish toohello root of our workflow graph, with a published name of "video1bitrate" and a readable display name of "Video 1 Bitrate".</span></span> <span data-ttu-id="2f7bc-359">Konfigurálja az egyéni csoport neve "Streaming Bitrates" néven, majd nyomja le a közzététel.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-359">Configure a custom group name called "Streaming Bitrates" and hit Publish.</span></span>

![Közzétételi hello sávszélességű tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

<span data-ttu-id="2f7bc-361">*Átviteli sebesség tulajdonság közzétételi párbeszédpanel*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-361">*Publishing dialog for bitrate property*</span></span>

<span data-ttu-id="2f7bc-362">Ismétlődő hello hello hello sávszélességű tulajdonságának azonos a második AVC kódoló, és nevezze el "Videó 2 sávszélességű" megjelenítendő nevű "video2bitrate", a hello azonos egyéni csoport "Streaming Bitrates".</span><span class="sxs-lookup"><span data-stu-id="2f7bc-362">Repeat hello same for hello bitrate property of hello second AVC encoder and name it "video2bitrate" with a display name of "Video 2 Bitrate", in hello same custom group "Streaming Bitrates".</span></span>

<span data-ttu-id="2f7bc-363">A Microsoft hello munkafolyamat legfelső szintű tulajdonságok most vizsgálja meg, ha megtanulhatja az egyéni csoport hello két közzétett tulajdonság jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-363">If we now inspect hello workflow root properties, we'll see our custom group with hello two published properties show up.</span></span> <span data-ttu-id="2f7bc-364">Mindkét van tükrözve hello értékének a megfelelő AVC kódoló sávszélességű.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-364">Both are reflecting hello value of their respective AVC encoder bitrate.</span></span>

![A munkafolyamat legfelső szintű közzétett sávszélességű tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

<span data-ttu-id="2f7bc-366">Ha azt szeretné, hogy tooaccess ezeket a tulajdonságokat, kódból vagy egy kifejezésből, azt is megteheti ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-366">Whenever we want tooaccess these properties from code or from an expression, we can do so like this:</span></span>

* <span data-ttu-id="2f7bc-367">a beágyazott kód egy összetevő hello gyökér alatti jobb oldali: node.getPropertyAsString('.. / video1bitrate ", null)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-367">from inline code from a component right below hello root: node.getPropertyAsString('../video1bitrate',null)</span></span>
* <span data-ttu-id="2f7bc-368">kifejezésben: ${ROOT_video1bitrate}</span><span class="sxs-lookup"><span data-stu-id="2f7bc-368">within an expression: ${ROOT_video1bitrate}</span></span>

<span data-ttu-id="2f7bc-369">Hello "Streaming Bitrates" csoport befejezéseként közzététele a zenei sávszélességű rajta is.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-369">Let's complete hello "Streaming Bitrates" group by publishing our audio track bitrate on it as well.</span></span> <span data-ttu-id="2f7bc-370">Belül hello AAC kódoló hello tulajdonságait keresse meg a hello sávszélességű beállítás, és válassza a Publish hello legördülő következő tooit parancsát.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-370">Within hello properties of hello AAC Encoder, search for hello Bitrate setting and select Publish from hello dropdown next tooit.</span></span> <span data-ttu-id="2f7bc-371">A neve "audio1bitrate" hello graph toohello gyökérmappájában közzététele, és megjelenítendő név "Hang 1 sávszélességű" a "Folyamatos átviteli Bitrates" egyéni csoporton belül.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-371">Publish toohello root of hello graph with name "audio1bitrate" and display name "Audio 1 Bitrate" within our custom group "Streaming Bitrates".</span></span>

![A hang sávszélességű közzétételi párbeszédpanel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

<span data-ttu-id="2f7bc-373">*A hang sávszélességű közzétételi párbeszédpanel*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-373">*Publishing dialog for audio bitrate*</span></span>

![A gyökérszintű eredményül kapott video- és tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

<span data-ttu-id="2f7bc-375">*A gyökérszintű eredményül kapott video- és tulajdonságai*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-375">*Resulting video and audio props on root*</span></span>

<span data-ttu-id="2f7bc-376">Ne feledje, hogy ezen három megváltoztatásával is értékei, konfigurálja újra módosítások hello hello megfelelő összetevőinek kapcsolódnak az értékeket (és egyes esetekben közzétett).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-376">Note that changing any of those three values also re-configures and changes hello values on hello respective components they are linked with (and where published from).</span></span>

### <span data-ttu-id="2f7bc-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Kimeneti fájl nevét közzétett tulajdonságértékek támaszkodnak hozta létre.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-377"><a id="MXF_to__multibitrate_MP4_output_files"></a>Have generated output file names rely on published property values</span></span>
<span data-ttu-id="2f7bc-378">Hardcoding helyett a létrehozott fájl nevének azt módosíthatja az egyes hello kimeneti fájl összetevők toorely hello sávszélességű tulajdonságainál azt csak közzétett hello gráf legfelső szintű fájlnév kifejezés.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-378">Instead of hardcoding our generated file names, we can now change our filename expression on each of hello File Output components toorely on hello bitrate properties we just published on hello graph root.</span></span> <span data-ttu-id="2f7bc-379">Az első kimeneti fájl verziótól kezdődően található hello fájl tulajdonság, és ehhez hasonló hello kifejezés szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-379">Starting with our first file output, find hello File property and edit hello expression like this:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

<span data-ttu-id="2f7bc-380">hello különböző paraméterek ebben a kifejezésben érhető el, és szerezze meg a hello dollárjel hello billentyűzet hello kifejezés ablakban adja meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-380">hello different parameters in this expression can be accessed and entered by hitting hello dollar sign on hello keyboard while in hello expression window.</span></span> <span data-ttu-id="2f7bc-381">Hello rendelkezésre álló paraméterek egyike a video1bitrate tulajdonság, amely azt a korábban közzétett.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-381">One of hello available parameters is our video1bitrate property which we published earlier.</span></span>

![Paraméterek kifejezésben elérése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

<span data-ttu-id="2f7bc-383">*Paraméterek kifejezésben elérése*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-383">*Accessing parameters within an expression*</span></span>

<span data-ttu-id="2f7bc-384">Hello ugyanazt a hello kimenetét a második videót:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-384">Do hello same for hello file output for our second video:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

<span data-ttu-id="2f7bc-385">és hello csak fájl kimeneti:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-385">and for hello audio-only file output:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

<span data-ttu-id="2f7bc-386">Ha most módosítjuk hello sávszélességű bármely hello video- vagy hangfájlok, hello megfelelő kódoló újrakonfigurálni, és hello sávszélességű alapú fájl neve egyezmény szerződéses kötelezettségeket összes automatikus.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-386">If we now change hello bitrate for any of hello video or audio files, hello respective encoder will be reconfigured and hello bitrate-based file name convention will be honored all automatic.</span></span>

## <span data-ttu-id="2f7bc-387"><a id="thumbnails_to__multibitrate_MP4"></a>Miniatűrök toomultibitrate MP4 kimenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-387"><a id="thumbnails_to__multibitrate_MP4"></a>Adding thumbnails toomultibitrate MP4 output</span></span>
<span data-ttu-id="2f7bc-388">Amely hoz létre egy munkafolyamat-től kezdődő [egy multibitrate MP4 kimenete egy bemeneti MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), azt most lehet szüksége a miniatűrök toohello kimenet hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-388">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into adding thumbnails toohello output.</span></span>

### <span data-ttu-id="2f7bc-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Munkafolyamat áttekintése tooadd miniatűrök</span><span class="sxs-lookup"><span data-stu-id="2f7bc-389"><a id="thumbnails_to__multibitrate_MP4_overview"></a>Workflow overview tooadd thumbnails to</span></span>
![A Multibitrate MP4 munkafolyamat toostart](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

<span data-ttu-id="2f7bc-391">*A Multibitrate MP4 munkafolyamat toostart*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-391">*Multibitrate MP4 workflow toostart from*</span></span>

### <span data-ttu-id="2f7bc-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG kódolás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-392"><a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Adding JPG Encoding</span></span>
<span data-ttu-id="2f7bc-393">a miniatűr generációs hello szív hello JPG kódoló összetevő, JPG-fájlokat képes toooutput lesz.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-393">hello heart of our thumbnail generation will be hello JPG Encoder component, able toooutput JPG files.</span></span>

![JPG kódoló](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

<span data-ttu-id="2f7bc-395">*JPG kódoló*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-395">*JPG Encoder*</span></span>

<span data-ttu-id="2f7bc-396">Az adathordozó fájl bemeneti hello hello JPG kódoló be azonban közvetlenül a tömörítetlen Video-adatfolyamot nem kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-396">We cannot however directly connect our Uncompressed Video stream from hello Media File Input into hello JPG encoder.</span></span> <span data-ttu-id="2f7bc-397">Ehelyett vár toobe egyes keretek átadni.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-397">Instead, it expects toobe handed individual frames.</span></span> <span data-ttu-id="2f7bc-398">Ez a Microsoft hello videó keret kapu összetevő segítségével teheti meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-398">This, we can do through hello Video Frame Gate component.</span></span>

![A keret kapu toohello JPG kódoló csatlakozás](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

<span data-ttu-id="2f7bc-400">*A keret kapu toohello JPG kódoló csatlakozás*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-400">*Connecting a frame gate toohello JPG encoder*</span></span>

<span data-ttu-id="2f7bc-401">hello keret kapu, miután minden olyan sok másodperceken vagy keretek lehetővé teszi, hogy a videó keret toopass.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-401">hello frame gate once every so many seconds or frames allows a video frame toopass.</span></span> <span data-ttu-id="2f7bc-402">hello időköz és hello időeltolódás, amelyhez ez akkor fordul elő hello tulajdonságaiban konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-402">hello interval and hello time offset with which this happens is configurable in hello properties.</span></span>

![Videó keret kapu tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

<span data-ttu-id="2f7bc-404">*Videó keret kapu tulajdonságai*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-404">*Video Frame Gate properties*</span></span>

<span data-ttu-id="2f7bc-405">Most percenként miniatűr létrehozása úgy, hogy hello mód tooTime (másodpercben), és időköz too60 hello.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-405">Let's create a thumbnail every minute by setting hello mode tooTime (seconds) and hello Interval too60.</span></span>

### <span data-ttu-id="2f7bc-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>A szín terület átalakítás foglalkozó</span><span class="sxs-lookup"><span data-stu-id="2f7bc-406"><a id="thumbnails_to__multibitrate_MP4_color_space"></a>Dealing with Color Space conversion</span></span>
<span data-ttu-id="2f7bc-407">Közben logikai tűnik mindkét hello keret kapu és hello Media fájl bemeneti tömörítetlen videó PIN-kód most csatlakozhat, a figyelmeztetés azt visszajelzést kap, ha azt szeretné ehhez.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-407">While it would seem logical both Uncompressed Video pins of hello frame gate and hello Media File Input can now be connected, we would get a warning if we would do so.</span></span>

![Bemeneti szín helyének hibája](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

<span data-ttu-id="2f7bc-409">*Bemeneti szín helyének hibája*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-409">*Input color space error*</span></span>

<span data-ttu-id="2f7bc-410">Ennek oka az, mely színű információk jelennek meg az eredeti nyers tömörítetlen video-adatfolyammá alakítja, a MXF érkező hello módja eltér milyen hello JPG kódoló által várt paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-410">This is because hello way in which colour information is represented in our original raw uncompressed video stream, coming from our MXF, is different from what hello JPG Encoder is expecting.</span></span> <span data-ttu-id="2f7bc-411">Több, egy úgynevezett "szín lemezterület" "RGB" vagy "Szürkeárnyalatos" van tooflow a várt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-411">More specifically, a so-called "color space" of "RGB" or "Grayscale" is expected tooflow in.</span></span> <span data-ttu-id="2f7bc-412">Ez azt jelenti, hogy hello videó keret kapu tartozó bejövő video-adatfolyamot kell toohave alkalmazza a szín terület vonatkozó először az átalakításhoz.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-412">This means that hello Video Frame Gate's inbound video stream will need toohave a conversion applied regarding its color space first.</span></span>

<span data-ttu-id="2f7bc-413">Hello munkafolyamat hello szín terület konverter - Intel alakzatot, és csatlakoztassa tooour keret kapu.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-413">Drag onto hello workflow hello Color Space Converter - Intel and connect it tooour frame gate.</span></span>

![Csatlakozás egy szín terület konverter](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

<span data-ttu-id="2f7bc-415">*Csatlakozás egy szín terület konverter*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-415">*Connecting a Color Space Convertor*</span></span>

<span data-ttu-id="2f7bc-416">Hello tulajdonságai ablakban válassza ki a hello BGR 24 bejegyzést hello előre definiált listából.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-416">In hello properties window, pick hello BGR 24 entry from hello Preset list.</span></span>

### <span data-ttu-id="2f7bc-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Írás hello miniatűrök</span><span class="sxs-lookup"><span data-stu-id="2f7bc-417"><a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Writing hello thumbnails</span></span>
<span data-ttu-id="2f7bc-418">Eltér a MP4 videó, hello összetevő kimeneteként JPG kódoló több mint egy fájl.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-418">Different from our MP4 video's, hello JPG Encoder component will output more than one file.</span></span> <span data-ttu-id="2f7bc-419">Ennek rendelés toodeal, a leképezni kívánt jelenetben keresési JPG fájl író összetevőt is használható: hello bejövő JPG miniatűrök igénybe vehet, és beírhatók, minden egyes eltérő számú által éppen utótaggal fájlnév.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-419">In order toodeal with this, a Scene Search JPG File Writer component can be used: it will take hello incoming JPG thumbnails and write them out, each filename being suffixed by a different number.</span></span> <span data-ttu-id="2f7bc-420">(hello azonosítószámát általában hello hello adatfolyamban mely hello miniatűr megrajzolása a másodperc/egységek számát.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-420">(hello number typically indicating hello number of seconds/units in hello stream which hello thumbnail was drawn from.)</span></span>

![Hello helyszín keresési JPG fájl író bemutatása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

<span data-ttu-id="2f7bc-422">*Hello helyszín keresési JPG fájl író bemutatása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-422">*Introducing hello Scene Search JPG File Writer*</span></span>

<span data-ttu-id="2f7bc-423">Hello kimeneti mappa elérési útja tulajdonság beállítása a hello kifejezéssel: ${ROOT_outputWriteDirectory}</span><span class="sxs-lookup"><span data-stu-id="2f7bc-423">Configure hello Output Folder Path property with hello expression: ${ROOT_outputWriteDirectory}</span></span>

<span data-ttu-id="2f7bc-424">és a fájlnév előtag tulajdonság hello:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-424">and hello Filename Prefix property with:</span></span>

    ${ROOT_sourceFileBaseName}_thumb_

<span data-ttu-id="2f7bc-425">hello előtag határozza meg, hogyan hello miniatűr fájlok neve alatt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-425">hello prefix will determine how hello thumbnail files are being named.</span></span> <span data-ttu-id="2f7bc-426">Azok a rendszer a hello adatfolyam pozíciója egy számot jelző hello görgetőgomb tartozó utótaggal.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-426">They will be suffixed with a number indicating hello thumb's position in hello stream.</span></span>

![Megjelenítés keresési JPG fájl író tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

<span data-ttu-id="2f7bc-428">*Megjelenítés keresési JPG fájl író tulajdonságai*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-428">*Scene Search JPG File Writer properties*</span></span>

<span data-ttu-id="2f7bc-429">Csatlakozás hello helyszín keresési JPG fájl író toohello kimeneti fájl/eszköz csomópont.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-429">Connect hello Scene Search JPG File Writer toohello Output File/Asset node.</span></span>

### <span data-ttu-id="2f7bc-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>A munkafolyamat hibáinak észleléséhez</span><span class="sxs-lookup"><span data-stu-id="2f7bc-430"><a id="thumbnails_to__multibitrate_MP4_errors"></a>Detecting errors in a workflow</span></span>
<span data-ttu-id="2f7bc-431">Csatlakozás hello szín terület konverter toohello nyers tömörítetlen videokimenetéhez hello bevitelt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-431">Connect hello input of hello color space converter toohello raw uncompressed video output.</span></span> <span data-ttu-id="2f7bc-432">Most futtassa a hello munkafolyamat helyi tesztjének elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-432">Now perform a local test run for hello workflow.</span></span> <span data-ttu-id="2f7bc-433">Egy jó eséllyel hello munkafolyamat hirtelen végrehajtása leállítása és egy piros vázlatot hello összetevő, amely a hibát jelző van:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-433">There's a good chance hello workflow will suddenly stop executing and indicate with a red outline on hello component that encountered an error:</span></span>

![Szín terület konverter hiba](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

<span data-ttu-id="2f7bc-435">*Szín terület konverter hiba*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-435">*Color Space Converter error*</span></span>

<span data-ttu-id="2f7bc-436">Kattintson a "E" ikonra kissé piros hello hello jobb felső sarkában hello szín terület konverter összetevő toosee mi hello OK hello kódolási kísérlet sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-436">Click hello little red "E" icon in hello top right corner of hello Color Space Converter component toosee what's hello reason hello encoding attempt failed.</span></span>

![Szín terület konverter hiba-párbeszédpanelen.](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

<span data-ttu-id="2f7bc-438">*Szín terület konverter hiba-párbeszédpanelen.*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-438">*Color Space Converter error dialog*</span></span>

<span data-ttu-id="2f7bc-439">Változik, ahogy látja, hogy rendelkezik-e a kért átalakításához YUV tooRGB toobe rec601 hello bejövő szín terület hello szín terület konverter szabvány.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-439">It turns out, as you can see, that hello incoming color space standard for hello color space converter has toobe rec601 for our requested conversion of YUV tooRGB.</span></span> <span data-ttu-id="2f7bc-440">Az adatfolyam látszólag nem jelzi annak rec601.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-440">Apparently our stream doesn't indicate it's rec601.</span></span> <span data-ttu-id="2f7bc-441">(Jav. 601 a digitális videót formában váltakozó analóg videó jelek kódolási szabványos.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-441">(Rec 601 is a standard for encoding interlaced analog video signals in digital video form.</span></span> <span data-ttu-id="2f7bc-442">Azt adja meg az aktív terület 720 fénysűrűség mintákat és 360 chrominance minták soronként.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-442">It specifies an active region covering 720 luminance samples and 360 chrominance samples per line.</span></span> <span data-ttu-id="2f7bc-443">rendszer kódolás hello szín YCbCr 4 néven: 2:2.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-443">hello color encoding system is known as YCbCr 4:2:2.)</span></span>

<span data-ttu-id="2f7bc-444">toofix, azt fogja az adatfolyam, amely jelenleg éppen foglalkozó rec601 tartalom hello metaadatainak jelzi.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-444">toofix this, we'll indicate on hello metadata of our stream that we're dealing with rec601 content.</span></span> <span data-ttu-id="2f7bc-445">toodo, egy videó adatok típusa Frissítőjének összetevő, amelyeket igazolnia kell a Between a nyers forrás- és hello szín terület átalakítás összetevőjének fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-445">toodo so we'll use a Video Data Type Updater component, which we'll put in between our raw source and hello color space conversion component.</span></span> <span data-ttu-id="2f7bc-446">Ezen adatok típusa frissítőjének hello kézi frissítés egyes videokártya adatok típustulajdonságokat teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-446">This data type updater allows for hello manual update of certain video data type properties.</span></span> <span data-ttu-id="2f7bc-447">A szín terület szabványos a "Rec 601" tooindicate konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-447">Configure it tooindicate a Color Space Standard of "Rec 601".</span></span> <span data-ttu-id="2f7bc-448">Ennek hatására hello videó adatok típusa Frissítőjének tootag hello adatfolyam hello "Rec 601" szín területtel rendelkező Ha nincs meghatározva szín terület történt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-448">This will cause hello Video Data Type Updater tootag hello stream with hello "Rec 601" color space if there was no color space defined yet.</span></span> <span data-ttu-id="2f7bc-449">(Nem felülírja a meglévő metaadatokat, kivéve, ha hello felülbírálás jelölőnégyzetet, de.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-449">(It will not override any existing metadata, unless hello Override checkbox was checked.)</span></span>

![Az adatok típusa Frissítőjének hello szín terület szabványos frissítése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

<span data-ttu-id="2f7bc-451">*Az adatok típusa Frissítőjének hello szín terület szabványos frissítése*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-451">*Updating Color Space Standard on hello Data Type Updater*</span></span>

### <span data-ttu-id="2f7bc-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Befejezett munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="2f7bc-452"><a id="thumbnails_to__multibitrate_MP4_finish"></a>Finished Workflow</span></span>
<span data-ttu-id="2f7bc-453">Most, hogy az a munkafolyamat befejeződött, akkor adjon át egy másik teszt futtatásakor toosee tegye.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-453">Now that our our workflow is finished, do another test run toosee it pass.</span></span>

![Befejezett munkafolyamat vázlattal multi-mp4-kimenet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

<span data-ttu-id="2f7bc-455">*Befejezett munkafolyamat vázlattal multi-mp4-kimenet*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-455">*Finished workflow for multi-mp4 output with thumbnails*</span></span>

## <span data-ttu-id="2f7bc-456"><a id="time_based_trim"></a>Tisztítás időalapú multibitrate MP4 kimenet</span><span class="sxs-lookup"><span data-stu-id="2f7bc-456"><a id="time_based_trim"></a>Time-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="2f7bc-457">Amely hoz létre egy munkafolyamat-től kezdődő [egy multibitrate MP4 kimenete egy bemeneti MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), azt most kell keresése a díszítésre hello forrás videó időbélyegeket alapján.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-457">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming hello source video based on time-stamps.</span></span>

### <span data-ttu-id="2f7bc-458"><a id="time_based_trim_start"></a>Munkafolyamat áttekintése toostart levágási történő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-458"><a id="time_based_trim_start"></a>Workflow overview toostart adding trimming to</span></span>
![A munkafolyamat tooadd levágási indítása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

<span data-ttu-id="2f7bc-460">*A munkafolyamat tooadd levágási indítása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-460">*Starting workflow tooadd trimming to*</span></span>

### <span data-ttu-id="2f7bc-461"><a id="time_based_trim_use_stream_trimmer"></a>Az adatfolyam vágó hello használata</span><span class="sxs-lookup"><span data-stu-id="2f7bc-461"><a id="time_based_trim_use_stream_trimmer"></a>Using hello Stream Trimmer</span></span>
<span data-ttu-id="2f7bc-462">hello adatfolyam vágó összetevő lehetővé teszi, hogy tootrim hello kezdetét és végét egy bemeneti adatfolyam alapjául információk (másodperc, perc,...). hello vágó nem támogatja a keret-alapú tisztítás.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-462">hello Stream Trimmer component allows tootrim hello beginning and ending of an input stream base on timing information (seconds, minutes, ...). hello trimmer does not support frame-based trimming.</span></span>

![Az adatfolyam vágó](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

<span data-ttu-id="2f7bc-464">*Az adatfolyam vágó*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-464">*Stream Trimmer*</span></span>

<span data-ttu-id="2f7bc-465">Helyett linking hello AVC kódolók és hangalapú pozíció assigner toohello Media fájl bemeneti közvetlenül, azt fogja helyezze azokat hello adatfolyam vágó Between.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-465">Instead of linking hello AVC encoders and speaker position assigner toohello Media File Input directly, we'll put in between those hello stream trimmer.</span></span> <span data-ttu-id="2f7bc-466">(Egy hello videó jel, egy másik kihagyásos hang jel hello pedig.)</span><span class="sxs-lookup"><span data-stu-id="2f7bc-466">(One for hello video signal and one for hello interleaved audio signal.)</span></span>

![Az adatfolyam vágó helyezze a kettő között](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

<span data-ttu-id="2f7bc-468">*Az adatfolyam vágó helyezze a kettő között*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-468">*Put Stream Trimmer in between*</span></span>

<span data-ttu-id="2f7bc-469">Hello vágó pedig konfiguráljuk az, hogy a videó és hang 15 másodperc és hello videó 60 másodperc között csak azt fogja feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-469">Let's configure hello trimmer so that we will only process video and audio between 15 seconds and 60 seconds in hello video.</span></span>

<span data-ttu-id="2f7bc-470">Nyissa meg hello videó adatfolyam vágó toohello tulajdonságait, és (15 mp) kezdete és a befejező időpont (60s) tulajdonságok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-470">Go toohello properties of hello Video Stream Trimmer and configure both Start Time (15s) and End Time (60s) properties.</span></span> <span data-ttu-id="2f7bc-471">arról, hogy mind a hang- és vágó mindig azonos mikor kezdődjön és fejeződjön értékek konfigurált toohello toomake fogunk közzé tenni azokat hello munkafolyamat toohello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-471">toomake sure both our audio and video trimmer are always configured toohello same start and end values, we will publish those toohello root of hello workflow.</span></span>

![A kezdési idő tulajdonságot adatfolyam vágó közzététele](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

<span data-ttu-id="2f7bc-473">*A kezdési idő tulajdonságot adatfolyam vágó közzététele*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-473">*Publish start time property from Stream Trimmer*</span></span>

![A kezdő időpont tulajdonság párbeszédpanel közzététele](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

<span data-ttu-id="2f7bc-475">*A kezdő időpont tulajdonság párbeszédpanel közzététele*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-475">*Publish property dialog for start time*</span></span>

![Tulajdonság párbeszédpanel közzé a befejezési időpontja](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

<span data-ttu-id="2f7bc-477">*Tulajdonság párbeszédpanel közzé a befejezési időpontja*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-477">*Publish property dialog for end time*</span></span>

<span data-ttu-id="2f7bc-478">Azt a munkafolyamat hello gyökérmappájában most vizsgálja meg, ha mindkét tulajdonság lesz egyszerű jelennek meg és konfigurálható onnan.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-478">If we now inspect hello root of our workflow, both properties will be neatly displayed and configurable from there.</span></span>

![A gyökérszintű elérhető közzétett tulajdonságok](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

<span data-ttu-id="2f7bc-480">*A gyökérszintű elérhető közzétett tulajdonságok*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-480">*Published properties available on root*</span></span>

<span data-ttu-id="2f7bc-481">Most hello levágási tulajdonságainak megnyitása hello hang vágó, és állítsa be a kezdő és záró időpontjának toohello hivatkozó kifejezést hello legfelső szintű a munkafolyamat tulajdonságok közzététele.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-481">Now open hello trimming properties from hello audio trimmer and configure both start and end times with an expression that refers toohello published properties on hello root of our workflow.</span></span>

<span data-ttu-id="2f7bc-482">A hello hang tisztítás kezdő időpontja:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-482">For hello audio trimming start time:</span></span>

    ${ROOT_TrimmingStartTime}

<span data-ttu-id="2f7bc-483">és a befejezési ideje:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-483">and for its end time:</span></span>

    ${ROOT_TrimmingEndTime}

### <span data-ttu-id="2f7bc-484"><a id="time_based_trim_finish"></a>Befejezett munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="2f7bc-484"><a id="time_based_trim_finish"></a>Finished Workflow</span></span>
![Befejezett munkafolyamat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

<span data-ttu-id="2f7bc-486">*Befejezett munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-486">*Finished Workflow*</span></span>

## <span data-ttu-id="2f7bc-487"><a id="scripting"></a>Introducing hello összetevő parancsprogrammal létrehozva</span><span class="sxs-lookup"><span data-stu-id="2f7bc-487"><a id="scripting"></a>Introducing hello Scripted Component</span></span>
<span data-ttu-id="2f7bc-488">Parancsprogram-alapú összetevők tetszőleges parancsfájlokat futtathat a munkafolyamat hello végrehajtási fázisában.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-488">Scripted Components can execute arbitrary scripts during hello execution phases of our workflow.</span></span> <span data-ttu-id="2f7bc-489">Négy különböző parancsprogramok hajt végre, az adott jellemzőit és a saját hello munkafolyamat életciklus-helyet:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-489">There are four different scripts that can be executed, each with specific characteristics and their own place in hello workflow life-cycle:</span></span>

* <span data-ttu-id="2f7bc-490">**commandScript**</span><span class="sxs-lookup"><span data-stu-id="2f7bc-490">**commandScript**</span></span>
* <span data-ttu-id="2f7bc-491">**realizeScript**</span><span class="sxs-lookup"><span data-stu-id="2f7bc-491">**realizeScript**</span></span>
* <span data-ttu-id="2f7bc-492">**processInputScript**</span><span class="sxs-lookup"><span data-stu-id="2f7bc-492">**processInputScript**</span></span>
* <span data-ttu-id="2f7bc-493">**lifeCycleScript**</span><span class="sxs-lookup"><span data-stu-id="2f7bc-493">**lifeCycleScript**</span></span>

<span data-ttu-id="2f7bc-494">hello hello dokumentációját parancsprogrammal létrehozva összetevő kerül részletesebben minden fenti hello.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-494">hello documentation of hello Scripted Component goes in more detail for each of hello above.</span></span> <span data-ttu-id="2f7bc-495">A [szakasz következő hello](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** hello munkafolyamat indításakor parancsfájl-kezelési összetevője használt tooconstruct egy cliplist xml a hello menet közben.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-495">In [hello following section](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** scripting component is used tooconstruct a cliplist xml on hello fly when hello workflow starts.</span></span> <span data-ttu-id="2f7bc-496">A parancsprogram neve csak egyszer történik meg életciklusának hello összetevő telepítése során.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-496">This script is called during hello component setup, which happens only once in it's lifecycle.</span></span>

### <span data-ttu-id="2f7bc-497"><a id="scripting_hello_world"></a>A munkafolyamaton belül Scripting: hello world</span><span class="sxs-lookup"><span data-stu-id="2f7bc-497"><a id="scripting_hello_world"></a>Scripting within a workflow: hello world</span></span>
<span data-ttu-id="2f7bc-498">Húzzon egy parancsprogrammal létrehozva összetevő hello Tervező felületére, és adjon neki (például "SetClipListXML").</span><span class="sxs-lookup"><span data-stu-id="2f7bc-498">Drag a Scripted Component onto hello designer surface and rename it (for example, "SetClipListXML").</span></span>

![A parancsfájlalapú összetevő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="2f7bc-500">*A parancsfájlalapú összetevő hozzáadása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-500">*Adding a Scripted Component*</span></span>

<span data-ttu-id="2f7bc-501">Amikor hello tulajdonságainak nézze meg hello összetevő parancsprogrammal létrehozva, hello négy különböző parancsfájl típusok megjelenik, minden konfigurálható tooa másik parancsprogramot.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-501">When you inspect hello properties of hello Scripted Component, hello four different script types will be shown, each configurable tooa different script.</span></span>

![A parancsfájlalapú összetevő tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="2f7bc-503">*A parancsfájlalapú összetevő tulajdonságai*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-503">*Scripted Component properties*</span></span>

<span data-ttu-id="2f7bc-504">Törölje a hello processInputScript és hello realizeScript hello-szerkesztő megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-504">Clear hello processInputScript and open hello editor for hello realizeScript.</span></span> <span data-ttu-id="2f7bc-505">Most azt beállításokat, és kész a toostart parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-505">Now we're set up and ready toostart scripting.</span></span>

<span data-ttu-id="2f7bc-506">Parancsfájlok Groovy, dinamikusan lefordított parancsnyelv hello Java platform, amely megőrzi a kompatibilitást a Java nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-506">Scripts are written in Groovy, a dynamically compiled scripting language for hello Java platform that retains compatibility with Java.</span></span> <span data-ttu-id="2f7bc-507">A legtöbb Java-kóddal ténylegesen, érvényes Groovy kód.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-507">Actually, most Java code is valid Groovy code.</span></span>

<span data-ttu-id="2f7bc-508">Ideje lefuttatni egy egyszerű hello world groovy parancsfájl a realizeScript hello környezetében.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-508">Let's write a simple hello world groovy script in hello context of our realizeScript.</span></span> <span data-ttu-id="2f7bc-509">Írja be a hello következő hello szerkesztőben:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-509">Enter hello following in hello editor:</span></span>

    node.log("hello world");

<span data-ttu-id="2f7bc-510">Egy tesztcélú helyi Futtatás most végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-510">Now execute a local test run.</span></span> <span data-ttu-id="2f7bc-511">Ehhez a futtató után vizsgálja meg (hello rendszer lapját, amelyen keresztül hello összetevő parancsprogrammal létrehozva) hello naplók tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-511">After this run, inspect (through hello System tab on hello Scripted Component) hello Logs property.</span></span>

![Hello world kimenet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

<span data-ttu-id="2f7bc-513">*Hello world kimenet*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-513">*Hello world log output*</span></span>

<span data-ttu-id="2f7bc-514">hello csomóponti objektum hello napló metódus hívása, tooour aktuális "csomópont" vagy a jelenleg éppen scripting belül hello összetevő hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-514">hello node object we call hello log method on, refers tooour current "node" or hello component we're scripting within.</span></span> <span data-ttu-id="2f7bc-515">Minden összetevő használja, így azt hello képességét toooutput naplózási adatokat, hello lap keresztül érhető el. Ebben az esetben azt a kimeneti hello a literál "hello world" karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-515">Every component as such has hello ability toooutput logging data, available through hello system tab. In this case, we output hello string literal "hello world".</span></span> <span data-ttu-id="2f7bc-516">Itt fontos toounderstand, hogy ez bizonyítja toobe egy hasznos információt hibakereső eszköz, így a ismereteket milyen hello parancsfájl ténylegesen tesz a.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-516">Important toounderstand here is that this can prove toobe an invaluable debugging tool, providing you with insight on what hello script is actually doing.</span></span>

<span data-ttu-id="2f7bc-517">A belül a parancsfájl-kezelési környezet, azt is rendelkezik hozzáféréssel tooproperties más összetevők.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-517">From within our scripting environment, we also have access tooproperties on other components.</span></span> <span data-ttu-id="2f7bc-518">Próbálkozzon a következővel:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-518">Try this:</span></span>

    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up tooother nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

<span data-ttu-id="2f7bc-519">A napló ablakban jelennek meg velünk a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-519">Our log window will show us hello following:</span></span>

![Napló kimeneti elérési útjának eléréséhez](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

<span data-ttu-id="2f7bc-521">*Napló kimeneti elérési útjának eléréséhez*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-521">*Log output for accessing node paths*</span></span>

## <span data-ttu-id="2f7bc-522"><a id="frame_based_trim"></a>Keret-alapú levágási multibitrate MP4 kimenet</span><span class="sxs-lookup"><span data-stu-id="2f7bc-522"><a id="frame_based_trim"></a>Frame-based trimming of multibitrate MP4 output</span></span>
<span data-ttu-id="2f7bc-523">Amely hoz létre egy munkafolyamat-től kezdődő [egy multibitrate MP4 kimenete egy bemeneti MXF](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), azt most kell keresése a díszítésre hello forrás videó keret érintett alapján.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-523">Starting from a workflow that generates [a multibitrate MP4 output from an MXF input](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), we will now be looking into trimming hello source video based on frame counts.</span></span>

### <span data-ttu-id="2f7bc-524"><a id="frame_based_trim_start"></a>Tervezetének áttekintése toostart levágási történő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-524"><a id="frame_based_trim_start"></a>Blueprint overview toostart adding trimming to</span></span>
![Munkafolyamat toostart levágási történő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

<span data-ttu-id="2f7bc-526">*Munkafolyamat toostart levágási történő hozzáadása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-526">*Workflow toostart adding trimming to*</span></span>

### <span data-ttu-id="2f7bc-527"><a id="frame_based_trim_clip_list"></a>Hello klip lista XML használatával</span><span class="sxs-lookup"><span data-stu-id="2f7bc-527"><a id="frame_based_trim_clip_list"></a>Using hello Clip List XML</span></span>
<span data-ttu-id="2f7bc-528">Az összes korábbi munkafolyamat oktatóanyag a bemeneti videoforrást hello Media fájl bemeneti összetevő használja azt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-528">In all previous workflow tutorials, we used hello Media File Input component as our video input source.</span></span> <span data-ttu-id="2f7bc-529">Ebben a forgatókönyvben azonban használni fogjuk hello klip forráslista összetevő helyette.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-529">For this specific scenario though, we'll be using hello Clip List Source component instead.</span></span> <span data-ttu-id="2f7bc-530">Vegye figyelembe, hogy lehetőleg ne legyen működő; hello előnyben részesített módja csak akkor alkalmazza hello klip forráslista, ha egy valódi OK toodo úgy van (például az alábbi esetet, ahol azt hajt hello hello klip lista levágási képességek használatát).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-530">Note that this should not be hello preferred way of working; only use hello Clip List Source when there's a real reason toodo so (like in hello below case, where we're making use of hello clip list trimming capabilities).</span></span>

<span data-ttu-id="2f7bc-531">az adathordozó fájl bemeneti toohello klip forráslista, a tooswitch hello klip forráslista összetevő húzza hello a tervezési felülethez, és csatlakozzon a hello klip lista XML PIN-kód toohello klip lista XML-csomópont hello munkafolyamat-Tervező.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-531">tooswitch from our Media File Input toohello Clip List Source, drag hello Clip List Source component onto hello design surface and connect hello Clip List XML pin toohello Clip List XML node of hello workflow designer.</span></span> <span data-ttu-id="2f7bc-532">Ez kitölti hello klip lista forrása a kimeneti PIN-kód, tooour bemeneti videó alapján történik.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-532">This should populate hello Clip List Source with output pins, according tooour input video.</span></span> <span data-ttu-id="2f7bc-533">Most kapcsolódó hello tömörítetlen videóban és tömörítetlen hang PIN-kódok hello hello klip forráslista toohello megfelelő AVC kódolók és hang adatfolyam Interleaver.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-533">Now connect hello Uncompressed Video and Uncompressed Audio pins from hello hello Clip List Source toohello respective AVC Encoders and Audio Stream Interleaver.</span></span> <span data-ttu-id="2f7bc-534">Hello Media fájl bemeneti eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-534">Now remove hello Media File Input.</span></span>

![Hello Media fájl bemeneti helyére hello klip forráslista](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

<span data-ttu-id="2f7bc-536">*Hello Media fájl bemeneti helyére hello klip forráslista*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-536">*Replaced hello Media File Input with hello Clip List Source*</span></span>

<span data-ttu-id="2f7bc-537">hello klip forráslista összetevő "Klip lista XML" fogadja a bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-537">hello Clip List Source component takes as its input a "Clip List XML".</span></span> <span data-ttu-id="2f7bc-538">Lehetőséget választva hello forrás fájl tootest rendelkező helyi, a a klip lista XML-kódja, automatikus feltöltve meg.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-538">When selecting hello source file tootest with locally, this clip list xml is auto-populated for you.</span></span>

![Automatikus feltöltve klip lista XML-tulajdonság](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

<span data-ttu-id="2f7bc-540">*Automatikus feltöltve klip lista XML-tulajdonság*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-540">*Auto-populated Clip List XML property*</span></span>

<span data-ttu-id="2f7bc-541">Egy kicsit szorosabb toohello xml keresése, ez az hogyan hasonlóan néz ki:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-541">Looking a bit closer toohello xml, this is how it looks like:</span></span>

![Szerkesztése klip lista párbeszédpanel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

<span data-ttu-id="2f7bc-543">*Szerkesztése klip lista párbeszédpanel*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-543">*Edit clip list dialog*</span></span>

<span data-ttu-id="2f7bc-544">Ez azonban nem tükrözi hello klip lista xml hello képességeit.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-544">This however does not reflect hello capabilities of hello clip list xml.</span></span> <span data-ttu-id="2f7bc-545">Egy tudunk elem tooadd egy "Vágás" elem alatt hello hang- és hang forrás ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-545">One option we have is tooadd a "Trim" element under both hello video and audio source, like this:</span></span>

![A vágás toohello klip elemlista hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

<span data-ttu-id="2f7bc-547">*A vágás toohello klip elemlista hozzáadása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-547">*Adding a trim element toohello clip list*</span></span>

<span data-ttu-id="2f7bc-548">Ha módosítja a hello klip lista xml ilyen felett, és helyi ellenőrzéséhez futtassa, látni fogja a hello videó megfelelően lett rövidített hello videó 10 és 20 másodperc között.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-548">If you modify hello clip list xml like this above and perform a local test run, you'll see hello video correctly been trimmed between 10 and 20 seconds in hello video.</span></span>

<span data-ttu-id="2f7bc-549">Ezt megteheti a helyi futtatás, bár ez nagyon azonos cliplist xml nem rendelkezik azonos érvényesíti az Azure Media Services futó munkafolyamatot alkalmazásakor hello ellentétes toowhat történik.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-549">Contrary toowhat happens when you do a local run though, this very same cliplist xml would not have hello same effect when applied in a workflow that runs in Azure Media Services.</span></span> <span data-ttu-id="2f7bc-550">Ha Azure prémium szintű kódolás elindul, hello cliplist xml jön létre minden alkalommal, amikor újra, alapján hello bemeneti hello fájlkódolás feladat lett megadva.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-550">When Azure Premium Encoder starts, hello cliplist xml is generated every time again, based on hello input file hello encoding job was given.</span></span> <span data-ttu-id="2f7bc-551">Ez azt jelenti, hogy módosításokat hello XML-végezzük volna sajnos bírálható felül.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-551">This means that any changes we do on hello xml would unfortunately be overridden.</span></span>

<span data-ttu-id="2f7bc-552">a kódolási feladat indításakor adatainak törlése toocounter hello cliplist xml azt is létre újból azt a hello menet közben a munkafolyamat hello elindítása után.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-552">toocounter hello cliplist xml being wiped when an encoding job is started, we can re-generate it on hello fly just after hello start of our workflow.</span></span> <span data-ttu-id="2f7bc-553">Ilyen egyéni műveletek lehessen állítani a "Component parancsprogrammal létrehozva" úgynevezett keresztül.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-553">Such custom actions can be taken through what is called a "Scripted Component".</span></span> <span data-ttu-id="2f7bc-554">További információkért lásd: [Introducing hello parancsprogrammal létrehozva összetevő](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span><span class="sxs-lookup"><span data-stu-id="2f7bc-554">For more information, see [Introducing hello Scripted Component](media-services-media-encoder-premium-workflow-tutorials.md#scripting).</span></span>

<span data-ttu-id="2f7bc-555">Húzzon egy parancsprogrammal létrehozva összetevő hello Tervező felületére, és nevezze át túl "SetClipListXML".</span><span class="sxs-lookup"><span data-stu-id="2f7bc-555">Drag a Scripted Component onto hello designer surface and rename it too"SetClipListXML".</span></span>

![A parancsfájlalapú összetevő hozzáadása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

<span data-ttu-id="2f7bc-557">*A parancsfájlalapú összetevő hozzáadása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-557">*Adding a Scripted Component*</span></span>

<span data-ttu-id="2f7bc-558">Amikor hello tulajdonságainak nézze meg hello összetevő parancsprogrammal létrehozva, hello négy különböző parancsfájl típusok megjelenik, minden konfigurálható tooa másik parancsprogramot.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-558">When you inspect hello properties of hello Scripted Component, hello four different script types will be shown, each configurable tooa different script.</span></span>

![A parancsfájlalapú összetevő tulajdonságai](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

<span data-ttu-id="2f7bc-560">*A parancsfájlalapú összetevő tulajdonságai*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-560">*Scripted Component properties*</span></span>

### <span data-ttu-id="2f7bc-561"><a id="frame_based_trim_modify_clip_list"></a>Egy parancsprogram összetevő hello klip listájának módosítása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-561"><a id="frame_based_trim_modify_clip_list"></a>Modifying hello clip list from a Scripted Component</span></span>
<span data-ttu-id="2f7bc-562">Mielőtt azt újra írhatna hello cliplist xml munkafolyamat indítása során létrehozott, szükség lesz toohave hozzáférés toohello cliplist xml tulajdonságot és tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-562">Before we can re-write hello cliplist xml that is generated during workflow startup, we'll need toohave access toohello cliplist xml property and contents.</span></span> <span data-ttu-id="2f7bc-563">Ehhez hasonló azt is megteheti:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-563">We can do so like this:</span></span>

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![A naplózott bejövő klip listája](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

<span data-ttu-id="2f7bc-565">*A naplózott bejövő klip listája*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-565">*Incoming clip list being logged*</span></span>

<span data-ttu-id="2f7bc-566">Először igazolnia kell a módon toodetermine vége: amikor tootrim azt szeretnénk, mely pontról hello videó.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-566">First we need a way toodetermine from which point till which point we want tootrim hello video.</span></span> <span data-ttu-id="2f7bc-567">toomake Ez kényelmes toohello kevésbé-technikai felhasználói hello munkafolyamat közzététele hello graph két tulajdonságok toohello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-567">toomake this convenient toohello less-technical user of hello workflow, publish two properties toohello root of hello graph.</span></span> <span data-ttu-id="2f7bc-568">toodo, kattintson jobb gombbal a Tervező felületére hello és "Tulajdonság hozzáadása" Válasszon:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-568">toodo this, right click hello designer surface and select "Add Property":</span></span>

* <span data-ttu-id="2f7bc-569">Első tulajdonság: "ClippingTimeStart" típusú: "IDŐKÓD"</span><span class="sxs-lookup"><span data-stu-id="2f7bc-569">First property: "ClippingTimeStart" of type: "TIMECODE"</span></span>
* <span data-ttu-id="2f7bc-570">A második tulajdonság: "ClippingTimeEnd" típusú: "IDŐKÓD"</span><span class="sxs-lookup"><span data-stu-id="2f7bc-570">Second property: "ClippingTimeEnd" of type: "TIMECODE"</span></span>

![Hozzáadása tulajdonsághoz párbeszédpanel Kivágás kezdési ideje](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

<span data-ttu-id="2f7bc-572">*Hozzáadása tulajdonsághoz párbeszédpanel Kivágás kezdési ideje*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-572">*Add Property dialog for clipping start time*</span></span>

![Közzétett a munkafolyamat legfelső szintű idő tulajdonságai Kivágás](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

<span data-ttu-id="2f7bc-574">*Közzétett a munkafolyamat legfelső szintű idő tulajdonságai Kivágás*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-574">*Published clipping time props on workflow root*</span></span>

<span data-ttu-id="2f7bc-575">Adja meg mindkét tulajdonságok tooa megfelelő értéket:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-575">Configure both properties tooa suitable value:</span></span>

![Kivágási, kezdő és záró tulajdonságok hello konfigurálása](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

<span data-ttu-id="2f7bc-577">*Kivágási, kezdő és záró tulajdonságok hello konfigurálása*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-577">*Configure hello clipping start and end properties*</span></span>

<span data-ttu-id="2f7bc-578">Most a belül a parancsfájl azt férhetnek hozzá mindkét tulajdonságok ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-578">Now, from within our script, we can access both properties, like this:</span></span>

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Kezdő és záró a Kivágás tartalmazó napló ablak](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

<span data-ttu-id="2f7bc-580">*Kezdő és záró a Kivágás tartalmazó napló ablak*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-580">*Log window showing start and end of clipping*</span></span>

<span data-ttu-id="2f7bc-581">Most elemezni hello időkód karakterláncok formába kényelmesebb toouse, egy egyszerű reguláris kifejezés használatával:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-581">Let's parse hello timecode strings into a more convenient toouse form, using a simple regular expression:</span></span>

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);

![Napló ablakban az elemzett időkód kimenete](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

<span data-ttu-id="2f7bc-583">*Napló ablakban az elemzett időkód kimenete*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-583">*Log window with output of parsed timecode*</span></span>

<span data-ttu-id="2f7bc-584">Az információ az elvégzendő azt mostantól módosíthatja a hello cliplist xml tooreflect hello kezdő, és befejezési idejének hello hello film keret pontos Kivágás szükséges.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-584">With this information at hand, we can now modify hello cliplist xml tooreflect hello start and end times for hello desired frame-accurate clipping of hello movie.</span></span>

![A parancsfájlok kód tooadd vágás elemei](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

<span data-ttu-id="2f7bc-586">*A parancsfájlok kód tooadd vágás elemei*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-586">*Script code tooadd trim elements*</span></span>

<span data-ttu-id="2f7bc-587">Ez normál karakterlánc fájlkezelési műveleteket keresztül végezhető el.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-587">This was done through normal string manipulation operations.</span></span> <span data-ttu-id="2f7bc-588">hello eredményül kapott módosított klip lista xml írása vissza toohello clipListXML tulajdonság hello munkafolyamat legfelső szintű hello "setProperty" metódussal.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-588">hello resulting modified clip list xml is written back toohello clipListXML property on hello workflow root through hello "setProperty" method.</span></span> <span data-ttu-id="2f7bc-589">hello napló ablakban egy másik teszt futtatása után szeretné megjelenítése velünk a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-589">hello log window after another test run would show us hello following:</span></span>

![Naplózási a klip listájában hello](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

<span data-ttu-id="2f7bc-591">*Naplózási a klip listájában hello*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-591">*Logging hello resulting clip list*</span></span>

<span data-ttu-id="2f7bc-592">Hajtsa végre a vizsgálat toosee, hogyan lettek hello video- és adatfolyamok levágva.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-592">Do a test-run toosee how hello video and audio streams have been clipped.</span></span> <span data-ttu-id="2f7bc-593">Módon teheti meg egynél több vizsgálat hello levágási pontok különböző értékekkel, láthatja, hogy a rendszer nem figyelembe kell venni azonban!</span><span class="sxs-lookup"><span data-stu-id="2f7bc-593">As you'll do more than one test-run with different values for hello trimming points, you'll notice that those will not be taken into account however!</span></span> <span data-ttu-id="2f7bc-594">hello ennek az oka, hogy hello Tervező hello Azure futásidejű eltérően, does nem felülbírálás hello cliplist xml minden futtatás.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-594">hello reason for this is that hello designer, unlike hello Azure runtime, does NOT override hello cliplist xml every run.</span></span> <span data-ttu-id="2f7bc-595">Ez azt jelenti, hogy csak hello állított hello első alkalommal bejövő és kimenő adatforgalma pontok, akkor hello xml tootransform, az összes hello más időpontokban, a őr záradék (Ha (clipListXML.indexOf ("<trim>") == -1)) megakadályozza, hogy a hello munkafolyamat vágás hozzáadása Ha már létezik egy elem.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-595">This means that only hello very first time you have set hello in and out points, will cause hello xml tootransform, all hello other times, our guard clause (if(clipListXML.indexOf("<trim>") == -1)) will prevent hello workflow from adding another trim element when there's already one present.</span></span>

<span data-ttu-id="2f7bc-596">toomake a munkafolyamat kényelmes tootest helyileg, a Microsoft ajánlott hozzáadása néhány house-karbantartási kódot, amely megvizsgálja, ha a vágás elem már található.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-596">toomake our workflow convenient tootest locally, we best add some house-keeping code that inspects if a trim element was already present.</span></span> <span data-ttu-id="2f7bc-597">Ha igen, azt eltávolítja azt hello xml hello új értékekkel módosításával a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-597">If so, we can remove it before continuing by modifying hello xml with hello new values.</span></span> <span data-ttu-id="2f7bc-598">Ahelyett, hogy az egyszerű karakterlánc-feldolgozás, akkor valószínűleg biztonságosabb toodo elemzési modell ez valós xml-objektumon keresztül.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-598">Rather than using plain string manipulations, it's probably safer toodo this through real xml object model parsing.</span></span>

<span data-ttu-id="2f7bc-599">Ahhoz azonban vehessen fel ilyen kód, kell hello importálási utasításokat számos indítsa el a parancsfájl először tooadd:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-599">Before we can add such code though, we'll need tooadd a number of import statements at hello start of our script first:</span></span>

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

<span data-ttu-id="2f7bc-600">Ezt követően hozzá lehessen adni hello tisztítás kód szükséges:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-600">After this, we can add hello required cleaning code:</span></span>

    //for local testing: delete any pre-existing trim elements from hello clip list xml by parsing hello xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
     //delete hello trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

<span data-ttu-id="2f7bc-601">Ez a kód fölött, amellyel jelenleg felvenni hello vágás elemek toohello cliplist xml hello pont kerül.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-601">This code goes just above hello point at which we add hello trim elements toohello cliplist xml.</span></span>

<span data-ttu-id="2f7bc-602">Ezen a ponton azt futtathatja és módosíthatja a munkafolyamat, mint azt szeretnénk, ha valaha is alkalmazott hello módosítások mellett idő.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-602">At this point, we can run and modify our workflow as much as we want while having hello changes applied ever time.</span></span>    

### <span data-ttu-id="2f7bc-603"><a id="frame_based_trim_clippingenabled_prop"></a>Egy ClippingEnabled kényelmi tulajdonság hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-603"><a id="frame_based_trim_clippingenabled_prop"></a>Adding a ClippingEnabled convenience property</span></span>
<span data-ttu-id="2f7bc-604">Előfordulhat, hogy nem mindig kívánt levágási toohappen, most Befejezés ki a munkafolyamat hozzáadásával egy kényelmes logikai jelző, amely azt jelzi-e azt szeretnénk, hogy tooenable díszítésre / kivágást.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-604">As you might not always want trimming toohappen, let's finish off our workflow by adding a convenient boolean flag that indicates whether or not we want tooenable trimming / clipping.</span></span>

<span data-ttu-id="2f7bc-605">Ahogy előtt, tegye közzé a munkafolyamat "ClippingEnabled" nevű új tulajdonság toohello gyökér "Logikai" típusúnak.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-605">Just as before, publish a new property toohello root of our workflow called "ClippingEnabled" of type "BOOLEAN".</span></span>

![Közzétett egy tulajdonság Kivágás engedélyezése](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

<span data-ttu-id="2f7bc-607">*Közzétett egy tulajdonság Kivágás engedélyezése*</span><span class="sxs-lookup"><span data-stu-id="2f7bc-607">*Published a property for enabling clipping*</span></span>

<span data-ttu-id="2f7bc-608">Az alábbi egyszerű őr záradék hello azt ellenőrizze, hogy a tisztítás szükség, és döntse el, hogy a klip listáját használja, így kell-e a módosítása, illetve nem toobe.</span><span class="sxs-lookup"><span data-stu-id="2f7bc-608">With hello below simple guard clause, we can check if trimming is required and decide if our clip list as such needs toobe modified or not.</span></span>

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <span data-ttu-id="2f7bc-609"><a id="code"></a>Teljes kód</span><span class="sxs-lookup"><span data-stu-id="2f7bc-609"><a id="code"></a>Complete code</span></span>
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from hello clip list xml by parsing hello xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
    //delete hello trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements toocliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }


## <a name="also-see"></a><span data-ttu-id="2f7bc-610">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2f7bc-610">Also see</span></span>
[<span data-ttu-id="2f7bc-611">Prémium szintű kódolás az Azure Media Services bemutatása</span><span class="sxs-lookup"><span data-stu-id="2f7bc-611">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[<span data-ttu-id="2f7bc-612">Hogyan prémium szintű Azure Media Services kódolási tooUse</span><span class="sxs-lookup"><span data-stu-id="2f7bc-612">How tooUse Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[<span data-ttu-id="2f7bc-613">Az Azure Media Services kódolási igény tartalom</span><span class="sxs-lookup"><span data-stu-id="2f7bc-613">Encoding On-Demand Content with Azure Media Service</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

[<span data-ttu-id="2f7bc-614">A Media Encoder Premium munkafolyamat formátumai és kodekei</span><span class="sxs-lookup"><span data-stu-id="2f7bc-614">Media Encoder Premium Workflow Formats and Codecs</span></span>](media-services-premium-workflow-encoder-formats.md)

[<span data-ttu-id="2f7bc-615">Minta munkafolyamat-fájlok</span><span class="sxs-lookup"><span data-stu-id="2f7bc-615">Sample workflow files</span></span>](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[<span data-ttu-id="2f7bc-616">Azure Media Services Explorer eszköz</span><span class="sxs-lookup"><span data-stu-id="2f7bc-616">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="2f7bc-617">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="2f7bc-617">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2f7bc-618">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="2f7bc-618">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

---
title: az Azure Media Analytics aaaDetect mozdulatok |} Microsoft Docs
description: "hello Azure Media mozgásérzékelő media processzor (MP) lehetővé teszi, hogy Ön tooefficiently azonosítani egy egyébként hosszú és Eseménytelen video házirendsablonokkal szakaszait."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: d144f813-1a55-442f-a895-5c4cb6d0aeae
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: milanga;juliako;
ms.openlocfilehash: cb431375c92222053ed2239dd4e45767524dab68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a><span data-ttu-id="26635-103">Az Azure Media Analytics mozdulatok észlelése</span><span class="sxs-lookup"><span data-stu-id="26635-103">Detect Motions with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="26635-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="26635-104">Overview</span></span>
<span data-ttu-id="26635-105">Hello **Azure Media mozgásérzékelő** media processzor (MP) lehetővé teszi, hogy Ön tooefficiently azonosítani egy egyébként hosszú és Eseménytelen video házirendsablonokkal szakaszait.</span><span class="sxs-lookup"><span data-stu-id="26635-105">hello **Azure Media Motion Detector** media processor (MP) enables you tooefficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="26635-106">Mozgásérzékelés statikus kamera felvételei tooidentify szakaszok hello videó hol mozgásérzékelési jelentkezik is használhatók.</span><span class="sxs-lookup"><span data-stu-id="26635-106">Motion detection can be used on static camera footage tooidentify sections of hello video where motion occurs.</span></span> <span data-ttu-id="26635-107">A metaadatok időbélyegeket és a régió, ahol hello esemény történt határolókeret hello tartalmazó JSON-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="26635-107">It generates a JSON file containing a metadata with timestamps and hello bounding region where hello event occurred.</span></span>

<span data-ttu-id="26635-108">Cél biztonsági videó hírcsatornák, ez a technológia képes toocategorize mozgásérzékelési kapcsolódó eseményeket és vakriasztások például árnyékok és megvilágítási módosításokat.</span><span class="sxs-lookup"><span data-stu-id="26635-108">Targeted towards security video feeds, this technology is able toocategorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="26635-109">Ez lehetővé teszi a kamera hírcsatornák toogenerate biztonsági riasztások alatt címünkre végtelen irreleváns események, ugyanakkor nem képes tooextract perc múlva a rendkívül hosszú felügyeleti videók érdeklő nélkül.</span><span class="sxs-lookup"><span data-stu-id="26635-109">This allows you toogenerate security alerts from camera feeds without being spammed with endless irrelevant events, while being able tooextract moments of interest from extremely long surveillance videos.</span></span>

<span data-ttu-id="26635-110">Hello **Azure Media mozgásérzékelő** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="26635-110">hello **Azure Media Motion Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="26635-111">Ez a témakör kapcsolatos részleteket nyújt **Azure Media mozgásérzékelő** és bemutatja, hogyan toouse a Media Services SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="26635-111">This topic gives details about  **Azure Media Motion Detector** and shows how toouse it with Media Services SDK for .NET</span></span>

## <a name="motion-detector-input-files"></a><span data-ttu-id="26635-112">Mozgásérzékelő – érzékelő bemeneti fájlok</span><span class="sxs-lookup"><span data-stu-id="26635-112">Motion Detector input files</span></span>
<span data-ttu-id="26635-113">Videofájlok lejátszását.</span><span class="sxs-lookup"><span data-stu-id="26635-113">Video files.</span></span> <span data-ttu-id="26635-114">Jelenleg a következő formátumok hello támogatottak: MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="26635-114">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration-preset"></a><span data-ttu-id="26635-115">A feladat konfigurációja (beállítás)</span><span class="sxs-lookup"><span data-stu-id="26635-115">Task configuration (preset)</span></span>
<span data-ttu-id="26635-116">A feladat létrehozásakor **Azure Media mozgásérzékelő**, meg kell adnia egy konfigurációs készletet.</span><span class="sxs-lookup"><span data-stu-id="26635-116">When creating a task with **Azure Media Motion Detector**, you must specify a configuration preset.</span></span> 

### <a name="parameters"></a><span data-ttu-id="26635-117">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="26635-117">Parameters</span></span>
<span data-ttu-id="26635-118">A következő paraméterek hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="26635-118">You can use hello following parameters:</span></span>

| <span data-ttu-id="26635-119">Név</span><span class="sxs-lookup"><span data-stu-id="26635-119">Name</span></span> | <span data-ttu-id="26635-120">Beállítások</span><span class="sxs-lookup"><span data-stu-id="26635-120">Options</span></span> | <span data-ttu-id="26635-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="26635-121">Description</span></span> | <span data-ttu-id="26635-122">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="26635-122">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="26635-123">sensitivityLevel</span><span class="sxs-lookup"><span data-stu-id="26635-123">sensitivityLevel</span></span> |<span data-ttu-id="26635-124">Karakterlánc: "alacsony", "közepes", "magas"</span><span class="sxs-lookup"><span data-stu-id="26635-124">String:'low', 'medium', 'high'</span></span> |<span data-ttu-id="26635-125">Beállítja hello bizalmassági szint mely mozdulatok jelenti.</span><span class="sxs-lookup"><span data-stu-id="26635-125">Sets hello sensitivity level at which motions is reported.</span></span> <span data-ttu-id="26635-126">Állítsa be úgy a vakriasztások tooadjust mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="26635-126">Adjust this tooadjust amount of false positives.</span></span> |<span data-ttu-id="26635-127">"közepes"</span><span class="sxs-lookup"><span data-stu-id="26635-127">'medium'</span></span> |
| <span data-ttu-id="26635-128">frameSamplingValue</span><span class="sxs-lookup"><span data-stu-id="26635-128">frameSamplingValue</span></span> |<span data-ttu-id="26635-129">Pozitív egész szám</span><span class="sxs-lookup"><span data-stu-id="26635-129">Positive integer</span></span> |<span data-ttu-id="26635-130">Beállítja hello gyakoriság mely algoritmus futtatása.</span><span class="sxs-lookup"><span data-stu-id="26635-130">Sets hello frequency at which algorithm runs.</span></span> <span data-ttu-id="26635-131">1 egyenlő minden keret, 2 azt jelenti, hogy minden 2. keret, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="26635-131">1 equals every frame, 2 means every 2nd frame, and so on.</span></span> |<span data-ttu-id="26635-132">1</span><span class="sxs-lookup"><span data-stu-id="26635-132">1</span></span> |
| <span data-ttu-id="26635-133">detectLightChange</span><span class="sxs-lookup"><span data-stu-id="26635-133">detectLightChange</span></span> |<span data-ttu-id="26635-134">Logikai: "true", "false"</span><span class="sxs-lookup"><span data-stu-id="26635-134">Boolean:'true', 'false'</span></span> |<span data-ttu-id="26635-135">Megadja, hogy e könnyű módosítások kimutatott hello eredmények</span><span class="sxs-lookup"><span data-stu-id="26635-135">Sets whether light changes are reported in hello results</span></span> |<span data-ttu-id="26635-136">"False"</span><span class="sxs-lookup"><span data-stu-id="26635-136">'False'</span></span> |
| <span data-ttu-id="26635-137">mergeTimeThreshold</span><span class="sxs-lookup"><span data-stu-id="26635-137">mergeTimeThreshold</span></span> |<span data-ttu-id="26635-138">Idő xs: Óó: pp:</span><span class="sxs-lookup"><span data-stu-id="26635-138">Xs-time: Hh:mm:ss</span></span><br/><span data-ttu-id="26635-139">. Példa: 00:00:03</span><span class="sxs-lookup"><span data-stu-id="26635-139">Example: 00:00:03</span></span> |<span data-ttu-id="26635-140">Megadja, ahol 2 esemény lesz kombinált és 1 jelentett mozgásérzékelési események között hello időkerete.</span><span class="sxs-lookup"><span data-stu-id="26635-140">Specifies hello time window between motion events where 2 events will be combined and reported as 1.</span></span> |<span data-ttu-id="26635-141">00:00:00</span><span class="sxs-lookup"><span data-stu-id="26635-141">00:00:00</span></span> |
| <span data-ttu-id="26635-142">detectionZones</span><span class="sxs-lookup"><span data-stu-id="26635-142">detectionZones</span></span> |<span data-ttu-id="26635-143">A tömb észlelési zónák:</span><span class="sxs-lookup"><span data-stu-id="26635-143">An array of detection zones:</span></span><br/><span data-ttu-id="26635-144">-Észlelési zóna: 3 vagy több pontok bájttömb</span><span class="sxs-lookup"><span data-stu-id="26635-144">- Detection Zone is an array of 3 or more points</span></span><br/><span data-ttu-id="26635-145">-Pont egy x és y koordináta a 0 too1.</span><span class="sxs-lookup"><span data-stu-id="26635-145">- Point is a x and y coordinate from 0 too1.</span></span> |<span data-ttu-id="26635-146">Ismerteti a sokszög észlelési zónák toobe használt hello listája.</span><span class="sxs-lookup"><span data-stu-id="26635-146">Describes hello list of polygonal detection zones toobe used.</span></span><br/><span data-ttu-id="26635-147">Az ID, az első egy lesznek "id" hello hello zónák lesz jelentve az eredmények: 0</span><span class="sxs-lookup"><span data-stu-id="26635-147">Results will be reported with hello zones as an ID, with hello first one being 'id':0</span></span> |<span data-ttu-id="26635-148">Egy zóna, amely magában foglalja a teljes keret hello.</span><span class="sxs-lookup"><span data-stu-id="26635-148">Single zone which covers hello entire frame.</span></span> |

### <a name="json-example"></a><span data-ttu-id="26635-149">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="26635-149">JSON example</span></span>
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }


## <a name="motion-detector-output-files"></a><span data-ttu-id="26635-150">Mozgásérzékelő – érzékelő kimeneti fájlok</span><span class="sxs-lookup"><span data-stu-id="26635-150">Motion Detector output files</span></span>
<span data-ttu-id="26635-151">A mozgás észlelése hello kimeneti eszköz, amely hello mozgásérzékelési riasztások és azok kategóriák belül hello videó ismerteti adja vissza egy JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="26635-151">A motion detection job will return a JSON file in hello output asset which describes hello motion alerts, and their categories, within hello video.</span></span> <span data-ttu-id="26635-152">hello fájl hello időpontja és időtartama található videó hello mozgási információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="26635-152">hello file will contain information about hello time and duration of motion detected in hello video.</span></span>

<span data-ttu-id="26635-153">hello Mozgásérzékelési érzékelő API mutatók biztosít, ha nincsenek objektumok mozgásérzékelési rögzített háttér video (pl. egy felügyeleti videó).</span><span class="sxs-lookup"><span data-stu-id="26635-153">hello Motion Detector API provides indicators once there are objects in motion in a fixed background video (e.g. a surveillance video).</span></span> <span data-ttu-id="26635-154">Mozgásérzékelő hello képzett tooreduce téves riasztásokat, például a megvilágítási és árnyékmásolat módosítások.</span><span class="sxs-lookup"><span data-stu-id="26635-154">hello Motion Detector is trained tooreduce false alarms, such as lighting and shadow changes.</span></span> <span data-ttu-id="26635-155">Aktuális korlátozások hello algoritmusok éjszakai stratégiai videók, félig átlátható és kis objektumok tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="26635-155">Current limitations of hello algorithms include night vision videos, semi-transparent objects, and small objects.</span></span>

### <span data-ttu-id="26635-156"><a id="output_elements"></a>Hello kimeneti JSON-fájl elemeinek</span><span class="sxs-lookup"><span data-stu-id="26635-156"><a id="output_elements"></a>Elements of hello output JSON file</span></span>
> [!NOTE]
> <span data-ttu-id="26635-157">Hello a legfrissebb verzióban hello kimeneti JSON formátumban megváltozott, és az egyes ügyfelek használhatatlanná tévő változást jelöl.</span><span class="sxs-lookup"><span data-stu-id="26635-157">In hello latest release, hello Output JSON format has changed and may represent a breaking change for some customers.</span></span>
> 
> 

<span data-ttu-id="26635-158">hello következő táblázatban hello kimeneti JSON-fájl elemeinek.</span><span class="sxs-lookup"><span data-stu-id="26635-158">hello following table describes elements of hello output JSON file.</span></span>

| <span data-ttu-id="26635-159">Elem</span><span class="sxs-lookup"><span data-stu-id="26635-159">Element</span></span> | <span data-ttu-id="26635-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="26635-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="26635-161">Verzió</span><span class="sxs-lookup"><span data-stu-id="26635-161">Version</span></span> |<span data-ttu-id="26635-162">Hello videó API verziója toohello utal.</span><span class="sxs-lookup"><span data-stu-id="26635-162">This refers toohello version of hello Video API.</span></span> <span data-ttu-id="26635-163">hello jelenlegi verzió: 2.</span><span class="sxs-lookup"><span data-stu-id="26635-163">hello current version is 2.</span></span> |
| <span data-ttu-id="26635-164">időskálára</span><span class="sxs-lookup"><span data-stu-id="26635-164">Timescale</span></span> |<span data-ttu-id="26635-165">"Ticks" hello videó másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="26635-165">"Ticks" per second of hello video.</span></span> |
| <span data-ttu-id="26635-166">Eltolás</span><span class="sxs-lookup"><span data-stu-id="26635-166">Offset</span></span> |<span data-ttu-id="26635-167">a "ticks" időbélyegeket hello időeltolódás.</span><span class="sxs-lookup"><span data-stu-id="26635-167">hello time offset for timestamps in "ticks".</span></span> <span data-ttu-id="26635-168">Videó API-k 1.0-s verziójában az mindig 0 lesz.</span><span class="sxs-lookup"><span data-stu-id="26635-168">In version 1.0 of Video APIs, this will always be 0.</span></span> <span data-ttu-id="26635-169">A jövőben támogatott forgatókönyveket, ezt az értéket módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="26635-169">In future scenarios we support, this value may change.</span></span> |
| <span data-ttu-id="26635-170">képkockasebességhez</span><span class="sxs-lookup"><span data-stu-id="26635-170">Framerate</span></span> |<span data-ttu-id="26635-171">Videó hello képkockasebessége.</span><span class="sxs-lookup"><span data-stu-id="26635-171">Frames per second of hello video.</span></span> |
| <span data-ttu-id="26635-172">Szélesség, Hosszúság</span><span class="sxs-lookup"><span data-stu-id="26635-172">Width, Height</span></span> |<span data-ttu-id="26635-173">Toohello szélessége és magassága képpontban videó hello hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="26635-173">Refers toohello width and height of hello video in pixels.</span></span> |
| <span data-ttu-id="26635-174">Indítás</span><span class="sxs-lookup"><span data-stu-id="26635-174">Start</span></span> |<span data-ttu-id="26635-175">hello indítsa el a "ticks" időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="26635-175">hello start timestamp in "ticks".</span></span> |
| <span data-ttu-id="26635-176">Időtartam</span><span class="sxs-lookup"><span data-stu-id="26635-176">Duration</span></span> |<span data-ttu-id="26635-177">hello esemény, a "ticks" Hello hossza.</span><span class="sxs-lookup"><span data-stu-id="26635-177">hello length of hello event, in "ticks".</span></span> |
| <span data-ttu-id="26635-178">időköz</span><span class="sxs-lookup"><span data-stu-id="26635-178">Interval</span></span> |<span data-ttu-id="26635-179">minden egyes bejegyzés hello esemény, a "ticks" Hello időközt.</span><span class="sxs-lookup"><span data-stu-id="26635-179">hello interval of each entry in hello event, in "ticks".</span></span> |
| <span data-ttu-id="26635-180">Események</span><span class="sxs-lookup"><span data-stu-id="26635-180">Events</span></span> |<span data-ttu-id="26635-181">Minden esemény-töredéket adott időtartamig belül észlelt hello mozgásérzékelési tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="26635-181">Each event fragment contains hello motion detected within that time duration.</span></span> |
| <span data-ttu-id="26635-182">Típus</span><span class="sxs-lookup"><span data-stu-id="26635-182">Type</span></span> |<span data-ttu-id="26635-183">Az aktuális verzióban hello Ez a "2" általános mozgásérzékelési.</span><span class="sxs-lookup"><span data-stu-id="26635-183">In hello current version, this is always ‘2’ for generic motion.</span></span> <span data-ttu-id="26635-184">Ez a címke videó API-k hello rugalmasságot toocategorize mozgásérzékelő – adja meg a jövőbeli verziókkal.</span><span class="sxs-lookup"><span data-stu-id="26635-184">This label gives Video APIs hello flexibility toocategorize motion in future versions.</span></span> |
| <span data-ttu-id="26635-185">RegionID</span><span class="sxs-lookup"><span data-stu-id="26635-185">RegionID</span></span> |<span data-ttu-id="26635-186">Ahogy fent, az mindig 0 lesz ebben a verzióban.</span><span class="sxs-lookup"><span data-stu-id="26635-186">As explained above, this will always be 0 in this version.</span></span> <span data-ttu-id="26635-187">Ezt a címkét ad Video API hello rugalmasságot toofind mozgásérzékelési különböző régiókban futó későbbi verzióiban.</span><span class="sxs-lookup"><span data-stu-id="26635-187">This label gives Video API hello flexibility toofind motion in various regions in future versions.</span></span> |
| <span data-ttu-id="26635-188">Régiók</span><span class="sxs-lookup"><span data-stu-id="26635-188">Regions</span></span> |<span data-ttu-id="26635-189">Toohello terület a videó ahol az Ön számára legfontosabb mozgásérzékelési hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="26635-189">Refers toohello area in your video where you care about motion.</span></span> <br/><br/><span data-ttu-id="26635-190">-az "id" jelöli hello régió terület – ebben a verzióban van csak egy, azonosító: 0.</span><span class="sxs-lookup"><span data-stu-id="26635-190">-"id" represents hello region area – in this version there is only one, ID 0.</span></span> <br/><span data-ttu-id="26635-191">-"type" az Ön számára legfontosabb a mozgásérzékelő – hello régió hello alakzat jelöli.</span><span class="sxs-lookup"><span data-stu-id="26635-191">-"type" represents hello shape of hello region you care about for motion.</span></span> <span data-ttu-id="26635-192">Jelenleg a "téglalap", "sokszög" támogatottak.</span><span class="sxs-lookup"><span data-stu-id="26635-192">Currently, "rectangle" and "polygon" are supported.</span></span><br/> <span data-ttu-id="26635-193">Ha a megadott "téglalap" hello régió dimenziója van X, Y, szélességét és magasságát.</span><span class="sxs-lookup"><span data-stu-id="26635-193">If you specified "rectangle", hello region has dimensions in X, Y, Width, and Height.</span></span> <span data-ttu-id="26635-194">hello X és Y koordinátáit képviselő hello felső bal oldali Oxykiszolgáló koordinátáit 0.0 too1.0 normalizált skálája hello-régiót.</span><span class="sxs-lookup"><span data-stu-id="26635-194">hello X and Y coordinates represent hello upper left hand XY coordinates of hello region in a normalized scale of 0.0 too1.0.</span></span> <span data-ttu-id="26635-195">hello szélességének és magasságának képviselő hello terület a 0.0 too1.0 normalizált skálája hello méretét.</span><span class="sxs-lookup"><span data-stu-id="26635-195">hello width and height represent hello size of hello region in a normalized scale of 0.0 too1.0.</span></span> <span data-ttu-id="26635-196">Hello aktuális verzió X, Y, szélességének és magasságának mindig rögzített 0, 0 és 1, 1.</span><span class="sxs-lookup"><span data-stu-id="26635-196">In hello current version, X, Y, Width, and Height are always fixed at 0, 0 and 1, 1.</span></span> <br/><span data-ttu-id="26635-197">Ha a megadott "sokszög" hello régió pontokon dimenziója van.</span><span class="sxs-lookup"><span data-stu-id="26635-197">If you specified "polygon", hello region has dimensions in points.</span></span> <br/> |
| <span data-ttu-id="26635-198">töredék</span><span class="sxs-lookup"><span data-stu-id="26635-198">Fragments</span></span> |<span data-ttu-id="26635-199">hello metaadatok töredék nevű különböző részekre van darabolásos fel.</span><span class="sxs-lookup"><span data-stu-id="26635-199">hello metadata is chunked up into different segments called fragments.</span></span> <span data-ttu-id="26635-200">Minden töredéke a start, a időtartama, a időszakának száma és a esemény (eke) tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="26635-200">Each fragment contains a start, duration, interval number, and event(s).</span></span> <span data-ttu-id="26635-201">Nincsenek események a töredéket azt jelenti, hogy semmilyen adott kezdő időpontja és időtartama során észlelt.</span><span class="sxs-lookup"><span data-stu-id="26635-201">A fragment with no events means that no motion was detected during that start time and duration.</span></span> |
| <span data-ttu-id="26635-202">Szögletes zárójelbe]</span><span class="sxs-lookup"><span data-stu-id="26635-202">Brackets []</span></span> |<span data-ttu-id="26635-203">Minden egyes zárójel hello esemény egy időköz jelöli.</span><span class="sxs-lookup"><span data-stu-id="26635-203">Each bracket represents one interval in hello event.</span></span> <span data-ttu-id="26635-204">Üres szögletes az adott időköz azt jelenti, hogy semmilyen volt észlelhető.</span><span class="sxs-lookup"><span data-stu-id="26635-204">Empty brackets for that interval means that no motion was detected.</span></span> |
| <span data-ttu-id="26635-205">Helyek</span><span class="sxs-lookup"><span data-stu-id="26635-205">locations</span></span> |<span data-ttu-id="26635-206">Az új bejegyzést események hello helyre, ahol hello mozgásérzékelési történt sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="26635-206">This new entry under events lists hello location where hello motion occurred.</span></span> <span data-ttu-id="26635-207">Ez a pontosabb, mint hello észlelési zónák.</span><span class="sxs-lookup"><span data-stu-id="26635-207">This is more specific than hello detection zones.</span></span> |

<span data-ttu-id="26635-208">hello az alábbiakban látható egy JSON kimeneti példa</span><span class="sxs-lookup"><span data-stu-id="26635-208">hello following is a JSON output example</span></span>

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],

    …
## <a name="limitations"></a><span data-ttu-id="26635-209">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="26635-209">Limitations</span></span>
* <span data-ttu-id="26635-210">hello támogatott bemeneti videó formátumnak tartalmaznia kell MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="26635-210">hello supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="26635-211">Mozgásérzékelés álló háttér videók van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="26635-211">Motion Detection is optimized for stationary background videos.</span></span> <span data-ttu-id="26635-212">téves riasztásokat, például a megvilágítási érintő változtatások, valamint az árnyékok csökkentése hello algoritmus összpontosít.</span><span class="sxs-lookup"><span data-stu-id="26635-212">hello algorithm focuses on reducing false alarms, such as lighting changes, and shadows.</span></span>
* <span data-ttu-id="26635-213">Néhány mozgásérzékelési tootechnical kihívást; miatt nem észlelhető például éjszaka stratégiai videók, félig átlátható és kis objektumok.</span><span class="sxs-lookup"><span data-stu-id="26635-213">Some motion may not be detected due tootechnical challenges; e.g. night vision videos, semi-transparent objects, and small objects.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="26635-214">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="26635-214">.NET sample code</span></span>

<span data-ttu-id="26635-215">hello következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="26635-215">hello following program shows how to:</span></span>

1. <span data-ttu-id="26635-216">Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.</span><span class="sxs-lookup"><span data-stu-id="26635-216">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="26635-217">Hozzon létre egy feladatot a json-készlet a következő hello tartalmazó konfigurációs fájl alapján Videós mozgásérzékelési észlelési feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="26635-217">Create a job with a video motion detection task based on a configuration file that contains hello following json preset.</span></span> 
   
        {
          "Version": "1.0",
          "Options": {
            "SensitivityLevel": "medium",
            "FrameSamplingValue": 1,
            "DetectLightChange": "False",
            "MergeTimeThreshold":
            "00:00:02",
            "DetectionZones": [
              [
                {"x": 0, "y": 0},
                {"x": 0.5, "y": 0},
                {"x": 0, "y": 1}
               ],
              [
                {"x": 0.3, "y": 0.3},
                {"x": 0.55, "y": 0.3},
                {"x": 0.8, "y": 0.3},
                {"x": 0.8, "y": 0.55},
                {"x": 0.8, "y": 0.8},
                {"x": 0.55, "y": 0.8},
                {"x": 0.3, "y": 0.8},
                {"x": 0.3, "y": 0.55}
              ]
            ]
          }
        }
3. <span data-ttu-id="26635-218">Hello kimeneti JSON-fájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="26635-218">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="26635-219">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26635-219">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="26635-220">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="26635-220">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="26635-221">Példa</span><span class="sxs-lookup"><span data-stu-id="26635-221">Example</span></span>


    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoMotionDetection
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

                // Run hello VideoMotionDetection job.
                var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoMotionDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
            }

            static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Motion Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Motion Detection Job");

                // Get a reference tooAzure Media Motion Detector.
                string MediaProcessorName = "Azure Media Motion Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="26635-222">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="26635-222">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="26635-223">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="26635-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="26635-224">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="26635-224">Related links</span></span>
[<span data-ttu-id="26635-225">Az Azure Media Services mozgásérzékelő blog</span><span class="sxs-lookup"><span data-stu-id="26635-225">Azure Media Services Motion Detector blog</span></span>](https://azure.microsoft.com/blog/motion-detector-update/)

[<span data-ttu-id="26635-226">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="26635-226">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="26635-227">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="26635-227">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


---
title: "Az Azure Media Analytics mozdulatok észlelése |} Microsoft Docs"
description: "Az Azure Media mozgásérzékelő media processzor (MP) lehetővé teszi egy egyébként hosszú és Eseménytelen video házirendsablonokkal szakaszok hatékonyan azonosításához."
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
ms.openlocfilehash: 115ad9dfd88062f23d5d17eed8897ce5d2ca8484
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a><span data-ttu-id="0db2d-103">Az Azure Media Analytics mozdulatok észlelése</span><span class="sxs-lookup"><span data-stu-id="0db2d-103">Detect Motions with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="0db2d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0db2d-104">Overview</span></span>
<span data-ttu-id="0db2d-105">A **Azure Media mozgásérzékelő** media processzor (MP) lehetővé teszi a szakaszok egy egyébként hosszú és Eseménytelen video házirendsablonokkal hatékonyan azonosításához.</span><span class="sxs-lookup"><span data-stu-id="0db2d-105">The **Azure Media Motion Detector** media processor (MP) enables you to efficiently identify sections of interest within an otherwise long and uneventful video.</span></span> <span data-ttu-id="0db2d-106">Mozgásérzékelés statikus kamerák felvételei szakaszok a videó hol mozgásérzékelési jelentkezik azonosításához használhatja.</span><span class="sxs-lookup"><span data-stu-id="0db2d-106">Motion detection can be used on static camera footage to identify sections of the video where motion occurs.</span></span> <span data-ttu-id="0db2d-107">Az időbélyegzőket és a határoló régió, ahol az esemény történt a metaadatokat tartalmazó JSON-fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0db2d-107">It generates a JSON file containing a metadata with timestamps and the bounding region where the event occurred.</span></span>

<span data-ttu-id="0db2d-108">Cél biztonsági videó hírcsatornák, ez a technológia el tudja mozgásérzékelési kategorizálása kapcsolódó eseményeket és vakriasztások például árnyékok és megvilágítási módosításokat.</span><span class="sxs-lookup"><span data-stu-id="0db2d-108">Targeted towards security video feeds, this technology is able to categorize motion into relevant events and false positives such as shadows and lighting changes.</span></span> <span data-ttu-id="0db2d-109">Ez lehetővé teszi, hogy a biztonsági riasztások generálása kamera hírcsatornák közben igényt érdeklő pillanat kinyerése rendkívül hosszú felügyeleti videók végtelen irreleváns események, éppen címünkre nélkül.</span><span class="sxs-lookup"><span data-stu-id="0db2d-109">This allows you to generate security alerts from camera feeds without being spammed with endless irrelevant events, while being able to extract moments of interest from extremely long surveillance videos.</span></span>

<span data-ttu-id="0db2d-110">A **Azure Media mozgásérzékelő** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="0db2d-110">The **Azure Media Motion Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="0db2d-111">Ez a témakör kapcsolatos részleteket nyújt **Azure Media mozgásérzékelő** és a .NET-keretrendszerhez készült Media Services SDK-val való használatát ismerteti</span><span class="sxs-lookup"><span data-stu-id="0db2d-111">This topic gives details about  **Azure Media Motion Detector** and shows how to use it with Media Services SDK for .NET</span></span>

## <a name="motion-detector-input-files"></a><span data-ttu-id="0db2d-112">Mozgásérzékelő – érzékelő bemeneti fájlok</span><span class="sxs-lookup"><span data-stu-id="0db2d-112">Motion Detector input files</span></span>
<span data-ttu-id="0db2d-113">Videofájlok lejátszását.</span><span class="sxs-lookup"><span data-stu-id="0db2d-113">Video files.</span></span> <span data-ttu-id="0db2d-114">Jelenleg a következő formátumok használhatók: MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="0db2d-114">Currently, the following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration-preset"></a><span data-ttu-id="0db2d-115">A feladat konfigurációja (beállítás)</span><span class="sxs-lookup"><span data-stu-id="0db2d-115">Task configuration (preset)</span></span>
<span data-ttu-id="0db2d-116">A feladat létrehozásakor **Azure Media mozgásérzékelő**, meg kell adnia egy konfigurációs készletet.</span><span class="sxs-lookup"><span data-stu-id="0db2d-116">When creating a task with **Azure Media Motion Detector**, you must specify a configuration preset.</span></span> 

### <a name="parameters"></a><span data-ttu-id="0db2d-117">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="0db2d-117">Parameters</span></span>
<span data-ttu-id="0db2d-118">A következő paramétereket használhatja:</span><span class="sxs-lookup"><span data-stu-id="0db2d-118">You can use the following parameters:</span></span>

| <span data-ttu-id="0db2d-119">Név</span><span class="sxs-lookup"><span data-stu-id="0db2d-119">Name</span></span> | <span data-ttu-id="0db2d-120">Beállítások</span><span class="sxs-lookup"><span data-stu-id="0db2d-120">Options</span></span> | <span data-ttu-id="0db2d-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="0db2d-121">Description</span></span> | <span data-ttu-id="0db2d-122">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="0db2d-122">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0db2d-123">sensitivityLevel</span><span class="sxs-lookup"><span data-stu-id="0db2d-123">sensitivityLevel</span></span> |<span data-ttu-id="0db2d-124">Karakterlánc: "alacsony", "közepes", "magas"</span><span class="sxs-lookup"><span data-stu-id="0db2d-124">String:'low', 'medium', 'high'</span></span> |<span data-ttu-id="0db2d-125">Beállítja az érzékenységi szint mely mozdulatok jelenti.</span><span class="sxs-lookup"><span data-stu-id="0db2d-125">Sets the sensitivity level at which motions is reported.</span></span> <span data-ttu-id="0db2d-126">Ez úgy, hogy téves mennyisége módosítása</span><span class="sxs-lookup"><span data-stu-id="0db2d-126">Adjust this to adjust amount of false positives.</span></span> |<span data-ttu-id="0db2d-127">"közepes"</span><span class="sxs-lookup"><span data-stu-id="0db2d-127">'medium'</span></span> |
| <span data-ttu-id="0db2d-128">frameSamplingValue</span><span class="sxs-lookup"><span data-stu-id="0db2d-128">frameSamplingValue</span></span> |<span data-ttu-id="0db2d-129">Pozitív egész szám</span><span class="sxs-lookup"><span data-stu-id="0db2d-129">Positive integer</span></span> |<span data-ttu-id="0db2d-130">A készletek algoritmus gyakorisága futtatja.</span><span class="sxs-lookup"><span data-stu-id="0db2d-130">Sets the frequency at which algorithm runs.</span></span> <span data-ttu-id="0db2d-131">1 egyenlő minden keret, 2 azt jelenti, hogy minden 2. keret, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="0db2d-131">1 equals every frame, 2 means every 2nd frame, and so on.</span></span> |<span data-ttu-id="0db2d-132">1</span><span class="sxs-lookup"><span data-stu-id="0db2d-132">1</span></span> |
| <span data-ttu-id="0db2d-133">detectLightChange</span><span class="sxs-lookup"><span data-stu-id="0db2d-133">detectLightChange</span></span> |<span data-ttu-id="0db2d-134">Logikai: "true", "false"</span><span class="sxs-lookup"><span data-stu-id="0db2d-134">Boolean:'true', 'false'</span></span> |<span data-ttu-id="0db2d-135">Megadja, hogy e könnyű módosítások jelentik az eredmények között</span><span class="sxs-lookup"><span data-stu-id="0db2d-135">Sets whether light changes are reported in the results</span></span> |<span data-ttu-id="0db2d-136">"False"</span><span class="sxs-lookup"><span data-stu-id="0db2d-136">'False'</span></span> |
| <span data-ttu-id="0db2d-137">mergeTimeThreshold</span><span class="sxs-lookup"><span data-stu-id="0db2d-137">mergeTimeThreshold</span></span> |<span data-ttu-id="0db2d-138">Idő xs: Óó: pp:</span><span class="sxs-lookup"><span data-stu-id="0db2d-138">Xs-time: Hh:mm:ss</span></span><br/><span data-ttu-id="0db2d-139">. Példa: 00:00:03</span><span class="sxs-lookup"><span data-stu-id="0db2d-139">Example: 00:00:03</span></span> |<span data-ttu-id="0db2d-140">Adja meg az időszak, ahol 2 esemény lesz kombinált és 1 jelentett mozgásérzékelési események között.</span><span class="sxs-lookup"><span data-stu-id="0db2d-140">Specifies the time window between motion events where 2 events will be combined and reported as 1.</span></span> |<span data-ttu-id="0db2d-141">00:00:00</span><span class="sxs-lookup"><span data-stu-id="0db2d-141">00:00:00</span></span> |
| <span data-ttu-id="0db2d-142">detectionZones</span><span class="sxs-lookup"><span data-stu-id="0db2d-142">detectionZones</span></span> |<span data-ttu-id="0db2d-143">A tömb észlelési zónák:</span><span class="sxs-lookup"><span data-stu-id="0db2d-143">An array of detection zones:</span></span><br/><span data-ttu-id="0db2d-144">-Észlelési zóna: 3 vagy több pontok bájttömb</span><span class="sxs-lookup"><span data-stu-id="0db2d-144">- Detection Zone is an array of 3 or more points</span></span><br/><span data-ttu-id="0db2d-145">-Pont egy x és y koordináta 0, 1.</span><span class="sxs-lookup"><span data-stu-id="0db2d-145">- Point is a x and y coordinate from 0 to 1.</span></span> |<span data-ttu-id="0db2d-146">Ismerteti a sokszög észlelési zónák használandó listáját.</span><span class="sxs-lookup"><span data-stu-id="0db2d-146">Describes the list of polygonal detection zones to be used.</span></span><br/><span data-ttu-id="0db2d-147">Az ID, az első egy folyamatban "id" a zónák lesz jelentve az eredményeket: 0</span><span class="sxs-lookup"><span data-stu-id="0db2d-147">Results will be reported with the zones as an ID, with the first one being 'id':0</span></span> |<span data-ttu-id="0db2d-148">Egy zóna, amely magában foglalja a teljes keret.</span><span class="sxs-lookup"><span data-stu-id="0db2d-148">Single zone which covers the entire frame.</span></span> |

### <a name="json-example"></a><span data-ttu-id="0db2d-149">JSON-példa</span><span class="sxs-lookup"><span data-stu-id="0db2d-149">JSON example</span></span>
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


## <a name="motion-detector-output-files"></a><span data-ttu-id="0db2d-150">Mozgásérzékelő – érzékelő kimeneti fájlok</span><span class="sxs-lookup"><span data-stu-id="0db2d-150">Motion Detector output files</span></span>
<span data-ttu-id="0db2d-151">A mozgás észlelése a JSON-fájl adja vissza a kimeneti adategységen, amely mozgásérzékelési riasztásait és a kategóriák belül a videó ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0db2d-151">A motion detection job will return a JSON file in the output asset which describes the motion alerts, and their categories, within the video.</span></span> <span data-ttu-id="0db2d-152">A fájl ideje és időtartama észlelte a videóból mozgásérzékelési fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="0db2d-152">The file will contain information about the time and duration of motion detected in the video.</span></span>

<span data-ttu-id="0db2d-153">A mozgás érzékelő API mutatók biztosít, amennyiben nincsenek objektumok mozgásérzékelési rögzített háttér video (pl. egy felügyeleti videó).</span><span class="sxs-lookup"><span data-stu-id="0db2d-153">The Motion Detector API provides indicators once there are objects in motion in a fixed background video (e.g. a surveillance video).</span></span> <span data-ttu-id="0db2d-154">A mozgásérzékelő be van tanítva, téves riasztásokat, például a megvilágítási és árnyékmásolat módosítások csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="0db2d-154">The Motion Detector is trained to reduce false alarms, such as lighting and shadow changes.</span></span> <span data-ttu-id="0db2d-155">Aktuális korlátozások algoritmusok éjszakai stratégiai videók, félig átlátható és kis objektumok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0db2d-155">Current limitations of the algorithms include night vision videos, semi-transparent objects, and small objects.</span></span>

### <span data-ttu-id="0db2d-156"><a id="output_elements"></a>A kimeneti JSON-fájl elemeinek</span><span class="sxs-lookup"><span data-stu-id="0db2d-156"><a id="output_elements"></a>Elements of the output JSON file</span></span>
> [!NOTE]
> <span data-ttu-id="0db2d-157">Legújabb kiadása a kimeneti JSON formátumban megváltozott, és az egyes ügyfelek használhatatlanná tévő változást jelöl.</span><span class="sxs-lookup"><span data-stu-id="0db2d-157">In the latest release, the Output JSON format has changed and may represent a breaking change for some customers.</span></span>
> 
> 

<span data-ttu-id="0db2d-158">A következő táblázat ismerteti a kimeneti JSON-fájl elemeinek.</span><span class="sxs-lookup"><span data-stu-id="0db2d-158">The following table describes elements of the output JSON file.</span></span>

| <span data-ttu-id="0db2d-159">Elem</span><span class="sxs-lookup"><span data-stu-id="0db2d-159">Element</span></span> | <span data-ttu-id="0db2d-160">Leírás</span><span class="sxs-lookup"><span data-stu-id="0db2d-160">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0db2d-161">Verzió</span><span class="sxs-lookup"><span data-stu-id="0db2d-161">Version</span></span> |<span data-ttu-id="0db2d-162">Ez a videó API verziója vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="0db2d-162">This refers to the version of the Video API.</span></span> <span data-ttu-id="0db2d-163">A jelenlegi verzió: 2.</span><span class="sxs-lookup"><span data-stu-id="0db2d-163">The current version is 2.</span></span> |
| <span data-ttu-id="0db2d-164">időskálára</span><span class="sxs-lookup"><span data-stu-id="0db2d-164">Timescale</span></span> |<span data-ttu-id="0db2d-165">A videó másodpercenként "ticks".</span><span class="sxs-lookup"><span data-stu-id="0db2d-165">"Ticks" per second of the video.</span></span> |
| <span data-ttu-id="0db2d-166">Eltolás</span><span class="sxs-lookup"><span data-stu-id="0db2d-166">Offset</span></span> |<span data-ttu-id="0db2d-167">A "ticks" időbélyegeket időeltolódás.</span><span class="sxs-lookup"><span data-stu-id="0db2d-167">The time offset for timestamps in "ticks".</span></span> <span data-ttu-id="0db2d-168">Videó API-k 1.0-s verziójában az mindig 0 lesz.</span><span class="sxs-lookup"><span data-stu-id="0db2d-168">In version 1.0 of Video APIs, this will always be 0.</span></span> <span data-ttu-id="0db2d-169">A jövőben támogatott forgatókönyveket, ezt az értéket módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0db2d-169">In future scenarios we support, this value may change.</span></span> |
| <span data-ttu-id="0db2d-170">képkockasebességhez</span><span class="sxs-lookup"><span data-stu-id="0db2d-170">Framerate</span></span> |<span data-ttu-id="0db2d-171">A videó képkockasebessége.</span><span class="sxs-lookup"><span data-stu-id="0db2d-171">Frames per second of the video.</span></span> |
| <span data-ttu-id="0db2d-172">Szélesség, Hosszúság</span><span class="sxs-lookup"><span data-stu-id="0db2d-172">Width, Height</span></span> |<span data-ttu-id="0db2d-173">A szélességét és magasságát képpontban hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="0db2d-173">Refers to the width and height of the video in pixels.</span></span> |
| <span data-ttu-id="0db2d-174">Indítás</span><span class="sxs-lookup"><span data-stu-id="0db2d-174">Start</span></span> |<span data-ttu-id="0db2d-175">A start időbélyegzőjéhez viszonyítva a "ticks".</span><span class="sxs-lookup"><span data-stu-id="0db2d-175">The start timestamp in "ticks".</span></span> |
| <span data-ttu-id="0db2d-176">Időtartam</span><span class="sxs-lookup"><span data-stu-id="0db2d-176">Duration</span></span> |<span data-ttu-id="0db2d-177">Az esemény, a "ticks" hosszát.</span><span class="sxs-lookup"><span data-stu-id="0db2d-177">The length of the event, in "ticks".</span></span> |
| <span data-ttu-id="0db2d-178">időköz</span><span class="sxs-lookup"><span data-stu-id="0db2d-178">Interval</span></span> |<span data-ttu-id="0db2d-179">Az időköz az egyes bejegyzések az eseményben a "ticks".</span><span class="sxs-lookup"><span data-stu-id="0db2d-179">The interval of each entry in the event, in "ticks".</span></span> |
| <span data-ttu-id="0db2d-180">Események</span><span class="sxs-lookup"><span data-stu-id="0db2d-180">Events</span></span> |<span data-ttu-id="0db2d-181">Minden esemény-töredéket adott időtartamig belül észlelhető a mozgásérzékelési tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0db2d-181">Each event fragment contains the motion detected within that time duration.</span></span> |
| <span data-ttu-id="0db2d-182">Típus</span><span class="sxs-lookup"><span data-stu-id="0db2d-182">Type</span></span> |<span data-ttu-id="0db2d-183">A jelenlegi verzióban ez a "2" általános mozgásérzékelési.</span><span class="sxs-lookup"><span data-stu-id="0db2d-183">In the current version, this is always ‘2’ for generic motion.</span></span> <span data-ttu-id="0db2d-184">A címke által biztosított videó API-k a rugalmasságot kategorizálásának mozgási későbbi verzióit.</span><span class="sxs-lookup"><span data-stu-id="0db2d-184">This label gives Video APIs the flexibility to categorize motion in future versions.</span></span> |
| <span data-ttu-id="0db2d-185">RegionID</span><span class="sxs-lookup"><span data-stu-id="0db2d-185">RegionID</span></span> |<span data-ttu-id="0db2d-186">Ahogy fent, az mindig 0 lesz ebben a verzióban.</span><span class="sxs-lookup"><span data-stu-id="0db2d-186">As explained above, this will always be 0 in this version.</span></span> <span data-ttu-id="0db2d-187">Ezt a címkét mozgásérzékelési későbbi verzióiban különböző régiókban található rugalmasságot biztosít a videó API.</span><span class="sxs-lookup"><span data-stu-id="0db2d-187">This label gives Video API the flexibility to find motion in various regions in future versions.</span></span> |
| <span data-ttu-id="0db2d-188">Régiók</span><span class="sxs-lookup"><span data-stu-id="0db2d-188">Regions</span></span> |<span data-ttu-id="0db2d-189">A terület a videó, ahol az Ön számára legfontosabb mozgásérzékelési hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="0db2d-189">Refers to the area in your video where you care about motion.</span></span> <br/><br/><span data-ttu-id="0db2d-190">-az "id" jelöli a régió terület – ebben a verzióban van csak egy, azonosító: 0.</span><span class="sxs-lookup"><span data-stu-id="0db2d-190">-"id" represents the region area – in this version there is only one, ID 0.</span></span> <br/><span data-ttu-id="0db2d-191">-"type" jelenti. az Ön számára legfontosabb mozgásérzékelő – az a régió alakját.</span><span class="sxs-lookup"><span data-stu-id="0db2d-191">-"type" represents the shape of the region you care about for motion.</span></span> <span data-ttu-id="0db2d-192">Jelenleg a "téglalap", "sokszög" támogatottak.</span><span class="sxs-lookup"><span data-stu-id="0db2d-192">Currently, "rectangle" and "polygon" are supported.</span></span><br/> <span data-ttu-id="0db2d-193">Ha a megadott "téglalap", a régiót dimenziója van X, Y, szélességét és magasságát.</span><span class="sxs-lookup"><span data-stu-id="0db2d-193">If you specified "rectangle", the region has dimensions in X, Y, Width, and Height.</span></span> <span data-ttu-id="0db2d-194">Az X és Y koordinátáit a 0,0 és 1,0 normalizált skálája terület felső bal oldali Oxykiszolgáló koordinátáit jelölik.</span><span class="sxs-lookup"><span data-stu-id="0db2d-194">The X and Y coordinates represent the upper left hand XY coordinates of the region in a normalized scale of 0.0 to 1.0.</span></span> <span data-ttu-id="0db2d-195">A szélesség és magasság határoz meg a 0,0 és 1,0 normalizált skálája terület méretét.</span><span class="sxs-lookup"><span data-stu-id="0db2d-195">The width and height represent the size of the region in a normalized scale of 0.0 to 1.0.</span></span> <span data-ttu-id="0db2d-196">Az aktuális verzió X, Y, szélességének és magasságának mindig rögzített 0, 0 és 1, 1.</span><span class="sxs-lookup"><span data-stu-id="0db2d-196">In the current version, X, Y, Width, and Height are always fixed at 0, 0 and 1, 1.</span></span> <br/><span data-ttu-id="0db2d-197">Ha meg van adva "sokszög", az a régió pontokon dimenziója van.</span><span class="sxs-lookup"><span data-stu-id="0db2d-197">If you specified "polygon", the region has dimensions in points.</span></span> <br/> |
| <span data-ttu-id="0db2d-198">töredék</span><span class="sxs-lookup"><span data-stu-id="0db2d-198">Fragments</span></span> |<span data-ttu-id="0db2d-199">A metaadatok töredék nevű különböző részekre van darabolásos fel.</span><span class="sxs-lookup"><span data-stu-id="0db2d-199">The metadata is chunked up into different segments called fragments.</span></span> <span data-ttu-id="0db2d-200">Minden töredéke a start, a időtartama, a időszakának száma és a esemény (eke) tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0db2d-200">Each fragment contains a start, duration, interval number, and event(s).</span></span> <span data-ttu-id="0db2d-201">Nincsenek események a töredéket azt jelenti, hogy semmilyen adott kezdő időpontja és időtartama során észlelt.</span><span class="sxs-lookup"><span data-stu-id="0db2d-201">A fragment with no events means that no motion was detected during that start time and duration.</span></span> |
| <span data-ttu-id="0db2d-202">Szögletes zárójelbe]</span><span class="sxs-lookup"><span data-stu-id="0db2d-202">Brackets []</span></span> |<span data-ttu-id="0db2d-203">Minden egyes zárójel az esemény egy időköz jelöli.</span><span class="sxs-lookup"><span data-stu-id="0db2d-203">Each bracket represents one interval in the event.</span></span> <span data-ttu-id="0db2d-204">Üres szögletes az adott időköz azt jelenti, hogy semmilyen volt észlelhető.</span><span class="sxs-lookup"><span data-stu-id="0db2d-204">Empty brackets for that interval means that no motion was detected.</span></span> |
| <span data-ttu-id="0db2d-205">Helyek</span><span class="sxs-lookup"><span data-stu-id="0db2d-205">locations</span></span> |<span data-ttu-id="0db2d-206">Az új belépési események alapján sorolja fel a helyre, ahol a mozgásérzékelési történt.</span><span class="sxs-lookup"><span data-stu-id="0db2d-206">This new entry under events lists the location where the motion occurred.</span></span> <span data-ttu-id="0db2d-207">Ez a pontosabb, mint a észlelési zónákat.</span><span class="sxs-lookup"><span data-stu-id="0db2d-207">This is more specific than the detection zones.</span></span> |

<span data-ttu-id="0db2d-208">A JSON kimeneti például</span><span class="sxs-lookup"><span data-stu-id="0db2d-208">The following is a JSON output example</span></span>

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
## <a name="limitations"></a><span data-ttu-id="0db2d-209">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="0db2d-209">Limitations</span></span>
* <span data-ttu-id="0db2d-210">A támogatott bemeneti videó formátumnak tartalmaznia kell MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="0db2d-210">The supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="0db2d-211">Mozgásérzékelés álló háttér videók van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="0db2d-211">Motion Detection is optimized for stationary background videos.</span></span> <span data-ttu-id="0db2d-212">Az algoritmus összpontosít téves riasztásokat, például a megvilágítási érintő változtatások, valamint az árnyékok csökkentése.</span><span class="sxs-lookup"><span data-stu-id="0db2d-212">The algorithm focuses on reducing false alarms, such as lighting changes, and shadows.</span></span>
* <span data-ttu-id="0db2d-213">Néhány mozgásérzékelő – műszaki akadályok; miatt nem észlelhető például éjszaka stratégiai videók, félig átlátható és kis objektumok.</span><span class="sxs-lookup"><span data-stu-id="0db2d-213">Some motion may not be detected due to technical challenges; e.g. night vision videos, semi-transparent objects, and small objects.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="0db2d-214">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="0db2d-214">.NET sample code</span></span>

<span data-ttu-id="0db2d-215">A következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="0db2d-215">The following program shows how to:</span></span>

1. <span data-ttu-id="0db2d-216">Hozzon létre egy eszközt, és adathordozó-fájl feltöltése az objektumba.</span><span class="sxs-lookup"><span data-stu-id="0db2d-216">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="0db2d-217">Hozzon létre egy feladatot a következő json-készletet tartalmazó konfigurációs fájl alapján Videós mozgásérzékelési észlelési feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="0db2d-217">Create a job with a video motion detection task based on a configuration file that contains the following json preset.</span></span> 
   
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
3. <span data-ttu-id="0db2d-218">A kimeneti JSON-fájlok letöltésére.</span><span class="sxs-lookup"><span data-stu-id="0db2d-218">Download the output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="0db2d-219">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0db2d-219">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="0db2d-220">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="0db2d-220">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="0db2d-221">Példa</span><span class="sxs-lookup"><span data-stu-id="0db2d-221">Example</span></span>


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

                // Run the VideoMotionDetection job.
                var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoMotionDetection\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
            }

            static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Motion Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Motion Detection Job");

                // Get a reference to Azure Media Motion Detector.
                string MediaProcessorName = "Azure Media Motion Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="0db2d-222">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0db2d-222">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0db2d-223">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0db2d-223">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="0db2d-224">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="0db2d-224">Related links</span></span>
[<span data-ttu-id="0db2d-225">Az Azure Media Services mozgásérzékelő blog</span><span class="sxs-lookup"><span data-stu-id="0db2d-225">Azure Media Services Motion Detector blog</span></span>](https://azure.microsoft.com/blog/motion-detector-update/)

[<span data-ttu-id="0db2d-226">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="0db2d-226">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="0db2d-227">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="0db2d-227">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)


---
title: "az Azure Media Analytics OCR aaaDigitize szöveg |} Microsoft Docs"
description: "Az Azure Media Analytics OCR (optikai karakter felismerés) lehetővé teszi tooconvert szöveges tartalom videofájlok szerkeszthető, kereshető digitális szöveggé.  Ez lehetővé teszi tooautomate hello kivonása jelentéssel bíró metaadatok hello videó Signal médiafájlt."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 0476c3ba3942b2c5182a34a429909adbf5c75ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-media-analytics-tooconvert-text-content-in-video-files-into-digital-text"></a><span data-ttu-id="aae47-104">Azure Media Analytics tooconvert szöveges tartalom digitális szöveggé videofájlok használata</span><span class="sxs-lookup"><span data-stu-id="aae47-104">Use Azure Media Analytics tooconvert text content in video files into digital text</span></span>
## <a name="overview"></a><span data-ttu-id="aae47-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="aae47-105">Overview</span></span>
<span data-ttu-id="aae47-106">Ha tooextract szöveges tartalom kell a video-fájlokból és létrehozni egy szerkeszthető, kereshető digitális szöveget, az Azure Media Analytics OCR (optikai karakter használata) kell használnia.</span><span class="sxs-lookup"><span data-stu-id="aae47-106">If you need tooextract text content from your video files and generate an editable, searchable digital text, you should use Azure Media Analytics OCR (optical character recognition).</span></span> <span data-ttu-id="aae47-107">Az Azure Media processzor szöveges tartalom észleli a videofájlok lejátszását, és állít elő, a szöveg fájljait.</span><span class="sxs-lookup"><span data-stu-id="aae47-107">This Azure Media Processor detects text content in your video files and generates text files for your use.</span></span> <span data-ttu-id="aae47-108">OCR lehetővé teszi, hogy Ön tooautomate hello kibontásával jelentéssel bíró metaadatok hello videó Signal médiafájlt.</span><span class="sxs-lookup"><span data-stu-id="aae47-108">OCR enables you tooautomate hello extraction of meaningful metadata from hello video signal of your media.</span></span>

<span data-ttu-id="aae47-109">Ha együtt használják a keresőprogramok találatait, egyszerűen médiatartalmak index a szöveg, és hello felderíthetőség a tartalom javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="aae47-109">When used in conjunction with a search engine, you can easily index your media by text, and enhance hello discoverability of your content.</span></span> <span data-ttu-id="aae47-110">Ez különösen fontos például videó-felvételt vagy diavetítési bemutató képernyőfelvétel a magas szöveges videó.</span><span class="sxs-lookup"><span data-stu-id="aae47-110">This is extremely useful in highly textual video, like a video recording or screen-capture of a slideshow presentation.</span></span> <span data-ttu-id="aae47-111">hello Azure OCR Media processzor digitális szöveg van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="aae47-111">hello Azure OCR Media Processor is optimized for digital text.</span></span>

<span data-ttu-id="aae47-112">Hello **Azure Media OCR** media processzor jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="aae47-112">hello **Azure Media OCR** media processor is currently in Preview.</span></span>

<span data-ttu-id="aae47-113">Ez a témakör kapcsolatos részleteket nyújt **Azure Media OCR** és bemutatja, hogyan toouse a Media Services SDK for .NET.</span><span class="sxs-lookup"><span data-stu-id="aae47-113">This topic gives details about  **Azure Media OCR** and shows how toouse it with Media Services SDK for .NET.</span></span> <span data-ttu-id="aae47-114">További információt és példákat lásd: [ebben a blogban](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span><span class="sxs-lookup"><span data-stu-id="aae47-114">For additional information and examples, see [this blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).</span></span>

## <a name="ocr-input-files"></a><span data-ttu-id="aae47-115">A bemeneti fájlok OCR</span><span class="sxs-lookup"><span data-stu-id="aae47-115">OCR input files</span></span>
<span data-ttu-id="aae47-116">Videofájlok lejátszását.</span><span class="sxs-lookup"><span data-stu-id="aae47-116">Video files.</span></span> <span data-ttu-id="aae47-117">Jelenleg a következő formátumok hello támogatottak: MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="aae47-117">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="task-configuration"></a><span data-ttu-id="aae47-118">A feladat konfigurációja</span><span class="sxs-lookup"><span data-stu-id="aae47-118">Task configuration</span></span>
<span data-ttu-id="aae47-119">A feladat konfigurációja (beállítás).</span><span class="sxs-lookup"><span data-stu-id="aae47-119">Task configuration (preset).</span></span> <span data-ttu-id="aae47-120">A feladat létrehozásakor **Azure Media OCR**, meg kell adnia egy készlet használatával JSON vagy XML-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="aae47-120">When creating a task with **Azure Media OCR**, you must specify a configuration preset using JSON  or XML.</span></span> 

>[!NOTE]
><span data-ttu-id="aae47-121">hello OCR motor mindössze egy lemezkép a régióban, minimális 40 képpont toomaximum 32000 képpont mindkét magasság/szélesség egy érvényes bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="aae47-121">hello OCR engine only takes an image region with minimum 40 pixels toomaximum 32000 pixels as a valid input in both height/width.</span></span>
>

### <a name="attribute-descriptions"></a><span data-ttu-id="aae47-122">Az attribútumok leírása</span><span class="sxs-lookup"><span data-stu-id="aae47-122">Attribute descriptions</span></span>
| <span data-ttu-id="aae47-123">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="aae47-123">Attribute name</span></span> | <span data-ttu-id="aae47-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="aae47-124">Description</span></span> |
| --- | --- |
|<span data-ttu-id="aae47-125">AdvancedOutput</span><span class="sxs-lookup"><span data-stu-id="aae47-125">AdvancedOutput</span></span>| <span data-ttu-id="aae47-126">Ha AdvancedOutput tootrue, hello JSON-kimenetét (a hozzáadása toophrases és régiók) minden egyszavas pozicionális adatait fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="aae47-126">If you set AdvancedOutput tootrue, hello JSON output will contain positional data for every single word (in addition toophrases and regions).</span></span> <span data-ttu-id="aae47-127">Ha nem szeretné, hogy toosee ezeket az adatokat, állítsa be a hello jelző toofalse.</span><span class="sxs-lookup"><span data-stu-id="aae47-127">If you do not want toosee these details, set hello flag toofalse.</span></span> <span data-ttu-id="aae47-128">hello alapértelmezett értéke hamis.</span><span class="sxs-lookup"><span data-stu-id="aae47-128">hello default value is false.</span></span> <span data-ttu-id="aae47-129">További információkért lásd: [ebben a blogban](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span><span class="sxs-lookup"><span data-stu-id="aae47-129">For more information, see [this blog](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).</span></span>|
| <span data-ttu-id="aae47-130">Nyelv</span><span class="sxs-lookup"><span data-stu-id="aae47-130">Language</span></span> |<span data-ttu-id="aae47-131">(választható) ismerteti, mely toolook szövege hello nyelvét.</span><span class="sxs-lookup"><span data-stu-id="aae47-131">(optional) describes hello language of text for which toolook.</span></span> <span data-ttu-id="aae47-132">Hello a következők egyikét: automatikus felismerés (alapértelmezett), arab, ChineseSimplified, ChineseTraditional, dán Cseh, holland, angol, finn, francia, német, görög, magyar, olasz, japán, koreai, lengyel, lengyel, portugál, román, orosz, SerbianCyrillic, SerbianLatin, szlovák, spanyol, svéd, török.</span><span class="sxs-lookup"><span data-stu-id="aae47-132">One of hello following: AutoDetect (default), Arabic, ChineseSimplified, ChineseTraditional, Czech Danish, Dutch, English, Finnish, French, German,  Greek, Hungarian, Italian, Japanese, Korean, Norwegian, Polish, Portuguese, Romanian, Russian, SerbianCyrillic, SerbianLatin, Slovak, Spanish, Swedish, Turkish.</span></span> |
| <span data-ttu-id="aae47-133">TextOrientation</span><span class="sxs-lookup"><span data-stu-id="aae47-133">TextOrientation</span></span> |<span data-ttu-id="aae47-134">(választható) ismerteti, mely toolook szövege hello tájolását.</span><span class="sxs-lookup"><span data-stu-id="aae47-134">(optional) describes hello orientation of text for which toolook.</span></span>  <span data-ttu-id="aae47-135">"Bal oldali" azt jelenti, hogy az összes felső hello felé hello bal vannak változó.</span><span class="sxs-lookup"><span data-stu-id="aae47-135">"Left" means that hello top of all letters are pointed towards hello left.</span></span>  <span data-ttu-id="aae47-136">Alapértelmezett szöveg (például, amely itt található: egy könyv) hívható "Mentés" objektumorientált.</span><span class="sxs-lookup"><span data-stu-id="aae47-136">Default text (like that which can be found in a book) can be called "Up" oriented.</span></span>  <span data-ttu-id="aae47-137">Hello a következők egyikét: AutoDetect (alapértelmezett), akár, jobbra, lefelé, balra.</span><span class="sxs-lookup"><span data-stu-id="aae47-137">One of hello following: AutoDetect (default), Up, Right, Down, Left.</span></span> |
| <span data-ttu-id="aae47-138">TimeInterval</span><span class="sxs-lookup"><span data-stu-id="aae47-138">TimeInterval</span></span> |<span data-ttu-id="aae47-139">(választható) hello mintavételi ráta ismerteti.</span><span class="sxs-lookup"><span data-stu-id="aae47-139">(optional) describes hello sampling rate.</span></span>  <span data-ttu-id="aae47-140">Alapértelmezett érték 1/2 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="aae47-140">Default is every 1/2 second.</span></span><br/><span data-ttu-id="aae47-141">JSON formátumban – óó: pp:. Biztonságos tár szolgáltatás (alapértelmezett 00:00:00.500)</span><span class="sxs-lookup"><span data-stu-id="aae47-141">JSON format – HH:mm:ss.SSS (default 00:00:00.500)</span></span><br/><span data-ttu-id="aae47-142">XML-formátum – a W3C XSD időtartama egyszerű (alapértelmezett PT0.5)</span><span class="sxs-lookup"><span data-stu-id="aae47-142">XML format – W3C XSD duration primitive (default PT0.5)</span></span> |
| <span data-ttu-id="aae47-143">DetectRegions</span><span class="sxs-lookup"><span data-stu-id="aae47-143">DetectRegions</span></span> |<span data-ttu-id="aae47-144">(választható) Melyik toodetect szöveget ad meg a régiók hello videó keretbe DetectRegion objektumokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="aae47-144">(optional) An array of DetectRegion objects specifying regions within hello video frame in which toodetect text.</span></span><br/><span data-ttu-id="aae47-145">Egy DetectRegion objektum hello négy egész érték a következő történik:</span><span class="sxs-lookup"><span data-stu-id="aae47-145">A DetectRegion object is made of hello following four integer values:</span></span><br/><span data-ttu-id="aae47-146">Left – a bal margó hello képpontban megadva</span><span class="sxs-lookup"><span data-stu-id="aae47-146">Left – pixels from hello left-margin</span></span><br/><span data-ttu-id="aae47-147">Felső – hello felső-margónál képpontban megadva</span><span class="sxs-lookup"><span data-stu-id="aae47-147">Top – pixels from hello top-margin</span></span><br/><span data-ttu-id="aae47-148">Szélesség – szélessége képpontban hello régió</span><span class="sxs-lookup"><span data-stu-id="aae47-148">Width – width of hello region in pixels</span></span><br/><span data-ttu-id="aae47-149">Magasság – magassága képpontban hello régió</span><span class="sxs-lookup"><span data-stu-id="aae47-149">Height – height of hello region in pixels</span></span> |

#### <a name="json-preset-example"></a><span data-ttu-id="aae47-150">Előre definiált JSON-példa</span><span class="sxs-lookup"><span data-stu-id="aae47-150">JSON preset example</span></span>

    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }


#### <a name="xml-preset-example"></a><span data-ttu-id="aae47-151">Előre definiált XML-példa</span><span class="sxs-lookup"><span data-stu-id="aae47-151">XML preset example</span></span>
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

## <a name="ocr-output-files"></a><span data-ttu-id="aae47-152">OCR kimeneti fájlok</span><span class="sxs-lookup"><span data-stu-id="aae47-152">OCR output files</span></span>
<span data-ttu-id="aae47-153">hello hello OCR media processzor eredménye egy JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="aae47-153">hello output of hello OCR media processor is a JSON file.</span></span>

### <a name="elements-of-hello-output-json-file"></a><span data-ttu-id="aae47-154">Hello kimeneti JSON-fájl elemeinek</span><span class="sxs-lookup"><span data-stu-id="aae47-154">Elements of hello output JSON file</span></span>
<span data-ttu-id="aae47-155">hello videó OCR kimeneti idő szegmentált adatokat biztosít a videó megtalálható hello karaktere.</span><span class="sxs-lookup"><span data-stu-id="aae47-155">hello Video OCR output provides time-segmented data on hello characters found in your video.</span></span>  <span data-ttu-id="aae47-156">Attribútumok nyelven vagy toohone a tájolás például, hogy érdekli elemzése pontosan hello szavak is használhatja.</span><span class="sxs-lookup"><span data-stu-id="aae47-156">You can use attributes such as language or orientation toohone-in on exactly hello words that you are interested in analyzing.</span></span> 

<span data-ttu-id="aae47-157">hello kimenet tartalmazza a következő attribútumok hello:</span><span class="sxs-lookup"><span data-stu-id="aae47-157">hello output contains hello following attributes:</span></span>

| <span data-ttu-id="aae47-158">Elem</span><span class="sxs-lookup"><span data-stu-id="aae47-158">Element</span></span> | <span data-ttu-id="aae47-159">Leírás</span><span class="sxs-lookup"><span data-stu-id="aae47-159">Description</span></span> |
| --- | --- |
| <span data-ttu-id="aae47-160">időskálára</span><span class="sxs-lookup"><span data-stu-id="aae47-160">Timescale</span></span> |<span data-ttu-id="aae47-161">"ticks" hello videó másodpercenként</span><span class="sxs-lookup"><span data-stu-id="aae47-161">"ticks" per second of hello video</span></span> |
| <span data-ttu-id="aae47-162">Eltolás</span><span class="sxs-lookup"><span data-stu-id="aae47-162">Offset</span></span> |<span data-ttu-id="aae47-163">az időbélyegekhez időeltolódás.</span><span class="sxs-lookup"><span data-stu-id="aae47-163">time offset for timestamps.</span></span> <span data-ttu-id="aae47-164">Videó API-k 1.0-s verziójában az mindig 0 lesz.</span><span class="sxs-lookup"><span data-stu-id="aae47-164">In version 1.0 of Video APIs, this will always be 0.</span></span> |
| <span data-ttu-id="aae47-165">képkockasebességhez</span><span class="sxs-lookup"><span data-stu-id="aae47-165">Framerate</span></span> |<span data-ttu-id="aae47-166">Videó hello képkockasebessége</span><span class="sxs-lookup"><span data-stu-id="aae47-166">Frames per second of hello video</span></span> |
| <span data-ttu-id="aae47-167">Szélessége</span><span class="sxs-lookup"><span data-stu-id="aae47-167">width</span></span> |<span data-ttu-id="aae47-168">videó képpontban hello szélessége</span><span class="sxs-lookup"><span data-stu-id="aae47-168">width of hello video in pixels</span></span> |
| <span data-ttu-id="aae47-169">Magassága</span><span class="sxs-lookup"><span data-stu-id="aae47-169">height</span></span> |<span data-ttu-id="aae47-170">videó képpontban hello magassága</span><span class="sxs-lookup"><span data-stu-id="aae47-170">height of hello video in pixels</span></span> |
| <span data-ttu-id="aae47-171">töredék</span><span class="sxs-lookup"><span data-stu-id="aae47-171">Fragments</span></span> |<span data-ttu-id="aae47-172">mely hello be metaadatok darabolásos van videó időalapú adattömbök tömbje</span><span class="sxs-lookup"><span data-stu-id="aae47-172">array of time-based chunks of video into which hello metadata is chunked</span></span> |
| <span data-ttu-id="aae47-173">start</span><span class="sxs-lookup"><span data-stu-id="aae47-173">start</span></span> |<span data-ttu-id="aae47-174">a "ticks" töredéket kezdete</span><span class="sxs-lookup"><span data-stu-id="aae47-174">start time of a fragment in "ticks"</span></span> |
| <span data-ttu-id="aae47-175">Időtartam</span><span class="sxs-lookup"><span data-stu-id="aae47-175">duration</span></span> |<span data-ttu-id="aae47-176">a "ticks" töredéket hossza</span><span class="sxs-lookup"><span data-stu-id="aae47-176">length of a fragment in "ticks"</span></span> |
| <span data-ttu-id="aae47-177">interval</span><span class="sxs-lookup"><span data-stu-id="aae47-177">interval</span></span> |<span data-ttu-id="aae47-178">minden esemény belül töredéket adott hello időköz</span><span class="sxs-lookup"><span data-stu-id="aae47-178">interval of each event within hello given fragment</span></span> |
| <span data-ttu-id="aae47-179">események</span><span class="sxs-lookup"><span data-stu-id="aae47-179">events</span></span> |<span data-ttu-id="aae47-180">régiók tartalmazó tömb</span><span class="sxs-lookup"><span data-stu-id="aae47-180">array containing regions</span></span> |
| <span data-ttu-id="aae47-181">Régió</span><span class="sxs-lookup"><span data-stu-id="aae47-181">region</span></span> |<span data-ttu-id="aae47-182">szavakat vagy kifejezéseket objektumot képviselő észlelt</span><span class="sxs-lookup"><span data-stu-id="aae47-182">object representing detected words or phrases</span></span> |
| <span data-ttu-id="aae47-183">nyelv</span><span class="sxs-lookup"><span data-stu-id="aae47-183">language</span></span> |<span data-ttu-id="aae47-184">régión belül észlelt hello szöveg</span><span class="sxs-lookup"><span data-stu-id="aae47-184">language of hello text detected within a region</span></span> |
| <span data-ttu-id="aae47-185">Tájolás</span><span class="sxs-lookup"><span data-stu-id="aae47-185">orientation</span></span> |<span data-ttu-id="aae47-186">régión belül észlelt hello szöveg irányának</span><span class="sxs-lookup"><span data-stu-id="aae47-186">orientation of hello text detected within a region</span></span> |
| <span data-ttu-id="aae47-187">sorok</span><span class="sxs-lookup"><span data-stu-id="aae47-187">lines</span></span> |<span data-ttu-id="aae47-188">a tömb sornyi szöveget észlelt régión belül</span><span class="sxs-lookup"><span data-stu-id="aae47-188">array of lines of text detected within a region</span></span> |
| <span data-ttu-id="aae47-189">Szöveg</span><span class="sxs-lookup"><span data-stu-id="aae47-189">text</span></span> |<span data-ttu-id="aae47-190">hello tényleges szöveg</span><span class="sxs-lookup"><span data-stu-id="aae47-190">hello actual text</span></span> |

### <a name="json-output-example"></a><span data-ttu-id="aae47-191">JSON kimeneti példa</span><span class="sxs-lookup"><span data-stu-id="aae47-191">JSON output example</span></span>
<span data-ttu-id="aae47-192">hello alábbi példa a kimenetre hello általános videó információkat és tartalmaz több videó töredék.</span><span class="sxs-lookup"><span data-stu-id="aae47-192">hello following output example contains hello general video information and several video fragments.</span></span> <span data-ttu-id="aae47-193">Minden videó töredékben szereplő minden egyes régió OCR MP hello nyelv és a szöveg tájolás által észlelt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aae47-193">In every video fragment, it contains every region which is detected by OCR MP with hello language and its text orientation.</span></span> <span data-ttu-id="aae47-194">hello régió minden word sor ebben a régióban hello sor szöveget, hello sor helye és a sor minden word információkkal (word tartalmat, és adott abban, hogy) is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="aae47-194">hello region also contains every word line in this region with hello line’s text, hello line’s position, and every word information (word content, position and confidence) in this line.</span></span> <span data-ttu-id="aae47-195">hello az alábbiakban látható egy példa, és szeretnék bizonyos megjegyzéseket beágyazott helyezése</span><span class="sxs-lookup"><span data-stu-id="aae47-195">hello following is an example, and I put some comments inline.</span></span>

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // hello time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // hello detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including hello text and hello position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="net-sample-code"></a><span data-ttu-id="aae47-196">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="aae47-196">.NET sample code</span></span>

<span data-ttu-id="aae47-197">hello következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="aae47-197">hello following program shows how to:</span></span>

1. <span data-ttu-id="aae47-198">Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.</span><span class="sxs-lookup"><span data-stu-id="aae47-198">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="aae47-199">Hozzon létre egy feladatot OCR konfigurációs/előre definiált fájllal.</span><span class="sxs-lookup"><span data-stu-id="aae47-199">Create a job with an OCR configuration/preset file.</span></span>
3. <span data-ttu-id="aae47-200">Hello kimeneti JSON-fájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="aae47-200">Download hello output JSON files.</span></span> 
   
#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="aae47-201">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="aae47-201">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="aae47-202">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="aae47-202">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="aae47-203">Példa</span><span class="sxs-lookup"><span data-stu-id="aae47-203">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace OCR
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

                // Run hello OCR job.
                var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                            @"C:\supportFiles\OCR\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
            }

            static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My OCR Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My OCR Job");

                // Get a reference tooAzure Media OCR.
                string MediaProcessorName = "Azure Media OCR";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My OCR Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="aae47-204">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="aae47-204">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="aae47-205">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="aae47-205">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="aae47-206">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="aae47-206">Related links</span></span>
[<span data-ttu-id="aae47-207">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="aae47-207">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)


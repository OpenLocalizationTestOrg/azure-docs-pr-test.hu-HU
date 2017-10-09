---
title: "aaaDetect Arcfelismerés és az Azure Media Analytics Érzelemfelismerési |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toodetect néz és az Azure Media Analytics érzelmek."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/18/2017
ms.author: milanga;juliako;
ms.openlocfilehash: f58d81d82dde08a694cdb4d92c6bab6a40a9c157
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a><span data-ttu-id="1167c-103">Arcfelismerés és az Azure Media Analytics Érzelemfelismerési észlelése</span><span class="sxs-lookup"><span data-stu-id="1167c-103">Detect Face and Emotion with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="1167c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1167c-104">Overview</span></span>
<span data-ttu-id="1167c-105">Hello **Azure Media Arcfelismerési érzékelő** media processzor (MP) lehetővé teszi a toocount, nyomon követése típusú áthelyezések, és még a mérőműszer célközönség részvételét és reakciót arcfelismerést kifejezések keresztül.</span><span class="sxs-lookup"><span data-stu-id="1167c-105">hello **Azure Media Face Detector** media processor (MP) enables you toocount, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="1167c-106">Ez a szolgáltatás két funkciókat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1167c-106">This service contains two features:</span></span> 

* <span data-ttu-id="1167c-107">**Arcfelismerési észlelése**</span><span class="sxs-lookup"><span data-stu-id="1167c-107">**Face detection**</span></span>
  
    <span data-ttu-id="1167c-108">Arcfelismerési észlelési talál, és nyomon követi a videó emberi lapjaira.</span><span class="sxs-lookup"><span data-stu-id="1167c-108">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="1167c-109">Több lapokat észlelhető, és ezt követően nyomon követhetők egy JSON-fájl által visszaadott hello ideje és helye metaadatokkal körül, mozgás.</span><span class="sxs-lookup"><span data-stu-id="1167c-109">Multiple faces can be detected and subsequently be tracked as they move around, with hello time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="1167c-110">Nyomon követése, során a program megpróbálja toogive egy egységes azonosító toohello azonos szembesülhetnek, amíg hello személy van Navigálás a képernyőn, még akkor is, ha kényszerítő vagy röviden hagyja hello keret.</span><span class="sxs-lookup"><span data-stu-id="1167c-110">During tracking, it will attempt toogive a consistent ID toohello same face while hello person is moving around on screen, even if they are obstructed or briefly leave hello frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="1167c-111">Ez a szolgáltatás nem végez arcfelismerést.</span><span class="sxs-lookup"><span data-stu-id="1167c-111">This service does not perform facial recognition.</span></span> <span data-ttu-id="1167c-112">Személy hello keret hagyja, vagy a válik fedhetik túl hosszú kap egy új Azonosítót amikor azok tér vissza.</span><span class="sxs-lookup"><span data-stu-id="1167c-112">An individual who leaves hello frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="1167c-113">**Érzelemfelismerés**</span><span class="sxs-lookup"><span data-stu-id="1167c-113">**Emotion detection**</span></span>
  
    <span data-ttu-id="1167c-114">Érzelemfelismerés hello Arcfelismerési észlelési Media processzor ad vissza elemzés több érzelmi attribútum hello lapok észlel, például Boldogsága, sadness, félelem, utasítás és egyéb választható összetevőként.</span><span class="sxs-lookup"><span data-stu-id="1167c-114">Emotion Detection is an optional component of hello Face Detection Media Processor that returns analysis on multiple emotional attributes from hello faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

<span data-ttu-id="1167c-115">Hello **Azure Media Arcfelismerési érzékelő** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="1167c-115">hello **Azure Media Face Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="1167c-116">Ez a témakör kapcsolatos részleteket nyújt **Azure Media Arcfelismerési érzékelő** és bemutatja, hogyan toouse a Media Services SDK for .NET.</span><span class="sxs-lookup"><span data-stu-id="1167c-116">This topic gives details about  **Azure Media Face Detector** and shows how toouse it with Media Services SDK for .NET.</span></span>

## <a name="face-detector-input-files"></a><span data-ttu-id="1167c-117">A bemeneti fájlok érzékelő szembesülhetnek</span><span class="sxs-lookup"><span data-stu-id="1167c-117">Face Detector input files</span></span>
<span data-ttu-id="1167c-118">Videofájlok lejátszását.</span><span class="sxs-lookup"><span data-stu-id="1167c-118">Video files.</span></span> <span data-ttu-id="1167c-119">Jelenleg a következő formátumok hello támogatottak: MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="1167c-119">Currently, hello following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="face-detector-output-files"></a><span data-ttu-id="1167c-120">Szembesülhetnek érzékelő kimeneti fájlok</span><span class="sxs-lookup"><span data-stu-id="1167c-120">Face Detector output files</span></span>
<span data-ttu-id="1167c-121">hello arcfelismerési felderítését és a nyomon követési API biztosít magas pontosság arcfelismerési hely felderítését és a nyomkövetési, amely észlelni tudja a too64 emberi lapokat a videó be.</span><span class="sxs-lookup"><span data-stu-id="1167c-121">hello face detection and tracking API provides high precision face location detection and tracking that can detect up too64 human faces in a video.</span></span> <span data-ttu-id="1167c-122">Elülső lapok hello legjobb eredmények elérése érdekében ügyféloldali lapokat és kis lapok közben adja meg (kevesebb, mint vagy egyenlő too24x24 képpont) nem lehet olyan pontos.</span><span class="sxs-lookup"><span data-stu-id="1167c-122">Frontal faces provide hello best results, while side faces and small faces (less than or equal too24x24 pixels) might not be as accurate.</span></span>

<span data-ttu-id="1167c-123">hello észlelt és a nyomon követett lapokat a rendszer visszairányítja koordináták (bal, felső, szélességét és magasságát) jelző hello kép képpontban lapok hello helyét, valamint egy oldallal azonosító szám, amely jelzi, hello az, hogy egyes követését.</span><span class="sxs-lookup"><span data-stu-id="1167c-123">hello detected and tracked faces are returned with coordinates (left, top, width, and height) indicating hello location of faces in hello image in pixels, as well as a face ID number indicating hello tracking of that individual.</span></span> <span data-ttu-id="1167c-124">Arcfelismerési azonosítószámát esetén körülmények nagyon eséllyel fordulnak elő tooreset hello elülső arcfelismerési elvesztése vagy átfedésben hello keretében, néhány első hozzárendelt több azonosítók egyének eredményez.</span><span class="sxs-lookup"><span data-stu-id="1167c-124">Face ID numbers are prone tooreset under circumstances when hello frontal face is lost or overlapped in hello frame, resulting in some individuals getting assigned multiple IDs.</span></span>

## <span data-ttu-id="1167c-125"><a id="output_elements"></a>Hello kimeneti JSON-fájl elemeinek</span><span class="sxs-lookup"><span data-stu-id="1167c-125"><a id="output_elements"></a>Elements of hello output JSON file</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

<span data-ttu-id="1167c-126">Arcfelismerési érzékelő (ahol hello események törik fel, ha túl nagy elérték) töredezettsége (ahol az időalapú adattömbök hello metaadatait is osztható fel és letöltheti a csak találja), és a szegmentálási technikák használja.</span><span class="sxs-lookup"><span data-stu-id="1167c-126">Face Detector uses techniques of fragmentation (where hello metadata can be broken up in time-based chunks and you can download only what you need), and segmentation (where hello events are broken up in case they get too large).</span></span> <span data-ttu-id="1167c-127">Néhány egyszerű számítások segítségével hello adatok.</span><span class="sxs-lookup"><span data-stu-id="1167c-127">Some simple calculations can help you transform hello data.</span></span> <span data-ttu-id="1167c-128">Például, ha egy esemény használatába 6300 (ticks), egy időskálára 2997 (ticks/másodperc), és a 29,97 (keret/mp), majd képkockasebességhez:</span><span class="sxs-lookup"><span data-stu-id="1167c-128">For example, if an event started at 6300 (ticks), with a timescale of 2997 (ticks/sec) and framerate of 29.97 (frames/sec), then:</span></span>

* <span data-ttu-id="1167c-129">Start/időskálára = 2.1 másodperc</span><span class="sxs-lookup"><span data-stu-id="1167c-129">Start/Timescale = 2.1 seconds</span></span>
* <span data-ttu-id="1167c-130">X Framerate másodperc 63 keretek =</span><span class="sxs-lookup"><span data-stu-id="1167c-130">Seconds x Framerate = 63 frames</span></span>

## <a name="face-detection-input-and-output-example"></a><span data-ttu-id="1167c-131">Szembesülhetnek észlelési bemeneti és kimeneti példa</span><span class="sxs-lookup"><span data-stu-id="1167c-131">Face detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="1167c-132">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="1167c-132">Input video</span></span>
[<span data-ttu-id="1167c-133">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="1167c-133">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="1167c-134">A feladat konfigurációja (beállítás)</span><span class="sxs-lookup"><span data-stu-id="1167c-134">Task configuration (preset)</span></span>
<span data-ttu-id="1167c-135">A feladat létrehozásakor **Azure Media Arcfelismerési érzékelő**, meg kell adnia egy konfigurációs készletet.</span><span class="sxs-lookup"><span data-stu-id="1167c-135">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="1167c-136">a következő konfigurációs készlet hello folyamat csak arcfelismerési észlelése.</span><span class="sxs-lookup"><span data-stu-id="1167c-136">hello following configuration preset is just for face detection.</span></span>

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a><span data-ttu-id="1167c-137">Az attribútumok leírása</span><span class="sxs-lookup"><span data-stu-id="1167c-137">Attribute descriptions</span></span>
| <span data-ttu-id="1167c-138">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="1167c-138">Attribute name</span></span> | <span data-ttu-id="1167c-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="1167c-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1167c-140">Mód</span><span class="sxs-lookup"><span data-stu-id="1167c-140">Mode</span></span> |<span data-ttu-id="1167c-141">Gyors - feldolgozása gyors sebességét, de kevésbé pontos (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="1167c-141">Fast - fast processing speed, but less accurate (default).</span></span>|

### <a name="json-output"></a><span data-ttu-id="1167c-142">JSON kimeneti</span><span class="sxs-lookup"><span data-stu-id="1167c-142">JSON output</span></span>
<span data-ttu-id="1167c-143">a következő példa a JSON-kimenetét hello csonkolódott.</span><span class="sxs-lookup"><span data-stu-id="1167c-143">hello following example of JSON output was truncated.</span></span>

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

## <a name="emotion-detection-input-and-output-example"></a><span data-ttu-id="1167c-144">Érzelemfelismerés bemeneti és kimeneti példa</span><span class="sxs-lookup"><span data-stu-id="1167c-144">Emotion detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="1167c-145">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="1167c-145">Input video</span></span>
[<span data-ttu-id="1167c-146">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="1167c-146">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="1167c-147">A feladat konfigurációja (beállítás)</span><span class="sxs-lookup"><span data-stu-id="1167c-147">Task configuration (preset)</span></span>
<span data-ttu-id="1167c-148">A feladat létrehozásakor **Azure Media Arcfelismerési érzékelő**, meg kell adnia egy konfigurációs készletet.</span><span class="sxs-lookup"><span data-stu-id="1167c-148">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="1167c-149">a következő konfigurációs készlet hello toocreate JSON hello érzelemfelismerés alapján határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1167c-149">hello following configuration preset specifies toocreate JSON based on hello emotion detection.</span></span>

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a><span data-ttu-id="1167c-150">Az attribútumok leírása</span><span class="sxs-lookup"><span data-stu-id="1167c-150">Attribute descriptions</span></span>
| <span data-ttu-id="1167c-151">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="1167c-151">Attribute name</span></span> | <span data-ttu-id="1167c-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="1167c-152">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1167c-153">Mód</span><span class="sxs-lookup"><span data-stu-id="1167c-153">Mode</span></span> |<span data-ttu-id="1167c-154">Lapok: Csak szembesülhetnek észlelése.</span><span class="sxs-lookup"><span data-stu-id="1167c-154">Faces: Only face detection.</span></span><br/><span data-ttu-id="1167c-155">PerFaceEmotion: Visszatérési érzelemfelismerési egymástól függetlenül az egyes arcfelismerési észlelése.</span><span class="sxs-lookup"><span data-stu-id="1167c-155">PerFaceEmotion: Return emotion independently for each face detection.</span></span><br/><span data-ttu-id="1167c-156">AggregateEmotion: Minden lap keretében átlagos érzelemfelismerési visszatérési értékei.</span><span class="sxs-lookup"><span data-stu-id="1167c-156">AggregateEmotion: Return average emotion values for all faces in frame.</span></span> |
| <span data-ttu-id="1167c-157">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="1167c-157">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="1167c-158">Ha a kiválasztott AggregateEmotion módot használja.</span><span class="sxs-lookup"><span data-stu-id="1167c-158">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="1167c-159">Adja meg videó használt tooproduce hello hosszát minden összesített eredmény.</span><span class="sxs-lookup"><span data-stu-id="1167c-159">Specifies hello length of video used tooproduce each aggregate result, in milliseconds.</span></span> |
| <span data-ttu-id="1167c-160">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="1167c-160">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="1167c-161">Ha a kiválasztott AggregateEmotion módot használja.</span><span class="sxs-lookup"><span data-stu-id="1167c-161">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="1167c-162">Határozza meg, milyen gyakoriság tooproduce összesített eredmények.</span><span class="sxs-lookup"><span data-stu-id="1167c-162">Specifies with what frequency tooproduce aggregate results.</span></span> |

#### <a name="aggregate-defaults"></a><span data-ttu-id="1167c-163">Összesített alapértelmezései</span><span class="sxs-lookup"><span data-stu-id="1167c-163">Aggregate defaults</span></span>
<span data-ttu-id="1167c-164">Alább javasoltak hello összesítő és időköz beállítások értékeit.</span><span class="sxs-lookup"><span data-stu-id="1167c-164">Below are recommended values for hello aggregate window and interval settings.</span></span> <span data-ttu-id="1167c-165">AggregateEmotionWindowMs hosszabb, mint AggregateEmotionIntervalMs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1167c-165">AggregateEmotionWindowMs should be longer than AggregateEmotionIntervalMs.</span></span>

|| <span data-ttu-id="1167c-166">Alapértelmezett (s)</span><span class="sxs-lookup"><span data-stu-id="1167c-166">Defaults(s)</span></span> | <span data-ttu-id="1167c-167">Min(s)</span><span class="sxs-lookup"><span data-stu-id="1167c-167">Min(s)</span></span> | <span data-ttu-id="1167c-168">Max(s)</span><span class="sxs-lookup"><span data-stu-id="1167c-168">Max(s)</span></span> |
|--- | --- | --- | --- |
| <span data-ttu-id="1167c-169">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="1167c-169">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="1167c-170">0.5</span><span class="sxs-lookup"><span data-stu-id="1167c-170">0.5</span></span> |<span data-ttu-id="1167c-171">2</span><span class="sxs-lookup"><span data-stu-id="1167c-171">2</span></span> |<span data-ttu-id="1167c-172">0.25</span><span class="sxs-lookup"><span data-stu-id="1167c-172">0.25</span></span>|
| <span data-ttu-id="1167c-173">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="1167c-173">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="1167c-174">0.5</span><span class="sxs-lookup"><span data-stu-id="1167c-174">0.5</span></span> |<span data-ttu-id="1167c-175">1</span><span class="sxs-lookup"><span data-stu-id="1167c-175">1</span></span> |<span data-ttu-id="1167c-176">0.25</span><span class="sxs-lookup"><span data-stu-id="1167c-176">0.25</span></span>|

### <a name="json-output"></a><span data-ttu-id="1167c-177">JSON kimeneti</span><span class="sxs-lookup"><span data-stu-id="1167c-177">JSON output</span></span>
<span data-ttu-id="1167c-178">Összesített érzelemfelismerési (csonkolt) a kimeneti JSON:</span><span class="sxs-lookup"><span data-stu-id="1167c-178">JSON output for aggregate emotion (truncated):</span></span>

    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,

## <a name="limitations"></a><span data-ttu-id="1167c-179">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="1167c-179">Limitations</span></span>
* <span data-ttu-id="1167c-180">hello támogatott bemeneti videó formátumnak tartalmaznia kell MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="1167c-180">hello supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="1167c-181">hello észlelhető arcfelismerési mérete tartománya 24 x 24 too2048x2048 képpont.</span><span class="sxs-lookup"><span data-stu-id="1167c-181">hello detectable face size range is 24x24 too2048x2048 pixels.</span></span> <span data-ttu-id="1167c-182">hello lapokat a tartományon kívül nem fogja észlelni.</span><span class="sxs-lookup"><span data-stu-id="1167c-182">hello faces out of this range will not be detected.</span></span>
* <span data-ttu-id="1167c-183">Minden videó visszaadott lapok maximális számát hello esetén 64.</span><span class="sxs-lookup"><span data-stu-id="1167c-183">For each video, hello maximum number of faces returned is 64.</span></span>
* <span data-ttu-id="1167c-184">Bizonyos lapok tootechnical kihívást; miatt nem észlelhető például a nagyon nagy arcfelismerési szögek (head-jelentő), és nagy hangelnyelés.</span><span class="sxs-lookup"><span data-stu-id="1167c-184">Some faces may not be detected due tootechnical challenges; e.g. very large face angles (head-pose), and large occlusion.</span></span> <span data-ttu-id="1167c-185">Elülső és közelében elülső lapok rendelkezik hello legjobb eredmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="1167c-185">Frontal and near-frontal faces have hello best results.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="1167c-186">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="1167c-186">.NET sample code</span></span>

<span data-ttu-id="1167c-187">hello következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="1167c-187">hello following program shows how to:</span></span>

1. <span data-ttu-id="1167c-188">Hozzon létre egy eszközt, és töltse fel a médiafájl hello objektumba.</span><span class="sxs-lookup"><span data-stu-id="1167c-188">Create an asset and upload a media file into hello asset.</span></span>
2. <span data-ttu-id="1167c-189">Hozzon létre egy feladatot a json-készlet a következő hello tartalmazó konfigurációs fájl alapján arcfelismerési észlelési feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="1167c-189">Create a job with a face detection task based on a configuration file that contains hello following json preset.</span></span> 
   
        {
            "version": "1.0"
        }
3. <span data-ttu-id="1167c-190">Hello kimeneti JSON-fájlok letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="1167c-190">Download hello output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="1167c-191">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1167c-191">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="1167c-192">A fejlesztési környezet kialakítása és feltöltése hello app.config fájl kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="1167c-192">Set up your development environment and populate hello app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="1167c-193">Példa</span><span class="sxs-lookup"><span data-stu-id="1167c-193">Example</span></span>

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace FaceDetection
    {
        class Program
        {
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

                // Run hello FaceDetection job.
                var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\FaceDetection\config.json");

                // Download hello job output asset.
                DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
            }

            static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload hello input media file toostorage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Face Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Face Detection Job");

                // Get a reference tooAzure Media Face Detector.
                string MediaProcessorName = "Azure Media Face Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from hello specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with hello encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Face Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify hello input asset.
                task.InputAssets.Add(asset);

                // Add an output asset toocontain hello results of hello job.
                task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="1167c-194">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="1167c-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1167c-195">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="1167c-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="1167c-196">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="1167c-196">Related links</span></span>
[<span data-ttu-id="1167c-197">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="1167c-197">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="1167c-198">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="1167c-198">Azure Media Analytics demos</span></span>](http://amslabs.azurewebsites.net/demos/Analytics.html)


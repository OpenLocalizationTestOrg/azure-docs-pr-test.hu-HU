---
title: "Arcfelismerés és az Azure Media Analytics Érzelemfelismerési észlelése |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan lapokat és az Azure Media Analytics érzelmek észleléséhez."
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
ms.openlocfilehash: d7f3bc6c0d21db7adbb0c16c752d4ce49e99da5a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a><span data-ttu-id="8c714-103">Arcfelismerés és az Azure Media Analytics Érzelemfelismerési észlelése</span><span class="sxs-lookup"><span data-stu-id="8c714-103">Detect Face and Emotion with Azure Media Analytics</span></span>
## <a name="overview"></a><span data-ttu-id="8c714-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8c714-104">Overview</span></span>
<span data-ttu-id="8c714-105">A **Azure Media Arcfelismerési érzékelő** media processzor (MP) lehetővé teszi a száma, nyomon követésére típusú áthelyezések, és akkor is fel tudja mérni a célközönség részvételét és reakciót arcfelismerést kifejezések keresztül.</span><span class="sxs-lookup"><span data-stu-id="8c714-105">The **Azure Media Face Detector** media processor (MP) enables you to count, track movements, and even gauge audience participation and reaction via facial expressions.</span></span> <span data-ttu-id="8c714-106">Ez a szolgáltatás két funkciókat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8c714-106">This service contains two features:</span></span> 

* <span data-ttu-id="8c714-107">**Arcfelismerési észlelése**</span><span class="sxs-lookup"><span data-stu-id="8c714-107">**Face detection**</span></span>
  
    <span data-ttu-id="8c714-108">Arcfelismerési észlelési talál, és nyomon követi a videó emberi lapjaira.</span><span class="sxs-lookup"><span data-stu-id="8c714-108">Face detection finds and tracks human faces within a video.</span></span> <span data-ttu-id="8c714-109">Több lapokat észlelhető, és ezt követően nyomon követheti az adott vissza egy JSON-fájlban ideje és helye metaadatokkal körül, mozgás.</span><span class="sxs-lookup"><span data-stu-id="8c714-109">Multiple faces can be detected and subsequently be tracked as they move around, with the time and location metadata returned in a JSON file.</span></span> <span data-ttu-id="8c714-110">Nyomon követése, során a program megpróbálja adjon egységes azonosító ugyanazon felületére, amíg a személy van Navigálás a képernyőn, még akkor is, ha kényszerítő vagy röviden hagyja a keret.</span><span class="sxs-lookup"><span data-stu-id="8c714-110">During tracking, it will attempt to give a consistent ID to the same face while the person is moving around on screen, even if they are obstructed or briefly leave the frame.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="8c714-111">Ez a szolgáltatás nem végez arcfelismerést.</span><span class="sxs-lookup"><span data-stu-id="8c714-111">This service does not perform facial recognition.</span></span> <span data-ttu-id="8c714-112">Személy elhagyja a keret vagy a válik fedhetik túl hosszú kap egy új Azonosítót amikor azok tér vissza.</span><span class="sxs-lookup"><span data-stu-id="8c714-112">An individual who leaves the frame or becomes obstructed for too long will be given a new ID when they return.</span></span>
  > 
  > 
* <span data-ttu-id="8c714-113">**Érzelemfelismerés**</span><span class="sxs-lookup"><span data-stu-id="8c714-113">**Emotion detection**</span></span>
  
    <span data-ttu-id="8c714-114">Érzelemfelismerés Arcfelismerési észlelési adathordozó-processzor elemzés több érzelmi attribútum vissza a lapok észlel, például Boldogsága, sadness, félelem, utasítás és egyéb választható összetevőként.</span><span class="sxs-lookup"><span data-stu-id="8c714-114">Emotion Detection is an optional component of the Face Detection Media Processor that returns analysis on multiple emotional attributes from the faces detected, including happiness, sadness, fear, anger, and more.</span></span> 

<span data-ttu-id="8c714-115">A **Azure Media Arcfelismerési érzékelő** felügyeleti csomag jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="8c714-115">The **Azure Media Face Detector** MP is currently in Preview.</span></span>

<span data-ttu-id="8c714-116">Ez a témakör kapcsolatos részleteket nyújt **Azure Media Arcfelismerési érzékelő** és a .NET-keretrendszerhez készült Media Services SDK-val való használatát ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8c714-116">This topic gives details about  **Azure Media Face Detector** and shows how to use it with Media Services SDK for .NET.</span></span>

## <a name="face-detector-input-files"></a><span data-ttu-id="8c714-117">A bemeneti fájlok érzékelő szembesülhetnek</span><span class="sxs-lookup"><span data-stu-id="8c714-117">Face Detector input files</span></span>
<span data-ttu-id="8c714-118">Videofájlok lejátszását.</span><span class="sxs-lookup"><span data-stu-id="8c714-118">Video files.</span></span> <span data-ttu-id="8c714-119">Jelenleg a következő formátumok használhatók: MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="8c714-119">Currently, the following formats are supported: MP4, MOV, and WMV.</span></span>

## <a name="face-detector-output-files"></a><span data-ttu-id="8c714-120">Szembesülhetnek érzékelő kimeneti fájlok</span><span class="sxs-lookup"><span data-stu-id="8c714-120">Face Detector output files</span></span>
<span data-ttu-id="8c714-121">A tapasztalt felderítését és a nyomon követési API magas pontosság arcfelismerési hely észlelési és követési, amely észlelni tudja a videó legfeljebb 64 emberi lapok biztosít.</span><span class="sxs-lookup"><span data-stu-id="8c714-121">The face detection and tracking API provides high precision face location detection and tracking that can detect up to 64 human faces in a video.</span></span> <span data-ttu-id="8c714-122">Elülső lapok nyújtanak a legjobb eredmények elérése érdekében, közben ügyféloldali lapok és kis (legfeljebb 24 x 24 képpont) lapok nem feltétlenül legpontosabb.</span><span class="sxs-lookup"><span data-stu-id="8c714-122">Frontal faces provide the best results, while side faces and small faces (less than or equal to 24x24 pixels) might not be as accurate.</span></span>

<span data-ttu-id="8c714-123">Az észlelt és a nyomon követett lapok koordináták (bal, felső, szélességét és magasságát) küld vissza a rendszer jelzi a lapok képpont, valamint egy oldallal azonosítószámát, jelezve, hogy egyes követését a lemezkép helyét.</span><span class="sxs-lookup"><span data-stu-id="8c714-123">The detected and tracked faces are returned with coordinates (left, top, width, and height) indicating the location of faces in the image in pixels, as well as a face ID number indicating the tracking of that individual.</span></span> <span data-ttu-id="8c714-124">Arcfelismerési azonosítószámát nagyon eséllyel fordulnak elő a elülső arcfelismerési ellopása vagy átfedésben vannak a keretében körülmények alaphelyzetbe néhány első hozzárendelt több azonosítók egyének eredményez.</span><span class="sxs-lookup"><span data-stu-id="8c714-124">Face ID numbers are prone to reset under circumstances when the frontal face is lost or overlapped in the frame, resulting in some individuals getting assigned multiple IDs.</span></span>

## <span data-ttu-id="8c714-125"><a id="output_elements"></a>A kimeneti JSON-fájl elemeinek</span><span class="sxs-lookup"><span data-stu-id="8c714-125"><a id="output_elements"></a>Elements of the output JSON file</span></span>

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

<span data-ttu-id="8c714-126">Arcfelismerési érzékelő (ahol az események törik fel, ha túl nagy elérték) töredezettsége (ahol a metaadatokat az időalapú adattömbök is osztható fel és letöltheti a csak találja), és a szegmentálási technikák használja.</span><span class="sxs-lookup"><span data-stu-id="8c714-126">Face Detector uses techniques of fragmentation (where the metadata can be broken up in time-based chunks and you can download only what you need), and segmentation (where the events are broken up in case they get too large).</span></span> <span data-ttu-id="8c714-127">Néhány egyszerű számítások segítségével átalakíthatja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="8c714-127">Some simple calculations can help you transform the data.</span></span> <span data-ttu-id="8c714-128">Például, ha egy esemény használatába 6300 (ticks), egy időskálára 2997 (ticks/másodperc), és a 29,97 (keret/mp), majd képkockasebességhez:</span><span class="sxs-lookup"><span data-stu-id="8c714-128">For example, if an event started at 6300 (ticks), with a timescale of 2997 (ticks/sec) and framerate of 29.97 (frames/sec), then:</span></span>

* <span data-ttu-id="8c714-129">Start/időskálára = 2.1 másodperc</span><span class="sxs-lookup"><span data-stu-id="8c714-129">Start/Timescale = 2.1 seconds</span></span>
* <span data-ttu-id="8c714-130">X Framerate másodperc 63 keretek =</span><span class="sxs-lookup"><span data-stu-id="8c714-130">Seconds x Framerate = 63 frames</span></span>

## <a name="face-detection-input-and-output-example"></a><span data-ttu-id="8c714-131">Szembesülhetnek észlelési bemeneti és kimeneti példa</span><span class="sxs-lookup"><span data-stu-id="8c714-131">Face detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="8c714-132">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="8c714-132">Input video</span></span>
[<span data-ttu-id="8c714-133">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="8c714-133">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="8c714-134">A feladat konfigurációja (beállítás)</span><span class="sxs-lookup"><span data-stu-id="8c714-134">Task configuration (preset)</span></span>
<span data-ttu-id="8c714-135">A feladat létrehozásakor **Azure Media Arcfelismerési érzékelő**, meg kell adnia egy konfigurációs készletet.</span><span class="sxs-lookup"><span data-stu-id="8c714-135">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="8c714-136">A következő konfigurációs készlet folyamat csak arcfelismerési észlelése.</span><span class="sxs-lookup"><span data-stu-id="8c714-136">The following configuration preset is just for face detection.</span></span>

    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }

#### <a name="attribute-descriptions"></a><span data-ttu-id="8c714-137">Az attribútumok leírása</span><span class="sxs-lookup"><span data-stu-id="8c714-137">Attribute descriptions</span></span>
| <span data-ttu-id="8c714-138">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="8c714-138">Attribute name</span></span> | <span data-ttu-id="8c714-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="8c714-139">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8c714-140">Mód</span><span class="sxs-lookup"><span data-stu-id="8c714-140">Mode</span></span> |<span data-ttu-id="8c714-141">Gyors - feldolgozása gyors sebességét, de kevésbé pontos (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="8c714-141">Fast - fast processing speed, but less accurate (default).</span></span>|

### <a name="json-output"></a><span data-ttu-id="8c714-142">JSON kimeneti</span><span class="sxs-lookup"><span data-stu-id="8c714-142">JSON output</span></span>
<span data-ttu-id="8c714-143">A következő példa a JSON-kimenetét csonkolódott.</span><span class="sxs-lookup"><span data-stu-id="8c714-143">The following example of JSON output was truncated.</span></span>

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

## <a name="emotion-detection-input-and-output-example"></a><span data-ttu-id="8c714-144">Érzelemfelismerés bemeneti és kimeneti példa</span><span class="sxs-lookup"><span data-stu-id="8c714-144">Emotion detection input and output example</span></span>
### <a name="input-video"></a><span data-ttu-id="8c714-145">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="8c714-145">Input video</span></span>
[<span data-ttu-id="8c714-146">A bemeneti videó</span><span class="sxs-lookup"><span data-stu-id="8c714-146">Input Video</span></span>](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a><span data-ttu-id="8c714-147">A feladat konfigurációja (beállítás)</span><span class="sxs-lookup"><span data-stu-id="8c714-147">Task configuration (preset)</span></span>
<span data-ttu-id="8c714-148">A feladat létrehozásakor **Azure Media Arcfelismerési érzékelő**, meg kell adnia egy konfigurációs készletet.</span><span class="sxs-lookup"><span data-stu-id="8c714-148">When creating a task with **Azure Media Face Detector**, you must specify a configuration preset.</span></span> <span data-ttu-id="8c714-149">A következő konfigurációs beállítás határozza meg a érzelemfelismerés alapján JSON létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8c714-149">The following configuration preset specifies to create JSON based on the emotion detection.</span></span>

    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


#### <a name="attribute-descriptions"></a><span data-ttu-id="8c714-150">Az attribútumok leírása</span><span class="sxs-lookup"><span data-stu-id="8c714-150">Attribute descriptions</span></span>
| <span data-ttu-id="8c714-151">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="8c714-151">Attribute name</span></span> | <span data-ttu-id="8c714-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="8c714-152">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8c714-153">Mód</span><span class="sxs-lookup"><span data-stu-id="8c714-153">Mode</span></span> |<span data-ttu-id="8c714-154">Lapok: Csak szembesülhetnek észlelése.</span><span class="sxs-lookup"><span data-stu-id="8c714-154">Faces: Only face detection.</span></span><br/><span data-ttu-id="8c714-155">PerFaceEmotion: Visszatérési érzelemfelismerési egymástól függetlenül az egyes arcfelismerési észlelése.</span><span class="sxs-lookup"><span data-stu-id="8c714-155">PerFaceEmotion: Return emotion independently for each face detection.</span></span><br/><span data-ttu-id="8c714-156">AggregateEmotion: Minden lap keretében átlagos érzelemfelismerési visszatérési értékei.</span><span class="sxs-lookup"><span data-stu-id="8c714-156">AggregateEmotion: Return average emotion values for all faces in frame.</span></span> |
| <span data-ttu-id="8c714-157">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="8c714-157">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="8c714-158">Ha a kiválasztott AggregateEmotion módot használja.</span><span class="sxs-lookup"><span data-stu-id="8c714-158">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="8c714-159">Megadja azt az időtartamot, ezredmásodpercben minden összesített eredmény létrehozásához használt videó.</span><span class="sxs-lookup"><span data-stu-id="8c714-159">Specifies the length of video used to produce each aggregate result, in milliseconds.</span></span> |
| <span data-ttu-id="8c714-160">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="8c714-160">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="8c714-161">Ha a kiválasztott AggregateEmotion módot használja.</span><span class="sxs-lookup"><span data-stu-id="8c714-161">Use if AggregateEmotion mode selected.</span></span> <span data-ttu-id="8c714-162">Itt adhatja meg, milyen gyakorisággal összesített eredmények eredményezett.</span><span class="sxs-lookup"><span data-stu-id="8c714-162">Specifies with what frequency to produce aggregate results.</span></span> |

#### <a name="aggregate-defaults"></a><span data-ttu-id="8c714-163">Összesített alapértelmezései</span><span class="sxs-lookup"><span data-stu-id="8c714-163">Aggregate defaults</span></span>
<span data-ttu-id="8c714-164">Alább összesített ablakot, és időköz beállítások értékei használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="8c714-164">Below are recommended values for the aggregate window and interval settings.</span></span> <span data-ttu-id="8c714-165">AggregateEmotionWindowMs hosszabb, mint AggregateEmotionIntervalMs kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8c714-165">AggregateEmotionWindowMs should be longer than AggregateEmotionIntervalMs.</span></span>

|| <span data-ttu-id="8c714-166">Alapértelmezett (s)</span><span class="sxs-lookup"><span data-stu-id="8c714-166">Defaults(s)</span></span> | <span data-ttu-id="8c714-167">Min(s)</span><span class="sxs-lookup"><span data-stu-id="8c714-167">Min(s)</span></span> | <span data-ttu-id="8c714-168">Max(s)</span><span class="sxs-lookup"><span data-stu-id="8c714-168">Max(s)</span></span> |
|--- | --- | --- | --- |
| <span data-ttu-id="8c714-169">AggregateEmotionWindowMs</span><span class="sxs-lookup"><span data-stu-id="8c714-169">AggregateEmotionWindowMs</span></span> |<span data-ttu-id="8c714-170">0.5</span><span class="sxs-lookup"><span data-stu-id="8c714-170">0.5</span></span> |<span data-ttu-id="8c714-171">2</span><span class="sxs-lookup"><span data-stu-id="8c714-171">2</span></span> |<span data-ttu-id="8c714-172">0.25</span><span class="sxs-lookup"><span data-stu-id="8c714-172">0.25</span></span>|
| <span data-ttu-id="8c714-173">AggregateEmotionIntervalMs</span><span class="sxs-lookup"><span data-stu-id="8c714-173">AggregateEmotionIntervalMs</span></span> |<span data-ttu-id="8c714-174">0.5</span><span class="sxs-lookup"><span data-stu-id="8c714-174">0.5</span></span> |<span data-ttu-id="8c714-175">1</span><span class="sxs-lookup"><span data-stu-id="8c714-175">1</span></span> |<span data-ttu-id="8c714-176">0.25</span><span class="sxs-lookup"><span data-stu-id="8c714-176">0.25</span></span>|

### <a name="json-output"></a><span data-ttu-id="8c714-177">JSON kimeneti</span><span class="sxs-lookup"><span data-stu-id="8c714-177">JSON output</span></span>
<span data-ttu-id="8c714-178">Összesített érzelemfelismerési (csonkolt) a kimeneti JSON:</span><span class="sxs-lookup"><span data-stu-id="8c714-178">JSON output for aggregate emotion (truncated):</span></span>

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

## <a name="limitations"></a><span data-ttu-id="8c714-179">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="8c714-179">Limitations</span></span>
* <span data-ttu-id="8c714-180">A támogatott bemeneti videó formátumnak tartalmaznia kell MP4 MOV és WMV.</span><span class="sxs-lookup"><span data-stu-id="8c714-180">The supported input video formats include MP4, MOV, and WMV.</span></span>
* <span data-ttu-id="8c714-181">A észlelhető arcfelismerési mérete tartománya 24 x 24 a 2048 x 2048 képpontban megadva.</span><span class="sxs-lookup"><span data-stu-id="8c714-181">The detectable face size range is 24x24 to 2048x2048 pixels.</span></span> <span data-ttu-id="8c714-182">A tartományon kívül esik a lapok nem fogja észlelni.</span><span class="sxs-lookup"><span data-stu-id="8c714-182">The faces out of this range will not be detected.</span></span>
* <span data-ttu-id="8c714-183">Minden egyes videó visszaadott oldalak maximális számának 64 esetén.</span><span class="sxs-lookup"><span data-stu-id="8c714-183">For each video, the maximum number of faces returned is 64.</span></span>
* <span data-ttu-id="8c714-184">Bizonyos lapok műszaki akadályok; miatt nem észlelhető például a nagyon nagy arcfelismerési szögek (head-jelentő), és nagy hangelnyelés.</span><span class="sxs-lookup"><span data-stu-id="8c714-184">Some faces may not be detected due to technical challenges; e.g. very large face angles (head-pose), and large occlusion.</span></span> <span data-ttu-id="8c714-185">Elülső és közelében elülső lapok van a legjobb eredmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="8c714-185">Frontal and near-frontal faces have the best results.</span></span>

## <a name="net-sample-code"></a><span data-ttu-id="8c714-186">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="8c714-186">.NET sample code</span></span>

<span data-ttu-id="8c714-187">A következő program bemutatja hogyan:</span><span class="sxs-lookup"><span data-stu-id="8c714-187">The following program shows how to:</span></span>

1. <span data-ttu-id="8c714-188">Hozzon létre egy eszközt, és adathordozó-fájl feltöltése az objektumba.</span><span class="sxs-lookup"><span data-stu-id="8c714-188">Create an asset and upload a media file into the asset.</span></span>
2. <span data-ttu-id="8c714-189">Hozzon létre egy feladatot a következő json-készletet tartalmazó konfigurációs fájl alapján arcfelismerési észlelési feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="8c714-189">Create a job with a face detection task based on a configuration file that contains the following json preset.</span></span> 
   
        {
            "version": "1.0"
        }
3. <span data-ttu-id="8c714-190">A kimeneti JSON-fájlok letöltésére.</span><span class="sxs-lookup"><span data-stu-id="8c714-190">Download the output JSON files.</span></span> 

#### <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="8c714-191">Egy Visual Studio-projekt létrehozása és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8c714-191">Create and configure a Visual Studio project</span></span>

<span data-ttu-id="8c714-192">Állítsa be a fejlesztési környezetet, és töltse fel az app.config fájlt a kapcsolatadatokkal a [.NET-keretrendszerrel történő Media Services-fejlesztést](media-services-dotnet-how-to-use.md) ismertető dokumentumban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="8c714-192">Set up your development environment and populate the app.config file with connection information, as described in [Media Services development with .NET](media-services-dotnet-how-to-use.md).</span></span> 

#### <a name="example"></a><span data-ttu-id="8c714-193">Példa</span><span class="sxs-lookup"><span data-stu-id="8c714-193">Example</span></span>

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

                // Run the FaceDetection job.
                var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                            @"C:\supportFiles\FaceDetection\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
            }

            static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Face Detection Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Face Detection Job");

                // Get a reference to Azure Media Face Detector.
                string MediaProcessorName = "Azure Media Face Detector";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Face Detection Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="8c714-194">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="8c714-194">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="8c714-195">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="8c714-195">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="8c714-196">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="8c714-196">Related links</span></span>
[<span data-ttu-id="8c714-197">Azure Media Services elemző áttekintése</span><span class="sxs-lookup"><span data-stu-id="8c714-197">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="8c714-198">Az Azure Médiaelemzés bemutatók</span><span class="sxs-lookup"><span data-stu-id="8c714-198">Azure Media Analytics demos</span></span>](http://amslabs.azurewebsites.net/demos/Analytics.html)


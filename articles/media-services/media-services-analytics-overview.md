---
title: aaaMedia Analytics hello Media Services platformon |} Microsoft Docs
description: "A Médiaelemzés beszéd- és számítógép stratégiai szolgáltatásának vállalati méretezés, a megfelelőségi, a biztonsági és a globális reach gyűjteménye public Preview – áttekintés"
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c56e3781-8510-4f7f-b5ff-a218c1bb6f4c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2017
ms.author: milanga;juliako;johndeu
ms.openlocfilehash: 7545f0532d7618164ebe65e2f4232c5f63453cfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-analytics-on-hello-media-services-platform"></a><span data-ttu-id="35da6-103">Media Analytics hello Media Services platform</span><span class="sxs-lookup"><span data-stu-id="35da6-103">Media Analytics on hello Media Services platform</span></span>
## <a name="overview"></a><span data-ttu-id="35da6-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="35da6-104">Overview</span></span>
<span data-ttu-id="35da6-105">Több szervezet használja videó mint előnyben részesített közepes tootrain hello az alkalmazottak számára, azok az ügyfelek és a dokumentum üzleti funkciók kódolása.</span><span class="sxs-lookup"><span data-stu-id="35da6-105">More organizations are using video as hello preferred medium tootrain their employees, engage their customers, and document business functions.</span></span> <span data-ttu-id="35da6-106">A felhőalapú megoldások tartalmaz egy módja toostore adatfolyamként, és nagy médiafájlokat eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="35da6-106">Cloud computing provides a way toostore, stream, and access these large media files.</span></span> <span data-ttu-id="35da6-107">De videotartalom könyvtárának a vállalat növekedésével egyaránt hatékony elemzések hello tartalom kibontása eszközt kell.</span><span class="sxs-lookup"><span data-stu-id="35da6-107">But as a company's library of video content grows, it needs an equally effective means of extracting insights from hello content.</span></span> 

<span data-ttu-id="35da6-108">tooaddress egyre növekvő igénynek, Azure Media Services kínál az Azure Médiaelemzés használatával.</span><span class="sxs-lookup"><span data-stu-id="35da6-108">tooaddress this growing need, Azure Media Services offers Azure Media Analytics.</span></span> <span data-ttu-id="35da6-109">Médiaelemzés beszéd- és vizuális összetevők gyűjteménye, amely egyszerűbbé teszi a szervezetek és vállalatok számára tooderive gyakorlatban használható elemzések készítsenek.</span><span class="sxs-lookup"><span data-stu-id="35da6-109">Media Analytics is a collection of speech and vision components that makes it easier for organizations and enterprises tooderive actionable insights from their video files.</span></span> <span data-ttu-id="35da6-110">Media Analytics hello alapösszetevőket Media Services platform használatával készültek, kezelni tud napon az egyik léptékű feldolgozás adathordozó.</span><span class="sxs-lookup"><span data-stu-id="35da6-110">Built by using hello core Media Services platform components, Media Analytics can handle media processing at scale on day one.</span></span>

<span data-ttu-id="35da6-111">Media Analytics fejlesztők is gyorsan vonja fejlett videó funkciói alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="35da6-111">With Media Analytics, developers can quickly bring advanced video functionality into applications.</span></span> <span data-ttu-id="35da6-112">Vállalati környezetben nyújtja a hello teljes méretezés, a megfelelőségi, a biztonsági és a globális reach szükséges nagy méretű szervezeteknek.</span><span class="sxs-lookup"><span data-stu-id="35da6-112">It provides enterprise environments with hello full scale, compliance, security, and global reach required by large organizations.</span></span>

<span data-ttu-id="35da6-113">hello alábbi ábrán látható Médiaelemzés és egyéb hello Media Services platform fontosabb részei.</span><span class="sxs-lookup"><span data-stu-id="35da6-113">hello following diagram shows Media Analytics and other major parts of hello Media Services platform.</span></span> 

![VoD-munkafolyamat](./media/media-services-analytics-overview/media-services-analytics-overview01.png)

<span data-ttu-id="35da6-115">A Médiaelemzés médiafeldolgozói MP4- vagy JSON-fájlokat hoznak létre.</span><span class="sxs-lookup"><span data-stu-id="35da6-115">Media Analytics media processors produce MP4 files or JSON files.</span></span> <span data-ttu-id="35da6-116">Egy media processzor MP4-fájlokat hoz létre, ha hello fájl fokozatosan lehet letölteni.</span><span class="sxs-lookup"><span data-stu-id="35da6-116">If a media processor produces an MP4 file, you can progressively download hello file.</span></span> <span data-ttu-id="35da6-117">Egy media processzor JSON-fájlt hoz létre, ha az Azure Blob storage letöltheti hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="35da6-117">If a media processor produces a JSON file, you can download hello file from Azure Blob storage.</span></span> 

## <a name="media-analytics-services"></a><span data-ttu-id="35da6-118">Médiaelemzés-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="35da6-118">Media Analytics services</span></span>

### <a name="indexer"></a><span data-ttu-id="35da6-119">Indexelő</span><span class="sxs-lookup"><span data-stu-id="35da6-119">Indexer</span></span>
<span data-ttu-id="35da6-120">Az Azure Media Indexer hogy tartalom kereshető és készítése kódolt-feliratok követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="35da6-120">With Azure Media Indexer, you can make content searchable and generate closed-captioning tracks.</span></span> <span data-ttu-id="35da6-121">Összehasonlított toohello előző verzió, Azure Media Indexer 2 Preview rendelkezik gyorsabban indexelési és szélesebb körű nyelv támogatja.</span><span class="sxs-lookup"><span data-stu-id="35da6-121">Compared toohello previous version, Azure Media Indexer 2 Preview has faster indexing and broader language support.</span></span> <span data-ttu-id="35da6-122">Támogatott nyelvek angol, német, francia, német, olasz, kínai, portugál és arab.</span><span class="sxs-lookup"><span data-stu-id="35da6-122">Supported languages include English, Spanish, French, German, Italian, Chinese, Portuguese, and Arabic.</span></span> <span data-ttu-id="35da6-123">Részletes útmutatást és példákat, lásd: [videók feldolgozása az Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span><span class="sxs-lookup"><span data-stu-id="35da6-123">For detailed information and examples, see [Process videos with Azure Media Indexer 2](media-services-process-content-with-indexer2.md).</span></span>
### <a name="hyperlapse"></a><span data-ttu-id="35da6-124">Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="35da6-124">Hyperlapse</span></span>
<span data-ttu-id="35da6-125">Microsoft Hyperlapse videó exportstabilizációt és gyorsított felvétel funkció toocreate gyors és fogyasztható a hosszú tartalomról videók egyesíti.</span><span class="sxs-lookup"><span data-stu-id="35da6-125">Microsoft Hyperlapse combines video stabilization and time-lapse capability toocreate quick, consumable videos from your long-form content.</span></span> <span data-ttu-id="35da6-126">Gyorsított felvétel videó létrehozása, mellett Hyperlapse toocreate stabil videók a mobiltelefonokat és kamerák rögzített shaky videók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="35da6-126">Besides creating time-lapse video, you can use Hyperlapse toocreate stable videos from shaky videos captured via cell phones and camcorders.</span></span> <span data-ttu-id="35da6-127">Részletes útmutatást és példákat, lásd: [Hyperlapse médiafájlok az Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span><span class="sxs-lookup"><span data-stu-id="35da6-127">For detailed information and examples, see [Hyperlapse media files with Azure Media Hyperlapse](media-services-hyperlapse-content.md).</span></span>
### <a name="motion-detector"></a><span data-ttu-id="35da6-128">Mozgásérzékelő</span><span class="sxs-lookup"><span data-stu-id="35da6-128">Motion Detector</span></span>
<span data-ttu-id="35da6-129">Mozgásérzékelő toodetect mozgásérzékelési használhatja a helyhez kötött hátterek videó.</span><span class="sxs-lookup"><span data-stu-id="35da6-129">You can use Motion Detector toodetect motion in a video with stationary backgrounds.</span></span> <span data-ttu-id="35da6-130">Így a vakriasztások lehetséges toocheck mozgásérzékelési események által térfigyelő kamerák használatát észlelte.</span><span class="sxs-lookup"><span data-stu-id="35da6-130">This makes it possible toocheck for false positives on motion events detected by surveillance cameras.</span></span> <span data-ttu-id="35da6-131">Részletes útmutatást és példákat, lásd: [Mozgásérzékelés az Azure Médiaelemzés használatával](media-services-motion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="35da6-131">For detailed information and examples, see [Motion detection for Azure Media Analytics](media-services-motion-detection.md).</span></span>
### <a name="face-detector"></a><span data-ttu-id="35da6-132">Arcérzékelő</span><span class="sxs-lookup"><span data-stu-id="35da6-132">Face Detector</span></span>
<span data-ttu-id="35da6-133">Arcfelismerési érzékelő használatával Népi lapokat és a érzelmek, beleértve a Boldogsága, sadness és jelzés nélküli észlelését.</span><span class="sxs-lookup"><span data-stu-id="35da6-133">By using Face Detector, you can detect people’s faces and their emotions, including happiness, sadness, and surprise.</span></span> <span data-ttu-id="35da6-134">Számos hasznos iparági alkalmazások, későbbi, beleértve összesítésére és elemzésére esemény részt vevő személyek reakciók azt.</span><span class="sxs-lookup"><span data-stu-id="35da6-134">This has several useful industry applications, described later, including aggregating and analyzing reactions of people attending an event.</span></span> <span data-ttu-id="35da6-135">Részletes útmutatást és példákat, lásd: [Arcfelismerés és érzelemfelismerési észlelési az Azure Médiaelemzés használatával](media-services-face-and-emotion-detection.md).</span><span class="sxs-lookup"><span data-stu-id="35da6-135">For detailed information and examples, see [Face and emotion detection for Azure Media Analytics](media-services-face-and-emotion-detection.md).</span></span>
### <a name="video-summarization"></a><span data-ttu-id="35da6-136">Videóösszegzés</span><span class="sxs-lookup"><span data-stu-id="35da6-136">Video summarization</span></span>
<span data-ttu-id="35da6-137">Videóösszegzés segítségével létre videók kijelölésével automatikusan érdekes kódtöredékek hello forrás videó.</span><span class="sxs-lookup"><span data-stu-id="35da6-137">Video summarization can help you create summaries of long videos by automatically selecting interesting snippets from hello source video.</span></span> <span data-ttu-id="35da6-138">Ez a lehetőség akkor hasznos, ha azt szeretné, hogy milyen hosszú videó tooexpect gyors áttekintést tooprovide.</span><span class="sxs-lookup"><span data-stu-id="35da6-138">This ability is useful when you want tooprovide a quick overview of what tooexpect in a long video.</span></span> <span data-ttu-id="35da6-139">Részletes útmutatást és példákat, lásd: [használata Azure Media videó miniatűrök toocreate videóösszegzés](media-services-video-summarization.md).</span><span class="sxs-lookup"><span data-stu-id="35da6-139">For detailed information and examples, see [Use Azure Media Video Thumbnails toocreate video summarization](media-services-video-summarization.md).</span></span>
### <a name="optical-character-recognition"></a><span data-ttu-id="35da6-140">Optikai karakterfelismerés</span><span class="sxs-lookup"><span data-stu-id="35da6-140">Optical character recognition</span></span>
<span data-ttu-id="35da6-141">Az Azure Media OCR (optikai karakter használata) átalakíthatja videofájlok a szöveges tartalom szerkeszthető, kereshető digitális szöveg.</span><span class="sxs-lookup"><span data-stu-id="35da6-141">With Azure Media OCR (optical character recognition), you can convert text content in video files into editable, searchable digital text.</span></span> <span data-ttu-id="35da6-142">Automatizálható hello kivonása jelentéssel bíró metaadatok hello videó Signal médiafájlt.</span><span class="sxs-lookup"><span data-stu-id="35da6-142">You can then automate hello extraction of meaningful metadata from hello video signal of your media.</span></span>
### <a name="scalable-face-redaction"></a><span data-ttu-id="35da6-143">Méretezhető arcfelismerési kivonási</span><span class="sxs-lookup"><span data-stu-id="35da6-143">Scalable face redaction</span></span>
<span data-ttu-id="35da6-144">Az Azure Media Redactor, amely méretezhető arcfelismerési kivonási hello felhőben nyújt Médiaelemzés media processzort.</span><span class="sxs-lookup"><span data-stu-id="35da6-144">Azure Media Redactor is a Media Analytics media processor that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="35da6-145">Arcfelismerési kivonási használatával módosíthatja a kijelölt személyek a videó tooblur felületei.</span><span class="sxs-lookup"><span data-stu-id="35da6-145">By using face redaction, you can modify your video tooblur faces of selected individuals.</span></span> <span data-ttu-id="35da6-146">Érdemes lehet toouse hello arcfelismerési kivonási szolgáltatás hírek media, vagy ha nyilvános biztonsági van szó.</span><span class="sxs-lookup"><span data-stu-id="35da6-146">You might want toouse hello face redaction service in news media or when public safety is involved.</span></span> <span data-ttu-id="35da6-147">Több lapokat tartalmazó felvételei, néhány percet is igénybe vehet óra tooredact manuálisan, de ezzel a szolgáltatással arcfelismerési kivonási tart néhány egyszerű lépésben.</span><span class="sxs-lookup"><span data-stu-id="35da6-147">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service, face redaction takes just a few simple steps.</span></span> <span data-ttu-id="35da6-148">További információkért lásd: hello [az Azure Media Analytics lapok kivonás](media-services-face-redaction.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="35da6-148">For more information, see hello [Redact faces with Azure Media Analytics](media-services-face-redaction.md) article.</span></span>

## <a name="common-scenarios"></a><span data-ttu-id="35da6-149">Gyakori forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="35da6-149">Common scenarios</span></span>
<span data-ttu-id="35da6-150">Media Analytics segítségével a szervezetek és vállalatok glean videó új információkat kaphat, és több hogyan kezelhet hatékonyan videotartalom nagy mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="35da6-150">Media Analytics can help organizations and enterprises glean new insights from video and more effectively manage large volumes of video content.</span></span> <span data-ttu-id="35da6-151">Az alábbiakban néhány forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="35da6-151">Here are several scenarios:</span></span>

* <span data-ttu-id="35da6-152">**Erőforrások hívás**.</span><span class="sxs-lookup"><span data-stu-id="35da6-152">**Call centers**.</span></span> <span data-ttu-id="35da6-153">Akkor is közösségi hello megjelenésével ügyfél telefonos ügyfélszolgálatok továbbra is megkönnyíteni az ügyfélszolgálati tranzakciók nagy része.</span><span class="sxs-lookup"><span data-stu-id="35da6-153">Even with hello advent of social media, customer call centers still facilitate a large percentage of customer-service transactions.</span></span> <span data-ttu-id="35da6-154">Ügyféladatok, amely nagy mennyiségű elemzett tooachieve magasabb ügyfelek elégedettségének a hangadatok kódolású van.</span><span class="sxs-lookup"><span data-stu-id="35da6-154">Encoded in this audio data is a large amount of customer information that can be analyzed tooachieve higher customer satisfaction.</span></span> <span data-ttu-id="35da6-155">Media Indexer használatával a szervezetek szöveg és keresési indexek és irányítópultokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="35da6-155">By using Media Indexer, organizations can extract text and build search indexes and dashboards.</span></span> <span data-ttu-id="35da6-156">Majd kinyerhessék eszközintelligencia gyakran panaszkodnak, panaszokat források és egyéb kapcsolódó adatokat.</span><span class="sxs-lookup"><span data-stu-id="35da6-156">Then they can extract intelligence around common complaints, sources of complaints, and other relevant data.</span></span>
* <span data-ttu-id="35da6-157">**Felhasználó által létrehozott tartalom moderálás**.</span><span class="sxs-lookup"><span data-stu-id="35da6-157">**User-generated content moderation**.</span></span> <span data-ttu-id="35da6-158">Hírek media kimeneteket toopolice részlegek, a számos szervezet rendelkezik nyilvánosan elérhető portálok fogadja el a felhasználók által létrehozott adathordozók, például videók és a képeket.</span><span class="sxs-lookup"><span data-stu-id="35da6-158">From news media outlets toopolice departments, many organizations have public-facing portals that accept user-generated media such as videos and images.</span></span> <span data-ttu-id="35da6-159">hello mennyiségű tartalmat is lefoglalását toounexpected események miatt.</span><span class="sxs-lookup"><span data-stu-id="35da6-159">hello volume of content can spike due toounexpected events.</span></span> <span data-ttu-id="35da6-160">Ezekben az esetekben nehéz tooconduct hatékony manuális értékelést, melyekkel tartalom.</span><span class="sxs-lookup"><span data-stu-id="35da6-160">In these scenarios, it is difficult tooconduct effective manual reviews of content for appropriateness.</span></span> <span data-ttu-id="35da6-161">Az ügyfelek támaszkodhat hello tartalom-moderálás szolgáltatás toofocus a megfelelő tartalom.</span><span class="sxs-lookup"><span data-stu-id="35da6-161">Customers can rely on hello content-moderation service toofocus on content that is appropriate.</span></span>
* <span data-ttu-id="35da6-162">**Felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="35da6-162">**Surveillance**.</span></span> <span data-ttu-id="35da6-163">A hello növekedési használt IP-kamerák egy egyre bővülő leltár felügyeleti videó származnak.</span><span class="sxs-lookup"><span data-stu-id="35da6-163">With hello growth in use of IP cameras comes a growing inventory of surveillance video.</span></span> <span data-ttu-id="35da6-164">Manuálisan megtekintésével felügyeleti videó idő intenzív és hibalehetőségeket rejt magában toohuman hiba.</span><span class="sxs-lookup"><span data-stu-id="35da6-164">Manually reviewing surveillance video is time intensive and prone toohuman error.</span></span> <span data-ttu-id="35da6-165">Media Analytics például mozgásérzékelés arcfelismerési észlelési vagy Hyperlapse toomake hello folyamat áttekintése, kezelése és származékaik egyszerűbb létrehozását szolgáltatásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="35da6-165">Media Analytics provides services such as motion detection, face detection, and Hyperlapse toomake hello process of reviewing, managing, and creating derivatives easier.</span></span>

## <a name="media-analytics-media-processors"></a><span data-ttu-id="35da6-166">Media Analytics media processzor</span><span class="sxs-lookup"><span data-stu-id="35da6-166">Media Analytics media processors</span></span>
<span data-ttu-id="35da6-167">Hogyan a szakasz a listák hello Médiaelemzés media processzorral és mutat be toouse .NET és REST tooget egy adathordozó processzor (MP) objektum.</span><span class="sxs-lookup"><span data-stu-id="35da6-167">This section lists hello Media Analytics media processors and shows how toouse .NET or REST tooget a media processor (MP) object.</span></span>

### <a name="mp-names"></a><span data-ttu-id="35da6-168">Felügyeleti csomag neve</span><span class="sxs-lookup"><span data-stu-id="35da6-168">MP names</span></span>
* <span data-ttu-id="35da6-169">Az Azure Media Indexer 2 előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="35da6-169">Azure Media Indexer 2 Preview</span></span>
* <span data-ttu-id="35da6-170">Azure Media Indexer</span><span class="sxs-lookup"><span data-stu-id="35da6-170">Azure Media Indexer</span></span>
* <span data-ttu-id="35da6-171">Azure Media Hyperlapse</span><span class="sxs-lookup"><span data-stu-id="35da6-171">Azure Media Hyperlapse</span></span>
* <span data-ttu-id="35da6-172">Azure Media Face Detector</span><span class="sxs-lookup"><span data-stu-id="35da6-172">Azure Media Face Detector</span></span>
* <span data-ttu-id="35da6-173">Azure Media Motion Detector</span><span class="sxs-lookup"><span data-stu-id="35da6-173">Azure Media Motion Detector</span></span>
* <span data-ttu-id="35da6-174">Azure Media Video Thumbnails</span><span class="sxs-lookup"><span data-stu-id="35da6-174">Azure Media Video Thumbnails</span></span>
* <span data-ttu-id="35da6-175">Azure Media OCR</span><span class="sxs-lookup"><span data-stu-id="35da6-175">Azure Media OCR</span></span>

### <a name="net"></a><span data-ttu-id="35da6-176">.NET</span><span class="sxs-lookup"><span data-stu-id="35da6-176">.NET</span></span>
<span data-ttu-id="35da6-177">a következő függvény vesz egy hello a hello megadott felügyeleti csomag neve és a felügyeleti csomag objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="35da6-177">hello following function takes one of hello specified MP names and returns an MP object.</span></span>

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


### <a name="rest"></a><span data-ttu-id="35da6-178">REST</span><span class="sxs-lookup"><span data-stu-id="35da6-178">REST</span></span>
<span data-ttu-id="35da6-179">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="35da6-179">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net

<span data-ttu-id="35da6-180">Válasz:</span><span class="sxs-lookup"><span data-stu-id="35da6-180">Response:</span></span>

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

## <a name="demos"></a><span data-ttu-id="35da6-181">Bemutatók</span><span class="sxs-lookup"><span data-stu-id="35da6-181">Demos</span></span>
<span data-ttu-id="35da6-182">Lásd: [Azure Médiaelemzés használatával bemutatók](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span><span class="sxs-lookup"><span data-stu-id="35da6-182">See [Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html).</span></span>

## <a name="next-steps"></a><span data-ttu-id="35da6-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35da6-183">Next steps</span></span>
<span data-ttu-id="35da6-184">Tekintse át a Media Services képzési terveket.</span><span class="sxs-lookup"><span data-stu-id="35da6-184">Review Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="35da6-185">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="35da6-185">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="35da6-186">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="35da6-186">Related articles</span></span>
<span data-ttu-id="35da6-187">Lásd: [Médiaelemzés-szolgáltatások közlemény](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span><span class="sxs-lookup"><span data-stu-id="35da6-187">See [Media Services Analytics announcement](https://azure.microsoft.com/blog/introducing-azure-media-analytics/).</span></span>

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png

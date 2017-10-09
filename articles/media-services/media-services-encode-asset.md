---
title: "aaaOverview és összehasonlítása az Azure media kódolók igény |} Microsoft Docs"
description: "Ez a témakör minden áttekintése és az igény szerinti media kódolók Azure összehasonlítása."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: juliako
ms.openlocfilehash: 24a3e0a16162b1bebfcde290b6baf2dd8dbfff17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a><span data-ttu-id="69209-103">Áttekintés és az igény szerinti media kódolók Azure összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="69209-103">Overview and comparison of Azure on demand media encoders</span></span>
## <a name="encoding-overview"></a><span data-ttu-id="69209-104">Kódolási áttekintése</span><span class="sxs-lookup"><span data-stu-id="69209-104">Encoding overview</span></span>
<span data-ttu-id="69209-105">Az Azure Media Services hello kódolás hello felhőben adathordozó több lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="69209-105">Azure Media Services provides multiple options for hello encoding of media in hello cloud.</span></span>

<span data-ttu-id="69209-106">Amikor kezdte meg a Media Services, fontos toounderstand hello különbségének kodekeket és a fájl formátuma nem.</span><span class="sxs-lookup"><span data-stu-id="69209-106">When starting out with Media Services, it is important toounderstand hello difference between codecs and file formats.</span></span>
<span data-ttu-id="69209-107">A kodekeket hello szoftver, amely hello tömörítési/kitömörítés algoritmusok, mivel fájlformátumok tárolói tömörített hello videó tartsa.</span><span class="sxs-lookup"><span data-stu-id="69209-107">Codecs are hello software that implements hello compression/decompression algorithms whereas file formats are containers that hold hello compressed video.</span></span>

<span data-ttu-id="69209-108">A Media Services dinamikus becsomagolást, amely lehetővé teszi az adaptív sávszélességű MP4 vagy Smooth Streaming-kódolású tartalmak (MPEG DASH, HLS, Smooth Streaming) Media Services által támogatott streamformátumok toodeliver biztosít anélkül, hogy toore-csomagját ezek adatfolyam-továbbítási formátumokba.</span><span class="sxs-lookup"><span data-stu-id="69209-108">Media Services provides dynamic packaging which allows you toodeliver your adaptive bitrate MP4 or Smooth Streaming encoded content in streaming formats supported by Media Services (MPEG DASH, HLS, Smooth Streaming) without you having toore-package into these streaming formats.</span></span>

>[!NOTE]
><span data-ttu-id="69209-109">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="69209-109">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="69209-110">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="69209-110">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> <span data-ttu-id="69209-111">tootake előnyeit [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md), toodo hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="69209-111">tootake advantage of [dynamic packaging](media-services-dynamic-packaging-overview.md), you need toodo hello following:</span></span>
>
><span data-ttu-id="69209-112">Emellett kódolja a forrásfájlt adaptív sávszélességű MP4-vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá (az oktatóanyag későbbi részében hello kódolás lépéseit egy).</span><span class="sxs-lookup"><span data-stu-id="69209-112">Also, encode your source file into a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files (hello encoding steps are demonstrated later in this tutorial).</span></span>

<span data-ttu-id="69209-113">A Media Services hello következő igény szerinti kódolók ebben a cikkben ismertetett támogatja:</span><span class="sxs-lookup"><span data-stu-id="69209-113">Media Services supports hello following on demand encoders that are described in this article:</span></span>

* [<span data-ttu-id="69209-114">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="69209-114">Media Encoder Standard</span></span>](media-services-encode-asset.md#media-encoder-standard)
* [<span data-ttu-id="69209-115">Media Encoder Premium-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="69209-115">Media Encoder Premium Workflow</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)

<span data-ttu-id="69209-116">Ez a cikk rövid áttekintést nyújt az igény szerinti media kódolók, és hivatkozások tooarticles, amely részletesebb információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="69209-116">This article gives a brief overview of on demand media encoders and provides links tooarticles that give more detailed information.</span></span> <span data-ttu-id="69209-117">hello is témakör hello kódolók összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="69209-117">hello topic also provides comparison of hello encoders.</span></span>

>[!NOTE]
><span data-ttu-id="69209-118">Alapértelmezés szerint minden Media Services-fiók lehet aktív kódolási feladat is egyszerre.</span><span class="sxs-lookup"><span data-stu-id="69209-118">By default each Media Services account can have one active encoding task at a time.</span></span> <span data-ttu-id="69209-119">Kódolási egység, amely lehetővé teszi toohave több kódolási feladat fut egyidejűleg, egy minden egyes megvásárolt kódolási fenntartott egységnek a foglalhat.</span><span class="sxs-lookup"><span data-stu-id="69209-119">You can reserve encoding units that allow you toohave multiple encoding tasks running concurrently, one for each encoding reserved unit you purchase.</span></span> <span data-ttu-id="69209-120">További információ: [kódolási egységek méretezése](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="69209-120">For information, see [Scaling encoding units](media-services-scale-media-processing-overview.md).</span></span>

## <a name="media-encoder-standard"></a><span data-ttu-id="69209-121">Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="69209-121">Media Encoder Standard</span></span>
### <a name="how-toouse"></a><span data-ttu-id="69209-122">Hogyan toouse</span><span class="sxs-lookup"><span data-stu-id="69209-122">How toouse</span></span>
[<span data-ttu-id="69209-123">Hogyan tooencode a Media Encoder Standard</span><span class="sxs-lookup"><span data-stu-id="69209-123">How tooencode with Media Encoder Standard</span></span>](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a><span data-ttu-id="69209-124">Formázza</span><span class="sxs-lookup"><span data-stu-id="69209-124">Formats</span></span>
[<span data-ttu-id="69209-125">Formátumok és kodekek</span><span class="sxs-lookup"><span data-stu-id="69209-125">Formats and codecs</span></span>](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a><span data-ttu-id="69209-126">Készletek</span><span class="sxs-lookup"><span data-stu-id="69209-126">Presets</span></span>
<span data-ttu-id="69209-127">Media Encoder Standard segítségével konfigurálható: az egyik hello kódoló készletek leírt [Itt](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="69209-127">Media Encoder Standard is configured using one of hello encoder presets described [here](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).</span></span>

### <a name="input-and-output-metadata"></a><span data-ttu-id="69209-128">Bemeneti és kimeneti metaadatok</span><span class="sxs-lookup"><span data-stu-id="69209-128">Input and output metadata</span></span>
<span data-ttu-id="69209-129">hello kódolók bemeneti metaadatok leírását [Itt](media-services-input-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="69209-129">hello encoders input metadata is described [here](media-services-input-metadata-schema.md).</span></span>

<span data-ttu-id="69209-130">hello kódolók kimeneti metaadatok leírását [Itt](media-services-output-metadata-schema.md).</span><span class="sxs-lookup"><span data-stu-id="69209-130">hello encoders output metadata is described [here](media-services-output-metadata-schema.md).</span></span>

### <a name="generate-thumbnails"></a><span data-ttu-id="69209-131">Indexképének létrehozására</span><span class="sxs-lookup"><span data-stu-id="69209-131">Generate thumbnails</span></span>
<span data-ttu-id="69209-132">További információ: [hogyan Media Encoder Standard használatával toogenerate miniatűrök](media-services-advanced-encoding-with-mes.md#thumbnails).</span><span class="sxs-lookup"><span data-stu-id="69209-132">For information, see [How toogenerate thumbnails using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#thumbnails).</span></span>

### <a name="trim-videos-clipping"></a><span data-ttu-id="69209-133">Trim (vágás) videók</span><span class="sxs-lookup"><span data-stu-id="69209-133">Trim videos (clipping)</span></span>
<span data-ttu-id="69209-134">További információ: [hogyan Media Encoder Standard használatával tootrim videók](media-services-advanced-encoding-with-mes.md#trim_video).</span><span class="sxs-lookup"><span data-stu-id="69209-134">For information, see [How tootrim videos using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#trim_video).</span></span>

### <a name="create-overlays"></a><span data-ttu-id="69209-135">Átfedések létrehozása</span><span class="sxs-lookup"><span data-stu-id="69209-135">Create overlays</span></span>
<span data-ttu-id="69209-136">További információ: [hogyan Media Encoder Standard használatával toocreate átfedések](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="69209-136">For information, see [How toocreate overlays using Media Encoder Standard](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

### <a name="see-also"></a><span data-ttu-id="69209-137">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="69209-137">See also</span></span>
[<span data-ttu-id="69209-138">hello Media Services blog</span><span class="sxs-lookup"><span data-stu-id="69209-138">hello Media Services blog</span></span>](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a><span data-ttu-id="69209-139">Media Encoder Premium-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="69209-139">Media Encoder Premium Workflow</span></span>
### <a name="overview"></a><span data-ttu-id="69209-140">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="69209-140">Overview</span></span>
[<span data-ttu-id="69209-141">Prémium szintű kódolás az Azure Media Services bemutatása</span><span class="sxs-lookup"><span data-stu-id="69209-141">Introducing Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-toouse"></a><span data-ttu-id="69209-142">Hogyan toouse</span><span class="sxs-lookup"><span data-stu-id="69209-142">How toouse</span></span>
<span data-ttu-id="69209-143">Media Encoder prémium munkafolyamat bonyolult munkafolyamatok segítségével lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="69209-143">Media Encoder Premium Workflow is configured using complex workflows.</span></span> <span data-ttu-id="69209-144">Munkafolyamat-fájlok létrehozása sikerült, és a hello frissítve [munkafolyamat-Tervező](media-services-workflow-designer.md) eszköz.</span><span class="sxs-lookup"><span data-stu-id="69209-144">Workflow files could be created and updated using hello [Workflow Designer](media-services-workflow-designer.md) tool.</span></span>

[<span data-ttu-id="69209-145">Hogyan prémium szintű Azure Media Services kódolási tooUse</span><span class="sxs-lookup"><span data-stu-id="69209-145">How tooUse Premium Encoding in Azure Media Services</span></span>](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a><span data-ttu-id="69209-146">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="69209-146">Known issues</span></span>
<span data-ttu-id="69209-147">Ha a bemeneti videó nem tartalmaz a kódolt, a hello kimeneti eszköz továbbra is egy üres TTML fájlt fogja tartalmazni.</span><span class="sxs-lookup"><span data-stu-id="69209-147">If your input video does not contain closed captioning, hello output Asset will still contain an empty TTML file.</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="69209-148">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="69209-148">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="69209-149">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="69209-149">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a><span data-ttu-id="69209-150">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="69209-150">Related articles</span></span>
* [<span data-ttu-id="69209-151">Speciális feladatokra kódolási Media Encoder Standard készletek testreszabásával.</span><span class="sxs-lookup"><span data-stu-id="69209-151">Perform advanced encoding tasks by customizing Media Encoder Standard presets</span></span>](media-services-custom-mes-presets-with-dotnet.md)
* [<span data-ttu-id="69209-152">Kvóták és korlátozások</span><span class="sxs-lookup"><span data-stu-id="69209-152">Quotas and Limitations</span></span>](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/

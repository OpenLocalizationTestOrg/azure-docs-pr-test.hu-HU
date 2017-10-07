---
title: "aaaAzure Media Services dinamikus becsomagolás áttekintése |} Microsoft Docs"
description: "hello témakör által biztosított és a dinamikus becsomagolás áttekintése."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 970e24eba800e098774172c87f56629430b227a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-packaging"></a><span data-ttu-id="7523d-103">Dinamikus csomagolás</span><span class="sxs-lookup"><span data-stu-id="7523d-103">Dynamic packaging</span></span>
## <a name="overview"></a><span data-ttu-id="7523d-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7523d-104">Overview</span></span>
<span data-ttu-id="7523d-105">A Microsoft Azure Media Services lehet használt toodeliver sok media forrás fájlformátumokat, media formátumban és a védett tartalom formátumok tooa számos ügyfél technológiák (például iOS, XBOX, a Silverlight, a Windows 8).</span><span class="sxs-lookup"><span data-stu-id="7523d-105">Microsoft Azure Media Services can be used toodeliver many media source file formats, media streaming formats, and content protection formats tooa variety of client technologies (for example, iOS, XBOX, Silverlight, Windows 8).</span></span> <span data-ttu-id="7523d-106">Ezek az ügyfelek ismertetése különböző protokollok, például iOS igényel-e egy HTTP-Live Streaming (HLS) V4 formátum, és a Silverlight- és Xbox igényelnek, Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="7523d-106">These clients understand different protocols, for example iOS requires an HTTP Live Streaming (HLS) V4 format and Silverlight and Xbox require Smooth Streaming.</span></span> <span data-ttu-id="7523d-107">Ha rendelkezik egy adaptív sávszélességű (többféle sávszélességű) készletét MP4 (ISO talál médiafájlok 14496-12) vagy egy adaptív sávszélességű Smooth Streaming-fájlsorozattá, hogy szeretné-e, ismernie MPEG DASH, HLS vagy Smooth Streaming tooserve tooclients, meg kell előnyeit adathordozó Services dinamikus becsomagolást.</span><span class="sxs-lookup"><span data-stu-id="7523d-107">If you have a set of adaptive bitrate (multi-bitrate) MP4 (ISO Base Media 14496-12) files or a set of adaptive bitrate Smooth Streaming files that you want tooserve tooclients that understand MPEG DASH, HLS or Smooth Streaming, you should take advantage of Media Services dynamic packaging.</span></span>

<span data-ttu-id="7523d-108">A dinamikus csomagolás szüksége van egy eszköz, amely tartalmazza az adaptív sávszélességű MP4-vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá toocreate.</span><span class="sxs-lookup"><span data-stu-id="7523d-108">With dynamic packaging all you need is toocreate an asset that contains a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="7523d-109">Majd hello hello jegyzékben megadott formátumnak alapján kérelem darabolható, vagy igény szerinti adatfolyam-kiszolgáló biztosítja a megjelenő hello adatfolyam a kiválasztott protokollal hello hello.</span><span class="sxs-lookup"><span data-stu-id="7523d-109">Then, based on hello specified format in hello manifest or fragment request, hello On-Demand Streaming server will ensure that you receive hello stream in hello protocol you have chosen.</span></span> <span data-ttu-id="7523d-110">Ennek eredményeképpen csak akkor kell toostore és fizetési hello fájlokat egyetlen tárolási formátumban és a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ hello.</span><span class="sxs-lookup"><span data-stu-id="7523d-110">As a result, you only need toostore and pay for hello files in single storage format and Media Services service will build and serve hello appropriate response based on requests from a client.</span></span>

<span data-ttu-id="7523d-111">hello alábbi ábrán látható hello hagyományos kódolás és statikus csomagolás munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="7523d-111">hello following diagram shows hello traditional encoding and static packaging workflow.</span></span>

![Statikus kódolás](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

<span data-ttu-id="7523d-113">hello alábbi ábrán látható hello dinamikus becsomagolás munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="7523d-113">hello following diagram shows hello dynamic packaging workflow.</span></span>

![Dinamikus kódolás](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a><span data-ttu-id="7523d-115">Egy gyakori alaphelyzete</span><span class="sxs-lookup"><span data-stu-id="7523d-115">Common scenario</span></span>
1. <span data-ttu-id="7523d-116">Töltse fel a bemeneti fájl (néven mezzazine-fájlt).</span><span class="sxs-lookup"><span data-stu-id="7523d-116">Upload an input file (called a mezzanine file).</span></span> <span data-ttu-id="7523d-117">Például H.264, MP4 vagy WMV (hello támogatott formátumok listája: [Media Encoder Standard hello által támogatott formátumok](media-services-media-encoder-standard-formats.md).</span><span class="sxs-lookup"><span data-stu-id="7523d-117">For example, H.264, MP4, or WMV (for hello list of supported formats see [Formats Supported by hello Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span></span>
2. <span data-ttu-id="7523d-118">Kódolja a mezzanine fájl tooH.264 MP4 adaptív sávszélességű beállítása.</span><span class="sxs-lookup"><span data-stu-id="7523d-118">Encode your mezzanine file tooH.264 MP4 adaptive bitrate sets.</span></span>
3. <span data-ttu-id="7523d-119">Tegye közzé hello eszköz, amely tartalmazza a hello adaptív sávszélességű MP4 típusú beállításkészlettel hello igény kereső létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="7523d-119">Publish hello asset that contains hello adaptive bitrate MP4 set by creating hello On-Demand Locator.</span></span>
4. <span data-ttu-id="7523d-120">Build a streaming URL-címek tooaccess hello és a tartalmak.</span><span class="sxs-lookup"><span data-stu-id="7523d-120">Build hello streaming URLs tooaccess and stream your content.</span></span>

## <a name="preparing-assets-for-dynamic-streaming"></a><span data-ttu-id="7523d-121">Dinamikus streaming eszközök előkészítése</span><span class="sxs-lookup"><span data-stu-id="7523d-121">Preparing assets for dynamic streaming</span></span>
<span data-ttu-id="7523d-122">tooprepare dinamikus streaming meg az eszköz két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="7523d-122">tooprepare your asset for dynamic streaming you have two options:</span></span>

1. <span data-ttu-id="7523d-123">[Töltse fel a fő fájlt](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="7523d-123">[Upload a master file](media-services-dotnet-upload-files.md).</span></span>
2. <span data-ttu-id="7523d-124">[Hello Media Encoder Standard encoder tooproduce H.264 MP4 adaptív sávszélességű készletek használata](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="7523d-124">[Use hello Media Encoder Standard encoder tooproduce H.264 MP4 adaptive bitrate sets](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>
3. <span data-ttu-id="7523d-125">[A tartalmak](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7523d-125">[Stream your content](media-services-deliver-content-overview.md).</span></span>

## <span data-ttu-id="7523d-126"><a id="unsupported_formats"></a>Dinamikus becsomagolás által nem támogatott</span><span class="sxs-lookup"><span data-stu-id="7523d-126"><a id="unsupported_formats"></a>Formats that are not supported by dynamic packaging</span></span>
<span data-ttu-id="7523d-127">hello következő forrás formátumok nem támogatja a dinamikus csomagolás.</span><span class="sxs-lookup"><span data-stu-id="7523d-127">hello following source file formats are not supported by dynamic packaging.</span></span>

* <span data-ttu-id="7523d-128">Dolby digitális mp4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7523d-128">Dolby digital mp4 files.</span></span>
* <span data-ttu-id="7523d-129">Dolby digitális zökkenőmentes fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7523d-129">Dolby digital smooth files.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="7523d-130">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="7523d-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7523d-131">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7523d-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


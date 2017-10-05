---
title: "Az Azure Media Services dinamikus becsomagolás áttekintése |} Microsoft Docs"
description: "A témakör által biztosított és a dinamikus becsomagolás áttekintése."
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
ms.openlocfilehash: 2d212599302fced3f60085ab30cdeaefc1ee2e6a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="dynamic-packaging"></a><span data-ttu-id="66ab8-103">Dinamikus csomagolás</span><span class="sxs-lookup"><span data-stu-id="66ab8-103">Dynamic packaging</span></span>
## <a name="overview"></a><span data-ttu-id="66ab8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="66ab8-104">Overview</span></span>
<span data-ttu-id="66ab8-105">Microsoft Azure Media Services segítségével kézbesítése sok media forrás fájlformátumokat, media adatfolyam-továbbítási formátumokba, és a tartalomvédelem formátumokat számos különböző ügyfél technológiák (például iOS, XBOX, a Silverlight, a Windows 8).</span><span class="sxs-lookup"><span data-stu-id="66ab8-105">Microsoft Azure Media Services can be used to deliver many media source file formats, media streaming formats, and content protection formats to a variety of client technologies (for example, iOS, XBOX, Silverlight, Windows 8).</span></span> <span data-ttu-id="66ab8-106">Ezek az ügyfelek ismertetése különböző protokollok, például iOS igényel-e egy HTTP-Live Streaming (HLS) V4 formátum, és a Silverlight- és Xbox igényelnek, Smooth Streaming.</span><span class="sxs-lookup"><span data-stu-id="66ab8-106">These clients understand different protocols, for example iOS requires an HTTP Live Streaming (HLS) V4 format and Silverlight and Xbox require Smooth Streaming.</span></span> <span data-ttu-id="66ab8-107">Ha rendelkezik egy adaptív sávszélességű (többféle sávszélességű) készletét MP4 (ISO talál médiafájlok 14496-12) vagy egy adaptív sávszélességű Smooth Streaming-fájlsorozattá osztja ki az ügyfelek, MPEG DASH, HLS vagy Smooth Streaming megértéséhez kívánt, a Media Services dinamikus becsomagolás kell venni.</span><span class="sxs-lookup"><span data-stu-id="66ab8-107">If you have a set of adaptive bitrate (multi-bitrate) MP4 (ISO Base Media 14496-12) files or a set of adaptive bitrate Smooth Streaming files that you want to serve to clients that understand MPEG DASH, HLS or Smooth Streaming, you should take advantage of Media Services dynamic packaging.</span></span>

<span data-ttu-id="66ab8-108">A dinamikus csomagolás szükség, ha egy eszköz, amely tartalmazza az adaptív sávszélességű MP4-vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá.</span><span class="sxs-lookup"><span data-stu-id="66ab8-108">With dynamic packaging all you need is to create an asset that contains a set of adaptive bitrate MP4 files or adaptive bitrate Smooth Streaming files.</span></span> <span data-ttu-id="66ab8-109">Ezt követően a jegyzék vagy töredék kérelem, az Igényalapú Streamelési megadott formátumnak megfelelően kiszolgáló biztosítja az adatfolyam kapni a kiválasztott protokollal.</span><span class="sxs-lookup"><span data-stu-id="66ab8-109">Then, based on the specified format in the manifest or fragment request, the On-Demand Streaming server will ensure that you receive the stream in the protocol you have chosen.</span></span> <span data-ttu-id="66ab8-110">Így elég egyetlen tárolási formátumban tárolni a fájlokat (és kifizetni a tárhelyüket), a Media Services szolgáltatás elkészíti és kiszolgálja az ügyféltől érkező kérésnek megfelelő választ.</span><span class="sxs-lookup"><span data-stu-id="66ab8-110">As a result, you only need to store and pay for the files in single storage format and Media Services service will build and serve the appropriate response based on requests from a client.</span></span>

<span data-ttu-id="66ab8-111">Az alábbi ábrán látható, a hagyományos kódolás és a statikus csomagolás munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="66ab8-111">The following diagram shows the traditional encoding and static packaging workflow.</span></span>

![Statikus kódolás](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

<span data-ttu-id="66ab8-113">Az alábbi ábrán látható, a dinamikus becsomagolás munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="66ab8-113">The following diagram shows the dynamic packaging workflow.</span></span>

![Dinamikus kódolás](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a><span data-ttu-id="66ab8-115">Egy gyakori alaphelyzete</span><span class="sxs-lookup"><span data-stu-id="66ab8-115">Common scenario</span></span>
1. <span data-ttu-id="66ab8-116">Töltse fel a bemeneti fájl (néven mezzazine-fájlt).</span><span class="sxs-lookup"><span data-stu-id="66ab8-116">Upload an input file (called a mezzanine file).</span></span> <span data-ttu-id="66ab8-117">Például H.264, MP4 vagy WMV (a támogatott formátumok listája: [formátumokat támogatja a Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span><span class="sxs-lookup"><span data-stu-id="66ab8-117">For example, H.264, MP4, or WMV (for the list of supported formats see [Formats Supported by the Media Encoder Standard](media-services-media-encoder-standard-formats.md).</span></span>
2. <span data-ttu-id="66ab8-118">Kódolja a mezzanine fájl H.264 MP4 adaptív sávszélességű részhalmazához.</span><span class="sxs-lookup"><span data-stu-id="66ab8-118">Encode your mezzanine file to H.264 MP4 adaptive bitrate sets.</span></span>
3. <span data-ttu-id="66ab8-119">Tegye közzé az adategységet, amely tartalmazza az adaptív sávszélességű MP4 típusú beállításkészlettel az On-Demand-kereső létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="66ab8-119">Publish the asset that contains the adaptive bitrate MP4 set by creating the On-Demand Locator.</span></span>
4. <span data-ttu-id="66ab8-120">Az adatfolyam-továbbítási URL-címek elérését, és a tartalmak felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="66ab8-120">Build the streaming URLs to access and stream your content.</span></span>

## <a name="preparing-assets-for-dynamic-streaming"></a><span data-ttu-id="66ab8-121">Dinamikus streaming eszközök előkészítése</span><span class="sxs-lookup"><span data-stu-id="66ab8-121">Preparing assets for dynamic streaming</span></span>
<span data-ttu-id="66ab8-122">Az eszköz előkészítése dinamikus streaming két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="66ab8-122">To prepare your asset for dynamic streaming you have two options:</span></span>

1. <span data-ttu-id="66ab8-123">[Töltse fel a fő fájlt](media-services-dotnet-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="66ab8-123">[Upload a master file](media-services-dotnet-upload-files.md).</span></span>
2. <span data-ttu-id="66ab8-124">[Használja a Media Encoder Standard encoder H.264 MP4 adaptív sávszélességű készlet feladatvégrehajtás](media-services-dotnet-encode-with-media-encoder-standard.md).</span><span class="sxs-lookup"><span data-stu-id="66ab8-124">[Use the Media Encoder Standard encoder to produce H.264 MP4 adaptive bitrate sets](media-services-dotnet-encode-with-media-encoder-standard.md).</span></span>
3. <span data-ttu-id="66ab8-125">[A tartalmak](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="66ab8-125">[Stream your content](media-services-deliver-content-overview.md).</span></span>

## <span data-ttu-id="66ab8-126"><a id="unsupported_formats"></a>Dinamikus becsomagolás által nem támogatott</span><span class="sxs-lookup"><span data-stu-id="66ab8-126"><a id="unsupported_formats"></a>Formats that are not supported by dynamic packaging</span></span>
<span data-ttu-id="66ab8-127">A következő forrás formátumok nem támogatja a dinamikus csomagolás.</span><span class="sxs-lookup"><span data-stu-id="66ab8-127">The following source file formats are not supported by dynamic packaging.</span></span>

* <span data-ttu-id="66ab8-128">Dolby digitális mp4-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="66ab8-128">Dolby digital mp4 files.</span></span>
* <span data-ttu-id="66ab8-129">Dolby digitális zökkenőmentes fájlokat.</span><span class="sxs-lookup"><span data-stu-id="66ab8-129">Dolby digital smooth files.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="66ab8-130">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="66ab8-130">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="66ab8-131">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="66ab8-131">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


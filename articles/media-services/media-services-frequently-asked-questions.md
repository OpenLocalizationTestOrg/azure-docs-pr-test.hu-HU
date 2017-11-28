---
title: "Az Azure Media Services kapcsolatos gyakori kérdések |} Microsoft Docs"
description: "Gyakori kérdések (GYIK)"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5374f7f4-c189-43ef-8b7f-f2f4141e2748
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: 48f3924d44a084d61c1d38002cd5098094001acb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="frequently-asked-questions"></a><span data-ttu-id="ab5de-103">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="ab5de-103">Frequently asked questions</span></span>

<span data-ttu-id="ab5de-104">Ez a cikk foglalkozik az Azure Media Services (AMS) felhasználói Közösség által kiváltott gyakran ismételt kérdések.</span><span class="sxs-lookup"><span data-stu-id="ab5de-104">This article addresses frequently asked questions raised by the Azure Media Services (AMS) user community.</span></span>

## <a name="general-ams-faqs"></a><span data-ttu-id="ab5de-105">Általános AMS – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="ab5de-105">General AMS FAQs</span></span>
<span data-ttu-id="ab5de-106">K: hogyan, méretezhető, indexelő?</span><span class="sxs-lookup"><span data-stu-id="ab5de-106">Q: How do you scale indexing?</span></span>

<span data-ttu-id="ab5de-107">V: a fenntartott egységek esetén azonosak kódolás és indexelő feladat.</span><span class="sxs-lookup"><span data-stu-id="ab5de-107">A: The reserved units are the same for Encoding and Indexing tasks.</span></span> <span data-ttu-id="ab5de-108">Kövesse az utasításokat [méretezési kódoláshoz fenntartott egységek hogyan](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ab5de-108">Follow instructions on [How to Scale Encoding Reserved Units](media-services-scale-media-processing-overview.md).</span></span> <span data-ttu-id="ab5de-109">**Megjegyzés:** , hogy az indexelő teljesítmény nem érinti a fenntartott egység típusát.</span><span class="sxs-lookup"><span data-stu-id="ab5de-109">**Note** that Indexer performance is not affected by Reserved Unit Type.</span></span>

<span data-ttu-id="ab5de-110">K: I feltöltött, kódolt és videó közzé.</span><span class="sxs-lookup"><span data-stu-id="ab5de-110">Q: I uploaded, encoded, and published a video.</span></span> <span data-ttu-id="ab5de-111">Mi lehet a következő okból: a videó nem tölt meg adatfolyamként való?</span><span class="sxs-lookup"><span data-stu-id="ab5de-111">What would be the reason the video does not play when I try to stream it?</span></span>

<span data-ttu-id="ab5de-112">A leggyakoribb okai egy A:, nem rendelkezik a streamvégpontra, amelyről kívánt lejátszását a **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="ab5de-112">A: One of the most common reasons is you do not have the streaming endpoint from which you are trying to playback in the **Running** state.</span></span>  

<span data-ttu-id="ab5de-113">K: feladatokat lehet elvégezni egy élő adatfolyam összeállítás?</span><span class="sxs-lookup"><span data-stu-id="ab5de-113">Q: Can I do compositing on a live stream?</span></span>

<span data-ttu-id="ab5de-114">A: az élő adatfolyamok összeállítás jelenleg nem érhető el Azure Media Services, így előre állítható össze a számítógépen kell.</span><span class="sxs-lookup"><span data-stu-id="ab5de-114">A: Compositing on live streams is currently not offered in Azure Media Services, so you would need to pre-compose on your computer.</span></span>

<span data-ttu-id="ab5de-115">K: használhatok Azure CDN élő adatfolyam-továbbítási?</span><span class="sxs-lookup"><span data-stu-id="ab5de-115">Q: Can I use Azure CDN with Live Streaming?</span></span>

<span data-ttu-id="ab5de-116">V: Media Services támogatja az Azure CDN integrációja (további információkért lásd: [adatfolyam-továbbítási végpontok kezelése egy Media Services-fiók hogyan](media-services-portal-manage-streaming-endpoints.md)).</span><span class="sxs-lookup"><span data-stu-id="ab5de-116">A: Media Services supports integration with Azure CDN (for more information, see [How to Manage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)).</span></span>  <span data-ttu-id="ab5de-117">Live streaming CDN is használhatja.</span><span class="sxs-lookup"><span data-stu-id="ab5de-117">You can use Live streaming with CDN.</span></span> <span data-ttu-id="ab5de-118">Az Azure Media Services Smooth Streaming, HLS és MPEG-DASH kimenetek biztosít.</span><span class="sxs-lookup"><span data-stu-id="ab5de-118">Azure Media Services provides Smooth Streaming, HLS and MPEG-DASH outputs.</span></span> <span data-ttu-id="ab5de-119">Ezek a formátumok adatok átvitele a HTTP Protokollt használja, és a HTTP-gyorsítótárazás előnyök.</span><span class="sxs-lookup"><span data-stu-id="ab5de-119">All these formats use HTTP for transferring data and get benefits of HTTP caching.</span></span> <span data-ttu-id="ab5de-120">Élő adatfolyam tényleges videó/hang adatok felosztásának a töredékeket, és az egyes töredék beolvasása gyorsítótárazza a CDN.</span><span class="sxs-lookup"><span data-stu-id="ab5de-120">In live streaming actual video/audio data is divided to fragments and this individual fragments get cached in CDN.</span></span> <span data-ttu-id="ab5de-121">Csak adattárolási igényeinek frissíteni kell az jegyzék adatai.</span><span class="sxs-lookup"><span data-stu-id="ab5de-121">Only data needs to be refreshed is the manifest data.</span></span> <span data-ttu-id="ab5de-122">CDN rendszeresen frissülnek a jegyzék adatokat.</span><span class="sxs-lookup"><span data-stu-id="ab5de-122">CDN periodically refreshes manifest data.</span></span>

<span data-ttu-id="ab5de-123">K: Does Azure Media services támogatja a tárolni lemezképeket?</span><span class="sxs-lookup"><span data-stu-id="ab5de-123">Q: Does Azure Media services support storing images?</span></span>

<span data-ttu-id="ab5de-124">A:, ha most szeretne JPEG vagy PNG lemezképeket menteni, azokat az Azure Blob Storage legyen.</span><span class="sxs-lookup"><span data-stu-id="ab5de-124">A: If you are just looking to store JPEG or PNG images, you should keep those in Azure Blob Storage.</span></span> <span data-ttu-id="ab5de-125">Nincs a abba a Media Services-fiók kivéve, ha meg szeretné tartani ezeket a videó vagy a hang eszközök társított előnye.</span><span class="sxs-lookup"><span data-stu-id="ab5de-125">There is no benefit to putting them in your Media Services account unless you want to keep them associated with your Video or Audio Assets.</span></span> <span data-ttu-id="ab5de-126">Vagy előfordulhat, hogy a képek használják a videókódoló az átfedések kell. Media Encoder Standard felirataként Képek videók felett, és, hogy mi felsorolja JPEG és PNG támogatott formátumok bemeneti.</span><span class="sxs-lookup"><span data-stu-id="ab5de-126">Or if you might have a need to use the images as overlays in the video encoder.Media Encoder Standard supports overlaying images on top of videos, and that is what it lists JPEG and PNG as supported input formats.</span></span> <span data-ttu-id="ab5de-127">További információkért lásd: [létrehozása átfedések](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="ab5de-127">For more information, see [Creating Overlays](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

<span data-ttu-id="ab5de-128">K: Hogyan tudom átmásolhatja eszközök egy Media Services-fiók egy másikra.</span><span class="sxs-lookup"><span data-stu-id="ab5de-128">Q: How can I copy assets from one Media Services account to another.</span></span>

<span data-ttu-id="ab5de-129">V: eszközök másolhat egy Media Services-fiók egy másikra történő a .NET, használatával [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) kiterjesztésmetódus érhető el a [Azure Media Services .NET SDK-bővítmények](https://github.com/Azure/azure-sdk-for-media-services-extensions/) tárházba.</span><span class="sxs-lookup"><span data-stu-id="ab5de-129">A: To copy assets from one Media Services account to another using .NET, use [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extension method available in the [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) repository.</span></span> <span data-ttu-id="ab5de-130">További információkért lásd: [ez](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) fórum szál.</span><span class="sxs-lookup"><span data-stu-id="ab5de-130">For more information, see [this](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread.</span></span>

<span data-ttu-id="ab5de-131">K: Mik azok a fájlok elnevezési az AMS használatakor a támogatott karakterekből álló?</span><span class="sxs-lookup"><span data-stu-id="ab5de-131">Q: What are the supported characters for naming files when working with AMS?</span></span>

<span data-ttu-id="ab5de-132">V: Media Services a IAssetFile.Name tulajdonság értékét használja, amikor az adatfolyam-tartalmak (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) a URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="ab5de-132">A: Media Services uses the value of the IAssetFile.Name property when building URLs for the streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="ab5de-133">Értékét a **neve** tulajdonság nem lehet a következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="ab5de-133">The value of the **Name** property cannot have any of the following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="ab5de-134">Emellett csak lehet egy "."</span><span class="sxs-lookup"><span data-stu-id="ab5de-134">Also, there can only be one ‘.’</span></span> <span data-ttu-id="ab5de-135">a fájlnévkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="ab5de-135">for the file name extension.</span></span>

<span data-ttu-id="ab5de-136">K: hogyan csatlakozzon a többi használatával?</span><span class="sxs-lookup"><span data-stu-id="ab5de-136">Q: How to connect using REST?</span></span>

<span data-ttu-id="ab5de-137">V: információ az AMS API-hoz kapcsolódáshoz: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="ab5de-137">A: For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> <span data-ttu-id="ab5de-138">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="ab5de-138">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="ab5de-139">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="ab5de-139">You must make subsequent calls to the new URI.</span></span> 

<span data-ttu-id="ab5de-140">K: hogyan lehet videó elforgatása a kódolási során.</span><span class="sxs-lookup"><span data-stu-id="ab5de-140">Q: How can I rotate a video during the encoding process.</span></span>

<span data-ttu-id="ab5de-141">Válasz: a [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) elforgatási szög által 90/180 vagy 270 támogatja.</span><span class="sxs-lookup"><span data-stu-id="ab5de-141">A: The [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 90/180/270.</span></span> <span data-ttu-id="ab5de-142">Az alapértelmezett viselkedés az "Auto", ha megkísérli a Elforgatás metaadatok észlelése a bejövő MP4/MOV fájlban, és ellensúlyozza a azt.</span><span class="sxs-lookup"><span data-stu-id="ab5de-142">The default behavior is "Auto", where it tries to detect the rotation metadata in the incoming MP4/MOV file and compensate for it.</span></span> <span data-ttu-id="ab5de-143">Adja meg a következőket **források** elemben, amely a megadott json-készletek egyikét [Itt](media-services-mes-presets-overview.md):</span><span class="sxs-lookup"><span data-stu-id="ab5de-143">Include the following **Sources** element to one of the json presets defined [here](media-services-mes-presets-overview.md):</span></span>

    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...


## <a name="media-services-learning-paths"></a><span data-ttu-id="ab5de-144">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="ab5de-144">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ab5de-145">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="ab5de-145">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

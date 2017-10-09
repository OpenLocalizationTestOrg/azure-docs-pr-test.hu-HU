---
title: "Gyakori kérdések a Media Services aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 6d48a5c1291f3c2559d8445921d571718d0a0a6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions"></a><span data-ttu-id="bbd09-103">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="bbd09-103">Frequently asked questions</span></span>

<span data-ttu-id="bbd09-104">Ez a cikk foglalkozik hello Azure Media Services (AMS) felhasználói Közösség által kiváltott gyakran ismételt kérdések.</span><span class="sxs-lookup"><span data-stu-id="bbd09-104">This article addresses frequently asked questions raised by hello Azure Media Services (AMS) user community.</span></span>

## <a name="general-ams-faqs"></a><span data-ttu-id="bbd09-105">Általános AMS – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="bbd09-105">General AMS FAQs</span></span>
<span data-ttu-id="bbd09-106">K: hogyan, méretezhető, indexelő?</span><span class="sxs-lookup"><span data-stu-id="bbd09-106">Q: How do you scale indexing?</span></span>

<span data-ttu-id="bbd09-107">V: hello szolgáltatás számára fenntartott egység vannak hello azonos kódolás és indexelési feladatok.</span><span class="sxs-lookup"><span data-stu-id="bbd09-107">A: hello reserved units are hello same for Encoding and Indexing tasks.</span></span> <span data-ttu-id="bbd09-108">Kövesse az utasításokat [hogyan tooScale kódoláshoz fenntartott egységek](media-services-scale-media-processing-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bbd09-108">Follow instructions on [How tooScale Encoding Reserved Units](media-services-scale-media-processing-overview.md).</span></span> <span data-ttu-id="bbd09-109">**Megjegyzés:** , hogy az indexelő teljesítmény nem érinti a fenntartott egység típusát.</span><span class="sxs-lookup"><span data-stu-id="bbd09-109">**Note** that Indexer performance is not affected by Reserved Unit Type.</span></span>

<span data-ttu-id="bbd09-110">K: I feltöltött, kódolt és videó közzé.</span><span class="sxs-lookup"><span data-stu-id="bbd09-110">Q: I uploaded, encoded, and published a video.</span></span> <span data-ttu-id="bbd09-111">Mi lehet hello OK hello videó nem tölt meg toostream azt?</span><span class="sxs-lookup"><span data-stu-id="bbd09-111">What would be hello reason hello video does not play when I try toostream it?</span></span>

<span data-ttu-id="bbd09-112">V: egyik leggyakoribb oka, nem rendelkezik, amelyből a hello tooplayback kívánt streamvégpontra hello hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="bbd09-112">A: One of hello most common reasons is you do not have hello streaming endpoint from which you are trying tooplayback in hello **Running** state.</span></span>  

<span data-ttu-id="bbd09-113">K: feladatokat lehet elvégezni egy élő adatfolyam összeállítás?</span><span class="sxs-lookup"><span data-stu-id="bbd09-113">Q: Can I do compositing on a live stream?</span></span>

<span data-ttu-id="bbd09-114">A: az élő adatfolyamok összeállítás jelenleg nem érhető el Azure Media Services, ezért létre kell toopre-állítható össze a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bbd09-114">A: Compositing on live streams is currently not offered in Azure Media Services, so you would need toopre-compose on your computer.</span></span>

<span data-ttu-id="bbd09-115">K: használhatok Azure CDN élő adatfolyam-továbbítási?</span><span class="sxs-lookup"><span data-stu-id="bbd09-115">Q: Can I use Azure CDN with Live Streaming?</span></span>

<span data-ttu-id="bbd09-116">V: Media Services támogatja az Azure CDN integrációja (további információkért lásd: [hogyan tooManage adatfolyam-továbbítási végpontok Media Services-fiók](media-services-portal-manage-streaming-endpoints.md)).</span><span class="sxs-lookup"><span data-stu-id="bbd09-116">A: Media Services supports integration with Azure CDN (for more information, see [How tooManage Streaming Endpoints in a Media Services Account](media-services-portal-manage-streaming-endpoints.md)).</span></span>  <span data-ttu-id="bbd09-117">Live streaming CDN is használhatja.</span><span class="sxs-lookup"><span data-stu-id="bbd09-117">You can use Live streaming with CDN.</span></span> <span data-ttu-id="bbd09-118">Az Azure Media Services Smooth Streaming, HLS és MPEG-DASH kimenetek biztosít.</span><span class="sxs-lookup"><span data-stu-id="bbd09-118">Azure Media Services provides Smooth Streaming, HLS and MPEG-DASH outputs.</span></span> <span data-ttu-id="bbd09-119">Ezek a formátumok adatok átvitele a HTTP Protokollt használja, és a HTTP-gyorsítótárazás előnyök.</span><span class="sxs-lookup"><span data-stu-id="bbd09-119">All these formats use HTTP for transferring data and get benefits of HTTP caching.</span></span> <span data-ttu-id="bbd09-120">Az élő adatfolyam tényleges videó/hang osztott toofragments és az egyes töredék beolvasása gyorsítótárazza a CDN.</span><span class="sxs-lookup"><span data-stu-id="bbd09-120">In live streaming actual video/audio data is divided toofragments and this individual fragments get cached in CDN.</span></span> <span data-ttu-id="bbd09-121">Csak adatok igényeinek toobe frissíteni az hello jegyzék adatai.</span><span class="sxs-lookup"><span data-stu-id="bbd09-121">Only data needs toobe refreshed is hello manifest data.</span></span> <span data-ttu-id="bbd09-122">CDN rendszeresen frissülnek a jegyzék adatokat.</span><span class="sxs-lookup"><span data-stu-id="bbd09-122">CDN periodically refreshes manifest data.</span></span>

<span data-ttu-id="bbd09-123">K: Does Azure Media services támogatja a tárolni lemezképeket?</span><span class="sxs-lookup"><span data-stu-id="bbd09-123">Q: Does Azure Media services support storing images?</span></span>

<span data-ttu-id="bbd09-124">V: Ha csupán toostore JPEG vagy PNG-fájlok, az Azure Blob Storage legyen.</span><span class="sxs-lookup"><span data-stu-id="bbd09-124">A: If you are just looking toostore JPEG or PNG images, you should keep those in Azure Blob Storage.</span></span> <span data-ttu-id="bbd09-125">Nincs juttatás tooputting őket a Media Services fiók, kivéve, ha azt szeretné, hogy azokat a videó vagy a hang eszközök társított tookeep van.</span><span class="sxs-lookup"><span data-stu-id="bbd09-125">There is no benefit tooputting them in your Media Services account unless you want tookeep them associated with your Video or Audio Assets.</span></span> <span data-ttu-id="bbd09-126">Vagy ha lehetséges, hogy a szükséges toouse hello lemezképek, a hello videókódoló átfedések. Media Encoder Standard felirataként Képek videók felett, és, hogy mi felsorolja JPEG és PNG támogatott formátumok bemeneti.</span><span class="sxs-lookup"><span data-stu-id="bbd09-126">Or if you might have a need toouse hello images as overlays in hello video encoder.Media Encoder Standard supports overlaying images on top of videos, and that is what it lists JPEG and PNG as supported input formats.</span></span> <span data-ttu-id="bbd09-127">További információkért lásd: [létrehozása átfedések](media-services-advanced-encoding-with-mes.md#overlay).</span><span class="sxs-lookup"><span data-stu-id="bbd09-127">For more information, see [Creating Overlays](media-services-advanced-encoding-with-mes.md#overlay).</span></span>

<span data-ttu-id="bbd09-128">K: hogyan tudja másolni a Media Services-fiók egy tooanother eszközök.</span><span class="sxs-lookup"><span data-stu-id="bbd09-128">Q: How can I copy assets from one Media Services account tooanother.</span></span>

<span data-ttu-id="bbd09-129">A: a Media Services-fiók egy tooanother használata a .NET, toocopy eszközök használata [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) hello elérhető kiterjesztésmetódus [Azure Media Services .NET SDK-bővítmények](https://github.com/Azure/azure-sdk-for-media-services-extensions/) tárházba.</span><span class="sxs-lookup"><span data-stu-id="bbd09-129">A: toocopy assets from one Media Services account tooanother using .NET, use [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extension method available in hello [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) repository.</span></span> <span data-ttu-id="bbd09-130">További információkért lásd: [ez](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) fórum szál.</span><span class="sxs-lookup"><span data-stu-id="bbd09-130">For more information, see [this](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread.</span></span>

<span data-ttu-id="bbd09-131">K: milyen hello támogatott karaktereket az AMS használatakor a fájlok elnevezési?</span><span class="sxs-lookup"><span data-stu-id="bbd09-131">Q: What are hello supported characters for naming files when working with AMS?</span></span>

<span data-ttu-id="bbd09-132">V: Media Services hello hello IAssetFile.Name tulajdonság értékének használja, amikor a hello adatfolyam-tartalmat (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="bbd09-132">A: Media Services uses hello value of hello IAssetFile.Name property when building URLs for hello streaming content (for example, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) For this reason, percent-encoding is not allowed.</span></span> <span data-ttu-id="bbd09-133">hello értékének hello **neve** tulajdonság nem lehet hello következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ".</span><span class="sxs-lookup"><span data-stu-id="bbd09-133">hello value of hello **Name** property cannot have any of hello following [percent-encoding-reserved characters](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]".</span></span> <span data-ttu-id="bbd09-134">Emellett csak lehet egy "."</span><span class="sxs-lookup"><span data-stu-id="bbd09-134">Also, there can only be one ‘.’</span></span> <span data-ttu-id="bbd09-135">hello fájlnévkiterjesztés.</span><span class="sxs-lookup"><span data-stu-id="bbd09-135">for hello file name extension.</span></span>

<span data-ttu-id="bbd09-136">K: hogyan tooconnect használatával REST?</span><span class="sxs-lookup"><span data-stu-id="bbd09-136">Q: How tooconnect using REST?</span></span>

<span data-ttu-id="bbd09-137">V: kapcsolatos információk hogyan tooconnect toohello AMS API-ról: [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="bbd09-137">A: For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> <span data-ttu-id="bbd09-138">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="bbd09-138">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="bbd09-139">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="bbd09-139">You must make subsequent calls toohello new URI.</span></span> 

<span data-ttu-id="bbd09-140">K: hogyan lehet videó elforgatása hello kódolási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="bbd09-140">Q: How can I rotate a video during hello encoding process.</span></span>

<span data-ttu-id="bbd09-141">V: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) elforgatási szög által 90/180 vagy 270 támogatja.</span><span class="sxs-lookup"><span data-stu-id="bbd09-141">A: hello [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) supports rotation by angles of 90/180/270.</span></span> <span data-ttu-id="bbd09-142">hello alapértelmezés lesz az "Auto", ahol az megpróbál toodetect hello Elforgatás metaadatok hello bejövő MP4/MOV fájlban, és ellensúlyozza a azt.</span><span class="sxs-lookup"><span data-stu-id="bbd09-142">hello default behavior is "Auto", where it tries toodetect hello rotation metadata in hello incoming MP4/MOV file and compensate for it.</span></span> <span data-ttu-id="bbd09-143">Adja meg a következőket hello **források** elem tooone hello json készleteket definiált [Itt](media-services-mes-presets-overview.md):</span><span class="sxs-lookup"><span data-stu-id="bbd09-143">Include hello following **Sources** element tooone of hello json presets defined [here](media-services-mes-presets-overview.md):</span></span>

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


## <a name="media-services-learning-paths"></a><span data-ttu-id="bbd09-144">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="bbd09-144">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bbd09-145">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="bbd09-145">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

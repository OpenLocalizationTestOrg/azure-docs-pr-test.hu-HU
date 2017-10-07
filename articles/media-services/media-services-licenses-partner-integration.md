---
title: aaaUsing partnerek toodeliver Widevine-licencek tooAzure Media Services |} Microsoft Docs
description: "Ez a cikk ismerteti, hogyan használhatja az Azure Media Services (AMS) toodeliver olyan adatfolyamra, amely dinamikusan titkosítja a PlayReady vagy a Widevine DRMs AMS. hello PlayReady licenc Media Services PlayReady licenckiszolgáló származik, és Widevine-licenc castLabs licenckiszolgáló hozta."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5bcad5a4-c0bb-4871-9cce-808a913c53e6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 3c18a8a22ced239931dea5385020194bd6d83f28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-partners-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="0a751-104">Partnerek toodeliver Widevine-licencek tooAzure Media Services használatával</span><span class="sxs-lookup"><span data-stu-id="0a751-104">Using partners toodeliver Widevine licenses tooAzure Media Services</span></span>
## <a name="overview"></a><span data-ttu-id="0a751-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0a751-105">Overview</span></span>
<span data-ttu-id="0a751-106">A Microsoft Azure Media Services lehetővé teszi, hogy a toodeliver, MPEG-DASH védelme a Widevine DRM-Védelemmel, amelyek általános titkosítási (CENC) specifikáció hello titkosítása.</span><span class="sxs-lookup"><span data-stu-id="0a751-106">Microsoft Azure Media Services enables you toodeliver MPEG-DASH protected with Widevine DRM, which is encrypted per hello Common Encryption (CENC) specification.</span></span>

<span data-ttu-id="0a751-107">Hello Media Services .NET SDK verzió 3.5.2 verziótól kezdődően a Media Services lehetővé teszi, hogy Ön tooconfigure Widevine-licencsablon és Widevine-licencek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="0a751-107">Starting with hello Media Services .NET SDK version 3.5.2, Media Services enables you tooconfigure Widevine license template and get Widevine licenses.</span></span> <span data-ttu-id="0a751-108">Használhatja a következő AMS-partnereket toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="0a751-108">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

## <a name="castlabs"></a><span data-ttu-id="0a751-109">castLabs</span><span class="sxs-lookup"><span data-stu-id="0a751-109">castLabs</span></span>
<span data-ttu-id="0a751-110">Használhat [castLabs](http://castlabs.com/company/partners/azure/) toodeliver Widevine-licencek.</span><span class="sxs-lookup"><span data-stu-id="0a751-110">You can use [castLabs](http://castlabs.com/company/partners/azure/) toodeliver Widevine licenses.</span></span> <span data-ttu-id="0a751-111">További információkért lásd: [castLabs használatával toodeliver DRM licencek tooAzure Media Services](media-services-castlabs-integration.md)</span><span class="sxs-lookup"><span data-stu-id="0a751-111">For more information, see [Using castLabs toodeliver DRM licenses tooAzure Media Services](media-services-castlabs-integration.md)</span></span>

## <a name="axinom"></a><span data-ttu-id="0a751-112">Axinom</span><span class="sxs-lookup"><span data-stu-id="0a751-112">Axinom</span></span>
<span data-ttu-id="0a751-113">Használhat [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) toodeliver Widevine-licencek.</span><span class="sxs-lookup"><span data-stu-id="0a751-113">You can use [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) toodeliver Widevine licenses.</span></span> <span data-ttu-id="0a751-114">További információkért lásd: [használatával Axinom toodeliver DRM-licencek tooAzure Media Services](media-services-axinom-integration.md)</span><span class="sxs-lookup"><span data-stu-id="0a751-114">For more information, see [Using Axinom toodeliver DRM licenses tooAzure Media Services](media-services-axinom-integration.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="0a751-115">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="0a751-115">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="0a751-116">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="0a751-116">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="0a751-117">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0a751-117">See also</span></span>
[<span data-ttu-id="0a751-118">A PlayReady és/vagy Widevine Dynamic Common Encryption titkosítás használata</span><span class="sxs-lookup"><span data-stu-id="0a751-118">Using PlayReady and/or Widevine dynamic common encryption</span></span>](media-services-protect-with-drm.md)

[<span data-ttu-id="0a751-119">Mingfei's blog</span><span class="sxs-lookup"><span data-stu-id="0a751-119">Mingfei’s blog</span></span>](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)


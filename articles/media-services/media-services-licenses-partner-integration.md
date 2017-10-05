---
title: "Widevine-licencek kézbesíthet Azure Media Services használatával a partnerek |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan használható az Azure Media Services (AMS) által a PlayReady vagy a Widevine DRMs AMS dinamikusan titkosított adatfolyam továbbítására. A PlayReady-licenc Media Services PlayReady licenckiszolgáló származik, és Widevine-licenc castLabs licenckiszolgáló hozta."
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
ms.openlocfilehash: 6867e4f910970121df3858516c6bab3114c3c6f9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a><span data-ttu-id="a557a-104">Partnerek használata a Widevine licencek kézbesítéséhez az Azure Media Services szolgáltatásba</span><span class="sxs-lookup"><span data-stu-id="a557a-104">Using partners to deliver Widevine licenses to Azure Media Services</span></span>
## <a name="overview"></a><span data-ttu-id="a557a-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a557a-105">Overview</span></span>
<span data-ttu-id="a557a-106">A Microsoft Azure Media Services lehetővé teszi, hogy MPEG-DASH védelme a Widevine DRM-Védelemmel, amely titkosítása a Common Encryption (CENC) megadását.</span><span class="sxs-lookup"><span data-stu-id="a557a-106">Microsoft Azure Media Services enables you to deliver MPEG-DASH protected with Widevine DRM, which is encrypted per the Common Encryption (CENC) specification.</span></span>

<span data-ttu-id="a557a-107">A Media Services .NET SDK verzió 3.5.2 verziótól kezdődően a Media Services lehetővé teszi a Widevine-licencsablon konfigurálása és Widevine-licencek.</span><span class="sxs-lookup"><span data-stu-id="a557a-107">Starting with the Media Services .NET SDK version 3.5.2, Media Services enables you to configure Widevine license template and get Widevine licenses.</span></span> <span data-ttu-id="a557a-108">A Widevine-licencek továbbításának támogatásához a következő AMS-partnereket is használhatja: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) vagy [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="a557a-108">You can also use the following AMS partners to help you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

## <a name="castlabs"></a><span data-ttu-id="a557a-109">castLabs</span><span class="sxs-lookup"><span data-stu-id="a557a-109">castLabs</span></span>
<span data-ttu-id="a557a-110">Használhat [castLabs](http://castlabs.com/company/partners/azure/) a Widevine-licencek.</span><span class="sxs-lookup"><span data-stu-id="a557a-110">You can use [castLabs](http://castlabs.com/company/partners/azure/) to deliver Widevine licenses.</span></span> <span data-ttu-id="a557a-111">További információkért lásd: [Azure Media Services használatával castLabs képes biztosítani a DRM licencek](media-services-castlabs-integration.md)</span><span class="sxs-lookup"><span data-stu-id="a557a-111">For more information, see [Using castLabs to deliver DRM licenses to Azure Media Services](media-services-castlabs-integration.md)</span></span>

## <a name="axinom"></a><span data-ttu-id="a557a-112">Axinom</span><span class="sxs-lookup"><span data-stu-id="a557a-112">Axinom</span></span>
<span data-ttu-id="a557a-113">Használhat [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) a Widevine-licencek.</span><span class="sxs-lookup"><span data-stu-id="a557a-113">You can use [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) to deliver Widevine licenses.</span></span> <span data-ttu-id="a557a-114">További információkért lásd: [Axinom használatával képes biztosítani a DRM licencek Azure Media Services szolgáltatáshoz](media-services-axinom-integration.md)</span><span class="sxs-lookup"><span data-stu-id="a557a-114">For more information, see [Using Axinom to deliver DRM licenses to Azure Media Services](media-services-axinom-integration.md)</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="a557a-115">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="a557a-115">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="a557a-116">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="a557a-116">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="a557a-117">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a557a-117">See also</span></span>
[<span data-ttu-id="a557a-118">A PlayReady és/vagy Widevine Dynamic Common Encryption titkosítás használata</span><span class="sxs-lookup"><span data-stu-id="a557a-118">Using PlayReady and/or Widevine dynamic common encryption</span></span>](media-services-protect-with-drm.md)

[<span data-ttu-id="a557a-119">Mingfei's blog</span><span class="sxs-lookup"><span data-stu-id="a557a-119">Mingfei’s blog</span></span>](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)


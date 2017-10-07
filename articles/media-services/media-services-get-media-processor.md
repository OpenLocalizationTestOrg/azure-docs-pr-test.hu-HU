---
title: "egy media feldolgozó használt aaaHow tooCreate .NET hello Azure Media Services SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy adathordozó processzor összetevő tooencode formátum konvertálni, titkosítani, illetve visszafejteni a médiatartalom Azure Media Services. Kódminták C# nyelven íródtak, és a Media Services SDK hello használata a .NET-hez."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: dbf9496f-c6f0-42a7-aa36-70f89dcb8ea2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako
ms.openlocfilehash: f133565cc1321d366013f17302adc8bc7585b251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-get-a-media-processor-instance"></a><span data-ttu-id="1e5ff-104">Útmutató: a Media Processzorpéldány beolvasása</span><span class="sxs-lookup"><span data-stu-id="1e5ff-104">How to: Get a Media Processor Instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1e5ff-105">.NET</span><span class="sxs-lookup"><span data-stu-id="1e5ff-105">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="1e5ff-106">REST</span><span class="sxs-lookup"><span data-stu-id="1e5ff-106">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="1e5ff-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1e5ff-107">Overview</span></span>
<span data-ttu-id="1e5ff-108">A Media Services media processzort összetevő, amely kezeli a különleges feldolgozási feladat, például a kódolása titkosítása vagy visszafejtése médiatartalom az átalakítás formátumban.</span><span class="sxs-lookup"><span data-stu-id="1e5ff-108">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="1e5ff-109">Általában létrehozásakor media processzort egy feladat tooencode létrehozásakor, titkosítására, illetve hello formátum médiatartalom konvertálni.</span><span class="sxs-lookup"><span data-stu-id="1e5ff-109">You typically create a media processor when you are creating a task tooencode, encrypt, or convert hello format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="1e5ff-110">Az Azure media processzor</span><span class="sxs-lookup"><span data-stu-id="1e5ff-110">Azure media processors</span></span> 

<span data-ttu-id="1e5ff-111">a következő témakör hello sorolja fel az adathordozó processzorok:</span><span class="sxs-lookup"><span data-stu-id="1e5ff-111">hello following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="1e5ff-112">Kódolási media processzor</span><span class="sxs-lookup"><span data-stu-id="1e5ff-112">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="1e5ff-113">Elemzés media processzor</span><span class="sxs-lookup"><span data-stu-id="1e5ff-113">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a><span data-ttu-id="1e5ff-114">Media processzor beolvasása</span><span class="sxs-lookup"><span data-stu-id="1e5ff-114">Get Media Processor</span></span>

<span data-ttu-id="1e5ff-115">a következő módszert mutat be hogyan hello tooget egy media processzorpéldány.</span><span class="sxs-lookup"><span data-stu-id="1e5ff-115">hello following method shows how tooget a media processor instance.</span></span> <span data-ttu-id="1e5ff-116">hello Kódpélda feltételezi, hogy a modul szintű változó nevű hello használata **_context** tooreference hello kiszolgálói környezetbe hello szakaszban leírt módon [hogyan: csatlakozás tooMedia programozott módon szolgáltatások](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="1e5ff-116">hello code example assumes hello use of a module-level variable named **_context** tooreference hello server context as described in hello section [How to: Connect tooMedia Services Programmatically](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="1e5ff-117">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="1e5ff-117">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="1e5ff-118">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="1e5ff-118">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="1e5ff-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e5ff-119">Next Steps</span></span>
<span data-ttu-id="1e5ff-120">Most, hogy hogyan nyissa meg a media processzorpéldány tooget toohello [hogyan tooEncode egy eszköz](media-services-dotnet-encode-with-media-encoder-standard.md) témakör, amely bemutatja, hogyan toouse hello Media Encoder Standard tooencode egy eszköz.</span><span class="sxs-lookup"><span data-stu-id="1e5ff-120">Now that you know how tooget a media processor instance, go toohello [How tooEncode an Asset](media-services-dotnet-encode-with-media-encoder-standard.md) topic which will show you how toouse hello Media Encoder Standard tooencode an asset.</span></span>


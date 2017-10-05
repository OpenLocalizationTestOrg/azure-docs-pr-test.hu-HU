---
title: "Az Azure Media Services SDK használatával a .NET-keretrendszerhez készült media processzort létrehozása |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy adathordozó feldolgozó összetevő kódolni, formátum konvertálni, titkosítása vagy visszafejtése médiatartalom Azure Media Services. Kódminták C# nyelven íródtak, és a Media Services SDK használata a .NET-hez."
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
ms.openlocfilehash: c2cbe41b71afa8acc184f9d7f4cfe94686de783e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-get-a-media-processor-instance"></a><span data-ttu-id="3d977-104">Útmutató: a Media Processzorpéldány beolvasása</span><span class="sxs-lookup"><span data-stu-id="3d977-104">How to: Get a Media Processor Instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3d977-105">.NET</span><span class="sxs-lookup"><span data-stu-id="3d977-105">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="3d977-106">REST</span><span class="sxs-lookup"><span data-stu-id="3d977-106">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="3d977-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3d977-107">Overview</span></span>
<span data-ttu-id="3d977-108">A Media Services media processzort összetevő, amely kezeli a különleges feldolgozási feladat, például a kódolása titkosítása vagy visszafejtése médiatartalom az átalakítás formátumban.</span><span class="sxs-lookup"><span data-stu-id="3d977-108">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="3d977-109">Általában létrehozhat egy adathordozó processzor kódolása, titkosítása vagy alakítsa át a formátum a médiatartalom feladat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3d977-109">You typically create a media processor when you are creating a task to encode, encrypt, or convert the format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="3d977-110">Az Azure media processzor</span><span class="sxs-lookup"><span data-stu-id="3d977-110">Azure media processors</span></span> 

<span data-ttu-id="3d977-111">A következő témakör sorolja fel az adathordozó processzorok:</span><span class="sxs-lookup"><span data-stu-id="3d977-111">The following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="3d977-112">Kódolási media processzor</span><span class="sxs-lookup"><span data-stu-id="3d977-112">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="3d977-113">Elemzés media processzor</span><span class="sxs-lookup"><span data-stu-id="3d977-113">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

## <a name="get-media-processor"></a><span data-ttu-id="3d977-114">Media processzor beolvasása</span><span class="sxs-lookup"><span data-stu-id="3d977-114">Get Media Processor</span></span>

<span data-ttu-id="3d977-115">A következő metódus hogyan kérhet egy adathordozó processzorpéldány jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3d977-115">The following method shows how to get a media processor instance.</span></span> <span data-ttu-id="3d977-116">A Kódpélda feltételezi nevű modul szintű változó használatát **_context** kiszolgálói környezetbe hivatkozni, a szakaszban leírt módon [Útmutató: Media Services programokon keresztül csatlakozni](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="3d977-116">The code example assumes the use of a module-level variable named **_context** to reference the server context as described in the section [How to: Connect to Media Services Programmatically](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

        return processor;
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="3d977-117">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3d977-117">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3d977-118">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3d977-118">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="3d977-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d977-119">Next Steps</span></span>
<span data-ttu-id="3d977-120">Most, hogy hogyan kérhet egy adathordozó processzorpéldány ismeri, navigáljon a [egy eszköz kódolással](media-services-dotnet-encode-with-media-encoder-standard.md) témakör, amely tartalmazza a Media Encoder Standard segítségével egy eszköz kódolása.</span><span class="sxs-lookup"><span data-stu-id="3d977-120">Now that you know how to get a media processor instance, go to the [How to Encode an Asset](media-services-dotnet-encode-with-media-encoder-standard.md) topic which will show you how to use the Media Encoder Standard to encode an asset.</span></span>


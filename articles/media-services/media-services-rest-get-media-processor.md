---
title: "Hogyan REST-példány using Media processzor tooget aaa |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy adathordozó processzor összetevő tooencode formátum konvertálni, titkosítani, illetve visszafejteni a médiatartalom Azure Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f9ff1997-0da6-4528-aaed-792837e5be41
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 9f423648ab73c90405c64895ce0f5b6a457862e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-a-media-processor-instance"></a><span data-ttu-id="908c1-103">Hogyan tooget Media processzorpéldány</span><span class="sxs-lookup"><span data-stu-id="908c1-103">How tooget a Media Processor instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="908c1-104">.NET</span><span class="sxs-lookup"><span data-stu-id="908c1-104">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="908c1-105">REST</span><span class="sxs-lookup"><span data-stu-id="908c1-105">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="908c1-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="908c1-106">Overview</span></span>
<span data-ttu-id="908c1-107">A Media Services media processzort összetevő, amely kezeli a különleges feldolgozási feladat, például a kódolása titkosítása vagy visszafejtése médiatartalom az átalakítás formátumban.</span><span class="sxs-lookup"><span data-stu-id="908c1-107">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="908c1-108">Általában létrehozásakor media processzort egy feladat tooencode létrehozásakor, titkosítására, illetve hello formátum médiatartalom konvertálni.</span><span class="sxs-lookup"><span data-stu-id="908c1-108">You typically create a media processor when you are creating a task tooencode, encrypt, or convert hello format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="908c1-109">Az Azure media processzor</span><span class="sxs-lookup"><span data-stu-id="908c1-109">Azure media processors</span></span> 

<span data-ttu-id="908c1-110">a következő témakör hello sorolja fel az adathordozó processzorok:</span><span class="sxs-lookup"><span data-stu-id="908c1-110">hello following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="908c1-111">Kódolási media processzor</span><span class="sxs-lookup"><span data-stu-id="908c1-111">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="908c1-112">Elemzés media processzor</span><span class="sxs-lookup"><span data-stu-id="908c1-112">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
><span data-ttu-id="908c1-113">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="908c1-113">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="908c1-114">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="908c1-114">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="908c1-115">Connect tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="908c1-115">Connect tooMedia Services</span></span>

<span data-ttu-id="908c1-116">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="908c1-116">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="908c1-117">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="908c1-117">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="908c1-118">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="908c1-118">You must make subsequent calls toohello new URI.</span></span>

## <a name="get-a-media-processor"></a><span data-ttu-id="908c1-119">Egy media processzor beolvasása</span><span class="sxs-lookup"><span data-stu-id="908c1-119">Get a media processor</span></span>

<span data-ttu-id="908c1-120">REST-hívást a következő hello bemutatja, hogyan tooget media processzort példány neve (ebben az esetben **Media Encoder Standard**).</span><span class="sxs-lookup"><span data-stu-id="908c1-120">hello following REST call shows how tooget a media processor instance by name (in this case, **Media Encoder Standard**).</span></span> 

<span data-ttu-id="908c1-121">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="908c1-121">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="908c1-122">Válasz:</span><span class="sxs-lookup"><span data-stu-id="908c1-122">Response:</span></span>

    . . .

    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="908c1-123">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="908c1-123">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="908c1-124">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="908c1-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="908c1-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="908c1-125">Next Steps</span></span>
<span data-ttu-id="908c1-126">Most, hogy hogyan nyissa meg a media processzorpéldány tooget toohello [hogyan tooEncode egy eszköz](media-services-rest-get-started.md) témakör, amely bemutatja, hogyan toouse hello Media Encoder Standard tooencode egy eszköz.</span><span class="sxs-lookup"><span data-stu-id="908c1-126">Now that you know how tooget a media processor instance, go toohello [How tooEncode an Asset](media-services-rest-get-started.md) topic which will show you how toouse hello Media Encoder Standard tooencode an asset.</span></span>


---
title: "Hogyan kérhet egy adathordozó használatával REST processzorpéldány |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy adathordozó feldolgozó összetevő kódolni, formátum konvertálni, titkosítása vagy visszafejtése médiatartalom Azure Media Services."
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
ms.openlocfilehash: 4ad90ad979c5bd74fc55155098c88d5c13cb12e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-get-a-media-processor-instance"></a><span data-ttu-id="7cc48-103">Egy Media processzorpéldány beszerzése</span><span class="sxs-lookup"><span data-stu-id="7cc48-103">How to get a Media Processor instance</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7cc48-104">.NET</span><span class="sxs-lookup"><span data-stu-id="7cc48-104">.NET</span></span>](media-services-get-media-processor.md)
> * [<span data-ttu-id="7cc48-105">REST</span><span class="sxs-lookup"><span data-stu-id="7cc48-105">REST</span></span>](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="7cc48-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7cc48-106">Overview</span></span>
<span data-ttu-id="7cc48-107">A Media Services media processzort összetevő, amely kezeli a különleges feldolgozási feladat, például a kódolása titkosítása vagy visszafejtése médiatartalom az átalakítás formátumban.</span><span class="sxs-lookup"><span data-stu-id="7cc48-107">In Media Services a media processor is a component that handles a specific processing task, such as encoding, format conversion, encrypting, or decrypting media content.</span></span> <span data-ttu-id="7cc48-108">Általában létrehozhat egy adathordozó processzor kódolása, titkosítása vagy alakítsa át a formátum a médiatartalom feladat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7cc48-108">You typically create a media processor when you are creating a task to encode, encrypt, or convert the format of media content.</span></span>

## <a name="azure-media-processors"></a><span data-ttu-id="7cc48-109">Az Azure media processzor</span><span class="sxs-lookup"><span data-stu-id="7cc48-109">Azure media processors</span></span> 

<span data-ttu-id="7cc48-110">A következő témakör sorolja fel az adathordozó processzorok:</span><span class="sxs-lookup"><span data-stu-id="7cc48-110">The following topic provides lists of media processors:</span></span>

* [<span data-ttu-id="7cc48-111">Kódolási media processzor</span><span class="sxs-lookup"><span data-stu-id="7cc48-111">Encoding media processors</span></span>](scenarios-and-availability.md#encoding-media-processors)
* [<span data-ttu-id="7cc48-112">Elemzés media processzor</span><span class="sxs-lookup"><span data-stu-id="7cc48-112">Analytics media processors</span></span>](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
><span data-ttu-id="7cc48-113">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="7cc48-113">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="7cc48-114">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="7cc48-114">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="7cc48-115">Kapcsolódás a Media Services szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="7cc48-115">Connect to Media Services</span></span>

<span data-ttu-id="7cc48-116">Az AMS API-hoz kapcsolódáshoz információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="7cc48-116">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="7cc48-117">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="7cc48-117">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="7cc48-118">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="7cc48-118">You must make subsequent calls to the new URI.</span></span>

## <a name="get-a-media-processor"></a><span data-ttu-id="7cc48-119">Egy media processzor beolvasása</span><span class="sxs-lookup"><span data-stu-id="7cc48-119">Get a media processor</span></span>

<span data-ttu-id="7cc48-120">A következő REST-hívást bemutatja, hogyan kérhet egy adathordozó processzor-példány nevét (ebben az esetben **Media Encoder Standard**).</span><span class="sxs-lookup"><span data-stu-id="7cc48-120">The following REST call shows how to get a media processor instance by name (in this case, **Media Encoder Standard**).</span></span> 

<span data-ttu-id="7cc48-121">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="7cc48-121">Request:</span></span>

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

<span data-ttu-id="7cc48-122">Válasz:</span><span class="sxs-lookup"><span data-stu-id="7cc48-122">Response:</span></span>

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


## <a name="media-services-learning-paths"></a><span data-ttu-id="7cc48-123">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="7cc48-123">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="7cc48-124">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="7cc48-124">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="7cc48-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cc48-125">Next Steps</span></span>
<span data-ttu-id="7cc48-126">Most, hogy hogyan kérhet egy adathordozó processzorpéldány ismeri, navigáljon a [egy eszköz kódolással](media-services-rest-get-started.md) témakör, amely tartalmazza a Media Encoder Standard segítségével egy eszköz kódolása.</span><span class="sxs-lookup"><span data-stu-id="7cc48-126">Now that you know how to get a media processor instance, go to the [How to Encode an Asset](media-services-rest-get-started.md) topic which will show you how to use the Media Encoder Standard to encode an asset.</span></span>


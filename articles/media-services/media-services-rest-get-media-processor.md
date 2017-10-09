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
# <a name="how-tooget-a-media-processor-instance"></a>Hogyan tooget Media processzorpéldány
> [!div class="op_single_selector"]
> * [.NET](media-services-get-media-processor.md)
> * [REST](media-services-rest-get-media-processor.md)
> 
> 

## <a name="overview"></a>Áttekintés
A Media Services media processzort összetevő, amely kezeli a különleges feldolgozási feladat, például a kódolása titkosítása vagy visszafejtése médiatartalom az átalakítás formátumban. Általában létrehozásakor media processzort egy feladat tooencode létrehozásakor, titkosítására, illetve hello formátum médiatartalom konvertálni.

## <a name="azure-media-processors"></a>Az Azure media processzor 

a következő témakör hello sorolja fel az adathordozó processzorok:

* [Kódolási media processzor](scenarios-and-availability.md#encoding-media-processors)
* [Elemzés media processzor](scenarios-and-availability.md#analytics-media-processors)

>[!NOTE]
>A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre. További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).

## <a name="connect-toomedia-services"></a>Connect tooMedia szolgáltatások

Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.

## <a name="get-a-media-processor"></a>Egy media processzor beolvasása

REST-hívást a következő hello bemutatja, hogyan tooget media processzort példány neve (ebben az esetben **Media Encoder Standard**). 

A kérelem:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net

Válasz:

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


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Következő lépések
Most, hogy hogyan nyissa meg a media processzorpéldány tooget toohello [hogyan tooEncode egy eszköz](media-services-rest-get-started.md) témakör, amely bemutatja, hogyan toouse hello Media Encoder Standard tooencode egy eszköz.


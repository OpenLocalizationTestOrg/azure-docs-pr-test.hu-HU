---
title: "az Azure Media Services REST API aaaCreating szűrők |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toocreate szűrők, az ügyfél használhassa őket toostream konkrét szakaszokra az adatfolyam. A Media Services hoz létre dinamikus jegyzékfájlokban tooachieve a szelektív streaming."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f7d23daf-7cd2-49c7-a195-ab902912ab3c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: d0b5af3b193b35f22ac70887963c2f0a06b60bde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a>Szűrők létrehozásakor az Azure Media Services REST API-n
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

2.11 kiadástól kezdve a Media Services lehetővé teszi az eszközök toodefine szűrők. Ezek a szűrők, amelyek lehetővé teszik az ügyfelek toochoose toodo többek között a kiszolgáló oldalán szabályok: lejátszás videó (lejátszása helyett hello teljes videó), csak egy részét, vagy adjon meg, hogy a felhasználói eszköz képes kezelni (hang- és interpretációk csak egy részét minden hello interpretációk helyett társított hello eszköz). Ez a szűrés a eszközök archivált keresztül **dinamikus Manifest**a megadott szűrő alapján létrehozott, a felhasználói kérelem toostream videó s.

Részletesebb információk kapcsolódó toofilters és dinamikus Manifest: [dinamikus jelentkezik áttekintése](media-services-dynamic-manifest-overview.md).

Ez a témakör bemutatja, hogyan toouse REST API-k toocreate, frissítési és törlési szűrők. 

## <a name="types-used-toocreate-filters"></a>Toocreate szűrők használni
hello következő típusok használhatók szűrők létrehozásakor:  

* [Szűrő](https://docs.microsoft.com/rest/api/media/operations/filter)
* [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [FilterTrackSelect és FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

>A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre. További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).

## <a name="connect-toomedia-services"></a>Connect tooMedia szolgáltatások

Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.

## <a name="create-filters"></a>Szűrők létrehozása
### <a name="create-global-filters"></a>Globális szűrők létrehozása
toocreate globális szűrőként, a következő HTTP-kérelmek hello használata:  

#### <a name="http-request"></a>HTTP-kérelem
Kérelem fejlécei

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host:media.windows.net 

Kérés törzsében 

    {  
       "Name":"GlobalFilter",
       "PresentationTimeRange":{  
          "StartTimestamp":"0",
          "EndTimestamp":"9223372036854775807",
          "PresentationWindowDuration":"12000000000",
          "LiveBackoffDuration":"0",
          "Timescale":"10000000"
       },
       "Tracks":[  
          {  
             "PropertyConditions":
                  [  
                {  
                   "Property":"Type",
                   "Value":"audio",
                   "Operator":"Equal"
                },
                {  
                   "Property":"Bitrate",
                   "Value":"0-2147483647",
                   "Operator":"Equal"
                }
             ]
          }
       ]
    }




#### <a name="http-response"></a>HTTP-válasz
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a>Helyi AssetFilters létrehozása
egy helyi AssetFilter toocreate HTTP-kérelmekre a következő hello használata:  

#### <a name="http-request"></a>HTTP-kérelem
Kérelem fejlécei

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net  

Kérés törzsében 

    {   
       "Name":"AssetFilter", 
       "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
       "PresentationTimeRange":{   
          "StartTimestamp":"0", 
          "EndTimestamp":"9223372036854775807", 
          "PresentationWindowDuration":"12000000000", 
          "LiveBackoffDuration":"0", 
          "Timescale":"10000000" 
       }, 
       "Tracks":[   
          {   
             "PropertyConditions": 
                  [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

#### <a name="http-response"></a>HTTP-válasz
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a>Szűrők megjelenítése
### <a name="get-all-global-filters-in-hello-ams-account"></a>Minden globális beolvasása **szűrő**hello AMS-fiók létrehozása
toolist szűrők, a következő HTTP-kérelmek hello használata: 

#### <a name="http-request"></a>HTTP-kérelem
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a>Első **AssetFilter**egy eszközhöz társított s
#### <a name="http-request"></a>HTTP-kérelem
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a>Első egy **AssetFilter** az azonosítója alapján
#### <a name="http-request"></a>HTTP-kérelem
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a>Frissítés szűrők
Használjon javítás, PUT vagy EGYESÍTÉS tooupdate új tulajdonságértékek egy szűrő.  További információ ezekről a műveletekről: [javítás, PUT, EGYESÍTSE](http://msdn.microsoft.com/library/dd541276.aspx).

Ha frissíti a szűrőt, streaming endpoint toorefresh hello szabályok too2 percig is eltarthat. Ha hello tartalom állítása és kiszolgálása között szűrővel (és proxyk és a CDN a gyorsítótárba helyezett gyorsítótárak), player hibák frissítése a szűrő eredményezhet. Javasoljuk, tooclear hello gyorsítótár hello szűrő frissítése után. Ha ezt a beállítást nem lehetséges, érdemes lehet egy másik szűrőt.  

### <a name="update-global-filters"></a>Globális szűrők frissítése
tooupdate globális szűrőként, a következő HTTP-kérelmek hello használata: 

#### <a name="http-request"></a>HTTP-kérelem
Kérelem fejlécei: 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    Content-Length: 384

A kérelem törzse: 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

### <a name="update-local-assetfilters"></a>Helyi AssetFilters frissítése
tooupdate helyi szűrőt, a következő HTTP-kérelmek hello használata: 

#### <a name="http-request"></a>HTTP-kérelem
Kérelem fejlécei: 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

A kérelem törzse: 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 


## <a name="delete-filters"></a>Szűrők törlése
### <a name="delete-global-filters"></a>Globális szűrők törlése
toodelete globális szűrőként, a következő HTTP-kérelmek hello használata:

#### <a name="http-request"></a>HTTP-kérelem
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a>Helyi AssetFilters törlése
egy helyi AssetFilter toodelete HTTP-kérelmekre a következő hello használata:

#### <a name="http-request"></a>HTTP-kérelem
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a>Build a streaming URL-szűrők használata
Hogyan toopublish és kézbesítése az eszközök: kapcsolatos [tartalom továbbítása tooCustomers áttekintése](media-services-deliver-content-overview.md).

hello következő példák azt szemléltetik, hogyan tooadd szűrők tooyour streamelési URL-címek.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP élő adatfolyam-továbbítási (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP élő adatfolyam-továbbítási (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[Dinamikus jegyzékfájlokban áttekintése](media-services-dynamic-manifest-overview.md)


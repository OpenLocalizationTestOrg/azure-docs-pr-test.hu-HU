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
# <a name="creating-filters-with-azure-media-services-rest-api"></a><span data-ttu-id="3bad8-104">Szűrők létrehozásakor az Azure Media Services REST API-n</span><span class="sxs-lookup"><span data-stu-id="3bad8-104">Creating Filters with Azure Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3bad8-105">.NET</span><span class="sxs-lookup"><span data-stu-id="3bad8-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="3bad8-106">REST</span><span class="sxs-lookup"><span data-stu-id="3bad8-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="3bad8-107">2.11 kiadástól kezdve a Media Services lehetővé teszi az eszközök toodefine szűrők.</span><span class="sxs-lookup"><span data-stu-id="3bad8-107">Starting with 2.11 release, Media Services enables you toodefine filters for your assets.</span></span> <span data-ttu-id="3bad8-108">Ezek a szűrők, amelyek lehetővé teszik az ügyfelek toochoose toodo többek között a kiszolgáló oldalán szabályok: lejátszás videó (lejátszása helyett hello teljes videó), csak egy részét, vagy adjon meg, hogy a felhasználói eszköz képes kezelni (hang- és interpretációk csak egy részét minden hello interpretációk helyett társított hello eszköz).</span><span class="sxs-lookup"><span data-stu-id="3bad8-108">These filters are server side rules that will allow your customers toochoose toodo things like: playback only a section of a video (instead of playing hello whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all hello renditions that are associated with hello asset).</span></span> <span data-ttu-id="3bad8-109">Ez a szűrés a eszközök archivált keresztül **dinamikus Manifest**a megadott szűrő alapján létrehozott, a felhasználói kérelem toostream videó s.</span><span class="sxs-lookup"><span data-stu-id="3bad8-109">This filtering of your assets is archived through **Dynamic Manifest**s that are created upon your customer's request toostream a video based on specified filter(s).</span></span>

<span data-ttu-id="3bad8-110">Részletesebb információk kapcsolódó toofilters és dinamikus Manifest: [dinamikus jelentkezik áttekintése](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3bad8-110">For more detailed information related toofilters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="3bad8-111">Ez a témakör bemutatja, hogyan toouse REST API-k toocreate, frissítési és törlési szűrők.</span><span class="sxs-lookup"><span data-stu-id="3bad8-111">This topic shows how toouse REST APIs toocreate, update, and delete filters.</span></span> 

## <a name="types-used-toocreate-filters"></a><span data-ttu-id="3bad8-112">Toocreate szűrők használni</span><span class="sxs-lookup"><span data-stu-id="3bad8-112">Types used toocreate filters</span></span>
<span data-ttu-id="3bad8-113">hello következő típusok használhatók szűrők létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="3bad8-113">hello following types are used when creating filters:</span></span>  

* [<span data-ttu-id="3bad8-114">Szűrő</span><span class="sxs-lookup"><span data-stu-id="3bad8-114">Filter</span></span>](https://docs.microsoft.com/rest/api/media/operations/filter)
* [<span data-ttu-id="3bad8-115">AssetFilter</span><span class="sxs-lookup"><span data-stu-id="3bad8-115">AssetFilter</span></span>](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [<span data-ttu-id="3bad8-116">PresentationTimeRange</span><span class="sxs-lookup"><span data-stu-id="3bad8-116">PresentationTimeRange</span></span>](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [<span data-ttu-id="3bad8-117">FilterTrackSelect és FilterTrackPropertyCondition</span><span class="sxs-lookup"><span data-stu-id="3bad8-117">FilterTrackSelect and FilterTrackPropertyCondition</span></span>](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

><span data-ttu-id="3bad8-118">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="3bad8-118">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="3bad8-119">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="3bad8-119">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="3bad8-120">Connect tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3bad8-120">Connect tooMedia Services</span></span>

<span data-ttu-id="3bad8-121">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="3bad8-121">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="3bad8-122">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="3bad8-122">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="3bad8-123">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="3bad8-123">You must make subsequent calls toohello new URI.</span></span>

## <a name="create-filters"></a><span data-ttu-id="3bad8-124">Szűrők létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bad8-124">Create filters</span></span>
### <a name="create-global-filters"></a><span data-ttu-id="3bad8-125">Globális szűrők létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bad8-125">Create global Filters</span></span>
<span data-ttu-id="3bad8-126">toocreate globális szűrőként, a következő HTTP-kérelmek hello használata:</span><span class="sxs-lookup"><span data-stu-id="3bad8-126">toocreate a global Filter, use hello following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="3bad8-127">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-127">HTTP Request</span></span>
<span data-ttu-id="3bad8-128">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="3bad8-128">Request Headers</span></span>

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

<span data-ttu-id="3bad8-129">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="3bad8-129">Request body</span></span> 

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




#### <a name="http-response"></a><span data-ttu-id="3bad8-130">HTTP-válasz</span><span class="sxs-lookup"><span data-stu-id="3bad8-130">HTTP Response</span></span>
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a><span data-ttu-id="3bad8-131">Helyi AssetFilters létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bad8-131">Create local AssetFilters</span></span>
<span data-ttu-id="3bad8-132">egy helyi AssetFilter toocreate HTTP-kérelmekre a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="3bad8-132">toocreate a local AssetFilter, use hello following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="3bad8-133">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-133">HTTP Request</span></span>
<span data-ttu-id="3bad8-134">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="3bad8-134">Request Headers</span></span>

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

<span data-ttu-id="3bad8-135">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="3bad8-135">Request body</span></span> 

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

#### <a name="http-response"></a><span data-ttu-id="3bad8-136">HTTP-válasz</span><span class="sxs-lookup"><span data-stu-id="3bad8-136">HTTP Response</span></span>
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a><span data-ttu-id="3bad8-137">Szűrők megjelenítése</span><span class="sxs-lookup"><span data-stu-id="3bad8-137">List filters</span></span>
### <a name="get-all-global-filters-in-hello-ams-account"></a><span data-ttu-id="3bad8-138">Minden globális beolvasása **szűrő**hello AMS-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bad8-138">Get all global **Filter**s in hello AMS account</span></span>
<span data-ttu-id="3bad8-139">toolist szűrők, a következő HTTP-kérelmek hello használata:</span><span class="sxs-lookup"><span data-stu-id="3bad8-139">toolist filters, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="3bad8-140">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-140">HTTP Request</span></span>
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a><span data-ttu-id="3bad8-141">Első **AssetFilter**egy eszközhöz társított s</span><span class="sxs-lookup"><span data-stu-id="3bad8-141">Get **AssetFilter**s associated with an asset</span></span>
#### <a name="http-request"></a><span data-ttu-id="3bad8-142">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-142">HTTP Request</span></span>
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a><span data-ttu-id="3bad8-143">Első egy **AssetFilter** az azonosítója alapján</span><span class="sxs-lookup"><span data-stu-id="3bad8-143">Get an **AssetFilter** based on its Id</span></span>
#### <a name="http-request"></a><span data-ttu-id="3bad8-144">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-144">HTTP Request</span></span>
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a><span data-ttu-id="3bad8-145">Frissítés szűrők</span><span class="sxs-lookup"><span data-stu-id="3bad8-145">Update filters</span></span>
<span data-ttu-id="3bad8-146">Használjon javítás, PUT vagy EGYESÍTÉS tooupdate új tulajdonságértékek egy szűrő.</span><span class="sxs-lookup"><span data-stu-id="3bad8-146">Use PATCH, PUT or MERGE tooupdate a filter with new property values.</span></span>  <span data-ttu-id="3bad8-147">További információ ezekről a műveletekről: [javítás, PUT, EGYESÍTSE](http://msdn.microsoft.com/library/dd541276.aspx).</span><span class="sxs-lookup"><span data-stu-id="3bad8-147">For more information about these operations, see [PATCH, PUT, MERGE](http://msdn.microsoft.com/library/dd541276.aspx).</span></span>

<span data-ttu-id="3bad8-148">Ha frissíti a szűrőt, streaming endpoint toorefresh hello szabályok too2 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="3bad8-148">If you update a filter, it can take up too2 minutes for streaming endpoint toorefresh hello rules.</span></span> <span data-ttu-id="3bad8-149">Ha hello tartalom állítása és kiszolgálása között szűrővel (és proxyk és a CDN a gyorsítótárba helyezett gyorsítótárak), player hibák frissítése a szűrő eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="3bad8-149">If hello content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="3bad8-150">Javasoljuk, tooclear hello gyorsítótár hello szűrő frissítése után.</span><span class="sxs-lookup"><span data-stu-id="3bad8-150">It is recommend tooclear hello cache after updating hello filter.</span></span> <span data-ttu-id="3bad8-151">Ha ezt a beállítást nem lehetséges, érdemes lehet egy másik szűrőt.</span><span class="sxs-lookup"><span data-stu-id="3bad8-151">If this option is not possible, consider using a different filter.</span></span>  

### <a name="update-global-filters"></a><span data-ttu-id="3bad8-152">Globális szűrők frissítése</span><span class="sxs-lookup"><span data-stu-id="3bad8-152">Update global Filters</span></span>
<span data-ttu-id="3bad8-153">tooupdate globális szűrőként, a következő HTTP-kérelmek hello használata:</span><span class="sxs-lookup"><span data-stu-id="3bad8-153">tooupdate a global filter, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="3bad8-154">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-154">HTTP Request</span></span>
<span data-ttu-id="3bad8-155">Kérelem fejlécei:</span><span class="sxs-lookup"><span data-stu-id="3bad8-155">Request headers:</span></span> 

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

<span data-ttu-id="3bad8-156">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="3bad8-156">Request body:</span></span> 

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

### <a name="update-local-assetfilters"></a><span data-ttu-id="3bad8-157">Helyi AssetFilters frissítése</span><span class="sxs-lookup"><span data-stu-id="3bad8-157">Update local AssetFilters</span></span>
<span data-ttu-id="3bad8-158">tooupdate helyi szűrőt, a következő HTTP-kérelmek hello használata:</span><span class="sxs-lookup"><span data-stu-id="3bad8-158">tooupdate a local filter, use hello following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="3bad8-159">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-159">HTTP Request</span></span>
<span data-ttu-id="3bad8-160">Kérelem fejlécei:</span><span class="sxs-lookup"><span data-stu-id="3bad8-160">Request headers:</span></span> 

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

<span data-ttu-id="3bad8-161">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="3bad8-161">Request body:</span></span> 

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


## <a name="delete-filters"></a><span data-ttu-id="3bad8-162">Szűrők törlése</span><span class="sxs-lookup"><span data-stu-id="3bad8-162">Delete filters</span></span>
### <a name="delete-global-filters"></a><span data-ttu-id="3bad8-163">Globális szűrők törlése</span><span class="sxs-lookup"><span data-stu-id="3bad8-163">Delete global Filters</span></span>
<span data-ttu-id="3bad8-164">toodelete globális szűrőként, a következő HTTP-kérelmek hello használata:</span><span class="sxs-lookup"><span data-stu-id="3bad8-164">toodelete a global Filter, use hello following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="3bad8-165">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-165">HTTP Request</span></span>
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a><span data-ttu-id="3bad8-166">Helyi AssetFilters törlése</span><span class="sxs-lookup"><span data-stu-id="3bad8-166">Delete local AssetFilters</span></span>
<span data-ttu-id="3bad8-167">egy helyi AssetFilter toodelete HTTP-kérelmekre a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="3bad8-167">toodelete a local AssetFilter, use hello following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="3bad8-168">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="3bad8-168">HTTP Request</span></span>
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="3bad8-169">Build a streaming URL-szűrők használata</span><span class="sxs-lookup"><span data-stu-id="3bad8-169">Build streaming URLs that use filters</span></span>
<span data-ttu-id="3bad8-170">Hogyan toopublish és kézbesítése az eszközök: kapcsolatos [tartalom továbbítása tooCustomers áttekintése](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3bad8-170">For information on how toopublish and deliver your assets, see [Delivering Content tooCustomers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="3bad8-171">hello következő példák azt szemléltetik, hogyan tooadd szűrők tooyour streamelési URL-címek.</span><span class="sxs-lookup"><span data-stu-id="3bad8-171">hello following examples show how tooadd filters tooyour streaming URLs.</span></span>

<span data-ttu-id="3bad8-172">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="3bad8-172">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="3bad8-173">**Apple HTTP élő adatfolyam-továbbítási (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="3bad8-173">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="3bad8-174">**Apple HTTP élő adatfolyam-továbbítási (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="3bad8-174">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="3bad8-175">**Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="3bad8-175">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a><span data-ttu-id="3bad8-176">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3bad8-176">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3bad8-177">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3bad8-177">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="3bad8-178">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3bad8-178">See Also</span></span>
[<span data-ttu-id="3bad8-179">Dinamikus jegyzékfájlokban áttekintése</span><span class="sxs-lookup"><span data-stu-id="3bad8-179">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)


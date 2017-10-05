---
title: "Szűrők létrehozása az Azure Media Services REST API-t |} Microsoft Docs"
description: "Ez a témakör ismerteti, az ügyfél használhassa őket adott szakaszaival adatfolyam adatfolyam-szűrők létrehozásához. A Media Services dinamikus jegyzékfájlokban eléréséhez a szelektív streaming hoz létre."
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
ms.openlocfilehash: 76d2721138668d9f0a908af3fa42840309b068ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a><span data-ttu-id="ebdf6-104">Szűrők létrehozásakor az Azure Media Services REST API-n</span><span class="sxs-lookup"><span data-stu-id="ebdf6-104">Creating Filters with Azure Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ebdf6-105">.NET</span><span class="sxs-lookup"><span data-stu-id="ebdf6-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="ebdf6-106">REST</span><span class="sxs-lookup"><span data-stu-id="ebdf6-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="ebdf6-107">2.11 kiadástól kezdve a Media Services lehetővé teszi az eszközök szűrőit.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-107">Starting with 2.11 release, Media Services enables you to define filters for your assets.</span></span> <span data-ttu-id="ebdf6-108">Ezek a szűrők, amelyek lehetővé teszik az ügyfelek akkor megteheti, többek között a kiszolgáló oldalán szabályok: lejátszás videó (és nem a teljes videó lejátszása) részt vagy a hang- és interpretációk, amelyet a felhasználói eszköz kezelni tud (és nem minden a interpretációk társított adategységet) csak egy részét.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-108">These filters are server side rules that will allow your customers to choose to do things like: playback only a section of a video (instead of playing the whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all the renditions that are associated with the asset).</span></span> <span data-ttu-id="ebdf6-109">Ez a szűrés a eszközök archivált keresztül **dinamikus Manifest**khoz, az ügyfél kérésre videó adatfolyam jönnek létre a megadott szűrő alapján.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-109">This filtering of your assets is archived through **Dynamic Manifest**s that are created upon your customer's request to stream a video based on specified filter(s).</span></span>

<span data-ttu-id="ebdf6-110">Részletesebb szűrőket és dinamikus Manifest kapcsolatos adatokat, lásd: [dinamikus jelentkezik áttekintése](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ebdf6-110">For more detailed information related to filters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="ebdf6-111">Ez a témakör bemutatja, hogyan használható a REST API-k létrehozása, frissítése és törlése szűrők.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-111">This topic shows how to use REST APIs to create, update, and delete filters.</span></span> 

## <a name="types-used-to-create-filters"></a><span data-ttu-id="ebdf6-112">-Szűrők létrehozásához használt</span><span class="sxs-lookup"><span data-stu-id="ebdf6-112">Types used to create filters</span></span>
<span data-ttu-id="ebdf6-113">A következő típusok használhatók szűrők létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-113">The following types are used when creating filters:</span></span>  

* [<span data-ttu-id="ebdf6-114">Szűrő</span><span class="sxs-lookup"><span data-stu-id="ebdf6-114">Filter</span></span>](https://docs.microsoft.com/rest/api/media/operations/filter)
* [<span data-ttu-id="ebdf6-115">AssetFilter</span><span class="sxs-lookup"><span data-stu-id="ebdf6-115">AssetFilter</span></span>](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [<span data-ttu-id="ebdf6-116">PresentationTimeRange</span><span class="sxs-lookup"><span data-stu-id="ebdf6-116">PresentationTimeRange</span></span>](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [<span data-ttu-id="ebdf6-117">FilterTrackSelect és FilterTrackPropertyCondition</span><span class="sxs-lookup"><span data-stu-id="ebdf6-117">FilterTrackSelect and FilterTrackPropertyCondition</span></span>](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

><span data-ttu-id="ebdf6-118">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-118">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="ebdf6-119">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="ebdf6-119">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="ebdf6-120">Kapcsolódás a Media Services szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="ebdf6-120">Connect to Media Services</span></span>

<span data-ttu-id="ebdf6-121">Az AMS API-hoz kapcsolódáshoz információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="ebdf6-121">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="ebdf6-122">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-122">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="ebdf6-123">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-123">You must make subsequent calls to the new URI.</span></span>

## <a name="create-filters"></a><span data-ttu-id="ebdf6-124">Szűrők létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebdf6-124">Create filters</span></span>
### <a name="create-global-filters"></a><span data-ttu-id="ebdf6-125">Globális szűrők létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebdf6-125">Create global Filters</span></span>
<span data-ttu-id="ebdf6-126">Globális szűrőként létrehozásához használja a következő HTTP-kérelmek:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-126">To create a global Filter, use the following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="ebdf6-127">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-127">HTTP Request</span></span>
<span data-ttu-id="ebdf6-128">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="ebdf6-128">Request Headers</span></span>

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

<span data-ttu-id="ebdf6-129">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="ebdf6-129">Request body</span></span> 

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




#### <a name="http-response"></a><span data-ttu-id="ebdf6-130">HTTP-válasz</span><span class="sxs-lookup"><span data-stu-id="ebdf6-130">HTTP Response</span></span>
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a><span data-ttu-id="ebdf6-131">Helyi AssetFilters létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebdf6-131">Create local AssetFilters</span></span>
<span data-ttu-id="ebdf6-132">Egy helyi AssetFilter létrehozásához használja a következő HTTP-kérelmek:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-132">To create a local AssetFilter, use the following HTTP requests:</span></span>  

#### <a name="http-request"></a><span data-ttu-id="ebdf6-133">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-133">HTTP Request</span></span>
<span data-ttu-id="ebdf6-134">Kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="ebdf6-134">Request Headers</span></span>

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

<span data-ttu-id="ebdf6-135">Kérés törzsében</span><span class="sxs-lookup"><span data-stu-id="ebdf6-135">Request body</span></span> 

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

#### <a name="http-response"></a><span data-ttu-id="ebdf6-136">HTTP-válasz</span><span class="sxs-lookup"><span data-stu-id="ebdf6-136">HTTP Response</span></span>
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a><span data-ttu-id="ebdf6-137">Szűrők megjelenítése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-137">List filters</span></span>
### <a name="get-all-global-filters-in-the-ams-account"></a><span data-ttu-id="ebdf6-138">Minden globális beolvasása **szűrő**az AMS-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebdf6-138">Get all global **Filter**s in the AMS account</span></span>
<span data-ttu-id="ebdf6-139">A szűrők listában használja a következő HTTP-kérelmek:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-139">To list filters, use the following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="ebdf6-140">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-140">HTTP Request</span></span>
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a><span data-ttu-id="ebdf6-141">Első **AssetFilter**egy eszközhöz társított s</span><span class="sxs-lookup"><span data-stu-id="ebdf6-141">Get **AssetFilter**s associated with an asset</span></span>
#### <a name="http-request"></a><span data-ttu-id="ebdf6-142">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-142">HTTP Request</span></span>
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a><span data-ttu-id="ebdf6-143">Első egy **AssetFilter** az azonosítója alapján</span><span class="sxs-lookup"><span data-stu-id="ebdf6-143">Get an **AssetFilter** based on its Id</span></span>
#### <a name="http-request"></a><span data-ttu-id="ebdf6-144">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-144">HTTP Request</span></span>
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a><span data-ttu-id="ebdf6-145">Frissítés szűrők</span><span class="sxs-lookup"><span data-stu-id="ebdf6-145">Update filters</span></span>
<span data-ttu-id="ebdf6-146">Használjon javítás, PUT vagy egyesítési szűrő frissítése új tulajdonság értékekkel.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-146">Use PATCH, PUT or MERGE to update a filter with new property values.</span></span>  <span data-ttu-id="ebdf6-147">További információ ezekről a műveletekről: [javítás, PUT, EGYESÍTSE](http://msdn.microsoft.com/library/dd541276.aspx).</span><span class="sxs-lookup"><span data-stu-id="ebdf6-147">For more information about these operations, see [PATCH, PUT, MERGE](http://msdn.microsoft.com/library/dd541276.aspx).</span></span>

<span data-ttu-id="ebdf6-148">Ha frissíti a szűrőt, legfeljebb 2 percet, hogy frissíti a szabályok streamvégpont is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-148">If you update a filter, it can take up to 2 minutes for streaming endpoint to refresh the rules.</span></span> <span data-ttu-id="ebdf6-149">Ha a tartalom állítása és kiszolgálása között szűrővel (és proxyk és a CDN a gyorsítótárba helyezett gyorsítótárak), player hibák frissítése a szűrő eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-149">If the content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="ebdf6-150">A gyorsítótár kiürítése után a szűrő frissítése érdekében ajánlott.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-150">It is recommend to clear the cache after updating the filter.</span></span> <span data-ttu-id="ebdf6-151">Ha ezt a beállítást nem lehetséges, érdemes lehet egy másik szűrőt.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-151">If this option is not possible, consider using a different filter.</span></span>  

### <a name="update-global-filters"></a><span data-ttu-id="ebdf6-152">Globális szűrők frissítése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-152">Update global Filters</span></span>
<span data-ttu-id="ebdf6-153">Globális szűrőként frissítéséhez használja a következő HTTP-kérelmek:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-153">To update a global filter, use the following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="ebdf6-154">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-154">HTTP Request</span></span>
<span data-ttu-id="ebdf6-155">Kérelem fejlécei:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-155">Request headers:</span></span> 

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

<span data-ttu-id="ebdf6-156">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-156">Request body:</span></span> 

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

### <a name="update-local-assetfilters"></a><span data-ttu-id="ebdf6-157">Helyi AssetFilters frissítése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-157">Update local AssetFilters</span></span>
<span data-ttu-id="ebdf6-158">A helyi szűrőt frissítéséhez használja a következő HTTP-kérelmek:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-158">To update a local filter, use the following HTTP requests:</span></span> 

#### <a name="http-request"></a><span data-ttu-id="ebdf6-159">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-159">HTTP Request</span></span>
<span data-ttu-id="ebdf6-160">Kérelem fejlécei:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-160">Request headers:</span></span> 

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

<span data-ttu-id="ebdf6-161">A kérelem törzse:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-161">Request body:</span></span> 

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


## <a name="delete-filters"></a><span data-ttu-id="ebdf6-162">Szűrők törlése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-162">Delete filters</span></span>
### <a name="delete-global-filters"></a><span data-ttu-id="ebdf6-163">Globális szűrők törlése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-163">Delete global Filters</span></span>
<span data-ttu-id="ebdf6-164">Globális szűrőként törléséhez használja a következő HTTP-kérelmek:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-164">To delete a global Filter, use the following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="ebdf6-165">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-165">HTTP Request</span></span>
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a><span data-ttu-id="ebdf6-166">Helyi AssetFilters törlése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-166">Delete local AssetFilters</span></span>
<span data-ttu-id="ebdf6-167">Egy helyi AssetFilter törléséhez használja a következő HTTP-kérelmek:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-167">To delete a local AssetFilter, use the following HTTP requests:</span></span>

#### <a name="http-request"></a><span data-ttu-id="ebdf6-168">HTTP-kérelem</span><span class="sxs-lookup"><span data-stu-id="ebdf6-168">HTTP Request</span></span>
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.11 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="ebdf6-169">Build a streaming URL-szűrők használata</span><span class="sxs-lookup"><span data-stu-id="ebdf6-169">Build streaming URLs that use filters</span></span>
<span data-ttu-id="ebdf6-170">Információ közzétételére, és az eszközök: [ügyfelek áttekintés a tartalom továbbítása](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ebdf6-170">For information on how to publish and deliver your assets, see [Delivering Content to Customers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="ebdf6-171">A következő példák bemutatják, hogyan szűrők hozzáadása a streamelési URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="ebdf6-171">The following examples show how to add filters to your streaming URLs.</span></span>

<span data-ttu-id="ebdf6-172">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="ebdf6-172">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="ebdf6-173">**Apple HTTP élő adatfolyam-továbbítási (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="ebdf6-173">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="ebdf6-174">**Apple HTTP élő adatfolyam-továbbítási (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="ebdf6-174">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="ebdf6-175">**Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="ebdf6-175">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a><span data-ttu-id="ebdf6-176">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="ebdf6-176">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="ebdf6-177">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-177">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="ebdf6-178">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="ebdf6-178">See Also</span></span>
[<span data-ttu-id="ebdf6-179">Dinamikus jegyzékfájlokban áttekintése</span><span class="sxs-lookup"><span data-stu-id="ebdf6-179">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)


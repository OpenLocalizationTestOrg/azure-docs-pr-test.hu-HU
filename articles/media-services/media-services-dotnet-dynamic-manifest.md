---
title: "Azure Media Services .NET SDK-val aaaCreating szűrők"
description: "Ez a témakör ismerteti, hogyan toocreate szűrők, az ügyfél használhassa őket toostream konkrét szakaszokra az adatfolyam. A Media Services hoz létre dinamikus jegyzékfájlokban tooachieve a szelektív streaming."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 16d9497d48ab1d3f841dd97efb0f66016a2435c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a><span data-ttu-id="40d8f-104">Szűrők létrehozása az Azure Media Services .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="40d8f-104">Creating Filters with Azure Media Services .NET SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="40d8f-105">.NET</span><span class="sxs-lookup"><span data-stu-id="40d8f-105">.NET</span></span>](media-services-dotnet-dynamic-manifest.md)
> * [<span data-ttu-id="40d8f-106">REST</span><span class="sxs-lookup"><span data-stu-id="40d8f-106">REST</span></span>](media-services-rest-dynamic-manifest.md)
> 
> 

<span data-ttu-id="40d8f-107">2.11 kiadástól kezdve a Media Services lehetővé teszi az eszközök toodefine szűrők.</span><span class="sxs-lookup"><span data-stu-id="40d8f-107">Starting with 2.11 release, Media Services enables you toodefine filters for your assets.</span></span> <span data-ttu-id="40d8f-108">Ezek a szűrők, amelyek lehetővé teszik az ügyfelek toochoose toodo többek között a kiszolgáló oldalán szabályok: lejátszás videó (lejátszása helyett hello teljes videó), csak egy részét, vagy adjon meg, hogy a felhasználói eszköz képes kezelni (hang- és interpretációk csak egy részét minden hello interpretációk helyett társított hello eszköz).</span><span class="sxs-lookup"><span data-stu-id="40d8f-108">These filters are server side rules that will allow your customers toochoose toodo things like: playback only a section of a video (instead of playing hello whole video), or specify only a subset of audio and video renditions that your customer's device can handle (instead of all hello renditions that are associated with hello asset).</span></span> <span data-ttu-id="40d8f-109">Ez a szűrés a eszközök sorrendekben **dinamikus Manifest**a megadott szűrő alapján létrehozott, a felhasználói kérelem toostream videó s.</span><span class="sxs-lookup"><span data-stu-id="40d8f-109">This filtering of your assets is achieved through **Dynamic Manifest**s that are created upon your customer's request toostream a video based on specified filter(s).</span></span>

<span data-ttu-id="40d8f-110">Részletesebb információk kapcsolódó toofilters és dinamikus Manifest: [dinamikus jelentkezik áttekintése](media-services-dynamic-manifest-overview.md).</span><span class="sxs-lookup"><span data-stu-id="40d8f-110">For more detailed information related toofilters and Dynamic Manifest, see [Dynamic manifests overview](media-services-dynamic-manifest-overview.md).</span></span>

<span data-ttu-id="40d8f-111">Ez a témakör bemutatja, hogyan toouse Media Services .NET SDK toocreate, frissítési és törlési szűrők.</span><span class="sxs-lookup"><span data-stu-id="40d8f-111">This topic shows how toouse Media Services .NET SDK toocreate, update, and delete filters.</span></span> 

<span data-ttu-id="40d8f-112">Vegye figyelembe, ha frissíti a szűrőt, is eltarthat, az adatfolyamként történő végpont toorefresh hello szabályok too2 perc.</span><span class="sxs-lookup"><span data-stu-id="40d8f-112">Note if you update a filter, it can take up too2 minutes for streaming endpoint toorefresh hello rules.</span></span> <span data-ttu-id="40d8f-113">Ha hello tartalom állítása és kiszolgálása között szűrővel (és proxyk és a CDN a gyorsítótárba helyezett gyorsítótárak), player hibák frissítése a szűrő eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="40d8f-113">If hello content was served using this filter (and cached in proxies and CDN caches), updating this filter can result in player failures.</span></span> <span data-ttu-id="40d8f-114">Javasoljuk, tooclear hello gyorsítótár hello szűrő frissítése után.</span><span class="sxs-lookup"><span data-stu-id="40d8f-114">It is recommend tooclear hello cache after updating hello filter.</span></span> <span data-ttu-id="40d8f-115">Ha ezt a beállítást nem lehetséges, érdemes lehet egy másik szűrőt.</span><span class="sxs-lookup"><span data-stu-id="40d8f-115">If this option is not possible, consider using a different filter.</span></span> 

## <a name="types-used-toocreate-filters"></a><span data-ttu-id="40d8f-116">Toocreate szűrők használni</span><span class="sxs-lookup"><span data-stu-id="40d8f-116">Types used toocreate filters</span></span>
<span data-ttu-id="40d8f-117">hello következő típusok használhatók szűrők létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="40d8f-117">hello following types are used when creating filters:</span></span> 

* <span data-ttu-id="40d8f-118">**IStreamingFilter**.</span><span class="sxs-lookup"><span data-stu-id="40d8f-118">**IStreamingFilter**.</span></span>  <span data-ttu-id="40d8f-119">Ez a típus a REST API-t a következő hello alapul [szűrő](https://docs.microsoft.com/rest/api/media/operations/filter)</span><span class="sxs-lookup"><span data-stu-id="40d8f-119">This type is based on hello following REST API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)</span></span>
* <span data-ttu-id="40d8f-120">**IStreamingAssetFilter**.</span><span class="sxs-lookup"><span data-stu-id="40d8f-120">**IStreamingAssetFilter**.</span></span> <span data-ttu-id="40d8f-121">Ez a típus a REST API-t a következő hello alapul [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span><span class="sxs-lookup"><span data-stu-id="40d8f-121">This type is based on hello following REST API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)</span></span>
* <span data-ttu-id="40d8f-122">**PresentationTimeRange**.</span><span class="sxs-lookup"><span data-stu-id="40d8f-122">**PresentationTimeRange**.</span></span> <span data-ttu-id="40d8f-123">Ez a típus a REST API-t a következő hello alapul [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span><span class="sxs-lookup"><span data-stu-id="40d8f-123">This type is based on hello following REST API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)</span></span>
* <span data-ttu-id="40d8f-124">**FilterTrackSelectStatement** és **IFilterTrackPropertyCondition**.</span><span class="sxs-lookup"><span data-stu-id="40d8f-124">**FilterTrackSelectStatement** and **IFilterTrackPropertyCondition**.</span></span> <span data-ttu-id="40d8f-125">Ezek a típusok a REST API-k a következő hello alapuló [FilterTrackSelect és FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span><span class="sxs-lookup"><span data-stu-id="40d8f-125">These types are based on hello following REST APIs [FilterTrackSelect and FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)</span></span>

## <a name="createupdatereaddelete-global-filters"></a><span data-ttu-id="40d8f-126">Létrehozás/frissítés/olvasás/törlés globális szűrők</span><span class="sxs-lookup"><span data-stu-id="40d8f-126">Create/Update/Read/Delete global filters</span></span>
<span data-ttu-id="40d8f-127">hello következő kód bemutatja, hogyan toouse .NET toocreate, frissítése, olvassa el és törlése eszköz szűrők.</span><span class="sxs-lookup"><span data-stu-id="40d8f-127">hello following code shows how toouse .NET toocreate, update,read, and delete asset filters.</span></span>

    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


## <a name="createupdatereaddelete-asset-filters"></a><span data-ttu-id="40d8f-128">Létrehozás/frissítés/olvasás/törlés eszköz szűrők</span><span class="sxs-lookup"><span data-stu-id="40d8f-128">Create/Update/Read/Delete asset filters</span></span>
<span data-ttu-id="40d8f-129">hello következő kód bemutatja, hogyan toouse .NET toocreate, frissítése, olvassa el és törlése eszköz szűrők.</span><span class="sxs-lookup"><span data-stu-id="40d8f-129">hello following code shows how toouse .NET toocreate, update,read, and delete asset filters.</span></span>

    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();




## <a name="build-streaming-urls-that-use-filters"></a><span data-ttu-id="40d8f-130">Build a streaming URL-szűrők használata</span><span class="sxs-lookup"><span data-stu-id="40d8f-130">Build streaming URLs that use filters</span></span>
<span data-ttu-id="40d8f-131">Hogyan toopublish és kézbesítése az eszközök: kapcsolatos [tartalom továbbítása tooCustomers áttekintése](media-services-deliver-content-overview.md).</span><span class="sxs-lookup"><span data-stu-id="40d8f-131">For information on how toopublish and deliver your assets, see [Delivering Content tooCustomers Overview](media-services-deliver-content-overview.md).</span></span>

<span data-ttu-id="40d8f-132">hello következő példák azt szemléltetik, hogyan tooadd szűrők tooyour streamelési URL-címek.</span><span class="sxs-lookup"><span data-stu-id="40d8f-132">hello following examples show how tooadd filters tooyour streaming URLs.</span></span>

<span data-ttu-id="40d8f-133">**MPEG DASH**</span><span class="sxs-lookup"><span data-stu-id="40d8f-133">**MPEG DASH**</span></span> 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

<span data-ttu-id="40d8f-134">**Apple HTTP élő adatfolyam-továbbítási (HLS) V4**</span><span class="sxs-lookup"><span data-stu-id="40d8f-134">**Apple HTTP Live Streaming (HLS) V4**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

<span data-ttu-id="40d8f-135">**Apple HTTP élő adatfolyam-továbbítási (HLS) V3**</span><span class="sxs-lookup"><span data-stu-id="40d8f-135">**Apple HTTP Live Streaming (HLS) V3**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

<span data-ttu-id="40d8f-136">**Smooth Streaming**</span><span class="sxs-lookup"><span data-stu-id="40d8f-136">**Smooth Streaming**</span></span>

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a><span data-ttu-id="40d8f-137">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="40d8f-137">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="40d8f-138">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="40d8f-138">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="40d8f-139">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="40d8f-139">See Also</span></span>
[<span data-ttu-id="40d8f-140">Dinamikus jegyzékfájlokban áttekintése</span><span class="sxs-lookup"><span data-stu-id="40d8f-140">Dynamic manifests overview</span></span>](media-services-dynamic-manifest-overview.md)


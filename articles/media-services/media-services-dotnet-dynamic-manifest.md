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
# <a name="creating-filters-with-azure-media-services-net-sdk"></a>Szűrők létrehozása az Azure Media Services .NET SDK-val
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

2.11 kiadástól kezdve a Media Services lehetővé teszi az eszközök toodefine szűrők. Ezek a szűrők, amelyek lehetővé teszik az ügyfelek toochoose toodo többek között a kiszolgáló oldalán szabályok: lejátszás videó (lejátszása helyett hello teljes videó), csak egy részét, vagy adjon meg, hogy a felhasználói eszköz képes kezelni (hang- és interpretációk csak egy részét minden hello interpretációk helyett társított hello eszköz). Ez a szűrés a eszközök sorrendekben **dinamikus Manifest**a megadott szűrő alapján létrehozott, a felhasználói kérelem toostream videó s.

Részletesebb információk kapcsolódó toofilters és dinamikus Manifest: [dinamikus jelentkezik áttekintése](media-services-dynamic-manifest-overview.md).

Ez a témakör bemutatja, hogyan toouse Media Services .NET SDK toocreate, frissítési és törlési szűrők. 

Vegye figyelembe, ha frissíti a szűrőt, is eltarthat, az adatfolyamként történő végpont toorefresh hello szabályok too2 perc. Ha hello tartalom állítása és kiszolgálása között szűrővel (és proxyk és a CDN a gyorsítótárba helyezett gyorsítótárak), player hibák frissítése a szűrő eredményezhet. Javasoljuk, tooclear hello gyorsítótár hello szűrő frissítése után. Ha ezt a beállítást nem lehetséges, érdemes lehet egy másik szűrőt. 

## <a name="types-used-toocreate-filters"></a>Toocreate szűrők használni
hello következő típusok használhatók szűrők létrehozásakor: 

* **IStreamingFilter**.  Ez a típus a REST API-t a következő hello alapul [szűrő](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Ez a típus a REST API-t a következő hello alapul [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Ez a típus a REST API-t a következő hello alapul [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** és **IFilterTrackPropertyCondition**. Ezek a típusok a REST API-k a következő hello alapuló [FilterTrackSelect és FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Létrehozás/frissítés/olvasás/törlés globális szűrők
hello következő kód bemutatja, hogyan toouse .NET toocreate, frissítése, olvassa el és törlése eszköz szűrők.

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


## <a name="createupdatereaddelete-asset-filters"></a>Létrehozás/frissítés/olvasás/törlés eszköz szűrők
hello következő kód bemutatja, hogyan toouse .NET toocreate, frissítése, olvassa el és törlése eszköz szűrők.

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


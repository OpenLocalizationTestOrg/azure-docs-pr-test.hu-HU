---
title: "Azure Media Services tartalmakat a .NET használatával közzététele |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre egy keresőt a streamelési URL-cím létrehozásához használt. Kódminták C# nyelven íródtak, és a Media Services SDK használata a .NET-hez."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 2bcb012eef84faa7c1e13ed22e88e45e4300ed54
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="publish-azure-media-services-content-using-net"></a><span data-ttu-id="737aa-104">Azure Media Services tartalmakat a .NET használatával közzététele</span><span class="sxs-lookup"><span data-stu-id="737aa-104">Publish Azure Media Services content using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="737aa-105">REST</span><span class="sxs-lookup"><span data-stu-id="737aa-105">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> * [<span data-ttu-id="737aa-106">.NET</span><span class="sxs-lookup"><span data-stu-id="737aa-106">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="737aa-107">Portal</span><span class="sxs-lookup"><span data-stu-id="737aa-107">Portal</span></span>](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="737aa-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="737aa-108">Overview</span></span>
<span data-ttu-id="737aa-109">Egy adaptív sávszélességű MP4 típusú beállításkészlettel egy adatfolyam-továbbítási OnDemand-kereső létrehozásával, és a streamelési URL-cím összeállítása is adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="737aa-109">You can stream an adaptive bitrate MP4 set by creating an OnDemand streaming locator and building a streaming URL.</span></span> <span data-ttu-id="737aa-110">A [egy eszköz kódolás](media-services-encode-asset.md) a témakör bemutatja, hogyan kódolása egy adaptív sávszélességű MP4 állítsa be.</span><span class="sxs-lookup"><span data-stu-id="737aa-110">The [encoding an asset](media-services-encode-asset.md) topic shows how to encode into an adaptive bitrate MP4 set.</span></span> 

> [!NOTE]
> <span data-ttu-id="737aa-111">Ha a tartalom titkosított, objektumtovábbítási szabályzat konfigurálása (leírtak szerint [ez](media-services-dotnet-configure-asset-delivery-policy.md) témakör) egy lokátor létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="737aa-111">If your content is encrypted, configure asset delivery policy (as described in [this](media-services-dotnet-configure-asset-delivery-policy.md) topic) before creating a locator.</span></span> 
> 
> 

<span data-ttu-id="737aa-112">OnDemand-lokátor segítségével is MP4-fájlokat fokozatosan letölthető mutató URL-címek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="737aa-112">You can also use an OnDemand streaming locator to build URLs that point to MP4 files that can be progressively downloaded.</span></span>  

<span data-ttu-id="737aa-113">Ez a témakör bemutatja, hogyan hozzon létre egy OnDemand-lokátor tegye közzé az adategységet, és egy Smooth, MPEG DASH vagy HLS streamelési URL-címek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="737aa-113">This topic shows how to create an OnDemand streaming locator to publish your asset and build a Smooth, MPEG DASH, and HLS streaming URLs.</span></span> <span data-ttu-id="737aa-114">Azt is bemutatja, működés közbeni hozhat létre a progresszív letöltési URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="737aa-114">It also shows hot to build progressive download URLs.</span></span> 

## <a name="create-an-ondemand-streaming-locator"></a><span data-ttu-id="737aa-115">Hozzon létre egy OnDemand-lokátor</span><span class="sxs-lookup"><span data-stu-id="737aa-115">Create an OnDemand streaming locator</span></span>
<span data-ttu-id="737aa-116">Az adatfolyam-továbbítási OnDemand-kereső létrehozása és URL-címek lekérése, meg kell tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="737aa-116">To create the OnDemand streaming locator and get URLs, you need to do the following things:</span></span>

1. <span data-ttu-id="737aa-117">Ha a tartalom titkosított, adja meg a hozzáférési házirendek.</span><span class="sxs-lookup"><span data-stu-id="737aa-117">If the content is encrypted, define an access policy.</span></span>
2. <span data-ttu-id="737aa-118">Hozzon létre egy OnDemand-lokátor.</span><span class="sxs-lookup"><span data-stu-id="737aa-118">Create an OnDemand streaming locator.</span></span>
3. <span data-ttu-id="737aa-119">Ha azt tervezi, hogy adatfolyamként küldje el, beolvasása a folyamatos átviteli jegyzékfájl (.ism) az eszközt.</span><span class="sxs-lookup"><span data-stu-id="737aa-119">If you plan to stream, get the streaming manifest file (.ism) in the asset.</span></span> 
   
   <span data-ttu-id="737aa-120">Ha azt tervezi, fokozatosan letölteni, beolvasása az eszköz a MP4-fájlok nevét.</span><span class="sxs-lookup"><span data-stu-id="737aa-120">If you plan to progressively download, get the names of MP4 files in the asset.</span></span>  
4. <span data-ttu-id="737aa-121">A jegyzékfájl vagy MP4-fájlok összeállítása a URL-címek.</span><span class="sxs-lookup"><span data-stu-id="737aa-121">Build URLs to the manifest file or MP4 files.</span></span> 


>[!NOTE]
><span data-ttu-id="737aa-122">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="737aa-122">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="737aa-123">Az azonos házirend-azonosító akkor használja, ha mindig használja az ugyanazon nap / hozzáférési engedélyek.</span><span class="sxs-lookup"><span data-stu-id="737aa-123">Use the same policy ID if you are always using the same days / access permissions.</span></span> <span data-ttu-id="737aa-124">Például házirendek, amelyek célja, hogy továbbra is érvényben hosszú ideje (nem feltöltés házirendek) lokátorokat.</span><span class="sxs-lookup"><span data-stu-id="737aa-124">For example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="737aa-125">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="737aa-125">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

### <a name="use-media-services-net-sdk"></a><span data-ttu-id="737aa-126">Használja a Media Services .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="737aa-126">Use Media Services .NET SDK</span></span>
<span data-ttu-id="737aa-127">Adatfolyam-továbbítási URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="737aa-127">Build Streaming URLs</span></span> 

    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

<span data-ttu-id="737aa-128">A kimenetek:</span><span class="sxs-lookup"><span data-stu-id="737aa-128">The outputs:</span></span>

    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> <span data-ttu-id="737aa-129">Is SSL-kapcsolaton keresztül adatfolyam formájában a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="737aa-129">You can also stream your content over an SSL connection.</span></span> <span data-ttu-id="737aa-130">Hajtsa végre ezt a módszert használja, ellenőrizze, hogy a HTTPS adatfolyam-továbbítási URL-címek kezdődik.</span><span class="sxs-lookup"><span data-stu-id="737aa-130">To do this approach, make sure your streaming URLs start with HTTPS.</span></span> <span data-ttu-id="737aa-131">Jelenleg az AMS nem támogatja az SSL az egyéni tartomány.</span><span class="sxs-lookup"><span data-stu-id="737aa-131">Currently, AMS doesn’t support SSL with custom domains.</span></span>
> 
> 

<span data-ttu-id="737aa-132">Progresszív letöltési URL-címeket összeállítása</span><span class="sxs-lookup"><span data-stu-id="737aa-132">Build progressive download URLs</span></span> 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

<span data-ttu-id="737aa-133">A kimenetek:</span><span class="sxs-lookup"><span data-stu-id="737aa-133">The outputs:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a><span data-ttu-id="737aa-134">Media Services .NET SDK-bővítmények használata</span><span class="sxs-lookup"><span data-stu-id="737aa-134">Use Media Services .NET SDK Extensions</span></span>
<span data-ttu-id="737aa-135">Az alábbi kód olyan, egy kereső létrehozása, és amelyek adaptív streameléshez a Smooth Streaming, HLS, és MPEG-DASH URL-címek létrehozása .NET SDK bővítmények módszereket hív meg.</span><span class="sxs-lookup"><span data-stu-id="737aa-135">The following code calls .NET SDK extensions methods that create a locator and generate the Smooth Streaming, HLS, and MPEG-DASH URLs for adaptive streaming.</span></span>

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


## <a name="media-services-learning-paths"></a><span data-ttu-id="737aa-136">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="737aa-136">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="737aa-137">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="737aa-137">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="737aa-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="737aa-138">Next steps</span></span>
* [<span data-ttu-id="737aa-139">Töltse le az eszközök</span><span class="sxs-lookup"><span data-stu-id="737aa-139">Download assets</span></span>](media-services-deliver-asset-download.md)
* [<span data-ttu-id="737aa-140">Objektumtovábbítási szabályzat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="737aa-140">Configure asset delivery policy</span></span>](media-services-dotnet-configure-asset-delivery-policy.md)


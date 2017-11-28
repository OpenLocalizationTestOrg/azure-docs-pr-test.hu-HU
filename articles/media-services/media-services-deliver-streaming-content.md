---
title: "aaaPublish Azure Media Services tartalmakat a .NET használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy kereső, amely használt toobuild egy adatfolyam-továbbítási URL-címet. Kódminták C# nyelven íródtak, és a Media Services SDK hello használata a .NET-hez."
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
ms.openlocfilehash: c941cd93c252a96e66546cce2793bb426afac059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-azure-media-services-content-using-net"></a><span data-ttu-id="c8513-104">Azure Media Services tartalmakat a .NET használatával közzététele</span><span class="sxs-lookup"><span data-stu-id="c8513-104">Publish Azure Media Services content using .NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c8513-105">REST</span><span class="sxs-lookup"><span data-stu-id="c8513-105">REST</span></span>](media-services-rest-deliver-streaming-content.md)
> * [<span data-ttu-id="c8513-106">.NET</span><span class="sxs-lookup"><span data-stu-id="c8513-106">.NET</span></span>](media-services-deliver-streaming-content.md)
> * [<span data-ttu-id="c8513-107">Portál</span><span class="sxs-lookup"><span data-stu-id="c8513-107">Portal</span></span>](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="c8513-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c8513-108">Overview</span></span>
<span data-ttu-id="c8513-109">Egy adaptív sávszélességű MP4 típusú beállításkészlettel egy adatfolyam-továbbítási OnDemand-kereső létrehozásával, és a streamelési URL-cím összeállítása is adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="c8513-109">You can stream an adaptive bitrate MP4 set by creating an OnDemand streaming locator and building a streaming URL.</span></span> <span data-ttu-id="c8513-110">Hello [egy eszköz kódolás](media-services-encode-asset.md) a témakör bemutatja, hogyan tooencode be egy adaptív sávszélességű MP4 állíthatja.</span><span class="sxs-lookup"><span data-stu-id="c8513-110">hello [encoding an asset](media-services-encode-asset.md) topic shows how tooencode into an adaptive bitrate MP4 set.</span></span> 

> [!NOTE]
> <span data-ttu-id="c8513-111">Ha a tartalom titkosított, objektumtovábbítási szabályzat konfigurálása (leírtak szerint [ez](media-services-dotnet-configure-asset-delivery-policy.md) témakör) egy lokátor létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="c8513-111">If your content is encrypted, configure asset delivery policy (as described in [this](media-services-dotnet-configure-asset-delivery-policy.md) topic) before creating a locator.</span></span> 
> 
> 

<span data-ttu-id="c8513-112">Streamelési locator toobuild URL-címek, hogy pont tooMP4 fájlokat fokozatosan letölthető OnDemand is használható.</span><span class="sxs-lookup"><span data-stu-id="c8513-112">You can also use an OnDemand streaming locator toobuild URLs that point tooMP4 files that can be progressively downloaded.</span></span>  

<span data-ttu-id="c8513-113">Ez a témakör bemutatja, hogyan toocreate egy OnDemand-lokátor toopublish streaming, az eszköz és -buildek zavartalan, MPEG DASH vagy HLS streamelési URL-címek.</span><span class="sxs-lookup"><span data-stu-id="c8513-113">This topic shows how toocreate an OnDemand streaming locator toopublish your asset and build a Smooth, MPEG DASH, and HLS streaming URLs.</span></span> <span data-ttu-id="c8513-114">Azt is bemutatja, működés közbeni toobuild progresszív letöltési URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="c8513-114">It also shows hot toobuild progressive download URLs.</span></span> 

## <a name="create-an-ondemand-streaming-locator"></a><span data-ttu-id="c8513-115">Hozzon létre egy OnDemand-lokátor</span><span class="sxs-lookup"><span data-stu-id="c8513-115">Create an OnDemand streaming locator</span></span>
<span data-ttu-id="c8513-116">toocreate OnDemand-lokátor hello és URL-címek lekérése, a következő dolgot toodo hello van szüksége:</span><span class="sxs-lookup"><span data-stu-id="c8513-116">toocreate hello OnDemand streaming locator and get URLs, you need toodo hello following things:</span></span>

1. <span data-ttu-id="c8513-117">Ha hello tartalom titkosítása, határozza meg a hozzáférési házirendben.</span><span class="sxs-lookup"><span data-stu-id="c8513-117">If hello content is encrypted, define an access policy.</span></span>
2. <span data-ttu-id="c8513-118">Hozzon létre egy OnDemand-lokátor.</span><span class="sxs-lookup"><span data-stu-id="c8513-118">Create an OnDemand streaming locator.</span></span>
3. <span data-ttu-id="c8513-119">Ha azt tervezi, toostream, get hello streaming hello eszköz a jegyzékfájl (.ism).</span><span class="sxs-lookup"><span data-stu-id="c8513-119">If you plan toostream, get hello streaming manifest file (.ism) in hello asset.</span></span> 
   
   <span data-ttu-id="c8513-120">Ha azt tervezi, tooprogressively letöltési, beolvasása hello eszköz MP4 fájlok hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c8513-120">If you plan tooprogressively download, get hello names of MP4 files in hello asset.</span></span>  
4. <span data-ttu-id="c8513-121">URL-címek toohello jegyzékfájl vagy MP4-fájlok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c8513-121">Build URLs toohello manifest file or MP4 files.</span></span> 


>[!NOTE]
><span data-ttu-id="c8513-122">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="c8513-122">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="c8513-123">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek.</span><span class="sxs-lookup"><span data-stu-id="c8513-123">Use hello same policy ID if you are always using hello same days / access permissions.</span></span> <span data-ttu-id="c8513-124">Például a lokátorokat, amelyek házirendek szánt tooremain helyen hosszú ideig (nem feltöltés házirendek).</span><span class="sxs-lookup"><span data-stu-id="c8513-124">For example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="c8513-125">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="c8513-125">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

### <a name="use-media-services-net-sdk"></a><span data-ttu-id="c8513-126">Használja a Media Services .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="c8513-126">Use Media Services .NET SDK</span></span>
<span data-ttu-id="c8513-127">Adatfolyam-továbbítási URL-címek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8513-127">Build Streaming URLs</span></span> 

    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator toohello streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on hello locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference toohello streaming manifest file from hello  
        // collection of files in hello asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL toohello manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL toomanifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL toomanifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL toomanifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

<span data-ttu-id="c8513-128">hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="c8513-128">hello outputs:</span></span>

    URL toomanifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL toomanifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL toomanifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> <span data-ttu-id="c8513-129">Is SSL-kapcsolaton keresztül adatfolyam formájában a tartalmat.</span><span class="sxs-lookup"><span data-stu-id="c8513-129">You can also stream your content over an SSL connection.</span></span> <span data-ttu-id="c8513-130">toodo ez készíthető elő, ellenőrizze, hogy a HTTPS adatfolyam-továbbítási URL-címek kezdődik.</span><span class="sxs-lookup"><span data-stu-id="c8513-130">toodo this approach, make sure your streaming URLs start with HTTPS.</span></span> <span data-ttu-id="c8513-131">Jelenleg az AMS nem támogatja az SSL az egyéni tartomány.</span><span class="sxs-lookup"><span data-stu-id="c8513-131">Currently, AMS doesn’t support SSL with custom domains.</span></span>
> 
> 

<span data-ttu-id="c8513-132">Progresszív letöltési URL-címeket összeállítása</span><span class="sxs-lookup"><span data-stu-id="c8513-132">Build progressive download URLs</span></span> 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator toohello asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on hello locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL toohello MP4 files. Use this tooprogressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

<span data-ttu-id="c8513-133">hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="c8513-133">hello outputs:</span></span>

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a><span data-ttu-id="c8513-134">Media Services .NET SDK-bővítmények használata</span><span class="sxs-lookup"><span data-stu-id="c8513-134">Use Media Services .NET SDK Extensions</span></span>
<span data-ttu-id="c8513-135">hello következő kód .NET SDK bővítmények olyan módszereket hív meg, amely egy kereső létrehozása, és amelyek adaptív streameléshez hello Smooth Streaming, HLS és MPEG-DASH URL-címeket létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c8513-135">hello following code calls .NET SDK extensions methods that create a locator and generate hello Smooth Streaming, HLS, and MPEG-DASH URLs for adaptive streaming.</span></span>

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get hello streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


## <a name="media-services-learning-paths"></a><span data-ttu-id="c8513-136">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="c8513-136">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="c8513-137">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="c8513-137">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a><span data-ttu-id="c8513-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8513-138">Next steps</span></span>
* [<span data-ttu-id="c8513-139">Töltse le az eszközök</span><span class="sxs-lookup"><span data-stu-id="c8513-139">Download assets</span></span>](media-services-deliver-asset-download.md)
* [<span data-ttu-id="c8513-140">Objektumtovábbítási szabályzat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8513-140">Configure asset delivery policy</span></span>](media-services-dotnet-configure-asset-delivery-policy.md)


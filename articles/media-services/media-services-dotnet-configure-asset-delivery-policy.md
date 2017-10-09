---
title: "aaaConfigure adategység továbbítási házirendjeit .NET SDK-val |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure különböző adategység továbbítási házirendjeit Azure Media Services .NET SDK-val."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 3ec46f58-6cbb-4d49-bac6-1fd01a5a456b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/13/2017
ms.author: juliako;mingfeiy
ms.openlocfilehash: a6f2644d639cd36d4cdc269b6f01fd4acdf7160b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a><span data-ttu-id="57bfe-103">Konfigurálja az adategység továbbítási házirendjeit .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="57bfe-103">Configure asset delivery policies with .NET SDK</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a><span data-ttu-id="57bfe-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="57bfe-104">Overview</span></span>
<span data-ttu-id="57bfe-105">Ha azt tervezi, hogy a titkosított toodelivery eszközök, hello lépések egyikét a hello Media Services content kézbesítési munkafolyamatot továbbítási házirendjeit eszközök konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="57bfe-105">If you plan toodelivery encrypted assets, one of hello steps in hello Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="57bfe-106">hogyan szeretné az eszköz toobe kézbesíteni a közli hello objektumtovábbítási szabályzat Media Services: az adatfolyam-továbbítási protokoll kell az eszköz dinamikusan csomagolható (például MPEG DASH, HLS, Smooth Streaming, vagy az összes), toodynamically szeretné-e az eszköz titkosítása és hogyan (boríték vagy közös titkosítási).</span><span class="sxs-lookup"><span data-stu-id="57bfe-106">hello asset delivery policy tells Media Services how you want for your asset toobe delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want toodynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="57bfe-107">Ez a témakör ismerteti, miért és hogyan toocreate, és konfigurálja az adategység továbbítási házirendjeit.</span><span class="sxs-lookup"><span data-stu-id="57bfe-107">This topic discusses why and how toocreate and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="57bfe-108">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="57bfe-108">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="57bfe-109">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="57bfe-109">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="57bfe-110">Emellett toobe képes toouse dinamikus csomagolás és a dinamikus titkosítás az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét.</span><span class="sxs-lookup"><span data-stu-id="57bfe-110">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>


<span data-ttu-id="57bfe-111">Eltérő házirendek toohello alkalmazhat azonos eszköz.</span><span class="sxs-lookup"><span data-stu-id="57bfe-111">You could apply different policies toohello same asset.</span></span> <span data-ttu-id="57bfe-112">Például alkalmazhat PlayReady titkosítási tooSmooth Streaming és AES Envelope titkosítási tooMPEG DASH vagy HLS.</span><span class="sxs-lookup"><span data-stu-id="57bfe-112">For example, you could apply PlayReady encryption tooSmooth Streaming and AES Envelope encryption tooMPEG DASH and HLS.</span></span> <span data-ttu-id="57bfe-113">Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming.</span><span class="sxs-lookup"><span data-stu-id="57bfe-113">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="57bfe-114">hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán.</span><span class="sxs-lookup"><span data-stu-id="57bfe-114">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="57bfe-115">Ezt követően hello törölje a jelet minden protokoll engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="57bfe-115">Then, all protocols will be allowed in hello clear.</span></span>

<span data-ttu-id="57bfe-116">Ha azt szeretné, hogy a tároló titkosított eszköz toodeliver, konfigurálnia kell a hello adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="57bfe-116">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="57bfe-117">Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="57bfe-117">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="57bfe-118">Például toodeliver az eszköz titkosítva Advanced Encryption Standard (AES) boríték titkosítási kulcs, állítsa be a hello házirend típusa túl**DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="57bfe-118">For example, toodeliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set hello policy type too**DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="57bfe-119">tooremove tárolás titkosítása és az adatfolyam hello eszköz törlése, hello beállítása hello házirend típusa túl**NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="57bfe-119">tooremove storage encryption and stream hello asset in hello clear, set hello policy type too**NoDynamicEncryption**.</span></span> <span data-ttu-id="57bfe-120">Példák hogyan tooconfigure ezeket a házirend-típusainak kövesse.</span><span class="sxs-lookup"><span data-stu-id="57bfe-120">Examples that show how tooconfigure these policy types follow.</span></span>

<span data-ttu-id="57bfe-121">Attól függően, hogyan hello objektumtovábbítási szabályzat konfigurálása, lenne képes toodynamically csomag kell, dinamikusan titkosítani és adatfolyamként küldje el a következő adatfolyam-továbbítási protokollok hello: Smooth Streaming, HLS vagy MPEG DASH adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="57bfe-121">Depending on how you configure hello asset delivery policy you would be able toodynamically package, dynamically encrypt, and stream hello following streaming protocols: Smooth Streaming, HLS, and MPEG DASH streams.</span></span>

<span data-ttu-id="57bfe-122">hello alábbi lista mutatja azokat hello formátumok toostream Smooth, HLS és DASH használatát.</span><span class="sxs-lookup"><span data-stu-id="57bfe-122">hello following list shows hello formats that you use toostream Smooth, HLS, and DASH.</span></span>

<span data-ttu-id="57bfe-123">Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="57bfe-123">Smooth Streaming:</span></span>

<span data-ttu-id="57bfe-124">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="57bfe-124">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="57bfe-125">HLS:</span><span class="sxs-lookup"><span data-stu-id="57bfe-125">HLS:</span></span>

<span data-ttu-id="57bfe-126">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="57bfe-126">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="57bfe-127">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="57bfe-127">MPEG DASH</span></span>

<span data-ttu-id="57bfe-128">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="57bfe-128">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


## <a name="considerations"></a><span data-ttu-id="57bfe-129">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="57bfe-129">Considerations</span></span>
* <span data-ttu-id="57bfe-130">Egy AssetDeliveryPolicy társított egy eszköz, amíg az eszköz számára, hogy létezik egy (adatfolyam) OnDemand-kereső nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="57bfe-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="57bfe-131">hello ajánljuk tooremove hello házirend hello eszközből, hello házirend törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="57bfe-131">hello recommendation is tooremove hello policy from hello asset before deleting hello policy.</span></span>
* <span data-ttu-id="57bfe-132">A streamelési lokátorok létrehozásához egy tárolási titkosított eszköz nem hozható létre, ha nincs objektumtovábbítási szabályzat beállítása.</span><span class="sxs-lookup"><span data-stu-id="57bfe-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="57bfe-133">Ha hello eszköz nem alkalmaz, hello rendszer tájékoztatja lokátor és adatfolyam hello eszköz a hello törlése nélkül objektumtovábbítási szabályzat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="57bfe-133">If hello Asset isn’t storage encrypted, hello system will let you create a locator and stream hello asset in hello clear without an asset delivery policy.</span></span>
* <span data-ttu-id="57bfe-134">Egyetlen eszköz társított több adategység továbbítási házirendjeit is rendelkezik, de csak egyirányú toohandle egy adott AssetDeliveryProtocol adhat meg.</span><span class="sxs-lookup"><span data-stu-id="57bfe-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way toohandle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="57bfe-135">Tehát ha toolink két továbbítási házirendjeit által megadott hello AssetDeliveryProtocol.SmoothStreaming protokoll, amely a hibát okoz, mivel a hello rendszer nem tudja, melyik felel meg szeretné tooapply amikor egy ügyfél Smooth Streaming kérelmet.</span><span class="sxs-lookup"><span data-stu-id="57bfe-135">Meaning if you try toolink two delivery policies that specify hello AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because hello system does not know which one you want it tooapply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="57bfe-136">Ha egy eszköz rendelkezik egy meglévő streamelési locator, egy új házirend toohello eszköz (választható hello eszköz a meglévő házirend, vagy frissítse a továbbítási szabályzatban hello eszközhöz társított), ezért nem csatolható.</span><span class="sxs-lookup"><span data-stu-id="57bfe-136">If you have an asset with an existing streaming locator, you cannot link a new policy toohello asset (you can either unlink an existing policy from hello asset, or update a delivery policy associated with hello asset).</span></span>  <span data-ttu-id="57bfe-137">Ön először tooremove hello streamelési locator rendelkezik, állíthatja hello házirendeket és majd hozza újra létre a lokátor hello.</span><span class="sxs-lookup"><span data-stu-id="57bfe-137">You first have tooremove hello streaming locator, adjust hello policies, and then re-create hello streaming locator.</span></span>  <span data-ttu-id="57bfe-138">Hello azonos locatorId ismételt létrehozásakor streamelési locator, de hello győződjön meg arról, hogy problémákat nem okozhat az ügyfelek, mivel a tartalom gyorsítótárazható hello származási vagy egy alárendelt CDN által használható.</span><span class="sxs-lookup"><span data-stu-id="57bfe-138">You can use hello same locatorId when you recreate hello streaming locator but you should ensure that won’t cause issues for clients since content can be cached by hello origin or a downstream CDN.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="57bfe-139">Objektumtovábbítási szabályzat törlése</span><span class="sxs-lookup"><span data-stu-id="57bfe-139">Clear asset delivery policy</span></span>

<span data-ttu-id="57bfe-140">hello következő **ConfigureClearAssetDeliveryPolicy** módszer határozza meg, toonot alkalmazni a dinamikus titkosítás és toodeliver hello adatfolyam egyik sem szerepel a következő hello protokollok: MPEG DASH, HLS vagy Smooth Streaming protokollokat.</span><span class="sxs-lookup"><span data-stu-id="57bfe-140">hello following **ConfigureClearAssetDeliveryPolicy** method specifies toonot apply dynamic encryption and toodeliver hello stream in any of hello following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> <span data-ttu-id="57bfe-141">Érdemes tooapply a házirend tooyour alkalmaz eszközök.</span><span class="sxs-lookup"><span data-stu-id="57bfe-141">You might want tooapply this policy tooyour storage encrypted assets.</span></span>

<span data-ttu-id="57bfe-142">Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="57bfe-142">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="57bfe-143">Objektumtovábbítási szabályzat DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="57bfe-143">DynamicCommonEncryption asset delivery policy</span></span>

<span data-ttu-id="57bfe-144">hello következő **CreateAssetDeliveryPolicy** hoz létre hello **AssetDeliveryPolicy** , amely konfigurált tooapply a dynamic common encryption (**DynamicCommonEncryption**) a tooa smooth streaming (egyéb protokollok le lesz tiltva streaming) protokollt.</span><span class="sxs-lookup"><span data-stu-id="57bfe-144">hello following **CreateAssetDeliveryPolicy** method creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic common encryption (**DynamicCommonEncryption**) tooa smooth streaming protocol (other protocols will be blocked from streaming).</span></span> <span data-ttu-id="57bfe-145">hello metódus két paramétert fogad: **eszköz** (hello tooapply hello objektumtovábbítási szabályzat kívánt eszköz toowhich) és **IContentKey** (hello tartalomkulcsot a hello **CommonEncryption**típust, további információkért lásd: [tartalomkulcs létrehozása](media-services-dotnet-create-contentkey.md#common_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="57bfe-145">hello method takes two parameters : **Asset** (hello asset toowhich you want tooapply hello delivery policy) and **IContentKey** (hello content key of hello **CommonEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#common_contentkey)).</span></span>

<span data-ttu-id="57bfe-146">Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="57bfe-146">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
        
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
            {
                {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            };
    
            var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                    "AssetDeliveryPolicy",
                AssetDeliveryPolicyType.DynamicCommonEncryption,
                AssetDeliveryProtocol.SmoothStreaming,
                assetDeliveryPolicyConfiguration);
    
            // Add AssetDelivery Policy toohello asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }

<span data-ttu-id="57bfe-147">Az Azure Media Services lehetővé teszi tooadd Widevine titkosítása.</span><span class="sxs-lookup"><span data-stu-id="57bfe-147">Azure Media Services also enables you tooadd Widevine encryption.</span></span> <span data-ttu-id="57bfe-148">hello alábbi példa azt mutatja be mind a PlayReady, mind a Widevine hozzáadni kívánt toohello objektumtovábbítási szabályzat.</span><span class="sxs-lookup"><span data-stu-id="57bfe-148">hello following example demonstrates both PlayReady and Widevine being added toohello asset delivery policy.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get hello PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches hello KID toohello URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // hello WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // tooappend /? KID =< keyId > toohello end of hello url when creating hello manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need tooremove hello KID that in hello URL when we call GetKeyDeliveryUrl.

        Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
        UriBuilder uriBuilder = new UriBuilder(widevineUrl);
        uriBuilder.Query = String.Empty;
        widevineUrl = uriBuilder.Uri;

        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
        {
            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy toohello asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

> [!NOTE]
> <span data-ttu-id="57bfe-149">Widevine titkosításakor csak lenne képes toodeliver kötőjel használatával.</span><span class="sxs-lookup"><span data-stu-id="57bfe-149">When encrypting with Widevine, you would only be able toodeliver using DASH.</span></span> <span data-ttu-id="57bfe-150">Győződjön meg arról, hogy toospecify DASH a hello objektumtovábbítási protokoll.</span><span class="sxs-lookup"><span data-stu-id="57bfe-150">Make sure toospecify DASH in hello asset delivery protocol.</span></span>
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="57bfe-151">Objektumtovábbítási szabályzat DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="57bfe-151">DynamicEnvelopeEncryption asset delivery policy</span></span>
<span data-ttu-id="57bfe-152">hello következő **CreateAssetDeliveryPolicy** hoz létre hello **AssetDeliveryPolicy** , amely a konfigurált tooapply dinamikus boríték titkosítás (**DynamicEnvelopeEncryption** ) tooSmooth Streaming, HLS és kötőjel protokollt (Ha úgy dönt toonot néhány protokoll megadásához le lesznek tiltva a folyamatos átviteli).</span><span class="sxs-lookup"><span data-stu-id="57bfe-152">hello following **CreateAssetDeliveryPolicy** method creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic envelope encryption (**DynamicEnvelopeEncryption**) tooSmooth Streaming, HLS, and DASH protocols (if you decide toonot specify some protocols, they will be blocked from streaming).</span></span> <span data-ttu-id="57bfe-153">hello metódus két paramétert fogad: **eszköz** (hello tooapply hello objektumtovábbítási szabályzat kívánt eszköz toowhich) és **IContentKey** (hello tartalomkulcsot a hello **EnvelopeEncryption**típust, további információkért lásd: [tartalomkulcs létrehozása](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="57bfe-153">hello method takes two parameters : **Asset** (hello asset toowhich you want tooapply hello delivery policy) and **IContentKey** (hello content key of hello **EnvelopeEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span></span>

<span data-ttu-id="57bfe-154">Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="57bfe-154">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get hello Key Delivery Base Url by removing hello Query parameter.  hello Dynamic Encryption service will
        //  automatically add hello correct key identifier toohello url when it generates hello Envelope encrypted content
        //  manifest.  Omitting hello IV will also cause hello Dynamice Encryption service toogenerate a deterministic
        //  IV for hello content automatically.  By using hello EnvelopeBaseKeyAcquisitionUrl and omitting hello IV, this
        //  allows hello AssetDelivery policy toobe reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // hello following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended toohello envelope and
        //   hello Initialization Vector (IV) toouse for hello envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
        };

        IAssetDeliveryPolicy assetDeliveryPolicy =
            _context.AssetDeliveryPolicies.Create(
                        "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                        AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy toohello asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


## <span data-ttu-id="57bfe-155"><a id="types"></a>Típusok AssetDeliveryPolicy definiálásakor használja</span><span class="sxs-lookup"><span data-stu-id="57bfe-155"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <span data-ttu-id="57bfe-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="57bfe-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span></span>

<span data-ttu-id="57bfe-157">hello következő felsorolás ismerteti értékeket is hello objektumtovábbítási protokoll.</span><span class="sxs-lookup"><span data-stu-id="57bfe-157">hello following enum describes values you can set for hello asset delivery protocol.</span></span>

    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        ProgressiveDownload = 0x10, 
 
        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

### <span data-ttu-id="57bfe-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="57bfe-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span></span>

<span data-ttu-id="57bfe-159">hello következő felsorolás ismerteti, állíthatja be hello kézbesítési házirend típusú értékeket.</span><span class="sxs-lookup"><span data-stu-id="57bfe-159">hello following enum describes values you can set for hello asset delivery policy type.</span></span>  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// hello Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption toohello asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

### <span data-ttu-id="57bfe-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="57bfe-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>

<span data-ttu-id="57bfe-161">hello következő felsorolás ismerteti tooconfigure hello a kézbesítési módszer hello tartalom kulcs toohello ügyfél értékeket.</span><span class="sxs-lookup"><span data-stu-id="57bfe-161">hello following enum describes values you can use tooconfigure hello delivery method of hello content key toohello client.</span></span>
    
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

### <span data-ttu-id="57bfe-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="57bfe-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="57bfe-163">a következő felsorolás hello értékeket is tooconfigure használt kulcsok tooget adott konfigurációja objektumtovábbítási szabályzat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="57bfe-163">hello following enum describes values you can set tooconfigure keys used tooget specific configuration for an asset delivery policy.</span></span>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// hello PlayReady License Acquisition Url toouse for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// hello PlayReady Custom Attributes tooadd toohello PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// hello initialization vector toouse for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="57bfe-164">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="57bfe-164">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="57bfe-165">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="57bfe-165">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


---
title: "Konfigurálja az adategység továbbítási házirendjeit .NET SDK-val |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan különböző adategység továbbítási házirendjeit konfigurálása az Azure Media Services .NET SDK-val."
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
ms.openlocfilehash: 282fd9e24dc147e31613469926128894d48366f4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-asset-delivery-policies-with-net-sdk"></a><span data-ttu-id="5c33a-103">Konfigurálja az adategység továbbítási házirendjeit .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="5c33a-103">Configure asset delivery policies with .NET SDK</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a><span data-ttu-id="5c33a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5c33a-104">Overview</span></span>
<span data-ttu-id="5c33a-105">Ha azt tervezi, kézbesítési titkosított eszközökre, a Media Services-továbbítási munkafolyamat lépésben továbbítási házirendjeit eszközök konfigurálását végzi.</span><span class="sxs-lookup"><span data-stu-id="5c33a-105">If you plan to delivery encrypted assets, one of the steps in the Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="5c33a-106">Hogyan szeretné az eszköz kézbesítendő közli az adategység továbbítási házirendjét Media Services: be kell az eszköz dinamikusan csomagolható (például MPEG DASH, HLS, Smooth Streaming, vagy az összes), az eszköz dinamikusan titkosítani szeretné-e, és hogy melyik adatfolyam-protokoll (boríték vagy közös titkosítási).</span><span class="sxs-lookup"><span data-stu-id="5c33a-106">The asset delivery policy tells Media Services how you want for your asset to be delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want to dynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="5c33a-107">Ez a témakör ismerteti, miért és hogyan hozza létre és konfigurálja az adategység továbbítási házirendjeit.</span><span class="sxs-lookup"><span data-stu-id="5c33a-107">This topic discusses why and how to create and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="5c33a-108">Az AMS-fiók létrehozásakor a rendszer hozzáad egy **alapértelmezett** streamvégpontot a fiókhoz **Leállítva** állapotban.</span><span class="sxs-lookup"><span data-stu-id="5c33a-108">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="5c33a-109">A tartalom streamelésének megkezdéséhez, valamint a dinamikus csomagolás és a dinamikus titkosítás kihasználásához a tartalomstreameléshez használt streamvégpontnak **Fut** állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5c33a-109">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
>
><span data-ttu-id="5c33a-110">Is hogy fogja tudni használni a dinamikus csomagolás és a dinamikus titkosítás az objektumot kell foglal magában adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá.</span><span class="sxs-lookup"><span data-stu-id="5c33a-110">Also, to be able to use dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>


<span data-ttu-id="5c33a-111">Eltérő házirendek a azonos eszközhöz alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5c33a-111">You could apply different policies to the same asset.</span></span> <span data-ttu-id="5c33a-112">Például PlayReady-titkosítás beállíthat MPEG DASH vagy HLS, Smooth Streaming és AES Envelope titkosítás.</span><span class="sxs-lookup"><span data-stu-id="5c33a-112">For example, you could apply PlayReady encryption to Smooth Streaming and AES Envelope encryption to MPEG DASH and HLS.</span></span> <span data-ttu-id="5c33a-113">A továbbítási szabályzatban meg nem határozott protokollok streameléshez való használatát a rendszer nem engedélyezi (ilyen lehet például, ha csupán egyetlen szabályzatot állít be, amely kizárólag a HLS-protokoll használatát tartalmazza).</span><span class="sxs-lookup"><span data-stu-id="5c33a-113">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="5c33a-114">Kivételt jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="5c33a-114">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="5c33a-115">Ebben az esetben a rendszer az összes protokollt engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="5c33a-115">Then, all protocols will be allowed in the clear.</span></span>

<span data-ttu-id="5c33a-116">Ha egy tárolási titkosított eszköz kézbesíteni szeretné, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="5c33a-116">If you want to deliver a storage encrypted asset, you must configure the asset’s delivery policy.</span></span> <span data-ttu-id="5c33a-117">Mielőtt az eszköz továbbítható, a streamelési kiszolgáló a tárolás titkosítása eltávolítja, és az adatfolyamokat, a tartalom a megadott objektumtovábbítási szabályzat segítségével.</span><span class="sxs-lookup"><span data-stu-id="5c33a-117">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy.</span></span> <span data-ttu-id="5c33a-118">Például az Advanced Encryption Standard (AES) boríték titkosítási kulccsal titkosított objektumot, hogy állítsa a házirend típusát **DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="5c33a-118">For example, to deliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set the policy type to **DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="5c33a-119">Tárolás titkosítása és adatfolyamként küldje el az eszköz szövegként, állítsa be a házirend típusát **NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="5c33a-119">To remove storage encryption and stream the asset in the clear, set the policy type to **NoDynamicEncryption**.</span></span> <span data-ttu-id="5c33a-120">Az alábbi példák, amelyek bemutatják, hogyan konfigurálhatja ezeket a házirend-típusainak.</span><span class="sxs-lookup"><span data-stu-id="5c33a-120">Examples that show how to configure these policy types follow.</span></span>

<span data-ttu-id="5c33a-121">Attól függően, hogyan konfigurálja az adategység továbbítási házirendjét lehetővé válik a dinamikus csomag dinamikusan titkosítani és adatfolyamként küldje el a következő adatfolyam-továbbítási protokollok: Smooth Streaming, HLS vagy MPEG DASH adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="5c33a-121">Depending on how you configure the asset delivery policy you would be able to dynamically package, dynamically encrypt, and stream the following streaming protocols: Smooth Streaming, HLS, and MPEG DASH streams.</span></span>

<span data-ttu-id="5c33a-122">Az alábbi listában láthatók a formátumok Smooth, HLS és kötőjel adatfolyamként történő küldéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="5c33a-122">The following list shows the formats that you use to stream Smooth, HLS, and DASH.</span></span>

<span data-ttu-id="5c33a-123">Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="5c33a-123">Smooth Streaming:</span></span>

<span data-ttu-id="5c33a-124">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="5c33a-124">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="5c33a-125">HLS:</span><span class="sxs-lookup"><span data-stu-id="5c33a-125">HLS:</span></span>

<span data-ttu-id="5c33a-126">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="5c33a-126">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="5c33a-127">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="5c33a-127">MPEG DASH</span></span>

<span data-ttu-id="5c33a-128">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="5c33a-128">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


## <a name="considerations"></a><span data-ttu-id="5c33a-129">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="5c33a-129">Considerations</span></span>
* <span data-ttu-id="5c33a-130">Egy AssetDeliveryPolicy társított egy eszköz, amíg az eszköz számára, hogy létezik egy (adatfolyam) OnDemand-kereső nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="5c33a-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="5c33a-131">Az ajánljuk, hogy a házirend törlése előtt távolítsa el a házirend az eszköz.</span><span class="sxs-lookup"><span data-stu-id="5c33a-131">The recommendation is to remove the policy from the asset before deleting the policy.</span></span>
* <span data-ttu-id="5c33a-132">A streamelési lokátorok létrehozásához egy tárolási titkosított eszköz nem hozható létre, ha nincs objektumtovábbítási szabályzat beállítása.</span><span class="sxs-lookup"><span data-stu-id="5c33a-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="5c33a-133">Ha az eszköz nem alkalmaz, a rendszer tájékoztatja egy kereső létrehozása és adatfolyamként küldje el az eszköz nélkül objektumtovábbítási szabályzat szövegként.</span><span class="sxs-lookup"><span data-stu-id="5c33a-133">If the Asset isn’t storage encrypted, the system will let you create a locator and stream the asset in the clear without an asset delivery policy.</span></span>
* <span data-ttu-id="5c33a-134">Több adategység továbbítási házirendjeit egyetlen eszköz társított is rendelkezik, de csak egyik módja egy adott AssetDeliveryProtocol kezelni lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="5c33a-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way to handle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="5c33a-135">Tehát ha próbál-e a csatolás két továbbítási házirendjeit, adja meg a AssetDeliveryProtocol.SmoothStreaming protokoll, amely hibát eredményez, mert a rendszer nem tudja, melyik úgy, hogy alkalmazza, ha egy ügyfél egy Smooth Streaming-kérelmet küld.</span><span class="sxs-lookup"><span data-stu-id="5c33a-135">Meaning if you try to link two delivery policies that specify the AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because the system does not know which one you want it to apply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="5c33a-136">Ha egy eszköz rendelkezik egy meglévő streamelési locator, egy új házirend az eszközre, ezért nem csatolható (megszünteti az eszköz a meglévő házirend, vagy frissítse a továbbítási szabályzatban az eszközhöz társított).</span><span class="sxs-lookup"><span data-stu-id="5c33a-136">If you have an asset with an existing streaming locator, you cannot link a new policy to the asset (you can either unlink an existing policy from the asset, or update a delivery policy associated with the asset).</span></span>  <span data-ttu-id="5c33a-137">Először azt kell távolítsa el a streamelési locator, állíthatja a házirendeket, és hozza létre a streamelési lokátort.</span><span class="sxs-lookup"><span data-stu-id="5c33a-137">You first have to remove the streaming locator, adjust the policies, and then re-create the streaming locator.</span></span>  <span data-ttu-id="5c33a-138">Az azonos locatorId segítségével használhatja, ha a streamelési locator hozza létre újból, de győződjön meg arról, hogy nem problémákat okozhat az ügyfelek tartalmat a forrás vagy egy alárendelt CDN gyorsítótárazható óta.</span><span class="sxs-lookup"><span data-stu-id="5c33a-138">You can use the same locatorId when you recreate the streaming locator but you should ensure that won’t cause issues for clients since content can be cached by the origin or a downstream CDN.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="5c33a-139">Objektumtovábbítási szabályzat törlése</span><span class="sxs-lookup"><span data-stu-id="5c33a-139">Clear asset delivery policy</span></span>

<span data-ttu-id="5c33a-140">A következő **ConfigureClearAssetDeliveryPolicy** módszer határozza meg, nem vonatkoznak a dinamikus titkosítás és a következő protokoll egyik adatfolyam továbbítására: MPEG DASH, HLS vagy Smooth Streaming protokollokat.</span><span class="sxs-lookup"><span data-stu-id="5c33a-140">The following **ConfigureClearAssetDeliveryPolicy** method specifies to not apply dynamic encryption and to deliver the stream in any of the following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> <span data-ttu-id="5c33a-141">A tárolás titkosítása ezt a házirendet alkalmazni kívánt.</span><span class="sxs-lookup"><span data-stu-id="5c33a-141">You might want to apply this policy to your storage encrypted assets.</span></span>

<span data-ttu-id="5c33a-142">Milyen értékeket is megadhat egy AssetDeliveryPolicy létrehozásakor, témakörben olvashat a [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5c33a-142">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="5c33a-143">Objektumtovábbítási szabályzat DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="5c33a-143">DynamicCommonEncryption asset delivery policy</span></span>

<span data-ttu-id="5c33a-144">A következő **CreateAssetDeliveryPolicy** hoz létre a **AssetDeliveryPolicy** konfigurált alkalmazni a dynamic common encryption (**DynamicCommonEncryption**) egy smooth adatfolyam-továbbítási protokoll (egyéb protokollok le lesz tiltva streaming).</span><span class="sxs-lookup"><span data-stu-id="5c33a-144">The following **CreateAssetDeliveryPolicy** method creates the **AssetDeliveryPolicy** that is configured to apply dynamic common encryption (**DynamicCommonEncryption**) to a smooth streaming protocol (other protocols will be blocked from streaming).</span></span> <span data-ttu-id="5c33a-145">A módszer két paramétereket fogadja: **eszköz** (az objektum továbbítási szabályzatát alkalmazni kívánt) és **IContentKey** (a tartalomkulcsot, a **CommonEncryption** típusa, a További információkért lásd: [tartalomkulcs létrehozása](media-services-dotnet-create-contentkey.md#common_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="5c33a-145">The method takes two parameters : **Asset** (the asset to which you want to apply the delivery policy) and **IContentKey** (the content key of the **CommonEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#common_contentkey)).</span></span>

<span data-ttu-id="5c33a-146">Milyen értékeket is megadhat egy AssetDeliveryPolicy létrehozásakor, témakörben olvashat a [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5c33a-146">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>

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
    
            // Add AssetDelivery Policy to the asset
            asset.DeliveryPolicies.Add(assetDeliveryPolicy);
    
            Console.WriteLine();
            Console.WriteLine("Adding Asset Delivery Policy: " +
                assetDeliveryPolicy.AssetDeliveryPolicyType);
     }

<span data-ttu-id="5c33a-147">Az Azure Media Services lehetővé teszi Widevine titkosítása.</span><span class="sxs-lookup"><span data-stu-id="5c33a-147">Azure Media Services also enables you to add Widevine encryption.</span></span> <span data-ttu-id="5c33a-148">A következő példa bemutatja, mind a PlayReady, mind a Widevine felvenni kívánt az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="5c33a-148">The following example demonstrates both PlayReady and Widevine being added to the asset delivery policy.</span></span>

    static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        // Get the PlayReady license service URL.
        Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);


        // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
        // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
        // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
        // to append /? KID =< keyId > to the end of the url when creating the manifest.
        // As a result Widevine license acquisition URL will have KID appended twice, 
        // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.

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


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

> [!NOTE]
> <span data-ttu-id="5c33a-149">Widevine titkosításakor csak lenne DASH használatával küldött.</span><span class="sxs-lookup"><span data-stu-id="5c33a-149">When encrypting with Widevine, you would only be able to deliver using DASH.</span></span> <span data-ttu-id="5c33a-150">Győződjön meg arról, ha meg szeretné adni az objektumtovábbítási protokoll kötőjel.</span><span class="sxs-lookup"><span data-stu-id="5c33a-150">Make sure to specify DASH in the asset delivery protocol.</span></span>
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="5c33a-151">Objektumtovábbítási szabályzat DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="5c33a-151">DynamicEnvelopeEncryption asset delivery policy</span></span>
<span data-ttu-id="5c33a-152">A következő **CreateAssetDeliveryPolicy** hoz létre a **AssetDeliveryPolicy** dinamikus boríték titkosítási alkalmazandó konfigurált (**DynamicEnvelopeEncryption**) a Smooth Streaming, HLS vagy kötőjel protokollok (Ha úgy dönt, hogy nem adja meg az egyes protokollok, le lesznek tiltva a folyamatos átviteli).</span><span class="sxs-lookup"><span data-stu-id="5c33a-152">The following **CreateAssetDeliveryPolicy** method creates the **AssetDeliveryPolicy** that is configured to apply dynamic envelope encryption (**DynamicEnvelopeEncryption**) to Smooth Streaming, HLS, and DASH protocols (if you decide to not specify some protocols, they will be blocked from streaming).</span></span> <span data-ttu-id="5c33a-153">A módszer két paramétert fogad: **eszköz** (az objektum továbbítási szabályzatát alkalmazni kívánt) és **IContentKey** (a tartalomkulcsot, a **EnvelopeEncryption** típusa További információkért lásd: [tartalomkulcs létrehozása](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span><span class="sxs-lookup"><span data-stu-id="5c33a-153">The method takes two parameters : **Asset** (the asset to which you want to apply the delivery policy) and **IContentKey** (the content key of the **EnvelopeEncryption** type, for more information, see: [Creating a content key](media-services-dotnet-create-contentkey.md#envelope_contentkey)).</span></span>

<span data-ttu-id="5c33a-154">Milyen értékeket is megadhat egy AssetDeliveryPolicy létrehozásakor, témakörben olvashat a [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5c33a-154">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {

        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
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

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


## <span data-ttu-id="5c33a-155"><a id="types"></a>Típusok AssetDeliveryPolicy definiálásakor használja</span><span class="sxs-lookup"><span data-stu-id="5c33a-155"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <span data-ttu-id="5c33a-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="5c33a-156"><a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol</span></span>

<span data-ttu-id="5c33a-157">A következő felsorolás ismerteti értékeket adhatja meg az objektumtovábbítási protokoll.</span><span class="sxs-lookup"><span data-stu-id="5c33a-157">The following enum describes values you can set for the asset delivery protocol.</span></span>

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

### <span data-ttu-id="5c33a-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="5c33a-158"><a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType</span></span>

<span data-ttu-id="5c33a-159">A következő felsorolás ismerteti, állíthatja be a kézbesítési házirend típusú értékeket.</span><span class="sxs-lookup"><span data-stu-id="5c33a-159">The following enum describes values you can set for the asset delivery policy type.</span></span>  

    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
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

### <span data-ttu-id="5c33a-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="5c33a-160"><a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType</span></span>

<span data-ttu-id="5c33a-161">A következő felsorolás ismerteti a segítségével konfigurálhatja a kézbesítési módszert az ügyfél a tartalom kulcs értékeket.</span><span class="sxs-lookup"><span data-stu-id="5c33a-161">The following enum describes values you can use to configure the delivery method of the content key to the client.</span></span>
    
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

### <span data-ttu-id="5c33a-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="5c33a-162"><a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="5c33a-163">A következő felsorolás ismerteti a kulcsok segítségével kéri le a meghatározott konfigurációját objektumtovábbítási szabályzat konfigurálása és értékeket.</span><span class="sxs-lookup"><span data-stu-id="5c33a-163">The following enum describes values you can set to configure keys used to get specific configuration for an asset delivery policy.</span></span>

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
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

## <a name="media-services-learning-paths"></a><span data-ttu-id="5c33a-164">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="5c33a-164">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="5c33a-165">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="5c33a-165">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


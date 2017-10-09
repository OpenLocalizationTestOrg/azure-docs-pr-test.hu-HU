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
# <a name="configure-asset-delivery-policies-with-net-sdk"></a>Konfigurálja az adategység továbbítási házirendjeit .NET SDK-val
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

## <a name="overview"></a>Áttekintés
Ha azt tervezi, hogy a titkosított toodelivery eszközök, hello lépések egyikét a hello Media Services content kézbesítési munkafolyamatot továbbítási házirendjeit eszközök konfigurálását. hogyan szeretné az eszköz toobe kézbesíteni a közli hello objektumtovábbítási szabályzat Media Services: az adatfolyam-továbbítási protokoll kell az eszköz dinamikusan csomagolható (például MPEG DASH, HLS, Smooth Streaming, vagy az összes), toodynamically szeretné-e az eszköz titkosítása és hogyan (boríték vagy közös titkosítási).

Ez a témakör ismerteti, miért és hogyan toocreate, és konfigurálja az adategység továbbítási házirendjeit.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 
>
>Emellett toobe képes toouse dinamikus csomagolás és a dinamikus titkosítás az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét.


Eltérő házirendek toohello alkalmazhat azonos eszköz. Például alkalmazhat PlayReady titkosítási tooSmooth Streaming és AES Envelope titkosítási tooMPEG DASH vagy HLS. Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming. hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán. Ezt követően hello törölje a jelet minden protokoll engedélyezett.

Ha azt szeretné, hogy a tároló titkosított eszköz toodeliver, konfigurálnia kell a hello adategység továbbítási házirendjét. Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét. Például toodeliver az eszköz titkosítva Advanced Encryption Standard (AES) boríték titkosítási kulcs, állítsa be a hello házirend típusa túl**DynamicEnvelopeEncryption**. tooremove tárolás titkosítása és az adatfolyam hello eszköz törlése, hello beállítása hello házirend típusa túl**NoDynamicEncryption**. Példák hogyan tooconfigure ezeket a házirend-típusainak kövesse.

Attól függően, hogyan hello objektumtovábbítási szabályzat konfigurálása, lenne képes toodynamically csomag kell, dinamikusan titkosítani és adatfolyamként küldje el a következő adatfolyam-továbbítási protokollok hello: Smooth Streaming, HLS vagy MPEG DASH adatfolyamokat.

hello alábbi lista mutatja azokat hello formátumok toostream Smooth, HLS és DASH használatát.

Smooth Streaming:

{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest

HLS:

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


## <a name="considerations"></a>Megfontolandó szempontok
* Egy AssetDeliveryPolicy társított egy eszköz, amíg az eszköz számára, hogy létezik egy (adatfolyam) OnDemand-kereső nem törölhető. hello ajánljuk tooremove hello házirend hello eszközből, hello házirend törlése előtt.
* A streamelési lokátorok létrehozásához egy tárolási titkosított eszköz nem hozható létre, ha nincs objektumtovábbítási szabályzat beállítása.  Ha hello eszköz nem alkalmaz, hello rendszer tájékoztatja lokátor és adatfolyam hello eszköz a hello törlése nélkül objektumtovábbítási szabályzat létrehozása.
* Egyetlen eszköz társított több adategység továbbítási házirendjeit is rendelkezik, de csak egyirányú toohandle egy adott AssetDeliveryProtocol adhat meg.  Tehát ha toolink két továbbítási házirendjeit által megadott hello AssetDeliveryProtocol.SmoothStreaming protokoll, amely a hibát okoz, mivel a hello rendszer nem tudja, melyik felel meg szeretné tooapply amikor egy ügyfél Smooth Streaming kérelmet.
* Ha egy eszköz rendelkezik egy meglévő streamelési locator, egy új házirend toohello eszköz (választható hello eszköz a meglévő házirend, vagy frissítse a továbbítási szabályzatban hello eszközhöz társított), ezért nem csatolható.  Ön először tooremove hello streamelési locator rendelkezik, állíthatja hello házirendeket és majd hozza újra létre a lokátor hello.  Hello azonos locatorId ismételt létrehozásakor streamelési locator, de hello győződjön meg arról, hogy problémákat nem okozhat az ügyfelek, mivel a tartalom gyorsítótárazható hello származási vagy egy alárendelt CDN által használható.

## <a name="clear-asset-delivery-policy"></a>Objektumtovábbítási szabályzat törlése

hello következő **ConfigureClearAssetDeliveryPolicy** módszer határozza meg, toonot alkalmazni a dinamikus titkosítás és toodeliver hello adatfolyam egyik sem szerepel a következő hello protokollok: MPEG DASH, HLS vagy Smooth Streaming protokollokat. Érdemes tooapply a házirend tooyour alkalmaz eszközök.

Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.

    static public void ConfigureClearAssetDeliveryPolicy(IAsset asset)
    {
        IAssetDeliveryPolicy policy =
        _context.AssetDeliveryPolicies.Create("Clear Policy",
        AssetDeliveryPolicyType.NoDynamicEncryption,
        AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
        
        asset.DeliveryPolicies.Add(policy);
    }

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>Objektumtovábbítási szabályzat DynamicCommonEncryption

hello következő **CreateAssetDeliveryPolicy** hoz létre hello **AssetDeliveryPolicy** , amely konfigurált tooapply a dynamic common encryption (**DynamicCommonEncryption**) a tooa smooth streaming (egyéb protokollok le lesz tiltva streaming) protokollt. hello metódus két paramétert fogad: **eszköz** (hello tooapply hello objektumtovábbítási szabályzat kívánt eszköz toowhich) és **IContentKey** (hello tartalomkulcsot a hello **CommonEncryption**típust, további információkért lásd: [tartalomkulcs létrehozása](media-services-dotnet-create-contentkey.md#common_contentkey)).

Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.

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

Az Azure Media Services lehetővé teszi tooadd Widevine titkosítása. hello alábbi példa azt mutatja be mind a PlayReady, mind a Widevine hozzáadni kívánt toohello objektumtovábbítási szabályzat.

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
> Widevine titkosításakor csak lenne képes toodeliver kötőjel használatával. Győződjön meg arról, hogy toospecify DASH a hello objektumtovábbítási protokoll.
> 
> 

## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Objektumtovábbítási szabályzat DynamicEnvelopeEncryption
hello következő **CreateAssetDeliveryPolicy** hoz létre hello **AssetDeliveryPolicy** , amely a konfigurált tooapply dinamikus boríték titkosítás (**DynamicEnvelopeEncryption** ) tooSmooth Streaming, HLS és kötőjel protokollt (Ha úgy dönt toonot néhány protokoll megadásához le lesznek tiltva a folyamatos átviteli). hello metódus két paramétert fogad: **eszköz** (hello tooapply hello objektumtovábbítási szabályzat kívánt eszköz toowhich) és **IContentKey** (hello tartalomkulcsot a hello **EnvelopeEncryption**típust, további információkért lásd: [tartalomkulcs létrehozása](media-services-dotnet-create-contentkey.md#envelope_contentkey)).

Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.   

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


## <a id="types"></a>Típusok AssetDeliveryPolicy definiálásakor használja

### <a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol

hello következő felsorolás ismerteti értékeket is hello objektumtovábbítási protokoll.

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

### <a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

hello következő felsorolás ismerteti, állíthatja be hello kézbesítési házirend típusú értékeket.  

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

### <a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

hello következő felsorolás ismerteti tooconfigure hello a kézbesítési módszer hello tartalom kulcs toohello ügyfél értékeket.
    
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

### <a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

a következő felsorolás hello értékeket is tooconfigure használt kulcsok tooget adott konfigurációja objektumtovábbítási szabályzat ismerteti.

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

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


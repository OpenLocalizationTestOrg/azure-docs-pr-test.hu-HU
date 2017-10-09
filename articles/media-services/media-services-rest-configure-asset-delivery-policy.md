---
title: "aaaConfiguring adategység továbbítási házirendjeit Media Services REST API használatával |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconfigure különböző adategység továbbítási házirendjeit Media Services REST API használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 5cb9d32a-e68b-4585-aa82-58dded0691d0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 8203230d570935e17382c598820dbfe42f83f8d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-asset-delivery-policies"></a><span data-ttu-id="f22d3-103">Adategység továbbítási házirendjeit konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f22d3-103">Configuring asset delivery policies</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

<span data-ttu-id="f22d3-104">Ha azt tervezi, hogy dinamikusan titkosított toodeliver eszközök, hello lépések egyikét a hello Media Services content kézbesítési munkafolyamatot továbbítási házirendjeit eszközök konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="f22d3-104">If you plan toodeliver dynamically encrypted assets, one of hello steps in hello Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="f22d3-105">hogyan szeretné az eszköz toobe kézbesíteni a közli hello objektumtovábbítási szabályzat Media Services: az adatfolyam-továbbítási protokoll kell az eszköz dinamikusan csomagolható (például MPEG DASH, HLS, Smooth Streaming, vagy az összes), toodynamically szeretné-e az eszköz titkosítása és hogyan (boríték vagy közös titkosítási).</span><span class="sxs-lookup"><span data-stu-id="f22d3-105">hello asset delivery policy tells Media Services how you want for your asset toobe delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want toodynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="f22d3-106">Ez a témakör ismerteti, miért és hogyan toocreate, és konfigurálja az adategység továbbítási házirendjeit.</span><span class="sxs-lookup"><span data-stu-id="f22d3-106">This topic discusses why and how toocreate and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="f22d3-107">Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát.</span><span class="sxs-lookup"><span data-stu-id="f22d3-107">When your AMS account is created a **default** streaming endpoint is added tooyour account in hello **Stopped** state.</span></span> <span data-ttu-id="f22d3-108">a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.</span><span class="sxs-lookup"><span data-stu-id="f22d3-108">toostart streaming your content and take advantage of dynamic packaging and dynamic encryption, hello streaming endpoint from which you want toostream content has toobe in hello **Running** state.</span></span> 
>
><span data-ttu-id="f22d3-109">Emellett toobe képes toouse dinamikus csomagolás és a dinamikus titkosítás az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét.</span><span class="sxs-lookup"><span data-stu-id="f22d3-109">Also, toobe able toouse dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="f22d3-110">Eltérő házirendek toohello alkalmazhat azonos eszköz.</span><span class="sxs-lookup"><span data-stu-id="f22d3-110">You could apply different policies toohello same asset.</span></span> <span data-ttu-id="f22d3-111">Például alkalmazhat PlayReady titkosítási tooSmooth Streaming és AES Envelope titkosítási tooMPEG DASH vagy HLS.</span><span class="sxs-lookup"><span data-stu-id="f22d3-111">For example, you could apply PlayReady encryption tooSmooth Streaming and AES Envelope encryption tooMPEG DASH and HLS.</span></span> <span data-ttu-id="f22d3-112">Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming.</span><span class="sxs-lookup"><span data-stu-id="f22d3-112">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as hello protocol) will be blocked from streaming.</span></span> <span data-ttu-id="f22d3-113">hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán.</span><span class="sxs-lookup"><span data-stu-id="f22d3-113">hello exception toothis is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="f22d3-114">Ezt követően hello törölje a jelet minden protokoll engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="f22d3-114">Then, all protocols will be allowed in hello clear.</span></span>

<span data-ttu-id="f22d3-115">Ha azt szeretné, hogy a tároló titkosított eszköz toodeliver, konfigurálnia kell a hello adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="f22d3-115">If you want toodeliver a storage encrypted asset, you must configure hello asset’s delivery policy.</span></span> <span data-ttu-id="f22d3-116">Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="f22d3-116">Before your asset can be streamed, hello streaming server removes hello storage encryption and streams your content using hello specified delivery policy.</span></span> <span data-ttu-id="f22d3-117">Például toodeliver az eszköz titkosítva Advanced Encryption Standard (AES) boríték titkosítási kulcs, állítsa be a hello házirend típusa túl**DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="f22d3-117">For example, toodeliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set hello policy type too**DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="f22d3-118">tooremove tárolás titkosítása és az adatfolyam hello eszköz törlése, hello beállítása hello házirend típusa túl**NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="f22d3-118">tooremove storage encryption and stream hello asset in hello clear, set hello policy type too**NoDynamicEncryption**.</span></span> <span data-ttu-id="f22d3-119">Példák hogyan tooconfigure ezeket a házirend-típusainak kövesse.</span><span class="sxs-lookup"><span data-stu-id="f22d3-119">Examples that show how tooconfigure these policy types follow.</span></span>

<span data-ttu-id="f22d3-120">Attól függően, hogyan hello objektumtovábbítási szabályzat konfigurálása, lenne képes toodynamically csomag kell, dinamikusan titkosítani és adatfolyamként küldje el a következő adatfolyam-továbbítási protokollok hello: Smooth Streaming, HLS, MPEG DASH-streameket.</span><span class="sxs-lookup"><span data-stu-id="f22d3-120">Depending on how you configure hello asset delivery policy you would be able toodynamically package, dynamically encrypt, and stream hello following streaming protocols: Smooth Streaming, HLS, MPEG DASH streams.</span></span>

<span data-ttu-id="f22d3-121">a következő listán mutatja hello hello formázza toostream Smooth, HLS, DASH használatát.</span><span class="sxs-lookup"><span data-stu-id="f22d3-121">hello following list shows hello formats that you use toostream Smooth, HLS, DASH.</span></span>

<span data-ttu-id="f22d3-122">Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="f22d3-122">Smooth Streaming:</span></span>

<span data-ttu-id="f22d3-123">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="f22d3-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="f22d3-124">HLS:</span><span class="sxs-lookup"><span data-stu-id="f22d3-124">HLS:</span></span>

<span data-ttu-id="f22d3-125">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="f22d3-125">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="f22d3-126">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="f22d3-126">MPEG DASH</span></span>

<span data-ttu-id="f22d3-127">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="f22d3-127">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


<span data-ttu-id="f22d3-128">Hogyan toopublish egy eszköz és -buildek a streamelési URL-cím: kapcsolatos utasításokat [streamelési URL-cím összeállítása](media-services-deliver-streaming-content.md).</span><span class="sxs-lookup"><span data-stu-id="f22d3-128">For instructions on how toopublish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="f22d3-129">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="f22d3-129">Considerations</span></span>
* <span data-ttu-id="f22d3-130">Egy AssetDeliveryPolicy társított egy eszköz, amíg az eszköz számára, hogy létezik egy (adatfolyam) OnDemand-kereső nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="f22d3-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="f22d3-131">hello ajánljuk tooremove hello házirend hello eszközből, hello házirend törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="f22d3-131">hello recommendation is tooremove hello policy from hello asset before deleting hello policy.</span></span>
* <span data-ttu-id="f22d3-132">A streamelési lokátorok létrehozásához egy tárolási titkosított eszköz nem hozható létre, ha nincs objektumtovábbítási szabályzat beállítása.</span><span class="sxs-lookup"><span data-stu-id="f22d3-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="f22d3-133">Ha hello eszköz nem alkalmaz, hello rendszer tájékoztatja lokátor és adatfolyam hello eszköz a hello törlése nélkül objektumtovábbítási szabályzat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f22d3-133">If hello Asset isn’t storage encrypted, hello system will let you create a locator and stream hello asset in hello clear without an asset delivery policy.</span></span>
* <span data-ttu-id="f22d3-134">Egyetlen eszköz társított több adategység továbbítási házirendjeit is rendelkezik, de csak egyirányú toohandle egy adott AssetDeliveryProtocol adhat meg.</span><span class="sxs-lookup"><span data-stu-id="f22d3-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way toohandle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="f22d3-135">Tehát ha toolink két továbbítási házirendjeit által megadott hello AssetDeliveryProtocol.SmoothStreaming protokoll, amely a hibát okoz, mivel a hello rendszer nem tudja, melyik felel meg szeretné tooapply amikor egy ügyfél Smooth Streaming kérelmet.</span><span class="sxs-lookup"><span data-stu-id="f22d3-135">Meaning if you try toolink two delivery policies that specify hello AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because hello system does not know which one you want it tooapply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="f22d3-136">Ha egy eszköz rendelkezik egy meglévő streamelési locator, nem egy új házirend toohello eszköz hivatkozásra, leválasztását hello eszköz a meglévő házirend, és nem frissíthetők a továbbítási szabályzatban hello eszközhöz társított.</span><span class="sxs-lookup"><span data-stu-id="f22d3-136">If you have an asset with an existing streaming locator, you cannot link a new policy toohello asset, unlink an existing policy from hello asset, or update a delivery policy associated with hello asset.</span></span>  <span data-ttu-id="f22d3-137">Ön először tooremove hello streamelési locator rendelkezik, állíthatja hello házirendeket és majd hozza újra létre a lokátor hello.</span><span class="sxs-lookup"><span data-stu-id="f22d3-137">You first have tooremove hello streaming locator, adjust hello policies, and then re-create hello streaming locator.</span></span>  <span data-ttu-id="f22d3-138">Hello azonos locatorId ismételt létrehozásakor streamelési locator, de hello győződjön meg arról, hogy problémákat nem okozhat az ügyfelek, mivel a tartalom gyorsítótárazható hello származási vagy egy alárendelt CDN által használható.</span><span class="sxs-lookup"><span data-stu-id="f22d3-138">You can use hello same locatorId when you recreate hello streaming locator but you should ensure that won’t cause issues for clients since content can be cached by hello origin or a downstream CDN.</span></span>

>[!NOTE]

><span data-ttu-id="f22d3-139">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="f22d3-139">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="f22d3-140">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="f22d3-140">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="f22d3-141">Connect tooMedia szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="f22d3-141">Connect tooMedia Services</span></span>

<span data-ttu-id="f22d3-142">Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="f22d3-142">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="f22d3-143">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="f22d3-143">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="f22d3-144">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="f22d3-144">You must make subsequent calls toohello new URI.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="f22d3-145">Objektumtovábbítási szabályzat törlése</span><span class="sxs-lookup"><span data-stu-id="f22d3-145">Clear asset delivery policy</span></span>
### <span data-ttu-id="f22d3-146"><a id="create_asset_delivery_policy"></a>Objektumtovábbítási szabályzat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f22d3-146"><a id="create_asset_delivery_policy"></a>Create asset delivery policy</span></span>
<span data-ttu-id="f22d3-147">hello következő HTTP-kérelem hoz létre, amelyben toonot alkalmazni a dinamikus titkosítás és toodeliver hello adatfolyam egyik sem szerepel a következő hello protokollok objektumtovábbítási szabályzat: MPEG DASH, HLS vagy Smooth Streaming protokollokat.</span><span class="sxs-lookup"><span data-stu-id="f22d3-147">hello following HTTP request creates an asset delivery policy that specifies toonot apply dynamic encryption and toodeliver hello stream in any of hello following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> 

<span data-ttu-id="f22d3-148">Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f22d3-148">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="f22d3-149">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="f22d3-149">Request:</span></span>

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net

    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

<span data-ttu-id="f22d3-150">Válasz:</span><span class="sxs-lookup"><span data-stu-id="f22d3-150">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT

    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}

### <span data-ttu-id="f22d3-151"><a id="link_asset_with_asset_delivery_policy"></a>Kapcsolat eszköz az adategység továbbítási házirendjét</span><span class="sxs-lookup"><span data-stu-id="f22d3-151"><a id="link_asset_with_asset_delivery_policy"></a>Link asset with asset delivery policy</span></span>
<span data-ttu-id="f22d3-152">a következő HTTP-kérelem hivatkozások hello hello eszköz toohello objektumtovábbítási szabályzat számára megadott.</span><span class="sxs-lookup"><span data-stu-id="f22d3-152">hello following HTTP request links hello specified asset toohello asset delivery policy to.</span></span>

<span data-ttu-id="f22d3-153">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="f22d3-153">Request:</span></span>

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net

    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

<span data-ttu-id="f22d3-154">Válasz:</span><span class="sxs-lookup"><span data-stu-id="f22d3-154">Response:</span></span>

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="f22d3-155">Objektumtovábbítási szabályzat DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="f22d3-155">DynamicEnvelopeEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-hello-envelopeencryption-type-and-link-it-toohello-asset"></a><span data-ttu-id="f22d3-156">Hello EnvelopeEncryption típusú tartalomkulcs létrehozása és csatolása toohello eszköz</span><span class="sxs-lookup"><span data-stu-id="f22d3-156">Create content key of hello EnvelopeEncryption type and link it toohello asset</span></span>
<span data-ttu-id="f22d3-157">DynamicEnvelopeEncryption objektumtovábbítási szabályzat megadása esetén kell toomake meg arról, hogy toolink az eszköz tooa tartalom hello EnvelopeEncryption típus kulcsát.</span><span class="sxs-lookup"><span data-stu-id="f22d3-157">When specifying DynamicEnvelopeEncryption delivery policy, you need toomake sure toolink your asset tooa content key of hello EnvelopeEncryption type.</span></span> <span data-ttu-id="f22d3-158">További információkért lásd: [tartalomkulcs létrehozása](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="f22d3-158">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <span data-ttu-id="f22d3-159"><a id="get_delivery_url"></a>Kézbesítési URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="f22d3-159"><a id="get_delivery_url"></a>Get delivery URL</span></span>
<span data-ttu-id="f22d3-160">Get hello kézbesítési URL-címe hello megadott hello előző lépésben létrehozott hello tartalomkulcsot a kézbesítési módszert.</span><span class="sxs-lookup"><span data-stu-id="f22d3-160">Get hello delivery URL for hello specified delivery method of hello content key created in hello previous step.</span></span> <span data-ttu-id="f22d3-161">Egy ügyfél URL-cím toorequest visszaadott hello használ az AES-kulcs vagy egy PlayReady licenc rendelés tooplayback hello védett tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="f22d3-161">A client uses hello returned URL toorequest an AES key or a PlayReady license in order tooplayback hello protected content.</span></span>

<span data-ttu-id="f22d3-162">Adja meg hello URL-cím tooget hello típusú hello hello HTTP-kérelem törzsét.</span><span class="sxs-lookup"><span data-stu-id="f22d3-162">Specify hello type of hello URL tooget in hello body of hello HTTP request.</span></span> <span data-ttu-id="f22d3-163">Véd a tartalmaknak a PlayReady, a Media Services PlayReady licenc licenckérési URL-cím kérése 1 használ hello keyDeliveryType: {"keyDeliveryType": 1}.</span><span class="sxs-lookup"><span data-stu-id="f22d3-163">If you are protecting your content with PlayReady, request a Media Services PlayReady license acquisition URL, using 1 for hello keyDeliveryType: {"keyDeliveryType":1}.</span></span> <span data-ttu-id="f22d3-164">Véd-e a tartalom hello boríték titkosított, egy kulcs-licenckérési URL-cím kérése az keyDeliveryType 2 megadásával: {"keyDeliveryType": 2}.</span><span class="sxs-lookup"><span data-stu-id="f22d3-164">If you are protecting your content with hello envelope encryption, request a key acquisition URL by specifying 2 for keyDeliveryType: {"keyDeliveryType":2}.</span></span>

<span data-ttu-id="f22d3-165">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="f22d3-165">Request:</span></span>

    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21

    {"keyDeliveryType":2}

<span data-ttu-id="f22d3-166">Válasz:</span><span class="sxs-lookup"><span data-stu-id="f22d3-166">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


### <a name="create-asset-delivery-policy"></a><span data-ttu-id="f22d3-167">Objektumtovábbítási szabályzat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f22d3-167">Create asset delivery policy</span></span>
<span data-ttu-id="f22d3-168">hello következő HTTP-kérelem létrehozása hello **AssetDeliveryPolicy** , amely a konfigurált tooapply dinamikus boríték titkosítás (**DynamicEnvelopeEncryption**) toohello **HLS**protokoll (ebben a példában egyéb protokollok le lesz tiltva streaming).</span><span class="sxs-lookup"><span data-stu-id="f22d3-168">hello following HTTP request creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic envelope encryption (**DynamicEnvelopeEncryption**) toohello **HLS** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="f22d3-169">Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f22d3-169">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="f22d3-170">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="f22d3-170">Request:</span></span>

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}


<span data-ttu-id="f22d3-171">Válasz:</span><span class="sxs-lookup"><span data-stu-id="f22d3-171">Response:</span></span>

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT

    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="f22d3-172">Kapcsolat eszköz az adategység továbbítási házirendjét</span><span class="sxs-lookup"><span data-stu-id="f22d3-172">Link asset with asset delivery policy</span></span>
<span data-ttu-id="f22d3-173">Lásd: [hivatkozás eszköz az adategység továbbítási házirendjét](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="f22d3-173">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="f22d3-174">Objektumtovábbítási szabályzat DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="f22d3-174">DynamicCommonEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-hello-commonencryption-type-and-link-it-toohello-asset"></a><span data-ttu-id="f22d3-175">Hello CommonEncryption típusú tartalomkulcs létrehozása és csatolása toohello eszköz</span><span class="sxs-lookup"><span data-stu-id="f22d3-175">Create content key of hello CommonEncryption type and link it toohello asset</span></span>
<span data-ttu-id="f22d3-176">DynamicCommonEncryption objektumtovábbítási szabályzat megadása esetén kell toomake meg arról, hogy toolink az eszköz tooa tartalom hello CommonEncryption típus kulcsát.</span><span class="sxs-lookup"><span data-stu-id="f22d3-176">When specifying DynamicCommonEncryption delivery policy, you need toomake sure toolink your asset tooa content key of hello CommonEncryption type.</span></span> <span data-ttu-id="f22d3-177">További információkért lásd: [tartalomkulcs létrehozása](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="f22d3-177">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <a name="get-delivery-url"></a><span data-ttu-id="f22d3-178">Kézbesítési URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="f22d3-178">Get Delivery URL</span></span>
<span data-ttu-id="f22d3-179">Hello kézbesítési URL-cím beszerzése a hello PlayReady kézbesítési módszert a hello előző lépésben létrehozott hello tartalomkulcsot.</span><span class="sxs-lookup"><span data-stu-id="f22d3-179">Get hello delivery URL for hello PlayReady delivery method of hello content key created in hello previous step.</span></span> <span data-ttu-id="f22d3-180">Egy ügyfél URL-cím toorequest rendelés tooplayback hello a PlayReady licenc védett tartalom visszaadott hello használ.</span><span class="sxs-lookup"><span data-stu-id="f22d3-180">A client uses hello returned URL toorequest a PlayReady license in order tooplayback hello protected content.</span></span> <span data-ttu-id="f22d3-181">További információkért lásd: [kézbesítési URL-cím beszerzése](#get_delivery_url).</span><span class="sxs-lookup"><span data-stu-id="f22d3-181">For more information, see [Get Delivery URL](#get_delivery_url).</span></span>

### <a name="create-asset-delivery-policy"></a><span data-ttu-id="f22d3-182">Objektumtovábbítási szabályzat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f22d3-182">Create asset delivery policy</span></span>
<span data-ttu-id="f22d3-183">hello következő HTTP-kérelem létrehozása hello **AssetDeliveryPolicy** , amely konfigurált tooapply a dynamic common encryption (**DynamicCommonEncryption**) toohello **Smooth Streaming**  protokoll (ebben a példában egyéb protokollok le lesz tiltva streaming).</span><span class="sxs-lookup"><span data-stu-id="f22d3-183">hello following HTTP request creates hello **AssetDeliveryPolicy** that is configured tooapply dynamic common encryption (**DynamicCommonEncryption**) toohello **Smooth Streaming** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="f22d3-184">Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f22d3-184">For information on what values you can specify when creating an AssetDeliveryPolicy, see hello [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="f22d3-185">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="f22d3-185">Request:</span></span>

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


<span data-ttu-id="f22d3-186">Ha tooprotect a tartalom Widevine DRM segítségével, hello AssetDeliveryConfiguration értékek toouse WidevineLicenseAcquisitionUrl (amely hello értéke a 7) frissítése, és adjon meg egy licenctovábbítási szolgáltatása hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f22d3-186">If you want tooprotect your content using Widevine DRM, update hello AssetDeliveryConfiguration values toouse WidevineLicenseAcquisitionUrl (which has hello value of 7) and specify hello URL of a license delivery service.</span></span> <span data-ttu-id="f22d3-187">Használhatja a következő AMS-partnereket toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="f22d3-187">You can use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="f22d3-188">Példa:</span><span class="sxs-lookup"><span data-stu-id="f22d3-188">For example:</span></span> 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> <span data-ttu-id="f22d3-189">Widevine titkosításakor csak lenne képes toodeliver kötőjel használatával.</span><span class="sxs-lookup"><span data-stu-id="f22d3-189">When encrypting with Widevine, you would only be able toodeliver using DASH.</span></span> <span data-ttu-id="f22d3-190">Győződjön meg arról, hogy toospecify kötőjel (2) a hello objektumtovábbítási protokoll.</span><span class="sxs-lookup"><span data-stu-id="f22d3-190">Make sure toospecify DASH (2) in hello asset delivery protocol.</span></span>
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="f22d3-191">Kapcsolat eszköz az adategység továbbítási házirendjét</span><span class="sxs-lookup"><span data-stu-id="f22d3-191">Link asset with asset delivery policy</span></span>
<span data-ttu-id="f22d3-192">Lásd: [hivatkozás eszköz az adategység továbbítási házirendjét](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="f22d3-192">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <span data-ttu-id="f22d3-193"><a id="types"></a>Típusok AssetDeliveryPolicy definiálásakor használja</span><span class="sxs-lookup"><span data-stu-id="f22d3-193"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <a name="assetdeliveryprotocol"></a><span data-ttu-id="f22d3-194">AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="f22d3-194">AssetDeliveryProtocol</span></span>

<span data-ttu-id="f22d3-195">hello következő felsorolás ismerteti értékeket is hello objektumtovábbítási protokoll.</span><span class="sxs-lookup"><span data-stu-id="f22d3-195">hello following enum describes values you can set for hello asset delivery protocol.</span></span>

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

### <a name="assetdeliverypolicytype"></a><span data-ttu-id="f22d3-196">AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="f22d3-196">AssetDeliveryPolicyType</span></span>

<span data-ttu-id="f22d3-197">hello következő felsorolás ismerteti, állíthatja be hello kézbesítési házirend típusú értékeket.</span><span class="sxs-lookup"><span data-stu-id="f22d3-197">hello following enum describes values you can set for hello asset delivery policy type.</span></span>  

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

### <a name="contentkeydeliverytype"></a><span data-ttu-id="f22d3-198">ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="f22d3-198">ContentKeyDeliveryType</span></span>

<span data-ttu-id="f22d3-199">hello következő felsorolás ismerteti tooconfigure hello a kézbesítési módszer hello tartalom kulcs toohello ügyfél értékeket.</span><span class="sxs-lookup"><span data-stu-id="f22d3-199">hello following enum describes values you can use tooconfigure hello delivery method of hello content key toohello client.</span></span>
    
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


### <a name="assetdeliverypolicyconfigurationkey"></a><span data-ttu-id="f22d3-200">AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="f22d3-200">AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="f22d3-201">a következő felsorolás hello értékeket is tooconfigure használt kulcsok tooget adott konfigurációja objektumtovábbítási szabályzat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f22d3-201">hello following enum describes values you can set tooconfigure keys used tooget specific configuration for an asset delivery policy.</span></span>

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="f22d3-202">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="f22d3-202">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="f22d3-203">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="f22d3-203">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


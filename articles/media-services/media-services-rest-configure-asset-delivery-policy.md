---
title: "Konfigurálása az adategység továbbítási házirendjeit Media Services REST API használatával |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan konfigurálhatja a különböző adategység továbbítási házirendjeit Media Services REST API használatával."
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
ms.openlocfilehash: 7ffbde11b943961dd3a3b5edebd0cfd52429e845
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-asset-delivery-policies"></a><span data-ttu-id="bec62-103">Adategység továbbítási házirendjeit konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bec62-103">Configuring asset delivery policies</span></span>
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

<span data-ttu-id="bec62-104">Ha azt tervezi, hogy dinamikusan titkosított eszközökre, a Media Services-továbbítási munkafolyamat lépésben továbbítási házirendjeit eszközök konfigurálását végzi.</span><span class="sxs-lookup"><span data-stu-id="bec62-104">If you plan to deliver dynamically encrypted assets, one of the steps in the Media Services content delivery workflow is configuring delivery policies for assets.</span></span> <span data-ttu-id="bec62-105">Hogyan szeretné az eszköz kézbesítendő közli az adategység továbbítási házirendjét Media Services: be kell az eszköz dinamikusan csomagolható (például MPEG DASH, HLS, Smooth Streaming, vagy az összes), az eszköz dinamikusan titkosítani szeretné-e, és hogy melyik adatfolyam-protokoll (boríték vagy közös titkosítási).</span><span class="sxs-lookup"><span data-stu-id="bec62-105">The asset delivery policy tells Media Services how you want for your asset to be delivered: into which streaming protocol should your asset be dynamically packaged (for example, MPEG DASH, HLS, Smooth Streaming, or all), whether or not you want to dynamically encrypt your asset and how (envelope or common encryption).</span></span>

<span data-ttu-id="bec62-106">Ez a témakör ismerteti, miért és hogyan hozza létre és konfigurálja az adategység továbbítási házirendjeit.</span><span class="sxs-lookup"><span data-stu-id="bec62-106">This topic discusses why and how to create and configure asset delivery policies.</span></span>

>[!NOTE]
><span data-ttu-id="bec62-107">Az AMS-fiók létrehozásakor a rendszer hozzáad egy **alapértelmezett** streamvégpontot a fiókhoz **Leállítva** állapotban.</span><span class="sxs-lookup"><span data-stu-id="bec62-107">When your AMS account is created a **default** streaming endpoint is added to your account in the **Stopped** state.</span></span> <span data-ttu-id="bec62-108">A tartalom streamelésének megkezdéséhez, valamint a dinamikus csomagolás és a dinamikus titkosítás kihasználásához a tartalomstreameléshez használt streamvégpontnak **Fut** állapotban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bec62-108">To start streaming your content and take advantage of dynamic packaging and dynamic encryption, the streaming endpoint from which you want to stream content has to be in the **Running** state.</span></span> 
>
><span data-ttu-id="bec62-109">Is hogy fogja tudni használni a dinamikus csomagolás és a dinamikus titkosítás az objektumot kell foglal magában adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá.</span><span class="sxs-lookup"><span data-stu-id="bec62-109">Also, to be able to use dynamic packaging and dynamic encryption your asset must contain a set of adaptive bitrate MP4s or adaptive bitrate Smooth Streaming files.</span></span>

<span data-ttu-id="bec62-110">Eltérő házirendek a azonos eszközhöz alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="bec62-110">You could apply different policies to the same asset.</span></span> <span data-ttu-id="bec62-111">Például PlayReady-titkosítás beállíthat MPEG DASH vagy HLS, Smooth Streaming és AES Envelope titkosítás.</span><span class="sxs-lookup"><span data-stu-id="bec62-111">For example, you could apply PlayReady encryption to Smooth Streaming and AES Envelope encryption to MPEG DASH and HLS.</span></span> <span data-ttu-id="bec62-112">A továbbítási szabályzatban meg nem határozott protokollok streameléshez való használatát a rendszer nem engedélyezi (ilyen lehet például, ha csupán egyetlen szabályzatot állít be, amely kizárólag a HLS-protokoll használatát tartalmazza).</span><span class="sxs-lookup"><span data-stu-id="bec62-112">Any protocols that are not defined in a delivery policy (for example, you add a single policy that only specifies HLS as the protocol) will be blocked from streaming.</span></span> <span data-ttu-id="bec62-113">Kivételt jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot.</span><span class="sxs-lookup"><span data-stu-id="bec62-113">The exception to this is if you have no asset delivery policy defined at all.</span></span> <span data-ttu-id="bec62-114">Ebben az esetben a rendszer az összes protokollt engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="bec62-114">Then, all protocols will be allowed in the clear.</span></span>

<span data-ttu-id="bec62-115">Ha egy tárolási titkosított eszköz kézbesíteni szeretné, konfigurálnia kell az adategység továbbítási házirendjét.</span><span class="sxs-lookup"><span data-stu-id="bec62-115">If you want to deliver a storage encrypted asset, you must configure the asset’s delivery policy.</span></span> <span data-ttu-id="bec62-116">Mielőtt az eszköz továbbítható, a streamelési kiszolgáló a tárolás titkosítása eltávolítja, és az adatfolyamokat, a tartalom a megadott objektumtovábbítási szabályzat segítségével.</span><span class="sxs-lookup"><span data-stu-id="bec62-116">Before your asset can be streamed, the streaming server removes the storage encryption and streams your content using the specified delivery policy.</span></span> <span data-ttu-id="bec62-117">Például az Advanced Encryption Standard (AES) boríték titkosítási kulccsal titkosított objektumot, hogy állítsa a házirend típusát **DynamicEnvelopeEncryption**.</span><span class="sxs-lookup"><span data-stu-id="bec62-117">For example, to deliver your asset encrypted with Advanced Encryption Standard (AES) envelope encryption key, set the policy type to **DynamicEnvelopeEncryption**.</span></span> <span data-ttu-id="bec62-118">Tárolás titkosítása és adatfolyamként küldje el az eszköz szövegként, állítsa be a házirend típusát **NoDynamicEncryption**.</span><span class="sxs-lookup"><span data-stu-id="bec62-118">To remove storage encryption and stream the asset in the clear, set the policy type to **NoDynamicEncryption**.</span></span> <span data-ttu-id="bec62-119">Az alábbi példák, amelyek bemutatják, hogyan konfigurálhatja ezeket a házirend-típusainak.</span><span class="sxs-lookup"><span data-stu-id="bec62-119">Examples that show how to configure these policy types follow.</span></span>

<span data-ttu-id="bec62-120">Attól függően, hogyan konfigurálja az adategység továbbítási házirendjét lehetővé válik a dinamikus csomag dinamikusan titkosítani és adatfolyamként küldje el a következő adatfolyam-továbbítási protokollok: Smooth Streaming, HLS, MPEG DASH-streameket.</span><span class="sxs-lookup"><span data-stu-id="bec62-120">Depending on how you configure the asset delivery policy you would be able to dynamically package, dynamically encrypt, and stream the following streaming protocols: Smooth Streaming, HLS, MPEG DASH streams.</span></span>

<span data-ttu-id="bec62-121">Az alábbi listában láthatók a formátumok adatfolyam Smooth, HLS, DASH segítségével.</span><span class="sxs-lookup"><span data-stu-id="bec62-121">The following list shows the formats that you use to stream Smooth, HLS, DASH.</span></span>

<span data-ttu-id="bec62-122">Smooth Streaming:</span><span class="sxs-lookup"><span data-stu-id="bec62-122">Smooth Streaming:</span></span>

<span data-ttu-id="bec62-123">{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="bec62-123">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest</span></span>

<span data-ttu-id="bec62-124">HLS:</span><span class="sxs-lookup"><span data-stu-id="bec62-124">HLS:</span></span>

<span data-ttu-id="bec62-125">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span><span class="sxs-lookup"><span data-stu-id="bec62-125">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)</span></span>

<span data-ttu-id="bec62-126">MPEG DASH</span><span class="sxs-lookup"><span data-stu-id="bec62-126">MPEG DASH</span></span>

<span data-ttu-id="bec62-127">{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span><span class="sxs-lookup"><span data-stu-id="bec62-127">{streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)</span></span>


<span data-ttu-id="bec62-128">További információk az objektumok közzétételéről és a streamelési URL-cím létrehozásáról: [Build a streaming URL](media-services-deliver-streaming-content.md) (Streamelési URL-cím létrehozása).</span><span class="sxs-lookup"><span data-stu-id="bec62-128">For instructions on how to publish an asset and build a streaming URL, see [Build a streaming URL](media-services-deliver-streaming-content.md).</span></span>

## <a name="considerations"></a><span data-ttu-id="bec62-129">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="bec62-129">Considerations</span></span>
* <span data-ttu-id="bec62-130">Egy AssetDeliveryPolicy társított egy eszköz, amíg az eszköz számára, hogy létezik egy (adatfolyam) OnDemand-kereső nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="bec62-130">You cannot delete an AssetDeliveryPolicy associated with an asset while an OnDemand (streaming) locator exists for that asset.</span></span> <span data-ttu-id="bec62-131">Az ajánljuk, hogy a házirend törlése előtt távolítsa el a házirend az eszköz.</span><span class="sxs-lookup"><span data-stu-id="bec62-131">The recommendation is to remove the policy from the asset before deleting the policy.</span></span>
* <span data-ttu-id="bec62-132">A streamelési lokátorok létrehozásához egy tárolási titkosított eszköz nem hozható létre, ha nincs objektumtovábbítási szabályzat beállítása.</span><span class="sxs-lookup"><span data-stu-id="bec62-132">A streaming locator cannot be created on a storage encrypted asset when no asset delivery policy is set.</span></span>  <span data-ttu-id="bec62-133">Ha az eszköz nem alkalmaz, a rendszer tájékoztatja egy kereső létrehozása és adatfolyamként küldje el az eszköz nélkül objektumtovábbítási szabályzat szövegként.</span><span class="sxs-lookup"><span data-stu-id="bec62-133">If the Asset isn’t storage encrypted, the system will let you create a locator and stream the asset in the clear without an asset delivery policy.</span></span>
* <span data-ttu-id="bec62-134">Több adategység továbbítási házirendjeit egyetlen eszköz társított is rendelkezik, de csak egyik módja egy adott AssetDeliveryProtocol kezelni lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="bec62-134">You can have multiple asset delivery policies associated with a single asset but you can only specify one way to handle a given AssetDeliveryProtocol.</span></span>  <span data-ttu-id="bec62-135">Tehát ha próbál-e a csatolás két továbbítási házirendjeit, adja meg a AssetDeliveryProtocol.SmoothStreaming protokoll, amely hibát eredményez, mert a rendszer nem tudja, melyik úgy, hogy alkalmazza, ha egy ügyfél egy Smooth Streaming-kérelmet küld.</span><span class="sxs-lookup"><span data-stu-id="bec62-135">Meaning if you try to link two delivery policies that specify the AssetDeliveryProtocol.SmoothStreaming protocol that will result in an error because the system does not know which one you want it to apply when a client makes a Smooth Streaming request.</span></span>
* <span data-ttu-id="bec62-136">Ha egy eszköz rendelkezik egy meglévő streamelési locator, nem egy új házirendet csatolása az eszközhöz, megszünteti az eszköz a meglévő házirend, és nem frissíthetők a továbbítási szabályzatban az eszközhöz társított.</span><span class="sxs-lookup"><span data-stu-id="bec62-136">If you have an asset with an existing streaming locator, you cannot link a new policy to the asset, unlink an existing policy from the asset, or update a delivery policy associated with the asset.</span></span>  <span data-ttu-id="bec62-137">Először azt kell távolítsa el a streamelési locator, állíthatja a házirendeket, és hozza létre a streamelési lokátort.</span><span class="sxs-lookup"><span data-stu-id="bec62-137">You first have to remove the streaming locator, adjust the policies, and then re-create the streaming locator.</span></span>  <span data-ttu-id="bec62-138">Az azonos locatorId segítségével használhatja, ha a streamelési locator hozza létre újból, de győződjön meg arról, hogy nem problémákat okozhat az ügyfelek tartalmat a forrás vagy egy alárendelt CDN gyorsítótárazható óta.</span><span class="sxs-lookup"><span data-stu-id="bec62-138">You can use the same locatorId when you recreate the streaming locator but you should ensure that won’t cause issues for clients since content can be cached by the origin or a downstream CDN.</span></span>

>[!NOTE]

><span data-ttu-id="bec62-139">A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="bec62-139">When accessing entities in Media Services, you must set specific header fields and values in your HTTP requests.</span></span> <span data-ttu-id="bec62-140">További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).</span><span class="sxs-lookup"><span data-stu-id="bec62-140">For more information, see [Setup for Media Services REST API Development](media-services-rest-how-to-use.md).</span></span>

## <a name="connect-to-media-services"></a><span data-ttu-id="bec62-141">Kapcsolódás a Media Services szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="bec62-141">Connect to Media Services</span></span>

<span data-ttu-id="bec62-142">Az AMS API-hoz kapcsolódáshoz információkért lásd: [elérni az Azure Media Services API-t az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="bec62-142">For information on how to connect to the AMS API, see [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="bec62-143">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="bec62-143">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="bec62-144">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="bec62-144">You must make subsequent calls to the new URI.</span></span>

## <a name="clear-asset-delivery-policy"></a><span data-ttu-id="bec62-145">Objektumtovábbítási szabályzat törlése</span><span class="sxs-lookup"><span data-stu-id="bec62-145">Clear asset delivery policy</span></span>
### <span data-ttu-id="bec62-146"><a id="create_asset_delivery_policy"></a>Objektumtovábbítási szabályzat létrehozása</span><span class="sxs-lookup"><span data-stu-id="bec62-146"><a id="create_asset_delivery_policy"></a>Create asset delivery policy</span></span>
<span data-ttu-id="bec62-147">A következő HTTP-kérést hoz létre, amely meghatározza a dinamikus titkosítás nem alkalmazandó, és a következő protokoll egyik adatfolyam továbbítására objektumtovábbítási szabályzat: MPEG DASH, HLS vagy Smooth Streaming protokollokat.</span><span class="sxs-lookup"><span data-stu-id="bec62-147">The following HTTP request creates an asset delivery policy that specifies to not apply dynamic encryption and to deliver the stream in any of the following protocols:  MPEG DASH, HLS, and Smooth Streaming protocols.</span></span> 

<span data-ttu-id="bec62-148">Milyen értékeket is megadhat egy AssetDeliveryPolicy létrehozásakor, témakörben olvashat a [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec62-148">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="bec62-149">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="bec62-149">Request:</span></span>

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

<span data-ttu-id="bec62-150">Válasz:</span><span class="sxs-lookup"><span data-stu-id="bec62-150">Response:</span></span>

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

### <span data-ttu-id="bec62-151"><a id="link_asset_with_asset_delivery_policy"></a>Kapcsolat eszköz az adategység továbbítási házirendjét</span><span class="sxs-lookup"><span data-stu-id="bec62-151"><a id="link_asset_with_asset_delivery_policy"></a>Link asset with asset delivery policy</span></span>
<span data-ttu-id="bec62-152">A következő HTTP-kérelem az adategység továbbítási házirendjét, hogy a megadott eszköz hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bec62-152">The following HTTP request links the specified asset to the asset delivery policy to.</span></span>

<span data-ttu-id="bec62-153">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="bec62-153">Request:</span></span>

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

<span data-ttu-id="bec62-154">Válasz:</span><span class="sxs-lookup"><span data-stu-id="bec62-154">Response:</span></span>

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a><span data-ttu-id="bec62-155">Objektumtovábbítási szabályzat DynamicEnvelopeEncryption</span><span class="sxs-lookup"><span data-stu-id="bec62-155">DynamicEnvelopeEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a><span data-ttu-id="bec62-156">A EnvelopeEncryption típusú tartalomkulcs létrehozása és csatolása az eszközhöz</span><span class="sxs-lookup"><span data-stu-id="bec62-156">Create content key of the EnvelopeEncryption type and link it to the asset</span></span>
<span data-ttu-id="bec62-157">DynamicEnvelopeEncryption objektumtovábbítási szabályzat megadása esetén ügyeljen arra, hogy az eszköz kapcsolódik egy tartalomkulcsot a EnvelopeEncryption típusú kell.</span><span class="sxs-lookup"><span data-stu-id="bec62-157">When specifying DynamicEnvelopeEncryption delivery policy, you need to make sure to link your asset to a content key of the EnvelopeEncryption type.</span></span> <span data-ttu-id="bec62-158">További információkért lásd: [tartalomkulcs létrehozása](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="bec62-158">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <span data-ttu-id="bec62-159"><a id="get_delivery_url"></a>Kézbesítési URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="bec62-159"><a id="get_delivery_url"></a>Get delivery URL</span></span>
<span data-ttu-id="bec62-160">A megadott kézbesítési módszert a tartalom kulcs az előző lépésben létrehozott kézbesítési URL beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bec62-160">Get the delivery URL for the specified delivery method of the content key created in the previous step.</span></span> <span data-ttu-id="bec62-161">Egy ügyfél használ a visszaadott URL-cím kérése az AES-kulccsal, vagy a PlayReady licenc lejátszásához ahhoz a védett tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="bec62-161">A client uses the returned URL to request an AES key or a PlayReady license in order to playback the protected content.</span></span>

<span data-ttu-id="bec62-162">Adja meg az URL-címének a HTTP-kérelem törzse beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bec62-162">Specify the type of the URL to get in the body of the HTTP request.</span></span> <span data-ttu-id="bec62-163">Véd a tartalmaknak a PlayReady, a Media Services PlayReady licenc licenckérési URL-cím kérése 1 használ a keyDeliveryType: {"keyDeliveryType": 1}.</span><span class="sxs-lookup"><span data-stu-id="bec62-163">If you are protecting your content with PlayReady, request a Media Services PlayReady license acquisition URL, using 1 for the keyDeliveryType: {"keyDeliveryType":1}.</span></span> <span data-ttu-id="bec62-164">Véd-e a tartalom a boríték titkosított, egy kulcs-licenckérési URL-cím kérése az keyDeliveryType 2 megadásával: {"keyDeliveryType": 2}.</span><span class="sxs-lookup"><span data-stu-id="bec62-164">If you are protecting your content with the envelope encryption, request a key acquisition URL by specifying 2 for keyDeliveryType: {"keyDeliveryType":2}.</span></span>

<span data-ttu-id="bec62-165">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="bec62-165">Request:</span></span>

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

<span data-ttu-id="bec62-166">Válasz:</span><span class="sxs-lookup"><span data-stu-id="bec62-166">Response:</span></span>

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


### <a name="create-asset-delivery-policy"></a><span data-ttu-id="bec62-167">Objektumtovábbítási szabályzat létrehozása</span><span class="sxs-lookup"><span data-stu-id="bec62-167">Create asset delivery policy</span></span>
<span data-ttu-id="bec62-168">A következő HTTP-kérést hoz létre a **AssetDeliveryPolicy** dinamikus boríték titkosítási alkalmazandó konfigurált (**DynamicEnvelopeEncryption**) számára a **HLS** protokoll (ebben a példában egyéb protokollok le lesz tiltva streaming).</span><span class="sxs-lookup"><span data-stu-id="bec62-168">The following HTTP request creates the **AssetDeliveryPolicy** that is configured to apply dynamic envelope encryption (**DynamicEnvelopeEncryption**) to the **HLS** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="bec62-169">Milyen értékeket is megadhat egy AssetDeliveryPolicy létrehozásakor, témakörben olvashat a [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec62-169">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="bec62-170">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="bec62-170">Request:</span></span>

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


<span data-ttu-id="bec62-171">Válasz:</span><span class="sxs-lookup"><span data-stu-id="bec62-171">Response:</span></span>

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


### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="bec62-172">Kapcsolat eszköz az adategység továbbítási házirendjét</span><span class="sxs-lookup"><span data-stu-id="bec62-172">Link asset with asset delivery policy</span></span>
<span data-ttu-id="bec62-173">Lásd: [hivatkozás eszköz az adategység továbbítási házirendjét](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="bec62-173">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <a name="dynamiccommonencryption-asset-delivery-policy"></a><span data-ttu-id="bec62-174">Objektumtovábbítási szabályzat DynamicCommonEncryption</span><span class="sxs-lookup"><span data-stu-id="bec62-174">DynamicCommonEncryption asset delivery policy</span></span>
### <a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a><span data-ttu-id="bec62-175">A CommonEncryption típusú tartalomkulcs létrehozása és csatolása az eszközhöz</span><span class="sxs-lookup"><span data-stu-id="bec62-175">Create content key of the CommonEncryption type and link it to the asset</span></span>
<span data-ttu-id="bec62-176">DynamicCommonEncryption objektumtovábbítási szabályzat megadása esetén ügyeljen arra, hogy az eszköz kapcsolódik egy tartalomkulcsot a CommonEncryption típusú kell.</span><span class="sxs-lookup"><span data-stu-id="bec62-176">When specifying DynamicCommonEncryption delivery policy, you need to make sure to link your asset to a content key of the CommonEncryption type.</span></span> <span data-ttu-id="bec62-177">További információkért lásd: [tartalomkulcs létrehozása](media-services-rest-create-contentkey.md)).</span><span class="sxs-lookup"><span data-stu-id="bec62-177">For more information, see: [Creating a content key](media-services-rest-create-contentkey.md)).</span></span>

### <a name="get-delivery-url"></a><span data-ttu-id="bec62-178">Kézbesítési URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="bec62-178">Get Delivery URL</span></span>
<span data-ttu-id="bec62-179">Kézbesítési URL beolvasása a PlayReady kézbesítési módszert a tartalom az előző lépésben létrehozott kulcs.</span><span class="sxs-lookup"><span data-stu-id="bec62-179">Get the delivery URL for the PlayReady delivery method of the content key created in the previous step.</span></span> <span data-ttu-id="bec62-180">Egy ügyfél a védett tartalmak lejátszásához sorrendben PlayReady licencet lekérni a visszaadott URL-címet használ.</span><span class="sxs-lookup"><span data-stu-id="bec62-180">A client uses the returned URL to request a PlayReady license in order to playback the protected content.</span></span> <span data-ttu-id="bec62-181">További információkért lásd: [kézbesítési URL-cím beszerzése](#get_delivery_url).</span><span class="sxs-lookup"><span data-stu-id="bec62-181">For more information, see [Get Delivery URL](#get_delivery_url).</span></span>

### <a name="create-asset-delivery-policy"></a><span data-ttu-id="bec62-182">Objektumtovábbítási szabályzat létrehozása</span><span class="sxs-lookup"><span data-stu-id="bec62-182">Create asset delivery policy</span></span>
<span data-ttu-id="bec62-183">A következő HTTP-kérést hoz létre a **AssetDeliveryPolicy** konfigurált alkalmazni a dynamic common encryption (**DynamicCommonEncryption**) számára a **Smooth Streaming**protokoll (ebben a példában egyéb protokollok le lesz tiltva streaming).</span><span class="sxs-lookup"><span data-stu-id="bec62-183">The following HTTP request creates the **AssetDeliveryPolicy** that is configured to apply dynamic common encryption (**DynamicCommonEncryption**) to the **Smooth Streaming** protocol (in this example, other protocols will be blocked from streaming).</span></span> 

<span data-ttu-id="bec62-184">Milyen értékeket is megadhat egy AssetDeliveryPolicy létrehozásakor, témakörben olvashat a [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.</span><span class="sxs-lookup"><span data-stu-id="bec62-184">For information on what values you can specify when creating an AssetDeliveryPolicy, see the [Types used when defining AssetDeliveryPolicy](#types) section.</span></span>   

<span data-ttu-id="bec62-185">A kérelem:</span><span class="sxs-lookup"><span data-stu-id="bec62-185">Request:</span></span>

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


<span data-ttu-id="bec62-186">Ha a tartalom Widevine DRM segítségével védeni kívánt, frissítse az AssetDeliveryConfiguration értékeket használatára (amely értéke a 7) WidevineLicenseAcquisitionUrl, és adjon meg egy licenctovábbítási szolgáltatása URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bec62-186">If you want to protect your content using Widevine DRM, update the AssetDeliveryConfiguration values to use WidevineLicenseAcquisitionUrl (which has the value of 7) and specify the URL of a license delivery service.</span></span> <span data-ttu-id="bec62-187">A következő AMS-partnereket segítségével Widevine-licencek segítségével: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="bec62-187">You can use the following AMS partners to help you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="bec62-188">Példa:</span><span class="sxs-lookup"><span data-stu-id="bec62-188">For example:</span></span> 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> <span data-ttu-id="bec62-189">Widevine titkosításakor csak lenne DASH használatával küldött.</span><span class="sxs-lookup"><span data-stu-id="bec62-189">When encrypting with Widevine, you would only be able to deliver using DASH.</span></span> <span data-ttu-id="bec62-190">Győződjön meg arról, ha meg szeretné adni az objektumtovábbítási protokoll kötőjel (2).</span><span class="sxs-lookup"><span data-stu-id="bec62-190">Make sure to specify DASH (2) in the asset delivery protocol.</span></span>
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a><span data-ttu-id="bec62-191">Kapcsolat eszköz az adategység továbbítási házirendjét</span><span class="sxs-lookup"><span data-stu-id="bec62-191">Link asset with asset delivery policy</span></span>
<span data-ttu-id="bec62-192">Lásd: [hivatkozás eszköz az adategység továbbítási házirendjét](#link_asset_with_asset_delivery_policy)</span><span class="sxs-lookup"><span data-stu-id="bec62-192">See [Link asset with asset delivery policy](#link_asset_with_asset_delivery_policy)</span></span>

## <span data-ttu-id="bec62-193"><a id="types"></a>Típusok AssetDeliveryPolicy definiálásakor használja</span><span class="sxs-lookup"><span data-stu-id="bec62-193"><a id="types"></a>Types used when defining AssetDeliveryPolicy</span></span>

### <a name="assetdeliveryprotocol"></a><span data-ttu-id="bec62-194">AssetDeliveryProtocol</span><span class="sxs-lookup"><span data-stu-id="bec62-194">AssetDeliveryProtocol</span></span>

<span data-ttu-id="bec62-195">A következő felsorolás ismerteti értékeket adhatja meg az objektumtovábbítási protokoll.</span><span class="sxs-lookup"><span data-stu-id="bec62-195">The following enum describes values you can set for the asset delivery protocol.</span></span>

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

### <a name="assetdeliverypolicytype"></a><span data-ttu-id="bec62-196">AssetDeliveryPolicyType</span><span class="sxs-lookup"><span data-stu-id="bec62-196">AssetDeliveryPolicyType</span></span>

<span data-ttu-id="bec62-197">A következő felsorolás ismerteti, állíthatja be a kézbesítési házirend típusú értékeket.</span><span class="sxs-lookup"><span data-stu-id="bec62-197">The following enum describes values you can set for the asset delivery policy type.</span></span>  

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

### <a name="contentkeydeliverytype"></a><span data-ttu-id="bec62-198">ContentKeyDeliveryType</span><span class="sxs-lookup"><span data-stu-id="bec62-198">ContentKeyDeliveryType</span></span>

<span data-ttu-id="bec62-199">A következő felsorolás ismerteti a segítségével konfigurálhatja a kézbesítési módszert az ügyfél a tartalom kulcs értékeket.</span><span class="sxs-lookup"><span data-stu-id="bec62-199">The following enum describes values you can use to configure the delivery method of the content key to the client.</span></span>
    
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


### <a name="assetdeliverypolicyconfigurationkey"></a><span data-ttu-id="bec62-200">AssetDeliveryPolicyConfigurationKey</span><span class="sxs-lookup"><span data-stu-id="bec62-200">AssetDeliveryPolicyConfigurationKey</span></span>

<span data-ttu-id="bec62-201">A következő felsorolás ismerteti a kulcsok segítségével kéri le a meghatározott konfigurációját objektumtovábbítási szabályzat konfigurálása és értékeket.</span><span class="sxs-lookup"><span data-stu-id="bec62-201">The following enum describes values you can set to configure keys used to get specific configuration for an asset delivery policy.</span></span>

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

## <a name="media-services-learning-paths"></a><span data-ttu-id="bec62-202">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="bec62-202">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="bec62-203">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="bec62-203">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


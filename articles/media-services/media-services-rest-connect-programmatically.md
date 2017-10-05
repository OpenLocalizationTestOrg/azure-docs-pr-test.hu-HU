---
title: "Kapcsolódás a Media Services-fiók REST API használatával |} Microsoft Docs"
description: "Ebben a témakörben bemutatjuk, hogyan csatlakozhat a Media Services előrejelzése REST API-t."
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 79dc64f1-15d8-4a81-b9d9-3d3c44d2e1e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 4feb0eb81823835e8e0b701463d85b27f5598019
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a><span data-ttu-id="cabc6-103">Kapcsolódás a Media Services-fiók Media Services REST API használatával</span><span class="sxs-lookup"><span data-stu-id="cabc6-103">Connecting to Media Services Account using Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cabc6-104">.NET</span><span class="sxs-lookup"><span data-stu-id="cabc6-104">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> * [<span data-ttu-id="cabc6-105">REST</span><span class="sxs-lookup"><span data-stu-id="cabc6-105">REST</span></span>](media-services-rest-connect-programmatically.md)
> 
> 

<span data-ttu-id="cabc6-106">Ez a témakör ismerteti az beszerzése programozott kapcsolódni a Microsoft Azure Media Services, ha meg vannak programozási Media Services REST API-val.</span><span class="sxs-lookup"><span data-stu-id="cabc6-106">This topic describes how to obtain a programmatic connection to Microsoft Azure Media Services when you are programming with the Media Services REST API.</span></span>

<span data-ttu-id="cabc6-107">Két dolog szükség, ha a Microsoft Azure Media Services szolgáltatásainak eléréséhez: egy hozzáférési jogkivonat segítségével Azure Access Control szolgáltatások (ACS), és a Media Services URI magát.</span><span class="sxs-lookup"><span data-stu-id="cabc6-107">Two things are required when accessing Microsoft Azure Media Services: An access token provided by Azure Access Control Services (ACS), and the URI of Media Services itself.</span></span> <span data-ttu-id="cabc6-108">Azt szeretné, hogy ezek a kérelmek létrehozásakor, mindaddig, amíg adja meg a megfelelő fejléc értékét, és a hozzáférési jogkivonat megfelelően előhívásakor továbbítsa a Media Services bármilyen módon használhatja.</span><span class="sxs-lookup"><span data-stu-id="cabc6-108">You can use any means you want when creating these requests as long as you specify the correct header values and pass in the access token correctly when calling into Media Services.</span></span>

<span data-ttu-id="cabc6-109">A következő lépések bemutatják a leggyakrabban használt munkafolyamat, a Media Services REST API való csatlakozáshoz a Media Services használata esetén:</span><span class="sxs-lookup"><span data-stu-id="cabc6-109">The following steps describe the most common workflow when using the Media Services REST API to connect to Media Services:</span></span>

1. <span data-ttu-id="cabc6-110">Egy hozzáférési jogkivonat beolvasása</span><span class="sxs-lookup"><span data-stu-id="cabc6-110">Getting an access token</span></span> 
2. <span data-ttu-id="cabc6-111">Csatlakozás a Media Services URI</span><span class="sxs-lookup"><span data-stu-id="cabc6-111">Connecting to the Media Services URI</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="cabc6-112">Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni.</span><span class="sxs-lookup"><span data-stu-id="cabc6-112">After successfully connecting to https://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="cabc6-113">Meg kell nyitnia az új URI későbbi hívásokat.</span><span class="sxs-lookup"><span data-stu-id="cabc6-113">You must make subsequent calls to the new URI.</span></span>
   > <span data-ttu-id="cabc6-114">A HTTP/1.1 200 választ, amely tartalmazza a ODATA API-metaadatok leírását is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="cabc6-114">You may also receive a HTTP/1.1 200 response that contains the ODATA API metadata description.</span></span>
   > 
   > 
3. <span data-ttu-id="cabc6-115">Írjon a következő API-hívások új URL-címet.</span><span class="sxs-lookup"><span data-stu-id="cabc6-115">Post your subsequent API calls to the new URL.</span></span> 
   
    <span data-ttu-id="cabc6-116">Például ha az próbál csatlakozni, után kapott a következő:</span><span class="sxs-lookup"><span data-stu-id="cabc6-116">For example, if after trying to connect, you got the following:</span></span>
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    <span data-ttu-id="cabc6-117">A következő API-hívások https://wamsbayclus001rest-hs.cloudapp.net/api/ tegye meg.</span><span class="sxs-lookup"><span data-stu-id="cabc6-117">You should post your subsequent API calls to https://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

    >[!NOTE]
    ><span data-ttu-id="cabc6-118">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="cabc6-118">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="cabc6-119">Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cabc6-119">You should use the same policy ID if you are always using the same days / access permissions, for example, policies for locators that are intended to remain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="cabc6-120">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="cabc6-120">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

## <a name="access-control-address"></a><span data-ttu-id="cabc6-121">MAC-címe</span><span class="sxs-lookup"><span data-stu-id="cabc6-121">Access control address</span></span>
<span data-ttu-id="cabc6-122">A Media Services MAC-címe https://wamsprodglobal001acs.accesscontrol.windows.net, kivéve a Észak Kína régió, ahol https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span><span class="sxs-lookup"><span data-stu-id="cabc6-122">Media Services access control address is https://wamsprodglobal001acs.accesscontrol.windows.net, except for North China region, where it is https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span></span>

## <a name="getting-an-access-token"></a><span data-ttu-id="cabc6-123">Egy hozzáférési jogkivonat beolvasása</span><span class="sxs-lookup"><span data-stu-id="cabc6-123">Getting an access token</span></span>
<span data-ttu-id="cabc6-124">Media Services eléréséhez közvetlenül a REST API-n keresztül, egy hozzáférési jogkivonatot beolvasni az ACS-től, illetve használják során minden HTTP-kérelem elvégezte a szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="cabc6-124">To access Media Services directly through the REST API, retrieve an access token from ACS and use it during every HTTP request you make into the service.</span></span> <span data-ttu-id="cabc6-125">A jogkivonat nagyon hasonló a HTTP-kérelem és a OAuth v2 protokollt használó fejlécben megadott hozzáférési jogcímei alapján ACS által biztosított egyéb jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="cabc6-125">This token is similar to other tokens provided by ACS based on access claims provided in the header of an HTTP request and using the OAuth v2 protocol.</span></span> <span data-ttu-id="cabc6-126">Media Services való közvetlen csatlakozás előtt nem kell más előfeltételeket.</span><span class="sxs-lookup"><span data-stu-id="cabc6-126">You do not need any other prerequisites before directly connecting to Media Services.</span></span>

<span data-ttu-id="cabc6-127">A következő példa bemutatja a HTTP-kérés fejlécének és a szervezet jogkivonat beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cabc6-127">The following example shows the HTTP request header and body used to retrieve a token.</span></span>

<span data-ttu-id="cabc6-128">**Fejléc**:</span><span class="sxs-lookup"><span data-stu-id="cabc6-128">**Header**:</span></span>

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


<span data-ttu-id="cabc6-129">**Törzs**:</span><span class="sxs-lookup"><span data-stu-id="cabc6-129">**Body**:</span></span>

<span data-ttu-id="cabc6-130">A kérelem; törzsében client_id és client_secret értékek igazolnia kell client_id és client_secret felel meg az AccountName és az AccountKey értékeket, illetve.</span><span class="sxs-lookup"><span data-stu-id="cabc6-130">You need to prove the client_id and client_secret values in the body of this request; client_id and client_secret correspond to the AccountName and AccountKey values, respectively.</span></span> <span data-ttu-id="cabc6-131">Ezek az értékek vannak által biztosított Media Services a fiók beállításakor.</span><span class="sxs-lookup"><span data-stu-id="cabc6-131">These values are provided to you by Media Services when you set up your account.</span></span> 

<span data-ttu-id="cabc6-132">Vegye figyelembe, hogy a Media Services-fiókhoz tartozó AccountKey csak URL-kódolású (lásd: [százalék-kódolás](http://tools.ietf.org/html/rfc3986#section-2.1) azt a hozzáférési token kérelem client_secret beállítás használatakor.</span><span class="sxs-lookup"><span data-stu-id="cabc6-132">Note that the AccountKey for your Media Services account must be URL-encoded (see [Percent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) when using it as the client_secret value in your access token request.</span></span>

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="cabc6-133">Példa:</span><span class="sxs-lookup"><span data-stu-id="cabc6-133">For example:</span></span> 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="cabc6-134">A következő példa bemutatja a HTTP-válasz, amely tartalmazza a hozzáférési jogkivonat a válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="cabc6-134">The following example shows the HTTP response that contains the access token in the response body.</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670

    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }


> [!NOTE]
> <span data-ttu-id="cabc6-135">Javasoljuk, hogy gyorsítótárazzák a "access_token" és "expires_in" értékek külső tárolóra.</span><span class="sxs-lookup"><span data-stu-id="cabc6-135">It is recommended to cache the "access_token " and "expires_in " values to an external storage.</span></span> <span data-ttu-id="cabc6-136">A tokenadatforrások később is olvassa be a tároló és a Media Services REST API-hívásokban használt újra.</span><span class="sxs-lookup"><span data-stu-id="cabc6-136">The token data could later be retrieved from the storage and re-used in your Media Services REST API calls.</span></span> <span data-ttu-id="cabc6-137">Ez különösen fontos a forgatókönyvek, ahol a token biztonságosan megoszthatók több folyamatok vagy számítógépek között.</span><span class="sxs-lookup"><span data-stu-id="cabc6-137">This is especially useful for scenarios where the token can be securely shared among multiple processes or computers.</span></span>
> 
> 

<span data-ttu-id="cabc6-138">Ügyeljen arra, hogy a "expires_in" értéket, és a hozzáférési jogkivonat figyelheti és frissítheti a REST API-hívások új jogkivonatokkal, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="cabc6-138">Make sure to monitor the "expires_in" value of the access token and update your REST API calls with new tokens as needed.</span></span>

### <a name="connecting-to-the-media-services-uri"></a><span data-ttu-id="cabc6-139">Csatlakozás a Media Services URI</span><span class="sxs-lookup"><span data-stu-id="cabc6-139">Connecting to the Media Services URI</span></span>
<span data-ttu-id="cabc6-140">A legfelső szintű URI a Media Services https://media.windows.net/ nem.</span><span class="sxs-lookup"><span data-stu-id="cabc6-140">The root URI for Media Services is https://media.windows.net/.</span></span> <span data-ttu-id="cabc6-141">Először csatlakozzon a URI, és ha 301 átirányítást vissza válaszként, meg kell győződnie későbbi hívások az új URI.</span><span class="sxs-lookup"><span data-stu-id="cabc6-141">You should initially connect to this URI, and if you get a 301 redirect back in response, you should make subsequent calls to the new URI.</span></span> <span data-ttu-id="cabc6-142">Továbbá ne használjon bármely automatikus átirányítási/követhető logika a a kérelmek.</span><span class="sxs-lookup"><span data-stu-id="cabc6-142">In addition, do not use any auto-redirect/follow logic in your requests.</span></span> <span data-ttu-id="cabc6-143">HTTP-műveletekre és a kérelem szervek a rendszer nem továbbítja az új URI.</span><span class="sxs-lookup"><span data-stu-id="cabc6-143">HTTP verbs and request bodies will not be forwarded to the new URI.</span></span>

<span data-ttu-id="cabc6-144">Vegye figyelembe, hogy a legfelső szintű fel-és eszköz fájlok letöltése URI https://yourstorageaccount.blob.core.windows.net/ ahol a tárfiók neve megegyezik egy a Media Services-fiók beállításakor használt.</span><span class="sxs-lookup"><span data-stu-id="cabc6-144">Note that the root URI for uploading and downloading Asset files is https://yourstorageaccount.blob.core.windows.net/ where the storage account name is the same one you used during your Media Services account setup.</span></span>

<span data-ttu-id="cabc6-145">A következő példa bemutatja a Media Services legfelső szintű URI (https://media.windows.net/) HTTP-kérelem.</span><span class="sxs-lookup"><span data-stu-id="cabc6-145">The following example demonstrates HTTP request to the Media Services root URI (https://media.windows.net/).</span></span> <span data-ttu-id="cabc6-146">A kérelem vissza válaszként 301 átirányítást lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="cabc6-146">The request gets a 301 redirect back in response.</span></span> <span data-ttu-id="cabc6-147">A későbbi kérés használja az új URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span><span class="sxs-lookup"><span data-stu-id="cabc6-147">The subsequent request is using the new URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span></span>     

<span data-ttu-id="cabc6-148">**HTTP-kérelem**:</span><span class="sxs-lookup"><span data-stu-id="cabc6-148">**HTTP Request**:</span></span>

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


<span data-ttu-id="cabc6-149">**HTTP-válasz**:</span><span class="sxs-lookup"><span data-stu-id="cabc6-149">**HTTP Response**:</span></span>

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164

    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


<span data-ttu-id="cabc6-150">**HTTP-kérelem** (új URI segítségével):</span><span class="sxs-lookup"><span data-stu-id="cabc6-150">**HTTP Request** (using the new URI):</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="cabc6-151">**HTTP-válasz**:</span><span class="sxs-lookup"><span data-stu-id="cabc6-151">**HTTP Response**:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}



> [!NOTE]
> <span data-ttu-id="cabc6-152">Miután az új URI, ez az URI, amely a Media Services folytatott kommunikációhoz használandó.</span><span class="sxs-lookup"><span data-stu-id="cabc6-152">Once you get the new URI, that is the URI that should be used to communicate with Media Services.</span></span> 
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="cabc6-153">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="cabc6-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="cabc6-154">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="cabc6-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


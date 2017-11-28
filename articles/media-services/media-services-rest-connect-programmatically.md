---
title: "aaaConnecting tooMedia Services-fiók REST API használatával |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan tooconnect tooMedia szolgáltatások előrejelzése REST API-t."
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
ms.openlocfilehash: 1d5064a3612dc96f5c5ad910d183d84fb70a3b6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-toomedia-services-account-using-media-services-rest-api"></a><span data-ttu-id="81d96-103">Csatlakozás a Media Services REST API használatával Services-fiók tooMedia</span><span class="sxs-lookup"><span data-stu-id="81d96-103">Connecting tooMedia Services Account using Media Services REST API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="81d96-104">.NET</span><span class="sxs-lookup"><span data-stu-id="81d96-104">.NET</span></span>](media-services-dotnet-connect-programmatically.md)
> * [<span data-ttu-id="81d96-105">REST</span><span class="sxs-lookup"><span data-stu-id="81d96-105">REST</span></span>](media-services-rest-connect-programmatically.md)
> 
> 

<span data-ttu-id="81d96-106">Ez a témakör ismerteti, hogyan tooobtain egy Azure Media Services, amikor a rendszer programozás a szoftveres kapcsolatot tooMicrosoft hello Media Services REST API-t.</span><span class="sxs-lookup"><span data-stu-id="81d96-106">This topic describes how tooobtain a programmatic connection tooMicrosoft Azure Media Services when you are programming with hello Media Services REST API.</span></span>

<span data-ttu-id="81d96-107">Két dolog szükség, ha a Microsoft Azure Media Services szolgáltatásainak eléréséhez: egy hozzáférési jogkivonat segítségével Azure Access Control szolgáltatások (ACS), és a URI Media Services hello magát.</span><span class="sxs-lookup"><span data-stu-id="81d96-107">Two things are required when accessing Microsoft Azure Media Services: An access token provided by Azure Access Control Services (ACS), and hello URI of Media Services itself.</span></span> <span data-ttu-id="81d96-108">Azt szeretné, hogy ezek a kérelmek létrehozásakor, mindaddig, amíg hello megfelelő fejléc értékeket megadni, és a hozzáférési jogkivonat hello megfelelően előhívásakor továbbítsa a Media Services bármilyen módon használhatja.</span><span class="sxs-lookup"><span data-stu-id="81d96-108">You can use any means you want when creating these requests as long as you specify hello correct header values and pass in hello access token correctly when calling into Media Services.</span></span>

<span data-ttu-id="81d96-109">a lépéseket követve hello hello leggyakoribb munkafolyamat mutatják be, amikor szolgáltatási hello Media Services REST API tooconnect tooMedia használatával:</span><span class="sxs-lookup"><span data-stu-id="81d96-109">hello following steps describe hello most common workflow when using hello Media Services REST API tooconnect tooMedia Services:</span></span>

1. <span data-ttu-id="81d96-110">Egy hozzáférési jogkivonat beolvasása</span><span class="sxs-lookup"><span data-stu-id="81d96-110">Getting an access token</span></span> 
2. <span data-ttu-id="81d96-111">Csatlakozás a Media Services URI toohello</span><span class="sxs-lookup"><span data-stu-id="81d96-111">Connecting toohello Media Services URI</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="81d96-112">Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást.</span><span class="sxs-lookup"><span data-stu-id="81d96-112">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="81d96-113">Meg kell nyitnia a további hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="81d96-113">You must make subsequent calls toohello new URI.</span></span>
   > <span data-ttu-id="81d96-114">A HTTP/1.1 200 válasz hello ODATA API-metaadatok leírását tartalmazó is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="81d96-114">You may also receive a HTTP/1.1 200 response that contains hello ODATA API metadata description.</span></span>
   > 
   > 
3. <span data-ttu-id="81d96-115">Közzététel az ezt követő API hívások toohello új URL-címe.</span><span class="sxs-lookup"><span data-stu-id="81d96-115">Post your subsequent API calls toohello new URL.</span></span> 
   
    <span data-ttu-id="81d96-116">Például ha az tooconnect próbálja, után kapott hello következő:</span><span class="sxs-lookup"><span data-stu-id="81d96-116">For example, if after trying tooconnect, you got hello following:</span></span>
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    <span data-ttu-id="81d96-117">A következő API-hívások toohttps://wamsbayclus001rest-hs.cloudapp.net/api/ tegye meg.</span><span class="sxs-lookup"><span data-stu-id="81d96-117">You should post your subsequent API calls toohttps://wamsbayclus001rest-hs.cloudapp.net/api/.</span></span>

    >[!NOTE]
    ><span data-ttu-id="81d96-118">A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat.</span><span class="sxs-lookup"><span data-stu-id="81d96-118">There is a limit of 1,000,000 policies for different AMS policies (for example, for Locator policy or ContentKeyAuthorizationPolicy).</span></span> <span data-ttu-id="81d96-119">Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek.</span><span class="sxs-lookup"><span data-stu-id="81d96-119">You should use hello same policy ID if you are always using hello same days / access permissions, for example, policies for locators that are intended tooremain in place for a long time (non-upload policies).</span></span> <span data-ttu-id="81d96-120">További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.</span><span class="sxs-lookup"><span data-stu-id="81d96-120">For more information, see [this](media-services-dotnet-manage-entities.md#limit-access-policies) topic.</span></span>

## <a name="access-control-address"></a><span data-ttu-id="81d96-121">MAC-címe</span><span class="sxs-lookup"><span data-stu-id="81d96-121">Access control address</span></span>
<span data-ttu-id="81d96-122">A Media Services MAC-címe https://wamsprodglobal001acs.accesscontrol.windows.net, kivéve a Észak Kína régió, ahol https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span><span class="sxs-lookup"><span data-stu-id="81d96-122">Media Services access control address is https://wamsprodglobal001acs.accesscontrol.windows.net, except for North China region, where it is https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.</span></span>

## <a name="getting-an-access-token"></a><span data-ttu-id="81d96-123">Egy hozzáférési jogkivonat beolvasása</span><span class="sxs-lookup"><span data-stu-id="81d96-123">Getting an access token</span></span>
<span data-ttu-id="81d96-124">tooaccess Media Services közvetlenül hello REST API-t, a hozzáférési jogkivonatot lekérdezni az ACS-től, illetve használják során minden HTTP-kérelem hello szolgáltatásban végez.</span><span class="sxs-lookup"><span data-stu-id="81d96-124">tooaccess Media Services directly through hello REST API, retrieve an access token from ACS and use it during every HTTP request you make into hello service.</span></span> <span data-ttu-id="81d96-125">Ez a jogkivonat nagyon hasonló tooother jogkivonatok HTTP-kérelem és hello OAuth v2 protokollt használó hello fejlécben megadott hozzáférési jogcímei alapján ACS által biztosított.</span><span class="sxs-lookup"><span data-stu-id="81d96-125">This token is similar tooother tokens provided by ACS based on access claims provided in hello header of an HTTP request and using hello OAuth v2 protocol.</span></span> <span data-ttu-id="81d96-126">Egyéb előfeltételeket nem kell előtt tooMedia szolgáltatások használatával közvetlenül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="81d96-126">You do not need any other prerequisites before directly connecting tooMedia Services.</span></span>

<span data-ttu-id="81d96-127">hello következő példa bemutatja hello HTTP-kérés fejlécének és használt törzs tooretrieve jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="81d96-127">hello following example shows hello HTTP request header and body used tooretrieve a token.</span></span>

<span data-ttu-id="81d96-128">**Fejléc**:</span><span class="sxs-lookup"><span data-stu-id="81d96-128">**Header**:</span></span>

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


<span data-ttu-id="81d96-129">**Törzs**:</span><span class="sxs-lookup"><span data-stu-id="81d96-129">**Body**:</span></span>

<span data-ttu-id="81d96-130">Tooprove hello client_id és client_secret hello törzsében a kérelem; értékek van szüksége client_id és client_secret megfelelnek a toohello AccountName és az AccountKey értékeket, illetve.</span><span class="sxs-lookup"><span data-stu-id="81d96-130">You need tooprove hello client_id and client_secret values in hello body of this request; client_id and client_secret correspond toohello AccountName and AccountKey values, respectively.</span></span> <span data-ttu-id="81d96-131">Ezek az értékek tooyou által biztosított Media Services a fiók beállításakor.</span><span class="sxs-lookup"><span data-stu-id="81d96-131">These values are provided tooyou by Media Services when you set up your account.</span></span> 

<span data-ttu-id="81d96-132">Vegye figyelembe, hogy hello accountkey elemeket, a Media Services-fiók URL-kódolású kell lennie (lásd: [százalék-kódolás](http://tools.ietf.org/html/rfc3986#section-2.1) azt a hozzáférési token kérés hello client_secret értékként használatakor.</span><span class="sxs-lookup"><span data-stu-id="81d96-132">Note that hello AccountKey for your Media Services account must be URL-encoded (see [Percent-Encoding](http://tools.ietf.org/html/rfc3986#section-2.1) when using it as hello client_secret value in your access token request.</span></span>

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="81d96-133">Példa:</span><span class="sxs-lookup"><span data-stu-id="81d96-133">For example:</span></span> 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


<span data-ttu-id="81d96-134">hello következő példa bemutatja hello HTTP-válasz, amely tartalmazza a hello access token hello válasz törzsében.</span><span class="sxs-lookup"><span data-stu-id="81d96-134">hello following example shows hello HTTP response that contains hello access token in hello response body.</span></span>

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
> <span data-ttu-id="81d96-135">Ajánlott toocache hello "access_token" és "expires_in" értékek tooan külső tárhelyen.</span><span class="sxs-lookup"><span data-stu-id="81d96-135">It is recommended toocache hello "access_token " and "expires_in " values tooan external storage.</span></span> <span data-ttu-id="81d96-136">hello tokenadatforrások később is hello tárolási lekért és a Media Services REST API-hívásokban használt újra.</span><span class="sxs-lookup"><span data-stu-id="81d96-136">hello token data could later be retrieved from hello storage and re-used in your Media Services REST API calls.</span></span> <span data-ttu-id="81d96-137">Ez különösen fontos a forgatókönyvek, ahol hello token biztonságosan megoszthatók több folyamatok vagy számítógépek között.</span><span class="sxs-lookup"><span data-stu-id="81d96-137">This is especially useful for scenarios where hello token can be securely shared among multiple processes or computers.</span></span>
> 
> 

<span data-ttu-id="81d96-138">Győződjön meg arról, hogy toomonitor hello "expires_in" érték hello hozzáférési jogkivonat, és szükség szerint új jogkivonatokkal frissítse a REST API-hívásokat.</span><span class="sxs-lookup"><span data-stu-id="81d96-138">Make sure toomonitor hello "expires_in" value of hello access token and update your REST API calls with new tokens as needed.</span></span>

### <a name="connecting-toohello-media-services-uri"></a><span data-ttu-id="81d96-139">Csatlakozás a Media Services URI toohello</span><span class="sxs-lookup"><span data-stu-id="81d96-139">Connecting toohello Media Services URI</span></span>
<span data-ttu-id="81d96-140">hello legfelső szintű Media Services URI https://media.windows.net/.</span><span class="sxs-lookup"><span data-stu-id="81d96-140">hello root URI for Media Services is https://media.windows.net/.</span></span> <span data-ttu-id="81d96-141">Kezdetben a toothis URI kell csatlakoznia, és ha 301 átirányítást vissza válaszként, meg kell győződnie későbbi hívások toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="81d96-141">You should initially connect toothis URI, and if you get a 301 redirect back in response, you should make subsequent calls toohello new URI.</span></span> <span data-ttu-id="81d96-142">Továbbá ne használjon bármely automatikus átirányítási/követhető logika a a kérelmek.</span><span class="sxs-lookup"><span data-stu-id="81d96-142">In addition, do not use any auto-redirect/follow logic in your requests.</span></span> <span data-ttu-id="81d96-143">HTTP-műveletekre és a kérelem szervek nem továbbítják toohello új URI.</span><span class="sxs-lookup"><span data-stu-id="81d96-143">HTTP verbs and request bodies will not be forwarded toohello new URI.</span></span>

<span data-ttu-id="81d96-144">Vegye figyelembe, hogy hello legfelső szintű URI feltöltése és eszköz fájlok letöltése https://yourstorageaccount.blob.core.windows.net/ ahol hello tárfiókneve hello azonos egy, a Media Services-fiók beállításakor használt.</span><span class="sxs-lookup"><span data-stu-id="81d96-144">Note that hello root URI for uploading and downloading Asset files is https://yourstorageaccount.blob.core.windows.net/ where hello storage account name is hello same one you used during your Media Services account setup.</span></span>

<span data-ttu-id="81d96-145">a következő példa hello HTTP kérelem toohello Media Services gyökér URI (https://media.windows.net/) mutatja be.</span><span class="sxs-lookup"><span data-stu-id="81d96-145">hello following example demonstrates HTTP request toohello Media Services root URI (https://media.windows.net/).</span></span> <span data-ttu-id="81d96-146">hello kérelem 301 átirányítást lekérdezi vissza válaszként.</span><span class="sxs-lookup"><span data-stu-id="81d96-146">hello request gets a 301 redirect back in response.</span></span> <span data-ttu-id="81d96-147">hello későbbi kérés használ hello új URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span><span class="sxs-lookup"><span data-stu-id="81d96-147">hello subsequent request is using hello new URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).</span></span>     

<span data-ttu-id="81d96-148">**HTTP-kérelem**:</span><span class="sxs-lookup"><span data-stu-id="81d96-148">**HTTP Request**:</span></span>

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


<span data-ttu-id="81d96-149">**HTTP-válasz**:</span><span class="sxs-lookup"><span data-stu-id="81d96-149">**HTTP Response**:</span></span>

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
    <h2>Object moved too<a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


<span data-ttu-id="81d96-150">**HTTP-kérelem** (új URI hello használatával):</span><span class="sxs-lookup"><span data-stu-id="81d96-150">**HTTP Request** (using hello new URI):</span></span>

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


<span data-ttu-id="81d96-151">**HTTP-válasz**:</span><span class="sxs-lookup"><span data-stu-id="81d96-151">**HTTP Response**:</span></span>

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
> <span data-ttu-id="81d96-152">Miután hello új URI, hello URI, amely a Media Services használt toocommunicate kell lennie.</span><span class="sxs-lookup"><span data-stu-id="81d96-152">Once you get hello new URI, that is hello URI that should be used toocommunicate with Media Services.</span></span> 
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="81d96-153">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="81d96-153">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="81d96-154">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="81d96-154">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


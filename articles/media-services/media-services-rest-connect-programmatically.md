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
# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Kapcsolódás a Media Services-fiók Media Services REST API használatával
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-connect-programmatically.md)
> * [REST](media-services-rest-connect-programmatically.md)
> 
> 

Ez a témakör ismerteti az beszerzése programozott kapcsolódni a Microsoft Azure Media Services, ha meg vannak programozási Media Services REST API-val.

Két dolog szükség, ha a Microsoft Azure Media Services szolgáltatásainak eléréséhez: egy hozzáférési jogkivonat segítségével Azure Access Control szolgáltatások (ACS), és a Media Services URI magát. Azt szeretné, hogy ezek a kérelmek létrehozásakor, mindaddig, amíg adja meg a megfelelő fejléc értékét, és a hozzáférési jogkivonat megfelelően előhívásakor továbbítsa a Media Services bármilyen módon használhatja.

A következő lépések bemutatják a leggyakrabban használt munkafolyamat, a Media Services REST API való csatlakozáshoz a Media Services használata esetén:

1. Egy hozzáférési jogkivonat beolvasása 
2. Csatlakozás a Media Services URI 
   
   > [!NOTE]
   > Sikeresen csatlakoztassa a https://media.windows.net, adja meg egy másik Media Services URI 301 átirányítást fog kapni. Meg kell nyitnia az új URI későbbi hívásokat.
   > A HTTP/1.1 200 választ, amely tartalmazza a ODATA API-metaadatok leírását is megjelenhet.
   > 
   > 
3. Írjon a következő API-hívások új URL-címet. 
   
    Például ha az próbál csatlakozni, után kapott a következő:
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    A következő API-hívások https://wamsbayclus001rest-hs.cloudapp.net/api/ tegye meg.

    >[!NOTE]
    >A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Ha mindig ugyanazokat a napokat/hozzáférési engedélyeket használja (például olyan keresők szabályzatait, amelyek hosszú ideig érvényben maradnak, vagyis nem feltöltött szabályzatokat), a szabályzatazonosítónak is ugyanannak kell lennie. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.

## <a name="access-control-address"></a>MAC-címe
A Media Services MAC-címe https://wamsprodglobal001acs.accesscontrol.windows.net, kivéve a Észak Kína régió, ahol https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

## <a name="getting-an-access-token"></a>Egy hozzáférési jogkivonat beolvasása
Media Services eléréséhez közvetlenül a REST API-n keresztül, egy hozzáférési jogkivonatot beolvasni az ACS-től, illetve használják során minden HTTP-kérelem elvégezte a szolgáltatásba. A jogkivonat nagyon hasonló a HTTP-kérelem és a OAuth v2 protokollt használó fejlécben megadott hozzáférési jogcímei alapján ACS által biztosított egyéb jogkivonatokat. Media Services való közvetlen csatlakozás előtt nem kell más előfeltételeket.

A következő példa bemutatja a HTTP-kérés fejlécének és a szervezet jogkivonat beolvasása.

**Fejléc**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


**Törzs**:

A kérelem; törzsében client_id és client_secret értékek igazolnia kell client_id és client_secret felel meg az AccountName és az AccountKey értékeket, illetve. Ezek az értékek vannak által biztosított Media Services a fiók beállításakor. 

Vegye figyelembe, hogy a Media Services-fiókhoz tartozó AccountKey csak URL-kódolású (lásd: [százalék-kódolás](http://tools.ietf.org/html/rfc3986#section-2.1) azt a hozzáférési token kérelem client_secret beállítás használatakor.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Példa: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


A következő példa bemutatja a HTTP-válasz, amely tartalmazza a hozzáférési jogkivonat a válasz törzsében.

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
> Javasoljuk, hogy gyorsítótárazzák a "access_token" és "expires_in" értékek külső tárolóra. A tokenadatforrások később is olvassa be a tároló és a Media Services REST API-hívásokban használt újra. Ez különösen fontos a forgatókönyvek, ahol a token biztonságosan megoszthatók több folyamatok vagy számítógépek között.
> 
> 

Ügyeljen arra, hogy a "expires_in" értéket, és a hozzáférési jogkivonat figyelheti és frissítheti a REST API-hívások új jogkivonatokkal, igény szerint.

### <a name="connecting-to-the-media-services-uri"></a>Csatlakozás a Media Services URI
A legfelső szintű URI a Media Services https://media.windows.net/ nem. Először csatlakozzon a URI, és ha 301 átirányítást vissza válaszként, meg kell győződnie későbbi hívások az új URI. Továbbá ne használjon bármely automatikus átirányítási/követhető logika a a kérelmek. HTTP-műveletekre és a kérelem szervek a rendszer nem továbbítja az új URI.

Vegye figyelembe, hogy a legfelső szintű fel-és eszköz fájlok letöltése URI https://yourstorageaccount.blob.core.windows.net/ ahol a tárfiók neve megegyezik egy a Media Services-fiók beállításakor használt.

A következő példa bemutatja a Media Services legfelső szintű URI (https://media.windows.net/) HTTP-kérelem. A kérelem vissza válaszként 301 átirányítást lekérdezi. A későbbi kérés használja az új URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-kérelem**:

    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-válasz**:

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


**HTTP-kérelem** (új URI segítségével):

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-válasz**:

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
> Miután az új URI, ez az URI, amely a Media Services folytatott kommunikációhoz használandó. 
> 
> 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


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
# <a name="connecting-toomedia-services-account-using-media-services-rest-api"></a>Csatlakozás a Media Services REST API használatával Services-fiók tooMedia
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-connect-programmatically.md)
> * [REST](media-services-rest-connect-programmatically.md)
> 
> 

Ez a témakör ismerteti, hogyan tooobtain egy Azure Media Services, amikor a rendszer programozás a szoftveres kapcsolatot tooMicrosoft hello Media Services REST API-t.

Két dolog szükség, ha a Microsoft Azure Media Services szolgáltatásainak eléréséhez: egy hozzáférési jogkivonat segítségével Azure Access Control szolgáltatások (ACS), és a URI Media Services hello magát. Azt szeretné, hogy ezek a kérelmek létrehozásakor, mindaddig, amíg hello megfelelő fejléc értékeket megadni, és a hozzáférési jogkivonat hello megfelelően előhívásakor továbbítsa a Media Services bármilyen módon használhatja.

a lépéseket követve hello hello leggyakoribb munkafolyamat mutatják be, amikor szolgáltatási hello Media Services REST API tooconnect tooMedia használatával:

1. Egy hozzáférési jogkivonat beolvasása 
2. Csatlakozás a Media Services URI toohello 
   
   > [!NOTE]
   > Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.
   > A HTTP/1.1 200 válasz hello ODATA API-metaadatok leírását tartalmazó is megjelenhet.
   > 
   > 
3. Közzététel az ezt követő API hívások toohello új URL-címe. 
   
    Például ha az tooconnect próbálja, után kapott hello következő:
   
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
   
    A következő API-hívások toohttps://wamsbayclus001rest-hs.cloudapp.net/api/ tegye meg.

    >[!NOTE]
    >A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.

## <a name="access-control-address"></a>MAC-címe
A Media Services MAC-címe https://wamsprodglobal001acs.accesscontrol.windows.net, kivéve a Észak Kína régió, ahol https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

## <a name="getting-an-access-token"></a>Egy hozzáférési jogkivonat beolvasása
tooaccess Media Services közvetlenül hello REST API-t, a hozzáférési jogkivonatot lekérdezni az ACS-től, illetve használják során minden HTTP-kérelem hello szolgáltatásban végez. Ez a jogkivonat nagyon hasonló tooother jogkivonatok HTTP-kérelem és hello OAuth v2 protokollt használó hello fejlécben megadott hozzáférési jogcímei alapján ACS által biztosított. Egyéb előfeltételeket nem kell előtt tooMedia szolgáltatások használatával közvetlenül csatlakozik.

hello következő példa bemutatja hello HTTP-kérés fejlécének és használt törzs tooretrieve jogkivonatot.

**Fejléc**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json


**Törzs**:

Tooprove hello client_id és client_secret hello törzsében a kérelem; értékek van szüksége client_id és client_secret megfelelnek a toohello AccountName és az AccountKey értékeket, illetve. Ezek az értékek tooyou által biztosított Media Services a fiók beállításakor. 

Vegye figyelembe, hogy hello accountkey elemeket, a Media Services-fiók URL-kódolású kell lennie (lásd: [százalék-kódolás](http://tools.ietf.org/html/rfc3986#section-2.1) azt a hozzáférési token kérés hello client_secret értékként használatakor.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Példa: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


hello következő példa bemutatja hello HTTP-válasz, amely tartalmazza a hello access token hello válasz törzsében.

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
> Ajánlott toocache hello "access_token" és "expires_in" értékek tooan külső tárhelyen. hello tokenadatforrások később is hello tárolási lekért és a Media Services REST API-hívásokban használt újra. Ez különösen fontos a forgatókönyvek, ahol hello token biztonságosan megoszthatók több folyamatok vagy számítógépek között.
> 
> 

Győződjön meg arról, hogy toomonitor hello "expires_in" érték hello hozzáférési jogkivonat, és szükség szerint új jogkivonatokkal frissítse a REST API-hívásokat.

### <a name="connecting-toohello-media-services-uri"></a>Csatlakozás a Media Services URI toohello
hello legfelső szintű Media Services URI https://media.windows.net/. Kezdetben a toothis URI kell csatlakoznia, és ha 301 átirányítást vissza válaszként, meg kell győződnie későbbi hívások toohello új URI. Továbbá ne használjon bármely automatikus átirányítási/követhető logika a a kérelmek. HTTP-műveletekre és a kérelem szervek nem továbbítják toohello új URI.

Vegye figyelembe, hogy hello legfelső szintű URI feltöltése és eszköz fájlok letöltése https://yourstorageaccount.blob.core.windows.net/ ahol hello tárfiókneve hello azonos egy, a Media Services-fiók beállításakor használt.

a következő példa hello HTTP kérelem toohello Media Services gyökér URI (https://media.windows.net/) mutatja be. hello kérelem 301 átirányítást lekérdezi vissza válaszként. hello későbbi kérés használ hello új URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

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
    <h2>Object moved too<a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-kérelem** (új URI hello használatával):

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
> Miután hello új URI, hello URI, amely a Media Services használt toocommunicate kell lennie. 
> 
> 

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


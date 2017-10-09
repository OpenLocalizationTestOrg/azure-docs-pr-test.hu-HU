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
# <a name="configuring-asset-delivery-policies"></a>Adategység továbbítási házirendjeit konfigurálása
[!INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Ha azt tervezi, hogy dinamikusan titkosított toodeliver eszközök, hello lépések egyikét a hello Media Services content kézbesítési munkafolyamatot továbbítási házirendjeit eszközök konfigurálását. hogyan szeretné az eszköz toobe kézbesíteni a közli hello objektumtovábbítási szabályzat Media Services: az adatfolyam-továbbítási protokoll kell az eszköz dinamikusan csomagolható (például MPEG DASH, HLS, Smooth Streaming, vagy az összes), toodynamically szeretné-e az eszköz titkosítása és hogyan (boríték vagy közös titkosítási).

Ez a témakör ismerteti, miért és hogyan toocreate, és konfigurálja az adategység továbbítási házirendjeit.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 
>
>Emellett toobe képes toouse dinamikus csomagolás és a dinamikus titkosítás az eszköz tartalmaznia kell egy adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá készletét.

Eltérő házirendek toohello alkalmazhat azonos eszköz. Például alkalmazhat PlayReady titkosítási tooSmooth Streaming és AES Envelope titkosítási tooMPEG DASH vagy HLS. Nincsenek megadva a továbbítási szabályzatban protokollok (például hozzáadhat egy szabályzatban, amely csak HLS hello protokoll) le lesz tiltva streaming. hello kivétel toothis jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán. Ezt követően hello törölje a jelet minden protokoll engedélyezett.

Ha azt szeretné, hogy a tároló titkosított eszköz toodeliver, konfigurálnia kell a hello adategység továbbítási házirendjét. Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét. Például toodeliver az eszköz titkosítva Advanced Encryption Standard (AES) boríték titkosítási kulcs, állítsa be a hello házirend típusa túl**DynamicEnvelopeEncryption**. tooremove tárolás titkosítása és az adatfolyam hello eszköz törlése, hello beállítása hello házirend típusa túl**NoDynamicEncryption**. Példák hogyan tooconfigure ezeket a házirend-típusainak kövesse.

Attól függően, hogyan hello objektumtovábbítási szabályzat konfigurálása, lenne képes toodynamically csomag kell, dinamikusan titkosítani és adatfolyamként küldje el a következő adatfolyam-továbbítási protokollok hello: Smooth Streaming, HLS, MPEG DASH-streameket.

a következő listán mutatja hello hello formázza toostream Smooth, HLS, DASH használatát.

Smooth Streaming:

{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest

HLS:

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Hogyan toopublish egy eszköz és -buildek a streamelési URL-cím: kapcsolatos utasításokat [streamelési URL-cím összeállítása](media-services-deliver-streaming-content.md).

## <a name="considerations"></a>Megfontolandó szempontok
* Egy AssetDeliveryPolicy társított egy eszköz, amíg az eszköz számára, hogy létezik egy (adatfolyam) OnDemand-kereső nem törölhető. hello ajánljuk tooremove hello házirend hello eszközből, hello házirend törlése előtt.
* A streamelési lokátorok létrehozásához egy tárolási titkosított eszköz nem hozható létre, ha nincs objektumtovábbítási szabályzat beállítása.  Ha hello eszköz nem alkalmaz, hello rendszer tájékoztatja lokátor és adatfolyam hello eszköz a hello törlése nélkül objektumtovábbítási szabályzat létrehozása.
* Egyetlen eszköz társított több adategység továbbítási házirendjeit is rendelkezik, de csak egyirányú toohandle egy adott AssetDeliveryProtocol adhat meg.  Tehát ha toolink két továbbítási házirendjeit által megadott hello AssetDeliveryProtocol.SmoothStreaming protokoll, amely a hibát okoz, mivel a hello rendszer nem tudja, melyik felel meg szeretné tooapply amikor egy ügyfél Smooth Streaming kérelmet.
* Ha egy eszköz rendelkezik egy meglévő streamelési locator, nem egy új házirend toohello eszköz hivatkozásra, leválasztását hello eszköz a meglévő házirend, és nem frissíthetők a továbbítási szabályzatban hello eszközhöz társított.  Ön először tooremove hello streamelési locator rendelkezik, állíthatja hello házirendeket és majd hozza újra létre a lokátor hello.  Hello azonos locatorId ismételt létrehozásakor streamelési locator, de hello győződjön meg arról, hogy problémákat nem okozhat az ügyfelek, mivel a tartalom gyorsítótárazható hello származási vagy egy alárendelt CDN által használható.

>[!NOTE]

>A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre. További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).

## <a name="connect-toomedia-services"></a>Connect tooMedia szolgáltatások

Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.

## <a name="clear-asset-delivery-policy"></a>Objektumtovábbítási szabályzat törlése
### <a id="create_asset_delivery_policy"></a>Objektumtovábbítási szabályzat létrehozása
hello következő HTTP-kérelem hoz létre, amelyben toonot alkalmazni a dinamikus titkosítás és toodeliver hello adatfolyam egyik sem szerepel a következő hello protokollok objektumtovábbítási szabályzat: MPEG DASH, HLS vagy Smooth Streaming protokollokat. 

Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.   

A kérelem:

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

Válasz:

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

### <a id="link_asset_with_asset_delivery_policy"></a>Kapcsolat eszköz az adategység továbbítási házirendjét
a következő HTTP-kérelem hivatkozások hello hello eszköz toohello objektumtovábbítási szabályzat számára megadott.

A kérelem:

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

Válasz:

    HTTP/1.1 204 No Content


## <a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Objektumtovábbítási szabályzat DynamicEnvelopeEncryption
### <a name="create-content-key-of-hello-envelopeencryption-type-and-link-it-toohello-asset"></a>Hello EnvelopeEncryption típusú tartalomkulcs létrehozása és csatolása toohello eszköz
DynamicEnvelopeEncryption objektumtovábbítási szabályzat megadása esetén kell toomake meg arról, hogy toolink az eszköz tooa tartalom hello EnvelopeEncryption típus kulcsát. További információkért lásd: [tartalomkulcs létrehozása](media-services-rest-create-contentkey.md)).

### <a id="get_delivery_url"></a>Kézbesítési URL-cím beszerzése
Get hello kézbesítési URL-címe hello megadott hello előző lépésben létrehozott hello tartalomkulcsot a kézbesítési módszert. Egy ügyfél URL-cím toorequest visszaadott hello használ az AES-kulcs vagy egy PlayReady licenc rendelés tooplayback hello védett tartalmakat.

Adja meg hello URL-cím tooget hello típusú hello hello HTTP-kérelem törzsét. Véd a tartalmaknak a PlayReady, a Media Services PlayReady licenc licenckérési URL-cím kérése 1 használ hello keyDeliveryType: {"keyDeliveryType": 1}. Véd-e a tartalom hello boríték titkosított, egy kulcs-licenckérési URL-cím kérése az keyDeliveryType 2 megadásával: {"keyDeliveryType": 2}.

A kérelem:

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

Válasz:

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


### <a name="create-asset-delivery-policy"></a>Objektumtovábbítási szabályzat létrehozása
hello következő HTTP-kérelem létrehozása hello **AssetDeliveryPolicy** , amely a konfigurált tooapply dinamikus boríték titkosítás (**DynamicEnvelopeEncryption**) toohello **HLS**protokoll (ebben a példában egyéb protokollok le lesz tiltva streaming). 

Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.   

A kérelem:

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


Válasz:

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


### <a name="link-asset-with-asset-delivery-policy"></a>Kapcsolat eszköz az adategység továbbítási házirendjét
Lásd: [hivatkozás eszköz az adategység továbbítási házirendjét](#link_asset_with_asset_delivery_policy)

## <a name="dynamiccommonencryption-asset-delivery-policy"></a>Objektumtovábbítási szabályzat DynamicCommonEncryption
### <a name="create-content-key-of-hello-commonencryption-type-and-link-it-toohello-asset"></a>Hello CommonEncryption típusú tartalomkulcs létrehozása és csatolása toohello eszköz
DynamicCommonEncryption objektumtovábbítási szabályzat megadása esetén kell toomake meg arról, hogy toolink az eszköz tooa tartalom hello CommonEncryption típus kulcsát. További információkért lásd: [tartalomkulcs létrehozása](media-services-rest-create-contentkey.md)).

### <a name="get-delivery-url"></a>Kézbesítési URL-cím beszerzése
Hello kézbesítési URL-cím beszerzése a hello PlayReady kézbesítési módszert a hello előző lépésben létrehozott hello tartalomkulcsot. Egy ügyfél URL-cím toorequest rendelés tooplayback hello a PlayReady licenc védett tartalom visszaadott hello használ. További információkért lásd: [kézbesítési URL-cím beszerzése](#get_delivery_url).

### <a name="create-asset-delivery-policy"></a>Objektumtovábbítási szabályzat létrehozása
hello következő HTTP-kérelem létrehozása hello **AssetDeliveryPolicy** , amely konfigurált tooapply a dynamic common encryption (**DynamicCommonEncryption**) toohello **Smooth Streaming**  protokoll (ebben a példában egyéb protokollok le lesz tiltva streaming). 

Információk milyen értékeket adhat meg egy AssetDeliveryPolicy létrehozásakor, a következő témakörben: hello [AssetDeliveryPolicy meghatározásakor használhatja típusok](#types) szakasz.   

A kérelem:

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


Ha tooprotect a tartalom Widevine DRM segítségével, hello AssetDeliveryConfiguration értékek toouse WidevineLicenseAcquisitionUrl (amely hello értéke a 7) frissítése, és adjon meg egy licenctovábbítási szolgáltatása hello URL-CÍMÉT. Használhatja a következő AMS-partnereket toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Példa: 

    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

> [!NOTE]
> Widevine titkosításakor csak lenne képes toodeliver kötőjel használatával. Győződjön meg arról, hogy toospecify kötőjel (2) a hello objektumtovábbítási protokoll.
> 
> 

### <a name="link-asset-with-asset-delivery-policy"></a>Kapcsolat eszköz az adategység továbbítási házirendjét
Lásd: [hivatkozás eszköz az adategység továbbítási házirendjét](#link_asset_with_asset_delivery_policy)

## <a id="types"></a>Típusok AssetDeliveryPolicy definiálásakor használja

### <a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol

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

### <a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

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

### <a name="contentkeydeliverytype"></a>ContentKeyDeliveryType

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


### <a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

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


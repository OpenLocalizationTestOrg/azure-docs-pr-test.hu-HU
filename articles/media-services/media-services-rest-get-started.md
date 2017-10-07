---
title: "aaaGet a többi segítségével igény szerinti tartalomtovábbítás használatába |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello végrehajtási egy on igény szerinti tartalomtovábbító alkalmazást az Azure Media Services REST API használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 88194b59-e479-43ac-b179-af4f295e3780
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: f270ed59e9ae9745e8403ec6e19d5c3533fc82b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a>Ismerkedés a többi segítségével igény szerinti tartalomtovábbítás
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

A gyors üzembe helyezés végigvezeti hello végrehajtási egy Video-on-Demand (VoD) tartalomtovábbító alkalmazást Azure Media Services (AMS) REST API-k használatával.

hello útmutató bemutatja a Media Services alapvető munkafolyamatait hello és hello leggyakoribb programozási objektumokat és a Media Services-fejlesztés szükséges feladatok. A hello hello az oktatóanyag befejezése után képes toostream kell vagy fokozatosan letölteni egy feltöltött, kódolt és letöltött példa médiafájlt lesz.

hello következő kép bemutatja a leggyakrabban használt hello objektumok hello Media Services OData modellre VoD-alkalmazások fejlesztése során.

Kattintson a hello kép tooview, teljes méret.  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a>Előfeltételek
hello következő előfeltételeket szükséges toostart fejlesztés a Media Services REST API-khoz.

* Egy Azure-fiók. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* Egy Media Services-fiók. egy Media Services-fiók toocreate lásd [hogyan tooCreate Media Services-fiók](media-services-portal-create-account.md).
* Megismernie az toodevelop Media Services REST API-t. További információkért lásd: [Media Services REST API – áttekintés](media-services-rest-how-to-use.md).
* Az Ön által választott küldő HTTP-kérések és válaszok alkalmazás. Ez az oktatóanyag használja [Fiddler](http://www.telerik.com/download/fiddler).

a következő feladatok hello láthatók a gyors üzembe helyezés.

1. Indítsa el az adatfolyam-továbbítási végpontra (hello Azure-portál használatával).
2. Csatlakozás toohello Media Services-fiók REST API-t.
3. Hozzon létre egy új eszközt, és a REST API-t egy videofájl feltöltése.
4. Kódolja hello forrásfájl az adaptív sávszélességű MP4-fájlsorozattá REST API-t.
5. Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-címet a REST API-t.
6. Tartalom lejátszása

>[!NOTE]
>A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.

Ebben a témakörben használt AMS REST entitások kapcsolatos részletekért lásd: [Azure Media Services REST API-referencia](/rest/api/media/services/azure-media-services-rest-api-reference). Lásd még [Azure Media Services alapfogalmaiért](media-services-concepts.md).

>[!NOTE]
>A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre. További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md).

## <a name="start-streaming-endpoints-using-hello-azure-portal"></a>Indítsa el az adatfolyam-végpontok hello Azure-portál használatával

Ha az Azure Media Services egyik leggyakoribb forgatókönyve hello elkötelezett használatával adatfolyam adaptív sávszélességű streamelést működik. A Media Services dinamikus becsomagolást biztosít, amely lehetővé teszi toodeliver az adaptív sávszélességű MP4-kódolású tartalmak anélkül, hogy előre csomagolt toostore (MPEG DASH, HLS, Smooth Streaming), a Media Services just-in-time, által támogatott streamformátumok Ezekbe a formátumokba egyes verzióit.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.

toostart hello streamvégpontra, a következő hello:

1. Jelentkezzen be hello [Azure-portálon](https://portal.azure.com/).
2. Hello-beállítások ablakában kattintson a Streaming végpontok.
3. Kattintson a hello alapértelmezett streamvégpontra.

    hello alapértelmezett STREAMING ENDPOINT részletek ablak.

4. Kattintson a hello Start ikonra.
5. Kattintson a hello Mentés gombra toosave a módosításokat.

## <a id="connect"></a>Csatlakozás toohello Media Services-fiók REST API-hoz

Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.

Például ha az tooconnect próbálja, után kapott hello következő:

    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

A következő API-hívások toohttps://wamsbayclus001rest-hs.cloudapp.net/api/ tegye meg.

## <a id="upload"></a>Hozzon létre egy új eszközt, és a REST API-t egy videofájl feltöltése

A Media Services szolgáltatásban a digitális fájlok feltöltése egy adategységbe történik. Hello **eszköz** entitás tartalmazhat videót, hang, képek, miniatűröket, szöveg nyomon követi és feliratfájlokat fájlokat (és mindezen fájlok metaadatait hello.)  Miután hello objektumba hello fájlok feltöltése után a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-hello felhő.

Hello értékek tooprovide lehet, ha egy eszköz létrehozása adategység-létrehozási lehetőségek egyikét. Hello **beállítások** tulajdonsága egy számbavételi érték, amely leírja, hogy egy eszköz hozhatók létre hello titkosítási lehetőséget. Érvényes értéket kell az hello listán, nem a lista értékeinek kombinációja hello értékek egyikét:

* **Nincs** = **0** – nincs titkosítás. Ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.
    Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást.
* **StorageEncrypted** = **1** - titkosítja a tiszta tartalom helyileg az AES-256 bites titkosítás használata és tooAzure helyén tárolás titkosítása feltöltését. Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve. a tárolás titkosítása hello elsődleges használati eset az, amikor toosecure a kiváló minőségű bemeneti médiafájljait erős titkosítással aktívan a lemezen.
* **CommonEncryptionProtected** = **2** -használja ezt a beállítást, ha már titkosítva és védve általános titkosítás vagy a PlayReady DRM által (például Smooth Streaming tartalom védett PlayReady DRM).
* **EnvelopeEncryptionProtected** = **4** – használja ezt a beállítást, ha AES által titkosított HLS. hello fájlokat kell kódolni és Transform Manager használatával titkosított.

### <a name="create-an-asset"></a>Egy eszköz létrehozása
Az eszköz egy olyan tároló, típusok vagy a Media Services, beleértve a videó, hang, képeket, miniatűröket, szöveges nyomon követi és feliratfájlokat fájlok objektumokat. A REST API-t, az eszköz létrehozása a FELADÁS egy vagy több küldenie kell hello kérhetnek tooMedia szolgáltatásokat, és helyezi el az eszköz minden tulajdonságadatokat hello kérés törzsében.

a következő példa azt mutatja meg hogyan hello toocreate eszközként.

**HTTP-kérelem**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45

    {"Name":"BigBuckBunny.mp4", "Options":"0"}


**HTTP-válasz**

Ha sikeres, hello következő adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a>Hozzon létre egy AssetFile
Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entitás egy blob-tárolóban tárolt video- vagy fájlt jelöli. Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz tartalmazhat egy vagy több AssetFiles. hello Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.

Miután a digitális adathordozójának fájl feltöltése a blob-tárolóba, hello az **EGYESÍTÉSE** HTTP kérelem tooupdate hello AssetFile a adatainak media (hello témakör későbbi részében szerint).

**HTTP-kérelem**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-válasz**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-hello-accesspolicy-with-write-permission"></a>Írási engedéllyel rendelkező hello AccessPolicy létrehozása
Fájlok feltöltése a blob-tárolóba, mielőtt házirend jogosultságok tooan eszköz írásra hello hozzáférés beállítása. Ezt követően egy HTTP-kérelem toohello AccessPolicies entitás utáni toodo beállítása. Adjon meg egy DurationInMinutes számot a létrehozása után, vagy egy belső kiszolgálót 500 hibaüzenetet kapja vissza válaszként. A AccessPolicies további információkért lásd: [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

a következő példa azt mutatja meg hogyan hello toocreate egy AccessPolicy:

**HTTP-kérelem**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"}

**HTTP-válasz**

Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-hello-upload-url"></a>Hello feltöltése URL-cím beszerzése

tooreceive hello tényleges feltöltési URL-cím, egy SAS-kereső létrehozása. Keresők ügyfelek számára, szeretné, hogy egy eszköz tooaccess fájlok hello kezdési ideje és a csatlakozási végpont típusa határozza meg. Létrehozhat több lokátor entitás egy adott AccessPolicy és eszköz pár toohandle különböző ügyfél, kérések és a vonatkozó igényeket. A keresők mindegyike használ, hello StartTime érték plus hello DurationInMinutes értékének hello AccessPolicy toodetermine hello mennyi ideig egy URL-cím használható. További információkért lásd: [lokátor](https://docs.microsoft.com/rest/api/media/operations/locator).

Egy SAS URL-címnek a következő formátumban hello:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Vegye figyelembe a következőket:

* Egy adott eszközhöz társított egyszerre legfeljebb öt egyedi keresők tartalmazhat. További információkért tekintse meg a lokátor.
* Ha tooupload a fájlok azonnal, a StartTime érték toofive perc hello aktuális időpont előtt kell beállítani. Ennek az az oka lehet óra eltérésére az ügyfélszámítógép és a Media Services között. A StartTime érték is, kell lennie a következő dátum és idő formátumban hello: éééé-hh-SSz (például "2014-05-23T17:53:50Z").    
* Előfordulhat, hogy a 30-40 második késleltetés akkor érhető el használatra toowhen egy kereső létrehozása után. A probléma tooboth SAS URL-cím és a forrás-keresők vonatkozik.

További információ a SAS keresők: [ez](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.

hello a következő példa bemutatja, hogyan toocreate SAS URL-cím lokátor, által definiált hello hello kérés törzsében ("1" egy SAS-kereső) és egy az Igényalapú származási kereső "2" típusú tulajdonság. Hello **elérési** tulajdonság visszaadott hello URL-címet, hogy fájlt kell használnia tooupload a tartalmaz.

**HTTP-kérelem**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178

    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-válasz**

Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Egy blob storage tárolóba-fájl feltöltése
Miután hello AccessPolicy és a lokátor-készlet, a fájl tényleges hello feltöltött tooan Azure blob storage tárolót hello Azure Storage REST API-k használatával. Hello fájlok blokkblobként kell feltöltenie. Lapblobokat nem Azure Media Services által támogatott.  

> [!NOTE]
> Hozzá kell adnia a hello fájlnév hello fájlt keresi tooupload toohello lokátor **elérési** hello korábbi szakaszában kapott érték. Például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . .
>
>

Az Azure storage blobs munkavégzés további információkért lásd: [Blob szolgáltatás REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

### <a name="update-hello-assetfile"></a>Frissítés hello AssetFile
Most, hogy a fájl feltöltése hello FileAsset méret (és más) adatainak frissítése. Példa:

    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-válasz**

Ha sikeres, hello következő adja vissza:

    HTTP/1.1 204 No Content
    ...

## <a name="delete-hello-locator-and-accesspolicy"></a>Hello lokátor és AccessPolicy törlése
**HTTP-kérelem**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-válasz**

Ha sikeres, hello következő adja vissza:

    HTTP/1.1 204 No Content
    ...

**HTTP-kérelem**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-válasz**

Ha sikeres, hello következő adja vissza:

    HTTP/1.1 204 No Content
    ...

## <a id="encode"></a>Hello forrásfájl kódolása adaptív sávszélességű MP4-fájlsorozattá készletére

Választásával dolgozhat fel, a Media Services szolgáltatásban adathordozó eszközök kódolható, transmuxed teljesítményjellemzőit, és így tovább, miután előtt biztosítását tooclients. Ezek a tevékenységek ütemezett, és több háttér szerepkör példányok tooensure nagy teljesítményt és rendelkezésre állás futtatni. Ezeket a tevékenységeket feladatoknak nevezzük, és minden feladat áll hello hello objektumfájlt valódi munkát atomi feladatokhoz (további információkért lásd: [feladat](/rest/api/media/services/job), [feladat](/rest/api/media/services/task) leírása).

Mivel korábban már említettük, ha működik az Azure Media Services egyik hello leggyakoribb forgatókönyve az adaptív sávszélességű streamelési tooyour ügyfelek elkötelezett. A Media Services tudja dinamikusan csomagolni adaptív sávszélességű MP4-fájlok készlete hello a következő formátumok egyikét: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.

a következő szakasz hello bemutatja, hogyan toocreate egy feladatot, amely tartalmaz egy kódolási feladat. hello feladat Megadja, hogy tootranscode hello mezzazine-fájlt használatával adaptív sávszélességű MP4 készletére **Media Encoder Standard**. hello szakasz azt is bemutatja, hogyan toomonitor hello feladat feldolgozása folyamatban van. Ha hello feladat befejeződött, lehetővé válik a szükséges tooget hozzáférés tooyour eszközöket képes toocreate lokátorokat.

### <a name="get-a-media-processor"></a>Egy media processzor beolvasása
A Media Services szolgáltatásban a media processzor összetevő, amely egy meghatározott feldolgozási feladatot, például a kódolása, titkosítása vagy visszafejtése közben médiatartalmak konverzióval, kezeli. A kódolási feladat ebben az oktatóanyagban szereplő hello fogjuk toouse hello Media Encoder Standard.

a következő kód kérelmek hello kódoló azonosító hello.

**HTTP-kérelem**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-válasz**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Feladat létrehozása
Minden egyes feladat egy vagy, hogy a feldolgozási hello típusától függően további feladatok tooaccomplish szeretné. Hello REST API-t, használatával hozhat létre feladatokat és azok kapcsolódó feladatok az alábbi két módszer egyikével: feladatok lehet meghatározott beágyazott hello feladatok navigációs tulajdonság a feladat entitásokat vagy OData kötegfeldolgozási révén. Media Services SDK hello kötegfeldolgozási használja. Hello olvashatóság érdekében hello kódpéldák ebben a témakörben, azonban a feladatok nem definiált beágyazott. Kötegfeldolgozási információkért lásd: [nyitott Data (OData) protokollnak kötegelt feldolgozása](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

hello a következő példa bemutatja, hogyan toocreate és a feladás egy vagy több feladat is egy feladatot állíthatja tooencode videó adott feloldási és minőségi. a következő dokumentáció hello megtalálható hello az összes hello [készletek feladat](http://msdn.microsoft.com/library/mt269960) hello Media Encoder Standard processzor által támogatott.  

**HTTP-kérelem**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"Adaptive Streaming",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-válasz**

Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  

             ]
          }
       }
    }


Van néhány fontos dolgot toonote feladat kérésének:

* TaskBody tulajdonságok literális XML toodefine hello számú bemeneti vagy kimeneti eszközök hello tevékenység által használt kell használnia. hello feladat témakör hello XML Schema definíció hello XML számára.
* Hello TaskBody definícióját, az egyes belső értékének <inputAsset> és <outputAsset> JobInputAsset(value) vagy JobOutputAsset(value) be kell állítani.
* Kimeneti adategységek feladatonként. Egy JobOutputAsset(x) csak egyszer használható egy feladatot a feladatok kimeneteként.
* A tevékenység bemeneti eszközként JobInputAsset vagy JobOutputAsset adhat meg.
* Feladatok kell nem alkotnak hurkot.
* hello érték paraméter átadására tooJobInputAsset vagy JobOutputAsset hello index értékét jelöli az adott eszköz számára. hello tényleges eszközök hello InputMediaAssets és hello entitás feladatdefiníció OutputMediaAssets navigációs tulajdonságainak vannak definiálva.

> [!NOTE]
> A Media Services OData v3 épül, mert egyes eszközöket a InputMediaAssets hello és OutputMediaAssets navigációs tulajdonság gyűjtemények hivatkozott keresztül a "__metadata: uri" név-érték pár.
>
>

* InputMediaAssets tooone vagy a Media Services létrehozott további eszközök képezi le. OutputMediaAssets hello rendszer hozza létre. Egy meglévő eszköz azok többé ne hivatkozzanak.
* OutputMediaAssets elnevezheti hello assetName attribútum használatával. Ha ez az attribútum nincs jelen, akkor hello OutputMediaAsset hello nevét bármilyen hello belső szöveges értékét hello <outputAsset> elem utótaggal hello feladat nevét, és közül hello feladatazonosító érték (hello esetet, ahol hello Name tulajdonság nincs megadva). Például ha egy érték a assetName túl "Sample", majd OutputMediaAsset Name tulajdonság van beállítva hello túl "Sample". Azonban ha nem állított assetName értékét, de adta meg az hello feladat neve túl "NewJob", majd hello OutputMediaAsset neve lesz "JobOutputAsset (érték) _NewJob".

    hello a következő példa bemutatja, hogyan tooset hello assetName attribútum:

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* tooenable feladat láncolás:

  * Egy feladat legalább két tevékenységet kell rendelkeznie.
  * Legalább egy tevékenységet, amelynek a bemeneti érték egy másik feladat hello feladat kimenetének kell lennie.

További információ: [egy kódolási feladat létrehozása a Media Services REST API hello](media-services-rest-encode-asset.md).

### <a name="monitor-processing-progress"></a>A figyelő feldolgozása folyamatban van
Hello-State tulajdonsága, használatával lekérhető hello feladat állapotát, ahogy az alábbi példa hello.

**HTTP-kérelem**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-válasz**

Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT

    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Feladatok megszakítása
A Media Services lehetővé teszi a futó feladatok keresztül hello CancelJob függvény toocancel. Ez a hívás 400 hibakódot ad vissza, ha toocancel egy feladat állapotában visszavonásakor, visszavonás, hiba, vagy befejeződött.

a következő példa azt mutatja meg hogyan hello toocall CancelJob.

**HTTP-kérelem**

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Ha sikeres, nem az üzenet törzse 204 válaszkód is visszaad.

> [!NOTE]
> Hello feladatazonosító URL-kódolni kell (általában nb:jid:UUID: somevalue) történő átadásakor legyen, mint egy paraméterrel tooCancelJob.
>
>

### <a name="get-hello-output-asset"></a>Hello kimeneti eszköz beolvasása
hello következő kód bemutatja, hogyan toorequest hello kimeneti eszköz azonosítója.

**HTTP-kérelem**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-válasz**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Hello objektum közzététele, és a streamelési és a progresszív letöltési URL-címet a REST API-n

toostream vagy egy eszköz letöltése, akkor először kell túl "közzététele" egy kereső létrehozásával. Lokátorok biztosítanak hozzáférést toofiles hello eszköz szerepel. Media Services két lokátortípust támogat: OnDemandOrigin keresők használt toostream media (például MPEG DASH, HLS vagy Smooth Streaming) és a hozzáférési aláírás (SAS) lokátortípus, toodownload médiafájlok használt. További információ a SAS keresők: [ez](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/) blog.

Miután létrehozott egy hello lokátorokat, hozhat létre hello URL-címek, amelyek használt toostream, vagy töltse le a fájlokat.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát.

Smooth Streaming egy adatfolyam-továbbítási URL-címnek a hello a következő formátumban:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Egy HLS streamelési URL-címet a következő formátumban hello rendelkezik:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG DASH-továbbítási egy URL-formátum a következő hello rendelkezik:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


A használt SAS URL-cím toodownload fájlok hello formátuma a következő esetében:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Ez a szakasz bemutatja, hogyan tooperform hello következő feladatok szükséges túl "közzététele" az eszközök.  

* Olvasási engedéllyel rendelkező hello AccessPolicy létrehozása
* Letölti a tartalmat egy SAS URL-cím létrehozása
* Hozzon létre egy forrás URL-CÍMÉT egy adatfolyam-tartalom

### <a name="creating-hello-accesspolicy-with-read-permission"></a>Olvasási engedéllyel rendelkező hello AccessPolicy létrehozása
Letöltése, illetve bármely médiatartalom streaming előtt először egy AccessPolicy olvasási engedély megadása, és hozzon létre hello megfelelő lokátor entitás hello típust megadó kézbesítési mechanizmus az ügyfelek számára a kívánt tooenable. Hello tulajdonságok érhetők el a további információkért lásd: [AccessPolicy Entitástulajdonságok](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).

hello a következő példa bemutatja, hogyan toospecify hello AccessPolicy olvasási engedéllyel az adott eszköz.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Ha sikeres, a 201 sikeres vissza hello AccessPolicy entitás létrehozott leíró. Ezt követően az hello AccessPolicy azonosító együtt hello hello eszköz hello fájlt (például egy kimeneti eszköz) toodeliver toocreate hello lokátor entitást tartalmazó objektum azonosítója.

> [!NOTE]
> Ez a munkafolyamat alapvető van hello ugyanaz, mint a fájlt feltölteni, amikor egy eszköz választásával dolgozhat fel, (a korábban a jelen témakörben bemutatott volt). Is például fájlok feltöltése, ha Ön (vagy az ügyfelek) tooaccess kell a fájlok azonnal, állítsa be a StartTime érték toofive perc hello aktuális ideje előtt. Ez a művelet szükség, mert előfordulhatnak óra eltérésére hello ügyfél és a Media Services között. hello StartTime érték hello a következő dátum és idő formátumban kell megadni: éééé-hh-SSz (például "2014-05-23T17:53:50Z").
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a>Letölti a tartalmat egy SAS URL-cím létrehozása
hello a következő kód bemutatja, hogyan tooget lehet használt toodownload egy médiafájlt URL-Címeket létrehozni és korábban feltöltött. hello AccessPolicy engedélycsoport rendelkezik-e olvasási és hello lokátor elérési út tooa SAS letöltési URL-CÍMRE hivatkozik.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


visszaadott hello **elérési** tulajdonság hello SAS URL-címet tartalmazza.

> [!NOTE]
> Tárolási titkosított tartalom letöltéséhez esetén kell manuálisan előtt azt visszafejteni, vagy a hello tárolási visszafejtési MediaProcessor egy feldolgozási feladat toooutput a feldolgozott hello egyértelmű tooan OutputAsset fájlokat, és töltse le az adott eszközre. A feldolgozási további információkért lásd: hello Media Services REST API-t egy kódolási feladat létrehozásakor. SAS URL-cím keresők nem lehet frissíteni, azok létrehozását követően. Például nem használhat újra hello frissített StartTime értékkel azonos Lokátort. Ez az SAS URL-címek jönnek létre hello módon miatt. Ha egy eszköz tooaccess letölthető egy kereső lejárta után, akkor egy új StartTime egy újat kell létrehoznia.
>
>

### <a name="download-files"></a>Fájlok letöltése
Miután hello AccessPolicy és a lokátor-készlet, letöltheti a fájlokat a hello Azure Storage REST API-k.  

> [!NOTE]
> Hozzá kell adnia a hello fájlnév hello fájlt keresi toodownload toohello lokátor **elérési** hello korábbi szakaszában kapott érték. Például https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . .
>
>

Az Azure storage blobs munkavégzés további információkért lásd: [Blob szolgáltatás REST API](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Miatt hello korábban végrehajtott feladat kódolás (kódolása adaptív MP4 állítsa be) hogy több MP4-fájlokat fokozatosan lehet letölteni. Példa:    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Hozzon létre egy adatfolyam-továbbítási URL-címet egy adatfolyam-tartalom
a következő kód bemutatja hogyan hello toocreate URL-címet a streamelési Lokátorok létrehozásához:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Ha sikeres, a következő válasz hello adja vissza:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

egy Smooth Streaming forrás URL-címet egy adatfolyam-továbbítási media Player toostream, hozzá kell fűzni a hello Smooth Streaming manifest fájl "/ manifest" nevű hello hello Path tulajdonságot.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS, toostream hozzáfűzése (formátum = m3u8-aapl) után hello "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

toostream MPEG DASH, hozzáfűző (formátum mpd-idő-csf =) után hello "/ manifest".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Tartalom lejátszása
toostream videó, használhat [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

tootest progresszív letöltés, illessze be egy URL-címet a böngészőjébe (például az Internet Explorer, az Chrome, a Safari).

## <a name="next-steps-media-services-learning-paths"></a>Következő lépések: Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

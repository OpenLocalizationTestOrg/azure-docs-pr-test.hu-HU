---
title: "aaaEncrypting a tartalmaknak a tárolás titkosítása AMS REST API használatával"
description: "Megtudhatja, hogyan tooencrypt a tartalmaknak a tárolás titkosítása AMS REST API-k használatával."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: d5f8cb8dd1dcded76c9fededccc772d8102ccbad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a>A tárolás titkosítása tartalom titkosítása

Lehetőleg tooencrypt a tartalomhoz helyileg az AES-256 algoritmussal titkosítási bit, és azt a rendszer hol tárolja tárolás titkosítása tooAzure feltöltése.

Ez a cikk áttekintést AMS tárolás titkosítása, és bemutatja, hogyan tooupload hello tárolási titkosított tartalom:

* Hozzon létre egy tartalomkulcsot.
* Hozzon létre egy eszközt. Állítsa be a hello AssetCreationOption tooStorageEncryption hello eszköz létrehozásakor.
  
     Titkosított eszközök kapcsolódó tartalom toobe rendelkezik.
* Hivatkozás hello tartalom kulcs toohello eszköz.  
* Állítsa be hello titkosítási kapcsolatos paramétereket az hello AssetFile entitásokra.

## <a name="considerations"></a>Megfontolandó szempontok 

Ha azt szeretné, hogy a tároló titkosított eszköz toodeliver, konfigurálnia kell a hello adategység továbbítási házirendjét. Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét. További információkért lásd: [konfigurálása az adategység továbbítási házirendjeit](media-services-rest-configure-asset-delivery-policy.md).

A Media Services entitások elérésekor be kell meghatározott fejlécmezők és értékek a HTTP-kérelmekre. További információkért lásd: [a Media Services REST API fejlesztési telepítő](media-services-rest-how-to-use.md). 

## <a name="connect-toomedia-services"></a>Connect tooMedia szolgáltatások

Hogyan tooconnect toohello AMS API-ról: kapcsolatos [hozzáférés hello Azure Media Services API az Azure AD-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md). 

>[!NOTE]
>Toohttps://media.windows.net sikeres csatlakozás után kapni fog egy másik Media Services URI megadása 301 átirányítást. Meg kell nyitnia a további hívások toohello új URI.

## <a name="storage-encryption-overview"></a>Tárolás titkosítási – áttekintés
hello AMS tárolás titkosítása vonatkozik **AES-Parancsra** mód titkosítási toohello teljes fájlt.  AES-Parancsra módja a blokktitkosításon, amelyek titkosíthatják a tetszőleges hosszúságú adatok kitöltési szükségessége nélkül. Egy számláló blokkot, amelynek hello AES algoritmus és XOR-ing hello kimenet az AES hello adatok tooencrypt titkosításával működik, illetve visszafejteni.  hello számláló blokk használt összeállított hello InitializationVector toobytes 0 too7 hello számláló értékének hello értékének másolásával és a 8 bájtos too15 hello számláló értékének toozero. Hello 16 bájtos számláló blokk, 8 bájtos too15 (azaz hello legkisebb helyiértékű bájt) kell használni, mint egy egyszerű 64 bites előjel nélküli egész szám, amely minden ezt követő adatblokk adatfeldolgozási eggyel növekszik, és van hálózati tartani. Figyelje meg, hogy ha az egész hello maximális értékének elérésekor (0xFFFFFFFFFFFFFFFF) majd növekvő hello blokk számláló toozero (8 bájtos too15) visszaállítja az nem befolyásolja a többi hello hello számláló (vagyis 0 bájt too7) 64 bites.   Rendelés toomaintain hello biztonsági hello AES-Parancsra mód titkosítási, hello InitializationVector érték egy adott kulcsazonosító minden tartalomkulcsot az egyes fájlok egyedinek kell lennie, és fájlok legfeljebb 2 ^ 64 blokkok hossza.  Ez a tooensure, hogy a számláló értéke soha nem használja fel újra egy adott kulccsal. Hello Parancsra móddal kapcsolatos további információkért lásd: [a wiki lapon](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (hello wikicikket használ hello kifejezés "Nonce" helyett "InitializationVector").

Használjon **tárolás titkosítása** tooencrypt a tiszta tartalom helyileg az AES-256 algoritmussal bit a titkosítási, majd töltse fel tooAzure helyén tárolás titkosítása. Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve. hello elsődleges használati eset a tárolás titkosítása akkor, ha azt szeretné, hogy a jó minőségű bemeneti médiafájljait erős titkosítással, rest-lemezen toosecure.

A sorrend toodeliver tárolási titkosított eszköz konfigurálnia kell hello adategység továbbítási házirendjét, a Media Services tudja, hogyan szeretné toodeliver a tartalom. Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét (például AES, általános titkosítás vagy titkosítás nélkül).

## <a name="create-contentkeys-used-for-encryption"></a>A titkosításhoz használt ContentKeys létrehozása
Titkosított eszközök tárolás titkosítása kulcshoz tartozó toobe rendelkezik. Hello tartalom kulcs toobe titkosítja az eszköz fájlok hello létrehozása előtt létre kell hoznia. Ez a szakasz ismerteti, hogyan toocreate egy tartalomkulcsot.

hello az alábbiakban általános lépésekkel végezhető tartalomkulcs fog társítani eszközök titkosított toobe generálásához. 

1. A tárolás titkosítása véletlenszerűen 32 bájtos AES kulcs létrehozása. 
   
    Ez az objektum, amely azt jelenti, hogy a hozzárendelt összes fájl hello tartalomkulcsot lesz az adott eszközre fog kell toouse hello azonos tartalomkulcsot a visszafejtés során. 
2. Hello hívás [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) és [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) módszerek tooget hello kell lennie a használt tooencrypt megfelelő X.509-tanúsítvány a tartalomkulcsot.
3. Hello nyilvános kulcsával hello X.509 tanúsítvány a tartalom kulcs titkosításához. 
   
   Media Services .NET SDK RSA végrehajtásakor hello titkosítási OAEP típusú használ.  Láthatja, hogy a .NET típusú példát a hello [EncryptSymmetricKeyData függvény](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. Hozzon létre egy ellenőrzőösszeget hello kulcsazonosító és tartalomkulcsot számítja. .NET típusú példát a következő hello kiszámítja a hello kulcsazonosító hello GUID része hello ellenőrzőösszeg és hello törölje tartalomkulcsot.

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting hello KID
            // with hello content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }

1. Hozzon létre hello tartalomkulcsot hello **EncryptedContentKey** (átalakítás toobase64 kódolású karakterlánc) **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, és **ellenőrzőösszeg** értékeket az előző lépésben kapott.

    A tárolás titkosítása hello következő tulajdonságok tartozhatnak hello kérés törzsében.

    Kérelem törzse tulajdonság    | Leírás
    ---|---
    Azonosító | hello ContentKey azonosítója, amely azt készítése ragozott formáival használatával a következő hello formázásához "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Ez az hello kulcs tartalomtípus a tartalom kulcs egész szám lehet. Azt adja át a tárolás titkosítása hello értéke 1.
    EncryptedContentKey | Létrehozhatunk egy új tartalom kulcs értéke pedig 256 bites (32 bájt) értéket. hello kulcs Titkosított hello tárolási titkosítási X.509 tanúsítvány, amely által egy HTTP GET kérelem hello GetProtectionKeyId és GetProtectionKey metódusok végrehajtása nem beolvasni a Microsoft Azure Media Services használatával. Tegyük fel, tekintse meg a következő kód .NET hello: hello **EncryptSymmetricKeyData** definiált metódus [Itt](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Ez az hello védelmi hello storage encryption X.509 tanúsítvány, de a használt tooencrypt azonosítója a tartalomkulcsot.
    ProtectionKeyType | Ez az hello titkosítási típus hello védelmi kulcs, de a használt tooencrypt hello tartalomkulcsot. Ez az érték a fenti példában StorageEncryption(1).
    Ellenőrzőösszeg |hello MD5 számított ellenőrzőösszege hello tartalomkulcsot. Hello tartalmat azonosító hello tartalomkulcsot titkosításával számítja ki. hello példakód bemutatja, hogyan toocalculate hello ellenőrzőösszeg.


### <a name="retrieve-hello-protectionkeyid"></a>Hello ProtectionKeyId beolvasása
hello a következő példa bemutatja, hogyan tooretrieve hello ProtectionKeyId, egy tanúsítvány-ujjlenyomat, hello tanúsítványt kell használni a tartalom kulcs titkosításához. Hajtsa végre a lépés toomake meg arról, hogy már rendelkezik a megfelelő tanúsítvánnyal hello a számítógépen.

A kérelem:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net

Válasz:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

### <a name="retrieve-hello-protectionkey-for-hello-protectionkeyid"></a>A hello ProtectionKeyId ProtectionKey hello beolvasása
hello következő példa bemutatja, hogyan tooretrieve hello X.509-tanúsítvány hello ProtectionKeyId kapott hello előző lépésben.

A kérelem:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

Válasz:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-hello-content-key"></a>Hello tartalomkulcs létrehozása
Hello X.509 tanúsítvány lekérése és használja a nyilvános kulcs tooencrypt a tartalomkulcsot, akkor létre kell hoznia egy **ContentKey** entitás és a tulajdonság értékek ennek megfelelően beállítva.

Hello tartalom hello típus kulcs létrehozása, hogy kell-e állítva mikor hello értékek egyike. Hello tárolás titkosítását, ha a hello értéke "1". 

a következő példa azt mutatja meg hogyan hello toocreate egy **ContentKey** rendelkező egy **ContentKeyType** beállítása a tárolás titkosítása ("1") és hello **ProtectionKeyType** beállítása túl "0" amely védelmi kulcs azonosítója hello tooindicate hello X.509 tanúsítvány ujjlenyomata.  

Kérés

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }

Válasz:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Egy eszköz létrehozása
a következő példa azt mutatja meg hogyan hello toocreate eszközként.

**HTTP-kérelem**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

**HTTP-válasz**

Ha sikeres, hello következő adja vissza:

    HTP/1.1 201 Created
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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

## <a name="associate-hello-contentkey-with-an-asset"></a>Egy eszköz hello ContentKey társítása
Miután létrehozta a hello ContentKey, társíthatja hello $links művelettel, az eszköz a hello a következő példában látható módon:

A kérelem:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Válasz:

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a>Hozzon létre egy AssetFile
Hello [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entitás egy blob-tárolóban tárolt video- vagy fájlt jelöli. Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz egy vagy több eszköz fájlt tartalmaz. hello Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.

Vegye figyelembe, hogy hello **AssetFile** példány és a hello tényleges media fájl is két különböző objektum. hello AssetFile példány hello media fájlról metaadatot tartalmaz, amíg hello médiafájl tartalmaz hello tényleges médiatartalmakat.

Miután a digitális adathordozójának fájl feltöltése a blob-tárolóba, szüksége lesz a hello **EGYESÍTÉSE** HTTP kérelem tooupdate hello AssetFile a adatainak media (ebben a témakörben nem látható). 

**HTTP-kérelem**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }

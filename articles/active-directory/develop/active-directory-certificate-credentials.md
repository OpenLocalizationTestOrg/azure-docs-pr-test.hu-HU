---
title: "aaaCertificate hitelesítő adatokat az Azure AD |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek hello regisztrálása és az alkalmazás-hitelesítési tanúsítvány hitelesítő adatok használatát"
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 3508803112ac06268d553db86ab74812aa53e455
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-credentials-for-application-authentication"></a>Alkalmazás-hitelesítési tanúsítvány hitelesítő adatai

Az Azure Active Directory lehetővé teszi, hogy egy alkalmazás toouse a saját hitelesítő adatok, például az OAuth 2.0 ügyfél hitelesítő adatok Grant flow hello és hello a-meghatalmazásos folyamat.
A hitelesítő adatok, használhat egy formátuma egy JSON webes Token(JWT) helyességi feltétel hello alkalmazás birtokló a tanúsítvánnyal aláírt.

## <a name="format-of-hello-assertion"></a>Hello helyességi feltétel formátuma
toocompute hello helyességi feltétel, érdemes toouse hello közül sok [JSON Web Token](https://jwt.io/) hello nyelvű tárak. hello információk hello token végzi a következő:

#### <a name="header"></a>Fejléc

| Paraméter |  Megjegyzés |
| --- | --- | --- |
| `alg` | Meg kell **RS256** |
| `typ` | Meg kell **jwt-t** |
| `x5t` | Hello X.509 tanúsítvány SHA-1 ujjlenyomat kell lennie. |

#### <a name="claims-payload"></a>Jogcímek (tartalom)

| Paraméter |  Megjegyzés |
| --- | --- | --- |
| `aud` | A célközönség: Kell  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token ** |
| `exp` | Lejárat dátuma: hello jogkivonat lejáratának hello dátum. hello idő egy jelenik meg azon másodpercek számát hello 1970. január 1. a (1970-01-01T0:0:0Z) UTC szerinti eléréséig hello idő hello token érvényességét.|
| `iss` | Kibocsátó: hello client_id (hello ügyfélszolgáltatás alkalmazásazonosító) kell lennie. |
| `jti` | GUID-ja: hello JWT azonosítója |
| `nbf` | Hatálybalépési idő: hello dátuma előtt mely hello token nem használható. hello idő egy jelenik meg azon másodpercek számát hello 1970. január 1. a (1970-01-01T0:0:0Z) UTC amíg hello idő hello token lett kiállítva. |
| `sub` | Tulajdonos: megegyezik a `iss`, hello client_id (hello ügyfélszolgáltatás alkalmazásazonosító) kell lennie. |

#### <a name="signature"></a>Aláírás
hello aláírás számított hello tanúsítvány alkalmazása a hello [JSON Web Token RFC7519 meghatározása](https://tools.ietf.org/html/rfc7519)

### <a name="example-of-a-decoded-jwt-assertion"></a>A dekódolt JWT helyességi feltétel – példa
```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
}
.
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

### <a name="example-of-an-encoded-jwt-assertion"></a>Egy kódolt JWT helyességi feltétel – példa
a következő karakterlánc hello kódolt helyességi feltétel példája. Gondosan tekintse meg a három pont (.) választja el szakasz figyelje meg.
első szakasz hello kódolja hello fejléc, hello második hello hasznos, és hello utolsó hello első két szakaszok tartalmának hello hello tanúsítványokat hello aláírással.
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a>A tanúsítvány regisztrálása az Azure AD-val
tooassociate hello tanúsítvány hitelesítő adatai az ügyfélalkalmazás hello Azure AD-ben, meg kell tooedit hello alkalmazás jegyzékében.
Tanúsítvány céllal rendelkezik, toocompute kell:
- `$base64Thumbprint`, amely hello, base64 kódolást hello tanúsítvány kivonata
- `$base64Value`, amely hello, hello tanúsítványi nyers adatok base64 kódolása

Emellett szükség van egy GUID tooidentify hello kulcs hello alkalmazásjegyzékben tooprovide (`$keyId`)

Hello Azure-alkalmazás regisztrációja hello ügyfélalkalmazáshoz, nyissa meg az alkalmazásjegyzék hello, és cserélje le a hello *keyCredentials* tulajdonságot, amelyben az új tanúsítvány adatait a következő séma hello:
```
"keyCredentials": [
    {
        "customKeyIdentifier": "$base64Thumbprint",
        "keyId": "$keyid",
        "type": "AsymmetricX509Cert",
        "usage": "Verify",
        "value":  "$base64Value"
    }
]
```

Hello módosításokat toohello alkalmazásjegyzék mentéséhez, és töltse fel a tooAzure AD. hello keyCredentials tulajdonság többértékű, így előfordulhat, hogy több kulcskezelési több tanúsítványok feltöltése.

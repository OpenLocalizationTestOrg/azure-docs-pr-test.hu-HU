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
# <a name="certificate-credentials-for-application-authentication"></a><span data-ttu-id="68e54-103">Alkalmazás-hitelesítési tanúsítvány hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="68e54-103">Certificate credentials for application authentication</span></span>

<span data-ttu-id="68e54-104">Az Azure Active Directory lehetővé teszi, hogy egy alkalmazás toouse a saját hitelesítő adatok, például az OAuth 2.0 ügyfél hitelesítő adatok Grant flow hello és hello a-meghatalmazásos folyamat.</span><span class="sxs-lookup"><span data-stu-id="68e54-104">Azure Active Directory allows an application toouse its own credentials for authentication, for example, in hello OAuth 2.0 Client Credentials Grant flow and hello On-Behalf-Of flow.</span></span>
<span data-ttu-id="68e54-105">A hitelesítő adatok, használhat egy formátuma egy JSON webes Token(JWT) helyességi feltétel hello alkalmazás birtokló a tanúsítvánnyal aláírt.</span><span class="sxs-lookup"><span data-stu-id="68e54-105">One form of credential that can be used is a JSON Web Token(JWT) assertion signed with a certificate that hello application owns.</span></span>

## <a name="format-of-hello-assertion"></a><span data-ttu-id="68e54-106">Hello helyességi feltétel formátuma</span><span class="sxs-lookup"><span data-stu-id="68e54-106">Format of hello assertion</span></span>
<span data-ttu-id="68e54-107">toocompute hello helyességi feltétel, érdemes toouse hello közül sok [JSON Web Token](https://jwt.io/) hello nyelvű tárak.</span><span class="sxs-lookup"><span data-stu-id="68e54-107">toocompute hello assertion, you probably want toouse one of hello many [JSON Web Token](https://jwt.io/) libraries in hello language of your choice.</span></span> <span data-ttu-id="68e54-108">hello információk hello token végzi a következő:</span><span class="sxs-lookup"><span data-stu-id="68e54-108">hello information carried by hello token is:</span></span>

#### <a name="header"></a><span data-ttu-id="68e54-109">Fejléc</span><span class="sxs-lookup"><span data-stu-id="68e54-109">Header</span></span>

| <span data-ttu-id="68e54-110">Paraméter</span><span class="sxs-lookup"><span data-stu-id="68e54-110">Parameter</span></span> |  <span data-ttu-id="68e54-111">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="68e54-111">Remark</span></span> |
| --- | --- | --- |
| `alg` | <span data-ttu-id="68e54-112">Meg kell **RS256**</span><span class="sxs-lookup"><span data-stu-id="68e54-112">Should be **RS256**</span></span> |
| `typ` | <span data-ttu-id="68e54-113">Meg kell **jwt-t**</span><span class="sxs-lookup"><span data-stu-id="68e54-113">Should be **JWT**</span></span> |
| `x5t` | <span data-ttu-id="68e54-114">Hello X.509 tanúsítvány SHA-1 ujjlenyomat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="68e54-114">Should be hello X.509 Certificate SHA-1 thumbprint</span></span> |

#### <a name="claims-payload"></a><span data-ttu-id="68e54-115">Jogcímek (tartalom)</span><span class="sxs-lookup"><span data-stu-id="68e54-115">Claims (Payload)</span></span>

| <span data-ttu-id="68e54-116">Paraméter</span><span class="sxs-lookup"><span data-stu-id="68e54-116">Parameter</span></span> |  <span data-ttu-id="68e54-117">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="68e54-117">Remark</span></span> |
| --- | --- | --- |
| `aud` | <span data-ttu-id="68e54-118">A célközönség: Kell  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token **</span><span class="sxs-lookup"><span data-stu-id="68e54-118">Audience: Should be **https://login.microsoftonline.com/*tenant_Id*/oauth2/token**</span></span> |
| `exp` | <span data-ttu-id="68e54-119">Lejárat dátuma: hello jogkivonat lejáratának hello dátum.</span><span class="sxs-lookup"><span data-stu-id="68e54-119">Expiration date: hello date when hello token expires.</span></span> <span data-ttu-id="68e54-120">hello idő egy jelenik meg azon másodpercek számát hello 1970. január 1. a (1970-01-01T0:0:0Z) UTC szerinti eléréséig hello idő hello token érvényességét.</span><span class="sxs-lookup"><span data-stu-id="68e54-120">hello time is represented as hello number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until hello time hello token validity expires.</span></span>|
| `iss` | <span data-ttu-id="68e54-121">Kibocsátó: hello client_id (hello ügyfélszolgáltatás alkalmazásazonosító) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="68e54-121">Issuer: should be hello client_id (Application Id of hello client service)</span></span> |
| `jti` | <span data-ttu-id="68e54-122">GUID-ja: hello JWT azonosítója</span><span class="sxs-lookup"><span data-stu-id="68e54-122">GUID: hello JWT ID</span></span> |
| `nbf` | <span data-ttu-id="68e54-123">Hatálybalépési idő: hello dátuma előtt mely hello token nem használható.</span><span class="sxs-lookup"><span data-stu-id="68e54-123">Not Before: hello date before which hello token cannot be used.</span></span> <span data-ttu-id="68e54-124">hello idő egy jelenik meg azon másodpercek számát hello 1970. január 1. a (1970-01-01T0:0:0Z) UTC amíg hello idő hello token lett kiállítva.</span><span class="sxs-lookup"><span data-stu-id="68e54-124">hello time is represented as hello number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until hello time hello token was issued.</span></span> |
| `sub` | <span data-ttu-id="68e54-125">Tulajdonos: megegyezik a `iss`, hello client_id (hello ügyfélszolgáltatás alkalmazásazonosító) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="68e54-125">Subject: As for `iss`, should be hello client_id (Application Id of hello client service)</span></span> |

#### <a name="signature"></a><span data-ttu-id="68e54-126">Aláírás</span><span class="sxs-lookup"><span data-stu-id="68e54-126">Signature</span></span>
<span data-ttu-id="68e54-127">hello aláírás számított hello tanúsítvány alkalmazása a hello [JSON Web Token RFC7519 meghatározása](https://tools.ietf.org/html/rfc7519)</span><span class="sxs-lookup"><span data-stu-id="68e54-127">hello signature is computed applying hello certificate as described in hello [JSON Web Token RFC7519 specification](https://tools.ietf.org/html/rfc7519)</span></span>

### <a name="example-of-a-decoded-jwt-assertion"></a><span data-ttu-id="68e54-128">A dekódolt JWT helyességi feltétel – példa</span><span class="sxs-lookup"><span data-stu-id="68e54-128">Example of a decoded JWT assertion</span></span>
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

### <a name="example-of-an-encoded-jwt-assertion"></a><span data-ttu-id="68e54-129">Egy kódolt JWT helyességi feltétel – példa</span><span class="sxs-lookup"><span data-stu-id="68e54-129">Example of an encoded JWT assertion</span></span>
<span data-ttu-id="68e54-130">a következő karakterlánc hello kódolt helyességi feltétel példája.</span><span class="sxs-lookup"><span data-stu-id="68e54-130">hello following string is an example of encoded assertion.</span></span> <span data-ttu-id="68e54-131">Gondosan tekintse meg a három pont (.) választja el szakasz figyelje meg.</span><span class="sxs-lookup"><span data-stu-id="68e54-131">If you look carefully, you notice three sections separated by dots (.).</span></span>
<span data-ttu-id="68e54-132">első szakasz hello kódolja hello fejléc, hello második hello hasznos, és hello utolsó hello első két szakaszok tartalmának hello hello tanúsítványokat hello aláírással.</span><span class="sxs-lookup"><span data-stu-id="68e54-132">hello first section encodes hello header, hello second hello payload, and hello last is hello signature computed with hello certificates from hello content of hello first two sections.</span></span>
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a><span data-ttu-id="68e54-133">A tanúsítvány regisztrálása az Azure AD-val</span><span class="sxs-lookup"><span data-stu-id="68e54-133">Register your certificate with Azure AD</span></span>
<span data-ttu-id="68e54-134">tooassociate hello tanúsítvány hitelesítő adatai az ügyfélalkalmazás hello Azure AD-ben, meg kell tooedit hello alkalmazás jegyzékében.</span><span class="sxs-lookup"><span data-stu-id="68e54-134">tooassociate hello certificate credential with hello client application in Azure AD, you need tooedit hello application manifest.</span></span>
<span data-ttu-id="68e54-135">Tanúsítvány céllal rendelkezik, toocompute kell:</span><span class="sxs-lookup"><span data-stu-id="68e54-135">Having hold of a certificate, you need toocompute:</span></span>
- <span data-ttu-id="68e54-136">`$base64Thumbprint`, amely hello, base64 kódolást hello tanúsítvány kivonata</span><span class="sxs-lookup"><span data-stu-id="68e54-136">`$base64Thumbprint`, which is hello base64 encoding of hello certificate Hash</span></span>
- <span data-ttu-id="68e54-137">`$base64Value`, amely hello, hello tanúsítványi nyers adatok base64 kódolása</span><span class="sxs-lookup"><span data-stu-id="68e54-137">`$base64Value`, which is hello base64 encoding of hello certificate raw data</span></span>

<span data-ttu-id="68e54-138">Emellett szükség van egy GUID tooidentify hello kulcs hello alkalmazásjegyzékben tooprovide (`$keyId`)</span><span class="sxs-lookup"><span data-stu-id="68e54-138">you also need tooprovide a GUID tooidentify hello key in hello application manifest (`$keyId`)</span></span>

<span data-ttu-id="68e54-139">Hello Azure-alkalmazás regisztrációja hello ügyfélalkalmazáshoz, nyissa meg az alkalmazásjegyzék hello, és cserélje le a hello *keyCredentials* tulajdonságot, amelyben az új tanúsítvány adatait a következő séma hello:</span><span class="sxs-lookup"><span data-stu-id="68e54-139">In hello Azure app registration for hello client application, open hello application manifest, and replace hello *keyCredentials* property with your new certificate information using hello following schema:</span></span>
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

<span data-ttu-id="68e54-140">Hello módosításokat toohello alkalmazásjegyzék mentéséhez, és töltse fel a tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="68e54-140">Save hello edits toohello application manifest, and upload tooAzure AD.</span></span> <span data-ttu-id="68e54-141">hello keyCredentials tulajdonság többértékű, így előfordulhat, hogy több kulcskezelési több tanúsítványok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="68e54-141">hello keyCredentials property is multi-valued, so you may upload multiple certificates for richer key management.</span></span>

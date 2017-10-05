---
title: "Az Azure AD-hitelesítő tanúsítványt |} Microsoft Docs"
description: "Ez a cikk ismerteti a regisztrációs és az alkalmazás-hitelesítési tanúsítvány hitelesítő adatok használatát"
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
ms.openlocfilehash: 08bb5140bb35bbd120aaa506afeab8ad247f81e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="certificate-credentials-for-application-authentication"></a><span data-ttu-id="6b69e-103">Alkalmazás-hitelesítési tanúsítvány hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="6b69e-103">Certificate credentials for application authentication</span></span>

<span data-ttu-id="6b69e-104">Az Azure Active Directory lehetővé teszi, hogy az alkalmazás saját hitelesítő adatait a hitelesítéshez, például az OAuth 2.0 ügyfél hitelesítő adatok Grant flow és az On-meghatalmazásos folyamat.</span><span class="sxs-lookup"><span data-stu-id="6b69e-104">Azure Active Directory allows an application to use its own credentials for authentication, for example, in the OAuth 2.0 Client Credentials Grant flow and the On-Behalf-Of flow.</span></span>
<span data-ttu-id="6b69e-105">A hitelesítő adatok, használhat egy formátuma egy JSON webes Token(JWT) helyességi feltételt, amely az alkalmazás tulajdonosa a tanúsítvánnyal aláírt.</span><span class="sxs-lookup"><span data-stu-id="6b69e-105">One form of credential that can be used is a JSON Web Token(JWT) assertion signed with a certificate that the application owns.</span></span>

## <a name="format-of-the-assertion"></a><span data-ttu-id="6b69e-106">A helyességi feltétel formátuma</span><span class="sxs-lookup"><span data-stu-id="6b69e-106">Format of the assertion</span></span>
<span data-ttu-id="6b69e-107">A helyességi feltétel kiszámításához, érdemes lehet a több valamelyikével [JSON Web Token](https://jwt.io/) szalagtárak a választott nyelven történik.</span><span class="sxs-lookup"><span data-stu-id="6b69e-107">To compute the assertion, you probably want to use one of the many [JSON Web Token](https://jwt.io/) libraries in the language of your choice.</span></span> <span data-ttu-id="6b69e-108">A token által az információkat a következő:</span><span class="sxs-lookup"><span data-stu-id="6b69e-108">The information carried by the token is:</span></span>

#### <a name="header"></a><span data-ttu-id="6b69e-109">Fejléc</span><span class="sxs-lookup"><span data-stu-id="6b69e-109">Header</span></span>

| <span data-ttu-id="6b69e-110">Paraméter</span><span class="sxs-lookup"><span data-stu-id="6b69e-110">Parameter</span></span> |  <span data-ttu-id="6b69e-111">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="6b69e-111">Remark</span></span> |
| --- | --- | --- |
| `alg` | <span data-ttu-id="6b69e-112">Meg kell **RS256**</span><span class="sxs-lookup"><span data-stu-id="6b69e-112">Should be **RS256**</span></span> |
| `typ` | <span data-ttu-id="6b69e-113">Meg kell **jwt-t**</span><span class="sxs-lookup"><span data-stu-id="6b69e-113">Should be **JWT**</span></span> |
| `x5t` | <span data-ttu-id="6b69e-114">Az X.509 tanúsítvány SHA-1 ujjlenyomat kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6b69e-114">Should be the X.509 Certificate SHA-1 thumbprint</span></span> |

#### <a name="claims-payload"></a><span data-ttu-id="6b69e-115">Jogcímek (tartalom)</span><span class="sxs-lookup"><span data-stu-id="6b69e-115">Claims (Payload)</span></span>

| <span data-ttu-id="6b69e-116">Paraméter</span><span class="sxs-lookup"><span data-stu-id="6b69e-116">Parameter</span></span> |  <span data-ttu-id="6b69e-117">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="6b69e-117">Remark</span></span> |
| --- | --- | --- |
| `aud` | <span data-ttu-id="6b69e-118">A célközönség: Kell  **https://login.microsoftonline.com/*tenant_Id*  /oauth2/token **</span><span class="sxs-lookup"><span data-stu-id="6b69e-118">Audience: Should be **https://login.microsoftonline.com/*tenant_Id*/oauth2/token**</span></span> |
| `exp` | <span data-ttu-id="6b69e-119">Lejárat dátuma: a dátum, amikor a jogkivonat érvényessége lejár.</span><span class="sxs-lookup"><span data-stu-id="6b69e-119">Expiration date: the date when the token expires.</span></span> <span data-ttu-id="6b69e-120">Az idő másodpercben 1970. január 1. a ki (1970-01-01T0:0:0Z) UTC, amíg a token érvényességi lejárati idejének.</span><span class="sxs-lookup"><span data-stu-id="6b69e-120">The time is represented as the number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until the time the token validity expires.</span></span>|
| `iss` | <span data-ttu-id="6b69e-121">Kibocsátó: a client_id (az ügyfélszolgáltatás alkalmazásazonosító) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6b69e-121">Issuer: should be the client_id (Application Id of the client service)</span></span> |
| `jti` | <span data-ttu-id="6b69e-122">GUID-ja: a JWT azonosító</span><span class="sxs-lookup"><span data-stu-id="6b69e-122">GUID: the JWT ID</span></span> |
| `nbf` | <span data-ttu-id="6b69e-123">Hatálybalépési idő: az a dátum előtt, amely a token nem használható.</span><span class="sxs-lookup"><span data-stu-id="6b69e-123">Not Before: the date before which the token cannot be used.</span></span> <span data-ttu-id="6b69e-124">Az idő másodpercben 1970. január 1. a ki (1970-01-01T0:0:0Z) UTC, amíg a token lett kiállítva.</span><span class="sxs-lookup"><span data-stu-id="6b69e-124">The time is represented as the number of seconds from January 1, 1970 (1970-01-01T0:0:0Z) UTC until the time the token was issued.</span></span> |
| `sub` | <span data-ttu-id="6b69e-125">Tulajdonos: megegyezik a `iss`, a client_id (az ügyfélszolgáltatás alkalmazásazonosító) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6b69e-125">Subject: As for `iss`, should be the client_id (Application Id of the client service)</span></span> |

#### <a name="signature"></a><span data-ttu-id="6b69e-126">Aláírás</span><span class="sxs-lookup"><span data-stu-id="6b69e-126">Signature</span></span>
<span data-ttu-id="6b69e-127">Az aláírás alkalmazása a tanúsítványt, a számított a [JSON Web Token RFC7519 meghatározása](https://tools.ietf.org/html/rfc7519)</span><span class="sxs-lookup"><span data-stu-id="6b69e-127">The signature is computed applying the certificate as described in the [JSON Web Token RFC7519 specification](https://tools.ietf.org/html/rfc7519)</span></span>

### <a name="example-of-a-decoded-jwt-assertion"></a><span data-ttu-id="6b69e-128">A dekódolt JWT helyességi feltétel – példa</span><span class="sxs-lookup"><span data-stu-id="6b69e-128">Example of a decoded JWT assertion</span></span>
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

### <a name="example-of-an-encoded-jwt-assertion"></a><span data-ttu-id="6b69e-129">Egy kódolt JWT helyességi feltétel – példa</span><span class="sxs-lookup"><span data-stu-id="6b69e-129">Example of an encoded JWT assertion</span></span>
<span data-ttu-id="6b69e-130">A következő karakterlánc kódolt helyességi feltétel példája.</span><span class="sxs-lookup"><span data-stu-id="6b69e-130">The following string is an example of encoded assertion.</span></span> <span data-ttu-id="6b69e-131">Gondosan tekintse meg a három pont (.) választja el szakasz figyelje meg.</span><span class="sxs-lookup"><span data-stu-id="6b69e-131">If you look carefully, you notice three sections separated by dots (.).</span></span>
<span data-ttu-id="6b69e-132">Az első szakaszban kódolja a fejlécet, a második, a tartalom, és az utolsó a tanúsítványokat az első két szakaszok tartalmának aláírással.</span><span class="sxs-lookup"><span data-stu-id="6b69e-132">The first section encodes the header, the second the payload, and the last is the signature computed with the certificates from the content of the first two sections.</span></span>
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a><span data-ttu-id="6b69e-133">A tanúsítvány regisztrálása az Azure AD-val</span><span class="sxs-lookup"><span data-stu-id="6b69e-133">Register your certificate with Azure AD</span></span>
<span data-ttu-id="6b69e-134">A tanúsítvány hitelesítő adat társítandó az ügyfélalkalmazás Azure AD-ben, akkor az alkalmazás jegyzékében módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="6b69e-134">To associate the certificate credential with the client application in Azure AD, you need to edit the application manifest.</span></span>
<span data-ttu-id="6b69e-135">Tanúsítvány céllal rendelkezik, kell számítási:</span><span class="sxs-lookup"><span data-stu-id="6b69e-135">Having hold of a certificate, you need to compute:</span></span>
- <span data-ttu-id="6b69e-136">`$base64Thumbprint`, vagyis a base64 kódolás a tanúsítvány kivonata</span><span class="sxs-lookup"><span data-stu-id="6b69e-136">`$base64Thumbprint`, which is the base64 encoding of the certificate Hash</span></span>
- <span data-ttu-id="6b69e-137">`$base64Value`, vagyis a base64 kódolása nyers Tanúsítványadatok</span><span class="sxs-lookup"><span data-stu-id="6b69e-137">`$base64Value`, which is the base64 encoding of the certificate raw data</span></span>

<span data-ttu-id="6b69e-138">is meg kell adnia a kulcs az alkalmazásjegyzékben szereplő azonosító egy GUID (`$keyId`)</span><span class="sxs-lookup"><span data-stu-id="6b69e-138">you also need to provide a GUID to identify the key in the application manifest (`$keyId`)</span></span>

<span data-ttu-id="6b69e-139">Az ügyfélalkalmazást Azure-alkalmazás regisztrációja, nyissa meg az alkalmazás jegyzékében, és cserélje le a *keyCredentials* tulajdonságot, amelyben az új tanúsítvány adatait a következő séma:</span><span class="sxs-lookup"><span data-stu-id="6b69e-139">In the Azure app registration for the client application, open the application manifest, and replace the *keyCredentials* property with your new certificate information using the following schema:</span></span>
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

<span data-ttu-id="6b69e-140">Mentse a módosításokat az alkalmazás jegyzékében, és töltse fel az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6b69e-140">Save the edits to the application manifest, and upload to Azure AD.</span></span> <span data-ttu-id="6b69e-141">A keyCredentials tulajdonság többértékű, így előfordulhat, hogy több kulcskezelési több tanúsítványok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="6b69e-141">The keyCredentials property is multi-valued, so you may upload multiple certificates for richer key management.</span></span>

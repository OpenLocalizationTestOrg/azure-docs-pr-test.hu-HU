---
title: "aaaAzure AD szolgáltatás tooService Auth OAuth2.0 használatával |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse HTTP-üzenetek tooimplement szolgáltatás hello OAuth2.0 ügyfél hitelesítő adatok grant flow tooservice hitelesítéshez."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f4bfd4ea8a7de1929c7dcf7ad65a156edff74f71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Szolgáltatás tooservice hívásokon ügyfél hitelesítő adatait (közös titkos kulcs vagy tanúsítvány)
OAuth 2.0 ügyfél hitelesítő adatok Grant Flow hello lehetővé teszi egy webszolgáltatás-bővítmény (*bizalmas ügyfél*) toouse webszolgáltatás helyett a felhasználó megszemélyesítésekor, egy másik meghívásakor tooauthenticate saját hitelesítő adatait. Ebben az esetben a hello ügyfél esetében általában egy középső rétegbeli webes szolgáltatás, egy démon szolgáltatás vagy a webhely. A magasabb szintű megbízhatóság az Azure AD is lehetővé teszi, hogy hello hívja service toouse hitelesítő adatot (nem a közös titkos kulcs) tanúsítványok.

## Ügyfél hitelesítő adatai megadják folyamatábrája
hello következő ábra bemutatja, hogyan a hello ügyfél hitelesítő adatai megadják a folyamat működéséről az Azure Active Directory (Azure AD).

![OAuth2.0 ügyfél hitelesítő adatok megadása folyamata](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. hello ügyfélalkalmazás toohello az Azure AD hitelesítési karakterlánc-kiállítási végpont hitelesíti, és egy hozzáférési jogkivonatot kéri.
2. az Azure AD hello jogkivonat kibocsátási végpont problémák hello hozzáférési jogkivonat.
3. hello access token használt tooauthenticate toohello védett erőforrás.
4. Védett erőforrás hello adatait toohello webalkalmazás adja vissza.

## Hello szolgáltatások regisztrálása az Azure ad-ben
Hello hívja service és a szolgáltatás Azure Active Directory (Azure AD) fogadásának hello regisztrálni. Részletes útmutatásért lásd: [alkalmazások integrálása az Azure Active Directory](active-directory-integrating-applications.md).

## A kérelem egy hozzáférési jogkivonatot:
toorequest hozzáférési tokent, egy HTTP POST toohello az Azure AD bérlő-specifikus végpontot használni.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## Szolgáltatás hozzáférési jogkivonat kérelem
Attól függően, hogy úgy dönt, hogy hello ügyfélalkalmazás egy közös titkot, vagy a tanúsítvány által védett toobe két esetben van.

### Először. eset: egy közös titkot a hozzáférési token kérelem
A közös titkos kulcs használata esetén a szolgáltatás hozzáférési kérelmek tartalmazza a következő paraméterek hello:

| Paraméter |  | Leírás |
| --- | --- | --- |
| grant_type |Szükséges |Adja meg a kért hello támogatás típusa. Az ügyfél-hitelesítő adatok Grant flow, hello értéknek kell lennie **client_credentials**. |
| client_id |Szükséges |Adja meg a webes szolgáltatás hívása hello hello Azure AD ügyfél-azonosítója. történt az alkalmazás ügyfél-azonosító, a hello toofind hello [Azure-portálon](https://portal.azure.com), kattintson a **Active Directory**directory váltani, kattintson a hello alkalmazás. hello client_id hello *Alkalmazásazonosító* |
| client_secret |Szükséges |Adjon meg egy hello hívja service vagy démon webalkalmazás az Azure ad-ben regisztrált kulcsot. toocreate egy kulcs, hello Azure-portálon kattintson **Active Directory**, directory váltani, kattintson a hello alkalmazást, majd **beállítások**, kattintson a **kulcsok**, és a kulcs hozzáadása.|
| Erőforrás |Szükséges |Adja meg az fogadó webszolgáltatás hello App ID URI hello. toofind hello App ID URI, az Azure-portálon hello kattintson **Active Directory**, directory váltani, kattintson hello szolgáltatásalkalmazás, majd **beállítások** és **tulajdonságai** |

#### Példa
hello alábbi HTTP POST kérelmek olyan hozzáférési jogkivonatot hello https://service.contoso.com/ webszolgáltatáshoz. Hello `client_id` hello webszolgáltatás hello hozzáférési jogkivonatot kérő azonosítja.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### A második esetben: hozzáférési jogkivonat kérelem tanúsítvánnyal
Szolgáltatás hozzáférési kérelmek tanúsítvánnyal hello a következő paramétereket tartalmazza:

| Paraméter |  | Leírás |
| --- | --- | --- |
| grant_type |Szükséges |Adja meg a kért választípus hello. Az ügyfél-hitelesítő adatok Grant flow, hello értéknek kell lennie **client_credentials**. |
| client_id |Szükséges |Adja meg a webes szolgáltatás hívása hello hello Azure AD ügyfél-azonosítója. történt az alkalmazás ügyfél-azonosító, a hello toofind hello [Azure-portálon](https://portal.azure.com), kattintson a **Active Directory**directory váltani, kattintson a hello alkalmazás. hello client_id hello *Alkalmazásazonosító* |
| client_assertion_type |Szükséges |hello értéknek kell lennie`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Szükséges | Egy helyességi feltétel (egy JSON Web Token), hogy toocreate van szüksége, és a jel hello a tanúsítványok, az alkalmazás hitelesítő adatként regisztrálva. További információ a [tanúsítvány a hitelesítő adatok](active-directory-certificate-credentials.md) toolearn hogyan tooregister hello helyességi feltétel a tanúsítvány és hello formátuma.|
| Erőforrás | Szükséges |Adja meg az fogadó webszolgáltatás hello App ID URI hello. Kattintson a toofind hello App ID URI hello Azure-portálon a **Active Directory**, kattintson hello címtár, kattintson hello alkalmazásra, majd **konfigurálása**. |

Figyelje meg, hogy hello paraméterei szinte hello ugyanaz, mint hello hello kérelem által megosztott titkot azzal a különbséggel, hogy hello client_secret paraméter helyébe két paramétert: egy client_assertion_type és client_assertion.

#### Példa
a következő HTTP POST hello olyan hozzáférési jogkivonatot hello https://service.contoso.com/ webszolgáltatáshoz tanúsítvánnyal kér. Hello `client_id` hello webszolgáltatás hello hozzáférési jogkivonatot kérő azonosítja.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### Szolgáltatás hozzáférési jogkivonat válasz

Sikerességi válasz egy JSON OAuth 2.0 válasz a következő paraméterek hello tartalmazza:

| Paraméter | Leírás |
| --- | --- |
| access_token |hello kért hozzáférési jogkivonat. webes szolgáltatás hívása hello használhatja a token tooauthenticate toohello webszolgáltatás fogadásakor. |
| token_type |Azt jelzi, hogy hello token objektumtípus-érték. csak olyan típusú, amely az Azure AD által támogatott hello **tulajdonosi**. Tulajdonosi jogkivonatok kapcsolatos további információkért lásd: hello [OAuth 2.0 hitelesítési keretrendszer: tulajdonosi jogkivonat használati (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| expires_on |hello hello hozzáférési jogkivonat lejárati idejének. hello dátum egy jelenik meg azon másodpercek számát hello a 1970-01-01T0:0:0Z UTC amíg hello lejárati ideje. Az értéket nem használt toodetermine hello élettartama gyorsítótárazott jogkivonatokat. |
| not_before |hello idő mely hello a hozzáférési jogkivonat lesz használható. hello dátum egy jelenik meg azon másodpercek számát hello a 1970-01-01T0:0:0Z UTC amíg hello token érvényességi ideje.|
| Erőforrás |a fogadó webszolgáltatás hello App ID URI hello. |

#### Válasz – példa
hello következő példa bemutatja, az access token tooa webszolgáltatás sikeres válasz tooa kérelmet.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## Lásd még:
* [OAuth 2.0 Azure AD-ben](active-directory-protocols-oauth-code.md)
* [A C# hello szolgáltatás tooservice hívás a közös titkos minta](https://github.com/Azure-Samples/active-directory-dotnet-daemon) és [C# hello szolgáltatás tooservice hívás tanúsítvánnyal minta](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

---
title: "aaaAzure AD szolgáltatás tooservice auth OAuth2.0 meghatározta a-nevében-: használatával |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan hello OAuth2.0 a-meghatalmazásos folyamat toouse HTTP üzenetek tooimplement tooservice használatával."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 55b7fcfe6c0223bddedd8d8fa2defcb5769b43c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Szolgáltatás tooservice meghívja a delegált felhasználói identitás használatát hello a-meghatalmazásos folyamat
hello OAuth 2.0 On-Behalf-Of folyamat egy másik szolgáltatás vagy webes API hello használati eset ahol hív meg egy alkalmazás, egy szolgáltatás vagy webes API, amely pedig szükséges toocall szolgálja ki. hello lényege toopropagate hello útján megszerezte a felhasználó identitása és azon keresztül hello kérelem lánc engedélyek. Hello középső rétegű toomake hitelesített kérelmek toohello alárendelt szolgáltatás esetén az Azure Active Directory (Azure AD), egy hozzáférési jogkivonatot toosecure hello felhasználó nevében kell.

## A-nevében-: folyamatábrája
Tegyük fel, hogy hello a felhasználó hitelesítése a hello használó alkalmazások [OAuth 2.0 hitelesítési kódot adjon folyamat](active-directory-protocols-oauth-code.md). Ezen a ponton hello alkalmazás hello felhasználói jogcímek és hozzájárulási tooaccess hello középső rétegbeli webes API-k (API-A) rendelkezik egy hozzáférési jogkivonatot (lexikális elem A). Most az API A kell toomake egy hitelesített kérelmet toohello alsóbb rétegbeli webes API-t (API-B).

hello lépések hello a-meghatalmazásos folyamat jelent, és magyarázatát olvashatja a következő diagram hello hello segítségével.

![OAuth2.0 a-meghatalmazásos folyamat](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. hello ügyfélalkalmazás lehetővé teszi a kérelem tooAPI A hello token A.
2. API A toohello az Azure AD hitelesítési karakterlánc-kiállítási végpont hitelesíti, és kéri a token tooaccess API a b kiszolgálóra.
3. hello Azure AD hitelesítési karakterlánc-kiállítási végpont ellenőrzi az API A jogkivonat A hitelesítő adatokat, és problémák hozzáférési jogkivonat hello API B (lexikális elem B).
4. hello token B értéke hello engedélyezési fejlécének hello kérelem tooAPI a b kiszolgálóra.
5. Védett erőforrás hello adatokat ad vissza API a b kiszolgálóra.

## Hello alkalmazás és szolgáltatás regisztrálása az Azure ad-ben
Hello ügyfélalkalmazást és a hello középső rétegbeli szolgáltatás regisztrálása az Azure ad-ben.
### Hello középső rétegbeli szolgáltatás regisztrálása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.
4. Kattintson a **App regisztrációk** válassza **új alkalmazás regisztrációja**.
5. Adjon meg egy rövid nevet hello alkalmazás, és jelölje ki hello alkalmazás. Hello alkalmazás típusától függően set hello bejelentkezési URL-cím vagy átirányítási URL-cím toohello alap URL-címet. Kattintson a **létrehozása** toocreate hello alkalmazás.
6. Miközben továbbra is a hello Azure-portálon, válassza ki az alkalmazást, majd kattintson a **beállítások**. Hello beállítások menüből **kulcsok** és kulcs hozzáadása – 1 év vagy a 2 év kulcs időtartam kiválasztása. Ha ezen a lapon menti, hello kulcsérték jelenik meg, másolja ki és mentse a hello érték egy biztonságos helyre - e kulcsok későbbi tooconfigure hello Alkalmazásbeállítások szüksége lesz a megvalósításban - ezt a kulcsértéket nem lesz újra megjelenített, sem lekérhető bármelyik más módon kell megoldani, ezért kérjük, rekord azt, amint az Azure portál hello látható.

### Hello ügyfél alkalmazás regisztrálása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon, kattintson a fiókjában, és a hello **Directory** menüben válassza ki, hol kívánja tooregister, az alkalmazás hello Active Directory-bérlő.
3. Kattintson a **több szolgáltatások** a bal oldali navigációs hello, és válassza a **Azure Active Directory**.
4. Kattintson a **App regisztrációk** válassza **új alkalmazás regisztrációja**.
5. Adjon meg egy rövid nevet hello alkalmazás, és jelölje ki hello alkalmazás. Hello alkalmazás típusától függően set hello bejelentkezési URL-cím vagy átirányítási URL-cím toohello alap URL-címet. Kattintson a **létrehozása** toocreate hello alkalmazás.
6. Engedélyek konfigurálása az alkalmazás - hello beállítási menüben, kattintson a hello **szükséges engedélyek** területen kattintson a **Hozzáadás**, majd **API kiválasztása**, és a típus hello hello szövegmezőjének hello középső rétegbeli szolgáltatás neve. Kattintson a **Select engedélyeket** válassza ki "hozzáférési *szolgáltatásnév*".

### Ismert ügyfélalkalmazások konfigurálása
Ebben a forgatókönyvben a hello középső rétegbeli szolgáltatás nincs felhasználói beavatkozás tooobtain hello felhasználói hozzájárulás tooaccess hello alsóbb rétegbeli API rendelkezik. Ezért a beállítást toogrant hozzáférés toohello biztosítani kell az alsóbb rétegbeli API hello társaságuk hello hozzájárulási lépés a hitelesítés során részeként.
tooachieve a, hajtsa végre hello végre az alábbi lépéseket tooexplicitly bind hello ügyfél alkalmazás regisztrálásának az Azure AD hello középső rétegbeli szolgáltatást, amely egyesíti az egyetlen párbeszédpanel hello ügyfél és a középső rétegbeli által igényelt hello hozzájárulási hello regisztrációját.
1. Keresse meg a középső rétegbeli szolgáltatás regisztrációs toohello, és kattintson a **Manifest** tooopen hello jegyzék szerkesztő.
2. Hello jegyzékben, keresse meg a hello `knownClientApplications` tömb tulajdonság, és adja hozzá a hello hello ügyfél alkalmazás Ügyfélazonosítója elemeként.
3. Hello jegyzékfájl mentése hello Mentés gombra kattintva.

## Tooservice access token szolgáltatáskérés
toorequest hozzáférési tokent, ellenőrizze a HTTP POST toohello az Azure AD bérlő-specifikus végpont a következő paraméterek hello.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```
Attól függően, hogy úgy dönt, hogy hello ügyfélalkalmazás egy közös titkot, vagy a tanúsítvány által védett toobe két esetben van.

### Először. eset: egy közös titkot a hozzáférési token kérelem
A közös titkos kulcs használata esetén a szolgáltatás hozzáférési kérelmek tartalmazza a következő paraméterek hello:

| Paraméter |  | Leírás |
| --- | --- | --- |
| grant_type |Szükséges | hello jogkivonatkérelem hello típusa. A kérelmek jwt-t használ, hello értéknek kell lennie **urn: ietf:params:oauth:grant-típus: jwt-tulajdonosi**. |
| helyességi feltétel |Szükséges | hello érték, amelyet hello kérés hello jogkivonat. |
| client_id |Szükséges | hello Alkalmazásazonosító hozzárendelt toohello hívó szolgáltatás az Azure ad-vel a regisztráció során. Kattintson a toofind hello Alkalmazásazonosító hello Azure felügyeleti portálon, a **Active Directory**, hello directory kattintson, majd hello alkalmazás neve. |
| client_secret |Szükséges | hello kulcs hello szolgáltatás hívása az Azure ad-ben regisztrált. Ez az érték rendelkezik lett jegyezni a regisztrációs hello időpontjában. |
| Erőforrás |Szükséges | hello App ID URI-azonosítója hello szolgáltatást (védett erőforrás) fogadása. toofind hello App ID URI, a hello Azure felügyeleti portálon, kattintson a **Active Directory**, kattintson a hello directory, kattintson a hello alkalmazás nevét, majd **összes beállítás** , majd **tulajdonságai** . |
| requested_token_use |Szükséges | Itt adhatja meg, hogyan kell feldolgozni hello kérelmet. A-meghatalmazásos folyamat hello, a hello értéknek kell lennie **on_behalf_of**. |
| Hatókör |Szükséges | Egy szóközzel elválasztott hello jogkivonatkérelem hatókört. Az OpenID Connect, hello hatókör **openid** meg kell adni.|

#### Példa
hello alábbi HTTP POST kérelmek hello https://graph.windows.net webes API-k számára egy jogkivonatot. Hello `client_id` hello hozzáférési jogkivonatot kérő hello szolgáltatást azonosítja.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

### A második esetben: hozzáférési jogkivonat kérelem tanúsítvánnyal
Szolgáltatás hozzáférési kérelmek tanúsítvánnyal hello a következő paramétereket tartalmazza:

| Paraméter |  | Leírás |
| --- | --- | --- |
| grant_type |Szükséges | hello jogkivonatkérelem hello típusa. A kérelmek jwt-t használ, hello értéknek kell lennie **urn: ietf:params:oauth:grant-típus: jwt-tulajdonosi**. |
| helyességi feltétel |Szükséges | hello érték, amelyet hello kérés hello jogkivonat. |
| client_id |Szükséges | hello Alkalmazásazonosító hozzárendelt toohello hívó szolgáltatás az Azure ad-vel a regisztráció során. Kattintson a toofind hello Alkalmazásazonosító hello Azure felügyeleti portálon, a **Active Directory**, hello directory kattintson, majd hello alkalmazás neve. |
| client_assertion_type |Szükséges |hello értéknek kell lennie`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Szükséges | Egy helyességi feltétel (egy JSON Web Token), hogy toocreate van szüksége, és a jel hello a tanúsítványok, az alkalmazás hitelesítő adatként regisztrálva.  További információ a [tanúsítvány a hitelesítő adatok](active-directory-certificate-credentials.md) toolearn hogyan tooregister hello helyességi feltétel a tanúsítvány és hello formátuma.|
| Erőforrás |Szükséges | hello App ID URI-azonosítója hello szolgáltatást (védett erőforrás) fogadása. toofind hello App ID URI, a hello Azure felügyeleti portálon, kattintson a **Active Directory**, kattintson a hello directory, kattintson a hello alkalmazás nevét, majd **összes beállítás** , majd **tulajdonságai** . |
| requested_token_use |Szükséges | Itt adhatja meg, hogyan kell feldolgozni hello kérelmet. A-meghatalmazásos folyamat hello, a hello értéknek kell lennie **on_behalf_of**. |
| Hatókör |Szükséges | Egy szóközzel elválasztott hello jogkivonatkérelem hatókört. Az OpenID Connect, hello hatókör **openid** meg kell adni.|

Figyelje meg, hogy hello paraméterei szinte hello ugyanaz, mint hello hello kérelem által megosztott titkot azzal a különbséggel, hogy hello client_secret paraméter helyébe két paramétert: egy client_assertion_type és client_assertion.

#### Példa
a következő HTTP POST hello olyan hozzáférési jogkivonatot hello https://graph.windows.net Web API a tanúsítvánnyal kér. Hello `client_id` hello hozzáférési jogkivonatot kérő hello szolgáltatást azonosítja.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## Szolgáltatás tooservice hozzáférési jogkivonat válasz
Sikerességi válasz egy JSON OAuth 2.0 válasz a következő paraméterek hello.

| Paraméter | Leírás |
| --- | --- |
| token_type |Azt jelzi, hogy hello token objektumtípus-érték. csak olyan típusú, amely az Azure AD által támogatott hello **tulajdonosi**. Tulajdonosi jogkivonatok kapcsolatos további információkért lásd: hello [OAuth 2.0 hitelesítési keretrendszer: tulajdonosi jogkivonat használati (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| Hatókör |hozzáférést biztosít a hello token hello hatókörében. |
| expires_in |idő hello hozzáférési jogkivonat hello hossza (másodpercben) érvényes. |
| expires_on |hello hello hozzáférési jogkivonat lejárati idejének. hello dátum egy jelenik meg azon másodpercek számát hello a 1970-01-01T0:0:0Z UTC amíg hello lejárati ideje. Az értéket nem használt toodetermine hello élettartama gyorsítótárazott jogkivonatokat. |
| Erőforrás |hello App ID URI-azonosítója hello szolgáltatást (védett erőforrás) fogadása. |
| access_token |hello kért hozzáférési jogkivonat. hello hívja service szolgáltatással a token tooauthenticate toohello fogadó. |
| id_token |hello megadott azonosítóhoz lexikális eleme. hello szolgáltatás hívása a tooverify hello felhasználói identitás használatára, és a hello felhasználói munkamenet elindításához. |
| refresh_token |hello frissítési jogkivonat hello kért hozzáférési jogkivonat. hello szolgáltatás hívása használhatja a token toorequest egy új hozzáférési jogkivonat hello aktuális hozzáférési jogkivonat lejárata után is. |

### Sikerességi válasz – példa
hello következő példa bemutatja egy hozzáférési jogkivonatot hello https://graph.windows.net webes API-t a sikeres válasz tooa kérelmet.

```
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.windows.net",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### Hiba válasz – példa
Egy hibaüzenetet ad vissza az Azure AD-jogkivonat végpontjához hello alsóbb rétegbeli API-hoz, egy hozzáférési jogkivonatot tooacquire közben például többtényezős hitelesítés beállítása a feltételes hozzáférési házirend hello alsóbb rétegbeli API-e. hello középső rétegbeli szolgáltatás kell surface e hiba toohello ügyfélalkalmazást, így hello ügyfélalkalmazás biztosítani tudja hello felhasználói beavatkozás toosatisfy hello feltételes hozzáférési szabályzat.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due tooa configuration change made by your administrator, or because you moved tooa new location, you must enroll in multi-factor authentication tooaccess 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## Védett erőforrás hello access token tooaccess hello használata
Most hello középső rétegbeli szolgáltatás után használhatja hello token szerzett be a fenti toomake hitelesített kérelmek toohello webes API-t, által hello token beállítása hello `Authorization` fejléc.

### Példa
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## Következő lépések
További információ a hello OAuth 2.0 protokoll és egy másik módja tooperform szolgáltatás tooservice hitelesítési ügyfél hitelesítő adataival.
* [Szolgáltatás tooservice auth OAuth 2.0 ügyfél hitelesítő adatok megadása az Azure AD segítségével](active-directory-protocols-oauth-service-to-service.md)
* [OAuth 2.0 Azure AD-ben](active-directory-protocols-oauth-code.md)

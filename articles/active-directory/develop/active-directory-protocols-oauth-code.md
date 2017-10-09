---
title: "OAuth 2.0 hitelesítésikód-folyamata az Azure AD aaaUnderstand hello |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan férhetnek hozzá a toouse HTTP üzenetek tooauthorize a tooweb alkalmazások és a webes API-k használata az Azure Active Directory és az OAuth 2.0-bérlőben."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: de3412cb-5fde-4eca-903a-4e9c74db68f2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4a6fe67d786a5fcb87d1059c2e94ba0c88d26cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Engedélyezi a hozzáférést tooweb alkalmazások OAuth 2.0 és az Azure Active Directory használatával
Azure Active Directory (Azure AD) használja OAuth 2.0 tooenable tooauthorize tooweb alkalmazásokat, és a webes API-knak az Azure AD-bérlőben. Ez az útmutató nyelvfüggetlen, és ismerteti, hogyan toosend és HTTP-üzenetek fogadása a nyílt forráskódú kódtárai bármelyikét használata nélkül.

OAuth 2.0 hitelesítésikód-folyamata hello ismertetett [hello OAuth 2.0-s szabvány 4.1 szakasz](https://tools.ietf.org/html/rfc6749#section-4.1). Használt tooperform hitelesítési és engedélyezési a legtöbb alkalmazás típusok, beleértve a webalkalmazások és natív módon telepített alkalmazásokat.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## OAuth 2.0 hitelesítési folyamata
Magas szinten az alkalmazás teljes engedélyezési folyamata hello néz ki egy kicsit:

![OAuth hitelesítési folyamata](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## Az engedélyezési kód kérése
hello hitelesítésikód-folyamata kezdődik arra utasíthatja a hello felhasználói toohello hello ügyfél `/authorize` végpont. Ebben a kérelemben hello ügyfél azt jelzi, hello engedélyek tooacquire hello felhasználó szükséges. Kaphat hello OAuth 2.0 típusú végpontok a klasszikus Azure portál, az alkalmazás oldalról hello **végpontok megtekintése** hello alsó fiókban gombra.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett: bérlői azonosítók, például `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` vagy `common` bérlői független jogkivonatokat |
| client_id |Szükséges |hello alkalmazásazonosító hozzárendelt tooyour app és az Azure AD regisztrálásakor. Ez a hello Azure portálon találja meg. Kattintson a **Active Directory**, hello directory kattintson, válassza ki a hello alkalmazást, és kattintson **konfigurálása** |
| response_type |Szükséges |Tartalmaznia kell `code` a hello hitelesítésikód-folyamata. |
| redirect_uri |Ajánlott |az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszok redirect_uri hello.  Azt pontosan egyeznie kell azzal a különbséggel url-kódolású kell lennie az hello portálon regisztrált hello redirect_uris.  Natív & mobileszköz-alkalmazások esetén használjon hello alapértelmezett értékének `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode |Ajánlott |Határozza meg, amelyeket használt toosend hello eredményül kapott jogkivonat hátsó tooyour app hello módszerét.  Lehet `query` vagy `form_post`. |
| state |Ajánlott |Hello kérelem is hello token válaszként visszaadott szerepel érték. Egy véletlenszerűen generált egyedi érték jellemzően a [webhelyközi kérések hamisításának megakadályozása támadások megelőzése](http://tools.ietf.org/html/rfc6749#section-10.12).  hello állapota is hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |
| Erőforrás |Nem kötelező |hello App ID URI hello Web API (védett erőforrás). toofind hello App ID URI hello Web API-t a hello Azure portál, kattintson a **Active Directory**hello directory kattintson, hello alkalmazás, majd kattintson gombra **konfigurálása**. |
| parancssor |Nem kötelező |Hello jelzi a felhasználói beavatkozás szükséges.<p> Érvényes értékek a következők: <p> *bejelentkezési*: hello felhasználói felszólító tooreauthenticate kell lennie. <p> *hozzájárulás*: felhasználói hozzájárulás rendelkezik, de toobe frissíteni kell. hello felhasználói felszólító tooconsent kell lennie. <p> *admin_consent*: egy rendszergazda felszólító tooconsent kell lennie a szervezetben lévő összes felhasználó nevében |
| login_hint |Nem kötelező |Használt toopre-kitöltés hello felhasználónév vagy e-mail cím mező hello bejelentkezési oldal hello felhasználó, akkor lehet, ha ismeri a felhasználónevét időben.  Gyakran alkalmazások újrahitelesítés, amelyek már kivont hello felhasználónév előző bejelentkezés során használja ezt a paramétert használó hello `preferred_username` jogcímek. |
| domain_hint |Nem kötelező |Itt hello bérlő és a tartományhoz, amely a felhasználó hello javaslat a toosign kell használnia. hello hello domain_hint értéke egy regisztrált tartományt a hello bérlő. Ha hello bérlői összevont tooan helyszíni címtárat, az aad-ben toohello megadott tenant összevonási kiszolgáló irányítja át. |

> [!NOTE]
> Ha hello felhasználói része egy szervezet, hello szervezet rendszergazdája hozzájárulás vagy elutasítása hello felhasználó nevében, vagy hello felhasználói tooconsent engedélyezése. hello felhasználó kap hello beállítás tooconsent csak akkor, ha hello rendszergazda engedélyezi azt.
>
>

Ezen a ponton hello felhasználói tooenter kéri a hitelesítő adatokat és a hozzájárulási toohello engedélyek jelzett hello `scope` lekérdezési paraméter. Miután hello felhasználó hitelesíti, és engedélyezi a hozzájárulási küld-e az Azure AD egy válasz tooyour app hello, `redirect_uri` a kérés címe.

### A sikeres válasz
A sikeres válasz nézhet ki:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Paraméter | Leírás |
| --- | --- |
| admin_consent |hello érték értéke igaz, ha a rendszergazda átadni kívánt hozzájárult, e tooa jóváhagyási kérelem kérése. |
| Kód |hello engedélyezési kódot kért hello alkalmazás. hello alkalmazás hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használja hello cél erőforráson. |
| session_state |Hello jelenlegi felhasználói munkamenetet azonosító egyedi érték. Ez az érték egy GUID, de egy vizsgálat nélkül átadott nem átlátszó értéket kell kezelni. |
| state |Ha a state paraméter hello kérelem, ugyanazt az értéket meg kell jelennie a hello válasz hello is szerepel. Ajánlott a hello alkalmazás tooverify, hogy hello állapot hello kérelem-válasz értékei azonos hello válasz használata előtt. Ezzel a megoldással toodetect [többhelyes kérelem hamisítására (CSRF) támadások](https://tools.ietf.org/html/rfc6749#section-10.12) hello ügyfél ellen. |

### Hibaválaszba
Hibaválaszok is küldhet toohello `redirect_uri` , hogy hello alkalmazás kezeli őket megfelelően.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód értékét, az 5.2 hello szakaszban meghatározott [OAuth 2.0 hitelesítési keretrendszer](http://tools.ietf.org/html/rfc6749). hello következő táblázat ismerteti az Azure AD visszaadó hello hibakódok. |
| error_description |Hello hiba részletes leírását. Ez az üzenet nem feltétlenül toobe végfelhasználói leíró. |
| state |hello állapot értéke nem használja fel újra egy véletlenszerűen generált érték hello kérelemben küldött és az eredmény abban hello válasz tooprevent webhelyközi kérések hamisítására (CSRF) támadások. |

#### Engedélyezési végpont hibái hibakódok
hello következő táblázatban található hello különböző hibakódok hello visszaküldött `error` hello hibaválaszba paramétere.

| Hibakód: | Leírás | Ügyfélművelet |
| --- | --- | --- |
| invalid_request |Protokollhiba történt, például a hiányzó kötelező paraméter. |Javítsa ki, és küldje el újra hello kérelmet. A fejlesztési hiba, és a kezdeti tesztelés során általában kiszűri. |
| unauthorized_client |hello ügyfélalkalmazás nincs engedélyezett toorequest egy engedélyezési kódot. |Ez általában akkor fordul elő, amikor hello ügyfélalkalmazás nincs regisztrálva az Azure ad-ben, vagy az Azure AD-bérlő toohello felhasználó nincs hozzáadva. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |
| ACCESS_DENIED |Erőforrás tulajdonosa hozzájárulási megtagadva |hello ügyfélalkalmazás értesítheti hello felhasználó számára, akkor nem lehet folytatni, kivéve, ha hello felhasználó. |
| unsupported_response_type |hello engedélyezési kiszolgáló nem támogatja a hello választípus hello kérelemben. |Javítsa ki, és küldje el újra hello kérelmet. A fejlesztési hiba, és a kezdeti tesztelés során általában kiszűri. |
| server_error |hello kiszolgáló váratlan hibát észlelt. |Ismételje meg a hello kérelmet. Ezeket a hibákat okozhat az átmeneti állapotot. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy válaszában késleltetett tooa ideiglenes hiba miatt. |
| temporarily_unavailable |hello kiszolgáló egy ideiglenesen túlterhelt toohandle hello kérelem. |Ismételje meg a hello kérelmet. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy válaszában tooa ideiglenes állapot miatt késik. |
| invalid_resource |hello célerőforrása érvénytelen, mert nem létezik, az Azure AD a fájl nem található vagy nincs megfelelően konfigurálva. |Ez azt jelzi, hogy hello erőforrás, ha létezik, nem konfiguráltak hello bérlő. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |

## Hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használja
Jut hozzá az engedélyezési kódot, és hello felhasználói a engedélyt kaptak, akkor is beválthatja hello code access token toohello szükséges erőforrás, egy POST kérést toohello elküldésével `/token` végpont:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett: bérlői azonosítók, például `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` vagy `common` bérlői független jogkivonatokat |
| client_id |Szükséges |hello alkalmazásazonosító hozzárendelt tooyour app és az Azure AD regisztrálásakor. Ez a hello klasszikus Azure portálon találja meg. Kattintson a **Active Directory**, hello directory kattintson, válassza ki a hello alkalmazást, és kattintson **konfigurálása** |
| grant_type |Szükséges |Kell `authorization_code` a hello hitelesítésikód-folyamata. |
| Kód |Szükséges |Hello `authorization_code` hello előző szakaszban beszerzett |
| redirect_uri |Szükséges |hello azonos `redirect_uri` érték, de a használt tooacquire hello `authorization_code`. |
| client_secret |a web Apps szükséges |az alkalmazás hello app regisztrációs portálon létrehozott hello alkalmazáskulcsot.  Akkor kell nem használható natív alkalmazás, mert client_secrets megbízhatóan nem tárolható az eszközökön.  Szükséges, hogy az webalkalmazások és webes API-k, amelyek hello képességét toostore hello `client_secret` biztonságosan a hello kiszolgálóoldali. |
| Erőforrás |szükséges, ha a kérelemben megadott engedélyezési kódot, ellenkező esetben nem kötelező |hello App ID URI hello Web API (védett erőforrás). |

Kattintson a toofind hello App ID URI hello Azure felügyeleti portálon, a **Active Directory**, kattintson hello címtár, kattintson hello alkalmazásra, majd **konfigurálása**.

### A sikeres válasz
Az Azure AD, a sikeres válasz hozzáférési jogkivonatot ad vissza. toominimize hálózati hívások hello ügyfélalkalmazás és azok kapcsolódó késés, hello ügyfélalkalmazás hello a jogkivonatok élettartama hello OAuth 2.0 válasz megadott hozzáférési jogkivonatok kell gyorsítótárba. toodetermine hello jogkivonat élettartamát, vagy hello használata `expires_in` vagy `expires_on` paraméterértékeket.

Ha a webes API-erőforrás adja vissza egy `invalid_token` hibakód, ez azt jelentheti, hogy a hello erőforrás megállapította, hogy hello jogkivonat érvényessége lejárt. Ha hello ügyfél- és erőforrás óra alkalommal különböző (ismert egy "eltérésére alkalommal"), a hello erőforrás fontolóra hello token toobe lejárt, mielőtt hello jogkivonat nincs bejelölve hello ügyfél-gyorsítótárból. Ha ez történik, törölje a hello token a gyorsítótárból hello, akkor is, ha továbbra is számított élettartamuk belül.

A sikeres válasz nézhet ki:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
  "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ."
}

```

| Paraméter | Leírás |
| --- | --- |
| access_token |hello kért hozzáférési jogkivonat. hello alkalmazást a védett erőforrások, például a webes API-k token tooauthenticate toohello használhatja. |
| token_type |Azt jelzi, hogy hello token objektumtípus-érték. hello csak adja meg, hogy az Azure AD által támogatott tulajdonosi-e. Tulajdonosi jogkivonatok kapcsolatos további információkért lásd: [OAuth2.0 hitelesítési keretrendszer: tulajdonosi jogkivonat használati (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| expires_on |hello hello hozzáférési jogkivonat lejárati idejének. hello dátum egy jelenik meg azon másodpercek számát hello a 1970-01-01T0:0:0Z UTC amíg hello lejárati ideje. Az értéket nem használt toodetermine hello élettartama gyorsítótárazott jogkivonatokat. |
| Erőforrás |hello App ID URI hello Web API (védett erőforrás). |
| Hatókör |Megszemélyesítés engedélyek toohello ügyfélalkalmazást. hello alapértelmezett engedély `user_impersonation`. védett erőforrás hello hello tulajdonosa további értékeket regisztrálhatja az Azure ad-ben. |
| refresh_token |Az OAuth 2.0-s frissítési jogkivonat. hello app használhatja a token tooacquire további hozzáférési jogkivonatok hello aktuális hozzáférési jogkivonat lejárata után is.  Frissítési jogkivonatok hosszú élettartamú, és hosszabb ideig használt tooretain hozzáférés tooresources lehet. |
| id_token |Az aláírás nélküli JSON webes jogkivonat (JWT). hello alkalmazást is base64Url hello szegmensek a bejelentkezett felhasználó hello token toorequest információinak dekódolására. hello app hello értékek gyorsítótárazása és megjeleníteni azokat is, de azt nem igazolható a azokat bármilyen engedélyezési vagy a biztonsági határokat. |

### JWT jogkivonat jogcímek
hello JWT jogkivonat hello hello értékének `id_token` paraméter is dekódolható hello jogcímek a következő:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

JSON webes jogkivonatainak kapcsolatos további információkért lásd: hello [JWT IETF-vázlat specifikáció](http://go.microsoft.com/fwlink/?LinkId=392344). Hello tokentípusokat és a jogcímeket kapcsolatos további információkért olvassa el a [támogatott jogkivonat és jogcímtípusok](active-directory-token-and-claims.md)

Hello `id_token` paraméter a következő jogcímtípusokat hello foglalja magában:

| Jogcím típusa | Leírás |
| --- | --- |
| és |A célközönség hello jogkivonat. Amikor hello token tooa ügyfélalkalmazást, hello célközönségét hello `client_id` hello ügyfél. |
| Exp |Lejárati idő. hello hello jogkivonat lejárati idejének. Hello token toobe érvényes, hello aktuális dátum és idő kell kisebb vagy egyenlő, mint toohello `exp` érték. hello idő egy jelenik meg azon másodpercek számát hello 1970. január 1. a (1970-01-01T0:0:0Z) UTC amíg hello idő hello token lett kiállítva. |
| family_name |Felhasználó Vezetéknév vagy családnév. hello alkalmazás meg tudja jeleníteni ezt az értéket. |
| given_name |Felhasználó első nevét. hello alkalmazás meg tudja jeleníteni ezt az értéket. |
| IAT |Kiadott időpontban. hello idő, amikor hello JWT lett kiállítva. hello idő egy jelenik meg azon másodpercek számát hello 1970. január 1. a (1970-01-01T0:0:0Z) UTC amíg hello idő hello token lett kiállítva. |
| iss |Jogkivonatot kibocsátó hello azonosítja. |
| NBF |Nem korábbi. hello idő, amikor hello jogkivonat érvénybe lép. Hello token toobe érvényes, a hello aktuális dátum/idő értéknek kell lennie nullánál toohello Nbf. hello idő egy jelenik meg azon másodpercek számát hello 1970. január 1. a (1970-01-01T0:0:0Z) UTC amíg hello idő hello token lett kiállítva. |
| OID |Objektumazonosító (ID) hello felhasználói objektum az az Azure ad-ben. |
| Sub |Token tulajdonos azonosítója. Ez az egy állandó és nem módosítható azonosítót a hello token hello felhasználó ismerteti. Használja ezt az értéket logika gyorsítótárazását. |
| TID |A bérlői azonosító hello jogkivonatot kibocsátó hello Azure AD-bérlő. |
| unique_name |Egyedi azonosítója, amely megjelenített toohello felhasználó lehet. Ez általában az egyszerű felhasználónév (UPN). |
| egyszerű felhasználónév |Hello felhasználó egyszerű felhasználóneve. |
| ver |Verzió. hello hello JWT jogkivonat, általában az 1.0-s verziója. |

### Hibaválaszba
hello jogkivonat kibocsátási végpont olyan HTTP hibakódok, mert hello ügyfélhívásokat hello jogkivonat kibocsátási végpont közvetlenül. Ezenkívül toohello HTTP-állapotkód, hello Azure AD hitelesítési karakterlánc-kiállítási végpont is egy dokumentumot ad vissza JSON hello hiba leíró objektummal.

Egy minta hibaüzenetet nézhet ki:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: hello provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |
| error_codes |STS-specifikus hibakódok, amelyek segítik a diagnosztika listáját. |
| időbélyeg |hello idő hello hiba lépett fel. |
| trace_id |Hello kérelem, amelyek segítik a diagnosztika egyedi azonosítója. |
| correlation_id |Hello kérelem, amelyek segítik a diagnosztika összetevői között egyedi azonosítója. |

#### HTTP-állapotkódok
hello következő táblázatban hello HTTP állapotkódokat hello jogkivonat kibocsátási végpont értéket ad vissza. Bizonyos esetekben hello hibakód elegendő toodescribe hello válasz, azonban ha nincsenek hibák, kell tooparse hello kísérő JSON-dokumentum, és vizsgálja meg a hibakódot.

| HTTP-kód | Leírás |
| --- | --- |
| 400 |Alapértelmezett HTTP-kód. A legtöbb esetben használja, és általában tooa formátumú kérelem miatt. Javítsa ki, és küldje el újra hello kérelmet. |
| 401 |A hitelesítés sikertelen volt. Például hello kérelemhez hello client_secret paraméter nincs megadva. |
| 403 |Nem sikerült engedélyezése. Például hello felhasználónak nincs engedélye tooaccess hello erőforrás. |
| 500 |Belső hiba történt a következő hello szolgáltatást. Ismételje meg a hello kérelmet. |

#### A token-végpont hibákat hibakódok
| Hibakód: | Leírás | Ügyfélművelet |
| --- | --- | --- |
| invalid_request |Protokollhiba történt, például a hiányzó kötelező paraméter. |Javítsa ki, és küldje el újra a kérelmet hello |
| invalid_grant |hello engedélyezési kód érvénytelen vagy lejárt. |Próbálja meg egy új kérelmet toohello `/authorize` végpont |
| unauthorized_client |hello hitelesített az ügyfél nem jogosult toouse az engedélyezés támogatás típusa. |Ez általában akkor fordul elő, amikor hello ügyfélalkalmazás nincs regisztrálva az Azure ad-ben, vagy az Azure AD-bérlő toohello felhasználó nincs hozzáadva. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |
| invalid_client |Nem sikerült ügyfél-hitelesítéshez. |hello ügyfél hitelesítő adatok érvénytelenek. toofix, hello alkalmazás-rendszergazda frissítése hello hitelesítő adatokat. |
| unsupported_grant_type |hello engedélyezési kiszolgáló nem támogatja a hello engedélyezési engedélytípus. |Változás hello támogatás hello kérelem típusa. Hiba az ilyen típusú csak a fejlesztés során megtörténik, és a kezdeti tesztelés során észlelt. |
| invalid_resource |hello célerőforrása érvénytelen, mert nem létezik, az Azure AD a fájl nem található vagy nincs megfelelően konfigurálva. |Ez azt jelzi, hogy hello erőforrás, ha létezik, nem konfiguráltak hello bérlő. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |
| interaction_required |hello kérelem felhasználói beavatkozást igényel. Például egy további hitelesítési lépésre szükség. | Helyett a nem interaktív kérelmet, próbálkozzon újra egy interaktív engedélyezési kérelmet hello azonos erőforrás. |
| temporarily_unavailable |hello kiszolgáló egy ideiglenesen túlterhelt toohandle hello kérelem. |Ismételje meg a hello kérelmet. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy válaszában tooa ideiglenes állapot miatt késik. |

## Hello access token tooaccess hello erőforrás használata
Most, hogy sikeresen jut hozzá egy `access_token`, használható hello token kérelmek tooWeb API-k, belefoglalja azt hello `Authorization` fejléc. Hello [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) megadását ismerteti, hogyan toouse tulajdonosi jogkivonatokhoz HTTP-kérelmek tooaccess védett erőforrásokhoz.

### Mintakérelem
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### Hibaválaszba
Védett erőforrások, amelyek megvalósítják az RFC 6750 probléma HTTP-állapotkódok. Ha hello kérelem nem tartalmazza a hitelesítő adatok, vagy hiányzik a hello token, hello válasz egy `WWW-Authenticate` fejléc. Amikor egy kérelem sikertelen lesz, hello erőforrás kiszolgáló válaszol, HTTP-állapotkód: hello és a hibakódot.

hello következő egy példa egy sikertelen válasz esetén hello ügyfél kérelem nem tartalmazza a hello tulajdonosi jogkivonatot:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="hello access token is missing.",
```

#### Hiba paramétereit
| Paraméter | Leírás |
| --- | --- |
| authorization_uri |hello hello engedélyezési kiszolgáló URI-val (fizikai endpoint). Ezt az értéket a keresési kulcs tooget hello kiszolgáló a felderítési végpont kapcsolatos további információk is szolgál. <p><p> hello ügyfélnek ellenőrizni kell, hogy hello engedélyezési kiszolgáló megbízható. Amikor hello erőforrás Azure AD által védett, nem megfelelő tooverify https://login.microsoftonline.com karakterlánccal kezdődik hello URL-címet vagy egy másik állomásnév, amely támogatja az Azure AD. A bérlő-specifikus erőforrás mindig kell visszaadnia egy bérlő vonatkozó engedélyezési URI Azonosítót. |
| error |Egy hiba kód értékét, az 5.2 hello szakaszban meghatározott [OAuth 2.0 hitelesítési keretrendszer](http://tools.ietf.org/html/rfc6749). |
| error_description |Hello hiba részletes leírását. Ez az üzenet nem feltétlenül toobe végfelhasználói leíró. |
| erőforrás_azonosítója |Beolvasása hello hello erőforrás egyedi azonosítója. hello ügyfélalkalmazás használhatja ezt az azonosítót hello hello értékeként `resource` paraméter hello erőforrás jogkivonat kérelem során. <p><p> Fontos a hello ügyfél alkalmazás tooverify ezt az értéket, ellenkező esetben a rosszindulatú szolgáltatás képes tooinduce előfordulhat, hogy egy **utasítással történő jogosultságszint-az-jogosultságokkal** támadás <p><p> ajánlott stratégia megakadályozza az olyan támadások érték, amely hello tooverify hello `resource_id` megfelel hello hello Web API URL-címet talál, amely alatt érhető el. Például ha https://service.contoso.com/data is hozzáférnek, hello `resource_id` htttps://service.contoso.com/ lehet. hello ügyfélalkalmazás elutasítása kell egy `resource_id` , nem kezdődik hello alap URL-cím kivéve, ha egy másodlagos megbízható módot tooverify hello azonosítója. |

#### Tulajdonosi séma hibakódok
hello RFC 6750 specification határozza meg a következő hibák hello válaszul hello WWW-Authenticate fejléc és tulajdonosi sémát használó erőforrásokhoz tartozó hello.

| HTTP-állapotkód | Hibakód: | Leírás | Ügyfélművelet |
| --- | --- | --- | --- |
| 400 |invalid_request |hello kérés formátuma helytelen. Például akkor lehet, hogy hiányzik egy paraméter, vagy ugyanazon paraméter kétszer hello segítségével. |Javítsa ki hello hibát, majd próbálja megismételni a hello kérelem. Hiba az ilyen típusú csak a fejlesztés során megtörténik, és a kezdeti tesztelése észlelhető. |
| 401 |invalid_token |hello hozzáférési jogkivonat hiányzó, érvénytelen, vagy vissza lett vonva. hello error_description paraméter értékének hello további részletesen ismerteti. |Kérjen egy új jogkivonatot hello engedélyezési kiszolgálótól. Ha nem sikerül hello új jogkivonatot, váratlan hiba történt. Egy hiba üzenet toohello felhasználó küldeni, és véletlenszerű késleltetése után próbálkozzon újra. |
| 403 |insufficient_scope |hello hozzáférési jogkivonat nem tartalmaz hello megszemélyesítési engedélyek szükséges tooaccess hello erőforrás. |Új engedélyezési kérelem toohello engedélyezési végpont küldése. Ha hello válasz hello hatókör-paramétert tartalmaz, hello kérelem toohello erőforrás hello hatókör azonosító használata. |
| 403 |insufficient_access |hello tulajdonos hello jogkivonat nem rendelkezik, amelyek a szükséges tooaccess hello erőforrás hello engedélyekkel. |Rákérdezés hello felhasználói toouse más fiókot vagy toorequest engedélyek toohello megadott erőforrás. |

## Hello hozzáférési jogkivonatok frissítése
Hozzáférési jogkivonatok rövid élettartamú és toocontinue erőforrások elérése után frissíteni kell. Hello is frissítheti `access_token` szerint egy másik `POST` toohello kérelem `/token` végpont, de ezúttal hello biztosító `refresh_token` helyett hello `code`.

Frissítési jogkivonatok nem rendelkezik megadott élettartama. A frissítési jogkivonatokat hello élettartama általában viszonylag hosszú. Azonban bizonyos esetekben a frissítési jogkivonatokat lejár, vissza lenne vonva, vagy nem rendelkezik elegendő jogosultsággal szükséges hello a művelethez. Az alkalmazás megfelelően hello jogkivonat kibocsátási végpont által visszaadott tooexpect és leíró hibák kell.

Amikor a frissítési token hibával választ, elveti az hello jelenlegi frissítési jogkivonat és új hitelesítési kódot kér, vagy hozzáférési tokent. Különösen akkor, ha egy frissítés használatával lexikális elem: hello engedélyezési Code Grant flow, a Ha tartalmazó hello választ kap `interaction_required` vagy `invalid_grant` hibakódok hello frissítési jogkivonat elvetése és az új hitelesítési kódot kér.

Egy minta kérelem toohello **bérlői-specifikus** végpont (használhatja hello **közös** végpont) tooget egy frissítési jogkivonat használatával új tokenre néz ki:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

### A sikeres válasz
A sikeres token válasz hasonlóan fog kinézni:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```
| Paraméter | Leírás |
| --- | --- |
| token_type |hello lexikális elem típusa. értéke csak akkor támogatott hello **tulajdonosi**. |
| expires_in |hello hátralévő hello jogkivonat élettartamát másodpercben. Jellemző értéke 3600 (egy óra). |
| expires_on |hello dátum és idő alapján, amely hello jogkivonat lejár. hello dátum egy jelenik meg azon másodpercek számát hello a 1970-01-01T0:0:0Z UTC amíg hello lejárati ideje. |
| Erőforrás |Azonosítja a hello védett erőforrás a hello hozzáférési jogkivonatot használt tooaccess is lehet. |
| Hatókör |Megszemélyesítés engedélyek toohello natív ügyfélalkalmazást. hello alapértelmezett engedély **user_impersonation**. hello cél erőforrás tulajdonosa hello választható érték regisztrálhatja az Azure AD-ben. |
| access_token |hello új hozzáférési jogkivonat kért. |
| refresh_token |Egy új OAuth 2.0 refresh_token hello a válaszban lejártakor használt toorequest új hozzáférési jogkivonatok használható. |

### Hibaválaszba
Egy minta hibaüzenetet nézhet ki:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: hello application named https://foo.microsoft.com/mail.read was not found in hello tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if hello application has not been installed by hello administrator of hello tenant or consented tooby any user in hello tenant.  You might have sent your authentication request toohello wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |
| error_codes |STS-specifikus hibakódok, amelyek segítik a diagnosztika listáját. |
| időbélyeg |hello idő hello hiba lépett fel. |
| trace_id |Hello kérelem, amelyek segítik a diagnosztika egyedi azonosítója. |
| correlation_id |Hello kérelem, amelyek segítik a diagnosztika összetevői között egyedi azonosítója. |

Hello hibakódokat és a javasolt művelet ügyfél hello ismertetését lásd: [token-végpont hibák hibakódok](#error-codes-for-token-endpoint-errors).

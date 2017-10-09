---
title: "felhasználói beavatkozás nélkül erőforrások biztonságba helyezése az Azure AD aaaUse v2.0 tooaccess |} Microsoft Docs"
description: "A webalkalmazások Azure AD hello végrehajtásának hello OAuth 2.0 hitelesítési protokoll használatával."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 0003ec836d633a5466c48033adedac1108f27203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Az Azure Active Directory v2.0 és hello OAuth 2.0 ügyfél hitelesítő adatai
Használhatja a hello [OAuth 2.0 ügyfél hitelesítő adatai megadják](http://tools.ietf.org/html/rfc6749#section-4.4), más néven *két Egyszárú OAuth*, tooaccess webkiszolgáló által szolgáltatott erőforrások alkalmazás hello identitásával. Gyakran engedélyezze az ilyen típusú kiszolgálók – olyan műveleteket, amelyek kell hello háttérben futnak, a felhasználó azonnali közreműködése nélkül szolgál. Ilyen típusú alkalmazások gyakran történik a hivatkozott tooas *démonok* vagy *szolgáltatásfiókok*.

> [!NOTE]
> hello v2.0-végpontra nem támogatja, minden Azure Active Directory forgatókönyvek és funkciók. toodetermine kell használnia a hello v2.0-végpontra, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
>
>

A több tipikus hello *három Egyszárú OAuth*, ügyfélalkalmazás rendelt engedélyek tooaccess erőforrás egy adott felhasználó nevében. hello engedély delegált hello felhasználói toohello alkalmazás, általában során hello [hozzájárulás](active-directory-v2-scopes.md) folyamat. Azonban az ügyfél-hitelesítő adatok folyamata hello, engedélyekkel közvetlenül toohello alkalmazás saját magát. Amikor hello alkalmazás megjeleníti a token tooa erőforrás, hello erőforrás hello alkalmazás maga engedélyezési tooperform egy művelettel rendelkezik, és nem hello rendelkezik engedélyezési érvénybe lépteti.

## Protokoll diagramja
hello teljes ügyfél-hitelesítő adatok folyamata hasonló toohello következő diagram keres. Azt ismertetik hello lépéseket a cikk későbbi részében.

![Ügyfél hitelesítő adatai](../../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## Közvetlen engedélyezési beolvasása
Az alkalmazások általában kap közvetlen engedélyezési tooaccess erőforrás az alábbi két módszer egyikével: hello erőforrás: hozzáférés-vezérlési listaként (ACL) vagy alkalmazás engedély-hozzárendelést az Azure Active Directory (Azure AD) révén. Két módszer közül hello leggyakoribb Azure AD-ben, és az ügyfelek és az erőforrásokat, hajtsa végre az ügyfél-hitelesítő adatok folyamata hello ajánlott azokat. Egy erőforrás választhat tooauthorize az ügyfelek más módon azonban. Minden erőforrás server választható hello metódus, amely hello legtöbb az alkalmazás.

### Hozzáférés-vezérlési listák
Egy erőforrás-szolgáltató kényszerítése előfordulhat, hogy egy jogosultsági ellenőrzés, hogy tudja, és engedélyezi a hozzáférést a valamilyen konkrét szintje Alkalmazásazonosítók listája alapján. Amikor hello erőforrás jogkivonatot kap hello v2.0-végpontra, dekódolása hello jogkivonatot, és hello ügyfél Alkalmazásazonosító kinyerése hello `appid` és `iss` jogcímeket. Ezután összeveti a hozzáférés-vezérlési Listában, amely a karbantartott hello alkalmazást. hozzáférés-vezérlési lista granularitási hello és metódus erőforrások közötti jelentősen változhat.

Gyakori használati eset egy hozzáférés-vezérlési lista toorun teszteli, webalkalmazás, vagy egy webes API toouse. hello Web API előfordulhat, hogy engedélyezze a teljes körű engedélyekkel tooa adott ügyfél csak egy részét. toorun végpont teszteket a hello API, hozzon létre szerez be a v2.0-végponttól hello származó jogkivonatokat, majd elküldi azokat toohello API teszt ügyfél. hello API majd ellenőrzések hello ACL hello a teszt teljes hozzáférési toohello API teljes funkció ügyfél alkalmazás azonosítója. Ha ilyen típusú hozzáférés-vezérlési lista használja, ne feledje toovalidate, nem csak a hívó hello `appid` érték. Is ellenőrzi, hogy hello `iss` hello token értéke megbízható.

Ez a hitelesítési típus közös démonok és fogyasztói személyes Microsoft-fiókkal rendelkező felhasználók tulajdonában lévő tooaccess adatokat igénylő szolgáltatásfiókokat. A szervezet tulajdonában lévő adatokat azt javasoljuk, hogy hello szükséges engedélyezési Alkalmazásengedélyek keresztül kap.

### Alkalmazásengedélyek
Hozzáférés-vezérlési listák helyett használhat API-k tooexpose alkalmazás engedélyekkel. Egy alkalmazás engedélyt tooan alkalmazás egy szervezet rendszergazdája, és a használt csak tooaccess adatok tartozhat, hogy a szervezet és az alkalmazottait. Például a Microsoft Graph több alkalmazás engedélyek toodo hello következő mutatja:

* E-mailek olvasása az összes postaládában
* E-mailek olvasása és írása az összes postaládában
* E-mailek küldése bármely felhasználó nevében
* Címtáradatok olvasása

Alkalmazásengedélyek kapcsolatos további információkért lépjen túl[Microsoft Graph](https://graph.microsoft.io).

az alkalmazás toouse Alkalmazásengedélyek hello hello következő szakaszokban arról lesz szó lépéseket.

#### Hello app regisztrációs portálon hello engedélyek kéréséhez
1. Nyissa meg a hello tooyour alkalmazás [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy [hozzon létre egy alkalmazást](active-directory-v2-app-registration.md), ha még nem tette meg. Toouse szüksége lesz legalább egy alkalmazás titkos kulcs az alkalmazás létrehozásakor.
2. Keresse meg a hello **közvetlen Alkalmazásengedélyek** szakaszt, és adja hozzá a hello az engedélyeket, az alkalmazás használatához.
3. **Mentés** hello alkalmazás regisztrációja.

#### Ajánlott: Bejelentkezési hello felhasználói tooyour alkalmazásban
Általában Alkalmazásengedélyek használó alkalmazások készítéséhez hello alkalmazás megköveteli lapon vagy a mely hello admin jóvá hello app engedélyek nézetben. Ezen a lapon lehet hello app bejelentkezési folyamata, hello app beállításokban része, vagy egy dedikált "Csatlakozás" folyamat lehet. Sok esetben érdemes a hello app tooshow a "Csatlakozás" nézetben, csak azt követően a felhasználó a munkahelyi vagy iskolai Microsoft-fiókkal van bejelentkezve.

Tooyour alkalmazásban hello felhasználó bejelentkezik, ha hello szervezet kiválaszthat toowhich hello felhasználói előtt tegye fel hello felhasználói tooapprove hello Alkalmazásengedélyek tartozik. Bár nem feltétlenül szükséges, ez segíthet a felhasználók intuitívabb környezetet. toosign hello felhasználó, hajtsa végre a [v2.0 protokoll oktatóanyagok](active-directory-v2-protocols.md).

#### Hello engedélyeket kérhet a directory-rendszergazda
Amikor készen áll a toorequest engedélyek hello szervezetének rendszergazdájával, átirányítja a hello felhasználói toohello v2.0 *rendszergazda jóváhagyását végpont*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting hello following request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Paraméter | Az állapot | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |hello directory-bérlőt, amelyet toorequest engedélyt. Ez lehet GUID vagy rövid név formátumban. Ha nem tudja hello bérlői felhasználói tartozik azt szeretné, hogy azok jelentkezzen be minden bérlő, használjon toolet tooand `common`. |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tooyour app rendelve. |
| redirect_uri |Szükséges |hello átirányítási URI, ahol azt szeretné, hogy az alkalmazás toohandle küldött hello válasz toobe. Az pontosan egyeznie kell hello átirányítási URI-azonosítók regisztrált hello portálon, azzal a különbséggel, hogy az URL-kódolású kell lennie, és további szegmenst veheti fel. |
| state |Ajánlott |Egy érték, amely része a hello kérelem hello token válaszul is visszaadott. Bármely, a kívánt tartalmat karakterlánc lehet. hello állapota hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |

Ezen a ponton az Azure AD kényszeríti annak engedélyezése, hogy csak a bérlői rendszergazda toocomplete hello kérelmet tud bejelentkezni. hello rendszergazda összes hello közvetlen Alkalmazásengedélyek kért az alkalmazáshoz a hello app regisztrációs portál tooapprove meg kell adnia.

##### A sikeres válasz
Üdvözöljük a rendszergazdákat jóvá hello engedélyek az alkalmazáshoz, ha a sikeres válasz hello néz ki:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Paraméter | Leírás |
| --- | --- | --- |
| Bérlői |hello directory-bérlő tartozik, amely engedéllyel rendelkezik az alkalmazás hello, amely azt kérte, GUID formátumban. |
| state |Egy érték, amely része a hello kérelem hello token válaszul is visszaadott. Bármely, a kívánt tartalmat karakterlánc lehet. hello állapota hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |
| admin_consent |Állítsa be a túl**igaz**. |

##### Hibaválaszba
Üdvözöljük a rendszergazdákat nem hagyja jóvá az alkalmazás hello engedélyek, ha a hello válasz néz sikertelen volt:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Paraméter | Leírás |
| --- | --- | --- |
| error |Egy hiba kód karakterlánc használható tooclassify típusok hibákat, és amelyek tooreact tooerrors használhatja. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hiba okának azonosításához. |

A sikeres válasz a hello alkalmazás üzembe helyezési végpont már fogadását követően az alkalmazás köszönhetően a kért hello közvetlen Alkalmazásengedélyek. Most hello erőforrás, amelyet egy jogkivonatot kérhet.

## A jogkivonat beolvasása
Ha hello szükséges engedélyezési jut hozzá az alkalmazáshoz, lépjen a hozzáférési tokenek beszerzése API-k esetében. hello ügyfél hitelesítő adatok megadása a jogkivonat tooget küldése egy POST kérést toohello `/token` v2.0-végponttal:

### Először. eset: egy közös titkot a hozzáférési token kérelem

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Paraméter | Az állapot | Leírás |
| --- | --- | --- |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tooyour app rendelve. |
| Hatókör |Szükséges |hello értéket kapott hello `scope` a kérésben paraméternek kell lennie a hello erőforrás-azonosítója (alkalmazás azonosítója URI) kívánt, a hello elhelyezni hello erőforrás `.default` utótag. Például hello Microsoft Graph hello értéke `https://graph.microsoft.com/.default`. Ez az érték tájékoztatja arról, hogy az összes hello közvetlen alkalmazás engedélyek beállítása az alkalmazáshoz konfigurált, azt kell ki hello kívánt hello erőforrás társított megfelelően a jogkivonat toouse hello v2.0-végponttól. |
| client_secret |Szükséges |az alkalmazás hello app regisztrációs portálon létrehozott Alkalmazáskulcsot hello. |
| grant_type |Szükséges |Kell `client_credentials`. |

### A második esetben: hozzáférési jogkivonat kérelem tanúsítvánnyal

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

| Paraméter | Az állapot | Leírás |
| --- | --- | --- |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tooyour app rendelve. |
| Hatókör |Szükséges |hello értéket kapott hello `scope` a kérésben paraméternek kell lennie a hello erőforrás-azonosítója (alkalmazás azonosítója URI) kívánt, a hello elhelyezni hello erőforrás `.default` utótag. Például hello Microsoft Graph hello értéke `https://graph.microsoft.com/.default`. Ez az érték tájékoztatja arról, hogy az összes hello közvetlen alkalmazás engedélyek beállítása az alkalmazáshoz konfigurált, azt kell ki hello kívánt hello erőforrás társított megfelelően a jogkivonat toouse hello v2.0-végponttól. |
| client_assertion_type |Szükséges |hello értéknek kell lennie`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Szükséges | Egy helyességi feltétel (egy JSON Web Token), hogy toocreate van szüksége, és a jel hello a tanúsítványok, az alkalmazás hitelesítő adatként regisztrálva. További információ a [tanúsítvány a hitelesítő adatok](active-directory-certificate-credentials.md) toolearn hogyan tooregister hello helyességi feltétel a tanúsítvány és hello formátuma.|
| grant_type |Szükséges |Kell `client_credentials`. |

Figyelje meg, hogy hello paraméterei szinte hello ugyanaz, mint hello hello kérelem által megosztott titkot azzal a különbséggel, hogy hello client_secret paraméter helyébe két paramétert: egy client_assertion_type és client_assertion.

### A sikeres válasz
A sikeres válasz így néz ki:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Paraméter | Leírás |
| --- | --- |
| access_token |hello kért hozzáférési jogkivonat. hello alkalmazást a védett erőforrások, például a Web API tooa token tooauthenticate toohello használhatja. |
| token_type |Azt jelzi, hogy hello token objektumtípus-érték. csak olyan típusú, amely az Azure AD által támogatott hello `bearer`. |
| expires_in |Mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |

### Hibaválaszba
Egy hiba történt egy válasz így néz ki:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely a előforduló hibákat, és tooreact tooerrors tooclassify típusok használhatók. |
| error_description |Egy adott hibaüzenet, amelyek segíthetnek hello hitelesítési hiba okának azonosításához. |
| error_codes |Diagnosztika segíthet STS-specifikus hibakódok listáját. |
| időbélyeg |hello idő hello hiba lépett fel. |
| trace_id |Hello kérelem, amelyek segíthetnek a diagnosztika egyedi azonosítója. |
| correlation_id |Hello kérelem, amelyek segíthetnek a diagnosztika összetevői között egyedi azonosítója. |

## Használja a tokent
Most, hogy jut hozzá a jogkivonatot, használja a hello token toomake kérelmek toohello erőforrás. Amikor hello-token érvényessége lejár, ismételje meg a hello kérelem toohello `/token` végpont tooacquire egy friss hozzáférési jogkivonat.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try hello following command! (Replace hello token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## Kódminta
egy alkalmazás például, hogy megvalósítja hello rendszergazda jóváhagyását végpont használatával ügyfél-hitelesítő adatok megadása hello toosee tekintse meg a [v2.0 démon kódminta](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

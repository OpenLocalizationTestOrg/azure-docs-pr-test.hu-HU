---
title: "OAuth-engedélyezési kód Flow aaaAzure AD v2.0 |} Microsoft Docs"
description: "Épület webes alkalmazásokat az Azure AD-megvalósítással hello OAuth 2.0 hitelesítési protokoll."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: dee58f2f5c627fef35cae279349728c3c7bf9421
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoll - OAuth 2.0 Hitelesítésikód-folyamata
egy eszköz toogain hozzáférés tooprotected-erőforrások, például webes API-k telepített alkalmazások hello OAuth 2.0 hitelesítésengedélyezési kód használható.  Az OAuth 2.0 hello app model v2.0-megvalósítással hozzáadhat bejelentkezés és API tooyour hordozható és asztali alkalmazások eléréséhez.  Ez az Útmutató nyelvtől független, és ismerteti, hogyan toosend és HTTP-üzenetek fogadása a nyílt forráskódú kódtárai bármelyikét használata nélkül.

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

hello OAuth 2.0 hitelesítésikód-folyamata a leírtak [hello OAuth 2.0-s szabvány 4.1 szakasz](http://tools.ietf.org/html/rfc6749).  Használt tooperform hitelesítési és engedélyezési alkalmazástípusok, beleértve a legtöbb hello [webalkalmazások](active-directory-v2-flows.md#web-apps) és [natívan telepített alkalmazásokat](active-directory-v2-flows.md#mobile-and-native-apps).  Ez lehetővé teszi a toosecurely megszerzése access_tokens, amely lehet használt tooaccess erőforrások hello v2.0-végponttól használatával védett alkalmazások.  

## Protokoll diagramja
Magas szinten a natív/mobile alkalmazás hello egész hitelesítési folyamat néz ki egy kicsit:

![OAuth hitelesítési folyamata](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## Az engedélyezési kód kérése
hello hitelesítésikód-folyamata kezdődik arra utasíthatja a hello felhasználói toohello hello ügyfél `/authorize` végpont.  Ebben a kérelemben hello ügyfél azt jelzi, a hello felhasználó tooacquire kell hello engedélyek:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Kattintson az alábbi tooexecute hello hivatkozását, ehhez a kérelemhez! Történő bejelentkezés után a böngésző átirányítsa túl`https://localhost/myapp/` rendelkező egy `code` hello címsorába.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett `common`, `organizations`, `consumers`, és a bérlői azonosítókat.  További részletekért lásd: [alapjai protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id |Szükséges |Alkalmazásazonosító, hogy hello regisztrációs portál hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) az alkalmazáshoz hozzárendelt. |
| response_type |Szükséges |Tartalmaznia kell `code` a hello hitelesítésikód-folyamata. |
| redirect_uri |Ajánlott |az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszok redirect_uri hello.  Azt pontosan egyeznie kell azzal a különbséggel url-kódolású kell lennie az hello portálon regisztrált hello redirect_uris.  Natív & mobileszköz-alkalmazások esetén használjon hello alapértelmezett értékének `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| Hatókör |Szükséges |Szóközökkel elválasztott listáját [hatókörök](active-directory-v2-scopes.md) , amelyet az hello felhasználói tooconsent számára. |
| response_mode |Ajánlott |Határozza meg, amelyeket használt toosend hello eredményül kapott jogkivonat hátsó tooyour app hello módszerét.  Lehet `query` vagy `form_post`. |
| state |Ajánlott |Hello token válaszul is visszaadott hello kérelemben szereplő érték.  Bármely, a kívánt tartalmat karakterlánc lehet.  Egy véletlenszerűen generált egyedi érték jellemzően a [webhelyközi kérések hamisításának megakadályozása támadások megelőzése](http://tools.ietf.org/html/rfc6749#section-10.12).  hello állapota is hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |
| parancssor |Nem kötelező |A felhasználói beavatkozás szükséges hello típusát jelzi.  hello csak most az érvényes értékek: "bejelentkezés", "none", és "".  `prompt=login`rendszer kényszerített hello felhasználói tooenter adott kérelem nem lehet negálni egyszeri bejelentkezést a hitelesítő adataikat.  `prompt=none`hello ellentétes - biztosítani fogja számára nem jelenik meg, hogy a felhasználó hello bármely minden interaktív kérdés.  Ha hello kérelem nem hajtható végre csendes keresztül egyszeri bejelentkezést, hello v2.0-végponttól hibát adnak vissza.  `prompt=consent`rendszer eseményindító hello OAuth hozzájárulás párbeszédpanel hello felhasználó jelentkezik be, miután hello felhasználói toogrant engedélyek toohello alkalmazást kéri. |
| login_hint |Nem kötelező |Kell használt toopre-kitöltés hello felhasználónév vagy e-mail cím mező hello a bejelentkezés lap hello felhasználó, ha tudja, hogy időben a felhasználónevét.  Gyakran alkalmazások ismételt hitelesítés, hogy már kivont hello felhasználónév előző bejelentkezés során fogja használni a paraméter használatával hello `preferred_username` jogcímek. |
| domain_hint |Nem kötelező |Egyike lehet `consumers` vagy `organizations`.  Ha tartalmazza, akkor kihagyja hello e-mail alapú felderítési folyamat, hogy a felhasználó végighalad a hello v2.0 bejelentkezési oldalára, ami tooa nagyobb jelentőséggel zökkenőmentes felhasználói élmény.  Gyakran alkalmazások használja ezt a paramétert ismételt hitelesítés során hello beolvasásával `tid` az előző bejelentkezés.  Ha hello `tid` jogcím értéke `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`.  Ellenkező esetben használja `domain_hint=organizations`. |

Ezen a ponton hello felhasználó fog tooenter kéri a hitelesítő adatokat és a teljes hello hitelesítés.  hello v2.0-végponttól is biztosítják, hogy hello a felhasználó hozzájárult hello jelzett toohello engedélyek `scope` lekérdezési paraméter.  Ha hello felhasználó nem hozzájárult tooany, ezeket az engedélyeket, a program kéri hello felhasználói tooconsent toohello szükséges engedélyekkel.  A részletek [engedélyek, beleegyezése és több-bérlős alkalmazásokhoz itt megadott](active-directory-v2-scopes.md).

Miután hello felhasználó hitelesíti, és engedélyezi a hozzájárulási hello v2.0-végponttól ad vissza egy válasz tooyour alkalmazást jelzett hello `redirect_uri`, hello megadott hello metódussal `response_mode` paraméter.

#### A sikeres válasz
A sikeres válasz használatával `response_mode=query` láthatóhoz hasonló:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Paraméter | Leírás |
| --- | --- |
| Kód |hello authorization_code, amely a kért alkalmazás hello. hello app hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használja hello cél erőforráson.  Authorization_codes nagyon rövid élettartamú, általában azok körülbelül 10 perc múlva lejár. |
| state |Ha a state paraméter hello kérelem, ugyanazt az értéket meg kell jelennie a hello válasz hello is szerepel. hello app győződjön meg arról, hogy a hello állapotértékek hello kérés és válasz megegyeznek-e. |

#### Hibaválaszba
Hibaválaszok is küldhet toohello `redirect_uri` , ezek a hello alkalmazást is megfelelően kezelése:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |

#### Engedélyezési végpont hibái hibakódok
hello következő táblázatban található hello különböző hibakódok hello visszaküldött `error` hello hibaválaszba paramétere.

| Hibakód: | Leírás | Ügyfélművelet |
| --- | --- | --- |
| invalid_request |Protokollhiba történt, például a hiányzó kötelező paraméter. |Javítsa ki, és küldje el újra hello kérelmet. Ez az fejlesztői hiba általában kezdeti tesztelés során lépett fel. |
| unauthorized_client |hello ügyfélalkalmazás nincs engedélyezett toorequest egy engedélyezési kódot. |Ez általában akkor fordul elő, amikor hello ügyfélalkalmazás nincs regisztrálva az Azure ad-ben, vagy az Azure AD-bérlő toohello felhasználó nincs hozzáadva. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |
| ACCESS_DENIED |Erőforrás tulajdonosa hozzájárulási megtagadva |hello ügyfélalkalmazás értesítheti hello felhasználó számára, akkor nem lehet folytatni, kivéve, ha hello felhasználó. |
| unsupported_response_type |hello engedélyezési kiszolgáló nem támogatja a hello választípus hello kérelemben. |Javítsa ki, és küldje el újra hello kérelmet. Ez az fejlesztői hiba általában kezdeti tesztelés során lépett fel. |
| server_error |hello kiszolgáló váratlan hibát észlelt. |Ismételje meg a hello kérelmet. Ezeket a hibákat okozhat az átmeneti állapotot. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy egy ideiglenes hiba miatt késik a válaszában. |
| temporarily_unavailable |hello kiszolgáló egy ideiglenesen túlterhelt toohandle hello kérelem. |Ismételje meg a hello kérelmet. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy egy ideiglenes állapot miatt késik a válaszában. |
| invalid_resource |hello célerőforrása érvénytelen, mert nem létezik, az Azure AD a fájl nem található vagy nincs megfelelően konfigurálva. |Ez azt jelzi, hogy hello erőforrás, ha létezik, nem konfiguráltak hello bérlő. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |

## A kérelem egy hozzáférési jogkivonatot:
Most, hogy egy authorization_code jut hozzá, és engedélyt kaptak hello felhasználó, akkor is beválthatja hello `code` a egy `access_token` toohello szükséges erőforrás, úgy, hogy küld egy `POST` toohello kérelem `/token` végpont:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Próbálja meg a kérelem végrehajtása a Postman! (Ne feledje tooreplace hello `code`) [ ![Postman futtatása](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett `common`, `organizations`, `consumers`, és a bérlői azonosítókat.  További részletekért lásd: [alapjai protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id |Szükséges |Alkalmazásazonosító, hogy hello regisztrációs portál hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) az alkalmazáshoz hozzárendelt. |
| grant_type |Szükséges |Kell `authorization_code` a hello hitelesítésikód-folyamata. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája.  a kért hello hatókörök Ez engedélyezi a kell kell egyenértékű tooor hello hatókörök részhalmazának kért hello első engedélyezi.  A kérelemben megadott hello hatókörök span több erőforrás-kiszolgálóit, ha hello v2.0-végponttól vissza hello első hatókörében meghatározott hello erőforrás jogkivonatot.  A hatókörök részletes ismertetése, tekintse meg túl[engedélyek, beleegyezése és hatókörök](active-directory-v2-scopes.md). |
| Kód |Szükséges |hello authorization_code hello folyamat első szakasza hello beszerzett. |
| redirect_uri |Szükséges |hello használt tooacquire hello authorization_code lett redirect_uri értéket. |
| client_secret |a web Apps szükséges |az alkalmazás hello app regisztrációs portálon létrehozott hello alkalmazáskulcsot.  Akkor kell nem használható natív alkalmazás, mert client_secrets megbízhatóan nem tárolható az eszközökön.  Akkor szükséges a webalkalmazások és webes API-k, amelyeknek hello képességét toostore hello client_secret biztonságosan hello kiszolgáló oldalán. |

#### A sikeres válasz
A sikeres token válasz hasonlóan fog kinézni:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Paraméter | Leírás |
| --- | --- |
| access_token |hello kért hozzáférési jogkivonat. hello alkalmazást a védett erőforrások, például a webes API-k token tooauthenticate toohello használhatja. |
| token_type |Azt jelzi, hogy hello token objektumtípus-érték. hello csak olyan típus, amely támogatja az Azure AD: tulajdonosi |
| expires_in |Mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| Hatókör |hello access_token hello hatókörök esetén érvényes. |
| refresh_token |Az OAuth 2.0-s frissítési jogkivonat. hello app használhatók a token szerezzen be további hozzáférési jogkivonatok hello aktuális hozzáférési jogkivonat lejárata után is.  Refresh_tokens hosszú élettartamú, és hosszabb ideig használt tooretain hozzáférés tooresources lehet.  További részletekért tekintse meg a toohello [v2.0 jogkivonat referenciái](active-directory-v2-tokens.md). |
| id_token |Az aláírás nélküli JSON webes jogkivonat (JWT). hello alkalmazást is base64Url hello szegmensek a bejelentkezett felhasználó hello token toorequest információinak dekódolására. hello app hello értékek gyorsítótárazása és megjeleníteni azokat is, de azt nem igazolható a azokat bármilyen engedélyezési vagy a biztonsági határokat.  További információ a id_tokens: hello [v2.0 jogkivonat végponthivatkozás](active-directory-v2-tokens.md). |

#### Hibaválaszba
Hibaválaszok hasonlóan fog kinézni:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |
| error_codes |Amelyek segítik a diagnosztika STS adott hibakódok listáját. |
| időbélyeg |hello idő hello hiba lépett fel. |
| trace_id |Hello kérelem, amelyek segítik a diagnosztika egyedi azonosítója. |
| correlation_id |Hello kérelem, amelyek segítik a diagnosztika összetevői között egyedi azonosítója. |

#### A token-végpont hibákat hibakódok
| Hibakód: | Leírás | Ügyfélművelet |
| --- | --- | --- |
| invalid_request |Protokollhiba történt, például a hiányzó kötelező paraméter. |Javítsa ki, és küldje el újra a kérelmet hello |
| invalid_grant |hello engedélyezési kód érvénytelen vagy lejárt. |Próbálja meg egy új kérelmet toohello `/authorize` végpont |
| unauthorized_client |hello hitelesített az ügyfél nem jogosult toouse az engedélyezés támogatás típusa. |Ez általában akkor fordul elő, amikor hello ügyfélalkalmazás nincs regisztrálva az Azure ad-ben, vagy az Azure AD-bérlő toohello felhasználó nincs hozzáadva. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |
| invalid_client |Nem sikerült ügyfél-hitelesítéshez. |hello ügyfél hitelesítő adatok érvénytelenek. toofix, hello alkalmazás-rendszergazda frissítése hello hitelesítő adatokat. |
| unsupported_grant_type |hello engedélyezési kiszolgáló nem támogatja a hello engedélyezési engedélytípus. |Változás hello támogatás hello kérelem típusa. Hiba az ilyen típusú csak a fejlesztés során megtörténik, és a kezdeti tesztelés során észlelt. |
| invalid_resource |hello célerőforrása érvénytelen, mert nem létezik, az Azure AD a fájl nem található vagy nincs megfelelően konfigurálva. |Ez azt jelzi, hogy hello erőforrás, ha létezik, nem konfiguráltak hello bérlő. hello alkalmazás kérheti a hello utasítás hello alkalmazás telepítése és tooAzure AD hozzáadni a felhasználót. |
| interaction_required |hello kérelem felhasználói beavatkozást igényel. Például egy további hitelesítési lépésre szükség. |Próbálja megismételni a kérelmet hello hello ugyanazt az erőforrást. |
| temporarily_unavailable |hello kiszolgáló egy ideiglenesen túlterhelt toohandle hello kérelem. |Ismételje meg a hello kérelmet. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy egy ideiglenes állapot miatt késik a válaszában. |

## Hello hozzáférési jogkivonat használata
Most, hogy sikeresen jut hozzá egy `access_token`, használható hello token kérelmek tooWeb API-k belefoglalja azt hello `Authorization` fejléc:

> [!TIP]
> A kérelem végrehajtása a Postman! (Cserélje le a hello `Authorization` fejléc első) [ ![Postman futtatása](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## Hello hozzáférési jogkivonat frissítése
Access_tokens rövid élt, és frissítenie kell őket toocontinue erőforrások elérése után.  Megteheti egy másik elküldése `POST` toohello kérelem `/token` végpont, így hello most `refresh_token` helyett hello `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Próbálja meg a kérelem végrehajtása a Postman! (Ne feledje tooreplace hello `refresh_token`) [ ![Postman futtatása](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett `common`, `organizations`, `consumers`, és a bérlői azonosítókat.  További részletekért lásd: [alapjai protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id |Szükséges |Alkalmazásazonosító, hogy hello regisztrációs portál hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) az alkalmazáshoz hozzárendelt. |
| grant_type |Szükséges |Kell `refresh_token` hello hitelesítésikód-folyamata ezen szakasza az. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája.  a kért hello hatókörök Ez engedélyezi a kell kell egyenértékű tooor hello hatókörök részhalmazának kért hello eredeti authorization_code kérést engedélyezi.  A kérelemben megadott hello hatókörök span több erőforrás-kiszolgálóit, ha hello v2.0-végponttól vissza hello első hatókörében meghatározott hello erőforrás jogkivonatot.  A hatókörök részletes ismertetése, tekintse meg túl[engedélyek, beleegyezése és hatókörök](active-directory-v2-scopes.md). |
| refresh_token |Szükséges |hello refresh_token hello folyamat második szakasza hello beszerzett. |
| redirect_uri |Szükséges |hello használt tooacquire hello authorization_code lett redirect_uri értéket. |
| client_secret |a web Apps szükséges |az alkalmazás hello app regisztrációs portálon létrehozott hello alkalmazáskulcsot.  Akkor kell nem használható natív alkalmazás, mert client_secrets megbízhatóan nem tárolható az eszközökön.  Akkor szükséges a webalkalmazások és webes API-k, amelyeknek hello képességét toostore hello client_secret biztonságosan hello kiszolgáló oldalán. |

#### A sikeres válasz
A sikeres token válasz hasonlóan fog kinézni:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Paraméter | Leírás |
| --- | --- |
| access_token |hello kért hozzáférési jogkivonat. hello alkalmazást a védett erőforrások, például a webes API-k token tooauthenticate toohello használhatja. |
| token_type |Azt jelzi, hogy hello token objektumtípus-érték. hello csak olyan típus, amely támogatja az Azure AD: tulajdonosi |
| expires_in |Mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| Hatókör |hello access_token hello hatókörök esetén érvényes. |
| refresh_token |Egy új OAuth 2.0-s frissítési jogkivonat. Hello régi frissítési le kell cserélni az újonnan megvásárolt frissítési jogkivonat tooensure jogkivonat a frissítési jogkivonatokat maradnak, amíg érvényes. |
| id_token |Az aláírás nélküli JSON webes jogkivonat (JWT). hello alkalmazást is base64Url hello szegmensek a bejelentkezett felhasználó hello token toorequest információinak dekódolására. hello app hello értékek gyorsítótárazása és megjeleníteni azokat is, de azt nem igazolható a azokat bármilyen engedélyezési vagy a biztonsági határokat.  További információ a id_tokens: hello [v2.0 jogkivonat végponthivatkozás](active-directory-v2-tokens.md). |

#### Hibaválaszba
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: hello provided value for hello input parameter 'scope' is not valid. hello scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |
| error_codes |Amelyek segítik a diagnosztika STS adott hibakódok listáját. |
| időbélyeg |hello idő hello hiba lépett fel. |
| trace_id |Hello kérelem, amelyek segítik a diagnosztika egyedi azonosítója. |
| correlation_id |Hello kérelem, amelyek segítik a diagnosztika összetevői között egyedi azonosítója. |

Hello hibakódokat és a javasolt művelet ügyfél hello ismertetését lásd: [token-végpont hibák hibakódok](#error-codes-for-token-endpoint-errors).


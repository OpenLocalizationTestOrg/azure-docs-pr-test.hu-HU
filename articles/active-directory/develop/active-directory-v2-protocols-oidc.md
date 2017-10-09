---
title: "az Active Directory v2.0 és hello OpenID Connect protokoll aaaAzure |} Microsoft Docs"
description: "A webalkalmazások hello Azure AD v2.0 végrehajtásának hello OpenID Connect hitelesítési protokoll használatával."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 277bb406dbb24ccefb09e4e4c928f0421755cf61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Az Azure Active Directory v2.0 és hello OpenID Connect protokoll
OpenID Connect egy olyan hitelesítési protokoll, használható toosecurely bejelentkezési felhasználói tooa webalkalmazás az OAuth 2.0 épül. Hello v2.0 endpoint OpenID Connect végrehajtásának használatakor be- és API access tooyour web alapú alkalmazásokra is hozzáadhat. Ez a cikk azt mutatja be ez független nyelv toodo. Azt ismerteti, hogy hogyan toosend és HTTP-üzenetek fogadása a Microsoft nyílt forráskódú kódtárai használata nélkül.

> [!NOTE]
> hello v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók. toodetermine kell használnia a hello v2.0-végpontra, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) kibővíti az OAuth 2.0 hello *engedélyezési* protokoll toouse, mint egy *hitelesítési* protokollt, így egyetlen hajthat végre bejelentkezés OAuth protokollt használó. OpenID Connect bemutatja hello egy *azonosító token*, amelyen egy biztonsági jogkivonatot, amely lehetővé teszi, hogy a hello ügyfél tooverify hello identitás hello felhasználó. hello azonosító token is hello felhasználó alapvető profiladataihoz adatait lekérdezi. OAuth 2.0 OpenID Connect terjeszti ki, mert az alkalmazások biztonságos lekérheti *hozzáférési jogkivonatok*, amely lehet által védett erőforrások használt tooaccess egy [engedélyezési server](active-directory-v2-protocols.md#the-basics). Javasoljuk, hogy használjon OpenID Connect készítésekor egy [webes alkalmazás](active-directory-v2-flows.md#web-apps) , a kiszolgáló által üzemeltetett és böngészőalapú hozzáférést biztosít.

## Protokoll diagramja: bejelentkezés
hello legalapvetőbb bejelentkezési folyamata rendelkezik hello lépéseket hello a következő ábrán is látható. Azt írja le a részletes információkat ebben a cikkben minden lépést.

![OpenID Connect protokoll: bejelentkezés](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## Hello OpenID Connect metaadat-dokumentum beolvasása
A metaadat-dokumentum, egy alkalmazás tooperform bejelentkezés szükséges hello adatok többségét tartalmazó OpenID Connect ismerteti. Ide tartoznak hello URL-címek toouse és hello szolgáltatást nyilvános aláírási kulcsok hello helyét. Hello v2.0-végpontra ez pedig hello OpenID Connect metaadat-dokumentum kell használnia:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```

Hello `{tenant}` négy értékek valamelyikét hajthatja végre:

| Érték | Leírás |
| --- | --- |
| `common` |A személyes Microsoft-fiókkal, és a munkahelyi vagy iskolai fiókkal az Azure Active Directory (Azure AD) felhasználó tud egyszerre bejelentkezni toohello alkalmazás. |
| `organizations` |Csak a felhasználók munkahelyi vagy iskolai fiókok Azure ad-bejelentkezés toohello alkalmazás. |
| `consumers` |Csak a személyes Microsoft-fiókkal rendelkező felhasználók bejelentkezhetnek toohello alkalmazás. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` |Csak a felhasználók a munkahelyi vagy iskolai fiókkal, egy adott Azure ad-bérlő toohello alkalmazás tud bejelentkezni. Hello rövid tartományneve hello Azure AD-bérlő vagy hello bérlői GUID-azonosító is használható. |

hello metaadatai egy egyszerű JavaScript Object Notation (JSON) dokumentumot. Tekintse meg a következő példa részlet hello. hello részlet tartozó tartalom teljes ismertetett hello [OpenID Connect specification](https://openid.net).

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/common\/discovery\/v2.0\/keys",

  ...

}
```

Általában használhatja a metaadat-dokumentum tooconfigure az OpenID Connect tár vagy SDK; hello könyvtár használna hello metaadatok toodo teendőit. Azonban nem használata OpenID Connect könyvtár előtti, lépésekkel hello hello további része a cikk tooperform bejelentkezés a webes alkalmazás a v2.0-végponttól hello segítségével.

## Hello bejelentkezési kérelem küldése
Ha a webalkalmazás tooauthenticate hello felhasználó, azt is közvetlen hello felhasználói toohello `/authorize` végpont. Ehhez a kérelemhez hasonló toohello első szakasza hello [OAuth 2.0 hitelesítésikód-folyamata](active-directory-v2-protocols-oauth-code.md), az alábbi fontos különbség:

* hello kérelem tartalmaznia kell hello `openid` hello hatókör `scope` paraméter.
* Hello `response_type` paraméternek tartalmaznia kell `id_token`.
* hello kérelem tartalmaznia kell hello `nonce` paraméter.

Példa:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Kattintson a következő hivatkozás tooexecute hello ehhez a kérelemhez. Miután bejelentkezik, a böngésző lesz átirányított toohttps://localhost/myapp/ hello címsor Azonosítóját tokenhez. Vegye figyelembe, hogy ehhez a kérelemhez használt `response_mode=query` (bemutatási célokra csak). Azt javasoljuk, hogy használjon `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=query&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Paraméter | Az állapot | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Használhatja a hello `{tenant}` hello elérési útja hello kérelem toocontrol, akik toohello alkalmazás bejelentkezés érték. hello két érték engedélyezett `common`, `organizations`, `consumers`, és a bérlői azonosítókat. További információkért lásd: [alapjai protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tooyour app rendelve. |
| response_type |Szükséges |Tartalmaznia kell `id_token` az OpenID Connect bejelentkezhet. Is tartalmazhat más `response_types` értékek, például a `code`. |
| redirect_uri |Ajánlott |hello átirányítási URI-címe, az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszokat. Azt pontosan egyeznie kell azzal a különbséggel, hogy az URL-címet kell lennie az hello portálon regisztrált URI-kódolású hello átirányítási. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája. Az OpenID Connect, magában kell foglalnia hello hatókör `openid`, amely az eszköz toohello hello hozzájárulási felhasználói felület "Bejelentkezés" engedéllyel. A kérésben a hozzájárulás kérése kiterjedhetnek más hatókörök. |
| Nonce |Szükséges |Hello kérelmet, amely szerepelni fog a hello kapott id_token érték jogcímként hello alkalmazás által generált szerepel érték. hello app ellenőrizheti a érték toomitigate hitelesítési karakterláncok ismétlésének támadásokat. hello érték jellemzően a véletlenszerű, egyedi karakterlánc, amely hello kérelem használt tooidentify hello forrása is lehet. |
| response_mode |Ajánlott |Határozza meg, amelyeket használt toosend hello eredményül kapott engedélyezési kód hátsó tooyour app hello módszerét. Egyike lehet `query`, `form_post`, vagy `fragment`. Webes alkalmazásokhoz, javasoljuk `response_mode=form_post`, tooensure hello jogkivonatok tooyour alkalmazás a legbiztonságosabb átvitelét. |
| state |Ajánlott |Hello kérelem is visszaadott hello biztonságijogkivonat-válaszban szereplő érték. Bármilyen tartalmat karakterlánc lehet. Egy véletlenszerűen generált egyedi érték jellemzően akkor alkalmazzák túl[webhelyközi kérések hamisításának megakadályozása támadások megelőzése érdekében](http://tools.ietf.org/html/rfc6749#section-10.12). hello is állapota hello felhasználói állapot hello alkalmazásban használt tooencode információ előtt hello hitelesítési kérelmet, például a hello lap vagy nézet hello felhasználói volt. |
| parancssor |Optional |A felhasználói beavatkozás szükséges hello típusát jelzi. hello csak érvényes jelenleg értékei `login`, `none`, és `consent`. Hello `prompt=login` jogcím kényszeríti hello felhasználói tooenter adott kérelem, amely az egyszeri bejelentkezés ellentettjét adja a hitelesítő adataikat. Hello `prompt=none` ellentétes hello nincs. A jogcím biztosítja számára nem jelenik meg, hogy a felhasználó hello bármely minden interaktív kérdés. Hello kérelem nem hajtható végre csendes keresztül egyszeri bejelentkezés, ha a v2.0-végponttól hello hibát ad vissza. Hello `prompt=consent` jogcím hello felhasználó bejelentkezése után az eseményindítók hello az OAuth-hozzájárulás párbeszédpanel. hello párbeszédpanel rákérdez hello felhasználói toogrant engedélyek toohello alkalmazást. |
| login_hint |Optional |Használhatja a paraméter toopre-kitöltés hello felhasználónevet és e-mail cím mező hello bejelentkezési oldal hello felhasználó esetében, ha tudja, hogy időben hello felhasználónév. Gyakran, alkalmazásokat a paraméter használata során ismételt hitelesítés után már kibontása hello felhasználónév egy korábbi bejelentkezés hello segítségével `preferred_username` jogcímek. |
| domain_hint |Optional |Az érték lehet `consumers` vagy `organizations`. Ha tartalmazza, akkor kihagyja hello e-mail alapú felderítési folyamat hello felhasználó végighalad a hello v2.0 bejelentkezési oldalára, hogy a nagyobb jelentőséggel zökkenőmentes felhasználói élmény. Gyakran, alkalmazások használja ezt a paramétert ismételt hitelesítés során hello beolvasásával `tid` jogcím hello azonosító jogkivonatból. Ha hello `tid` jogcím értéke `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`. Ellenkező esetben használja `domain_hint=organizations`. |

Ezen a ponton hello felhasználói felszólító tooenter van, a hitelesítő adatokat és a teljes hello hitelesítés. hello v2.0-végponttól ellenőrzi, hogy hello a felhasználó hozzájárult hello jelzett toohello engedélyek `scope` lekérdezési paraméter. Ha hello felhasználó nem hozzájárult tooany, ezeket az engedélyeket, a v2.0-végponttól hello hello felhasználói tooconsent toohello szükséges engedélyek megadását kéri. További tudnivalók [engedélyek, beleegyezése és több-bérlős alkalmazásokhoz](active-directory-v2-scopes.md).

Miután hello felhasználó hitelesíti, és engedélyezi a hozzájárulási hello v2.0 végpont adja vissza egy válasz tooyour alkalmazást hello jelzett átirányítási URI hello megadott hello módszer használatával `response_mode` paraméter.

### A sikeres válasz
A sikeres válasz használatakor `response_mode=form_post` dolgozunk:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Paraméter | Leírás |
| --- | --- |
| id_token |kért alkalmazás hello Azonosítót jogkivonatban hello. Használhatja a hello `id_token` paraméter tooverify hello a felhasználó identitását, és a hello felhasználói munkamenet elindításához. Azonosító-jogkivonatokat és azok tartalmát kapcsolatos további tudnivalókért lásd: hello [v2.0-végponttól jogkivonatok hivatkozás](active-directory-v2-tokens.md). |
| state |Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy a hello állapotértékek hello kérés és válasz megegyeznek-e. |

### Hibaválaszba
Hibaválaszok is lehet küldeni toohello átirányítási URI-t, hogy hello alkalmazást is kezeli azokat. Egy hiba történt egy válasz így néz ki:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely a előforduló hibákat, és tooreact tooerrors tooclassify típusok használhatók. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hitelesítési hiba okának azonosításához. |

### Engedélyezési végpont hibái hibakódok
hello következő táblázat azokat a hibakódokat ismerteti hello visszaküldött `error` hello hiba válasz paraméter:

| Hibakód: | Leírás | Ügyfélművelet |
| --- | --- | --- |
| invalid_request |Protokollhiba történt, például egy hiányzó kötelező paraméter. |Javítsa ki, és küldje el újra hello kérelmet. Ez a kezdeti tesztelés során általában kiszűri a fejlesztési hiba. |
| unauthorized_client |hello ügyfélalkalmazás egy engedélyezési kód nem lehet kérni. |Ez általában akkor fordul elő, amikor hello ügyfélalkalmazás nincs regisztrálva az Azure ad-ben, vagy az Azure AD-bérlő toohello felhasználó nincs hozzáadva. hello alkalmazás hello felhasználónak utasításokat tooinstall hello alkalmazással, és tooAzure AD hozzá. |
| ACCESS_DENIED |hello erőforrás tulajdonosát a megtagadott hozzájárulásukat adják. |hello ügyfélalkalmazás értesítheti hello felhasználó számára, akkor nem lehet folytatni, kivéve, ha hello felhasználó. |
| unsupported_response_type |hello engedélyezési kiszolgáló nem támogatja a hello választípus hello kérelemben. |Javítsa ki, és küldje el újra hello kérelmet. Ez a kezdeti tesztelés során általában kiszűri a fejlesztési hiba. |
| server_error |hello kiszolgáló váratlan hibát észlelt. |Ismételje meg a hello kérelmet. Ezeket a hibákat okozhat az átmeneti állapotot. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy válaszában késleltetett tooa ideiglenes hiba miatt. |
| temporarily_unavailable |hello kiszolgáló egy ideiglenesen túlterhelt toohandle hello kérelem. |Ismételje meg a hello kérelmet. hello ügyfélalkalmazás toohello felhasználói elmagyarázhatja, hogy válaszában tooa ideiglenes állapot miatt késik. |
| invalid_resource |hello célerőforrása érvénytelen, mert nem létezik, az Azure AD a fájl nem található vagy nincs megfelelően konfigurálva. |Ez azt jelzi, hogy hello erőforrás, ha létezik, nem konfiguráltak hello bérlő. hello alkalmazás kérheti a hello felhasználói hello alkalmazás telepítése, majd hozzáadásával tooAzure AD útmutatást. |

## Hello azonosító token ellenőrzése
Egy azonosító tokent fogadása nincs elegendő tooauthenticate hello felhasználó. Hello azonosító jogkivonat-aláírás ellenőrzése és ellenőrizze az alkalmazás követelményeinek / hello jogkivonat hello jogcímek is. hello v2.0-végponttól használ [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) és nyilvános kulcs titkosítás toosign jogkivonatokat, és győződjön meg arról, hogy azok érvényesek.

Ügyfélkód toovalidate hello azonosító token választhatja, azonban a gyakori eljárás toosend hello azonosító token tooa háttér-kiszolgálófiók és hello ellenőrzéshez van. Hello azonosító jogkivonat hello aláírás érvényesítése után kell tooverify néhány jogcímeket. További információt, beleértve a több kapcsolatos [jogkivonatok ellenőrzése](active-directory-v2-tokens.md#validating-tokens) és [fontos információkat a kulcsváltás](active-directory-v2-tokens.md#validating-tokens), lásd: hello [v2.0 jogkivonatok hivatkozás](active-directory-v2-tokens.md). Azt javasoljuk, hogy a szalagtár tooparse használja, és érvényesíthet jogkivonatokat. Van legalább egy ezeket a könyvtárakat legtöbb nyelvekhez és platformokhoz érhető el.
<!--TODO: Improve hello information on this-->

Érdemes azt is toovalidate további jogcímeket, a forgatókönyvtől függően. Néhány gyakori érvényesítést a következők:

* Győződjön meg arról, hogy hello felhasználó vagy a szervezet rendelkezik-höz készült hello alkalmazás.
* Győződjön meg arról, hogy a felhasználó hello szükséges engedélyezési vagy jogosultságokra.
* Győződjön meg arról, hogy bizonyos erősségével hitelesítési történt, például a többtényezős hitelesítést.

Egy Azonosítót jogkivonatban hello jogcímek kapcsolatos további információkért lásd: hello [v2.0-végponttól jogkivonatok hivatkozás](active-directory-v2-tokens.md).

Teljesen érvényesítése hello azonosító token után megkezdheti a munkamenet hello felhasználóval. Hello jogcímek használata a hello azonosító token tooget hello felhasználó az alkalmazás adatait. A kijelző, rekordok, engedélyek és így tovább használhatja ezeket az információkat.

## Kijelentkezési kérés küldése
Ha azt szeretné, toosign hello felhasználói ki az alkalmazásból, nincs elegendő tooclear az alkalmazás cookie-kat, vagy ellenkező esetben a hello felhasználói munkamenet befejezése. Hello felhasználói toohello v2.0 végpont toosign ki is irányíthatja át. Ha nem így tesz, hello felhasználó újra hitelesíti tooyour app a hitelesítő adatok újbóli megadása nélkül, mert egy érvényes egyszeri bejelentkezési munkamenet hello v2.0-végponttal rendelkeznek.

Hello felhasználói toohello átirányíthatók `end_session_endpoint` hello OpenID Connect metaadat-dokumentum szerepel:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Paraméter | Az állapot | Leírás |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Ajánlott | felhasználói hello hello URL-címe sikeresen kijelentkezés átirányított tooafter. Ha hello paraméter nincs megadva, hello felhasználói hello v2.0-végponttól által létrehozott általános üzenet jelenik meg. Az URL-cím hello átirányítási URI-azonosítók hello app regisztrációs portálra az alkalmazás regisztrálva egyikét meg kell egyeznie.  |

## Egyszeri kijelentkezés
Ha átirányítja a hello felhasználói toohello `end_session_endpoint`, hello v2.0-végponttól hello felhasználói munkamenet hello böngészőből törli. Hello felhasználó továbbra is köthető, amely a hitelesítéshez használandó Microsoft-fiókok tooother alkalmazások. tooenable ezen alkalmazások toosign hello felhasználói ki egyszerre, hello v2.0-végponttól küld egy HTTP GET kérést toohello regisztrált `LogoutUrl` alkalmazások hello adott hello jelenleg bejelentkezett felhasználó számára. Alkalmazások válaszolnia kell az munkamenet hello felhasználói törlését és azonosító törlésével toothis kérelem egy `200` választ.  Ha toosupport egyszeri bejelentkezési ki kívánja az alkalmazást, akkor meg kell valósítania például egy `LogoutUrl` az alkalmazás kódjában.  Beállíthatja a hello `LogoutUrl` hello app regisztrációs portálon.

## Protokoll diagramja: lexikális elem az beszerzése
Sok webalkalmazások toonot kizárólag egyetlen bejelentkezési hello felhasználó, hanem tooaccess egy webszolgáltatás-bővítmény hello felhasználó nevében kell OAuth használatával. Ebben a forgatókönyvben egyesíti az OpenID Connect engedélyezési egyidejűleg beolvasásakor a felhasználói hitelesítés használható tooget hozzáférési kód jogkivonatok hello OAuth hitelesítésikód-folyamata használata.

teljes OpenID Connect-bejelentkezés hello és token beszerzési folyamata hasonló toohello következő diagram jelenik meg. Azt ismerteti, hogy egyes részletei a következő szakaszokban hello hello cikk lépéseit.

![OpenID Connect protokoll: lexikális elem az beszerzése](../../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## A hozzáférési jogkivonatok lekérésére
tooacquire hozzáférési jogkivonatok, módosítsa a hello bejelentkezési kérelem:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'query', 'form_post', or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Kattintson a következő hivatkozás tooexecute hello ehhez a kérelemhez. Miután bejelentkezik, a böngésző használata átirányított toohttps://localhost/myapp/ egy azonosító jogkivonat és egy kódot hello címsorába. Vegye figyelembe, hogy ehhez a kérelemhez használt `response_mode=query` (bemutatási célokra csak). Azt javasoljuk, hogy használjon `response_mode=form_post`.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

Hello kérelmet, és az-engedélyhatókörök ot `response_type=id_token code`, hello v2.0-végponttól biztosítja, hogy hello a felhasználó hozzájárult hello jelzett toohello engedélyek `scope` lekérdezési paraméter. Az engedélyezési kód tooyour app tooexchange az olyan hozzáférési jogkivonatot ad vissza.

### A sikeres válasz
A sikeres válasz használatával `response_mode=form_post` dolgozunk:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Paraméter | Leírás |
| --- | --- |
| id_token |kért alkalmazás hello Azonosítót jogkivonatban hello. Hello azonosító token tooverify hello felhasználói identitás használatára, és a hello felhasználói munkamenet elindításához. Azonosító-jogkivonatokat és azok tartalmát további információkat találhat a hello [v2.0-végponttól jogkivonatok hivatkozás](active-directory-v2-tokens.md). |
| Kód |a kért alkalmazás hello engedélyezési kód hello. hello app hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használja hello cél erőforráson. Az engedélyezési kód nagyon rövid élettartamú. Általában egy engedélyezési kód körülbelül 10 perc múlva lejár. |
| state |Ha a state paraméter hello kérelem, ugyanazt az értéket meg kell jelennie a hello válasz hello is szerepel. hello app győződjön meg arról, hogy a hello állapotértékek hello kérés és válasz megegyeznek-e. |

### Hibaválaszba
Hibaválaszok is lehet küldeni toohello átirányítási URI-t, hogy hello alkalmazást is kezeli azokat megfelelően. Egy hiba történt egy válasz így néz ki:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely a előforduló hibákat, és tooreact tooerrors tooclassify típusok használhatók. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hitelesítési hiba okának azonosításához. |

A lehetséges hibakódok és ajánlott ügyfél válaszok ismertetését lásd: [engedélyezési végpont hibái hibakódok](#error-codes-for-authorization-endpoint-errors).

Ha van egy engedélyezési kódot és az azonosító tokent, hello felhasználói bejelentkezés, és a nevében a hozzáférési jogkivonatok lekérésére. toosign hello szereplő felhasználó esetében ellenőrizni kell hello azonosító token [leírtak pontosan](#validate-the-id-token). tooget hozzáférési jogkivonatok, kövesse az ismertetett lépések hello a [OAuth-protokoll dokumentációját](active-directory-v2-protocols-oauth-code.md#request-an-access-token).

---
title: "aaaUnderstand hello hitelesítési folyamata OpenID Connect Azure AD-ban |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan férhetnek hozzá a toouse HTTP üzenetek tooauthorize a tooweb alkalmazások és a webes API-k használata az Azure Active Directory és az OpenID Connect-bérlőben."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: fafd8ab906ee576c584fec2ef1e9de83ddb1f6e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Engedélyezi a hozzáférést tooweb alkalmazások OpenID Connect és az Azure Active Directory használatával
[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) egy egyszerű identitásrétegének hello OAuth 2.0 protokoll platformra épül. OAuth 2.0 meghatározása mechanizmusok tooobtain és -felhasználási **hozzáférési jogkivonatok** tooaccess védett erőforrások, de nem határoznak meg szabványos módszerek tooprovide azonosító adatok. Egy bővítmény toohello OAuth 2.0 hitelesítési folyamatot, a hitelesítési OpenID Connect valósítja meg. Információt ad a végfelhasználói hello hello formájában egy `id_token` , amely hello felhasználó identitásával az hello ellenőrzi és hello felhasználó alapvető profiladataihoz ismerteti.

Az ajánlott megoldás OpenID Connect, ha böngészőalapú hozzáférést biztosít, és a kiszolgálón futtatott webalkalmazás.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## Használja az OpenID Connect hitelesítési folyamat
hello legalapvetőbb bejelentkezési folyamata tartalmazza a lépéseket követve hello – azok az alábbiakban ismertetettek.

![OpenId Connect hitelesítési folyamat](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## OpenID Connect metaadat-dokumentum

A metaadat-dokumentum, egy alkalmazás tooperform bejelentkezés szükséges hello adatok többségét tartalmazó OpenID Connect ismerteti. Ide tartoznak hello URL-címek toouse és hello szolgáltatást nyilvános aláírási kulcsok hello helyét. hello OpenID Connect metaadat-dokumentum helyen találhatók:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
hello metaadatai egy egyszerű JavaScript Object Notation (JSON) dokumentumot. Tekintse meg a következő példa részlet hello. hello részlet tartozó tartalom teljes ismertetett hello [OpenID Connect specification](https://openid.net).

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    
    ...
}
```

## Hello bejelentkezési kérelem küldése
Ha a webes alkalmazás tooauthenticate hello felhasználó, azt kell közvetlen hello felhasználói toohello `/authorize` végpont. Ehhez a kérelemhez hasonló toohello első szakasza hello [OAuth 2.0 hitelesítési kód Flow](active-directory-protocols-oauth-code.md), néhány fontos különbség a:

* hello kérelem tartalmaznia kell hello hatókör `openid` a hello `scope` paraméter.
* Hello `response_type` paraméternek tartalmaznia kell `id_token`.
* hello kérelem tartalmaznia kell hello `nonce` paraméter.

Ezért egy minta kérelem néz ki:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett: bérlői azonosítók, például `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` vagy `contoso.onmicrosoft.com` vagy `common` bérlői független jogkivonatokat |
| client_id |Szükséges |hello alkalmazásazonosító hozzárendelt tooyour app és az Azure AD regisztrálásakor. Ez a hello Azure portálon találja meg. Kattintson a **Azure Active Directory**, kattintson a **App regisztrációk**, hello alkalmazás kiválasztása, és keresse meg az alkalmazásazonosítót hello hello alkalmazás lapján. |
| response_type |Szükséges |Tartalmaznia kell `id_token` az OpenID Connect bejelentkezhet.  Például a más response_types is tartalmazhat `code`. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája.  Az OpenID Connect, magában kell foglalnia hello hatókör `openid`, amely az eszköz toohello hello hozzájárulási felhasználói felület "Bejelentkezés" engedéllyel.  A kérésben a hozzájárulás kérése más hatókörök is. |
| Nonce |Szükséges |Egy foglalt hello kérelemben szereplő hello eredő hello alkalmazás által generált érték `id_token` jogcímként.  hello alkalmazás ezután ellenőrizheti a érték toomitigate hitelesítési karakterláncok ismétlésének támadásokat.  hello érték általában egy véletlenszerű, egyedi karakterlánc vagy a GUID, amely használt tooidentify hello származási hello kérelem. |
| redirect_uri |Ajánlott |az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszok redirect_uri hello.  Azt pontosan egyeznie kell azzal a különbséggel url-kódolású kell lennie az hello portálon regisztrált hello redirect_uris. |
| response_mode |Ajánlott |Határozza meg, amelyeket használt toosend hello eredményül kapott authorization_code hátsó tooyour app hello módszerét.  Támogatott értékek a következők `form_post` a *HTTP közzétett űrlapból* vagy `fragment` a *URL-cím töredék*.  Webes alkalmazásokhoz, javasoljuk `response_mode=form_post` tooensure hello jogkivonatok tooyour alkalmazás a legbiztonságosabb átvitelét. |
| state |Ajánlott |Hello token válaszként visszaadott hello kérelemben szereplő érték.  Bármely, a kívánt tartalmat karakterlánc lehet.  Egy véletlenszerűen generált egyedi érték jellemzően a [webhelyközi kérések hamisításának megakadályozása támadások megelőzése](http://tools.ietf.org/html/rfc6749#section-10.12).  hello állapota is hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |
| parancssor |Nem kötelező |A felhasználói beavatkozás szükséges hello típusát jelzi.  Jelenleg hello csak az érvényes értékek: "bejelentkezés", "none", és "".  `prompt=login`a hitelesítő adataikat az hello felhasználói tooenter kényszeríti, hogy kérésre nem lehet negálni egyszeri bejelentkezést.  `prompt=none`hello ellentétes - hello felhasználó számára nem jelenik meg semmilyen interaktív kérdés vállal biztosítja.  Ha hello kérelem nem hajtható végre csendes keresztül egyszeri bejelentkezést, hello végpont hibát ad vissza.  `prompt=consent`Elindítja a hello OAuth hozzájárulási párbeszédpanel hello felhasználó jelentkezik be, miután hello felhasználói toogrant engedélyek toohello alkalmazást kéri. |
| login_hint |Nem kötelező |Használt toopre-kitöltés hello felhasználónév vagy e-mail cím mező hello bejelentkezési oldal hello felhasználó, akkor lehet, ha ismeri a felhasználónevét időben.  Gyakran alkalmazások újrahitelesítés, amelyek már kivont hello felhasználónév előző bejelentkezés során használja ezt a paramétert használó hello `preferred_username` jogcímek. |

Ezen a ponton hello felhasználó van tooenter kéri a hitelesítő adatokat és a teljes hello hitelesítés.

### Mintaválasz
Egy mintaválasz hello felhasználó rendelkezik hitelesítése után nézhet ki:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Paraméter | Leírás |
| --- | --- |
| id_token |Hello `id_token` kért hello alkalmazást. Használhatja a hello `id_token` tooverify hello a felhasználó identitását, és a hello felhasználói munkamenet elindításához. |
| state |Hello kérelem is hello token válaszként visszaadott szerepel érték. Egy véletlenszerűen generált egyedi érték jellemzően a [webhelyközi kérések hamisításának megakadályozása támadások megelőzése](http://tools.ietf.org/html/rfc6749#section-10.12).  hello állapota is hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |

### Hibaválaszba
Hibaválaszok is küldhet toohello `redirect_uri` , ezek a hello alkalmazást is megfelelően kezelése:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |

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

## Hello id_token ellenőrzése
Csak fogadó egy `id_token` nincs elegendő tooauthenticate hello felhasználói; kell hello aláírás érvényesítése és ellenőrizze a hello hello jogcímek `id_token` / az alkalmazás követelményeinek. hello Azure AD-végpont JSON Web Tokens (JWTs) és a nyilvános kulcsú hitelesítésen toosign jogkivonatokat használ, és győződjön meg arról, hogy azok érvényesek.

Választhat toovalidate hello `id_token` ügyfél kódban, de a gyakori eljárás toosend hello `id_token` tooa háttérkiszolgáló, és végezze el a hello érvényesítési hiba. Amennyiben Ön ellenőrizte hello hello aláírása `id_token`, van néhány jogcímek szükséges tooverify áll.

Kezdésként érdemes lehet is toovalidate további jogcímeket a forgatókönyvtől függően. Néhány gyakori érvényesítést a következők:

* Hello felhasználó/szervezeti biztosítása rendelkezik regisztrált hello alkalmazást.
* Biztosítani kell hello felhasználó rendelkezik-e megfelelő engedélyezési vagy jogosultságokkal
* Bizonyos erősségével hitelesítés úgy történt, például a többtényezős hitelesítést.

Hello ellenőrzése után `id_token`, a hello felhasználói munkamenet elindításához és a hello jogcímek használata a hello `id_token` hello felhasználó az alkalmazás tooobtain adatait. Ez az információ alkalmas megjelenített, rekordok, engedélyek, stb. Hello tokentípusokat és a jogcímeket kapcsolatos további információkért olvassa el a [támogatott jogkivonat és jogcímtípusok](active-directory-token-and-claims.md).

## Kijelentkezési kérés küldése
Ha toosign hello felhasználói hello alkalmazásból, az nem elegendő tooclear az alkalmazás cookie-k és az egyéb utolsó hello hello felhasználói munkamenetet.  Is irányíthatja át hello felhasználói toohello `end_session_endpoint` a kijelentkezési.  Toodo ezért sikertelen lesz, ha hello felhasználó képes tooreauthenticate tooyour app a hitelesítő adatok újbóli megadása nélkül lesz, mert érvényes egyszeri bejelentkezés munkamenetet hello Azure AD-végponthoz rendelkeznek.

Egyszerűen átirányíthatók hello felhasználói toohello `end_session_endpoint` hello OpenID Connect metaadat-dokumentum szerepel:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Paraméter |  | Leírás |
| --- | --- | --- |
| post_logout_redirect_uri |Ajánlott |hello URL-címet, amely a felhasználó hello átirányított tooafter sikeres kijelentkezési kell lennie.  Ha nem szereplő, hello felhasználói általános üzenet jelenik meg. |

## Egyszeri kijelentkezés
Ha átirányítja a hello felhasználói toohello `end_session_endpoint`, az Azure AD hello felhasználói munkamenet hello böngészőből törli. Hello felhasználó továbbra is köthető, amely az Azure AD hitelesítéshez használandó tooother alkalmazások. tooenable ezen alkalmazások toosign egyidejűleg hello felhasználó, az Azure AD küld egy HTTP GET kérést toohello regisztrált `LogoutUrl` alkalmazások hello adott hello jelenleg bejelentkezett felhasználó számára. Alkalmazások válaszolnia kell az munkamenet hello felhasználói törlését és azonosító törlésével toothis kérelem egy `200` választ.  Ha toosupport egyszeri bejelentkezési ki kívánja az alkalmazást, akkor meg kell valósítania például egy `LogoutUrl` az alkalmazás kódjában.  Beállíthatja a hello `LogoutUrl` a hello Azure-portálon:

1. Keresse meg a toohello [Azure Portal](https://portal.azure.com).
2. Válassza ki az Active Directory parancsával hello jobb felső sarkában hello lap fiókjába.
3. Hello bal oldali navigációs panelen, válassza ki a **Azure Active Directory**, majd válassza **App regisztrációk** válassza ki az alkalmazást.
4. Kattintson a **tulajdonságok** és hello található **kijelentkezési URL-cím** szövegmezőben. 

## Token beszerzése
Sok webalkalmazások toonot csak bejelentkezési hello felhasználói az szükséges, de egy webszolgáltatás-bővítmény OAuth protokollt használó felhasználó nevében is elérhető. Ebben a forgatókönyvben egyesíti az OpenID Connect felhasználói hitelesítés során egyidejűleg az beszerzése egy `authorization_code` , amely lehet használt tooget `access_tokens` használatával hello OAuth-engedélyezési kód áramlását.

## A hozzáférési jogkivonatok lekérésére
tooacquire hozzáférési jogkivonatok, toomodify hello bejelentkezési kérelme a fenti kell:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                     
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

Többek között a engedélyhatókörök hello kérelemben és használatával `response_type=code+id_token`, hello `authorize` végpont biztosítja, hogy hello a felhasználó hozzájárult hello jelzett toohello engedélyek `scope` lekérdezésparaméter, és térjen vissza az alkalmazás egy engedélyezési kód a hozzáférési token tooexchange.

### A sikeres válasz
A sikeres válasz használatával `response_mode=form_post` láthatóhoz hasonló:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Paraméter | Leírás |
| --- | --- |
| id_token |Hello `id_token` kért hello alkalmazást. Használhatja a hello `id_token` tooverify hello a felhasználó identitását, és a hello felhasználói munkamenet elindításához. |
| Kód |hello authorization_code, amely a kért alkalmazás hello. hello app hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használja hello cél erőforráson. Authorization_codes rövid élt, és általában körülbelül 10 perc múlva lejár. |
| state |Ha a state paraméter hello kérelem, ugyanazt az értéket meg kell jelennie a hello válasz hello is szerepel. hello app győződjön meg arról, hogy a hello állapotértékek hello kérés és válasz megegyeznek-e. |

### Hibaválaszba
Hibaválaszok is küldhet toohello `redirect_uri` , ezek a hello alkalmazást is megfelelően kezelése:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |

Hello lehetséges hibakódokat és a javasolt művelet leírását, [engedélyezési végpont hibái hibakódok](#error-codes-for-authorization-endpoint-errors).

Miután egy engedélyezési igazoló `code` és egy `id_token`, hello felhasználói bejelentkezés és a hozzáférési jogkivonatok lekérésére, ha a nevében.  toosign hello szereplő felhasználó esetében ellenőrizni kell hello `id_token` pontosan a fent leírt módon. tooget hozzáférési jogkivonatok, lépésekkel hello hello "Hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használata" című részében ismertetett a [OAuth-protokoll dokumentációját](active-directory-protocols-oauth-code.md).

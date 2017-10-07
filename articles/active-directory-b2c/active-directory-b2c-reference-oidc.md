---
title: "Bejelentkezés az OpenID Connect - Azure AD B2C webes |} Microsoft Docs"
description: "Épület webalkalmazások OpenID Connect hitelesítési protokoll hello végrehajtásának hello Azure Active Directory használatával"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 21d420c8-3c10-4319-b681-adf2e89e7ede
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 89e9cfa28e4e5c34304aea355cca2dd0c4b42abc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Az Azure Active Directory B2C: Webes bejelentkezés az OpenID Connect
OpenID Connect egy olyan hitelesítési protokoll, OAuth 2.0-s, amely a tooweb alkalmazások használt toosecurely bejelentkezési felhasználói platformra épül. Hello Azure Active Directory B2C (az Azure AD B2C) végrehajtása az OpenID Connect használatával is erőforrás kihelyezése a regisztráció, bejelentkezés, és más Identitáskezelés észlel a a webes alkalmazások tooAzure Active Directory (Azure AD). Ez az útmutató bemutatja, hogyan toodo nyelvtől független módon igen. Leírja, hogyan toosend és HTTP-üzenetek fogadása a nyílt forráskódú kódtárai bármelyikét használata nélkül.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) kibővíti az OAuth 2.0 hello *engedélyezési* protokollt használni egy *hitelesítési* protokoll. Ez lehetővé teszi tooperform egyszeri bejelentkezés OAuth használatával. Hello fogalma okozna egy *azonosító token*, egy biztonsági jogkivonatot, amely lehetővé teszi, hogy a hello ügyfél tooverify hello hello felhasználó identitását, amely hello felhasználó alapvető profiladataihoz információt szerezni.

OAuth 2.0 terjeszti ki, mert emellett lehetővé teszi az alkalmazások toosecurely megszerzése *hozzáférési jogkivonatok*. Használhatja a access_tokens tooaccess erőforrások által védett egy [engedélyezési server](active-directory-b2c-reference-protocols.md#the-basics). Ajánlott OpenID Connect, ha a webes alkalmazás, amely a kiszolgálón futtatott és böngészőn keresztül elért most felépítése. Ha az Azure AD B2C segítségével tooadd identity management tooyour hordozható vagy asztali alkalmazásokkal, használjon [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) ahelyett, hogy az OpenID Connect.

Az Azure AD B2C hello szabványos OpenID Connect protokoll toodo több mint egyszerű hitelesítési és engedélyezési terjeszti ki. Hello okozna [házirend paraméter](active-directory-b2c-reference-policies.md), amely lehetővé teszi toouse OpenID Connect tooadd felhasználói élményt – például a regisztráció, bejelentkezés és a profilkezelés--tooyour alkalmazást. Itt azt mutatja be toouse ezen észlel, a webalkalmazások OpenID Connect és házirendek tooimplement. Is mutat be, hogyan tooget hozzáférési jogkivonatok eléréséhez webes API-khoz.

hello példa HTTP-kérelmek hello a következő szakaszban használja, a minta B2C-címtárban, fabrikamb2c.onmicrosoft.com, valamint a mintaalkalmazást, https://aadb2cplayground.azurewebsites.net és házirendek. Szabad kimenő hello tootry fel a kérelmeket saját kezűleg ezeket az értékeket, vagy lecserélheti őket a saját.
Ismerje meg, hogyan túl[saját B2C-bérlő, alkalmazások és házirendek](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Hitelesítési kérések küldése
A webalkalmazás tooauthenticate hello felhasználói kell, és egy házirend hajtható végre, akkor is közvetlen hello felhasználói toohello `/authorize` végpont. Ez az interaktív része hello hello folyamata, ahol a hello felhasználó végrehajtja a műveletet, attól függően, hogy hello házirend.

Ebben a kérelemben hello ügyfél azt jelzi, hogy kell-e a hello hello felhasználótól tooacquire hello engedélyek `scope` paraméter és hello házirend tooexecute a hello `p` paraméter. Három példák találhatók: hello a következő szakaszok (sortöréssel olvashatóság érdekében), minden más házirend segítségével. tooget abba az egyes kérelmek működése, próbálja meg végrehajtani a Beillesztés hello kérelmet egy böngészőt, és azt futtatja.

#### <a name="use-a-sign-in-policy"></a>A bejelentkezési házirend
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>A regisztrációs szabályzatban használata
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Egy házirend-profil szerkesztése
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [Azure-portálon](https://portal.azure.com/) tooyour app rendelve. |
| response_type |Szükséges |hello válasz típusát, amely az OpenID Connect tartalmaznia kell egy azonosító jogkivonatot. Ha a webalkalmazásnak is kell jogkivonatok hívásakor webes API-k, `code+id_token`, ahogy azt itt régebben már kötöttek. |
| redirect_uri |Ajánlott |Hello `redirect_uri` paraméter az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszokat. Ez pontosan egyeznie kell hello `redirect_uri` paraméterek olyan hello portálon, azzal a különbséggel, hogy az URL-kódolású kell lennie. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája. Egy hatókör érték tooAzure AD mindkét kérnek engedélyeket. Hello `openid` hatókör jelzi egy engedély toosign hello felhasználói és az adatok hello felhasználóról hello azonosító-jogkivonatokat (további toocome ezen hello cikk későbbi részében) formájában. Hello `offline_access` hatókör megadása nem kötelező web Apps. Azt jelzi, hogy az alkalmazás kell egy *frissítési jogkivonat* a hosszú élettartamú hozzáférés tooresources. |
| response_mode |Ajánlott |hello módszerhez használt toosend hello eredményül kapott engedélyezési kód hátsó tooyour app kell lennie. Ez lehet akár `query`, `form_post`, vagy `fragment`.  Hello `form_post` válasz mód használata ajánlott legjobb biztonsági. |
| state |Ajánlott |Hello kérelem is hello token válaszként visszaadott szerepel érték. Bármely, a kívánt tartalmat karakterlánc lehet. Egy véletlenszerűen generált egyedi érték webhelyközi kérések hamisításának megakadályozása támadások megelőzése általában szolgál. hello állapota is hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, mint amilyenek korábban voltak a hello lap előtt. |
| Nonce |Szükséges |Hello kérelem (hello alkalmazás által generált), amely szerepelni fog eredményül kapott Azonosítót jogkivonatban hello jogcímként szerepel érték. hello alkalmazás ezután ellenőrizheti a érték toomitigate hitelesítési karakterláncok ismétlésének támadásokat. hello érték általában egy véletlenszerű egyedi karakterlánc, amely használt tooidentify hello származási hello kérelem. |
| P |Szükséges |hello házirendet, amely végrehajtja. Olyan házirendet, amely a B2C bérlő létrehozása hello nevét. hello házirend név-érték előtaggal kell kezdődnie `b2c\_1\_`. További információ a házirendek és hello további [bővíthető szabályzat-keretrendszernek](active-directory-b2c-reference-policies.md). |
| parancssor |Optional |hello típusa a felhasználói beavatkozás szükséges. hello csak érvényes jelenleg értéke `login`, amely kényszeríti hello felhasználói tooenter adott kérelem hitelesítő adataikat. Egyszeri bejelentkezés nem lépnek érvénybe. |

Ezen a ponton hello felhasználónak kapcsolatba toocomplete hello házirend munkafolyamat. Ez lehet, hogy magában hello felhasználói írja be a felhasználónevét és jelszavát, bejelentkezés egy közösségi identitás regisztrál a hello könyvtár, vagy minden más szám lépésből áll, attól függően, hogy hogyan a hello házirend lett meghatározva.

Hello felhasználói hello házirend befejezése után az Azure AD adja vissza egy válasz tooyour alkalmazást jelzett hello `redirect_uri` paraméter módszerrel hello hello megadott `response_mode` paraméter. az egyes esetekben független hello házirend végrehajtott megelőző hello azonos van hello hello válasz.

A sikeres válasz használatával `response_mode=fragment` alábbihoz hasonlóan fog kinézni:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Paraméter | Leírás |
| --- | --- |
| id_token |kért alkalmazás hello Azonosítót jogkivonatban hello. Hello azonosító token tooverify hello felhasználói identitás használatára, és a hello felhasználói munkamenet elindításához. További részleteket a azonosító-jogkivonatokat és azok tartalmát szerepelnek hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). |
| Kód |hello engedélyezési kód hello alkalmazáshoz szükség, ha használt `response_type=code+id_token`. hello app hello engedélyezési kód toorequest olyan hozzáférési jogkivonatot használja a cél erőforráson. Hitelesítési kódok nagyon rövid élettartamú. Ezek általában körülbelül 10 perc múlva lejár. |
| state |Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy hello `state` hello kérelem-válasz szereplő értékek azonosak. |

Hibaválaszok is küldhetők toohello `redirect_uri` paraméter, hello alkalmazást is kezeli azokat megfelelően:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Paraméter | Leírás |
| --- | --- |
| error |Lehet, amely lehet használt tooreact tooerrors előforduló hibát használt tooclassify típusú hibakód karakterlánc. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |
| state |Hello teljes leírást az első tábla hello ebben a szakaszban talál. Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy hello `state` hello kérelem-válasz szereplő értékek azonosak. |

## <a name="validate-hello-id-token"></a>Hello azonosító token ellenőrzése
Csak fogadó egy azonosító jogkivonat nincs elég tooauthenticate hello felhasználó. Kell hello azonosító jogkivonat-aláírás ellenőrzése és hello jogcímek hello jogkivonat / az alkalmazás követelményeinek ellenőrzése. Használja az Azure AD B2C [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) és nyilvános kulcs titkosítás toosign jogkivonatokat, és győződjön meg arról, hogy azok érvényesek.

Nincsenek számos nyílt forráskódú kódtárai elérhető JWTs, attól függően, hogy a nyelvi preferencia szerinti ellenőrzése. Javasoljuk, hogy ezek a lehetőségek felfedezése megvalósító egyéni ellenőrzési logika helyett. Itt hello információk hasznosak bizonyulnak a tudja, hogyan tooproperly használja ezeket a könyvtárakat.

Az Azure AD B2C az OpenID Connect metaadat-végpontjához, amely lehetővé teszi egy alkalmazás toofetch kapcsolatos információkat az Azure AD B2C futásidőben rendelkezik. Ezen információk közé tartozik a végpontok, lexikális elem tartalmának és jogkivonat-aláíró kulcsok. Nincs a JSON metaadat-dokumentum-házirendet a B2C-bérlőben. Például hello metaadat-dokumentum a hello `b2c_1_sign_in` -szabályzat `fabrikamb2c.onmicrosoft.com` helyen található:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

A konfigurációs dokumentum hello tulajdonságait egyik `jwks_uri`, amelynek az értéke az ugyanabban a házirendben lenne hello:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

toodetermine mely házirendet használtak egy azonosító tokent aláíró (és az adott toofetch hello metaadatok), két lehetőség közül választhat. Első lépésként hello házirendnév hello megtalálható `acr` jogcím hello Azonosítót jogkivonatban. Hogyan tooparse hello jogcím-azonosító-jogkivonatból információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). A másik lehetőség egy tooencode hello házirend hello hello értékének `state` paraméter küldjön hello kérelmet, és milyen házirend lett megadva toodetermine dekódolására. Mindkét módszer esetén érvényes.

Hello metaadat-dokumentum hello-metaadatok endpoint OpenID Connect a korábban szerezte be, miután hello RSA 256 nyilvános kulcsok (amelyek a végpont található) használhatja toovalidate hello hello azonosító jogkivonat aláírását. Előfordulhat, hogy több idő álljon a végponti szereplő kulcsok, minden egyes által azonosított egy `kid` jogcímek. hello hello azonosító jogkivonat fejléc is tartalmaz egy `kid` jogcím, ami azt jelenti, hogy ezek a kulcsok eddig használt toosign hello azonosító token. További információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md) (a szakasz hello [jogkivonatok ellenőrzése](active-directory-b2c-reference-tokens.md#token-validation), különösen).
<!--TODO: Improve hello information on this-->

Hello azonosító jogkivonat hello aláírás érvényesítése után vannak, hogy kell-e tooverify több jogcímeket. Például:

* Ellenőriznie kell a hello `nonce` jogcím tooprevent hitelesítési karakterláncok ismétlésének támadásokat. Az érték lehet hello bejelentkezési kérelemben megadott.
* Ellenőriznie kell a hello `aud` jogcímek, az alkalmazás, amely azonosító token hello tooensure ki. Az érték lehet hello Alkalmazásazonosító az alkalmazás.
* Ellenőriznie kell a hello `iat` és `exp` jogcímeket, amely azonosító token hello tooensure nem járt le.

Nincsenek is végre kell hajtania néhány további érvényesítést. Ezekről az alábbi hello részletes [OpenID Connect Core Spec](http://openid.net/specs/openid-connect-core-1_0.html).  Érdemes lehet toovalidate további jogcímeket, a forgatókönyvtől függően. Néhány gyakori érvényesítést a következők:

* Győződjön meg arról, hogy a felhasználó/szervezeti hello rendelkezik regisztrált hello alkalmazást.
* Győződjön meg arról, hogy hello felhasználó rendelkezik-e megfelelő engedélyezési vagy jogosultságokkal.
* Győződjön meg arról, hogy bizonyos erősségével hitelesítési történt, például az Azure multi-factor Authentication.

A hello jogcímek egy Azonosítót jogkivonatban további információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md).

Hello azonosító token ellenőrzése után megkezdheti a munkamenet hello felhasználóval. Hello jogcímek hello azonosító token tooobtain hello felhasználói adatait az alkalmazás használata. Ezt az információt használja megjelenített rekordok és engedélyezési tartalmazza.

## <a name="get-a-token"></a>A jogkivonat beolvasása
Ha a webes alkalmazás tooonly kell végrehajtani a házirendek, továbbléphet hello a következő néhány szakasz. Ezek a szakaszok olyan alkalmazható csak tooweb alkalmazások, amelyek toomake hitelesített hívásokat tooa webes API kell, és az Azure AD B2C is védi.

Hello engedélyezési kód beszerzett is beválthatja (használatával `response_type=code+id_token`) token toohello szükséges erőforrás küldésével egy `POST` toohello kérelem `/token` végpont. Hello csak a kérhet egy token erőforrás jelenleg az alkalmazás saját webes API-t. hello egyezmény token tooyourself kér az alkalmazás ügyfél-azonosítója hello hatóköreként toouse:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| P |Szükséges |hello házirend, de a használt tooacquire hello engedélyezési kódot. A kérelem nem használhatja másik szabályt. Megjegyzés: a paraméter toohello lekérdezési karakterláncot, nem toohello hozzáadása `POST` törzsében. |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [Azure-portálon](https://portal.azure.com/) tooyour app rendelve. |
| grant_type |Szükséges |engedély megadása, amelynek támogatnia kell a típusú hello `authorization_code` a hello hitelesítésikód-folyamata. |
| Hatókör |Ajánlott |Hatókörök szóközökkel elválasztott listája. Egy hatókör érték tooAzure AD mindkét kérnek engedélyeket. Hello `openid` hatókör jelzi egy engedély toosign hello felhasználói és az adatok hello felhasználóról id_token paraméterek hello formában. Használt tooget jogkivonatok tooyour alkalmazás saját háttér-webes API-t, amely hello által képviselt azok hello ügyfél ugyanazon alkalmazás azonosítója. Hello `offline_access` hatókör azt jelzi, hogy az alkalmazás a hosszú élettartamú hozzáférés tooresources kell egy frissítési jogkivonat. |
| Kód |Szükséges |hello engedélyezési kód hello folyamat első szakasza hello beszerzett. |
| redirect_uri |Szükséges |Hello `redirect_uri` paraméter hello alkalmazás, amelyre hello engedélyezési kód kapta. |
| client_secret |Szükséges |hello létrehozott hello alkalmazáskulcsot [Azure-portálon](https://portal.azure.com/). Ez az alkalmazástitkok fontos biztonsági összetevő. Meg kell tárolja biztonságos helyen a kiszolgálón. Az ügyfélkulcs rendszeres időközönként elforgatása is kell. |

A sikeres token válasz néz ki:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Paraméter | Leírás |
| --- | --- |
| not_before |hello idő, mely hello jogkivonat érvényes, időbeli kortartományon. |
| token_type |hello token objektumtípus-érték. csak olyan típusú, amely az Azure AD által támogatott hello `Bearer`. |
| access_token |hello az Ön által kért JWT jogkivonat aláírása. |
| Hatókör |hello hatókörök mely hello lexikális elem érvénytelen. Ezek használhatók a gyorsítótárazáshoz jogkivonatok későbbi használatra. |
| expires_in |Hello, hogy mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| refresh_token |Az OAuth 2.0-s frissítési jogkivonat. hello app használhatja a token tooacquire további jogkivonatok hello aktuális jogkivonat lejárata után is. Frissítési jogkivonatok hosszú élettartamú, és hosszabb ideig használt tooretain hozzáférés tooresources lehet. További részletekért tekintse meg a toohello [B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). Megjegyzés: kell használt hello hatókör `offline_access` hello engedélyezési és token lévő kérelmek rendelés tooreceive egy frissítési jogkivonat. |

Hibaválaszok a következőhöz hasonló:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Paraméter | Leírás |
| --- | --- |
| error |Lehet, amely lehet használt tooreact tooerrors előforduló hibát használt tooclassify típusú hibakód karakterlánc. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |

## <a name="use-hello-token"></a>Hello jogkivonat használata
Most, hogy olyan hozzáférési jogkivonatot már sikeresen szerezte be, használhatja hello token kérelmek tooyour webes API-k belefoglalja azt hello `Authorization` fejléc:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-hello-token"></a>Hello token frissítése
Azonosító-jogkivonatokat rövid élettartamú. Frissítenie kell őket után toocontinue képes tooaccess erőforrások alatt. Megteheti egy másik elküldése `POST` toohello kérelem `/token` végpont. Most, adja meg a hello `refresh_token` paraméter helyett hello `code` paraméter:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Paraméter | Szükséges | Leírás |
| --- | --- | --- |
| P |Szükséges |hello házirend, de a használt tooacquire hello eredeti frissítési jogkivonat. A kérelem nem használhatja másik szabályt. Vegye figyelembe, hogy hozzáadta a paraméter toohello lekérdezési karakterláncot, nem toohello FELADÁS egy vagy több szervezet. |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [Azure-portálon](https://portal.azure.com/) tooyour app rendelve. |
| grant_type |Szükséges |hello típusa engedély megadása, amelynek hello hitelesítésikód-folyamata ezen szakasza a frissítési jogkivonatot kell lennie. |
| Hatókör |Ajánlott |Hatókörök szóközökkel elválasztott listája. Egy hatókör érték tooAzure AD mindkét kérnek engedélyeket. Hello `openid` hatókör jelzi egy engedély toosign hello felhasználói és az adatok hello felhasználóról azonosító-jogkivonatokat hello formájában. Használt tooget jogkivonatok tooyour alkalmazás saját háttér-webes API-t, amely hello által képviselt azok hello ügyfél ugyanazon alkalmazás azonosítója. Hello `offline_access` hatókör azt jelzi, hogy az alkalmazás a hosszú élettartamú hozzáférés tooresources kell egy frissítési jogkivonat. |
| redirect_uri |Ajánlott |Hello `redirect_uri` paraméter hello alkalmazás, amelyre hello engedélyezési kód kapta. |
| refresh_token |Szükséges |hello eredeti frissítési jogkivonat hello folyamat második szakasza hello beszerzett. Megjegyzés: kell használt hello hatókör `offline_access` hello engedélyezési és token lévő kérelmek rendelés tooreceive egy frissítési jogkivonat. |
| client_secret |Szükséges |hello létrehozott hello alkalmazáskulcsot [Azure-portálon](https://portal.azure.com/). Ez az alkalmazástitkok fontos biztonsági összetevő. Meg kell tárolja biztonságos helyen a kiszolgálón. Az ügyfélkulcs rendszeres időközönként elforgatása is kell. |

A sikeres token válasz néz ki:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Paraméter | Leírás |
| --- | --- |
| not_before |hello idő, mely hello jogkivonat érvényes, időbeli kortartományon. |
| token_type |hello token objektumtípus-érték. csak olyan típusú, amely az Azure AD által támogatott hello `Bearer`. |
| access_token |hello az Ön által kért JWT jogkivonat aláírása. |
| Hatókör |hello hatókör, amely a hello token esetén érvényes, gyorsítótárazás későbbi használatra jogkivonatok használható. |
| expires_in |Hello, hogy mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| refresh_token |Az OAuth 2.0-s frissítési jogkivonat. hello app használhatja a token tooacquire további jogkivonatok hello aktuális jogkivonat lejárata után is.  Frissítési jogkivonatok hosszú élettartamú, és hosszabb ideig használt tooretain hozzáférés tooresources lehet. További részletekért tekintse meg a toohello [B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). |

Hibaválaszok a következőhöz hasonló:

```
{
    "error": "access_denied",
    "error_description": "hello user revoked access toohello app.",
}
```

| Paraméter | Leírás |
| --- | --- |
| error |Lehet, amely lehet használt tooreact tooerrors előforduló hibát használt tooclassify típusú hibakód karakterlánc. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |

## <a name="send-a-sign-out-request"></a>Kijelentkezési kérés küldése
Ha azt szeretné, hogy toosign hello felhasználói hello alkalmazásból, az nem elég tooclear az alkalmazás cookie-k és az egyéb utolsó hello hello felhasználói munkamenetet. Hello felhasználói tooAzure AD toosign ki is irányíthatja át. Ha így sem működik a toodo, hello felhasználói lehet képes tooreauthenticate tooyour app hitelesítő adataikkal ismételt beírása nélkül. Ennek az az oka érvényes egyszeri bejelentkezés munkamenetet az Azure ad-val rendelkeznek.

Egyszerűen átirányíthatók hello felhasználói toohello `end_session` hello a korábban ismertetett végpont szerepel-e hello OpenID Connect metaadat-dokumentum "Hello azonosító token ellenőrzése" szakaszában:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| P |Szükséges |hello házirend, amelyet az toouse toosign hello felhasználói kívül az alkalmazást. |
| post_logout_redirect_uri |Ajánlott |hello URL hello felhasználó kell átirányított tooafter sikeres kijelentkezési. Ha nincs megadva, az Azure AD B2C hello felhasználói egy általános üzenetet jelenít meg. |

> [!NOTE]
> Bár arra utasíthatja a hello felhasználói toohello `end_session` végpont törölni fogja a néhány hello felhasználó állapotának egyszeri bejelentkezés az Azure AD B2C, nem a hello felhasználói kívül a közösségi identity provider (IDP) munkamenet alá fogja írni. Ha hello kijelölés ugyanahhoz az IDP hello későbbi bejelentkezés során, akkor lesz hitelesíthető, a hitelesítő adatok megadása nélkül. Ha egy felhasználó a B2C alkalmazás kívül toosign azt szeretné, akkor nem feltétlenül jelenti meg kívánják-toosign kívül a Facebook-fiókkal. Azonban a helyi fiókok esetében hello, hello felhasználói munkamenet véget ér megfelelően.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>A saját B2C-bérlő használata
Ha azt szeretné, tootry ezeket a kéréseket a szolgáltatást, először hajtsa végre a fenti három lépést, és az majd cserélje le az Ön által korábban leírt hello példaértékek:

1. [A B2C bérlő létrehozása](active-directory-b2c-get-started.md), és a bérlő hello nevét használja hello kérésekben.
2. [Hozzon létre egy alkalmazást](active-directory-b2c-app-registration.md) tooobtain egy azonosítót. Vegye fel az alkalmazás egy webes vagy webes API-t. Szükség esetén hozzon létre egy alkalmazás titkos kulcs.
3. [A házirendek létrehozása](active-directory-b2c-reference-policies.md) tooobtain a házirend nevét.


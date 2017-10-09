---
title: "Az Azure Active Directory B2C: Egyoldalas alkalmazások implicit engedélyezési folyamat |} Microsoft Docs"
description: "Ismerje meg, hogyan toobuild egyoldalas alkalmazások közvetlenül OAuth 2.0 típusú implicit használatával flow Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: a45cc74c-a37e-453f-b08b-af75855e0792
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: parakhj
ms.openlocfilehash: 5e52abc18fe545f0f6d1745cdb2ea42c5b2698cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-single-page-app-sign-in-by-using-oauth-20-implicit-flow"></a>Az Azure AD B2C: Egyoldalas alkalmazások bejelentkezés OAuth 2.0 típusú implicit engedélyezési folyamat használatával

> [!NOTE]
> A funkció jelenleg előzetes verzió.
> 

Számos modern alkalmazások alkalmazás előtér írt elsősorban a JavaScript rendelkezik. Hello app íródik gyakran, például az AngularJS, az Ember.js vagy a Durandal keretrendszer használatával. Egyoldalas alkalmazások és más elsősorban a böngészőben futó JavaScript-alkalmazások van néhány további kihívást hitelesítéshez:

* hello biztonsági jellemzőkkel ezeknek az alkalmazásoknak jelentősen különböznek a hagyományos server-alapú webes alkalmazásokhoz.
* Számos engedélyezési kiszolgálók és identitás-szolgáltatóktól nem támogatja az eltérő eredetű erőforrások megosztása (CORS) kérések.
* Hello alkalmazást innen máshová oldalas böngésző átirányítások lehet jelentősen invazív toohello felhasználói élményt.

toosupport ezeket az alkalmazásokat, Azure Active Directory B2C (az Azure AD B2C) hello OAuth 2.0 típusú implicit engedélyezési folyamat használja. hello OAuth 2.0 hitelesítési implicit grant flow ismertetett [hello OAuth 2.0-s szabvány 4.2 szakasz](http://tools.ietf.org/html/rfc6749). Az implicit engedélyezési folyamat hello alkalmazás-jogkivonatokat kap hello közvetlenül az Azure Active Directory (Azure AD) végpont, minden kiszolgáló-kiszolgáló exchange nélkül engedélyezik. Hitelesítési logikát és a munkamenet vesz kezelése teljes mértékben hello JavaScript ügyfélprogramokban, de további oldal átirányítások helyezi.

Az Azure AD B2C hello szabványos OAuth 2.0 típusú implicit engedélyezési folyamat toomore mint az egyszerű hitelesítés és engedélyezés terjeszti ki. Az Azure AD B2C bevezeti hello [házirend paraméter](active-directory-b2c-reference-policies.md). Hello házirend paraméterrel, használhatja az OAuth 2.0-s tooadd felhasználói tooyour alkalmazást észlel, például regisztráció, bejelentkezés és profilok kezelése. Ebben a cikkben megmutatjuk, hogyan toouse hello implicit engedélyezési folyamat és az Azure AD tooimplement minden olyan ezeket a szolgáltatásokat az egyoldalas alkalmazások. toohelp hozzáfogna, vessen egy pillantást a [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi) és [Microsoft .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi) minták.

A minta Azure AD B2C-címtár használjuk hello példa HTTP-kérelmek ebben a cikkben, **fabrikamb2c.onmicrosoft.com**. Is használhatja a saját mintaalkalmazás és a házirendeket. Megpróbálhatja hello kérelmek saját kezűleg ezen értékek használatával, vagy azokat lecserélheti a saját értékeit.
Ismerje meg, hogyan túl[beolvasása a saját Azure AD B2C directory, az alkalmazás és a házirendek](#use-your-own-b2c-tenant).


## <a name="protocol-diagram"></a>Protokoll diagramja

hello implicit bejelentkezési folyamata a következő ábra hello valahogy. Az egyes lépések hello cikk későbbi részében részletes leírása.

![OpenID Connect sávok](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Hitelesítési kérések küldése
Ha a webalkalmazás tooauthenticate hello felhasználói kell, és egy házirend hajtható végre, a rendszer arra utasítja hello felhasználói toohello `/authorize` végpont. Ez az interaktív része hello hello folyamata, ahol a hello felhasználó végrehajtja a műveletet, attól függően, hogy hello házirend. hello felhasználói lekérése egy azonosító token hello Azure AD-végpont.

Ebben a kérelemben hello ügyfél jelzi a hello `scope` paraméter hello engedélyeit, hogy kell-e a hello felhasználó tooacquire. A hello `p` paraméter azt jelzi, hogy hello házirend tooexecute. hello következő három példák (olvashatóság érdekében sortöréssel) minden használja egy másik házirendet. tooget abba az egyes kérelmek működése, próbálja meg végrehajtani a Beillesztés hello kérelmet egy böngészőt, és azt futtatja.

### <a name="use-a-sign-in-policy"></a>A bejelentkezési házirend
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>A regisztrációs szabályzatban használata
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Egy házirend-profil szerkesztése
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| client_id |Szükséges |hello Alkalmazásazonosító hozzárendelt hello tooyour alkalmazás [Azure-portálon](https://portal.azure.com). |
| response_type |Szükséges |Tartalmaznia kell `id_token` az OpenID Connect bejelentkezhet. Hello választípus is tartalmazhatja `token`. Ha `token`, az alkalmazás azonnal fogadhat olyan hozzáférési jogkivonatot hello engedélyezik a végponthoz, anélkül, hogy egy második kérelem toohello végpont hitelesítéséhez.  Ha hello `token` választípus, hello `scope` paraméternek tartalmaznia kell egy hatókör, amely jelzi, mely erőforrás tooissue hello lexikális eleme. |
| redirect_uri |Ajánlott |hello átirányítási URI-címe, az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszokat. Az pontosan egyeznie kell hello átirányítási URI-azonosítók regisztrált hello portálon, azzal a különbséggel, hogy az URL-kódolású kell lennie. |
| response_mode |Ajánlott |Megadja a hello metódus toouse toosend hello eredményül kapott jogkivonat hátsó tooyour alkalmazást.  Implicit adatfolyamok, használjon `fragment`. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája. Egy hatókör érték azt jelzi, hogy mindkét AD tooAzure hello engedélyeket, hogy kérnek. Hello `openid` hatókör jelzi egy engedély toosign hello felhasználói és az adatok hello felhasználóról azonosító-jogkivonatokat hello formájában. (Lesz döntésről bővebben ezt később hello cikkben.) Hello `offline_access` hatókör megadása nem kötelező web Apps. Azt jelzi, hogy kell-e az alkalmazást egy frissítési jogkivonat a hosszú élettartamú hozzáférés tooresources. |
| state |Ajánlott |Hello token válaszul is visszaadott hello kérelemben szereplő érték. Bármilyen tartalmat, hogy toouse karakterlánc lehet. Általában egy véletlenszerűen generált, egyedi érték használata esetén tooprevent webhelyközi kérések hamisításának megakadályozása támadásokat. hello állapota is használt tooencode információk hello alkalmazásban hello felhasználói állapotával kapcsolatos információkat előtt hello hitelesítési kérelmet, például a hello lap amilyenek korábban voltak a. |
| Nonce |Szükséges |Hello kérelem (hello alkalmazás által generált) hello eredményül kapott Azonosítót jogkivonatban jogcímként mellékelt szerepel érték. hello alkalmazás ezután ellenőrizheti a érték toomitigate hitelesítési karakterláncok ismétlésének támadásokat. Általában hello értéke egy véletlenszerű, egyedi karakterlánc, amely használt tooidentify hello származási hello kérelem. |
| P |Szükséges |hello házirend tooexecute. Az hello a házirend nevét, amely az Azure AD B2C-bérlő jön létre. hello házirend név-érték előtaggal kell kezdődnie **b2c\_1\_**. További információkért lásd: [beépített házirendek az Azure AD B2C](active-directory-b2c-reference-policies.md). |
| parancssor |Optional |hello típusa a felhasználói beavatkozás szükséges. Hello egyetlen érvényes érték jelenleg `login`. Ekkor elemezni hello felhasználói tooenter hitelesítő adataikkal, hogy kérésre. Egyszeri bejelentkezés nem lépnek érvénybe. |

Ezen a ponton hello felhasználónak kapcsolatba toocomplete hello házirend munkafolyamat. Ez lehet, hogy magában hello felhasználói írja be a felhasználónevét és jelszavát, bejelentkezés egy közösségi identitás regisztrál a hello könyvtár, vagy több lépésből áll. Felhasználói műveletek attól függ, hogyan a hello házirend lett meghatározva.

Hello felhasználói hello házirend befejezése után az Azure AD adja vissza egy válasz tooyour alkalmazást használt hello érték `redirect_uri`. Hello megadott hello módszer használ `response_mode` paraméter. van, hello válasz pontosan azonos hello az egyes hello felhasználói művelet esetek, független hello házirend, hogy végre lett hajtva.

### <a name="successful-response"></a>A sikeres válasz
A sikeres válasz használó `response_mode=fragment` és `response_type=id_token+token` tűnik hello következő olvashatóságát sortöréssel:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Paraméter | Leírás |
| --- | --- |
| access_token |a kért alkalmazás hello hello jogkivonatot.  hello hozzáférési jogkivonat nem szabad dekódolni, vagy más módon megvizsgálni. Nem átlátszó karakterláncként kezelhető. |
| token_type |hello token objektumtípus-érték. hello csak adja meg, hogy az Azure AD által támogatott tulajdonosi-e. |
| expires_in |Hello, hogy mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| Hatókör |hello token hello hatókörök esetén érvényes. Is használhatja hatókörök toocache jogkivonatok későbbi használatra. |
| id_token |kért alkalmazás hello Azonosítót jogkivonatban hello. Hello azonosító token tooverify hello felhasználói identitás használatára, és a hello felhasználói munkamenet elindításához. Azonosító-jogkivonatokat és azok tartalmát kapcsolatos további információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). |
| state |Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy hello `state` hello kérelem-válasz szereplő értékek azonosak. |

### <a name="error-response"></a>Hibaválaszba
Hibaválaszok is küldhetők toohello átirányítási URI-t, hogy hello alkalmazást is kezeli azokat megfelelően:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc használt tooclassify típusú előforduló hibákat. Is használhatja hello hibakód hibaelhárítási célból. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hitelesítési hiba okának azonosításához. |
| state |Lásd a táblázat megelőző hello hello teljes leírását. Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy hello `state` hello kérelem-válasz szereplő értékek azonosak.|

## <a name="validate-hello-id-token"></a>Hello azonosító token ellenőrzése
Egy azonosító tokent fogadása nincs elég tooauthenticate hello felhasználó. Hello azonosító jogkivonat-aláírás ellenőrzése, és ellenőrizze az alkalmazás követelményeinek / hello jogkivonat hello jogcímek. Használja az Azure AD B2C [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) és nyilvános kulcs titkosítás toosign jogkivonatokat, és győződjön meg arról, hogy azok érvényesek.

Számos nyílt forráskódú kódtárai elérhető JWTs érvényesítése, attól függően, hogy hello nyelvi toouse inkább. Vegye figyelembe a rendelkezésre álló nyílt forráskódú kódtárai felfedezése megvalósító egyéni ellenőrzési logika helyett. Hello információt is használhatja a cikk toohelp elsajátíthatja a tárak tooproperly használatát.

Az Azure AD B2C az OpenID Connect metaadatok végponttal rendelkezik. Egy alkalmazás hello végpont toofetch információ az Azure AD B2C használhatja futásidőben. Ezen információk közé tartozik a végpontok, lexikális elem tartalmának és jogkivonat-aláíró kulcsok. Nincs a JSON metaadat-dokumentum-házirendet az Azure AD B2C-bérlőben. Például hello metaadat-dokumentum hello b2c_1_sign_in házirend hello fabrikamb2c.onmicrosoft.com bérlői helyen található:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

A konfigurációs dokumentum hello tulajdonságait egyik hello `jwks_uri`. hello hello ugyanabban a házirendben lenne a következő:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

milyen házirend volt az ID-tokent használt toosign (, és ahol toofetch hello metaadatok) toodetermine két lehetőség közül választhat. Első lépésként hello házirendnév hello megtalálható `acr` a jogcím `id_token`. Hogyan tooparse hello jogcím-azonosító-jogkivonatból kapcsolatos információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md). A másik lehetőség egy tooencode hello házirend hello hello értékének `state` paraméter hello kérés elküldésekor. Ezt követően dekódolása hello `state` paraméter toodetermine milyen házirend lett megadva. Mindkét módszer esetén érvényes.

Miután hello metaadat-dokumentum hello-metaadatok endpoint OpenID Connect a korábban szerezte be, használhatja a hello RSA-256 (Ez a végpont található) nyilvános kulcsok toovalidate hello hello azonosító jogkivonat aláírását. Előfordulhat, hogy több-kulcsok a végpont egy adott időpontban, minden egyes által azonosított egy `kid`. hello fejlécének `id_token` is tartalmaz egy `kid` jogcímek. Azt jelzi, hogy ezek a kulcsok eddig használt toosign hello azonosító token. További információt, beleértve a megismerését [jogkivonatok ellenőrzése](active-directory-b2c-reference-tokens.md#token-validation), lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve hello information on this-->

Miután hello azonosító jogkivonat aláírását hello ellenőrzéséhez több jogcím ellenőrzés megkövetelése. Példa:

* Hello érvényesítése `nonce` jogcím tooprevent hitelesítési karakterláncok ismétlésének támadásokat. Az érték lehet hello bejelentkezési kérelemben megadott.
* Ellenőrizze a hello `aud` jogcímek, az alkalmazás, amely azonosító token hello tooensure ki. Az érték lehet hello Alkalmazásazonosító az alkalmazás.
* Hello érvényesítése `iat` és `exp` jogcímeket, amely azonosító token hello tooensure nem járt le.

Végre kell hajtania néhány további ellenőrzések hello részletesen ismerteti a [OpenID Connect Core Spec](http://openid.net/specs/openid-connect-core-1_0.html). Érdemes lehet toovalidate további jogcímeket, a forgatókönyvtől függően. Néhány gyakori érvényesítést a következők:

* Győződjön meg arról, hogy hello felhasználó vagy a szervezet rendelkezik-höz készült hello alkalmazást.
* Győződjön meg arról, hogy hello felhasználó rendelkezik-e megfelelő jogosultságokkal és engedélyekkel.
* Győződjön meg arról, hogy bizonyos erősségével hitelesítési történt, ilyen módon az Azure multi-factor Authentication használatával történő.

Egy Azonosítót jogkivonatban hello jogcímek kapcsolatos további információkért lásd: hello [Azure AD B2C-jogkivonatok referenciájából](active-directory-b2c-reference-tokens.md).

Teljesen érvényesítése hello azonosító token után megkezdheti a munkamenet hello felhasználóval. Az alkalmazás hello jogcímek használata a hello azonosító token tooobtain hello felhasználó adatait. Ezek az információk megjelenítése, rekordok, engedélyezési és így tovább használható.

## <a name="get-access-tokens"></a>A hozzáférési jogkivonatok lekérésére
Ha van a web apps igények toodo hello egyedül házirendek hajtható végre, akkor kihagyhatja hello a következő néhány szakasz. a következő részekben hello hello információk olyan alkalmazható csak tooweb alkalmazások, amelyeknek szükségük van a hitelesített toomake hívások tooa webes API, és amelyhez Azure AD B2C-vel védett.

Most, hogy most jelentkezett hello felhasználói tooyour egyoldalas alkalmazásban, hívó webes API-k, az Azure AD által védett kaphat jogkivonatot. Akkor is, ha korábban fogadott levelei jogkivonat hello segítségével `token` választípus nélkül is használható e metódus tooacquire jogkivonatok további erőforrások hello felhasználói toosign a újra átirányítása.

Egy tipikus web app folyamatában, akkor ehhez azáltal, hogy egy kérelem toohello `/token` végpont.  Azonban a hello végpont nem támogatja a CORS kérelmek Igen, így AJAX meghívja a tooget és a frissítési jogkivonatokat a lehetőség nem érhető el. Ehelyett használhatja hello implicit engedélyezési folyamat egy rejtett HTML iframe elemet tooget új jogkivonatokba más webes API-k. Íme egy példa, olvashatóságát sortöréssel:

```

https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
&p=b2c_1_sign_in
```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| client_id |Szükséges |hello Alkalmazásazonosító hozzárendelt hello tooyour alkalmazás [Azure-portálon](https://portal.azure.com). |
| response_type |Szükséges |Tartalmaznia kell `id_token` az OpenID Connect bejelentkezhet.  Az is előfordulhat, hogy tartalmazza a hello választípus `token`. Ha `token` itt, az alkalmazás azonnal fogadhat olyan hozzáférési jogkivonatot hello engedélyezik a végponthoz, anélkül, hogy egy második kérelem toohello végpont hitelesítéséhez. Ha hello `token` választípus, hello `scope` paraméternek tartalmaznia kell egy hatókör, amely jelzi, mely erőforrás tooissue hello lexikális eleme. |
| redirect_uri |Ajánlott |hello átirányítási URI-címe, az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszokat. Azt pontosan egyeznie kell hello átirányítási URI-azonosítók regisztrálta hello portálon, azzal a különbséggel, hogy az URL-kódolású kell lennie. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája.  A jogkivonatok lekérésének tartalmazza a minden hatókör szánt hello erőforrás szükséges. |
| response_mode |Ajánlott |Megadja a által használt toosend hello eredményül kapott jogkivonat hátsó tooyour alkalmazás hello módszert.  Lehet `query`, `form_post`, vagy `fragment`. |
| state |Ajánlott |Hello token válaszként visszaadott hello kérelemben szereplő érték.  Bármilyen tartalmat, hogy toouse karakterlánc lehet.  Általában egy véletlenszerűen generált, egyedi érték használata esetén tooprevent webhelyközi kérések hamisításának megakadályozása támadásokat.  hello is állapota hello felhasználói állapot hello alkalmazásban használt tooencode információ előtt hello hitelesítési kérelmet. Például hello lap vagy nézet hello felhasználói volt. |
| Nonce |Szükséges |Hello alkalmazás, hello eredményül kapott Azonosítót jogkivonatban jogcímként mellékelt által generált hello kérelemben szereplő érték.  hello alkalmazás ezután ellenőrizheti a érték toomitigate hitelesítési karakterláncok ismétlésének támadásokat. Általában hello értéke egy véletlenszerű, egyedi karakterlánc, amely azonosítja a hello kérelem hello forrása. |
| parancssor |Szükséges |a rejtett iframe toorefresh és get jogkivonatokat használja `prompt=none` , hello iframe tooensure nem elakadnak a hello bejelentkezési oldalára, és azonnal visszaadja. |
| login_hint |Szükséges |egy rejtett iframe toorefresh és get jogkivonatokat hello felhasználó felhasználóneve hello szerepeljenek a mutatót toodistinguish hello felhasználó megszerezte az adott egyszerre több munkamenetek között. Akkor is kinyerése hello felhasználónév egy korábbi bejelentkezés hello segítségével `preferred_username` jogcímek. |
| domain_hint |Szükséges |Lehet `consumers` vagy `organizations`.  Frissítése, és a jogkivonatok lekérésének a rejtett iframe, meg kell adni hello `domain_hint` hello kérelem értéke.  Bontsa ki a hello `tid` hello azonosító lexikális eleme egy korábbi bejelentkezési toodetermine a jogcím-mely érték toouse.  Ha hello `tid` jogcím értéke `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`.  Ellenkező esetben használja `domain_hint=organizations`. |

Hello beállítása által `prompt=none` paraméter, a kérelem vagy sikeres vagy sikertelen azonnal, és tooyour alkalmazás.  A sikeres válasz küldött tooyour app jelzett hello: átirányítási URI-címe, hello megadott hello módszerrel `response_mode` paraméter.

### <a name="successful-response"></a>A sikeres válasz
A sikeres válasz használatával `response_mode=fragment` dolgozunk:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Paraméter | Leírás |
| --- | --- |
| access_token |a kért alkalmazás hello hello-tokent. |
| token_type |a jogkivonat típusa hello mindig lesz tulajdonosi. |
| state |Ha egy `state` paraméter szerepel hello ugyanazt az értéket meg kell jelennie a hello válasz hello kérelem. hello app győződjön meg arról, hogy hello `state` hello kérelem-válasz szereplő értékek azonosak. |
| expires_in |Mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| Hatókör |hozzáférési jogkivonat hello hello hatókörök esetén érvényes. |

### <a name="error-response"></a>Hibaválaszba
Hibaválaszok is küldhetők toohello átirányítási URI-t, hogy hello alkalmazást is kezeli azokat megfelelően.  A `prompt=none`, várt hiba néz ki:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely a bekövetkező hibák használt tooclassify típusú lehet. Hello karakterlánc tooreact tooerrors is használhatja. |
| error_description |Egy adott hibaüzenet, melyek segíthetnek hello hitelesítési hiba okának azonosításához. |

Ha ez a hiba hello iframe kérelem érkezik, hello felhasználói interaktív módon be kell jelentkeznie újra tooretrieve egy új jogkivonatot. Ez úgy, hogy az alkalmazás számára legjobb képes kezelni.

## <a name="refresh-tokens"></a>Frissítési jogkivonatok
Azonosító-jogkivonatokat és a hozzáférési jogkivonatok mindkét rövid időn belül lejár. Az alkalmazás rendszeres időközönként ezeket a jogkivonatokat toorefresh elő kell készíteni.  toorefresh mindkét típusú jogkivonatot, hajtsa végre a korábbi példában hello segítségével használtuk kérésben rejtett iframe hello `prompt=none` paraméter toocontrol az Azure AD lépéseket.  új tooreceive `id_token` érték, lehet, hogy toouse `response_type=id_token` és `scope=openid`, és egy `nonce` paraméter.

## <a name="send-a-sign-out-request"></a>Kijelentkezési kérés küldése
Ha azt szeretné, hogy toosign hello felhasználói hello alkalmazásból, átirányítási hello felhasználói tooAzure AD toosign ki. Ha nem így tesz, hello felhasználói lehet képes tooreauthenticate tooyour app hitelesítő adataikkal ismételt beírása nélkül. Ennek az az oka érvényes egyszeri bejelentkezés munkamenetet az Azure ad-val rendelkeznek.

Egyszerűen átirányíthatók hello felhasználói toohello `end_session_endpoint` OpenID Connect metaadat-dokumentum azonos leírt, amely szerepel az hello [ellenőrzése hello azonosító token](#validate-the-id-token). Példa:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Paraméter | Kötelező? | Leírás |
| --- | --- | --- |
| P |Szükséges |hello házirend toouse toosign hello felhasználói kívül az alkalmazást. |
| post_logout_redirect_uri |Ajánlott |hello URL hello felhasználó kell átirányított tooafter sikeres kijelentkezési. Ha nincs megadva, az Azure AD B2C egy általános üzenet toohello felhasználói jeleníti meg. |

> [!NOTE]
> Arra utasíthatja a hello felhasználói toohello `end_session_endpoint` néhány hello felhasználó állapotának egyszeri bejelentkezés az Azure AD B2C törli. Azt azonban nem jelentkezik hello felhasználói hello felhasználói közösségi identity provider munkamenet kívül. Ha hello kijelölés hello azonos szolgáltató azonosítani egy későbbi bejelentkezés során, hello felhasználók újrahitelesítése, a hitelesítő adatok megadása nélkül. Ha egy felhasználó kívül az Azure AD B2C alkalmazás toosign azt szeretné, akkor nem feltétlenül jelenti például kívánják toocompletely jelentkezzen ki a Facebook-fiókjuk. Azonban a helyi fiókok hello felhasználói munkamenet véget ér megfelelően.
> 
> 

## <a name="use-your-own-azure-ad-b2c-tenant"></a>A saját Azure AD B2C bérlő használata
tootry ezek kéri, végezze el a következő három lépésből hello. Cserélje le a hello példaértékeket ebben a cikkben a saját értékeket használjuk:

1. [Az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md). A bérlő hello nevét használja hello kérésekben.
2. [Hozzon létre egy alkalmazást](active-directory-b2c-app-registration.md) tooobtain Azonosítóját és egy `redirect_uri` érték. Vegye fel az alkalmazást egy webalkalmazást vagy webes API. Ha szükséges egy alkalmazás titkos kulcs is létrehozhat.
3. [A házirendek létrehozása](active-directory-b2c-reference-policies.md) tooobtain a házirend nevét.

## <a name="samples"></a>Példák

* [Hozzon létre egy alkalmazás számára a Node.js segítségével](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi)
* [Hozzon létre egy egyoldalas alkalmazást .NET használatával](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi)


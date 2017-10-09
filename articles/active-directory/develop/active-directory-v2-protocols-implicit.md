---
title: "aaaSecure egylapos alkalmazások hello Azure AD v2.0 implicit engedélyezési folyamat használatával |} Microsoft Docs"
description: "Épület webes alkalmazások az Azure AD v2.0 hello implicit engedélyezési folyamat végrehajtása egylapos alkalmazások."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 2cdce4eee88be4af54966d15204b79fa4992a58e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# v2.0 protokoll - gyógyfürdők hello implicit engedélyezési folyamat használata
Hello v2.0-végponttal felhasználók jelentkezhetnek be a Microsoft a személyes és munkahelyi vagy iskolai fiókok egyoldalas alkalmazások.  Egyetlen lap, és más JavaScript-alkalmazásokat, amelyek futtatásához egy böngészőben arc érdekes néhány felkéri mikor elsősorban a előre tooauthentication:

* hello biztonsági jellemzőkkel ezeknek az alkalmazásoknak jelentősen különböznek a hagyományos server-alapú webes alkalmazásokhoz.
* Sok engedélyezési kiszolgálók & identitás-szolgáltatóktól nem támogatják a CORS-kérelmeket.
* Hello app távolabbi szomszédos egész lapon böngésző átirányítások különösen invazív toohello felhasználói élmény válik.

Az alkalmazás (gondolja, hogy: AngularJS, az Ember.js, React.js, stb) az Azure AD hello OAuth 2.0 Implicit Grant flow támogatja.  hello implicit engedélyezési folyamat hello ismertetett [OAuth 2.0 specifikáció](http://tools.ietf.org/html/rfc6749#section-4.2).  A fő előnye, hogy lehetővé teszi hello alkalmazási tooget jogkivonatok az Azure AD a háttér-hitelesítő adat adatcsere végrehajtása nélkül.  Így hello app toosign hello felhasználói munkamenet karbantartása és jogkivonatok tooother webes API-k minden belül hello ügyfélbeli JavaScript-kód beolvasása.  Van néhány fontos biztonsági szempontok tootake figyelembe kifejezetten körülbelül hello implicit engedélyezési folyamat - használatakor [ügyfél](http://tools.ietf.org/html/rfc6749#section-10.3) és [felhasználó megszemélyesítési](http://tools.ietf.org/html/rfc6749#section-10.3).

Ha azt szeretné, toouse hello implicit engedélyezési folyamat és az Azure AD tooadd hitelesítési tooyour JavaScript-alkalmazást, azt javasoljuk, használjon a nyílt forráskódú JavaScript kódtár [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Nincsenek elérhető néhány AngularJS oktatóanyagok [Itt](active-directory-appmodel-v2-overview.md#getting-started) toohelp használatának első lépéseit.  

Ha saját maga inkább a egylapos alkalmazások és a küldési protokoll üzenetek nem toouse egy könyvtár, azonban kövesse az alábbi hello általános lépéseket.

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

## Protokoll diagramja
hello teljes implicit bejelentkezési folyamata az alábbihoz hasonló, mert egyes hello lépéseket ismertetjük részletesen.

![OpenId Connect sávok](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## Hello bejelentkezési kérelem küldése
tooinitially bejelentkezési felhasználói hello az alkalmazásba, küldhet egy [OpenID Connect](active-directory-v2-protocols-oidc.md) engedélyezési kérelem és a get egy `id_token` a v2.0-végponttól hello:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Kattintson az alábbi tooexecute hello hivatkozását, ehhez a kérelemhez! Történő bejelentkezés után a böngésző átirányítsa túl`https://localhost/myapp/` rendelkező egy `id_token` hello címsorába.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>
> 
> 

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett `common`, `organizations`, `consumers`, és a bérlői azonosítókat.  További részletekért lásd: [alapjai protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id |Szükséges |Alkalmazásazonosító, hogy hello regisztrációs portál hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) az alkalmazáshoz hozzárendelt. |
| response_type |Szükséges |Tartalmaznia kell `id_token` az OpenID Connect bejelentkezhet.  Hello response_type is tartalmazhat `token`. Használatával `token` itt lehetővé teszi az alkalmazás tooreceive azonnal hello a hozzáférési token végpont engedélyezéséhez anélkül, hogy egy második kérelem toohello engedélyezik toomake végpont.  Ha hello `token` response_type, hello `scope` paraméternek tartalmaznia kell egy hatókör, amely jelzi, mely erőforrás tooissue hello lexikális eleme. |
| redirect_uri |Ajánlott |az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszok redirect_uri hello.  Azt pontosan egyeznie kell azzal a különbséggel url-kódolású kell lennie az hello portálon regisztrált hello redirect_uris. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája.  Az OpenID Connect, magában kell foglalnia hello hatókör `openid`, amely az eszköz toohello hello hozzájárulási felhasználói felület "Bejelentkezés" engedéllyel.  Opcionálisan is érdemes lehet tooinclude hello `email` vagy `profile` [hatókörök](active-directory-v2-scopes.md) való hozzáférés tooadditional felhasználói adatokat.  A kérelem jóváhagyását toovarious erőforrások kérelmezési más hatókörök is. |
| response_mode |Ajánlott |Határozza meg, amelyeket használt toosend hello eredményül kapott jogkivonat hátsó tooyour app hello módszerét.  Meg kell `fragment` hello implicit engedélyezési folyamat számára. |
| state |Ajánlott |Hello token válaszul is visszaadott hello kérelemben szereplő érték.  Bármely, a kívánt tartalmat karakterlánc lehet.  Egy véletlenszerűen generált egyedi érték jellemzően a [webhelyközi kérések hamisításának megakadályozása támadások megelőzése](http://tools.ietf.org/html/rfc6749#section-10.12).  hello állapota is hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |
| Nonce |Szükséges |Hello kérelmet, amely szerepelni fog a hello eredményül kapott id_token jogcímként hello alkalmazás által generált szerepel érték.  hello alkalmazás ezután ellenőrizheti a érték toomitigate hitelesítési karakterláncok ismétlésének támadásokat.  hello érték általában véletlenszerű, egyedi karakterlánc, amely hello kérelem használt tooidentify hello forrása is lehet. |
| parancssor |Nem kötelező |A felhasználói beavatkozás szükséges hello típusát jelzi.  hello csak most az érvényes értékek: "bejelentkezés", "none", és "".  `prompt=login`rendszer kényszerített hello felhasználói tooenter adott kérelem nem lehet negálni egyszeri bejelentkezést a hitelesítő adataikat.  `prompt=none`hello ellentétes - biztosítani fogja számára nem jelenik meg, hogy a felhasználó hello bármely minden interaktív kérdés.  Ha hello kérelem nem hajtható végre csendes keresztül egyszeri bejelentkezést, hello v2.0-végponttól hibát adnak vissza.  `prompt=consent`rendszer eseményindító hello OAuth hozzájárulás párbeszédpanel hello felhasználó jelentkezik be, miután hello felhasználói toogrant engedélyek toohello alkalmazást kéri. |
| login_hint |Nem kötelező |Kell használt toopre-kitöltés hello felhasználónév vagy e-mail cím mező hello a bejelentkezés lap hello felhasználó, ha tudja, hogy időben a felhasználónevét.  Gyakran alkalmazások ismételt hitelesítés, hogy már kivont hello felhasználónév előző bejelentkezés során fogja használni a paraméter használatával hello `preferred_username` jogcímek. |
| domain_hint |Nem kötelező |Egyike lehet `consumers` vagy `organizations`.  Ha tartalmazza, akkor kihagyja hello e-mail alapú felderítési folyamat, hogy a felhasználó végighalad a hello v2.0 bejelentkezési oldalára, ami tooa nagyobb jelentőséggel zökkenőmentes felhasználói élmény.  Gyakran alkalmazások használja ezt a paramétert ismételt hitelesítés során hello beolvasásával `tid` hello id_token származó jogcímek.  Ha hello `tid` jogcím értéke `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`.  Ellenkező esetben használja `domain_hint=organizations`. |

Ezen a ponton hello felhasználó fog tooenter kéri a hitelesítő adatokat és a teljes hello hitelesítés.  hello v2.0-végponttól is biztosítják, hogy hello a felhasználó hozzájárult hello jelzett toohello engedélyek `scope` lekérdezési paraméter.  Ha hello felhasználó nem hozzájárult tooany, ezeket az engedélyeket, a program kéri hello felhasználói tooconsent toohello szükséges engedélyekkel.  A részletek [engedélyek, beleegyezése és több-bérlős alkalmazásokhoz itt megadott](active-directory-v2-scopes.md).

Miután hello felhasználó hitelesíti, és engedélyezi a hozzájárulási hello v2.0-végponttól ad vissza egy válasz tooyour alkalmazást jelzett hello `redirect_uri`, hello megadott hello metódussal `response_mode` paraméter.

#### A sikeres válasz
A sikeres válasz használatával `response_mode=fragment` és `response_type=id_token+token` tűnik hello következő olvashatóságát sortöréssel:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Paraméter | Leírás |
| --- | --- |
| access_token |Belefoglalt if `response_type` tartalmaz `token`. hello hozzáférési jogkivonatot, amely a kért, alkalmazás hello ebben az esetben a Microsoft Graph hello.  hello hozzáférési jogkivonat nem szabad dekódolni, vagy más módon megvizsgálni, azt nem átlátszó karakterláncként kell kezelni. |
| token_type |Belefoglalt if `response_type` tartalmaz `token`.  Mindig `Bearer`. |
| expires_in |Belefoglalt if `response_type` tartalmaz `token`.  Azt jelzi, hogy hello hány másodpercig hello token érvénytelen, gyorsítótárazás céljából. |
| Hatókör |Belefoglalt if `response_type` tartalmaz `token`.  Azt jelzi, hogy mely hello access_token érvényes lesz hello hatókörök. |
| id_token |hello id_token, amely a kért alkalmazás hello. Hello id_token tooverify hello felhasználói identitás használatára, és a hello felhasználói munkamenet elindításához.  További részleteket a id_tokens és azok tartalmát megtalálható hello [v2.0 jogkivonat végponthivatkozás](active-directory-v2-tokens.md). |
| state |Ha a state paraméter hello kérelem, ugyanazt az értéket meg kell jelennie a hello válasz hello is szerepel. hello app győződjön meg arról, hogy a hello állapotértékek hello kérés és válasz megegyeznek-e. |

#### Hibaválaszba
Hibaválaszok is küldhet toohello `redirect_uri` , ezek a hello alkalmazást is megfelelően kezelése:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |

## Hello id_token ellenőrzése
Csak fogadó egy id_token nincs elegendő tooauthenticate hello felhasználói; hello id_token aláírás érvényesítése kell, és ellenőrizze az alkalmazás követelményeinek / hello jogkivonat hello jogcímek.  hello v2.0-végponttól használ [JSON Web Tokens (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) és nyilvános kulcs titkosítás toosign jogkivonatokat, és győződjön meg arról, hogy azok érvényesek.

Választhat toovalidate hello `id_token` ügyfél kódban, de a gyakori eljárás toosend hello `id_token` tooa háttérkiszolgáló, és végezze el a hello érvényesítési hiba.  Hello id_token hello aláírása ellenőrizte, ha nincsenek tooverify szükség lesz néhány jogcímeket.  Lásd: hello [v2.0 jogkivonat referenciái](active-directory-v2-tokens.md) további információkért többek között [jogkivonatok ellenőrzése](active-directory-v2-tokens.md#validating-tokens) és [fontos információk kapcsolatos aláírási kulcs átfordulási](active-directory-v2-tokens.md#validating-tokens).  Azt javasoljuk, elemzés és ellenőrzése a szalagtár alkalmazó jogkivonatok - van legalább egy legtöbb nyelvekhez és platformokhoz érhető el.
<!--TODO: Improve hello information on this-->

Kezdésként érdemes lehet is toovalidate további jogcímeket a forgatókönyvtől függően.  Néhány gyakori érvényesítést a következők:

* Hello felhasználó/szervezeti biztosítása rendelkezik regisztrált hello alkalmazást.
* Biztosítani kell hello felhasználó rendelkezik-e megfelelő engedélyezési vagy jogosultságokkal
* Bizonyos erősségével hitelesítés úgy történt, például a többtényezős hitelesítést.

Hello jogcím szerepel egy id_token további információkért lásd: hello [v2.0 jogkivonat végponthivatkozás](active-directory-v2-tokens.md).

Teljesen ellenőrzése hello id_token, után kezdje hello felhasználói munkamenetet, és hello jogcímek használata a hello id_token tooobtain hello felhasználó adatait az alkalmazás.  Ez az információ alkalmas megjelenített, rekordok, engedélyek, stb.

## A hozzáférési jogkivonatok lekérésére
Most, hogy az egyetlen oldal alkalmazásba hello felhasználói most jelentkezett, kaphat jogkivonatot hívó webes API-k által az Azure AD, például a hello védett [Microsoft Graph](https://graph.microsoft.io).  Akkor is, ha már érkezett egy token hello használatával `token` response_type, használhatja a metódus tooacquire jogkivonatok tooadditional erőforrások nélkül tooredirect hello felhasználói toosign újra.

Hello normál OpenID Connect/OAuth folyamatában, akkor ehhez azáltal, hogy egy kérelem toohello v2.0 `/token` végpont.  Azonban a hello v2.0-végpontra nem támogatja a CORS kérelmek Igen, így AJAX meghívja a tooget és frissítési jogkivonatokat hello kérdés kívül esik.  Ehelyett használhatja hello implicit engedélyezési folyamat egy rejtett iframe tooget új jogkivonatokba más webes API-kat: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [!TIP]
> Másolás & kérelem alatt hello beillesztése a böngészőlapon próbálja! (Ne feledje tooreplace hello `domain_hint` és hello `login_hint` hello értékeket, javítsa ki a felhasználó értékeket)
> 
> 

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett `common`, `organizations`, `consumers`, és a bérlői azonosítókat.  További részletekért lásd: [alapjai protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id |Szükséges |Alkalmazásazonosító, hogy hello regisztrációs portál hello ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) az alkalmazáshoz hozzárendelt. |
| response_type |Szükséges |Tartalmaznia kell `id_token` az OpenID Connect bejelentkezhet.  Például a más response_types is tartalmazhat `code`. |
| redirect_uri |Ajánlott |az alkalmazás, ahol küldött és az alkalmazás által fogadott a hitelesítési válaszok redirect_uri hello.  Azt pontosan egyeznie kell azzal a különbséggel url-kódolású kell lennie az hello portálon regisztrált hello redirect_uris. |
| Hatókör |Szükséges |Hatókörök szóközökkel elválasztott listája.  A jogkivonatok lekérésének tartalmazza az összes [hatókörök](active-directory-v2-scopes.md) érdeklő hello erőforrás van szüksége. |
| response_mode |Ajánlott |Határozza meg, amelyeket használt toosend hello eredményül kapott jogkivonat hátsó tooyour app hello módszerét.  Egyike lehet `query`, `form_post`, vagy `fragment`. |
| state |Ajánlott |Hello token válaszul is visszaadott hello kérelemben szereplő érték.  Bármely, a kívánt tartalmat karakterlánc lehet.  Egy véletlenszerűen generált egyedi érték webhelyközi kérések hamisításának megakadályozása támadások megelőzése általában szolgál.  hello állapota is hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |
| Nonce |Szükséges |Hello kérelmet, amely szerepelni fog a hello eredményül kapott id_token jogcímként hello alkalmazás által generált szerepel érték.  hello alkalmazás ezután ellenőrizheti a érték toomitigate hitelesítési karakterláncok ismétlésének támadásokat.  hello érték általában véletlenszerű, egyedi karakterlánc, amely hello kérelem használt tooidentify hello forrása is lehet. |
| parancssor |Szükséges |Frissítés & rejtett iframe a jogkivonatok lekérésének, érdemes használni a `prompt=none` , amely hello iframe tooensure nem lefagy hello v2.0 bejelentkezési oldalára, és azonnal visszaadja. |
| login_hint |Szükséges |Frissítés & rejtett iframe a jogkivonatok lekérésének, meg kell adni hello felhasználó felhasználóneve hello a mutatóban szereplő sorrendben toodistinguish hello felhasználó rendelkezhet egy időben több munkamenetek között. Az előző bejelentkezés is kibonthat egy hello felhasználónév hello segítségével `preferred_username` jogcímek. |
| domain_hint |Szükséges |Egyike lehet `consumers` vagy `organizations`.  Frissítés & rejtett iframe a jogkivonatok lekérésének, meg kell adni hello domain_hint hello kérelemben.  Ki kell bontania hello `tid` hello id_token egy korábbi bejelentkezési toodetermine, a jogcím-mely érték toouse.  Ha hello `tid` jogcím értéke `9188040d-6c67-4c5b-b112-36a304b66dad`, használjon `domain_hint=consumers`.  Ellenkező esetben használja `domain_hint=organizations`. |

Köszönjük, toohello `prompt=none` paraméter, a kérelem vagy sikeres vagy meghiúsul azonnal, és térjen vissza a tooyour alkalmazás.  A sikeres válasz küld tooyour app: hello jelzett `redirect_uri`, hello megadott hello metódussal `response_mode` paraméter.

#### A sikeres válasz
A sikeres válasz használatával `response_mode=fragment` láthatóhoz hasonló:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Paraméter | Leírás |
| --- | --- |
| access_token |a kért alkalmazás hello hello-tokent. |
| token_type |Mindig `Bearer`. |
| state |Ha a state paraméter hello kérelem, ugyanazt az értéket meg kell jelennie a hello válasz hello is szerepel. hello app győződjön meg arról, hogy a hello állapotértékek hello kérés és válasz megegyeznek-e. |
| expires_in |Mennyi ideig hello hozzáférési jogkivonat érvénytelen (másodpercben). |
| Hatókör |hozzáférési jogkivonat hello hello hatókörök esetén érvényes. |

#### Hibaválaszba
Hibaválaszok is küldhet toohello `redirect_uri` , ezek a hello alkalmazást is megfelelően kezelése.  Hello esetében `prompt=none`, várt hiba történt a következő lesz:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Paraméter | Leírás |
| --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hitelesítési hiba okának azonosításához. |

Ha ez a hiba hello iframe kérelem érkezik, hello felhasználói interaktív módon be kell jelentkeznie újra tooretrieve egy új jogkivonatot.  Választhatja ki toohandle ebben az esetben bármilyen módon szabálykészletében az alkalmazáshoz.

## Jogkivonatok frissítése
Mindkét `id_token`s és `access_token`s rövid időn belül lejár, így az alkalmazás toorefresh elő kell készíteni a rendszeres időközönként jogkivonatokat.  vagy írja be a jogkivonat toorefresh, hajthat végre hello promptjai hello segítségével kérésben rejtett iframe `prompt=none` paraméter toocontrol az Azure AD viselkedését.  Ha azt szeretné, hogy új tooreceive `id_token`, lehet, hogy toouse `response_type=id_token` és `scope=openid`, valamint egy `nonce` paraméter.

## A Kijelentkezés kérelem küldése
hello OpenIdConnect `end_session_endpoint` lehetővé teszi az alkalmazás toosend egy kérelem toohello v2.0 végpont tooend a felhasználó munkamenetét, és törölje a cookie-k állította hello v2.0-végponttól.  toofully jelentkezzen ki egy webes alkalmazás a felhasználó, az alkalmazás kell Befejezés saját hello felhasználói munkamenetet (általában egy token gyorsítótár kiürítése, vagy eldobja a cookie-k), majd irányítsa át hello böngészőben:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Paraméter |  | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |Hello `{tenant}` hello hello kérelem elérési út értéke lehet használt toocontrol, akik bejelentkezhetnek hello alkalmazásba.  hello két érték engedélyezett `common`, `organizations`, `consumers`, és a bérlői azonosítókat.  További részletekért lásd: [alapjai protokoll](active-directory-v2-protocols.md#endpoints). |
| post_logout_redirect_uri | Ajánlott | hello URL-címet, amely a felhasználó hello vissza kell adni az tooafter kijelentkezési befejeződik. Ezt az értéket meg kell egyeznie egy hello átirányítási URI-azonosítók regisztrált hello alkalmazáshoz. Nem tartalmazza, hello felhasználói megjelenik-e egy általános üzenet által hello v2.0-végponttól. |

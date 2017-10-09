---
title: "Az Azure Active Directory B2C: Lexikális elem: referencia |} Microsoft Docs"
description: "az Azure Active Directory B2C kiállított jogkivonatokat hello típusai"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/17/2017
ms.author: dastrock
ms.openlocfilehash: 776bc88cc0308875ac6e576f19ac9edf50319d08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-token-reference"></a>Az Azure AD B2C: Token referencia
Az Azure Active Directory B2C (az Azure AD B2C) számos különböző típusú biztonsági jogkivonatokat bocsát ki, amint az feldolgozza azokat egyes [hitelesítési folyamat](active-directory-b2c-apps.md). Ez a dokumentum ismerteti a hello formátumát, a biztonsági jellemzőkkel és a különböző típusú lexikális elem tartalmát.

## <a name="types-of-tokens"></a>A jogkivonatok típusok
Az Azure AD B2C támogatja hello [OAuth 2.0 protokoll](active-directory-b2c-reference-protocols.md), amely él mindkét hozzáférési jogkivonatok és frissítési jogkivonatok. Is támogatja a hitelesítés és bejelentkezés keresztül [OpenID Connect](active-directory-b2c-reference-protocols.md), amely bevezet egy harmadik típusú jogkivonat: hello azonosító token. Egyes ezeket a jogkivonatokat tulajdonosi jogkivonattal jelzi.

Egy tulajdonosi jogkivonatot egy egyszerűsített biztonsági jogkivonatot, hogy biztosít hello "tulajdonos" hozzáférési tooa erőforrás védelemmel ellátva. hello tulajdonosi, akik hello token félre. Az Azure AD először hitelesíteniük kell egy entitás előtt megkaphatja a tulajdonosi jogkivonattal. De ha hello szükséges lépések nem veszi toosecure hello lexikális elem szerepel az átvitel és a tárolási, azt is hozzá és egy nem kívánt entitás használja. Néhány biztonsági jogkivonatokat rendelkezik egy olyan beépített mechanizmus meggátolja, hogy a nem hitelesített felek használja őket, de tulajdonosi jogkivonatok nem rendelkezik a mechanizmus. Azok a biztonságos csatornákat, például a transport layer security (HTTPS) kell szállítani.

Egy tulajdonosi jogkivonatot kívül egy biztonságos csatornán kerül továbbításra, ha egy rosszindulatú entitás-átjárójának támadás tooacquire hello jogkivonatot használja, és toogain jogosulatlan hozzáférés tooa védett erőforrás használja. hello ugyanazt biztonsági elveket alkalmazza, ha a tulajdonosi jogkivonatok tárolt és gyorsítótárba helyezni későbbi használat. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon tárolja a tulajdonosi jogkivonatokhoz.

További biztonsági szempontok a tulajdonosi jogkivonatok, lásd: [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750).

Hello jogkivonatokat állít ki az Azure AD B2C számos használják, mint a JSON webes jogkivonatok (JWTs). Jwt-t egy kompakt biztonságos URL-címet a eszközöket információk átvitele a két fél között. JWTs jogcímekként ismert információkat tartalmaznak. Ezek a helyességi feltételek hello tulajdonosi információinak és hello tulajdonos hello jogkivonat. a rendszer kódolása, és a továbbításra szerializált JSON-objektumok a JWTs hello jogcímeket. Az Azure AD B2C által kibocsátott JWTs hello aláírt, de nincs titkosítva, mert könnyen vizsgálhatja meg a JWT toodebug hello tartalmát azt. Több eszközök érhetők el, amely ehhez, beleértve a [calebb.net](http://calebb.net). JWTs kapcsolatos további információkért tekintse meg túl[JWT specifikációk](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Azonosító-jogkivonatokat
Egy azonosító jogkivonat a biztonsági jogkivonatot, amely az alkalmazás fogad az Azure AD B2C hello egy formája, amely `authorize` és `token` végpontok. Azonosító-jogkivonatokat helyettesítik [JWTs](#types-of-tokens), és használható tooidentify felhasználók a saját alkalmazásában jogcímeket tartalmaznak. Ha azonosító-jogkivonatokat szerezni hello `authorize` végpontot, azok gyakran használt toosign felhasználók tooweb alkalmazásokban. Ha azonosító-jogkivonatokat szerezni hello `token` végpont, küldheti a HTTP-kérelmek hello két összetevői közötti kommunikáció során azonos alkalmazást vagy szolgáltatást. Hello jogcímek használata egy Azonosítót jogkivonatban igényei szerint. Azok a gyakran használt toodisplay fiók információkat vagy toomake hozzáférés-vezérlési döntéseket alkalmazásban.  

Azonosító-jogkivonatokat írt alá, de jelenleg nincsenek titkosítva. Ha az alkalmazás vagy API kap egy azonosító jogkivonatot, kell [hello aláírás érvényesítése](#token-validation) , amely a hello token tooprove hiteles. Az alkalmazás vagy API is ellenőrizni kell néhány jogcím szerepel hello token tooprove adatok érvényességét. Hello forgatókönyv-követelményeinek, attól függően, hogy az alkalmazás által érvényesített hello jogcímek eltérőek lehetnek, de az alkalmazás kell végeznie néhány [közös jogcím érvényesítést](#token-validation) minden forgatókönyvben.

#### <a name="sample-id-token"></a>Minta azonosító tokent
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Hozzáférési jogkivonatok
Olyan hozzáférési jogkivonatot is egy formája, amely a biztonsági jogkivonatot, amely az alkalmazás fogad az Azure AD B2C hello `authorize` és `token` végpontok. Hozzáférési jogkivonatok is helyettesítik [JWTs](#types-of-tokens), és használható tooidentify hello engedélyek tooyour API-k kap jogcímeket tartalmaznak. Hozzáférési jogkivonatok írt alá, de jelenleg nincsenek titkosítva. Hozzáférési jogkivonatok használt tooprovide access tooAPIs és erőforrás-kiszolgálóknak kell lennie. További tudnivalók túl[használja a hozzáférési jogkivonatok](active-directory-b2c-access-tokens.md). 

Ha az API-t kap egy hozzáférési jogkivonatot, kell [hello aláírás érvényesítése](#token-validation) , amely a hello token tooprove hiteles. Az API-t is ellenőrizni kell néhány jogcím szerepel hello token tooprove adatok érvényességét. Hello forgatókönyv-követelményeinek, attól függően, hogy az alkalmazás által érvényesített hello jogcímek eltérőek lehetnek, de az alkalmazás kell végeznie néhány [közös jogcím érvényesítést](#token-validation) minden forgatókönyvben.

### <a name="claims-in-id-and-access-tokens"></a>Az ID és a hozzáférési jogkivonatok jogcímek
Az Azure AD B2C használatakor befolyásolni részletes a tokenek hello tartalmát. Konfigurálható [házirendek](active-directory-b2c-reference-policies.md) bizonyos toosend állítja be az alkalmazás a műveleteket igényel jogcímeket a felhasználói adatok. Ezek a jogcímek tartalmazhatják szabványos tulajdonságait, például hello felhasználói `displayName` és `emailAddress`. Tartalmazhatják [felhasználói egyéni attribútumok](active-directory-b2c-reference-custom-attr.md) megadható a B2C-címtárban. Minden azonosítója és a hozzáférési jogkivonatot kapott biztonsági jogcímek az egyes készletét tartalmazza. Az alkalmazások használhatják ezeket a jogcímeket toosecurely felhasználók és a kérelmek hitelesítéséhez.

Figyelje meg, hogy bármely adott sorrendben nem lehet megjeleníteni a azonosító-jogkivonatokat hello jogcímeket. Emellett új jogcímeket is bevezetni azonosító-jogkivonatokat bármikor. Az alkalmazás nem kell törés az új jogcímeket belépéskor. Az alábbiakban a hello jogcím-azonosító és a hozzáférési jogkivonatok az Azure AD B2C által kibocsátott tooexist várt. További jogcímeket házirendek határozzák meg. Eljárás, próbálja ki hello jogcímek hello minta Azonosítót jogkivonatban ellenőrzése illeszti be [calebb.net](http://calebb.net). További részletek találhatók a hello [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html).

| Név | Jogcím | Példaérték | Leírás |
| --- | --- | --- | --- |
| Célközönség |`aud` |`90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` |Egy célközönség jogcím szánt hello címzett hello jogkivonat azonosítja. Az Azure AD B2C-ben hello célközönségét az alkalmazás azonosítója, hello app regisztrációs portálon rendelt tooyour alkalmazásként. Az alkalmazás kell ellenőrizni az értékét, és utasítsa el hello tokent, ha nem felel meg. |
| Kiállító |`iss` |`https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` |A jogcím azonosítja hello biztonságijogkivonat-szolgáltatás (STS) hoz létre, és hello jogkivonatot ad vissza. Meghatározza azt is, mely hello a felhasználó hitelesítési hello Azure AD-címtárban. Az alkalmazás érdemes ellenőrizni a hello kibocsátó jogcímet, amely a hello token tooensure hello Azure Active Directory v2.0-végponttól érkezett. |
| Ki: |`iat` |`1438535543` |Ezt az igényt az hello idő, mely hello token adta ki, epoch idő. |
| Lejárati idő |`exp` |`1438539443` |hello lejárati ideje nincs hello idő, mely hello token válik érvénytelen, az időbeli kortartományon kezeli őket. Az alkalmazás használja a jogkivonatok élettartama hello jogcím tooverify hello érvényességét. |
| Hatálybalépési idő |`nbf` |`1438535543` |Ezt az igényt az hello idő, mely hello token lesz érvényes, az időbeli kortartományon kezeli őket. Ez az általában hello azonos módon hello idő hello token ki. Az alkalmazás használja a jogkivonatok élettartama hello jogcím tooverify hello érvényességét. |
| Verzió |`ver` |`1.0` |Hello verziójáról hello azonosító jogkivonat, az Azure AD által definiált. |
| Kód kivonata |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Kód kivonatát része egy Azonosítót jogkivonatban csak amikor hello token együtt egy OAuth 2.0 hitelesítési kódot. A kód kivonatoló lehet használt toovalidate hello hitelességének egy engedélyezési kódot. További részletes információt a tooperform ezen ellenőrzés hello lásd [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html).  |
| Hozzáférési jogkivonat kivonata |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Az access token kivonatoló része egy Azonosítót jogkivonatban csak akkor, ha hello token együtt OAuth 2.0 hozzáférési tokent ad ki. Az access token kivonatoló lehet használt toovalidate hello hitelességének olyan hozzáférési jogkivonatot. Nyújt további tájékoztatást tooperform ezen ellenőrzés lásd: hello [OpenID Connect meghatározása](http://openid.net/specs/openid-connect-core-1_0.html)  |
| Nonce |`nonce` |`12345` |Egy egyszeri üzenet, stratégia használt toomitigate hitelesítési karakterláncok ismétlésének támadásokat. Az alkalmazás megadhat egy nonce engedélyezési kérelmet hello segítségével `nonce` lekérdezési paraméter. a hello változtatás nélkül hello kérelemben megadja hello érték lesz kibocsátott `nonce` jogcím csak egy azonosító jogkivonat. Ez lehetővé teszi az alkalmazás tooverify hello érték hello kérésére, amely az adott azonosító jogkivonat hello app munkamenet társítja azt megadott hello értékkel. Az alkalmazás végre kell hajtania az ellenőrzés hello azonosító token érvényesítési folyamat során. |
| Tárgy |`sub` |`884408e1-2918-4cz0-b12d-3aa027d7563b` |Ez a mely hello kapcsolatos token állításokat információkat, például egy alkalmazás hello felhasználói hello rendszerbiztonsági tag. Ez az érték nem módosítható és nem lehet újbóli hozzárendelése és nem használja fel újra. Ez lehet a használt tooperform engedélyezési ellenőrzések biztonságosan, amikor hello jogkivonat nem használt tooaccess erőforrás. Alapértelmezés szerint a hello tulajdonos jogcím telepítéskor hello Objektumazonosító hello felhasználó hello könyvtárban. toolearn több, lásd: [Azure Active Directory B2C: Token, munkamenet és egyszeri bejelentkezés konfigurációs](active-directory-b2c-token-session-sso.md). |
| Hitelesítési környezeti osztályait ismertető dokumentációban |`acr` |Nem alkalmazható |Jelenleg nem használt, kivéve a hello kis-és régebbi házirendek. toolearn több, lásd: [Azure Active Directory B2C: Token, munkamenet és egyszeri bejelentkezés konfigurációs](active-directory-b2c-token-session-sso.md). |
| Megbízhatósági keretrendszer házirend |`tfp` |`b2c_1_sign_in` |Ez az hello házirend, de a használt tooacquire hello azonosító token hello nevét. |
| Hitelesítési idő |`auth_time` |`1438535543` |Ez nincs hello ideje, amellyel egy felhasználó utolsó megadott hitelesítő adatokat, epoch idő. |

### <a name="refresh-tokens"></a>Frissítési jogkivonatok
Frissítési jogkivonatok olyan biztonsági jogkivonatok, hogy az alkalmazás használhat tooacquire új azonosító-jogkivonatokat és hozzáférési jogkivonatok az OAuth 2.0 folyamatból. Ezek biztosítanak az alkalmazás a hosszú távú hozzáférés tooresources felhasználók nevében anélkül, hogy azoknak a felhasználóknak való együttműködéshez.

a token válasz egy frissítési jogkivonat tooreceive, az alkalmazás hello kell igényelnie `offline_acesss` hatókör. További információk hello toolearn `offline_access` hatókörét, tekintse meg a toohello [az Azure AD B2C protokoll referenciája](active-directory-b2c-reference-protocols.md).

Frissítési jogkivonatok, és mindig lesz, nem teljesen átlátszó tooyour alkalmazást. Azok az Azure AD által kiállított és ellenőrzött és csak az Azure AD értelmezi. Hosszú élettartamú, de az alkalmazás nem írható a hello elvárás, hogy egy frissítési jogkivonat egy meghatározott ideig tart. Frissítési jogkivonatokat a számos okból bármikor érvénytelenített lehet. csak az alkalmazás tooknow, ha a frissítési jogkivonat érvényes megoldást tooattempt tooredeem hello azáltal, hogy egy kérés tooAzure AD azt.

Ha beváltja egy frissítési jogkivonat vagy egy új jogkivonatot (és ha az alkalmazás rendelkezik hello `offline_access` hatókör), egy új frissítési jogkivonat hello token választ kap. Mentse az újonnan kiadott hello frissítési jogkivonat. Az hello frissítési jogkivonat hello kérelemben használt váltja fel. Ez segít garantálni, hogy a frissítési jogkivonatokat maradnak, amíg érvényes.

## <a name="token-validation"></a>Jogkivonatok érvényesség-ellenőrzése
toovalidate jogkivonatot, az alkalmazás ellenőrizze hello aláírás és a jogcímek hello jogkivonat.

Számos nyílt forráskódú szalagtárak JWTs, attól függően, hogy a választott nyelv ellenőrzésével kapcsolatos érhetők el. Azt javasoljuk, hogy a saját ellenőrzési logika megvalósítása helyett inkább megismerheti azokat a beállításokat. hello információkat az útmutató segítségével megtudhatja, hogyan tooproperly használja ezeket a könyvtárakat.

### <a name="validate-hello-signature"></a>Hello aláírás érvényesítése
A jwt-t tartalmaz három szegmensből, hello `.` karakter. hello első szegmens pedig az hello *fejléc*, hello második hello *törzs*, és hello harmadik hello *aláírás*. hello aláírás szegmens használt toovalidate hello hitelességének hello token lehet, hogy az alkalmazás által megbízhatóak.

Az Azure AD B2C-jogkivonatok aláírt szabványos aszimmetrikus titkosítási algoritmusok, például RSA 256. titkosítási módszer használt toosign hello token és hello fejléc hello jogkivonat hello kulccsal kapcsolatos információkat tartalmazza:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Hello `alg` jogcím azt jelzi, de a használt toosign hello token hello algoritmus. Hello `kid` jogcím hello adott nyilvános kulcsot, de a használt toosign hello token jelzi.

Egy adott időpontban az Azure AD bármikor beléphet a jogkivonat közül legalább egy nyilvános-titkos kulcspárokat bizonyos készlet használatával. Az Azure AD rendszeres időközönként forog hello lehetséges kulcsok készlete, így az alkalmazás automatikusan megváltoztatja e kulcs toohandle kell írni. A frissítések toohello nyilvános kulcsok Azure AD által használt egy ésszerű gyakoriság toocheck 24 óránként van.

Az Azure AD B2C az OpenID Connect metaadatok végponttal rendelkezik. Ez lehetővé teszi alkalmazások toofetch információ az Azure AD B2C futásidőben. Ezen információk közé tartozik a végpontok, lexikális elem tartalmának és jogkivonat-aláíró kulcsok. A B2C-címtárat a JSON-házirendet metaadat-dokumentum tartalmazza. Például hello metaadat-dokumentum a hello `b2c_1_sign_in` -szabályzat `fabrikamb2c.onmicrosoft.com` helyen található:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`a hello B2C directory használt tooauthenticate hello felhasználó, és `b2c_1_sign_in` a hello házirend használt tooacquire hello jogkivonat. milyen házirend használt toosign jogkivonat (és ahol toogo toofetch hello metaadatok), toodetermine két lehetőség közül választhat. Első lépésként hello házirendnév hello megtalálható `acr` jogcím hello jogkivonat. Jogcímek hello törzsét hello JWT base-64 dekódolási hello szervezet kívül tudja értelmezni, és JSON karakterlánc deszerializálása hello adott eredmények. Hello `acr` jogcím hello házirend, de a használt tooissue hello token hello neve lesz.  A másik lehetőség egy tooencode hello házirend hello hello értékének `state` paraméter küldjön hello kérelmet, és milyen házirend lett megadva toodetermine dekódolására. Mindkét módszer esetén érvényes.

hello metaadat-dokumentum néhány hasznos adatot tartalmazó JSON-objektum. Ezek közé tartozik a hello végpontok szükséges tooperform OpenID Connect hitelesítési hello helyét. Ezek közé tartoznak `jwks_uri`, ami hello készlete, amelyek toosign használt nyilvános kulcsok hello helyét. Itt valósul meg erre a helyre, de ajánlott toofetch hello hely dinamikusan hello metaadat-dokumentum használatával, és a kimenő elemzése `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

az URL-címen található hello JSON-dokumentum tartalmaz minden hello nyilvánoskulcs-adatokat egy adott pillanatban használatban. Az alkalmazás használhat hello `kid` hello JWT fejléc tooselect hello levő nyilvános kulcs, amely használt toosign hello JSON-dokumentum a jogcím egy adott jogkivonatot. Majd hello megfelelő nyilvános kulccsal és hello jelzett algoritmus használatával végezheti el az aláírás érvényesítése.

Ez a dokumentum hatókörén kívül hello tooperform aláírás érvényesítése Mitől leírása. Számos nyílt forráskódú szalagtárak elérhető toohelp való ez, ha esetleg szükség lenne rá.

### <a name="validate-hello-claims"></a>Hello ellenőrizhesse
Ha az alkalmazás vagy API kap egy azonosító jogkivonatot, azt kell is ellenőrzi a több elleni hello jogcímek hello Azonosítót jogkivonatban. Ezek közé tartozik, de nem korlátozva:

* Hello **célközönség** igényelni: Ez ellenőrzi, hogy hello azonosító token lett tooyour app megadott tervezett toobe.
* Hello **nem előtt** és **lejárati idő** jogcímek: ezek győződjön meg arról, hogy hello azonosító jogkivonat nem járt le.
* Hello **kibocsátó** igényelni: Ez ellenőrzi, hogy hello token tooyour app adta ki az Azure ad.
* Hello **nonce**: Ez a hitelesítési karakterláncok ismétlésének támadás megoldás stratégiát is.

Érvényesítést, végre kell hajtania, az alkalmazás teljes listájáért tekintse meg a toohello [OpenID Connect specification](https://openid.net). A jogcímek hello várt értékek részleteit szerepelnek előző hello [szakasz token](#types-of-tokens).  

## <a name="token-lifetimes"></a>Token élettartama
a következő token élettartama hello vannak megadva toofurther a Tudásbázis. Akkor lehet hasznos, ha Ön által fejlesztett és alkalmazások hibakeresését. Vegye figyelembe, hogy az alkalmazások nem lehet írt tooexpect bármely ezek élettartama tooremain konstans. Akkor is, és változni fog. További információk a hello [token élettartamának testreszabása](active-directory-b2c-token-session-sso.md) az Azure AD B2C.

| Token | Élettartam | Leírás |
| --- | --- | --- |
| Azonosító-jogkivonatokat |Egy óra |Azonosító-jogkivonatokat érvényesek általában egy óra. A webalkalmazás a élettartama toomaintain használhatják a saját munkamenettel rendelkező felhasználók (ajánlott). A különböző munkamenetek élettartamát is beállíthatja. A tooget azonosító új tokent kell, hogy egyszerűen szükséges toomake egy új bejelentkezési kérelme tooAzure AD. Ha egy felhasználó egy érvényes böngésző-munkamenet az Azure ad-vel, hogy a felhasználó nem feltétlenül szükséges tooenter hitelesítő adatok újra. |
| Frissítési jogkivonatok |Too14 nap mentése |Egyetlen frissítési jogkivonat esetén legfeljebb 14 napig érvényes. Azonban a frissítési jogkivonat válhat érvénytelen számos oka bármikor. Az alkalmazás továbbra tootry toouse egy frissítési jogkivonat, amíg hello kérelem sikertelen lesz, vagy az alkalmazás egy új frissítési jogkivonat hello lecseréli. A frissítési jogkivonat érvénytelen, ha 90 nap eltelt óta hello felhasználó utoljára megadott hitelesítő adatok is válhat. |
| Hitelesítési kódok |Öt perc |Hitelesítési kódok szándékosan rövid élettartamú. Akkor kell váltható azonnal a hozzáférési jogkivonatok, azonosító-jogkivonatokat, vagy a frissítési jogkivonatokat érkezésükkor. |


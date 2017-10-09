---
title: aaaAzure Active Directory v2.0 jogkivonatok referencia |} Microsoft Docs
description: "az Azure AD v2.0-végpontra a hello által kibocsátott hello jogkivonatok és jogcímek különböző típusairól"
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 29eb5c3402aeae302ee7c6234488520495f85904
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-v20-tokens-reference"></a>Az Azure Active Directory v2.0 jogkivonatok referenciái
hello Azure Active Directory (Azure AD) v2.0-végponttól bocsát ki biztonsági jogkivonatokat az egyes többféle típusú [hitelesítési folyamat](active-directory-v2-flows.md). Ez az útmutató ismerteti, hello formátumát, a biztonsági jellemzőkkel és a különböző típusú lexikális elem tartalmát.

> [!NOTE]
> hello v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók. toodetermine kell használnia a hello v2.0-végpontra, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
>
>

## <a name="types-of-tokens"></a>A jogkivonatok típusok
hello v2.0-végponttól támogatja hello [OAuth 2.0 protokoll](active-directory-v2-protocols.md), használó tokenek hozzáférési és frissítési jogkivonatok. hello v2.0-végponttól is támogatja a hitelesítést és -bejelentkezés keresztül [OpenID Connect](active-directory-v2-protocols.md). Egy harmadik típusú lexikális elem, hello azonosító token OpenID Connect vezet be. Ezeket a jogkivonatokat mindegyikének egy jelenik meg egy *tulajdonosi* token.

Egy tulajdonosi jogkivonatot egy egyszerűsített biztonsági jogkivonatot, hogy biztosít hello tulajdonosi hozzáférés tooa erőforrás védelemmel ellátva. hello tulajdonosi, akik hello token félre. Egy entitás az Azure AD tooreceive hello tulajdonosi jogkivonatot kell hitelesítenie, ha nem intézkedéseket toosecure hello token átvitel és a tárolás során, habár hozzá, és egy nem kívánt félnek használják. Néhány biztonsági egy beépített mechanizmus tooprevent nem jogosult felek, de tulajdonosi jogkivonatok tegye használatával nem lehet. Tulajdonosi jogkivonatok kell szállítani, például a transport layer security (HTTPS) biztonságos csatornát. Egy tulajdonosi jogkivonatot továbbított az ilyen típusú biztonsági nélkül, ha egy rosszindulatú entitás sikerült használja a "-átjárójának támadás" tooacquire hello token és a jogosulatlan hozzáférés tooa védett erőforrás használni. hello ugyanazt biztonsági elveket alkalmazza, ha a tárolást, vagy a gyorsítótárazás tulajdonosi jogkivonatok későbbi használatra. Mindig győződjön meg arról, hogy az alkalmazás biztonságos továbbítja a tulajdonosi jogkivonatok tárolja. Tulajdonosi jogkivonatok további biztonsági szempontjait, lásd: [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750).

Számos hello v2.0-végponttól által kiállított hello jogkivonatokat használják, mint a JSON Web Tokens (JWTs). Jwt-t egy kompakt, URL-cím szálbiztos módon tootransfer adatokat a két fél között. a jwt-t hello információt nevezzük egy *jogcím*. Ez nem egy helyességi feltétel hello tulajdonosi kapcsolatos információk és a tulajdonos hello jogkivonat. a rendszer kódolása, és a továbbításra szerializált JavaScript Object Notation (JSON) objektumok hello jogcímeket a jwt-t. Mivel hello JWTs által kibocsátott hello v2.0-végponttól írták alá, de nincs titkosítva, könnyen vizsgálhatja hello tartalmát jwt-t a hibakereséshez. JWTs kapcsolatos további információkért lásd: hello [JWT specification](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Azonosító-jogkivonatokat
Egy azonosító tokent a bejelentkezési biztonsági jogkivonatot, amely az alkalmazás kapja meg, amikor használatával végez hitelesítést egy formája, amely [OpenID Connect](active-directory-v2-protocols.md). Azonosító-jogkivonatokat helyettesítik [JWTs](#types-of-tokens), és használható toosign hello felhasználói tooyour alkalmazásban jogcímeket tartalmaznak. Hello jogcímet egy Azonosítót jogkivonatban különböző módokon használhatja. Általában rendszergazdák használják azonosító jogkivonatok toodisplay fiók adatait vagy a hozzáférés-vezérlési döntéseket toomake alkalmazásban. hello v2.0-végponttól Azonosítójú elem, amely rendelkezik a jogcímeket, függetlenül attól, hogy a bejelentkezett felhasználó hello típusú konzisztens csak egyféle típusú problémákkal. hello formátum és azonosító-jogkivonatokat tartalmát vannak hello azonos személyes Microsoft-fiókot használó felhasználók és a munkahelyi vagy iskolai fiókját.

Jelenleg azonosító-jogkivonatokat írták alá, de nincs titkosítva. Ha az alkalmazás egy azonosító jogkivonatot kap, kell [hello aláírás érvényesítése](#validating-tokens) tooprove jogkivonat eredetiséget hello és néhány jogcímek a lexikális elem tooprove hello érvényességének ellenőrzése. hello jogcímek, az alkalmazás által érvényesített változó attól függően, hogy a forgatókönyv-követelményeinek, de az alkalmazás kell végeznie néhány [közös jogcím érvényesítést](#validating-tokens) minden forgatókönyvben.

Ad a hello részletes információ a azonosító-jogkivonatokat a jogcím hello következő szakaszokat, továbbá tooa minta azonosító tokent. Vegye figyelembe, hogy a jogcímek az azonosító-jogkivonatokat nem vissza meghatározott sorrendben. Emellett új jogcímeket is lehet bevezetni azonosító-jogkivonatokat bármikor. Az alkalmazás nem kell töréspont, ha új, új jogcímeket. a következő lista hello az alkalmazás jelenleg is megbízhatóan értelmezhetők hello jogcímeket tartalmaz. További részletek találhatók hello [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>Minta azonosító tokent
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Az eljárás az, tooinspect hello minta azonosító jogkivonatot hello jogcímek hello minta Azonosítót jogkivonatban, Beillesztés [calebb.net](http://calebb.net/).
>
>

#### <a name="claims-in-id-tokens"></a>Azonosító-jogkivonatokat a jogcím
| Név | Jogcím | Példaérték | Leírás |
| --- | --- | --- | --- |
| Célközönség |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Hello jogkivonat szánt hello címzett azonosítja. Az azonosító-jogkivonatokat a hello célközönségét az alkalmazás tooyour alkalmazást az alkalmazásregisztrációs portálra a Microsoft hello hozzárendelt Alkalmazásazonosító. Az alkalmazás kell ellenőrizni az értékét, és utasítsa el hello tokent, ha hello értéke nem egyezik. |
| Kiállító |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Hello biztonságijogkivonat-szolgáltatás (STS) hoz létre és hello jogkivonatot, és melyik hello a felhasználó hitelesítési hello Azure AD-bérlő visszaadó azonosítja. Az alkalmazás érdemes ellenőrizni a hello kibocsátó jogcímet, amely a hello token tooensure hello v2.0-végponttól érkezett. Azt is érdemes hello GUID része használható hello toorestrict hello jogcímkészlethez bérlők számától toohello app tud bejelentkezni. hello GUID, amely azt jelzi, hogy a felhasználó hello egy Microsoft-fiók fogyasztói felhasználó `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Ki: |`iat` |`1452285331` |hello idő, mely hello token adta ki, epoch idő. |
| Lejárati idő |`exp` |`1452289231` |mely hello token érvénytelen, válik hello idő epoch időt jelöli. Az alkalmazás használja a jogkivonatok élettartama hello jogcím tooverify hello érvényességét. |
| Hatálybalépési idő |`nbf` |`1452285331` |mely hello token hatályba lép, hello idő epoch időt jelöli. Általában hello azonos hello kiállítási alkalommal. Az alkalmazás használja a jogkivonatok élettartama hello jogcím tooverify hello érvényességét. |
| Verzió |`ver` |`2.0` |hello azonosító tokent, az Azure AD által definiált hello verzióját. Hello v2.0-végponttól hello értéke a `2.0`. |
| Bérlő azonosítója |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Hello Azure AD bérlői, felhasználói hello képviselő GUID származik. A munkahelyi és iskolai fiókok hello GUID hello nem módosítható bérlőhöz tartozik, amely a felhasználó hello hello szervezet azonosítója. A személyes fiókok hello értéke `9188040d-6c67-4c5b-b112-36a304b66dad`. Hello `profile` hatókörben szükség a rendezés tooreceive ezt a kérelmet. |
| Kód kivonata |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |csak amikor hello azonosító token OAuth 2.0 hitelesítési kóddal hello kód kivonatoló azonosító-jogkivonatokat tartalmazza. Az engedélyezési kód használt toovalidate hello hitelességének lehet. Ezen ellenőrzés végrehajtásával kapcsolatos részletekért lásd: hello [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html). |
| Hozzáférési jogkivonat kivonata |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |csak akkor, ha hello azonosító token kiadott OAuth 2.0 hozzáférési tokent hello access token kivonatoló azonosító-jogkivonatokat tartalmazza. Lehet, hogy a használt toovalidate hello hitelességének olyan hozzáférési jogkivonatot. Ezen ellenőrzés végrehajtásával kapcsolatos részletekért lásd: hello [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html). |
| Nonce |`nonce` |`12345` |hello nonce hitelesítési karakterláncok ismétlésének támadások kiküszöböléséhez stratégiát is. Az alkalmazás megadhat egy nonce engedélyezési kérelmet hello segítségével `nonce` lekérdezési paraméter. hello kérelemben megadja hello érték kibocsátott hello Azonosítót jogkivonatban `nonce` jogcím változtatás nélkül. Az alkalmazás és az azt megadott hello kérésére, amely összerendeli az hello app munkamenet egyedi azonosítója jogkivonatok hello érték hello érték ellenőrizheti. Az alkalmazás végre kell hajtania az ellenőrzés hello azonosító token érvényesítési folyamat során. |
| név |`name` |`Babe Ruth` |hello név jogcímet emberek számára olvasható érték, amely azonosítja a hello tulajdonos hello jogkivonat biztosít. hello érték nem garantált toobe egyedi, változtatható, és csak a megjelenítésre használt toobe tervezték. Hello `profile` hatókörben szükség a rendezés tooreceive ezt a kérelmet. |
| E-mailek |`email` |`thegreatbambino@nyy.onmicrosoft.com` |hello elsődleges e-mail címéhez tartozó hello felhasználói fiókot, ha van ilyen. Az érték változtatható és idővel változhatnak. Hello `email` hatókörben szükség a rendezés tooreceive ezt a kérelmet. |
| előnyben részesített felhasználónév |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |hello elsődleges felhasználónév, amely képviseli hello felhasználót a v2.0-végponttól hello. Ez lehet egy e-mail címet, telefonszámot vagy egy általános felhasználónév nélkül a megadott formátumban. Az érték változtatható és idővel változhatnak. Hello `profile` hatókörben szükség a rendezés tooreceive ezt a kérelmet. |
| Tulajdonos |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | hello egyszerű, amely hello token kapcsolatos információkat, például egy alkalmazás hello felhasználói állításokat. Ez az érték nem módosítható és nem lehet újbóli hozzárendelése és nem használja fel újra. Azok használt tooperform hitelesítési ellenőrzések biztonságosan, például ha hello token használt tooaccess erőforrás, és egy adatbázistáblákban kulcs használható. Mivel hello tulajdonos mindig hello jogkivonatok szerepel, hogy az Azure AD kibocsát, azt javasoljuk, ez az érték egy általános célú engedélyezési rendszerben. az hello tulajdonos, azonban a páros azonosítója – egyedi tooa adott alkalmazás azonosítója.  Ezért ha egy felhasználó bejelentkezik a két különböző alkalmazások két különböző ügyfél-azonosító, az alkalmazások két különböző érték is hello tulajdonos jogcím fog kapni.  Ez lehet, hogy vagy a architektúra és adatvédelmi követelményeitől függően nem is kívánatos. |
| objektum azonosítója |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` | az objektum nem módosítható azonosítót hello hello Microsoft identity rendszer ebben az esetben egy felhasználói fiókot.  Biztonságos, és egy adatbázistáblákban kulcsként használt tooperform hitelesítési ellenőrzések is lehet. Ez az azonosító egyedileg azonosítja a hello felhasználó alkalmazásra – hello ugyanazon felhasználó fog kapni bejelentkezéskor két különböző alkalmazás hello azonos értéket hello `oid` jogcím.  Ez azt jelenti, hogy használható tooMicrosoft online szolgáltatások, például a Microsoft Graph hello lekérdezések létrehozásakor.  Microsoft Graph hello visszaállítja ezt az Azonosítót, hello `id` tulajdonság egy adott felhasználói fiók.  Mivel hello `oid` lehetővé teszi, hogy több alkalmazás toocorrelate felhasználók hello `profile` hatókörben szükség a rendezés tooreceive ezt a kérelmet. Vegye figyelembe, hogy ha egy felhasználó több bérlő, hello felhasználói fogja tartalmazni az egyes bérlők különböző Objektumazonosító - nek minősíti azokat külön fiókot annak ellenére, hogy minden egyes fiókkal rendelkező felhasználó bejelentkezik hello hello ugyanazokat a hitelesítő adatokat. |

### <a name="access-tokens"></a>Hozzáférési jogkivonatok
Jelenleg csak a Microsoft Services által igénybe vehető hello v2.0-végponttól által kibocsátott jogkivonatot. Az alkalmazások nem szükséges tooperform bármely érvényesítés vagy a hozzáférési jogkivonatok vizsgálata a jelenleg támogatott hello esetekben. Hozzáférési jogkivonatok nem teljesen átlátszó, is kezelheti. Az alkalmazás a HTTP-kérelmek teljen tooMicrosoft csak karakterláncok.

A jövőbeli, hello v2.0 közelében hello végpont hello mostantól az alkalmazási tooreceive hozzáférési jogkivonatok bevezet azon egyéb ügyfelekről. Idő lejárta után a referencia-témakör hello információkat, amelyekre szüksége van az alkalmazás tooperform hozzáférési jogkivonatok érvényesség-ellenőrzése és más hasonló feladatok hello adatokkal fog frissülni.

Amikor hello v2.0-végpontra a kér le a hozzáférési tokent, hello v2.0-végpontra, az is hello hozzáférési jogkivonatát az alkalmazás toouse metaadatainak adja vissza. Ezen információk közé tartozik a hello lejárati idejét hello hozzáférési jogkivonat és hello hatókörök, amelyek esetében érvényes. Az alkalmazás használja, a metaadatok tooperform intelligens gyorsítótárazása hozzáférési jogkivonatok anélkül, hogy tooparse nyitott hello hozzáférési jogkivonat magát.

### <a name="refresh-tokens"></a>Frissítési jogkivonatok
Frissítési jogkivonatok a biztonsági jogkivonatokat, hogy az alkalmazás használhatja tooget új hozzáférési jogkivonatok az OAuth 2.0 folyamatból. Az alkalmazás használhat frissítési jogkivonatok tooachieve hosszú távú hozzáférés tooresources egy felhasználó nevében hello felhasználói beavatkozás nélkül.

Frissítési jogkivonatok több erőforrás. A frissítési token során egy erőforrást a kérés érkezett a is hozzáférési jogkivonatok tooa teljesen különböző erőforrás sikerült beváltani.

tooreceive token választ frissítéssel, az alkalmazás kell igényelnie, és biztosítani hello `offline_acesss` hatókör. toolearn hello kapcsolatos további `offline_access` hatókör, lásd: hello [hozzájárulási és hatókörök](active-directory-v2-scopes.md) cikk.

Frissítési jogkivonatok, és mindig lesz, nem teljesen átlátszó tooyour alkalmazást. Azok az Azure AD v2.0-végponttól hello által kiállított és csak felügyelete és hello v2.0-végponttól értelmezi. Hosszú élettartamú, de az alkalmazást nem kell írni, amely egy frissítési jogkivonat minden idõszak, amíg a rendszer utolsó tooexpect. Frissítési jogkivonatokat különböző okokból bármikor érvénytelenített lehet. csak az alkalmazás tooknow, ha a frissítési jogkivonat érvényes megoldást tooattempt tooredeem hello azt azáltal, hogy egy kérés toohello v2.0-végponttól.

Amikor új tokenre egy frissítési token beváltja (, és ha az alkalmazás megadták hello `offline_access` hatókör), kap egy új frissítési jogkivonat hello token válaszként. Mentse a hello újonnan kiadott frissítési jogkivonat, tooreplace hello egy használt hello kérelemben. Ez biztosítja, hogy, hogy a frissítési jogkivonatokat maradnak, amíg érvényes.

## <a name="validating-tokens"></a>Jogkivonatok ellenőrzése
Az alkalmazások kell csak jogkivonat érvényesítésére jelenleg hello tooperform érvényesítése azonosító-jogkivonatokat. toovalidate egy azonosító tokent, az alkalmazás kell ellenőrizhesse mindkét hello azonosító jogkivonat aláírása és hello hello Azonosítót jogkivonatban.

<!-- TODO: Link -->
A Microsoft biztosít a könyvtárak és kódpéldák, amelyek bemutatják, hogyan tooeasily kezelni a jogkivonatok érvényesség-ellenőrzése. A következő szakaszok hello azt az alapul szolgáló folyamat hello írja le. Több külső nyílt forráskódú kódtárai is elérhetők a JWT-ellenőrzéshez. Legalább egy tárat szinte minden platform és nyelvi beállítás van.

### <a name="validate-hello-signature"></a>Hello aláírás érvényesítése
A jwt-t tartalmazza az három, az elemek elválasztására pedig hello `.` karakter. hello első szegmens nevezik hello *fejléc*, hello második szegmens pedig az hello *törzs*, és hello harmadik szegmens pedig az hello *aláírás*. hello aláírás szegmens használt toovalidate hello hitelességének hello azonosító token lehet, hogy az alkalmazás által megbízhatóak.

Azonosító-jogkivonatokat aláírt szabványos aszimmetrikus titkosítási algoritmusok, például RSA 256. hello hello azonosító jogkivonat fejléc rendelkezik hello kulccsal kapcsolatos információkat, és titkosítási módszer toosign hello jogkivonat. Példa:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Hello `alg` jogcím azt jelzi, de a használt toosign hello token hello algoritmus. Hello `kid` jogcím hello nyilvános kulcsot, de a használt toosign hello token jelzi.

Bármikor hello v2.0-végponttól előfordulhat, hogy használatával írja alá az azonosító tokent egy meghatározott nyilvános-titkos kulcspárok valamelyikét. hello v2.0-végponttal rendszeres időközönként forog hello lehetséges kulcsok készlete, így az alkalmazás automatikusan megváltoztatja e kulcs toohandle kell írni. A frissítések toohello nyilvános kulcsok hello v2.0 végpont által használt egy ésszerű gyakoriság toocheck 24 óránként van.

Aláírási kulcs toovalidate hello aláírás hello OpenID Connect metaadat-dokumentum található a szükséges adatokat hello szerezheti be:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Próbálja meg egy böngészőben hello URL-címet!
>
>

A metaadat-dokumentum egy JSON-objektum, amely rendelkezik a számos hasznos információt tartalmaz, például a különböző végpontok az OpenID Connect hitelesítéshez szükséges hello hello helyét.  hello dokumentum is magában foglalja a *jwks_uri*, ennek révén a nyilvános kulcsok készlete hello hello helyét használt toosign jogkivonatokat. hello JSON-dokumentum hello jwks_uri helyen található összes hello nyilvánoskulcs-adatokat, amelyek jelenleg használatban van. Az alkalmazás használhat hello `kid` hello JWT fejléc tooselect mely nyilvános kulcs ebben a dokumentumban használt toosign elkészült a jogcím-jogkivonatot. Majd az aláírás érvényesítése és segítségével hajtja végre hello megfelelő nyilvános kulccsal hello jelzett algoritmus.

Ez a dokumentum hatókörén kívül hello aláírás-ellenőrzés végrehajtása. Számos nyílt forráskódú kódtárai elérhető toohelp ezt meg.

### <a name="validate-hello-claims"></a>Hello ellenőrizhesse
Ha az alkalmazás felhasználói bejelentkezés után egy azonosító jogkivonatot kap, azt kell is ellenőrzi a néhány elleni hello jogcímek hello Azonosítót jogkivonatban. Ezek közé tartozik, de nincsenek korlátozva:

* **a célközönség** jogcím, amely azonosító token hello tooverify lett tooyour app megadott tervezett toobe
* **hatálybalépési idő** és **lejárati ideje** jogcímek tooverify, amely hello azonosító jogkivonat nem járt.
* **kibocsátó** jogcím, amely a hello token tooverify hello v2.0-végponttól által kiállított tooyour alkalmazás
* **NONCE**, jogkivonatként visszajátszásos támadások megoldás

Jogcím-érvényesítést, végre kell hajtania, az alkalmazás teljes listáját lásd: hello [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

A jogcímek hello várt értékek részleteit szerepelnek hello [azonosító-jogkivonatokat](# ID tokens) szakasz.

## <a name="token-lifetimes"></a>Token élettartama
A következő token élettartama csak tájékoztatási hello nyújtunk. hello információ segíthet fejlesztése és alkalmazások hibakeresését. Az alkalmazások nem lehet írt tooexpect bármely ezek élettartama tooremain konstans. A token élettartama is, és bármikor változik.

| Token | Élettartam | Leírás |
| --- | --- | --- |
| Azonosító-jogkivonatokat (munkahelyi vagy iskolai fiókok) |1 óra |Azonosító-jogkivonatokat általában érvényesek 1 óra. A webes alkalmazás a saját munkamenetben hello felhasználóval (ajánlott), vagy választhatja azt egy teljesen más munkamenetek élettartamát azonos élettartama toomaintain használhatja. A tooget azonosító új tokent kell, hogy szükséges-e egy új bejelentkezés toomake kérelem toohello v2.0 végpont hitelesítéséhez. Ha hello felhasználó egy érvényes böngésző-munkamenet hello v2.0-végponttal rendelkezik, hello felhasználói előfordulhat, hogy nem kell kötelező tooenter hitelesítő adataikkal újra. |
| Azonosító-jogkivonatokat (személyes fiókok) |24 óra |A személyes fiókok azonosító-jogkivonatokat általában érvényes 24 órán át. A webes alkalmazás a saját munkamenetben hello felhasználóval (ajánlott), vagy választhatja azt egy teljesen más munkamenetek élettartamát azonos élettartama toomaintain használhatja. A tooget azonosító új tokent kell, hogy szükséges-e egy új bejelentkezés toomake kérelem toohello v2.0 végpont hitelesítéséhez. Ha hello felhasználó egy érvényes böngésző-munkamenet hello v2.0-végponttal rendelkezik, hello felhasználói előfordulhat, hogy nem kell kötelező tooenter hitelesítő adataikkal újra. |
| Hozzáférési jogkivonatok (munkahelyi vagy iskolai fiókok) |1 óra |Token válaszok jelzett hello token metaadatok részeként. |
| Access tokens (személyes fiókok) |1 óra |Token válaszok jelzett hello token metaadatok részeként. Hozzáférés kiadott jogkivonatokat, személyes fiókok nevében konfigurálhatja a különböző élettartamot, de tipikus 1 óra. |
| Frissítési jogkivonatok (munkahelyi vagy iskolai fiókkal) |Too14 nap mentése |Egyetlen frissítési jogkivonat esetén legfeljebb 14 napig érvényes. Azonban hello frissítési jogkivonat érvénytelenné válhatnak különböző okokból bármikor, az alkalmazás kell tootry toouse egy frissítési jogkivonat folytatni, amíg nem sikerül, vagy amíg az alkalmazás egy új frissítési jogkivonat lecseréli azt. A frissítési jogkivonat is lesz érvénytelen, ha azt óta 90 nap hello felhasználó megadott hitelesítő adataikat. |
| Frissítési jogkivonatok (személyes fiókok) |Másolatot too1 év |Egyetlen frissítési jogkivonat esetén legfeljebb 1 évig érvényes. Azonban számos okból bármikor hello frissítési jogkivonat érvénytelenné válhatnak, így az alkalmazás továbbra is tootry toouse sikertelen lesz, amíg a frissítési jogkivonat. |
| Hitelesítési kódok (munkahelyi vagy iskolai fiókok) |10 perc |Hitelesítési kódok szándékosan rövid életű, és kell azonnal váltható a hozzáférési jogkivonatok és a frissítési jogkivonatokat hello jogkivonatok fogadásakor. |
| Hitelesítési kódok (személyes fiókok) |5 perc |Hitelesítési kódok szándékosan rövid életű, és kell azonnal váltható a hozzáférési jogkivonatok és a frissítési jogkivonatokat hello jogkivonatok fogadásakor. Hitelesítési kódok kiállított személyes fiókok nevében egyszeri használatra szolgálnak. |

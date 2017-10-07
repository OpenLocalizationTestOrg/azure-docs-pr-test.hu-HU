---
title: "aaaLearn készül hello különböző jogkivonatot, és az Azure AD által támogatott jogcímtípusok |} Microsoft Docs"
description: "Ismertetése és értékelése hello jogcímek hello SAML 2.0 és a JSON Web Tokens (JWT) tokenek által Azure Active Directory (AAD) kiadott egy útmutató"
documentationcenter: na
author: dstrockis
services: active-directory
manager: mbaldwin
editor: 
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: c512894476613e94d86a994c32a8459d6cf00fbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-token-reference"></a>Az Azure AD-jogkivonatok referenciájából
Azure Active Directory (Azure AD) számos különböző típusú hello feldolgozása minden hitelesítési folyamat során a biztonsági jogkivonatokat bocsát ki. Ez a dokumentum ismerteti a hello formátumát, a biztonsági jellemzőkkel és a különböző típusú lexikális elem tartalmát.

## <a name="types-of-tokens"></a>A jogkivonatok típusok
Az Azure AD támogatja hello [OAuth 2.0 protokoll](active-directory-protocols-oauth-code.md), amely él access_tokens és refresh_tokens is.  Azt is támogatja a hitelesítést és -bejelentkezés keresztül [OpenID Connect](active-directory-protocols-openid-connect-code.md), amely bevezet egy harmadik típusú lexikális elem, hello id_token.  Ezeket a jogkivonatokat mindegyikének "tulajdonosi jogkivonattal" értéke jelöli.

Egy tulajdonosi jogkivonatot egy egyszerűsített biztonsági jogkivonatot, hogy biztosít hello "tulajdonos" hozzáférési tooa erőforrás védelemmel ellátva. Abban az értelemben a "tulajdonos" hello, akik hello token félre. Bár az Azure ad-val hitelesítést igényel sorrendben tooreceive egy tulajdonosi jogkivonatot, gondoskodni kell toosecure hello token, tooprevent hozzáférés egy nem kívánt félnek. Tulajdonosi jogkivonatok nem rendelkezik olyan beépített mechanizmus tooprevent nem jogosult felek tudják, mert azok kell szállítani, például a transport layer security (HTTPS) biztonságos csatornát. Egy tulajdonosi jogkivonatot egyértelmű hello küldött a rendszer, a man a hello középső támadás használt tooacquire hello jogkivonatot kell, és szerezhet a jogosulatlan hozzáférés tooa védett erőforrás. hello ugyanazt biztonsági elveket alkalmazza, ha a tárolást, vagy a gyorsítótárazás tulajdonosi jogkivonatok későbbi használatra. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon tárolja a tulajdonosi jogkivonatokhoz. További biztonsági szempontok a tulajdonosi jogkivonatok, lásd: [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750).

Számos olyan Azure AD által kiállított hello jogkivonatokat használják, mint a JSON webes jogkivonatainak vagy JWTs.  Jwt-t egy kompakt biztonságos URL-címet a eszközöket információk átvitele a két fél között.  JWTs lévő hello információt hello tulajdonosi és a tulajdonos hello token "jogcímek" vagy a helyességi feltételek adatok néven ismert.  hello jogcím a JWTs kódolva, és a továbbításra szerializált JSON-objektumok.  Mivel az Azure AD által kibocsátott hello JWTs aláírt, de nincs titkosítva, könnyen vizsgálhatja jwt-t hello tartalmát hibakeresési célra.  Nincsenek elérhető például a módon valósíthatja, több eszközt [jwt.calebb.net](http://jwt.calebb.net). További információ a JWTs, olvassa el a toohello [JWT specification](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens
Id_tokens a bejelentkezési biztonsági jogkivonatot, amely az alkalmazás kapja meg, amikor a hitelesítés végrehajtásához egy formája [OpenID Connect](active-directory-protocols-openid-connect-code.md).  Jelentésekként jelennek meg [JWTs](#types-of-tokens), és hello felhasználó jelentkezik be az alkalmazás használhat jogcímeket tartalmaznak.  Egy id_token hello jogcímeket, tetszés szerint – általában arra szolgálnak, fiók adatainak megjelenítéséhez, vagy a hozzáférés-vezérlési döntéseket és alkalmazáson belüli használhatja.

Id_tokens aláírt, de jelenleg nincs titkosítva.  Ha az alkalmazás egy id_token kap, kell [hello aláírás érvényesítése](#validating-tokens) tooprove jogkivonat eredetiséget hello és néhány jogcímek a lexikális elem tooprove hello érvényességének ellenőrzése.  hello jogcímek, az alkalmazás által érvényesített változó attól függően, hogy a forgatókönyv-követelményeinek, de van azonban néhány [közös jogcím érvényesítést](#validating-tokens) , amely az alkalmazás minden esetben kell elvégezni.

Tekintse meg a következő szakasz információkat id_tokens jogcímeket, valamint egy minta id_token hello.  Figyelje meg, hogy bármely adott sorrendben nem lehet megjeleníteni a id_tokens hello jogcímeket.  Emellett új jogcímeket is lehet bevezetni bármikor id_tokens időben, mert az alkalmazás nem törhetik belépéskor új jogcímeket.  hello alábbi lista tartalmazza az alkalmazás megbízhatóan tudja értelmezni a írásának hello időpontjában hello jogcímeket.  Ha szükséges, még akkor is további információkhoz juthat található hello [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>A minta id_token
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [!TIP]
> Eljárás, próbálja ki a hello minta id_token hello jogcímek ellenőrzése illeszti be [calebb.net](http://jwt.calebb.net).
> 
> 

#### <a name="claims-in-idtokens"></a>A id_tokens jogcímek
> [!div class="mx-codeBreakAll"]
| JWT jogcím | Név | Leírás |
| --- | --- | --- |
| `appid` |Alkalmazásazonosító |Határozza meg, amely hello token tooaccess erőforrás használja hello alkalmazást. hello alkalmazás működhet-e saját magát vagy egy felhasználó nevében. hello Alkalmazásazonosító általában egy alkalmazás az objektumot határozza meg, de a szolgáltatás egyszerű objektum az az Azure ad-ben is jelenthet. <br><br> **Példaérték JWT**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud` |Célközönség |hello szánt címzett hello jogkivonat. hello alkalmazás, amely megkapja a hello token ellenőriznie kell, hogy hello célközönség értéke megfelelő-e, és utasítsa el a szükséges jogkivonatokhoz, egy másik célközönség számára készült. <br><br> **Példa SAML érték**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Példaérték JWT**: <br> `"aud":"https://contoso.com"` |
| `appidacr` |Alkalmazás hitelesítési környezeti osztályait ismertető dokumentációban |Azt jelzi, hogy hello ügyfelek hitelesítésének módját. Nyilvános-ügyfél a hello értéke 0. Ügyfél-azonosító és a titkos ügyfélkódot használata esetén hello értéke 1. <br><br> **Példaérték JWT**: <br> `"appidacr": "0"` |
| `acr` |Hitelesítési környezeti osztályait ismertető dokumentációban |Azt jelzi, hogyan hello tulajdonos hitelesített, hello alkalmazás hitelesítési környezeti osztályait ismertető dokumentációban jogcímek megakadályozását toohello ügyfélként. "0" érték hello végfelhasználói hitelesítési nem felelt meg az ISO/IEC 29115 hello követelményeinek. <br><br> **Példaérték JWT**: <br> `"acr": "0"` |
| Azonnali hitelesítés |Rekordok hello dátum és idő, amikor hitelesítést történt. <br><br> **Példa SAML érték**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | |
| `amr` |Hitelesítési módszer |Hello tulajdonos hello jogkivonat hitelesítésének módját azonosítása <br><br> **Példa SAML érték**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Példaérték JWT**:`“amr”: ["pwd"]` |
| `given_name` |Utónév |Itt hello először, vagy "a megadott" hello felhasználó nevét, hello Azure AD felhasználói objektum vannak megadva. <br><br> **Példa SAML érték**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Példaérték JWT**: <br> `"given_name": "Frank"` |
| `groups` |Csoportok |Az objektumazonosító, amelyek megfelelnek a hello tulajdonos csoporttagságok biztosít. Ezek az értékek egyedi (lásd: objektumazonosító:) és a hozzáférés, például engedélyezési tooaccess erőforrás kényszerítése biztonságosan használható. hello csoportok jogcím szerepel hello csoport konfigurálva alkalmazásonkénti bontásban hello az alkalmazás jegyzékének hello "groupMembershipClaims" tulajdonságon keresztül. Null értékű kizárja az összes csoport, csak az Active Directory biztonsági csoport tagságát, és a "All" lesz tartalmazzák a biztonsági csoportok és Office 365 terjesztési listák "a(z)"biztonsági csoporthoz értéket tartalmazza. <br><br> **Példa SAML érték**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Példaérték JWT**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` |Identitásszolgáltató |Rekordok hello identitásszolgáltató hitelesített hello tulajdonos hello jogkivonat. Ez az érték azonos toohello értékének kibocsátó jogcím, kivéve, ha hello felhasználói fiók pedig egy másik bérlő mint hello kibocsátó hello. <br><br> **Példa SAML érték**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Példaérték JWT**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` |IssuedAt |Token ki, melyik hello hello idő tárolja. Is gyakran használt toomeasure token frissesség. <br><br> **Példa SAML érték**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Példaérték JWT**: <br> `"iat": 1390234181` |
| `iss` |Kiállító |Azonosítja a hello biztonságijogkivonat-szolgáltatás (STS) hoz létre, és hello jogkivonatot ad vissza. Hello jogkivonatokba az Azure AD eredményül hello nem sts.windows.net. hello GUID hello kibocsátó jogcím értékét a rendszer hello Bérlőazonosító hello Azure AD-címtár. hello Bérlőazonosító hello könyvtár nem módosítható és megbízható azonosító, amely. <br><br> **Példa SAML érték**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Példaérték JWT**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` |Vezetéknév |Az Azure AD-felhasználói objektum hello biztosít a hello Vezetéknév, Vezetéknév vagy családnév hello felhasználó. <br><br> **Példa SAML érték**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Példaérték JWT**: <br> `"family_name": "Miller"` |
| `unique_name` |Név |Emberi olvasható érték, amely azonosítja a hello tulajdonos hello jogkivonat biztosít. Ez az érték nem garantált toobe egyedi belül a bérlők és tervezett toobe csak megjelenítési célra szolgál. <br><br> **Példa SAML érték**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Példaérték JWT**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` |objektum azonosítója |Egy objektum egyedi azonosítóját tartalmazza az Azure ad-ben. Ez az érték nem módosítható és nem lehet újbóli hozzárendelése és nem használja fel újra. Hello objektum azonosítója tooidentify objektum használja a lekérdezések tooAzure AD. <br><br> **Példa SAML érték**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Példaérték JWT**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` |Szerepkörök |Jelöli hello tárgy összes alkalmazás-szerepkörök rendelkezik közvetlen és közvetett csoporttagság és használt tooenforce szerepköralapú hozzáférés-vezérlést. Alkalmazási szerepköröknek meghatározott alkalmazásonkénti bontásban keresztül hello `appRoles` hello az alkalmazás jegyzékének tulajdonság. Hello `value` minden egyes alkalmazás-szerepkör tulajdonsága hello szerepkörök jogcím megjelenő hello érték. <br><br> **Példa SAML érték**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Példaérték JWT**: <br> `“roles”: ["Admin", … ]` |
| `scp` |Hatókör |Azt jelzi, hogy hello megszemélyesítési engedélyek megadott toohello ügyfélalkalmazást. hello alapértelmezett engedély `user_impersonation`. védett erőforrás hello hello tulajdonosa további értékeket regisztrálhatja az Azure ad-ben. <br><br> **Példaérték JWT**: <br> `"scp": "user_impersonation"` |
| `sub` |Tárgy |Mely hello kapcsolatos token állításokat információkat, például egy alkalmazás felhasználója hello hello egyszerű azonosítja. Ez az érték nem módosítható és nem társítható újra, vagy használja fel újra, így azok használt tooperform engedélyezési ellenőrzések biztonságosan. Mivel hello tulajdonos mindig szerepel hello jogkivonatok hello Azure AD-problémák, javasoljuk, hogy ezt az értéket használja egy általános célú engedélyezési rendszerben. <br> `SubjectConfirmation`Nincs olyan jogcímet. Leírja, hogyan hello jogkivonat hello tulajdonos ellenőrzése. `Bearer`azt jelzi, hogy hello token birtokában megerősíti, hogy hello tulajdonos. <br><br> **Példa SAML érték**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Példaérték JWT**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"` |
| `tid` |Bérlőazonosító |Nem módosítható, egyszer használatos azonosító hello jogkivonatot kibocsátó hello directory bérlői azonosító. Egy több-bérlős alkalmazást is használhatja e érték tooaccess bérlői-specifikus címtárerőforrásokkal. Például használhatja adott érték tooidentify hello bérlőt egy hívás toohello Graph API-val. <br><br> **Példa SAML érték**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Példaérték JWT**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"` |
| `nbf`, `exp` |A jogkivonatok élettartama |Meghatározza a hello időtartam, amelyen belül a lexikális elem érvénytelen. hello szolgáltatás, amely érvényesíti hello jogkivonatot kell győződjön meg arról, hogy hello aktuális dátum hello a jogkivonatok élettartama, más azt kell utasítania hello token belül. hello szolgáltatás előfordulhat, hogy engedélyezi a hozzáférést a másolatot toofive perc hello a jogkivonatok élettartama tartomány tooaccount túl az eltérések idő ("idő döntés") az Azure AD között és hello szolgáltatást. <br><br> **Példa SAML érték**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Példaérték JWT**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn` |Egyszerű felhasználónév |Tárolók hello hello felhasználó egyszerű felhasználóneve.<br><br> **Példaérték JWT**: <br> `"upn": frankm@contoso.com` |
| `ver` |Verzió |Hello token hello verziószámát tárolja. <br><br> **Példaérték JWT**: <br> `"ver": "1.0"` |

## <a name="access-tokens"></a>Hozzáférési jogkivonatok
Sikeres hitelesítés után az Azure AD hozzáférési tokent, amely lehet használt tooaccess védett erőforrások adja vissza. hello access token base 64 kódolású JSON webes jogkivonat (JWT), és annak tartalmát a dekóder keresztül futtatásával ellenőrizni kell.

Ha az alkalmazás csak *használ* hozzáférési jogkivonatok hozzáférés tooAPIs tooget, akkor is (és kell) tekinti hozzáférési jogkivonatok nem teljesen átlátszó – csak karakterláncok, amelyek az alkalmazás a HTTP-kérelmek teljen tooresources.

Amikor kér le a hozzáférési tokent, akkor az Azure AD is az néhány hello hozzáférés jogkivonattá, az alkalmazás metaadatainak adja vissza.  Ezen információk közé tartozik a hello lejárati idejét hello hozzáférési jogkivonat és hello hatókörök, amelyek esetében érvényes.  Ez lehetővé teszi az alkalmazás tooperform intelligens anélkül, hogy nyissa meg a hello hozzáférési jogkivonat maga tooparse hozzáférési jogkivonatok gyorsítótárazását.

Ha az alkalmazás egy API-t, hogy a hozzáférési jogkivonatok vár a HTTP-kérelmek Azure AD-val védett, majd végre kell hajtania érvényesítési és hello jogkivonatokat kap vizsgálata. Az alkalmazás hello hozzáférési jogkivonat érvényesítése tooaccess erőforrások használata előtt végre kell hajtania. Az érvényesítési további információkért lásd: [ellenőrzése jogkivonatok](#validating-tokens).  
Hogyan toodo Ez a .NET, tekintse meg a részleteket [egy Azure ad-tulajdonosi jogkivonatok segítségével webes API védelme](active-directory-devquickstarts-webapi-dotnet.md).

## <a name="refresh-tokens"></a>Frissítési jogkivonatok

Frissítési jogkivonatok olyan biztonsági jogkivonatok, amely az alkalmazás használhatja tooacquire új hozzáférési jogkivonatok az OAuth 2.0 folyamatból.  Lehetővé teszi az alkalmazás tooachieve hosszú távú hozzáférés tooresources egy felhasználó nevében által hello felhasználói beavatkozás nélkül.

Frissítési jogkivonatok több erőforrás.  Ez az, hogy a frissítési jogkivonatot kapott kérelmek során egy erőforrást is sikerült beváltani, a hozzáférési jogkivonatok tooa teljesen különböző erőforrás toosay. toodo, a set hello `resource` paraméterének hello kérelem toohello céloz erőforrás.

Frissítési jogkivonatok nem teljesen átlátszó tooyour alkalmazást. Hosszú élettartamú, de az alkalmazást nem kell írni, amely egy frissítési jogkivonat minden idõszak, amíg a rendszer utolsó tooexpect.  Frissítési jogkivonatokat bármikor a számos okból lehetnek érvénytelenített.  csak az alkalmazás tooknow, ha a frissítési jogkivonat érvényes megoldást tooattempt tooredeem hello azt azáltal, hogy egy kérés tooAzure AD jogkivonat végpontjához.

Amikor új tokenre egy frissítési token beváltásához kapni fog egy új frissítési jogkivonat hello token válaszként.  Mentse az újonnan kiadott hello frissítési jogkivonat egy hello kérelemben használt hello cseréje.  Ez garantálja, hogy a frissítési jogkivonatokat maradnak, amíg érvényes.

## <a name="validating-tokens"></a>Jogkivonatok ellenőrzése

A sorrend toovalidate egy id_token vagy access_token az alkalmazás mindkét hello jogkivonat aláírása és hello jogcímek érdemes ellenőrizni. A sorrend toovalidate hozzáférési jogkivonatok az alkalmazás is ellenőrizni kell, hello kibocsátó, hello célközönség és hello jogkivonatok aláírásához. Ezeknek kell toobe ellenőrizni hello OpenID felderítési dokumentum hello értékei alapján. Például hello bérlői független hello dokumentum verziója található [https://login.microsoftonline.com/common/.well-known/openid-configuration](https://login.microsoftonline.com/common/.well-known/openid-configuration). Az Azure AD köztes ellenőrzéséhez a hozzáférési jogkivonatok beépített funkciókkal rendelkezik, és akkor böngészhet a [minták](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-code-samples) toofind az Ön által választott hello nyelven. Hogyan tooexplicitly ellenőrzése a JWT jogkivonat bővebben lásd: hello [JWT-ellenőrző manuális minta](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation).  

Szalagtárak és megjelenítése, hogyan tooeasily kezelni a jogkivonatok érvényesség-ellenőrzése - mintakódok nyújtunk hello alábbi információkat egyszerűen biztosított azok, akik az alapul szolgáló folyamat toounderstand hello kívánja.  Nincsenek is több harmadik féltől származó nyílt forráskódú szalagtárak JWT érvényesítési,-szinte minden platform és a nyelv ott legalább egy beállítást. Az Azure AD hitelesítési könyvtárak és kódpéldák kapcsolatos további információkért lásd: [az Azure AD-hitelesítési kódtárakkal](active-directory-authentication-libraries.md).

#### <a name="validating-hello-signature"></a>Hello aláírás érvényesítése

A jwt-t tartalmazza az három, az elemek elválasztására pedig hello `.` karakter.  hello első szegmens nevezik hello **fejléc**, hello második hello, **törzs**, és a harmadik, hello hello **aláírás**.  hello aláírás szegmens használt toovalidate hello hitelességének hello token lehet, hogy az alkalmazás által megbízhatóak.

Az Azure AD által kiállított jogkivonatokat aláírt iparági szabványos aszimmetrikus titkosítási algoritmusok, például RSA 256 használatával. titkosítási módszer használt toosign hello token és hello JWT hello fejlécében hello kulccsal kapcsolatos információkat tartalmazza:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Hello `alg` jogcím azt jelzi, de a használt toosign hello jogkivonatot, miközben hello hello algoritmus `x5t` jogcím hello adott nyilvános kulcsot, de a használt toosign hello token jelzi.

Az idő álljon az Azure AD aláírhatja egy id_token közül legalább egy bizonyos készlet nyilvános-titkos kulcspárok használatával. Az Azure AD forog hello lehetséges rendszeres időközönként, kulcsok készlete, így az alkalmazás automatikusan megváltoztatja e kulcs toohandle kell írni.  A frissítések toohello nyilvános kulcsok Azure AD által használt egy ésszerű gyakoriság toocheck 24 óránként van.

Aláírási kulcs adatok szükséges toovalidate hello aláírása hello OpenID Connect metaadat-dokumentum található használatával hello szerezheti be:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Próbálja meg egy böngészőben az URL-címet!
> 
> 

A metaadat-dokumentum számos hasznos információt tartalmaz, például a különböző végpontok OpenID Connect hitelesítési tevékenységek végrehajtásához szükséges hello hello helyét tartalmazó JSON-objektumból.  

Ezenkívül tartalmazza a `jwks_uri`, ennek révén a nyilvános kulcsok készlete hello hello helyét használt toosign jogkivonatokat.  hello JSON-dokumentum található hello `jwks_uri` tartalmazza az összes hello nyilvánoskulcs-adatokat használja idő az adott pillanatban.  Az alkalmazás használhat hello `kid` hello JWT fejléc tooselect mely nyilvános kulcs ebben a dokumentumban használt toosign elkészült a jogcím egy adott jogkivonatot.  Végezhet el az aláírás érvényesítése hello megfelelő nyilvános kulccsal és hello jelzett algoritmus használatával.

Aláírás-ellenőrzés végrehajtása a jelen dokumentum a külső hello hatókör - érhetők el számos nyílt forráskódú szalagtárak segít, ha szükséges.

#### <a name="validating-hello-claims"></a>Hello jogcímek ellenőrzése

Ha az alkalmazás (egy id_token felhasználói bejelentkezés során, vagy olyan hozzáférési jogkivonatot, mint egy tulajdonosi jogkivonatot hello HTTP-kérelem) jogkivonatot kap az hello token is végre kell hajtania néhány ellenőrzések hello jogcímek ellen.  Ezek közé tartozik, de nincsenek korlátozva:

* Hello **célközönség** jogcím - tooverify, amely a hello token tooyour app megadott tervezett toobe volt.
* Hello **nem előtt** és **lejárati idő** jogcím - tooverify, amely hello jogkivonat nem járt le.
* Hello **kibocsátó** jogcím - tooverify, amely a hello token valóban bocsátotta tooyour alkalmazást az Azure AD.
* Hello **Nonce** -toomitigate a hitelesítési karakterláncok ismétlésének támadás.
* és egyéb...

Az alkalmazás végre kell hajtania, azonosító-jogkivonatokat a jogcím érvényesítést teljes listájáért tekintse meg a toohello [OpenID Connect specification](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). A jogcímek hello várt értékek részleteit szerepelnek előző hello [id_token szakasz](#id-tokens) szakasz.

## <a name="sample-tokens"></a>A minta jogkivonatok

Ez a szakasz SAML és JWT-jogkivonatokat, amely visszaadja az Azure AD mintáit jeleníti meg. Ezeket a mintákat lehetővé teszik, hogy tekintse meg a környezetben hello jogcímeket.

### <a name="saml-token"></a>SAML-jogkivonat

Ez a minta egy tipikus SAML-jogkivonat.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT jogkivonat - felhasználó megszemélyesítése
Ez a jellemző JSON webes jogkivonatot (JWT) szerepel egy engedélyezési kód grant flow mintáját.
Továbbá tooclaims hello jogkivonatot tartalmaz egy verziószámot a **ver** és **appidacr**, hello hitelesítési környezeti osztályait ismertető dokumentációban, amely hello ügyfelek hitelesítésének módját jelzi. Nyilvános-ügyfél a hello értéke 0. Ha egy ügyfél-azonosító vagy a titkos ügyfélkulcs használt, hello értéke 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Kapcsolódó tartalom
* Tekintse meg az Azure AD Graph hello [házirend műveletek](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) és hello [házirend entitás](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), toolearn keresztül hello Azure AD Graph API a jogkivonatok élettartama házirend kezelésével kapcsolatos további.
* További információk és minták a PowerShell-parancsmagok, többek között a minták keresztül házirendek kezelése [konfigurálható jogkivonat élettartamát az Azure AD](../active-directory-configurable-token-lifetimes.md). 

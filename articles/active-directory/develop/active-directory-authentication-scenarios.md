---
title: "Forgatókönyvek az Azure AD aaaAuthentication |} Microsoft Docs"
description: "Hello öt leggyakoribb hitelesítési forgatókönyvek az Azure Active Directory (AAD) áttekintése"
services: active-directory
documentationcenter: dev-center-name
author: skwan
manager: mbaldwin
editor: 
ms.assetid: 0c84e7d0-16aa-4897-82f2-f53c6c990fd9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: skwan
ms.custom: aaddev
ms.openlocfilehash: 5b391e3f096fcb52934c4a8aff08688352bfd3dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-scenarios-for-azure-ad"></a>Hitelesítési forgatókönyvek az Azure AD-hez
Azure Active Directory (Azure AD) egyszerűsíti a fejlesztők hitelesítési azzal, hogy identitás szolgáltatásként,-támogatással rendelkező a szabványos protokollok, például az OAuth 2.0-s és OpenID Connect, valamint a különböző platformokon toohelp nyílt forráskódú szalagtárak akkor elkezdésére gyorsan. Ez a dokumentum segít ismeri hello különböző forgatókönyvek az Azure AD által támogatott, és bemutatja, hogyan tooget el. A következő szakaszok hello részből áll:

* [Az Azure AD hitelesítési alapjai](#basics-of-authentication-in-azure-ad)
* [Az Azure AD biztonsági jogkivonatokat a jogcím](#claims-in-azure-ad-security-tokens)
* [Egy alkalmazás regisztrálása az Azure AD alapjai](#basics-of-registering-an-application-in-azure-ad)
* [Alkalmazástípusok és forgatókönyvek](#application-types-and-scenarios)
  
  * [Webes böngésző tooWeb alkalmazás](#web-browser-to-web-application)
  * [Egylapos alkalmazások (SPA)](#single-page-application-spa)
  * [Natív alkalmazás tooWeb API](#native-application-to-web-api)
  * [Webes alkalmazás tooWeb API](#web-application-to-web-api)
  * [Démon vagy kiszolgálói alkalmazás tooWeb API](#daemon-or-server-application-to-web-api)

## <a name="basics-of-authentication-in-azure-ad"></a>Az Azure AD hitelesítési alapjai
Ha nem ismeri az Azure AD authentication alapvető fogalmait, olvassa el az ebben a szakaszban. Ellenkező esetben érdemes lehet tooskip le túl[alkalmazástípusok és forgatókönyvek](#application-types-and-scenarios).

Mérlegeljük, ahol szükség-e identitás hello legalapvetőbb forgatókönyv: egy webböngészőben a felhasználó számára szükséges tooauthenticate tooa webes alkalmazást. Ebben a forgatókönyvben a hello nagyobb részletességgel ismertetett [webböngésző tooWeb alkalmazás](#web-browser-to-web-application) szakaszában, de a egy hasznos hello képességek az Azure AD és a pont tooillustrate indítása conceptualize hello forgatókönyv működése. Vegye figyelembe a következő ábrán a forgatókönyv hello:

![Bejelentkezés tooweb alkalmazás áttekintése](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Vegye figyelembe a fenti ábrának hello, itt nem találja tooknow kapcsolatos különféle összetevői:

* Az Azure AD hello identitásszolgáltató, felhasználók és az alkalmazások, amely szerepel a munkahely címtárában hello identitásának ellenőrzésére, és végső soron a biztonsági jogkivonatokat ezen felhasználók és az alkalmazások sikeres hitelesítés után kiállító felelős.
* Szeretne toooutsource hitelesítési tooAzure AD alkalmazás az Azure AD, ami regisztrálja, és egyedi módon azonosítja az hello könyvtárban hello alkalmazást regisztrálni kell.
* A fejlesztők a hello nyílt forráskódú az Azure AD hitelesítési könyvtárak toomake hitelesítési könnyen hello protokoll alfolyamatot kezelnek meg. Lásd: [Azure Active Directory hitelesítési Kódtárai](active-directory-authentication-libraries.md) további információt.

• Egyszer a felhasználó hitelesítése, hello alkalmazás hello felhasználó biztonsági jogkivonat tooensure ellenőrizni kell, hogy a hitelesítés sikeres volt, a hello szánt felek. A fejlesztők a szalagtárak toohandle hitelesítése bármely jogkivonat adta meg az Azure AD, beleértve a JSON Web Tokens (JWT) vagy a SAML 2.0 hello. Ha manuálisan szeretné tooperform érvényesítési, lásd: hello [JWT jogkivonat-kezelő](https://msdn.microsoft.com/library/dn205065.aspx) dokumentációját.

> [!IMPORTANT]
> Az Azure AD nyilvános kulcsú hitelesítésen toosign jogkivonatokat használ, és győződjön meg arról, hogy azok érvényesek. Lásd: [fontos információk kapcsolatos aláírási kulcs átfordulási az Azure AD](active-directory-signing-key-rollover.md) hello szükséges logika további információt, szerepelnie kell az alkalmazás tooensure mindig frissíteni hello legújabb kulcsokkal.
> 
> 

• hello kérelmeit és válaszait hello hitelesítési folyamat határozzák meg hello hitelesítési protokoll, amelyet használt, például az OAuth 2.0, az OpenID Connect, vagy a WS-Federation, SAML 2.0-s. Ezek a protokollok hello részletes ismertetése [Azure Active Directory hitelesítési protokolljai](active-directory-authentication-protocols.md) témakör és az alábbi szakaszokban hello.

> [!NOTE]
> Az Azure AD hello OAuth 2.0 támogatja, és OpenID Connect szabványok, amelyek nagy mennyiségű használja a tulajdonosi jogkivonatok, beleértve a tulajdonosi jogkivonatok JWTs-ként. Egy tulajdonosi jogkivonatot egy egyszerűsített biztonsági jogkivonatot, hogy biztosít hello "tulajdonos" hozzáférési tooa erőforrás védelemmel ellátva. Abban az értelemben a "tulajdonos" hello, akik hello token félre. Egy entitás először hitelesítenie kell magát az Azure AD tooreceive hello tulajdonosi jogkivonattal, ha hello szükséges lépések nem veszi toosecure hello lexikális elem szerepel az átvitel és a tárolási, bár hozzá, és egy nem kívánt félnek használják. Míg néhány biztonsági jogkivonatokat egy beépített mechanizmust meggátolja, hogy a nem hitelesített felek használja őket, a tulajdonosi jogkivonatok nem rendelkezik a mechanizmus, és kell szállítani, például a transport layer security (HTTPS) biztonságos csatorna. Egy tulajdonosi jogkivonatot egyértelmű hello küldött a rendszer, a man a hello középső támadás rosszindulatú fél tooacquire hello jogkivonat használatával, és használhassák az illetéktelen hozzáféréstől tooa védett erőforrás. hello ugyanazt biztonsági elveket alkalmazza, ha a tárolást, vagy a gyorsítótárazás tulajdonosi jogkivonatok későbbi használatra. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon tárolja a tulajdonosi jogkivonatokhoz. További biztonsági szempontok a tulajdonosi jogkivonatok, lásd: [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750).
> 
> 

Most, hogy hello alapjai, olvassa el alább toounderstand hello szakaszokat áttekintést hogyan kiépítés működik az Azure ad-ben, és támogatja a hello gyakori forgatókönyvek az Azure AD.

## <a name="claims-in-azure-ad-security-tokens"></a>Az Azure AD biztonsági jogkivonatokat a jogcím
Az Azure AD által kiadott biztonsági jogkivonatokat jogcímeket vagy helyességi feltételek hitelesítése hello tulajdonos információt tartalmaz. Ezeket a jogcímeket a különböző feladatokhoz hello alkalmazás által használható. Például használt toovalidate hello jogkivonatot kell, azonosítása hello tulajdonos directory-bérlő, felhasználói információk megjelenítése, megállapítása hello tulajdonos, és így tovább. hello jogcím szerepel a megadott biztonsági jogkivonat hello típusú jogkivonat, hello használt hitelesítő adat tooauthenticate hello felhasználó, és hello Alkalmazáskonfiguráció függenek. Minden Azure AD által kibocsátott jogcím típusú rövid leírása az alábbi táblázat hello valósul meg. További információkért tekintse meg túl[támogatott jogkivonat és jogcímtípusok](active-directory-token-and-claims.md).

| Jogcím | Leírás |
| --- | --- |
| Alkalmazásazonosító |Határozza meg, amely hello jogkivonatot használja hello alkalmazást. |
| Célközönség |Azonosítja a hello címzett erőforrás hello jogkivonat számára készült. |
| Alkalmazás hitelesítési környezeti osztályait ismertető dokumentációban |Azt jelzi, hogyan hello ügyfél hitelesített (nyilvános ügyfél és a bizalmas ügyfél). |
| Azonnali hitelesítés |Rekordok hello dátum és idő, amikor hello hitelesítési történt. |
| Hitelesítési módszer |Azt jelzi, hogyan hello tulajdonos hello jogkivonat hitelesítési (jelszó, tanúsítvány, stb.). |
| Utónév |Biztosít hello felhasználó utóneve hello beállítása az Azure ad-ben. |
| Csoportok |Objektum azonosítók az Azure AD-csoportok hello felhasználó tagja tartalmazza. |
| Identitásszolgáltató |Rekordok hello identitásszolgáltató hitelesített hello tulajdonos hello jogkivonat. |
| Ki: |Rekordok hello idő, mely hello token adta ki, lexikális elem frissesség gyakran használják. |
| Kiállító |Hello STS, amely a kibocsátott jogkivonat hello, valamint a hello Azure AD-bérlő azonosítja. |
| Vezetéknév |Biztosít hello felhasználó vezetékneve hello beállítása az Azure ad-ben. |
| Név |Emberi olvasható érték, amely azonosítja a hello tulajdonos hello jogkivonat biztosít. |
| Objektum azonosítója |Az Azure AD hello tulajdonos nem módosítható, egyedi azonosítót tartalmaz. |
| Szerepkörök |Rövid nevét tartalmazza az Azure AD alkalmazás-szerepkörök adott hello felhasználó számára engedélyezett. |
| Hatókör |Azt jelzi, hogy hello engedélyek megadott toohello ügyfélalkalmazást. |
| Tárgy |Azt jelzi, hogy mely hello kapcsolatos token állításokat információk hello rendszerbiztonsági tag. |
| Bérlő azonosítója |Hello directory bérlő hello jogkivonatot kibocsátó nem módosítható, egyedi azonosítót tartalmaz. |
| A jogkivonatok élettartama |Meghatározza a hello időtartam, amelyen belül a lexikális elem érvénytelen. |
| Egyszerű felhasználónév |Hello egyszerű felhasználónév hello tulajdonos tartalmazza. |
| Verzió |Hello token hello verziószámát tartalmazza. |

## <a name="basics-of-registering-an-application-in-azure-ad"></a>Egy alkalmazás regisztrálása az Azure AD alapjai
Bármely alkalmazás outsources hitelesítési tooAzure AD regisztrálni kell a könyvtárban található. Ez a lépés magában foglalja a szólítja fel az Azure AD az alkalmazásokkal, beleértve a hello URL-cím, ahol kapcsolatos található, hello URL-cím toosend válaszol hello URI tooidentify a hitelesítés után az alkalmazás és egyebek. Ezek az információk néhány főbb okból is szükséges:

* Az Azure AD meg kell koordináták toocommunicate hello alkalmazással bejelentkezés vagy cserélő jogkivonatok kezelésekor. Ezek közé tartozik a következő hello:
  
  * Application ID URI: hello alkalmazás azonosítója. Ez az érték tooAzure AD hitelesítési tooindicate melyik alkalmazás hello hívó szeretne rendelni egy token során zajlik. Ezenkívül ez az érték szerepel a hello token így hello alkalmazás tudja hello szánt cél volt.
  * Válasz URL-CÍMÉT és átirányítási URI-: hello esetében egy webes API-t vagy a webes alkalmazás hello válasz URL-CÍMEN az Azure AD küldi hello hitelesítési választ, beleértve a jogkivonatot, ha a hitelesítés sikerült hello hely toowhich. Egy natív alkalmazás hello esetben hello átirányítási URI-egy egyedi azonosítója az Azure AD toowhich átirányítja a hello felhasználói ügynök egy OAuth 2.0-lekérdezésben.
  * Alkalmazásazonosító: hello azonosító egy alkalmazás, amely jön létre az Azure ad hello alkalmazás regisztrálásakor. Az engedélyezési kód vagy a token kérésekor hello Alkalmazásazonosítót és a kulcs küldése tooAzure AD a hitelesítés során.
  * Kulcs: hello kulcs egy Alkalmazásazonosítót együtt küldött tooAzure AD toocall egy webes API hitelesítésekor.
* Az Azure AD-igények tooensure hello alkalmazás hello szükséges engedélyek tooaccess a címtár adataihoz, a szervezet más alkalmazásokat, és így tovább

Kiépítés válik tisztább, Miután megismerte a fejlett és az Azure AD integrált alkalmazások két kategóriába sorolhatók:

* Bérlői alkalmazások egyszeri: egy bérlő egyetlen alkalmazás használatát az egyik szervezet számára készült. Ezek a általában az üzletági (LoB) alkalmazások a vállalati fejlesztők által írt. Egy bérlő egyetlen alkalmazást csak kell toobe elérhetők a felhasználók számára pedig egy címtárban, ezért az emiatt csak egy címtárban kiépített toobe. Ezek az alkalmazások általában egy fejlesztő hello szervezet által regisztrált.
* Több-bérlős alkalmazás: A több-bérlős alkalmazás készült, számos szervezet, nem csak egyetlen szervezet. Ezek a független szoftverszállító (ISV) által írt általában szoftver,--szolgáltatás (SaaS) alkalmazások. Több-bérlős alkalmazásokhoz kiosztott összes könyvtárban, ahol használni fogják őket, felhasználói vagy rendszergazdai hozzájárulási tooregister igénylő toobe kell őket. A hozzájárulási folyamat elindul, amikor egy alkalmazás hello könyvtárban regisztrálva van, és kap hozzáférést toohello Graph API vagy akár egy másik webes API. Amikor egy felhasználó vagy rendszergazda egy másik szervezet előfizet a toouse hello alkalmazás, jelenjenek meg ezek egy párbeszédpanelt, amely megjeleníti a hello engedélyek hello az alkalmazáshoz. hello felhasználó vagy a rendszergazda számára hozzájárulási toohello alkalmazás, ahol hello alkalmazás hozzáférés toohello közölt adatok, és végül regiszterekben hello alkalmazás a címtárban. További információkért lásd: [hello hozzájárulás keretrendszer áttekintése](active-directory-integrating-applications.md#overview-of-the-consent-framework).

További megfontolásokat helyett egy bérlő egyetlen alkalmazás egy több-bérlős alkalmazások fejlesztése során merülhetnek fel. Például, ha az alkalmazás elérhető toousers több könyvtárban, van szüksége egy mechanizmus toodetermine mely bérlői is a. Egy egybérlős alkalmazást közben egy több-bérlős alkalmazást egy adott felhasználó összes hello címtárakban tooidentify kell az Azure ad-ben csak kell a saját címtárban toolook egy felhasználó. tooaccomplish ebben a feladatban az Azure AD biztosít egy közös hitelesítési végpont, ahol több-bérlős alkalmazásokat lehet konfigurálni a bejelentkezési kérelmek, a bérlő-specifikus végpont helyett. Ehhez a végponthoz összes könyvtár https://login.microsoftonline.com/common az Azure ad-ben, mivel lehet, hogy a bérlő-specifikus végpont https://login.microsoftonline.com/contoso.onmicrosoft.com. hello közös végpont akkor különösen fontos tooconsider, ha az alkalmazás fejlesztése, mert szükség lesz hello szükséges logika toohandle több bérlő bejelentkezési, kijelentkezési, és a jogkivonatok érvényesség-ellenőrzése során.

Ha jelenleg egy egybérlős alkalmazást fejleszt, de szeretné, hogy toomake az elérhető toomany szervezetek könnyen tehet módosítások toohello alkalmazás és konfigurációja az Azure AD toomake, több-bérlős képes. Emellett az Azure AD használ hello azonos aláírási kulcs található összes összes jogkivonatokat, hogy meg van adva egy bérlő egyetlen vagy több-bérlős alkalmazás hitelesítési.

A jelen dokumentumban szereplő minden egyes forgatókönyv egy alterület, mely leírja az üzembe helyezési követelményei tartalmazza. Az Azure AD alkalmazás-kiépítés további részletes információt és hello egyetlen és több-bérlős alkalmazások közötti különbségeket, lásd: [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md) további információt. Továbbra is az Azure AD hello alkalmazás szabhatják toounderstand olvasásakor.

## <a name="application-types-and-scenarios"></a>Alkalmazástípusok és forgatókönyvek
Egyes hello forgatókönyveket a jelen dokumentumban ismertetett fejleszthetők a különböző nyelvekhez és platformokhoz használatával. Azok az összes üzemelnek elérhető teljes Kódminták a [mintakódok útmutató](active-directory-code-samples.md), vagy közvetlenül a megfelelő hello [minta GitHub-adattárak](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory). Emellett ha az alkalmazásnak egy adott vagy egy végpontok közötti forgatókönyv szegmens, a legtöbb esetben funkció felveheti egymástól függetlenül. Például ha egy natív alkalmazás, amely behívja a webes API-k, egyszerűen hozzáadhatja egy webes alkalmazás, amely is hívja hello webes API-t. hello következő diagram azt ábrázolja, ezek a forgatókönyvek és alkalmazástípusokat, és hogyan felveheti különböző összetevőket:

![Alkalmazástípusok és forgatókönyvek](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Hello Azure AD által támogatott öt elsődleges alkalmazás forgatókönyvek a következők:

* [Webalkalmazás-böngésző tooWeb alkalmazás](#web-browser-to-web-application): A felhasználó számára szükséges toosign tooa webes alkalmazás, amelyet az Azure AD.
* [Egyetlen lap alkalmazás (SPA)](#single-page-application-spa): A felhasználó számára szükséges toosign alkalmazásban tooa egyetlen lap, amelyet az Azure AD.
* [Natív alkalmazás tooWeb API](#native-application-to-web-api): egy natív alkalmazás, amely futtatja a telefon, táblagép vagy PC igények tooauthenticate egy felhasználó tooget erőforrások webes API-k, amelyet az Azure AD.
* [Webes alkalmazás tooWeb API-t](#web-application-to-web-api): webalkalmazás kell egy webes API-t az Azure AD által védett tooget erőforrásokat.
* [Démon vagy kiszolgálói alkalmazás tooWeb API](#daemon-or-server-application-to-web-api): egy démon alkalmazást vagy egy kiszolgálói alkalmazás webes felhasználói felület nélkül ki egy webes API-t az Azure AD által védett tooget erőforrásokat.

### <a name="web-browser-tooweb-application"></a>Webes böngésző tooWeb alkalmazás
Ez a szakasz olyan alkalmazás, amely hitelesíti a felhasználót egy webes böngésző tooa webalkalmazásban ismerteti. Ebben a forgatókönyvben hello webalkalmazás hello felhasználói böngésző toosign irányítja őket a tooAzure AD. Az Azure AD keresztül hello felhasználói browser bővítményt, amely tartalmaz egy biztonsági jogkivonatba hello felhasználóval kapcsolatos jogcímek bejelentkezési választ ad. Ebben a forgatókönyvben támogatja bejelentkezést hello WS-Federation, SAML 2.0 és OpenID Connect protokollok használatával.

#### <a name="diagram"></a>Ábra
![A böngésző tooweb alkalmazás hitelesítési folyamat](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protokoll folyamat leírása
1. Ha a felhasználó meglátogat hello alkalmazás, és a toosign, a rendszer visszairányítja a bejelentkezési kérelem toohello hitelesítési végpontot keresztül az Azure ad-ben.
2. hello felhasználó bejelentkezik hello bejelentkezési oldalára.
3. Sikeres hitelesítés esetén az Azure AD egy hitelesítési jogkivonatot hoz létre, és egy bejelentkezési válasz toohello alkalmazás válasz URL-CÍMEN hello Azure Portal konfigurált adja vissza. Termelési alkalmazások esetében a válasz URL-CÍMEN HTTPS kell lennie. adott vissza hello hello felhasználóról és az Azure AD hello alkalmazás toovalidate hello lexikális elem által igényelt jogcímeket tartalmaz.
4. hello alkalmazás használatával egy nyilvános aláírási kulcs és a kiállítói információk érhetők el hello összevonási metaadat-dokumentum az Azure AD érvényesíti hello jogkivonatot. Hello alkalmazás érvényesíti hello jogkivonatot, miután az Azure AD egy új munkamenet kezdődik hello felhasználó. Ehhez a munkamenethez lehetővé teszi a felhasználó tooaccess hello hello alkalmazást amíg le nem jár.

#### <a name="code-samples"></a>Kódminták
Lásd: hello mintakódok webböngésző tooWeb alkalmazás forgatókönyvek esetén. És foglalkozzon gyakran--jelenleg felvenni új minták összes hello idő. [Webalkalmazás-böngésző tooWeb alkalmazás](active-directory-code-samples.md#web-browser-to-web-application).

#### <a name="registering"></a>Regisztrálása
* Egybérlős: Ha egy alkalmazás csak a szervezet számára, regisztrálnia kell azt a vállalati címtárban hello Azure portál használatával.
* Több-Bérlős: Ha egy alkalmazás, amely a szervezeten kívüli felhasználók által használható, az szerepelnie kell a vállalati címtárban, hanem is regisztrálni kell a hello alkalmazást használó minden egyes szervezet címtárához. toomake az alkalmazás a címtár érhető el, megadhatja a regisztrációs folyamat, amely tooconsent tooyour alkalmazás lehetővé teszi az ügyfeleknek. Amikor bejelentkeznek az alkalmazáshoz, azok számára jelenik meg olyan párbeszédpanel, amely hello engedélyek hello alkalmazás igényel, és majd a beállítás tooconsent hello jeleníti meg. Attól függően, hello szükséges engedélyekkel, rendszergazda hello a másik szervezet lehet toogive hozzájárulási szükséges. Hello felhasználói vagy rendszergazdai hozzájárul, hello alkalmazás regisztrálja a címtárban. További információkért lásd: [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
hello felhasználói munkamenet lejár, ha az Azure AD által kibocsátott hello jogkivonat élettartamát hello lejár. Az alkalmazás ebben az időszakban szükség esetén például a felhasználók adott ideig tartó tétlenség alapján kijelentkezés lerövidíthető. Hello munkamenet lejártakor hello felhasználó el a kért toosign újra.

### <a name="single-page-application-spa"></a>Egylapos alkalmazások (SPA)
Ez a szakasz ismerteti a egyetlen lap, használó alkalmazások az Azure AD hitelesítési és hello OAuth 2.0 típusú implicit engedélyezési adja vissza a webes API end toosecure. Egyetlen lap alkalmazások általában struktúráját rétegként JavaScript bemutató (előtér-) futó hello böngésző és a Web API háttérből, amely az olyan kiszolgálón fut, és megvalósít hello alkalmazás üzleti logikát. További információ az hello implicit hitelesítésengedélyezési, és úgy dönt, hogy az alkalmazás helyzetnek megfelelő súgó toolearn lásd [ismertetése hello OAuth2 implicit adja meg az Azure Active Directoryban folyamat](active-directory-dev-understanding-oauth2-implicit-grant.md).

Ebben a forgatókönyvben a hello felhasználó jelentkezik be, amikor hello JavaScript első célból használ [Active Directory hitelesítési tár JavaScript (adal-t. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) és hello implicit engedélyezési tooobtain egy azonosító jogkivonatot (id_token) az Azure AD. hello jogkivonat található a gyorsítótárban, és hello ügyfél csatolja toohello kérelem hello tulajdonosi tokenként, amikor webes API tooits háttér, amely hello OWIN köztes használatával lett biztonságossá téve. 

#### <a name="diagram"></a>Ábra
![Egyetlen lapon alkalmazásdiagram](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Protokoll folyamat leírása
1. hello felhasználó toohello webalkalmazás lép.
2. hello alkalmazás hello JavaScript előtér (bemutató réteg) toohello böngésző adja vissza.
3. hello felhasználói bejelentkezés, például kezdeményez egy szereplő bejelentkezési hivatkozásra kattintva. hello böngésző küld egy GET toohello az Azure AD hitelesítési végpont toorequest egy azonosító jogkivonatot. A kérelem hello lekérdezési paraméterek hello alkalmazás azonosítója és válasz URL-címet tartalmazza.
4. Az Azure AD válasz URL-CÍMEN elleni hello regisztrált válasz URL-CÍMEN hello Azure Portal konfigurált hello ellenőrzi.
5. hello felhasználó bejelentkezik hello bejelentkezési oldalára.
6. Sikeres hitelesítés esetén az Azure AD egy azonosító adatblokkot hoz létre, és visszaadja egy URL-címe (#) töredék toohello alkalmazás válasz URL-címként. Termelési alkalmazások esetében a válasz URL-CÍMEN HTTPS kell lennie. adott vissza hello hello felhasználóról és az Azure AD hello alkalmazás toovalidate hello lexikális elem által igényelt jogcímeket tartalmaz.
7. hello hello böngészőjében futó JavaScript-Ügyfélkód hello token kiolvassa a hello válasz toouse toohello alkalmazás webes API vissza end hívások védelméhez.
8. hello böngésző hívások hello alkalmazás webes API vissza végződhet hello jogkivonatokkal hello authorization fejlécet.

#### <a name="code-samples"></a>Kódminták
Lásd: hello mintakódok lap alkalmazás (SPA) forgatókönyvek esetén. Meg arról, hogy toocheck vissza gyakran--azt hozzáadása új minták összes hello idő. [Egyetlen lap alkalmazás (SPA)](active-directory-code-samples.md#single-page-application-spa).

#### <a name="registering"></a>Regisztrálása
* Egybérlős: Ha egy alkalmazás csak a szervezet számára, regisztrálnia kell azt a vállalati címtárban hello Azure portál használatával.
* Több-Bérlős: Ha egy alkalmazás, amely a szervezeten kívüli felhasználók által használható, az szerepelnie kell a vállalati címtárban, hanem is regisztrálni kell a hello alkalmazást használó minden egyes szervezet címtárához. toomake az alkalmazás a címtár érhető el, megadhatja a regisztrációs folyamat, amely tooconsent tooyour alkalmazás lehetővé teszi az ügyfeleknek. Amikor bejelentkeznek az alkalmazáshoz, azok számára jelenik meg olyan párbeszédpanel, amely hello engedélyek hello alkalmazás igényel, és majd a beállítás tooconsent hello jeleníti meg. Attól függően, hello szükséges engedélyekkel, rendszergazda hello a másik szervezet lehet toogive hozzájárulási szükséges. Hello felhasználói vagy rendszergazdai hozzájárul, hello alkalmazás regisztrálja a címtárban. További információkért lásd: [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).

Hello alkalmazás regisztrálása után a konfigurált toouse OAuth 2.0 Implicit Grant protokoll kell legyen. Alapértelmezés szerint ez a protokoll le van tiltva, az alkalmazások. tooenable hello OAuth2 Implicit Grant protokollt az alkalmazás szerkesztése az alkalmazásjegyzék a hello Azure portálon, és állítsa be a hello "oauth2AllowImplicitFlow" érték tootrue. Részletes útmutatásért lásd: [engedélyezése OAuth 2.0 Implicit Grant egyetlen oldal alkalmazások](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
Az Azure ad-val ADAL.js toomanage hitelesítés használatakor, kihasználhatja a több funkciót is lehetővé teszi egy lejárt jogkivonat frissítését, valamint a jogkivonatok lekérésének további webes API erőforrások hello alkalmazás elnevezése lehet. Hello felhasználó sikeresen hitelesíti magát, és az Azure AD, ha a cookie-k által védett munkamenet hello felhasználó hello böngésző és az Azure AD között. Fontos, hogy hello munkamenet létezik hello felhasználói és az Azure AD között és kapcsolat nem hello felhasználói és hello webes alkalmazás hello kiszolgálón futó toonote. Ha a jogkivonat lejár, ADAL.js használja ehhez a munkamenethez toosilently egy másik jogkivonat beszerzése. Ezt egy rejtett iFrame toosend használatával hajtja végre, és hello OAuth Implicit Grant protokollt használó hello kérés fogadásához. ADAL.js is használhatja ugyanazt a mechanizmust toosilently hozzáférési tokenek beszerzése érdekében az Azure AD más webes API-erőforrások, hogy hello alkalmazás hívások mindaddig, amíg ezek az erőforrások támogatja az eltérő eredetű erőforrások megosztása (CORS) regisztrált hello felhasználói könyvtárba, és minden szükséges beleegyezést hello felhasználó adott bejelentkezés során.

### <a name="native-application-tooweb-api"></a>Natív alkalmazás tooWeb API
Ez a szakasz ismerteti a natív alkalmazás, amely behívja a webes API-k egy felhasználó nevében. Ebben a forgatókönyvben a hello OAuth 2.0 hitelesítési kód engedélytípus nyilvános ügyféllel épül, a hello 4.1 leírtak [OAuth 2.0 specifikáció](http://tools.ietf.org/html/rfc6749). hello natív alkalmazás hello felhasználói számára egy jogkivonatot kap hello OAuth 2.0 protokoll használatával. Ez a jogkivonat küldi el a rendszer a hello kérelem toohello webes API-t, amely engedélyezi a felhasználó hello hello szükséges erőforrás visszaadja.

#### <a name="diagram"></a>Ábra
![Natív alkalmazás tooWeb API diagramja](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-tooapi"></a>Natív alkalmazás tooAPI hitelesítési folyamata
#### <a name="description-of-protocol-flow"></a>Protokoll folyamat leírása
Hello AD hitelesítési könyvtárat használja, ha hello protokoll adatai az alábbiakban a legtöbb történik meg, például a hello böngészőben előugró ablak, a token-gyorsítótárazási és a frissítési jogkivonatokat kezelése.

1. A böngészőben előugró hello natív alkalmazás lehetővé teszi egy kérelem toohello engedélyezési végpont Azure AD-ben. Ehhez a kérelemhez a hello Alkalmazásazonosítót és hello átirányítási URI-t hello natív alkalmazás hello Azure portálon, és hello alkalmazás Alkalmazásazonosító URI hello webes API-t tartalmaz. Hello felhasználó még nem jelentkezett be, ha azok a kért toosign újra
2. Az Azure AD akkor hitelesíti a hello felhasználót. Ha olyan több-bérlős alkalmazás és a hozzájárulásukat adják szükséges toouse hello alkalmazás, hello felhasználói szükséges tooconsent lesz, ha azok még nem tette meg. Hozzájárulás megadása után, és a sikeres hitelesítés után az Azure AD kibocsát egy engedélyezési kódot válaszban hátsó toohello ügyfél alkalmazás átirányítási URI-t.
3. Ha az Azure AD kibocsát egy engedélyezési kód választ küld vissza toohello átirányítási URI-címe, hello ügyfél alkalmazás leáll böngésző interakció és kivonatok hello szereplő engedélyezési kódot hello válasz. Az engedélyezési kód használatával hello ügyfélalkalmazás küld egy kérelem tooAzure AD a token-végpont hello engedélyezési kódot tartalmazó, részletek készül hello ügyfélalkalmazás (alkalmazás Azonosítóját és átirányítási URI) és a kívánt erőforrás (Alkalmazásazonosító hello URI-JÁNAK hello webes API-k).
4. hello engedélyezési kód és információ hello ügyfélalkalmazást és a webes API-t az Azure AD érvényesíti. Sikeres ellenőrzés esetén az Azure AD két jogkivonatok adja vissza: a JWT jogkivonat és egy frissítési JWT jogkivonat. Ezenkívül az Azure AD hello felhasználó, például a megjelenítendő nevét és a bérlői azonosító alapvető információt ad vissza
5. A HTTPS PROTOKOLLOKON keresztül hello ügyfélalkalmazás hello hello engedélyezési fejlécének hello kérelem toohello webes API-t adott vissza a JWT access token tooadd hello JWT karakterlánc a "Tulajdonos" jelöléssel. hello webes API-k majd érvényesíti hello JWT jogkivonatot, és ellenőrzés nem jelez hibát, ha vissza hello szükséges erőforrás.
6. Amikor hello hozzáférési jogkivonat lejár, hello ügyfélalkalmazást, amely jelzi, hogy újra meg kell tooauthenticate hello felhasználó hiba fog kapni. Hello alkalmazásnak meg egy érvényes frissítési jogkivonat, ha azok egy új hozzáférési jogkivonat értesítése nélkül a felhasználó toosign újra hello használt tooacquire. Ha hello frissítési jogkivonat lejár, a hello alkalmazásnak szüksége lesz toointeractively ismét hello felhasználó hitelesítéséhez.

> [!NOTE]
> hello frissítési jogkivonat az Azure AD által kibocsátott lehet használt tooaccess több erőforrást. Például ha egy ügyfél-alkalmazás, amely rendelkezik engedéllyel a két toocall webes API-k, a hello frissítési jogkivonat lehet használt tooget egy hozzáférési jogkivonat toohello más webes API-t is.
> 
> 

#### <a name="code-samples"></a>Kódminták
Lásd: hello mintakódok natív alkalmazás tooWeb API forgatókönyvek esetén. És foglalkozzon gyakran--jelenleg felvenni új minták összes hello idő. [Natív alkalmazás tooWeb API](active-directory-code-samples.md#native-application-to-web-api).

#### <a name="registering"></a>Regisztrálása
* Egyetlen bérlői: Mindkét hello natív alkalmazás, és hello webes API-t kell regisztrálni az hello ugyanabban a könyvtárban az Azure ad-ben. hello webes API-k lehet konfigurált tooexpose engedélyekkel, amelyek tooits erőforrások használt toolimit hello natív alkalmazás eléréséhez. majd ügyfélalkalmazás hello hello szükséges engedélyek hello "Engedélyek tooOther alkalmazások" legördülő menüjében hello Azure portálon lévő választja ki.
* Több-Bérlős: Először hello natív alkalmazás csak regisztrált hello fejlesztői vagy közzétevő könyvtár. Hello natív alkalmazás, konfigurált tooindicate hello engedélyek működési toobe van szükség. A szükséges engedélyek listája párbeszédpanel jelenik meg, amikor egy felhasználó vagy rendszergazda hello célkönyvtárban hozzájárulási toohello alkalmazás, így a rendelkezésre álló tootheir szervezet. Egyes alkalmazások csak a felhasználói szintű engedélyek, amelyek hello szervezet bármely felhasználója is szükséges. Más alkalmazásoknak rendszergazdai engedélyek, amelyek hello szervezetben a felhasználók nem járulhatnak hozzá. A címtár csak a rendszergazda biztosíthat hozzájárulási tooapplications, ez a jogosultsági szint szükséges. Hello felhasználói vagy rendszergazdai hozzájárul, csak hello webes API-k regisztrálja a címtárban. További információkért lásd: [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
Hello natív alkalmazás használja az engedélyezési kód tooget a JWT jogkivonat, amikor JWT frissítési jogkivonat is megkapja. Amikor hello hozzáférési jogkivonat lejár, a hello frissítési jogkivonat lehet-e a használt toore-anélkül, hogy azokat újra a toosign hello felhasználó hitelesítéséhez. A frissítési jogkivonat nem használt tooauthenticate hello felhasználói, mely eredményezi, hogy egy új hozzáférési tokent, és frissítse token.

### <a name="web-application-tooweb-api"></a>Webes alkalmazás tooWeb API
Ez a szakasz ismerteti a webalkalmazás, amelyet a webes API-k tooget erőforrásokat. Ilyen esetben van két identity típus, amely a webes alkalmazás hello tooauthenticate használhatja és hello webes API hívása: az alkalmazás azonosítóját, vagy delegált felhasználói azonosítót.

*Az Alkalmazásidentitás:* a forgatókönyvet használja az OAuth 2.0 ügyfél hitelesítő adatai megadják tooauthenticate hello alkalmazás és a hozzáférés hello webes API-t. Ha egy alkalmazás identitásával hello webes API csak azt észleli, hogy hello webalkalmazás hívja meg, hello, webes API-k nem kapott információt hello felhasználóról. Hello alkalmazás hello felhasználó adatai kap, ha küldi hello alkalmazás protokollon keresztül, és azt nem írta alá az Azure AD. hello webes API-k megbízhatónak tekinti, hogy a webalkalmazás hello hello felhasználó hitelesítése. Ezért ez a minta egy megbízható alrendszer neve.

*Felhasználói azonosító meghatalmazott:* ebben a forgatókönyvben két módon valósítható meg: OpenID Connect, és a kód hitelesítésengedélyezési OAuth 2.0 bizalmas ügyféllel. hello webalkalmazás jut hozzá a hozzáférési token hello felhasználóhoz, amely bizonyítja toohello webes API-k hello sikeresen hitelesített felhasználó toohello webalkalmazáshoz, és hello webes alkalmazás nem képes tooobtain delegált felhasználói identitás toocall hello webes API-k. Ez a jogkivonat küldött hello kérelem toohello webes API-t, amely engedélyezi a felhasználó hello hello szükséges erőforrás visszaadja.

#### <a name="diagram"></a>Ábra
![Webszolgáltatás alkalmazásdiagram tooWeb API](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protokoll folyamat leírása
Mindkét hello Alkalmazásidentitás és delegált felhasználói identitás típusai hello folyamata az alábbi esik szó. hello a fő különbség a kettő között, hogy meghatalmazott hello felhasználói identitás először kell szerezni az engedélyezési kód, mielőtt hello felhasználó bejelentkezési és szerezhet hozzáférést toohello webes API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Az OAuth 2.0 ügyfél Alkalmazásidentitás hitelesítő adatok megadása
1. TooAzure AD hello webes alkalmazás a bejelentkezett felhasználó (lásd: hello [webböngésző tooWeb alkalmazás](#web-browser-to-web-application) fent).
2. hello webalkalmazásnak kell tooacquire olyan hozzáférési jogkivonatot, hogy a hitelesítéshez toohello webes API és hello szükséges erőforrás lekérése. A kérelem tooAzure AD meg jogkivonat végpontjához hello hitelesítő adat, Alkalmazásazonosítót és webes API-alkalmazás azonosítója URI teszi.
3. Az Azure AD hitelesíti a hello alkalmazás, és a JWT jogkivonat, amely használt toocall hello webes API adja vissza.
4. A HTTPS PROTOKOLLOKON keresztül hello webalkalmazás hello JWT access token tooadd hello JWT karakterlánc a "Tulajdonos" jelöléssel visszaadott hello engedélyezési fejlécének hello kérelem toohello webes API-t használ. hello webes API-k majd érvényesíti hello JWT jogkivonatot, és ellenőrzés nem jelez hibát, ha vissza hello szükséges erőforrás.

##### <a name="delegated-user-identity-with-openid-connect"></a>Delegált felhasználói identitás openid Connect
1. Tooa webalkalmazását az Azure AD használatával bejelentkezett felhasználó (lásd: hello [webböngésző tooWeb alkalmazás](#web-browser-to-web-application) fenti szakaszban). Ha hello felhasználói hello webes alkalmazás nem hozzájárult még tooallowing hello webes alkalmazás toocall hello webes API-t a nevében, hello felhasználói tooconsent kell. hello alkalmazás megjeleníti hello engedélyeket igényel, és ha ezek egyikét sem rendszergazdai engedélyek, a normál felhasználói hello címtárban nem lesz képes tooconsent. Hello alkalmazás már rendelkezik hello szükséges engedélyekkel a hozzájárulási folyamat csak toomulti bérlői alkalmazások, nem egybérlős alkalmazások alkalmazza. Hello bejelentkezett felhasználó, hello webalkalmazás kapott egy azonosító jogkivonat hello felhasználó, valamint az engedélyezési kód kapcsolatos információkat.
2. Hello engedélyezési kód az Azure AD által kibocsátott használatával, hello webes alkalmazás küld egy kérelem tooAzure AD a token-végpont hello engedélyezési kódot tartalmazó, hello ügyfélalkalmazás (alkalmazás Azonosítóját és átirányítási URI-t), és hello szükségeskonfiguráció-erőforrás) Application ID URI hello webes API).
3. hello engedélyezési kód és információ hello webalkalmazás és a webes API-t az Azure AD érvényesíti. Sikeres ellenőrzés esetén az Azure AD két jogkivonatok adja vissza: a JWT jogkivonat és egy frissítési JWT jogkivonat.
4. A HTTPS PROTOKOLLOKON keresztül hello webalkalmazás hello JWT access token tooadd hello JWT karakterlánc a "Tulajdonos" jelöléssel visszaadott hello engedélyezési fejlécének hello kérelem toohello webes API-t használ. hello webes API-k majd érvényesíti hello JWT jogkivonatot, és ellenőrzés nem jelez hibát, ha vissza hello szükséges erőforrás.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Az OAuth 2.0 kód Hitelesítésengedélyezési delegált felhasználók identitását
1. A felhasználó már bejelentkezett tooa webes alkalmazás, amelynek hitelesítési módszer használata az Azure AD független.
2. hello webes alkalmazáshoz szükséges engedélyezési kód a hozzáférési tokent, tooacquire, az általa kiállított hello böngésző tooAzure AD meg hitelesítési végpont, hello Alkalmazásazonosító megadása egy kérelmet, és átirányítási URI hello webalkalmazás után sikeres hitelesítés. hello felhasználó bejelentkezik tooAzure AD.
3. Ha hello felhasználói hello webes alkalmazás nem hozzájárult még tooallowing hello webes alkalmazás toocall hello webes API-t a nevében, hello felhasználói tooconsent kell. hello alkalmazás megjeleníti hello engedélyeket igényel, és ha ezek egyikét sem rendszergazdai engedélyek, a normál felhasználói hello címtárban nem lesz képes tooconsent. A hozzájárulási tooboth egyetlen és több-bérlős alkalmazás vonatkozik.  Hello egybérlős esetében egy rendszergazda hajthat végre a rendszergazda jóváhagyását tooconsent azok a felhasználók nevében.  Ezt megteheti hello segítségével `Grant Permissions` hello gombjára [Azure Portal](https://portal.azure.com). 
4. Miután hello felhasználó hozzájárult, hello webalkalmazás kap hello engedélyezési kódot, hogy kell-e tooacquire olyan hozzáférési jogkivonatot.
5. Hello engedélyezési kód az Azure AD által kibocsátott használatával, hello webes alkalmazás küld egy kérelem tooAzure AD a token-végpont hello engedélyezési kódot tartalmazó, hello ügyfélalkalmazás (alkalmazás Azonosítóját és átirányítási URI-t), és hello szükségeskonfiguráció-erőforrás) Application ID URI hello webes API).
6. hello engedélyezési kód és információ hello webalkalmazás és a webes API-t az Azure AD érvényesíti. Sikeres ellenőrzés esetén az Azure AD két jogkivonatok adja vissza: a JWT jogkivonat és egy frissítési JWT jogkivonat.
7. A HTTPS PROTOKOLLOKON keresztül hello webalkalmazás hello JWT access token tooadd hello JWT karakterlánc a "Tulajdonos" jelöléssel visszaadott hello engedélyezési fejlécének hello kérelem toohello webes API-t használ. hello webes API-k majd érvényesíti hello JWT jogkivonatot, és ellenőrzés nem jelez hibát, ha vissza hello szükséges erőforrás.

#### <a name="code-samples"></a>Kódminták
Lásd: hello mintakódok webalkalmazás tooWeb API forgatókönyvek esetén. És foglalkozzon gyakran--jelenleg felvenni új minták összes hello idő. Webes [alkalmazás tooWeb API](active-directory-code-samples.md#web-application-to-web-api).

#### <a name="registering"></a>Regisztrálása
* Egyetlen bérlői: Hello Alkalmazásidentitás és a felhasználó delegált identitása esetekben hello webalkalmazás és hello webes API-t kell regisztrálni az hello ugyanabban a könyvtárban az Azure ad-ben. hello webes API-k lehet konfigurált tooexpose engedélyekkel, amelyek tooits erőforrások használt toolimit hello webes alkalmazás eléréséhez. A delegált felhasználói identitástípus használatos, ha a hello webalkalmazásnak kell tooselect hello szükséges engedélyek a hello Azure Portal hello "Engedélyek tooOther alkalmazások" legördülő menüből. Ebben a lépésben nincs szükség, ha hello alkalmazás identity típus használatban van.
* Több-Bérlős: Először hello webalkalmazás konfigurált tooindicate hello engedélyek működési toobe van szükség. A szükséges engedélyek listája párbeszédpanel jelenik meg, amikor egy felhasználó vagy rendszergazda hello célkönyvtárban hozzájárulási toohello alkalmazás, így a rendelkezésre álló tootheir szervezet. Egyes alkalmazások csak a felhasználói szintű engedélyek, amelyek hello szervezet bármely felhasználója is szükséges. Más alkalmazásoknak rendszergazdai engedélyek, amelyek hello szervezetben a felhasználók nem járulhatnak hozzá. A címtár csak a rendszergazda biztosíthat hozzájárulási tooapplications, ez a jogosultsági szint szükséges. Amikor hello felhasználói vagy rendszergazdai hozzájárulásokat azoktól, hello webes alkalmazáshoz és hello webes API egyaránt regisztrálva van a címtárban.

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
Hello webalkalmazás az engedélyezési kód tooget a JWT jogkivonat használ, amikor JWT frissítési jogkivonat is megkapja. Amikor hello hozzáférési jogkivonat lejár, a hello frissítési jogkivonat lehet-e a használt toore-anélkül, hogy azokat újra a toosign hello felhasználó hitelesítéséhez. A frissítési jogkivonat nem használt tooauthenticate hello felhasználói, mely eredményezi, hogy egy új hozzáférési tokent, és frissítse token.

### <a name="daemon-or-server-application-tooweb-api"></a>Démon vagy kiszolgálói alkalmazás tooWeb API
Ez a szakasz ismerteti, amelyet a webes API-k tooget erőforrásokat démon vagy kiszolgálói alkalmazás. Két alárendelt esetben toothis szakasz alkalmazó: toocall egy webes API-t OAuth 2.0 ügyfél hitelesítő adatok engedélytípus; épülő igénylő démon és egy kiszolgáló alkalmazások (például a webes API-k), amelyet a toocall a webes API-k, épülő OAuth 2.0 On-Behalf-Of vázlat megadását.

Hello a forgatókönyvhöz egy démon alkalmazást toocall egy webes API-t kell esetén fontos toounderstand folyamatban van. Először nincs lehetőség démon alkalmazással, amelyhez hello alkalmazás toohave saját identitás szükséges felhasználói közreműködés. Egy démon alkalmazás például egy kötegelt vagy hello háttérben futó operációs rendszer szolgáltatásnak. Ez az alkalmazástípus hozzáférési tokent az Alkalmazásidentitás használatával, és a Alkalmazásazonosító, a hitelesítő adatok (jelszó vagy a tanúsítvány) és a kérelem URI azonosítója tooAzure AD bemutató kéri. Sikeres hitelesítést követően hello démon megkapja a hozzáférési tokent az Azure AD, amely akkor használt toocall hello webes API.

Hello a forgatókönyvhöz egy kiszolgálói alkalmazás toocall egy webes API-t kell esetén hasznos toouse példa. Tegyük fel, hogy a felhasználó a natív alkalmazás hitelesítette, és a natív alkalmazást kell toocall egy webes API-t. Az Azure AD kibocsát egy JWT access token toocall hello webes API-t. Hello webes API-t kell toocall egy másik alsóbb rétegbeli webes API-t, ha el hello a-meghatalmazásos folyamat toodelegate hello felhasználó identitását, és hitelesíteni toohello alacsonyabb szintű webes API-t.

#### <a name="diagram"></a>Ábra
![Démon vagy kiszolgálói alkalmazás tooWeb API diagramja](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protokoll folyamat leírása
##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Az OAuth 2.0 ügyfél Alkalmazásidentitás hitelesítő adatok megadása
1. Első lépésként hello server alkalmazást kell tooauthenticate az Azure ad szolgáltatással, mint például az interaktív bejelentkezési párbeszédpanel emberi beavatkozás nélkül. Így a kérelem tooAzure AD meg jogkivonat végpontjához hello hitelesítő adat, Alkalmazásazonosítót és alkalmazás URI azonosítója.
2. Az Azure AD hitelesíti a hello alkalmazás, és a JWT jogkivonat, amely használt toocall hello webes API adja vissza.
3. A HTTPS PROTOKOLLOKON keresztül hello webalkalmazás hello JWT access token tooadd hello JWT karakterlánc a "Tulajdonos" jelöléssel visszaadott hello engedélyezési fejlécének hello kérelem toohello webes API-t használ. hello webes API-k majd érvényesíti hello JWT jogkivonatot, és ellenőrzés nem jelez hibát, ha vissza hello szükséges erőforrás.

##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>OAuth 2.0 a nevében-az Vázlat meghatározásával delegált felhasználói azonosító
alább hello folyamata azt feltételezi, hogy, hogy a felhasználó hitelesítése egy másik alkalmazástól (például egy natív alkalmazást), és a felhasználói identitás használt tooacquire egy hozzáférési jogkivonat toohello első szintű webes API-t.

1. hello natív alkalmazás hello access token toohello első szintű webes API-t küld.
2. hello első szintű webes API-t küld egy kérelem tooAzure AD meg token-végpont, így az alkalmazás Azonosítóját és hitelesítő adatait, valamint hello felhasználói jogkivonat. Ezenkívül hello kérelem nem jut egy paraméter, amely azt jelzi, hello on_behalf_of API által kért új webes jogkivonatok toocall egy alsóbb rétegbeli webes API hello eredeti felhasználó nevében.
3. Az Azure AD ellenőrzi a hello első szintű web API engedélyek tooaccess hello alacsonyabb szintű webes API rendelkezik, és érvényesíti a hello kérelem vissza a JWT jogkivonat és egy JWT jogkivonat toohello első szintű webes API frissítése.
4. A HTTPS PROTOKOLLOKON keresztül hello első szintű webes API majd meghívja a második szintű webes API-k hello hello lexikális elem karakterlánca hello engedélyezési fejlécben hello kérelem hozzáfűzésével. hello első szintű webes API-t továbbra is toocall hello alacsonyabb szintű webes API-t, mindaddig, amíg hello hozzáférési jogkivonat és frissítési jogkivonatok érvényesek.

#### <a name="code-samples"></a>Kódminták
Lásd: hello mintakódok démon vagy kiszolgálói alkalmazás tooWeb API forgatókönyvek esetén. És foglalkozzon gyakran--jelenleg felvenni új minták összes hello idő. [Kiszolgáló vagy démon alkalmazás tooWeb API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Regisztrálása
* Egybérlős: Hello Alkalmazásidentitás és a felhasználó delegált identitása esetekben hello démon vagy server alkalmazást regisztrálni kell a hello ugyanabban a könyvtárban az Azure ad-ben. hello webes API-k konfigurált tooexpose engedélyekkel, amelyek használt toolimit hello démon vagy a kiszolgáló hozzáférés tooits erőforrásait is lehet. A delegált felhasználói identitástípus használatos, ha hello server alkalmazást kell hello "Engedélyek tooOther alkalmazások" legördülő menüjében hello Azure portálon lévő tooselect hello szükséges engedélyekkel. Ebben a lépésben nincs szükség, ha hello alkalmazás identity típus használatban van.
* Több-Bérlős: Először hello démon vagy kiszolgálói alkalmazás konfigurált tooindicate hello engedélyek működési toobe van szükség. A szükséges engedélyek listája párbeszédpanel jelenik meg, amikor egy felhasználó vagy rendszergazda hello célkönyvtárban hozzájárulási toohello alkalmazás, így a rendelkezésre álló tootheir szervezet. Egyes alkalmazások csak a felhasználói szintű engedélyek, amelyek hello szervezet bármely felhasználója is szükséges. Más alkalmazásoknak rendszergazdai engedélyek, amelyek hello szervezetben a felhasználók nem járulhatnak hozzá. A címtár csak a rendszergazda biztosíthat hozzájárulási tooapplications, ez a jogosultsági szint szükséges. Hello felhasználói vagy rendszergazdai hozzájárul, ha mindkét hello webes API-k a könyvtárban van regisztrálva.

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
Hello első alkalmazás használja az engedélyezési kód tooget a JWT jogkivonat, amikor JWT frissítési jogkivonat is megkapja. Amikor hello hozzáférési jogkivonat lejár, a hello frissítési jogkivonat lehet-e a használt toore-hello felhasználói hitelesítés nélkül kéri a hitelesítő adatokat. A frissítési jogkivonat nem használt tooauthenticate hello felhasználói, mely eredményezi, hogy egy új hozzáférési tokent, és frissítse token.

## <a name="see-also"></a>Lásd még:
[Az Azure Active Directory fejlesztői útmutatója](active-directory-developers-guide.md)

[Az Azure Active Directory-Kódminták](active-directory-code-samples.md)

[Fontos információkat a kulcsváltás Azure AD-ben](active-directory-signing-key-rollover.md)

[OAuth 2.0 Azure AD-ben](https://msdn.microsoft.com/library/azure/dn645545.aspx)


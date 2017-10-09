---
title: "egy olyan alkalmazáshoz, minden Azure AD-felhasználó bejelentkezés aaaHow toobuild |} Microsoft Docs"
description: "Lépésről lépésre, amely egy alkalmazás létrehozásához szükséges utasításokat jelentkezhetnek be a felhasználó bármely Azure Active Directory-bérlőhöz, más néven egy több-bérlős alkalmazást."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 35af95cb-ced3-46ad-b01d-5d2f6fd064a3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 123ea8125fa3c308ce0f124cc58e85ec28d476d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosign-in-any-azure-active-directory-ad-user-using-hello-multi-tenant-application-pattern"></a>Hogyan bármely Azure Active Directory (AD) felhasználó használatával toosign hello több-bérlős alkalmazásminta
Ha egy szoftver, a szolgáltatás alkalmazás toomany szervezetek, konfigurálhatja az alkalmazás tooaccept bejelentkezések bármely Azure AD-bérlő.  Az Azure ad-ben ezt nevezik, így az alkalmazás több-bérlős.  Az Azure AD-bérlő felhasználók fognak tooyour alkalmazás után tudja toosign küldőnek toouse az alkalmazás a fiókját.  

Ha egy meglévő alkalmazást a saját fiók rendszer, vagy egyéb forgalomból adódó bejelentkezés más felhőalapú szolgáltatók támogatja, felvétele az Azure AD-bejelentkezés a panelhez használata egyszerű. Csak az alkalmazás regisztrálásához, adja hozzá az OAuth2, az OpenID Connect vagy a SAML-bejelentkezés kódot, és a "Sign In with Microsoft" gomb elhelyezése az alkalmazás. Kattintson a következő gombra toolearn további információk az alkalmazás branding hello.

[! [Gomb bejelentkezés] [Az AAD-bejelentkezés]][AAD-App-Branding]

Ez a cikk feltételezi, hogy már ismeri az Azure AD egy egybérlős alkalmazás felépítése.  Ha nem, a head mentésére toohello [fejlesztői útmutató kezdőlap] [ AAD-Dev-Guide] és tekintse meg a gyors üzembe helyezések!

Nincsenek négy egyszerű lépésben tooconvert az alkalmazás az Azure AD több-bérlős alkalmazásba:

1. Az alkalmazás regisztrálása toobe többvállalatos frissítése
2. Frissítse a kódot toosend kérelmek toohello/Common végpont 
3. Frissítse a kódot toohandle több kibocsátó érték
4. Felhasználó és rendszergazda jóváhagyását megértéséhez, valamint a megfelelő kódot módosításokat

Nézzük részletes lépéseit. Akkor is is ugorhat rögtön túl[ebben a listában több-bérlős minták][AAD-Samples-MT].

## <a name="update-registration-toobe-multi-tenant"></a>Frissítse a regisztrációs toobe több-bérlős
Alapértelmezés szerint web app/API regisztráció az Azure ad-ben található egybérlős.  Akkor is használhatja a regisztrációs több-bérlős keresése hello "több központjaként" kapcsoló hello tulajdonságok lapján az alkalmazáshoz való regisztráció hello [Azure-portálon] [ AZURE-portal] és beállítása a túl "Yes".

Vegye figyelembe azt is, előtt egy alkalmazás több-bérlős, Azure AD szükséges hello hello alkalmazás toobe globálisan egyedi App ID URI Azonosítóját. hello App ID URI egy alkalmazás, amelynél az protokoll üzeneteinek hello módszerek valamelyikével.  Egyetlen bérlői alkalmazások a rendszer megfelelő hello App ID URI toobe egyedi bérlőre belül.  Egy több-bérlős alkalmazáshoz kell legyen globálisan egyedi, az Azure AD alkalmazás hello található összes bérlők között.  Globális egyediségi kikényszeríti igénylő hello App ID URI toohave egy állomásnevet, amely megfelel az Azure AD-bérlő hello ellenőrzött tartományt.  Például, ha a bérlő neve hello contoso.onmicrosoft.com, majd egy érvényes App ID URI lenne `https://contoso.onmicrosoft.com/myapp`.  Ha a bérlő az ellenőrzött tartományt kellett `contoso.com`, majd egy érvényes App ID URI is használhatók lesznek `https://contoso.com/myapp`.  Az alkalmazás beállítását több-bérlős sikertelen lesz, ha hello App ID URI nem követi ezt a mintát.

Natív ügyfél alapértelmezés szerint több-bérlős hiányoznak.  Bármely művelet toomake egy natív ügyfél alkalmazás regisztrációs többvállalatos tootake nem szükséges.

## <a name="update-your-code-toosend-requests-toocommon"></a>A kód toosend kérelmek túl/közös frissítése
Egy bérlő egyetlen alkalmazást a bejelentkezési kérelmek toohello bérlői bejelentkezési végpont kell küldeni. Például a contoso.onmicrosoft.com hello végpont a következő lesz:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Kérelmek küldése tooa tenant végpont bármikor beléphet a felhasználók (vagy a vendégek), hogy a bérlő bérlői tooapplications.  Egy több-bérlős alkalmazással hello alkalmazás nem tud előre kérelmek tooa tenant végpont milyen bérlői hello felhasználói származik, ezért nem lehet elküldeni.  Ehelyett a kérések érkeznek, amely minden Azure AD bérlők között multiplexes tooan végpont:

    https://login.microsoftonline.com/common

Ha az Azure AD a hello kérést kap/közös végpont hello felhasználó jelentkezik be, és azt, a következménye hello bérlői felhasználói deríti fel a.  hello/közös végpont működik együtt az összes Azure AD által támogatott hello hitelesítési protokollok: OpenID Connect, a OAuth 2.0, a SAML 2.0 és a WS-Federation.

hello bejelentkezési válasz toohello alkalmazás majd hello felhasználói jelképező jogkivonatot tartalmazza.  hello kibocsátó érték hello jogkivonat közli az alkalmazás milyen bérlői hello felhasználó.  Ha választ ad vissza hello, vagy közös végpontot, hello kibocsátó érték hello jogkivonat toohello felhasználó bérlői felel meg.  Fontos toonote hello / közös végpont nem a bérlőt, és nincs kibocsátó, hogy csak a multiplexer.  / Common használata esetén az alkalmazás toovalidate jogkivonatokba hello logika kell frissíteni toobe tootake ez figyelembe. 

Korábban említettük, a több-bérlős alkalmazásokhoz is biztosítania kell egy egységes bejelentkezés során tapasztal élmény a felhasználók számára, mert a következő hello branding irányelvek Azure AD-alkalmazást. Kattintson a következő gombra toolearn további információk az alkalmazás branding hello.

[! [Gomb bejelentkezés] [Az AAD-bejelentkezés]][AAD-App-Branding]

Vessen egy pillantást hello/Common hello használata végpont és a kód végrehajtása részletesebben.

## <a name="update-your-code-toohandle-multiple-issuer-values"></a>Frissítse a kódot toohandle több kibocsátó érték
Webalkalmazások és webes API-k kap, és érvényesítse az Azure AD.  

> [!NOTE]
> Natív ügyfélalkalmazások kérése és jogkivonatokat fogadni az Azure AD, így toosend tehetik tooAPIs, ahol érvényesíti azokat.  Natív alkalmazások nem érvényesíthet jogkivonatokat és kell kezeli őket nem átlátszó.
> 
> 

Nézzük, hogyan ellenőrzi az alkalmazás a jogkivonatokat kap az Azure AD.  Egy bérlő egyetlen alkalmazás általában lépnek egy végpont értékét, például:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

és egy metaadatainak URL-CÍMÉT (ebben az esetben az OpenID Connect), például tooconstruct használhatja:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

a két toodownload fontos adatot, amelyek használt toovalidate: hello bérlői tartozó aláíró kulcsok és a kiállító érték.  Minden Azure AD-bérlő értéke egyedi kibocsátó hello űrlap:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

ahol hello GUID érték hello átnevezése szálbiztos változatával hello bérlői azonosító hello bérlő.  Ha hello megelőző metaadatok hivatkozásra kattintva `contoso.onmicrosoft.com`, láthatja, hogy a kibocsátó érték hello dokumentumban.

Amikor egy bérlő egyetlen alkalmazás érvényesíti a jogkivonatot, ellenőrzi, hello jogkivonat-aláírási kulcsokat hello metaadat-dokumentum hello elleni hello aláírását. Ez lehetővé teszi azt toomake meg arról, hogy hello kibocsátó érték hello token megfelel hello egyet, amely hello metaadat-dokumentum található.

Hello óta/közös végpont tooa bérlő nem felel meg, és nem egy kibocsátó, ha megvizsgálja az hello metaadatok hello kibocsátó érték/közös rendelkezik sablonalapú URL-cím helyett egy tényleges érték:

    https://sts.windows.net/{tenantid}/

Ezért egy több-bérlős alkalmazás nem érvényesíthet jogkivonatokat hello hello metaadatait hello kibocsátó értéke csak egyeztetve `issuer` hello token értékét.  Egy több-bérlős alkalmazást kell logika toodecide kibocsátó értékek érvényesek, és amelyeket nem szerint hello bérlői Állomásazonosító részét feloszthatja hello kibocsátó érték alapján.  

Például ha egy több-bérlős alkalmazás csak lehetővé teszi, hogy bejelentkezési adott bérlő, aki regisztrált-e a szolgáltatás, akkor azt kell ellenőrizze hello kibocsátó érték vagy hello `tid` jogcím értéke a hello token toomake meg arról, hogy a bérlő az tartalmazó előfizetők.  Ha csak egy több-bérlős alkalmazás egyének foglalkozik, és nem minden hozzáférés alapján döntéseket bérlők, majd azt figyelmen kívül hagyhatja hello kibocsátó érték regisztrálását.

A több-bérlős minták hello hello [kapcsolódó tartalom](#related-content) letiltva tooenable bármely Azure AD bérlő toosign a szakasz ebben a cikkben kibocsátó érvényesítése hello végén.

Most hello felhasználói élmény a felhasználók számára, amely toomulti bérlői alkalmazások bejelentkezés vizsgáljuk meg.

## <a name="understanding-user-and-admin-consent"></a>Understanding felhasználói és rendszergazdai hozzájárulási
Egy felhasználó toosign tooan alkalmazásokban az Azure ad-ben, a hello alkalmazás meg kell jelennie hello felhasználó bérlői.  Ez lehetővé teszi, hogy hello szervezet toodo dolog, például a bérlő a felhasználók bejelentkezésekor toohello alkalmazás egyedi házirendeket alkalmazhat.  Egyetlen bérlői alkalmazások a regisztráció az egyszerű; az rendelkezik egy, akkor fordul elő, amikor regisztrál hello alkalmazás hello hello [Azure-portálon][AZURE-portal].

Egy több-bérlős alkalmazáshoz hello kezdeti regisztrációs hello alkalmazás él, hello Azure AD-bérlő hello fejlesztői használják.  Amikor egy felhasználó egy másik bérlőhöz toohello alkalmazás hello első alkalommal bejelentkezik, az Azure AD megkéri hello alkalmazás által kért tooconsent toohello engedélyek.  Ha azok hozzájárulás, akkor egy hello alkalmazás ábrázolása nevű egy *egyszerű* jön létre az hello felhasználó bérlői, és a bejelentkezés. A delegálás hello felhasználói hozzájárulás toohello alkalmazás rögzíti hello címtárban is létrejön. Lásd: [alkalmazás és szolgáltatás egyszerű objektumok] [ AAD-App-SP-Objects] hello alkalmazás alkalmazás- és szolgáltatásnév objektumokat, és azok más tooeach leírását.

![Hozzájárulás toosingle szintű alkalmazás][Consent-Single-Tier] 

A hozzájárulási élmény hello alkalmazás által kért hello engedélyek van hatással.  Az Azure AD kétféle engedélyek, csak alkalmazást, és a delegált támogatja:

* Egy engedélyt biztosít egy alkalmazás hello képességét tooact, mint a bejelentkezett felhasználó hello dolgot hello felhasználói egy része számára is.  Például egy alkalmazás hello engedélyt tooread hello bejelentkezett felhasználó naptár megadásához.
* Egy alkalmazás csak az engedélyt közvetlenül hello alkalmazás toohello identitását.  Például egy alkalmazás hello csak app tooread hello felhasználók listáján egy bérlőn, függetlenül attól, akik toohello alkalmazás alá van írva megadásához.

Néhány lehet engedélyeket hozzájárult tooby normál felhasználói, míg más egy Bérlői rendszergazda jóváhagyását. 

### <a name="admin-consent"></a>Felügyeleti hozzájárulás
Csak alkalmazás engedélyek minden esetben szükséges egy Bérlői rendszergazda jóváhagyását.  Ha az alkalmazás az alkalmazás csak engedélyt kér, és egy felhasználó megpróbál toosign toohello alkalmazásban, egy hibaüzenet jelenik meg arról, hogy hello felhasználó nem tud tooconsent.

Bizonyos delegált jogosultságokkal sikeresen telepítették is szükség lehet egy Bérlői rendszergazda jóváhagyását.  Hello képességét toowrite hátsó tooAzure AD hello bejelentkezett felhasználó, például egy Bérlői rendszergazda jóváhagyását igényli.  Csak alkalmazáshoz engedéllyel, például ha egy szokásos felhasználó megpróbál toosign tooan alkalmazás, amelyet a rendszergazda jóváhagyását igénylő engedélyt igényel az alkalmazás egy hibaüzenetet fog kapni.  Engedélyt igényel-e a rendszergazda jóváhagyását hello fejlesztői hello erőforrás közzétett határozza meg, és lehet hello erőforrás hello dokumentációjában található.  Hivatkozásokat tartalmaz a leíró hello elérhető engedélyek a hello Azure AD Graph API és a Microsoft Graph API a következők hello tootopics [kapcsolódó tartalom](#related-content) című szakaszát.

Ha az alkalmazás rendszergazda jóváhagyását igénylő engedélyek, kell toohave a hitelesítési mód, például egy gombra való kattintást vagy hivatkozás, ahol Üdvözöljük a rendszergazdákat hello művelettel is kezdeményezhető.  az alkalmazás elküld, ez a művelet nem a szokásos OAuth2/OpenID Connect hitelesítési kérést, de a hello is tartalmazó hello kérelem `prompt=admin_consent` lekérdezési karakterlánc.  Üdvözöljük a rendszergazdákat hozzájárult, és hello szolgáltatás egyszerű hello ügyfél bérlő jön létre, ha új bejelentkezési kérelmek nem kell hello `prompt=admin_consent` paraméter. Hello rendszergazda úgy dönt, hello kért engedélyeket elfogadhatók, mivel nincs más hello bérlői felhasználói bekéri a hozzájárulási ettől kezdve.

Hello `prompt=admin_consent` paraméter is használható az engedélyeket, nincs szükség a rendszergazda jóváhagyását igénylő alkalmazásokkal. Ez történik, amikor a hello alkalmazástelepítések szükséges élményt ahol hello Bérlői rendszergazda "előfizet" egy időben, és nincs más felhasználók felszólítást kapnak hozzájárulási ettől a.

Ha egy alkalmazás megkövetel egy rendszergazda jóváhagyását, és egy rendszergazda jelentkezik, de hello `prompt=admin_consent` paraméter nem küldi el, Üdvözöljük a rendszergazdákat sikeresen fog hozzájárulás toohello alkalmazás **csak a hozzájuk tartozó felhasználói fiókok**.  Rendszeres felhasználók nem marad a képes toosign és hozzájárulási toohello alkalmazás.  Ez akkor hasznos, ha azt szeretné, toogive hello Bérlői rendszergazda hello képességét tooexplore az alkalmazást, mielőtt más felhasználók hozzáférést.

A bérlői rendszergazda letilthatja a normál felhasználók tooconsent tooapplications hello képességét.  Ha ez a funkció le van tiltva, admin hozzájárulási mindig szükség hello alkalmazás toobe hello bérlői beállítása.  Ha azt szeretné, hogy az alkalmazás normál felhasználói hozzájárulásával letiltva tootest, hello konfigurációs kapcsolót az található hello Azure AD bérlő hello konfigurációs szakasza [Azure-portálon][AZURE-portal].

> [!NOTE]
> Egyes alkalmazások szeretné, hogy ha rendszeres felhasználók képes tooconsent kezdetben, és későbbi hello alkalmazás magába foglaló hello rendszergazda és a rendszergazda jóváhagyását igénylő engedélyeket élményt.  Nincs semmilyen módon toodo Ez egy egyetlen alkalmazás regisztrálása az Azure AD ma.  hello jövőbeli az Azure AD v2 végpont lehetővé teszi alkalmazások toorequest engedélyek futásidőben, ahelyett, hogy regisztrációs időpontban, amely lehetővé teszi, hogy ebben a forgatókönyvben.  További információkért lásd: hello [Azure AD alkalmazás-modell v2 – útmutató fejlesztőknek][AAD-V2-Dev-Guide].
> 
> 

### <a name="consent-and-multi-tier-applications"></a>Hozzájárulás és a többrétegű alkalmazások
Előfordulhat, hogy az alkalmazás több rétegek, a saját regisztrációs által képviselt minden Azure AD-ben.  Például egy webes API-t egy natív alkalmazás, vagy egy webes alkalmazás, amely meghívja a webes API-k.  Mindkét esetben a hello (a natív alkalmazással vagy a web app) ügyfélszámítógép engedélyek toocall hello erőforrás (webes API-k).  Hello ügyfél toobe sikeresen átadni kívánt hozzájárult e az ügyfél bérlői, az összes erőforrások toowhich kér engedélyeket már léteznie kell hello az ügyfél-bérlőben.  Ha ez a feltétel nem teljesül, az Azure AD vissza, amely erőforrás hello hiba először hozzá kell adni.

**Egy bérlő több rétegből**

Ez problémát okozhat, ha a logikai alkalmazás két vagy több alkalmazás regisztrációk, például egy külön ügyfél- és erőforrás áll.  Hogyan kapunk hello erőforrás hello ügyfél bérlői történő első?  Az Azure AD ebben az esetben ismerteti ügyfél engedélyezésével, és erőforrás-toobe átadni kívánt hozzájárult e egyetlen lépésben. hello felhasználónál hello ügyfél- és erőforrás hello hozzájárulási oldalon által kért hello engedélyek hello összessége.  tooenable ezt a viselkedést hello erőforrás-alkalmazás regisztrációja tartalmaznia kell a hello ügyfél alkalmazás azonosítója egy `knownClientApplications` az alkalmazásjegyzékben.  Példa:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Ez a tulajdonság hello erőforrás keresztül frissíthető [alkalmazás jegyzékfájlja][AAD-App-Manifest]. Ezt mutatják be egy natív ügyfél többrétegű, webes API-mintát hívása hello [kapcsolódó tartalom](#related-content) szakasz ebben a cikkben hello végén. hello következő ábra áttekintést nyújt hozzájárulási egy többrétegű alkalmazást, regisztrálni az egyetlen bérlő számára:

![Hozzájárulás toomulti szintű ismert ügyfélalkalmazás][Consent-Multi-Tier-Known-Client] 

**A több bérlő több rétegből**

Hasonló esetek történik, ha a különböző bérlők regisztrált hello különböző rétegek, egy alkalmazás.  Vegye figyelembe például egy natív ügyfélalkalmazást, amely behívja az Office 365 Exchange Online API hello kialakításának hello eset.  toodevelop hello natív alkalmazás, és az ügyfél-bérlőben hello natív alkalmazás toorun később, hello Exchange Online szolgáltatás egyszerű jelen kell lennie.  Ebben az esetben hello fejlesztői és az ügyfelek kell vásárolnia, Exchange Online hello szolgáltatás egyszerű toobe létrehozott bérlők számára.  

Az API-k által egy szervezet, nem a Microsoft hello esetben hello fejlesztői hello API tooprovide úgy azok az ügyfelek tooconsent hello alkalmazást az ügyfelek bérlők szüksége van. hello ajánlott tervezési hello 3. fél fejlesztői toobuild hello API számára úgy, hogy is működhetnek egy webes ügyfél tooimplement előfizetési:

1. Hajtsa végre a hello korábbi szakaszokban tooensure hello API megvalósítja hello több-bérlős regisztrációs/kód alkalmazáskövetelmények
2. Továbbá tooexposing hello API hatókörök/szerepkörök, győződjön meg arról, hello regisztrációs tartalmaz hello "Bejelentkezés és felhasználói profil olvasása" az Azure AD engedélyt (alapértelmezés szerint)
3. Bejelentkezési-a/regisztrációs oldal megvalósítását a hello webes ügyfél, a következő hello [admin hozzájárulási](#admin-consent) útmutatást taglaltak 
4. Miután hello felhasználó toohello alkalmazás hozzájárul, hello szolgáltatás egyszerű és beleegyezése delegálás hivatkozásokat hoz létre a bérlői, és hello natív alkalmazás tud jogkivonatokhoz hello API

a következő diagram hello hozzájárulási áttekintést nyújt a különböző bérlők regisztrált többrétegű alkalmazást:

![Több résztvevős toomulti szintű app hozzájárulás][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Hozzájárulás visszavonása
Felhasználók és rendszergazdák vissza tudja vonni a hozzájárulási tooyour alkalmazásból bármikor:

* Felhasználók revoke access tooindividual alkalmazások ehhez távolítsa el a saját [Panel alkalmazásokat] [ AAD-Access-Panel] listája.
* A rendszergazdák hozzáférés tooapplications visszavonása eltávolítja azokat az Azure AD hello Azure AD felügyeleti szakaszának hello segítségével [Azure-portálon][AZURE-portal].

Ha egy rendszergazda tooan alkalmazás a bérlőkben lévő összes felhasználó hozzájárul, felhasználók külön-külön hozzáférési nem visszavonása.  Csak hello rendszergazda visszavonhatja a hozzáférést, és csak a teljes alkalmazás hello.

### <a name="consent-and-protocol-support"></a>Hozzájárulás és protokollok támogatása
Hozzájárulás támogatott keresztül hello OAuth, OpenID Connect, az Azure AD-ben és a WS-Federation, SAML protokoll.  hello SAML és a WS-Federation protokollok nem támogatják a hello `prompt=admin_consent` paraméter, így a rendszergazda jóváhagyását csak az OAuth és az OpenID Connect keresztül lehetséges.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Több-Bérlős alkalmazásokhoz és a hozzáférési jogkivonatok gyorsítótár
Több-bérlős alkalmazásokhoz is kérheti le a hozzáférési jogkivonatok toocall API-k, az Azure AD által védett.  Általános hiba a több-bérlős alkalmazással hello Active Directory Authentication Library (ADAL) használatakor tooinitially kérelem egy felhasználó/Common használatával jogkivonatot, kapott választ, akkor kérjen az, hogy a felhasználók is használja a/Common későbbi tokent.  Mivel az Azure AD hello válasz nem származik a bérlő vagy közös, ADAL gyorsítótárazza a hello token hello bérlői származik. hello későbbi hívható meg olyan hozzáférési jogkivonatot hello felhasználói gyorsítótárbeli sikertelen keresések hello gyorsítótári bejegyzést, és hello felhasználó nem a kért toosign ismét túl/közös tooget.  Hiányzó hello gyorsítótár tooavoid, győződjön meg arról, hogy további hívások már bejelentkezett felhasználók toohello tenant végpont történik.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan meg toobuild olyan alkalmazás, amely a felhasználó bármely Azure Active Directory-bérlőhöz tud bejelentkezni. Miután engedélyezte az egyszeri bejelentkezés az alkalmazás és az Azure Active Directory között, az alkalmazás tooaccess Microsoft erőforrások, például az Office 365 által közzétett API-kban is frissítheti. Ezért kínálhat testreszabott élmént az alkalmazás például toohello felhasználók, például a profilkép vagy a következő naptári időpontot egyeztessen az környezetfüggő adatainak megjelenítése. További információ az API így toolearn meghívja tooAzure Active Directory, és Office 365-szolgáltatásokhoz, például az Exchange, SharePoint, a OneDrive, a OneNote, Planner, Excel és további, látogasson el: [Microsoft Graph API][MSFT-Graph-overview].


## <a name="related-content"></a>Kapcsolódó tartalom
* [Több-bérlős alkalmazás minták][AAD-Samples-MT]
* [Az alkalmazások arculati útmutatóját][AAD-App-Branding]
* [Az Azure AD fejlesztői útmutató][AAD-Dev-Guide]
* [Alkalmazás és szolgáltatás egyszerű objektumok][AAD-App-SP-Objects]
* [Alkalmazások integrálása az Azure Active Directoryval][AAD-Integrating-Apps]
* [Hello hozzájárulás keretrendszer áttekintése][AAD-Consent-Overview]
* [A Microsoft Graph API-Engedélyhatókörök][MSFT-Graph-permision-scopes]
* [Az Azure AD Graph API-Engedélyhatókörök][AAD-Graph-Perm-Scopes]

A következő megjegyzések szakasz tooprovide visszajelzés hello használata, és segítsen pontosítsa és a tartalom.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-portal]: https://portal.azure.com
[MSFT-Graph-overview]: https://graph.microsoft.io/en-us/docs/overview/overview
[MSFT-Graph-permision-scopes]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ../active-directory-appmodel-v2-overview.md
[AZURE-portal]: https://portal.azure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken















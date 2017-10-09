---
title: "CENC Multi-DRM-és hozzáférés-vezérlés: egy hivatkozás tervezési és megvalósítási Azure és az Azure Media Services |} Microsoft Docs"
description: "További információk a hogyan toolicensing hello Microsoft® Smooth Streaming ügyfél eljárás készlet."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 7814739b-cea9-4b9b-8370-538702e5c615
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;kilroyh;yanmf;juliako
ms.openlocfilehash: 033fb618650c4fbe9069d467159a8734da759bba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>CENC többplatformos DRM és hozzáférés-vezérlés használatával: Egy referenciaterv és megvalósítás az Azure-on és az Azure Media Services szolgáltatásban
 
## <a name="introduction"></a>Bevezetés
Fontos, hogy egy összetett feladat toodesign és az OTT DRM alrendszer építése jól ismert vagy online streamelési megoldások. És egy közös operátorok/online videó szolgáltatók toooutsource ezen része toospecialized DRM szolgáltatók gyakorlatot. hello a jelen dokumentum célja toopresent egy referencia-tervezési és megvalósítási végpont DRM alrendszer OTT vagy online adatfolyam megoldás.

a jelen dokumentum megcélzott hello olvasók működik-e OTT online streaming/több-screen megoldások és bármely DRM alrendszer iránt érdeklődik olvasók DRM alrendszer a mérnökök. hello feltételezése, hogy olvasók jártas hello DRM technológiák hello piacon, például a PlayReady, Widevine, FairPlay vagy Adobe hozzáférés legalább egyikét.

DRM is magában foglalja az CENC (Common Encryption) és multi-DRM. Online streamelésre és az OTT iparági fő változása érték toouse CENC a több-native-DRM különböző a ügyfélplatformokon, amely egy előző trend hello egyetlen DRM és az ügyfél SDK használatának a ügyfélplatformokon különböző a shift. Ha több native DRM CENC használ, mind a PlayReady, mind a Widevine titkosítása hello [Common Encryption (ISO/IEC 23001-7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) megadását.

hello és multi-DRM CENC előnyei a következők:

1. Csökkenti a költségeket, mert egy egyetlen titkosítási feldolgozási célzó különböző platformokon a natív DRMs; a használt titkosítási
2. Csökkenti a titkosított eszközök kezelésére, mert csak egy példányát a titkosított eszközökre van szükség; hello költség
3. Megszünteti a licencelési költségek, mivel hello natív DRM ügyfél általában a natív platformon szabad DRM-ügyfél.

Microsoft lett egy aktív hatnak DASH és CENC néhány fő iparági lejátszó együtt. A Microsoft Azure Media Services DASH és CENC támogatásának biztosítása rendelkezik lett. Friss közlemények talál Mingfei tartozó blogok: [bejelentése Google Widevine licenctovábbítási szolgáltatások az Azure Media Services](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/), és [Azure Media Services hozzáadja a Google Widevine-csomagolás multi-DRM adatfolyam kézbesítéséhez](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Ez a cikk áttekintése
Ez a cikk célja hello hello következőket tartalmazza:

1. Biztosít egy referencia-Tervező DRM alrendszer CENC használata multi-DRM;
2. Ez a témakör egy referencia-megvalósításban a Microsoft Azure vagy az Azure Media Services platform;
3. Néhány tervezési és megvalósítási témaköre ismerteti.

Hello cikkben "multi-DRM" hello következő ismerteti:

1. Microsoft PlayReady
2. Google Widevine
3. Apple FairPlay 

hello következő táblázat összefoglalja hello natív platform/natív alkalmazás, és minden DRM által támogatott böngészők.

| **Ügyfélplatform** | **Natív DRM-támogatás** | **Böngészőalkalmazás /** | **Formátumban** |
| --- | --- | --- | --- |
| **Intelligens televízió, operátor vezérléséhez, OTT vezérléséhez** |PlayReady, és/vagy Widevine, és/vagy más |Linux, az Opera, WebKit, más |Különböző formátumokba |
| **Windows 10 rendszerű eszközökhöz (Windows-Számítógépet, a Windows rendszerű táblagépek, Windows Phone, Xbox)** |PlayReady |Peremhálózati/IE11/EME MS<br/><br/><br/>UWP |KÖTŐJEL (a HLS, PlayReady nem támogatott)<br/><br/>DASH, Smooth Streaming (HLS a, a PlayReady nem támogatott) |
| **Android-eszközök (telefon, táblagép, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **(iPhone-, iPad-) iOS, OS X-ügyfelek és az Apple TV** |FairPlay |Safari 8 / EME |HLS |


Hello aktuális állapota minden egyes DRM-központitelepítési, figyelembe véve a szolgáltatás általában akkor érdemes tooimplement 2 vagy 3 DRMs toomake meg arról, hogy meg hello ajánlott végpontok összes hello típusú cím módon.

Egy kompromisszumot hello szolgáltatás logika hello összetettsége és között különböző ügyfelek hello tapasztalattal a hello ügyfél oldalán tooreach felhasználói bizonyos szintű hello összetettségét.

toomake a kijelölés, ne mind a tények:

* PlayReady natív módon valósul meg minden egyes Android-eszközökön, és a szoftver SDK-k szinte bármilyen platformon keresztül elérhető Windows-eszköz
* Minden Android-eszköz, a Chrome-ban, és néhány egyéb eszközök Widevine natív módon történik.
* FairPlay csak iOS és Mac OS ügyfelek vagy iTunes érhető el.

Egy tipikus multi-DRM így néz ki:

* 1. lehetőség: PlayReady és Widevine
* 2. lehetőség: A PlayReady, Widevine és FairPlay

## <a name="a-reference-design"></a>A referencia-Tervező
Ebben a szakaszban a jelenleg használt független tootechnologies tooimplement egyben hivatkozás kialakítást jelent azt.

Egy DRM alrendszer tartalmazhatja a következő összetevők hello:

1. Kulcskezelés
2. DRM-csomag
3. DRM-licenckézbesítés
4. Jogosultság ellenőrzése
5. Hitelesítési/engedélyezési
6. Player
7. Forrás/CDN

hello következő diagram azt ábrázolja hello magas szintű interakció egy DRM alrendszerben hello összetevők közötti.

![CENC DRM alrendszer](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Az alábbi három alapvető "réteg" hello kialakításában:

1. Irodai réteghez, amely nem érhetők el külsőleg; (a fekete)
2. "DMZ" réteg (kék) tartalmazó összes hello végpontok irányuló nyilvános;
3. Nyilvános Internet (könnyű kék) tartalmazó réteget CDN és a forgalom lejátszó nyilvános interneten keresztül.

A Tartalomkezelés eszköz DRM-védelem szabályozása kell lennie, függetlenül attól statikus vagy dinamikus titkosítást. hello bemenetek DRM titkosításhoz kell tartalmaznia:

1. MBR videotartalom;
2. Tartalomkulcs;
3. Licenc-licenckérési URL-címeket.

A lejátszás időszakban hello magas szintű folyamata a következő:

1. Felhasználó hitelesítése;
2. Engedélyezési jogkivonat létrejön hello felhasználói;
3. DRM-védelemmel (jegyzékfájl) tartalma letöltött tooplayer;
4. Player elküldi a beszerzési kérelem toolicense licenckiszolgálókat kulcs azonosítója és engedélyezési jogkivonat.

Áthelyezése toohello következő téma előtt hello kapcsolatos néhány szót kulcskezelés tervezhet.

| **ContentKey – eszköz** | **A forgatókönyv** |
| --- | --- |
| 1 – 1 |hello legegyszerűbb eset. Hello Legfinomabb felügyeletét biztosítja azt. Azonban ez általában annak az eredménye hello legmagasabb licenc kézbesítési költséggel jár. A minimális egy-egy licencet kérelme, mert minden védett eszköz szükséges. |
| 1 – a-többhöz |Hello ugyanaz a tartalom a kulcs több eszköz használható. Például az összes hello eszköz a logikai csoport például a genre vagy genre (vagy Movie gén) részhalmazát, használhat egy tartalomkulcsot. |
| Több-a-1 |Több tartalomkulcs minden eszköz szükséges. <br/><br/>Például ha tooapply dinamikus CENC-védelem és multi-DRM MPEG-DASH vagy HLS dinamikus AES-128 titkosítási, kell két külön tartalomkulcs, külön-külön ContentKeyType. (Hello tartalomkulcsot a dinamikus CENC védelemhez használt, a ContentKeyType.CommonEncryption kell használni, a tartalom dinamikus AES-128 titkosítást, ContentKeyType.EnvelopeEncryption használt kulcs használandó hello.)<br/><br/>Egy másik példában DASH CENC védelméről elméletileg tartalom, lehet, hogy egy tartalomkulcsot használt tooprotect video-adatfolyamot és egy másik tartalom kulcs tooprotect hangadatfolyam. |
| Sok – túl – több |Két esetben fent hello kombinációja: hello hello az adategységek azonos eszköz "csoport" kulcsot használnak az egyes tartalom egy készletét. |

Egy másik fontos tényező tooconsider állandó, és nem állandó licencek hello felhasználása.

Miért fontosak ezeket a szempontokat?

Közvetlen hatása toolicense kézbesítési költségeket, ha nyilvános felhő licenc kézbesítési rendelkeznek. Mérlegeljük, a következő két különböző kialakítási esetekben tooillustrate hello:

1. Havi előfizetést: állandó licenccel, illetve 1-a-többhöz tartalom kulcs eszköz leképezés. Például az összes hello gyermekek filmek egy egy titkosítási tartalomkulcsot használjuk. Ebben az esetben:

    Minden gyermekek filmek vagy eszköz kért # licencek teljes száma = 1
2. Havi előfizetést: nem állandó licenccel, illetve 1-1 leképezési tartalomkulcsot és eszköz közötti. Ebben az esetben:

    Minden gyermekek filmek vagy eszköz kért # licencek teljes száma = [# filmek figyelt] x [# munkamenetek]

Mint könnyen látható, két különböző kialakításokról eredményez nagyon különböző licenc hello elküldésére minták ezért licenc költségeket, ha a nyilvános felhők, például az Azure Media Services licenctovábbítási szolgáltatása biztosítja.

## <a name="mapping-design-tootechnology-for-implementation"></a>Leképezési tervezési tootechnology végrehajtásához
A következő azt képezze le a Microsoft Azure vagy az Azure Media Services platform, az általános tervezési tootechnologies mely technológia toouse minden építőelem a megadásával.

hello következő táblázatban hello leképezés:

| **Építőelem** | **Technológia** |
| --- | --- |
| **Player** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Az identitásszolgáltató (IDP)** |Azure Active Directory |
| **Biztonságos biztonságijogkivonat-szolgáltatás (STS)** |Azure Active Directory |
| **DRM Protection munkafolyamata** |Azure Media Services dinamikus védelme |
| **DRM licenc kézbesítés** |1. Az Azure Media Services-licenc kézbesítési (PlayReady, Widevine, FairPlay), vagy <br/>2. Axinom licenckiszolgáló, <br/>3. Egyéni PlayReady licenckiszolgáló |
| **Forrás** |Az Azure Media Services-Streamvégpont |
| **Kulcskezelés** |A hivatkozás végrehajtása esetén nem szükséges |
| **Tartalomkezelés** |A C# Konzolalkalmazás |

Identity Provider (IDP) és a Secure Token Service (STS) más szóval lesz az Azure AD. Player, használjuk [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). Az Azure Media Services és Azure Media Player támogatják a KÖTŐJELET és CENC multi-DRM.

a következő ábrán látható hello hello felépítéséről a fenti technológia leképezési hello.

![Az AMS CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Hello tartalomkezelési eszköz rendelés tooset dinamikus CENC titkosítási fel, a következő bemenet hello fog használni:

1. Nyissa meg a tartalom;
2. A következő kulcsból: tartalomkulcsot generációs/felügyeleti;
3. Licenc-licenckérési URL-címek;
4. Azure ad-információk listáját.

hello tartalomkezelési eszköz hello kimenete a következő lesz:

1. Hogyan licenc kézbesítési ellenőrzi a JWT jogkivonat és DRM licenc specifikációk; hello megadását tartalmazó ContentKeyAuthorizationPolicy
2. A AssetDeliveryPolicy tartalmazó beállításokat a streaming formátumban, DRM-védelem és licenc-licenckérési URL-címeket.

Futásidőben, hello folyamata van, mint alatt:

1. Felhasználók hitelesítése után a JWT jogkivonat jön létre;
2. Hello JWT jogkivonat lévő hello jogcímek egyik tartalmazó objektum Csoportazonosító hello "EntitledUserGroup" a "csoport" jogcímet. Ezt az igényt az "jogosultság ellenőrzése" továbbításához használható.
3. Player letöltések ügyfél jegyzékfájlt egy CENC a védett tartalmakat, és a "látja" hello következő:

   1. kulcs azonosítója,
   2. hello tartalma védve, CENC
   3. Licenc-licenckérési URL-címeket.
4. Player hello böngésző/DRM támogatott alapján licenc beszerzési kérelmet küld. Hello licenc beszerzési kérelem, kulcs azonosítója és hello JWT jogkivonat fog is küldhető el. Licenctovábbítási szolgáltatása ellenőrzi, hogy hello JWT jogkivonat és hello jogcímek tartalmazott, mielőtt a kiállító hello licenc szükséges.

## <a name="implementation"></a>Megvalósítás
### <a name="implementation-procedures"></a>Eljárások végrehajtása
hello megvalósítási lépések hello tartalmazza:

1. Készítse elő a teszt eszköz(ök): a vizsgálati videó toomulti sávszélességű kódolására, csomag töredékes MP4 Azure Media Services. Ez az eszköz nincs DRM-védelemmel. DRM-védelem később dinamikus védelmi végezhető el.
2. Kulcs azonosítója és a tartalom létrehozása (nem kötelező a kulcs kezdőérték) kulcs. A célra kulcskezelés rendszer nincs szükség, mert jelenleg csak egyetlen halmazába foglalkoznak azonosítója és a vizsgálati eszközök; több tartalomkulcs
3. AMS API tooconfigure multi-DRM-licenctovábbítási szolgáltatások hello vizsgálati eszköz használható. A vállalat vagy a vállalat forgalmazók Azure Media Services szolgáltatások helyett egyéni licenc kiszolgálókat használ, ha kihagyja ezt a lépést, és adja meg a licenc-licenckérési URL-címek konfigurálásához licenc kézbesítési hello lépésben. AMS API néhány részletes szükséges toospecify konfigurációkat, például az engedélyezési házirend-korlátozás licenc válasz sablonok DRM-licenc különböző szolgáltatások, stb. Jelenleg a hello Azure-portálon nem még meg hello ehhez a konfigurációhoz szükséges a felhasználói felület. API-szintű adatainak megkeresése és a mintakódot Ágnes Kornich dokumentumban: [segítségével PlayReady és/vagy Widevine Dynamic Common Encryption](media-services-protect-with-drm.md).
4. Használja az AMS API tooconfigure objektumtovábbítási szabályzat hello vizsgálati eszköz. API-szintű adatainak megkeresése és a mintakódot Ágnes Kornich dokumentumban: [segítségével PlayReady és/vagy Widevine Dynamic Common Encryption](media-services-protect-with-drm.md).
5. Hozza létre és konfigurálja az Azure Active Directory-bérlő az Azure rendszerben;
6. Néhány felhasználói fiókok és csoportok létrehozása az Azure Active Directory-bérlőhöz: legalább hozzon létre "EntitledUser" csoportot és toothis felhasználói csoport hozzáadása. A csoportban található felhasználók fogja továbbítani jogosultsági ellenőrzés licenc a beszerzési és nem a csoportban található felhasználók toopass hitelesítési ellenőrzés sikertelen lesz, és nem lesz képes tooacquire licenc. A "EntitledUser" csoport tagjaként a szükséges "csoportok" jogcím hello JWT jogkivonat Azure ad ki. Ez a követelmény jogcím multi-DRM licenc delivery services lépés konfigurálása kell megadni.
7. Hozzon létre egy ASP.NET MVC alkalmazást, amely a videólejátszó szolgáltató. Az ASP.NET alkalmazás felhasználóhitelesítéssel hello Azure Active Directory-bérlő elleni védelemmel látja el. Megfelelő jogcímeket fognak szerepelni felhasználók hitelesítése után hello hozzáférési jogkivonatok. Ez a lépés ajánlott OpenID Connect API. A következő NuGet-csomagok tooinstall hello lesz szüksége:
   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Install-Package Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
8. Hozzon létre egy player használt [Azure Media Player API](http://amp.azure.net/libs/amp/latest/docs/). [Az Azure Media Player ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) lehetővé teszi a toospecify mely DRM technológia toouse különböző DRM-platformon.
9. Teszt mátrix:

| **DRM** | **Böngésző** | **Jogosult felhasználó eredménye** | **Nem jogosult felhasználó eredménye** |
| --- | --- | --- | --- |
| **PlayReady** |MS peremhálózati vagy IE11 Windows 10 rendszeren |Sikeres |Sikertelen |
| **Widevine** |A Windows 10 Chrome |Sikeres |Sikertelen |
| **FairPlay** |TBD | | |

Az Azure Media Services csapatától George Trifonov írás biztosít az ASP.NET MVC player alkalmazásoknak az Azure Active Directory beállításának részletes lépéseket blog: [integrálni Azure Media Services OWIN MVC alkalmazás Azure Active Directory-alapú, és korlátozhatja a JWT jogcímei alapján kulcs tartalomkézbesítési](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George is írás blog a [JWT jogkivonat hitelesítés az Azure Media Services és a dinamikus titkosítás](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/). És itt a [mintát a Azure AD integrálása Azure Media Services kulcs kézbesítésével](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Információk az Azure Active Directoryban:

* A fejlesztői információt [Azure Active Directory fejlesztői útmutatója](../active-directory/active-directory-developers-guide.md).
* A rendszergazda információt [felügyelheti az Azure AD-címtár](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Néhány gotchas végrehajtása
Nincsenek egyes "gotchas" hello megvalósításában. Remélhetőleg "gotchas" listája a következő hello segítségével hibaelhárítási abban az esetben, ha problémákat tapasztal.

1. **Kibocsátó** URL-címet kell végződnie **"/"**.  

    **A célközönség** hello player alkalmazás ügyfél-azonosító és is fel kell **"/"** hello végén lévő hello kiállítójának URL-címe.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    A [JWT dekóder](http://jwt.calebb.net/), megtekintheti az **és** és **iss** hello JWT jogkivonat az alábbiak szerint:

    ![1. gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)
2. Engedélyek toohello alkalmazás felvétele az aad-ben (a konfigurálás lapon hello alkalmazás). Ez azért szükséges, az egyes alkalmazásokhoz (helyi és a telepített verzió).

    ![2. gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)
3. Hello helyes kibocsátói dinamikus CENC védelem beállításakor használja:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    hello következők nem fognak működni:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    hello GUID az hello AAD bérlő azonosítója. hello GUID Azure-portálon található végpontok előugró.
4. Támogatás csoporttagság jogosultságokat jogcímek. Ellenőrizze, hogy az aad-ben Alkalmazásjegyzék-fájl, hello következő tudunk

    "groupMembershipClaims": "All", (hello alapértelmezett értéke null)
5. A beállítás megfelelő TokenType korlátozás követelmények létrehozásakor.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Mivel hozzáadását támogatja a jwt-t (AAD) továbbá tooSWT (ACS), hello TokenType alapérték TokenType.JWT. Ha SWT/ACS használ, meg kell adni a tooTokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Kiegészítő témakörök az végrehajtása
Ezután azt fogja lemezre uss néhány kiegészítő témakörök a tervezési és megvalósítási.

### <a name="http-or-https"></a>HTTP vagy HTTPS?
hello azt beépített ASP.NET MVC lejátszóalkalmazás támogatnia kell a következő hello:

1. Az Azure AD, amely a HTTPS; toobe szükséges felhasználói hitelesítést
2. JWT jogkivonat exchange ügyfél és az Azure AD, amelyekre szüksége van a HTTPS; toobe között
3. DRM-licenc beszerzési hello ügyfél, amely a HTTPS szükséges toobe Ha licenc kézbesítési Azure Media Services biztosítja. Természetesen PlayReady termékcsomag határozza meg, hogy HTTPS licenc kézbesítésre. Ha a PlayReady licenckiszolgáló Azure Media Services kívül esik, HTTP vagy HTTPS használható.

Ezért az ASP.NET lejátszóalkalmazás hello fogja használni az HTTPS az ajánlott eljárás. Ez azt jelenti, hogy a hello Azure Media Player a HTTPS oldalon lesz. Azonban az adatfolyamként történő azt részesítsék előnyben a HTTP, ezért ellenőriznünk kell a vegyes tartalom probléma tooconsider.

1. Böngésző vegyes tartalom nem teszi lehetővé. De a beépülő modulok, például a Silverlight- és OSMF beépülő modul a smooth, valamint a kötőjel. Vegyes tartalom kapcsolatos egyik biztonsági szempont - ezt toohello fenyegetés miatt a hello képességét tooinject rosszindulatú JS, ami okozhat a felhasználói adatok toobe veszélyben hello.  Böngészők blokkolják a ez alapértelmezés szerint, és eddig hello csak úgy toowork köré hello (forrás) kiszolgáló oldalán, tooallow minden olyan tartománynál, (függetlenül attól, hogy https vagy http). Ez az valószínűleg nem jó ötlet vagy.
2. Azt a vegyes tartalom kerülendő: mindkettő használja a HTTP vagy HTTPS használatára is. Vegyes tartalom lejátszását, amikor a silverlightSS technológia szükséges, vegyes tartalom figyelmeztetés törlése. flashSS műszaki vegyes tartalom vegyes tartalom figyelmeztetés nélkül kezeli.
3. Ha a streamvégpontján augusztus 2014 előtt készült, nem fogja támogatni HTTPS. Ebben az esetben hozzon létre és használjon egy új streamvégpont HTTPS-hez.

Hello hivatkozás megvalósításban DRM védett tartalmakat, az alkalmazás- és adatfolyamként történő továbbítását fogja tudni HTTTPS. Tartalmának megnyitása hello player nem kell hitelesítést vagy licenc, így hello liberty toouse vagy HTTP vagy HTTPS.

### <a name="azure-active-directory-signing-key-rollover"></a>Az Azure Active Directory aláírási kulcs váltása
Ez az egy fontos pont tootake a megvalósítás figyelembe. Ha nem ez a megvalósítás a, befejeződött hello rendszer végül leállnak teljesen legfeljebb 6 héten belül.

Az Azure AD által használt iparági szabványos tooestablish megbízhatósági és alkalmazások az Azure AD között. Pontosabban az Azure AD nyilvános és titkos kulcsból álló kulcspárt tartalmazó aláíró kulcsot használ. Az Azure AD létrehoz egy biztonsági jogkivonatot, amely hello felhasználói információkat tartalmaz, ez a jogkivonat aláírását Azure ad használatával a titkos kulcsot, hátsó toohello alkalmazás elküldés előtt. amely a hello token tooverify érvényes és az Azure AD ténylegesen kezdeményezésű, hello alkalmazásnak ellenőriznie kell a nyilvános kulcsával. hello hello bérlői összevonási metaadat-dokumentum található, az Azure AD által elérhetővé tett hello jogkivonat aláírása. A nyilvános kulcs – hello aláírókulcsának, amelyből származik – pedig hello azonos használttól egyetlen bérlő számára az Azure ad-ben.

Részletes adatait az Azure AD kulcsváltás hello dokumentumban található: [aláírási kulcs átfordulási az Azure AD vonatkozó fontos információk](../active-directory/active-directory-signing-key-rollover.md).

Hello közötti [nyilvános-titkos kulcsból álló kulcspárt](https://login.microsoftonline.com/common/discovery/keys/),

* hello titkos kulcs használata esetén által az Azure Active Directory toogenerate a JWT jogkivonat;
* hello nyilvános kulcsot használja egy alkalmazás, például DRM-Licenctovábbítási szolgáltatások AMS tooverify hello JWT jogkivonat;

Biztonsági célra az Azure Active Directory forog ezt a tanúsítványt rendszeres időközönként (6 hét). Biztonsági problémák esetén hello kulcsváltás akkor fordulhat elő, amikor. Ezért hello licenctovábbítási szolgáltatások az AMS kell tooupdate hello nyilvános kulcsot használják az Azure AD forog hello kulcspár, ellenkező esetben az AMS tokent használó hitelesítés sikertelen lesz, és engedélyezve lesznek kiállítva.

Ez TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument úgy, hogy DRM-licenctovábbítási szolgáltatások konfigurálása esetén érhető el.

hello JWT jogkivonat-folyamata van, mint alatt:

1. Az Azure AD állít ki hello JWT jogkivonat hello aktuális titkos kulccsal, a hitelesített felhasználók;
2. Ha egy player egy CENC tartalmú multi-DRM-védelemmel látja, először megkeresi az Azure AD által kibocsátott hello JWT jogkivonat.
3. hello player hello JWT jogkivonat toolicense kézbesítési szolgáltatásokkal a licenc-beszerzési kérést küld az AMS;
4. hello licenctovábbítási szolgáltatások az AMS használandó hello aktuális érvényes nyilvános kulcsot az Azure AD tooverify hello JWT jogkivonat licencek kiadása előtt.

DRM-licenctovábbítási szolgáltatások mindig keresi hello aktuális érvényes nyilvános kulcsot az Azure AD. hello nyilvános kulcsot az Azure AD által használt ellenőrzése az Azure AD által kibocsátott JWT jogkivonat hello kulcs lesz.

Mi történik, ha a hello kulcsához kapcsolódó kulcsváltás után AAD JWT jogkivonat állít elő, de előtt hello JWT jogkivonat játékosok tooDRM licenctovábbítási szolgáltatások AMS az ellenőrzés által küldött történik?

A kulcs bármikor előfordulhat, hogy állítva, mert nincs mindig egynél több érvényes nyilvános kulccsal elérhető hello összevonási metaadatok dokumentumban. Az Azure Media Services licenc kézbesítési hello kulcsok megadott hello dokumentumot, mivel előfordulhat, hogy később is visszalátogatnia, állítva egy kulcs egy másik előfordulhat, hogy azok helyettesítéseivel, és így tovább használhatja.

### <a name="where-is-hello-access-token"></a>Ha van hello hozzáférési jogkivonat?
Ha tekinti meg hogyan webalkalmazás hívja az API-alkalmazás [OAuth 2.0 ügyfél hitelesítő adatok megadása az Alkalmazásidentitás](../active-directory/develop/active-directory-authentication-scenarios.md#web-application-to-web-api), hello hitelesítési folyamat van, mint alatt:

1. TooAzure AD hello webes alkalmazás a bejelentkezett felhasználó (lásd: hello [webböngésző tooWeb alkalmazás](../active-directory/develop/active-directory-authentication-scenarios.md#web-browser-to-web-application).
2. az Azure AD hello engedélyezési végpont hello felhasználói ügynök hátsó toohello ügyfélalkalmazás engedélyezési kóddal irányítja át. hello felhasználói ügynök engedélyezési kód toohello ügyfél alkalmazás átirányítási URI-t adja vissza.
3. hello webalkalmazásnak kell tooacquire olyan hozzáférési jogkivonatot, hogy a hitelesítéshez toohello webes API és hello szükséges erőforrás lekérése. A kérelem tooAzure AD meg jogkivonat végpontjához hello hitelesítő adat, ügyfél-Azonosítót és webes API-alkalmazás azonosítója URI teszi. Adott hello megadja hello engedélyezési kód tooprove felhasználó hozzájárult.
4. Az Azure AD hitelesíti a hello alkalmazás, és a JWT jogkivonat, amely használt toocall hello webes API adja vissza.
5. A HTTPS PROTOKOLLOKON keresztül hello webalkalmazás hello JWT access token tooadd hello JWT karakterlánc a "Tulajdonos" jelöléssel visszaadott hello engedélyezési fejlécének hello kérelem toohello webes API-t használ. hello webes API-k majd érvényesíti hello JWT jogkivonatot, és ellenőrzés nem jelez hibát, ha vissza hello szükséges erőforrás.

A "Alkalmazásidentitás" folyamatában hello webes API-k hello webes alkalmazás hitelesített hello felhasználó megbízik. Ezért ez a minta egy megbízható alrendszer neve. Hello [diagram ezen az oldalon](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) ismerteti, hogyan engedélyezési kódot adjon folyamata működik.

Licenc a beszerzési token korlátozás, hello azt követő azonos megbízható alrendszer mintát. És hello licenctovábbítási szolgáltatása Azure Media Services hello webes API-k erőforrás, "háttér-erőforrás" hello webalkalmazás tooaccess kell. Ezért hol található hello hozzáférési jogkivonat?

Valóban azt beszerzésekor hozzáférési jogkivonatot az Azure AD. A felhasználó sikeres hitelesítést követően engedélyezési kódot ad vissza. hello engedélyezési program használja, Azonosítóját és az alkalmazás ügyfélkulcsot, a hozzáférési token tooexchange együtt. És hello hozzáférési jogkivonat van mutató "mutató" alkalmazás eléréséhez, vagy Azure Media Services licenctovábbítási szolgáltatása jelző.

Igazolnia kell tooregister és hello "mutató" alkalmazás konfigurálása az Azure AD hello alábbi lépéseket követve:

1. Hello Azure AD-bérlőben

   * bejelentkezés az URL-címmel (erőforrás) alkalmazás hozzáadása:

   https://[resource_name].azurewebsites.NET/ és

   * App ID URL-címe:

   https://[aad_tenant_name].onmicrosoft.com/[resource_name];
2. Adja hozzá egy új kulcsot hello erőforrás alkalmazás;
3. Hello alkalmazás-jegyzékfájl frissítése az, hogy hello groupMembershipClaims tulajdonság értéke a következő hello: "groupMembershipClaims": "All",  
4. A mutató toohello player web app alkalmazásban hello szakaszban "engedélyek tooother alkalmazások" hello Azure AD alkalmazás adja hozzá a hello erőforrás-alkalmazás, amely hozzá lett adva a fenti 1. lépés. A "delegált engedélyek" Ellenőrizze a "Hozzáférés [resource_name]" jelet. Ez engedélyezi hello web app toocreate hozzáférési jogkivonatok hello erőforrás alkalmazás eléréséhez. Akkor tegye ezt hello webes alkalmazás a helyi és a telepített verziójához a Visual Studio és az Azure web app fejlesztői.

Az Azure AD által kibocsátott hello JWT jogkivonat ezért valóban hello hozzáférési jogkivonat az "mutató" erőforrás eléréséhez.

### <a name="what-about-live-streaming"></a>Mi a helyzet élő adatfolyam?
A fenti hello a vitafórum rendelkezik lett összpontosító igény szerinti eszközök. Mi a helyzet live streaming?

hello jó hírünk használható pontosan hello azonos tervezési és megvalósítási védelmére élő adatfolyamként történő Azure Media Services, a program "VOD eszközként" társított hello eszköz kezelésére.

Pontosabban is ismert, hogy toodo élő adatfolyam-Azure Media Services, toocreate kell egy csatornát, majd egy hello csatorna a programot. toocreate hello programot, szükség toocreate egy eszköz, amelynek hello élő archív hello program fogja tartalmazni. A sorrend tooprovide CENC multi-DRM-védelemmel hello élő tartalom toodo, szükség van tooapply hello azonos telepítő/feldolgozási toohello eszköz, mintha csak egy "VOD eszköz" hello program indítása előtt volt.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Mi a helyzet licenckiszolgálókat Azure Media Services kívül?
Az ügyfelek gyakran, előfordulhat, hogy befektettek licenc kiszolgálófarm vagy a saját data center vagy üzemeltetett DRM-szolgáltatók. Szerencsére Azure Media Services Content Protection lehetővé teszi toooperate hibrid módban: tartalma birtokolt, és dinamikusan védett Azure Media Services, amíg DRM-licencek kiszolgálók Azure Media Services kívül érkeznek. Ebben az esetben a következő szempontok változások hello van:

1. hello Secure Token Service igények tooissue jogkivonatok elfogadható és hello licenc kiszolgálófarm ellenőrizheti. Hello Widevine licenckiszolgálókat Axinom által biztosított például egy adott JWT jogkivonat, amely tartalmazza a "jogosultság üzenet" igényel. Ezért van szüksége az STS tooissue toohave ilyen JWT jogkivonat. hello szerzők egy végrehajtás elvégzése, valamint a dokumentum a következő hello hello részleteket megtalálja [Azure dokumentációs központban](https://azure.microsoft.com/documentation/): [használatával Axinom toodeliver Widevine-licencek tooAzure Media Services](media-services-axinom-integration.md).
2. Már nincs szüksége tooconfigure licenctovábbítási szolgáltatása (ContentKeyAuthorizationPolicy) Azure Media Services. Mit kell toodo tooprovide hello licenc licenckérési URL-címeit (PlayReady és Widevine FairPlay) konfigurálása során AssetDeliveryPolicy és multi-DRM CENC beállításában.

### <a name="what-if-i-want-toouse-a-custom-sts"></a>Mi történik, ha egy egyéni STS toouse érdemes?
Néhány oka, hogy az ügyfél biztosítani a JWT-jogkivonatokat is választhat toouse egy egyéni STS (Secure Token Service) lehet. Ezek a következők:

1. hello Identity Provider (IDP) hello ügyfél által használt nem támogatja az STS. Ebben az esetben egy egyéni STS lesz lehetőség.
2. hello ügyfél esetleg rugalmas vagy szorosabb vezérlő az STS integrálása számlázórendszerrel ügyfél előfizető. Például MVPD operátor több OTT előfizető csomagot kínálhat például prémium alapvető, sportruházat, stb. hello operátor érdemes toomatch hello jogcím egy jogkivonatba egy előfizető csomaggal, hogy csak a megfelelő csomag hello tartalmát szeretné elérhetővé tenni. Ebben az esetben egy egyéni STS biztosít hello szükséges rugalmasságot és.

Két módosításokat kell toobe végrehajtott egyéni STS használata esetén:

1. Az eszközhöz tartozó licenctovábbítási szolgáltatása konfigurálásakor be kell toospecify hello biztonsági kulcsot használja hello egyéni STS (további részleteket az alábbi) helyett hello aktuális kulcsot az Azure Active Directory-ellenőrzés.
2. Amikor JTW jogkivonat történik, a biztonsági kulcs helyett hello titkos kulcsot az Azure Active Directoryban hello aktuális X509 tanúsítvány van megadva.

A biztonsági kulcsok két típusa van:

1. Szimmetrikus kulcs: hello ugyanazzal a kulccsal létrehozása és ellenőrzése a JWT jogkivonat;
2. Aszimmetrikus kulcs: egy nyilvános-titkos kulcspár egy X509 tanúsítvánnyal titkosított/generálásához JWT jogkivonat és hello nyilvános kulcs ellenőrzésének hello jogkivonat titkos kulcsával.

#### <a name="tech-note"></a>Műszaki megjegyzés
.NET-keretrendszer használatakor / C#, a fejlesztői platform hello X509 aszimmetrikus kulcs használt tanúsítványnak rendelkeznie kell legalább 2048 bites kulcshosszt. Ez a követelmény a .NET-keretrendszer System.IdentityModel.Tokens.X509AsymmetricSecurityKey hello osztály. Ellenkező esetben a következő kivétel hello fog jelezni:

IDX10630: az aláíráshoz "System.IdentityModel.Tokens.X509AsymmetricSecurityKey" hello nem lehet kisebb, mint '2048' bit.

## <a name="hello-completed-system-and-test"></a>befejeződött hello rendszer és a teszt
Végigvezetjük néhány olyan forgatókönyvet befejeződött hello végpont rendszer, hogy az olvasók egy alapszintű hello viselkedés "képe" rendelkezhet egy bejelentkezési fiókjának beolvasása előtt.

hello player webalkalmazás és a bejelentkezési található [Itt](https://openidconnectweb.azurewebsites.net/).

Ha az alábbiakra lesz szüksége a "nem integrált" lehetőséget: videó eszközök Azure Media Services üzemeltetett, mely vagy védelem nélkül van, vagy védett DRM, de a jogkivonat (kibocsátó egy licenc toowhoever azt kérő) hitelesítés nélkül tesztelheti, bejelentkezési azonosító nélküli (váltásával tooHTTP a videoadatfolyam-továbbítás esetén HTTP Protokollon keresztül).

Ha mire van szüksége a végpontok közötti integrált lehetőséget: lexikális elem a hitelesítés és a rendszer az Azure AD által létrehozott JWT jogkivonat az Azure Media Services dinamikus DRM védelem alatt videó eszközök, akkor kell toologin.

### <a name="user-login"></a>Felhasználói bejelentkezés
Rendelés tootest hello végpontok közötti integrált DRM rendszer kell toohave egy "fiók" létrehozni vagy hozzáadni.

Milyen fiókot?

Bár az Azure eredetileg csak a Microsoft-fiókos felhasználó által hozzáférése engedélyezett, mostantól engedélyezi mindkét rendszer felhasználóinak hozzáférését. Ezt az tettük azzal, hogy az összes hello Azure-tulajdonság megbízik az Azure AD hitelesítésében, így az Azure AD hitelesíti a szervezeti felhasználókat, és hozzon létre összevonási kapcsolat ahol az Azure AD hello Microsoft fiók végfelhasználói identitásrendszere megbízik tooauthenticate identitásrendszerében a felhasználók. Ennek eredményeképpen az Azure AD képes tooauthenticate "Vendég" Microsoft-fiókok, valamint a "natív" Azure AD-fiókok.

Microsoft-fiók (MSA) tartományhoz az Azure AD megbízik, mivel azokat a fiókokat bármelyik hello következő tartományok toohello egyéni az Azure AD bérlői, és használja a hello fiók toologin is hozzáadhat:

| **Tartománynév** | **Tartomány** |
| --- | --- |
| **Egyéni az Azure AD bérlő tartománya** |somename.onmicrosoft.com |
| **Vállalati tartományhoz** |Microsoft.com |
| **Microsoft-fiók (MSA) tartomány** |Outlook.com-os, live.com, hotmail.com |

Hello szerzők toohave létrehozott új és az Ön fiókja is kapcsolódhat.

Az alábbiakban ugyanaz a tartományi fiókok által használt különböző bejelentkezési oldalak hello példaként bemutató képernyőképeket láthat.

**Egyéni az Azure AD tartományi fiók bérlői**: Ebben az esetben hello egyéni az Azure AD bérlői tartomány az hello testreszabott bejelentkezési oldal jelenik meg.

![Egyéni az Azure AD bérlő tartományi fiók](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Microsoft tartományi fiók intelligens kártyával**: Ebben az esetben látni hello bejelentkezési oldal vállalati Microsoft szerint testre szabott informatikai kéttényezős hitelesítéssel.

![Egyéni az Azure AD bérlő tartományi fiók](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-fiók (MSA)**: Ebben az esetben a fogyasztók látja Microsoft Account hello bejelentkezési oldalán.

![Egyéni az Azure AD bérlő tartományi fiók](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>PlayReady titkosított Media bővítményekkel
Olyan modern böngészőt a titkosított Media Extensions (EME) PlayReady támogatása, például a Windows 10-es PlayReady futó Internet Explorer 11 és mentése a Windows 8.1 és a Microsoft Edge böngésző lesz az alapul szolgáló DRM hello EME.

![PlayReady EME használata](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

hello sötét player terület toohello tényt, hogy PlayReady védelem megakadályozza, hogy egy képernyőfelvétel-készítés védett videó hajtsanak miatt van.

hello alábbi képernyőn látható hello player beépülő modulok és MSE/EME támogatás.

![PlayReady EME használata](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

A Microsoft Edge és a Windows 10 Internet Explorer 11 EME lehetővé teszi, hogy meghívása a [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) Windows 10 rendszerű eszközökön, amelyek támogatják azt. PlayReady SL3000 feloldja a bővített támogatás tartalom hello áramló (4 KB-os, HDR, stb.) és új tartalom üzemeltetési modell (a korai ablakának bővített tartalom).

Hello Windows-eszközök összpontosítani: PlayReady van hello csak a hello hardver (PlayReady SL3000) Windows-eszközökön elérhető DRM. Egy adatfolyam-továbbítási szolgáltatás használhatja a PlayReady vagy egy UWP-alkalmazás EME keresztül, és segítségével PlayReady SL3000 egy másik DRM-nál nagyobb videominőséget kínálnak. Általában 2K tartalom Chrome, Firefox vagy keresztül halad, és 4 K tartalom a Microsoft Edge/IE11 vagy egy UWP-alkalmazás hello ugyanarra az eszközre (attól függően, hogy szolgáltatás beállításainak és megvalósítása).

#### <a name="using-eme-for-widevine"></a>Widevine EME használata
Olyan modern böngészőt EME vagy Widevine-támogatással rendelkező, például a Chrome 41 + a Windows 10, Windows 8.1-, Mac OS x Yosemite és Chrome Android 4.4.4, a Google Widevine DRM mögött EME hello.

![Widevine EME használata](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Figyelje meg, hogy Widevine nem gátolja meg egy védett videó képernyőfelvétel-készítés hajtsanak végre.

![Widevine EME használata](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Nem jogosult felhasználók
Ha a felhasználó nincs "Jogosult felhasználók" csoport tagja, hello felhasználók pedig nem fogják tudni toopass "jogosultság ellenőrzése", és hello multi-DRM-licencelési szolgáltatástól fogja megtagadják tooissue hello kért licenc alább látható módon. hello részletes leírás: "licenc megszerzése sikertelen", amelyhez megfelelően van.

![Nem jogosult felhasználók](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Egyéni biztonságos biztonságijogkivonat-szolgáltatás fut
A futó egyéni Secure Token Service (STS) hello esetben hello JWT jogkivonat fogja kiállítani hello egyéni STS szimmetrikus vagy aszimmetrikus kulccsal.

hello eset (Chrome használatával történő) szimmetrikus kulcs használatával:

![Egyéni STS fut](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

kis-és egy X509 keresztül aszimmetrikus kulccsal hello tanúsítvány (Microsoft modern böngészőt használ).

![Egyéni STS fut](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Mindkét fenti esetekben hello felhasználói hitelesítési marad hello azonos – az Azure AD használatával. hello egyetlen különbség, hogy a JWT-jogkivonatokat hello egyéni STS helyett az Azure AD ki. Természetesen dinamikus CENC védelmi konfigurálásakor licenctovábbítási szolgáltatása hello korlátozása típust ad meg hello JWT jogkivonat szimmetrikus vagy aszimmetrikus kulcs.

## <a name="summary"></a>Összefoglalás
Ebben a dokumentumban, amiről beszéltünk CENC token hitelesítéssel több native DRM- és hozzáférés-vezérléssel: a kialakításakor és Azure, az Azure Media Services és az Azure Media Player megvalósítása.

* A referencia-tervező számára jelenik meg a tartalmazó hello szükséges lévő valamennyi összetevőnél DRM/CENC alrendszer;
* Egy Azure, az Azure Media Services és az Azure Media Player hivatkozás végrehajtására.
* Néhány közvetlenül részt hello tervezési és megvalósítási témaköröket is tartalmazza.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 

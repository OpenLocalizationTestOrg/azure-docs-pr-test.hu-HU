---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - egy hibrid identitás elfogadása stratégia kidolgozása |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi hello megadott feltételek hello felhasználói hitelesítés során, és mielőtt engedélyezi a hozzáférést toohello alkalmazás kiválasztása. Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 9ffca675d0c714392adfcbbc4dcfad12fccbac78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>A hibrid identitás bevezetési stratégia meghatározása
Ebben a feladatban fogja definiálni, hello hibrid identitás bevezetési stratégia hibrid identitáskezelési megoldás toomeet hello üzleti igényeinek a tárgyalt:

* [Az üzleti igények meghatározása](active-directory-hybrid-identity-design-considerations-business-needs.md)
* [Címtár-szinkronizálás követelmények meghatározása](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [A multi-factor authentication követelmények meghatározása](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Üzleti igények stratégia meghatározása
hello első lépése a üzleti igények meghatározása hello szervezetek megoldást.  Ez nagyon széles körű lehet, és a hatókör kötés akkor fordulhat elő, ha nincs gondos.  A hello kezdete legyen egyszerű, de ne felejtsen tooplan meghatározásához egy olyan és elősegítheti a jövőbeli hello változásáról.  Függetlenül attól, hogy egy egyszerű tervezett, akár a rendkívül összetett egyikét az Azure Active Directory hello Microsoft Identity platform, amely támogatja az Office 365, Microsoft Online Services és a felhőalapú alkalmazások.

## <a name="define-an-integration-strategy"></a>Az integráció stratégia meghatározása
A Microsoftnál három fő integrációs feladatokhoz, amelyek felhőbeli identitások, a szinkronizált identitások és az összevont identitások kialakítása.  Meg kell terveznie a integrációs stratégiák egyikét alkalmazásakor.  hello stratégia eltérőek lehetnek, ezúttal valamelyik hello szempontjait is lehetnek, milyen típusú felhasználói felület tooprovide kívánt, van néhány hello meglévő infrastruktúra már helyben, és mi az leginkább költséghatékony hello.  

![](./media/hybrid-id-design-considerations/integration-scenarios.png)

a fenti ábrán hello definiált hello forgatókönyvek a következők:

* **Felhőbeli identitások**: ezek a identitásokat tartalmaz, amelyek kizárólag hello felhőben található.  Hello esetében az Azure AD azok kellene lennie kifejezetten az Azure AD-címtár.
* **Szinkronizált**: ezek a meglévő helyszíni identitások és hello felhőben található.  Azure AD Connect használatával, ezek a felhasználók vannak létre, vagy a meglévő Azure AD-fiókokkal csatlakoztatva.  hello felhasználó Jelszókivonat felhőből szinkronizált hello a helyszíni környezet toohello a Jelszókivonat úgynevezett.  Szinkronizált használatával hello egy szerint esetén meg kell, hogy ha a felhasználó le van tiltva, a helyszíni környezetben hello, is eltarthat, az adott fiók állapota tooshow too3 órában az Azure ad-ben.  Ez a miatt toohello szinkronizálás alatt az időtartam alatt.
* **Összevont**: ezeket az identitásokat létezik mind a helyszíni és felhőben hello.  Azure AD Connect használatával, ezek a felhasználók vannak létre, vagy a meglévő Azure AD-fiókokkal csatlakoztatva.  

> [!NOTE]
> Hello szinkronizálási beállításokkal kapcsolatos további információkért olvassa el a [a helyszíni identitások integrálása az Azure Active Directoryval](connect/active-directory-aadconnect.md).
> 
> 

hello a következő táblázat segít meghatározni, hogy az egyes következő stratégiák hello hello előnyeit és hátrányait:

| Stratégia | Előnyei | Hátrányok |
| --- | --- | --- |
| **Felhőbeli identitások** |Egyszerűbb toomanage kis szervezet számára. <br> Semmi tooinstall helyi nem kiegészítő hardver szükséges.<br>Könnyen használható, ha hello felhasználó elhagyja hello vállalati |A felhasználóknak kell toosign a munkaterhelések hello felhőben való hozzáféréskor <br> Jelszavak is, vagy nem lehet ugyanaz a felhőalapú és helyszíni identitások hello |
| **Szinkronizálva** |A helyszíni jelszó lesz a helyszíni hitelesítéshez és a felhőcímtárakat <br>A kis, közepes vagy nagy szervezetek könnyebben toomanage <br>Az egyes erőforrások felhasználók rendelkezhetnek egyszeri bejelentkezés (SSO) <br> A szinkronizálás Microsoft előnyben részesített módszer <br> Egyszerűbb toomanage |Lehet, hogy egyes ügyfelek vonakodni fog a változtatásoktól toosynchronize hello a könyvtárak a felhő adott vállalat rendőrségi miatt |
| **Összevont** |Felhasználók rendelkezhetnek egyszeri bejelentkezés (SSO) <br>Ha a felhasználó le van állítva, vagy hagyja, hello fiókot is, azonnal letiltja, és visszavonja a hozzáférést,<br> Speciális forgatókönyvek, amelyek nem lehet, hitelesítéstípussal szinkronizálva |További lépések toosetup és konfigurálása <br> Magasabb karbantartás <br> Szükség lehet további hardverek hello STS-infrastruktúra <br> Kiegészítő hardvert tooinstall hello összevonási kiszolgáló lehet szükség. További szoftverek szükség, ha az AD FS szolgál <br> Széles körű beállítása szükséges az egyszeri bejelentkezés <br> Kritikus pont hiba, ha a hello összevonási kiszolgáló nem működik, a felhasználók nem tudják tooauthenticate |

### <a name="client-experience"></a>Ügyfélélmény
hello stratégia, amelyekkel hello felhasználói bejelentkezési élmény szabja meg.  hello alábbi táblázatok tartalmazzák a felhasználók milyen hello kell látnia a bejelentkezés vonatkozó tapasztal toobe.  Vegye figyelembe, hogy nem minden összevont identitás-szolgáltatóktól mindegyik forgatókönyvben támogatja az egyszeri Bejelentkezést.

**A tartomány tagja, és saját hálózati alkalmazások**:

|  | Szinkronizált identitás | Összevont identitás |
| --- | --- | --- |
| Webböngészők |Űrlapalapú hitelesítés |egyszeri bejelentkezéshez, egyes esetekben szükséges toosupply szervezet azonosítója |
| Outlook |Hitelesítő adatok kérése |Hitelesítő adatok kérése |
| Skype vállalati (Lync). |Hitelesítő adatok kérése |egyszeri bejelentkezést Lync, a hitelesítő adatokat kéri az Exchange-hez |
| SkyDrive Pro |Hitelesítő adatok kérése |az egyszeri bejelentkezés |
| Office Pro Plus előfizetés |Hitelesítő adatok kérése |az egyszeri bejelentkezés |

**Külső vagy nem megbízható forrásból**:

|  | Szinkronizált identitás | Összevont identitás |
| --- | --- | --- |
| Webböngészők |Űrlapalapú hitelesítés |Űrlapalapú hitelesítés |
| Az Outlook, a Skype vállalati (Lync) Skydrive Pro, Office-előfizetés |Hitelesítő adatok kérése |Hitelesítő adatok kérése |
| Exchange ActiveSync |Hitelesítő adatok kérése |egyszeri bejelentkezést Lync, a hitelesítő adatokat kéri az Exchange-hez |
| Mobilalkalmazások |Hitelesítő adatok kérése |Hitelesítő adatok kérése |

Ha megadta, hogy a feladat, hogy rendelkezik-e a 3. fél IdP vagy azok folyamatos toouse 1 és az Azure AD egy tooprovide összevonási, kell toobe tisztában legyen a következő hello támogatott képességek:

* Bármely SAML 2.0-s szolgáltatót, amely megfelel az SP-Lite profil hello támogathatja a hitelesítési tooAzure AD és a társított alkalmazások
* Támogatja a passzív hitelesítést, amely megkönnyíti a hitelesítési tooOWA, SPO, stb.
* Hello SAML 2.0 fokozott ügyfél profil (ECP) keresztül támogatja az ügyfelek Exchange online-hoz

Emellett figyelembe kell venni, milyen lehetőségek nem lesznek elérhetők:

* WS-megbízhatósági/összevonási támogatás nélkül megszakítja a más aktív ügyfelek
  * Ez azt jelenti, nem Lync ügyfelet, a OneDrive-ügyfél, Office-előfizetés, Office Mobile előzetes tooOffice 2016
* Átmenet Office toopassive hitelesítés lehetővé teszi toosupport tiszta SAML 2.0 IdPs, de a támogatás továbbra is elérhető lesz a ügyfél-ügyfél alap

> [!NOTE]
> Hello legnaprakészebb lista hello cikk http://aka.ms/ssoproviders olvassa el.
> 
> 

## <a name="define-synchronization-strategy"></a>Szinkronizálás stratégia meghatározása
Ebben a feladatban határozza meg, amely használt toosynchronize hello szervezet helyszíni adatok toohello felhő- és mi lesz hello eszközök topológia kell használnia.  Mivel a legtöbb szervezet számára az Active Directory használatával, információ az Azure AD Connect tooaddress hello fenti kérdések használatával valósul meg részletesen bemutatható.  Az Active Directory nem rendelkező környezetekben FIM 2010 R2 használatával kapcsolatos információk, vagy a MIM 2016 toohelp tervezze meg ezt a stratégiát.  Azonban későbbi kiadásokban az Azure AD Connect támogatja az LDAP-címtárak esetén, attól függően, a ütemterv ezt az információt, előfordulhat, hogy képes tooassist.

### <a name="synchronization-tools"></a>Szinkronizálási eszközök
Hello év alatt számos szinkronizálási eszköz rendelkezik létezett, és különböző környezetben használható.  Az Azure AD Connect jelenleg van hello lépjen tootool a választás az összes támogatott forgatókönyveket.  AAD-Szinkronizáló DirSync továbbra is körül és lehet telepítve legyen a környezetben most. 

> [!NOTE]
> Hello legfrissebb tudnivalókat hello támogatott képességek vonatkozó minden eszköz [címtár-integrációs eszközök összehasonlítása](active-directory-hybrid-identity-design-considerations-tools-comparison.md) cikk.  
> 
> 

### <a name="supported-topologies"></a>Támogatott topológiák
A szinkronizálás stratégia meghatározásakor hello topológia használt kell meghatározni. Attól függően, hogy hello információk segítségével meghatározhatja, melyik topológia 2. lépésben meghatározott hello megfelelő egy toouse. hello egyetlen erdő, egyetlen Azure AD-topológia hello leggyakoribb, és egyetlen Active Directory-erdő és az Azure AD egyetlen példányát áll.  Hello forgatókönyvek többsége használt toobe lesz, és a várt hello topológia esetén az Azure AD Connect Expressz telepítést használ, az alábbi hello ábrán látható módon.

![](./media/hybrid-id-design-considerations/single-forest.png)Egyetlen erdő forgatókönyv esetén igen elterjedt a még kis és nagy szervezetek toohave több erdőket, 5. ábrán látható módon.

> [!NOTE]
> További információ a különböző helyszíni hello és az Azure AD Connect szinkronizálása az Azure AD-topológiák hello a cikk elolvasása [az Azure AD Connect topológiák](connect/active-directory-aadconnect-topologies.md).
> 
> 

![](./media/hybrid-id-design-considerations/multi-forest.png) 

Többerdős forgatókönyv

Ha ebben az esetben hello majd hello több forest egyetlen Azure AD-topológia tekintendő hello következő elemek teljesülése esetén:

* Felhasználók csak 1 identitással rendelkezik erdők – hello, amely egyedileg azonosítja az alábbi felhasználók szakasz ismerteti, hogy ez további információkhoz juthat.
* hello felhasználó hitelesíti toohello erdőben, ahol az identitásukat
* Ez az erdő határozza meg UPN és Forráshorgony (megváltoztathatatlan azonosító)
* Minden erdők érhetők el az Azure AD Connect – Ez azt jelenti, hogy nem kell toobe tartományhoz, és ha ez elősegíti a szegélyhálózaton elhelyezni.
* A felhasználóknál csak egy postaláda
* hello erdő, amelyen a felhasználó postaládájához rendelkezik hello ajánlott az adatminőségi attribútumok látható hello Exchange globális cím lista (GAL)
* Ha hello a felhasználónak nincs postaláda van, akkor előfordulhat, hogy bármely erdőben kell használt toocontribute ezeket az értékeket
* Ha rendelkezhetnek hivatkozott postafiókkal, majd nincs is egy másik fiókot a egy másik erdőben használt toosign.

> [!NOTE]
> Objektumok, szerepel a helyszíni és felhőben hello "keresztül vannak csatlakoztatva" egyedi azonosító. A címtár-szinkronizálás hello kontextusában Ez az egyedi azonosító hivatkozott tooas hello SourceAnchor. Hello környezetben az egyszeri bejelentkezést Ez a hivatkozott tooas hello ImmutableId. [Az Azure AD Connect tervezési alapelvek](connect/active-directory-aadconnect-design-concepts.md#sourceanchor) hello általi használatára vonatkozó további szempontokról a SourceAnchor.
> 
> 

Ha a fenti hello nem teljesül, és több aktív fiókkal vagy egynél több postaláda van, az Azure AD Connect válasszon egyet, és figyelmen kívül más hello.  Ha postaládák, de nincs más fiók, ezek a fiókok csak akkor exportált tooAzure AD, és, hogy a felhasználó nem lesz csoportnak sem tagja.  Ez nem azonos a hogyan, mint a DirSync korábbi hello és szándékos toobetter támogatási Többerdős forgatókönyvekben. Egy Többerdős forgatókönyv hello az alábbi ábrán látható.

![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 

**Többerdős több Azure AD-forgatókönyv**

Ajánlott egy szervezet, de az az Azure AD-ben csak egyetlen könyvtár toohave azt egy 1:1 számosságú kapcsolatok tárhely az Azure AD Connect szinkronizálási kiszolgálót és az Azure AD-címtár között támogatott.  Minden Azure AD példányához szüksége lesz egy Azure AD Connect telepítése.  Is az Azure AD úgy lett kialakítva elkülönített és az Azure AD egy példányát a felhasználók nem lesz képes toosee felhasználók számára egy másik példánya.

Lehetséges, és egy támogatott tooconnect helyszíni példányát az Active Directory toomultiple Azure AD-címtártól a hello az alábbi ábrán látható módon:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 

**Egyerdős szűrési forgatókönyv**

A sorrend toodo a hello alábbiaknak igaznak kell lenniük:

* Az Azure AD Connect szinkronizálási kiszolgálót úgy, hogy egyes objektumok egymást kölcsönösen kizáró csoportja szűréshez kell konfigurálni.  Ez végezhető el, például minden kiszolgáló tooa az egyes tartományokhoz vagy szervezeti egység hatókörének.
* A DNS-tartomány csak egy regisztrálható Azure AD-címtárban, hello hello hello felhasználóinak UPN-EK a helyszíni AD különálló névterek kell használnia.
* Azure AD egy példányát a felhasználók csak akkor tudja toosee felhasználók a példányból.  Program nem tudja toosee felhasználók hello más esetekben kell
* Csak az egyik hello Azure AD-könyvtárban engedélyezheti az Exchange hibrid hello a helyszíni AD
* Kölcsönös kizárólagosság toowrite visszaírt is vonatkozik.  Így néhány késleltetve visszaírt funkció nem támogatott a topológia, mivel ezek feltételezik, hogy egyetlen helyszíni konfigurációt.  Ehhez a következőket:
  * Késleltetve visszaírt csoport alapértelmezett konfigurációja
  * Eszköz késleltetve visszaírt

Vegye figyelembe, hogy hello következő nem támogatott, és nem kell kiválasztani, megvalósítását:

* Nincs több Azure AD Connect szinkronizálási kiszolgálót toohello ugyanazt az Azure AD directory Kapcsolódás akkor is, ha egymást kölcsönösen kizáró konfigurált toosynchronize beállítása objektum támogatott toohave
* Ez nem támogatott toosync hello ugyanazon felhasználó toomultiple az Azure AD-könyvtárak. 
* Akkor is nem támogatott toomake konfigurációs módosítása toomake felhasználók egy, az Azure AD tooappear felveszi a kapcsolatot egy másik Azure AD-címtárban. 
* Akkor is nem támogatott toomodify az Azure AD Connect szinkronizálási tooconnect toomultiple az Azure AD könyvtárak.
* Az Azure AD könyvtárakban elkülönített azért vannak. Nem támogatott toochange hello konfigurálása az Azure AD Connect szinkronizálási tooread adatait egy kísérlet toobuild közös és egységes globális Címlista hello könyvtárak között egy másik Azure AD-címtár is. Egyben nem támogatott tooexport felhasználót tooanother kapcsolatba lép a helyszíni AD az Azure AD Connect szinkronizálási szolgáltatás használatával.

> [!NOTE]
> Ha a szervezet internetes toohello csatlakozzon a hálózaton lévő számítógépekre korlátozza, ez a cikk felsorolja hello végpontok (FQDN, IPv4 és IPv6-címtartományok) bele kell foglalni, listák és az Internet Explorer Megbízható helyek zónában, a kimenő engedélyezése ügyfél-számítógépek tooensure a számítógépek sikeresen használhatja az Office 365. További információk [Office 365 URL-címei és IP-címtartományok](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>A multi-factor authentication-stratégia meghatározása
Ebben a feladatban hello a multi-factor authentication stratégia toouse határozza meg.  Az Azure multi-factor Authentication két különböző verzióban elérhető lesz.  Egyik egy felhőalapú, más hello helyszíni Azure MFA kiszolgáló hello segítségével.  Hello kiértékelése adott fent, meghatározhatja, melyik megoldás van hello helyes-e egy másik pedig a stratégia alapján.  Hello táblázattal alatt toodetermine mely tervezési lehetőség a vállalati biztonsági követelmény teljesítésére:

Multi-factor Authentication kialakítási lehetőségeit:

| Eszköz toosecure | Többtényezős hitelesítés hello felhőben | Helyszíni MFA |
| --- | --- | --- |
| Microsoft-alkalmazások |igen |igen |
| SaaS-alkalmazásokat az hello alkalmazásgyűjtemény |igen |igen |
| Az Azure AD-alkalmazásproxyn keresztül közzétett IIS-alkalmazások |igen |igen |
| IIS alkalmazások nincsenek közzétéve hello Azure AD alkalmazás-Proxy |nem |igen |
| VPN-és a távoli asztali Átjárókiszolgáló távoli hozzáféréshez |nem |igen |

Annak ellenére, hogy előfordulhat, hogy kiegyenlítése után a megoldást a stratégia, toouse hello értékeli a fenti hol találhatók a felhasználók továbbra is kell.  Emiatt a hello megoldás toochange.  Táblázattal hello tooassist alatt, ez meghatározása:

| Felhasználó helye | Előnyben részesített tervezési beállítás |
| --- | --- |
| Azure Active Directory |Multi-FactorAuthentication hello felhőben |
| Azure AD és helyszíni AD összevonással az AD FS-sel |Mindkét |
| Az Azure AD és a helyszíni AD az Azure AD Connect nem jelszó-szinkronizálás |Mindkét |
| Az Azure AD és a helyszíni és a jelszó-szinkronizálás az Azure AD Connect használatával |Mindkét |
| A helyszíni AD |Multi-Factor Authentication-kiszolgáló |

> [!NOTE]
> Gondoskodnia kell arról, hogy hello a multi-factor authentication tervezési beállítás, amely a kijelölt támogatja-e a szükséges kívánt hello funkciókat.  További információk [hello többtényezős biztonsági megoldás kiválasztása az Ön](../multi-factor-authentication/multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).
> 
> 

## <a name="multi-factor-auth-provider"></a>Többtényezős hitelesítésszolgáltató
Többtényezős hitelesítés alapértelmezés szerint egy Azure Active Directory-bérlő rendelkező globális rendszergazdák számára érhető el. Azonban ha meg akarja tooextend a multi-factor authentication tooall a felhasználók és/vagy tooyour globális rendszergazdák toobe képes tootake előny szolgáltatások, mint a hello felügyeleti portál, az egyéni hónap és a jelentéseket szeretné, majd vásárolnia és konfigurálnia kell Többtényezős hitelesítési szolgáltató.

> [!NOTE]
> Gondoskodnia kell arról, hogy hello a multi-factor authentication tervezési beállítás, amely a kijelölt támogatja-e a szükséges kívánt hello funkciókat. 
> 
> 

## <a name="next-steps"></a>Következő lépések
[Adatvédelmi követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)


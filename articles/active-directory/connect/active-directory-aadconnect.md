---
title: "Az Active Directory csatlakoztatása az Azure Active Directoryhoz | Microsoft Docs"
description: "Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazásokhoz közös identitás tooprovide."
keywords: "Bevezetés tooAzure AD Connect, az Azure AD Connect áttekintése, mi az Azure AD Connect telepítése az active directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e49f2af4b67e9ed3ad093888541da7c82af0e052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-on-premises-directories-with-azure-active-directory"></a>A helyszíni címtárak integrálása az Azure Active Directoryval
Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazások a felhasználók közös identitás tooprovide. Ez a témakör végigvezeti hello tervezési, telepítési és működtetéshez szükséges lépéseken. Hivatkozások toohello témakörök kapcsolódó toothis terület gyűjteménye.

> [!IMPORTANT]
> [Az Azure AD Connect van hello a legjobb módja tooconnect a helyszíni címtár és az Azure AD és az Office 365. Ez az egy ideje tooupgrade tooAzure AD-csatlakozás a Windows Azure Active Directory-szinkronizálás (DirSync) vagy az Azure AD Sync mivel ezek az eszközök most elavult és támogatásuk 2017. április 13 megszűnik.](active-directory-aadconnect-dirsync-deprecated.md)
> 
> 

![Mi az az Azure AD Connect?](media/active-directory-aadconnect/arch.png)

## <a name="why-use-azure-ad-connect"></a>Miért érdemes az Azure AD Connect megoldást használni?
A helyszíni címtárak és az Azure AD integrálása révén a felhasználók munkája hatékonyabbá válik, mivel a felhőalapú és a helyszíni erőforrások hozzáféréséhez közös identitás áll a rendelkezésükre. Felhasználók és a szervezetek kihasználhatják a hello következő:

* A felhasználók használni egy egyetlen identitást tooaccess helyszíni alkalmazásokhoz és felhőszolgáltatásokhoz, mint például az Office 365.
* Egyetlen eszköz tooprovide a szinkronizálást, és jelentkezzen be egy egyszerű üzembe helyezését.
* Az esetek hello elérhető legújabb képességeket biztosít. Az Azure AD Connect olyan identitásintegrációs eszközök régebbi verzióit váltja fel, mint például a DirSync és az Azure AD Sync. További információk: [Hybrid Identity directory integration tools comparison](../active-directory-hybrid-identity-design-considerations-tools-comparison.md) (Hibrid identitás: a címtár-integrációs eszközök összehasonlítása).

### <a name="how-azure-ad-connect-works"></a>Az Azure AD Connect működése
Az Azure Active Directory Connect három elsődleges összetevőből készül: hello szinkronizálási szolgáltatások, az Active Directory összevonási szolgáltatások összetevők hello és hello nevű megfigyelési összetevőből [az Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md) .

<center>![Az Azure AD Connect-verem](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

* Szinkronizálás – ez az összetevő a felhasználók, csoportok és egyéb objektumok létrehozásáért felelős. Is felelős arról, hogy a helyi felhasználók és csoportok identitásinformációi van megfelelő hello felhő.
* Az AD FS - összevonási egy Azure AD Connect elhagyható összetevője, és lehet használt tooconfigure egy hibrid környezetben helyszíni AD FS infrastruktúra. Ez a szervezetek tooaddress összetett telepítések, például a tartomány-csatlakoztatási SSO, AD bejelentkezési házirend, és az intelligens kártya és a 3. fél MFA által is használható.
* Állapotfigyelés – az Azure AD Connect Health hatékony megfigyelési képességgel rendelkezik, és adja meg a központi helyet hello Azure portál tooview ezt a tevékenységet. További információk: [Azure Active Directory Connect Health](../connect-health/active-directory-aadconnect-health.md).

## <a name="install-azure-ad-connect"></a>Az Azure AD Connect telepítése
Az Azure AD Connect hello letöltési található [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).

| Megoldás | Forgatókönyv |
| --- | --- |
| Előkészületek – [Hardverkövetelmények és előfeltételek](active-directory-aadconnect-prerequisites.md) |<li>Lépéseket toocomplete tooinstall az Azure AD Connect megkezdése előtt.</li> |
| [Gyorsbeállítások](active-directory-aadconnect-get-started-express.md) |<li>Ha egyerdős AD majd ez hello ajánlott beállítás toouse.</li> <li>Felhasználói bejelentkezés hello ugyanaz a jelszó-szinkronizálás jelszó.</li> |
| [Testreszabott beállítások](active-directory-aadconnect-get-started-custom.md) |<li>Több erdő megléte esetén használatos. Számos helyszíni [topológiát](active-directory-aadconnect-topologies.md) támogat.</li> <li>Testre szabhatja a bejelentkezést, például ADFS-t állíthat be az összevonáshoz, vagy külső féltől származó identitásszolgáltatót használhat.</li> <li>Testre szabhatja a szinkronizálási funkciókat, például a szűrést és a visszaírást.</li> |
| [Frissítés a DirSync szolgáltatásról](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Akkor használatos, ha már rendelkezik működő DirSync-kiszolgálóval.</li> |
| [Frissítés Azure AD Sync-ről vagy Azure AD Connectről](active-directory-aadconnect-upgrade-previous-version.md) |<li>Igény szerint számos különböző módszer áll rendelkezésére.</li> |

[A telepítés után](active-directory-aadconnect-whats-next.md) ellenőriznünk kell a megfelelő működést, és a szükséges licencek kiosztása a felhasználók toohello.

### <a name="next-steps-tooinstall-azure-ad-connect"></a>Az Azure AD Connect tooInstall további lépések
|Témakör |Hivatkozás|  
| --- | --- |
|Az Azure AD Connect letöltése | [Az Azure AD Connect letöltése](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Telepítés gyorsbeállítások használatával | [Az Azure AD Connect gyorstelepítése](./active-directory-aadconnect-get-started-express.md)|
|Telepítés testreszabott beállítások használatával | [Az Azure AD Connect testreszabott telepítése](./active-directory-aadconnect-get-started-custom.md)|
|Frissítés a DirSync szolgáltatásról | [Frissítés az Azure AD szinkronizáló eszközéről (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|A telepítést követően | [Hello telepítésének ellenőrzése és licencek hozzárendelése](active-directory-aadconnect-whats-next.md)|

### <a name="learn-more-about-install-azure-ad-connect"></a>További információk az Azure AD Connect telepítésével kapcsolatban
Szeretné a tooprepare [működési](active-directory-aadconnectsync-operations.md) vonatkozik. A készenléti kiszolgáló toohave, akkor egyszerűen tartalék is esetén érdemes egy [katasztrófa](active-directory-aadconnectsync-operations.md#disaster-recovery). Ha azt tervezi, toomake gyakori konfigurációs módosításokat, tervezze meg a [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode) kiszolgáló.

|Témakör |Hivatkozás|  
| --- | --- |
|Támogatott topológiák | [Azure AD Connect-topológiák](active-directory-aadconnect-topologies.md)|
|Tervezési alapelvek | [Az Azure AD Connect tervezési alapelvei](active-directory-aadconnect-design-concepts.md)|
|Telepítési fiókok | [További információk az Azure AD Connect hitelesítő adataival és engedélyeivel kapcsolatban](./active-directory-aadconnect-accounts-permissions.md)|
|Az üzemeltetés megtervezése | [Az Azure AD Connect szinkronizálása: üzemeltetési feladatok és szempontok](active-directory-aadconnectsync-operations.md)|
|A felhasználói bejelentkezés lehetőségei | [A felhasználói bejelentkezés lehetőségei az Azure AD Connectben](active-directory-aadconnect-user-signin.md)|

## <a name="configure-sync-features"></a>A szinkronizálási funkciók konfigurálása
Az Azure AD Connect számos, szükség szerint bekapcsolható vagy alapértelmezés szerint engedélyezett funkcióval rendelkezik. Bizonyos forgatókönyvek és topológiák esetén ezen funkciók némelyike további konfigurációs beállítások megadását igényli.

[Szűrés](active-directory-aadconnectsync-configure-filtering.md) használatos, ha azt szeretné, hogy mely objektumok érhetők toolimit tooAzure AD szinkronizálva. Alapértelmezés szerint valamennyi felhasználó, névjegy és Windows 10 rendszert használó számítógép szinkronizálva van. Hello szűrési beállítások tartományok, szervezeti egységek vagy attribútumok módosíthatja.

[A jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md) hello Jelszókivonat az Active Directory tooAzure AD szinkronizálja. hello végfelhasználói használhatja ugyanazt a jelszót a helyi és hello felhőben, de csak az adatbázis felügyeletét az egyik helyen hello. Mivel a helyszíni Active Directory hello szolgáltatóként használ, saját jelszóházirendjét is használhatja.

[A jelszóvisszaírás](../active-directory-passwords-getting-started.md) engedélyezése a felhasználók toochange és visszaállíthassák a jelszavukat hello felhőben, és rendelkezik a helyszíni alkalmazott jelszóházirendnek.

[Eszközvisszaíró](active-directory-aadconnect-feature-device-writeback.md) lehetővé teszi egy eszköz regisztrálva az Azure AD toobe visszaírását tooon helyszíni Active Directory, a feltételes hozzáférés is használható.

Hello [véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) funkció alapértelmezés szerint be van kapcsolva, és a felhőcímtárat: hello számos törli a ugyanannyi időt vesz igénybe. Alapértelmezés szerint futtatásonként 500 törlést tesz lehetővé. Ezt a beállítást szervezetének mérete alapján módosíthatja.

[Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) a gyorsbeállításokkal végrehajtott telepítéseknél alapértelmezés szerint engedélyezve van, és biztosítja az Azure AD Connect mindig be toodate hello legújabb verziójában.

### <a name="next-steps-tooconfigure-sync-features"></a>Következő lépések tooconfigure szinkronizálási szolgáltatások
|Témakör |Hivatkozás|  
| --- | --- |
|A szűrés konfigurálása | [Az Azure AD Connect szinkronizálása: a szűrés konfigurálása](active-directory-aadconnectsync-configure-filtering.md)|
|Jelszó-szinkronizálás | [Az Azure AD Connect szinkronizálása: a jelszó-szinkronizálás megvalósítása](active-directory-aadconnectsync-implement-password-synchronization.md)|
|Jelszóvisszaíró | [A jelszókezelés első lépései](../active-directory-passwords-getting-started.md)|
|Eszközvisszaíró | [Eszközvisszaírás engedélyezése az Azure AD Connectben](active-directory-aadconnect-feature-device-writeback.md)|
|Véletlen törlések megakadályozása | [Az Azure AD Connect szinkronizálása: véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)|
|Automatikus frissítés | [Azure AD Connect: automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md)|

## <a name="customize-azure-ad-connect-sync"></a>Az Azure AD Connect szinkronizálásának testreszabása
Azure AD Connect szinkronizálása származik, amely a legtöbb ügyfél és topológia tervezett toowork alapértelmezett konfigurációja. Azonban mindig hello alapértelmezett konfigurációja nem működik, és úgy kell beállítani. A jelen szakaszban és a csatolt témakörökben leírtak szerint támogatott toomake módosításokat is.

Ha korábban nem dolgozott szinkronizált topológiákkal szeretné, hogy toostart toounderstand hello alapjai és kifejezések jegyzékét lásd: hello hello előtt [technikai kulcsfogalmak](active-directory-aadconnectsync-technical-concepts.md). Az Azure AD Connect MIIS2003 ILM2007 és FIM2010 hello fejlődéséhez. Még ha egyes elemek azonosak is, számos módosításra került sor.

Hello [alapértelmezett konfiguráció](active-directory-aadconnectsync-understanding-default-configuration.md) azt feltételezi, hogy lehet egynél több erdő hello konfigurációban. Ezekben a topológiákban a felhasználói objektumok kapcsolattartóként is megjelenhetnek egy másik erdőben. hello felhasználó már nem hivatkozott postafiókkal egy másik erőforráserdőben. hello hello alapértelmezett konfiguráció viselkedésének leírása [felhasználók és névjegyek](active-directory-aadconnectsync-understanding-users-and-contacts.md).

hello konfigurációs modell szinkronban nevezik [deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md). hello speciális attribútumfolyamok [funkciók](active-directory-aadconnectsync-functions-reference.md) tooexpress attribútum átalakításokat. Tekintse át, és vizsgálja meg a hello teljes konfigurációs eszközök használatával az Azure AD Connect amely. Ha toomake konfigurációs módosítások van szüksége, ellenőrizze, hogy hajtsa végre a hello [ajánlott eljárások](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) , hogy könnyebben tooadopt új kiadásokat.

### <a name="next-steps-toocustomize-azure-ad-connect-sync"></a>További lépések toocustomize az Azure AD Connect szinkronizálási szolgáltatás
|Témakör |Hivatkozás|  
| --- | --- |
|Az Azure AD Connect-szinkronizáláshoz kapcsolódó összes cikk | [Az Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md)|
|Technikai kulcsfogalmak | [Az Azure AD Connect szinkronizálása: technikai kulcsfogalmak](active-directory-aadconnectsync-technical-concepts.md)|
|Understanding hello alapértelmezett konfigurációja | [Azure AD Connect szinkronizálása: Understanding hello alapértelmezett konfigurációja](active-directory-aadconnectsync-understanding-default-configuration.md)|
|A felhasználók és a kapcsolattartók ismertetése | [Az Azure AD Connect szinkronizálása: a felhasználók és a kapcsolattartók ismertetése](active-directory-aadconnectsync-understanding-users-and-contacts.md)|
|Deklaratív kiépítés | [Az Azure AD Connect szinkronizálása: a deklaratív kiépítés kifejezéseinek ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)|
|Hello alapértelmezett konfigurációjának módosítása | [Hello alapértelmezett konfiguráció módosításának ajánlott eljárásai](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)|

## <a name="configure-federation-features"></a>Az összevonási funkciók konfigurálása
Az AD FS lehet konfigurált toosupport [több tartomány](active-directory-aadconnect-multiple-domains.md). Például lehetséges, hogy több legfelső szintű tartományt kell toouse az összevonáshoz.

Ha az ADFS-kiszolgáló nem lett konfigurálva tooautomatically tanúsítványok frissítése az Azure AD, vagy ha nem ADFS rendszerű megoldást használ, majd értesítést fog kapni Ha túl[tanúsítványok frissítése](active-directory-aadconnect-o365-certs.md).

### <a name="next-steps-tooconfigure-federation-features"></a>Következő lépések tooconfigure az összevonási funkciók
|Témakör |Hivatkozás|  
| --- | --- |
|Minden AD FS-cikk | [Azure AD Connect és összevonás](active-directory-aadconnectfed-whatis.md)|
|Az ADFS konfigurálása altartományokkal | [Többtartományos támogatás az Azure AD összevonási szolgáltatásához](active-directory-aadconnect-multiple-domains.md)|
|AD FS-farm kezelése | [Az AD FS kezelése és testreszabása az Azure AD Connect segítségével](active-directory-aadconnect-federation-management.md)|
|Összevonási tanúsítványok manuális frissítése | [Az Office 365 és az Azure AD összevonási tanúsítványainak megújítása](active-directory-aadconnect-o365-certs.md)|

## <a name="more-information-and-references"></a>További információk és hivatkozások
|Témakör |Hivatkozás|  
| --- | --- |
|Verzióelőzmények | [Verzióelőzmények](active-directory-aadconnect-version-history.md)|
|A DirSync, az Azure ADSync és az Azure AD Connect összehasonlítása | [Címtár-integrációs eszközök összehasonlítása](../active-directory-hybrid-identity-design-considerations-tools-comparison.md)|
|Az Azure AD nem ADFS-elemekkel fennálló kompatibilitásainak listája | [Az Azure AD összevonás kompatibilitási listája](active-directory-aadconnect-federation-compatibility.md)|
|SAML 2.0 identitásszolgáltató konfigurálása|[SAML 2.0 identitásszolgáltató (IdP) használata egyszeri bejelentkezéshez](active-directory-aadconnect-federation-saml-idp.md)|
|Szinkronizált attribútumok | [Szinkronizált attribútumok](active-directory-aadconnectsync-attributes-synchronized.md)|
|Megfigyelés az Azure AD Connect Health használatával | [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md)|
|Gyakori kérdések | [Azure AD Connect – gyakori kérdések](active-directory-aadconnect-faq.md)|

**További források**

Az ignite 2015 bemutatója a kiterjesztése a helyszíni címtárak toohello felhő.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3862/player]
> 
> 


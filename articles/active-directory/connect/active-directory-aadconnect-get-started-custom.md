---
title: "Azure AD Connect: Testre szabott telepítés | Microsoft Docs"
description: "Ez a dokumentum az Azure AD Connect hello egyéni telepítési beállításait részletezi. Ezen utasításokat tooinstall Active Directory-ban az Azure AD Connect használja."
services: active-directory
keywords: "mi az Azure AD Connect, az Active Directory telepítése, az Azure AD szükséges összetevői"
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 6d42fb79-d9cf-48da-8445-f482c4c536af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: c49724fab78c5a1688662043f69506fff6f82104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="custom-installation-of-azure-ad-connect"></a>Az Azure AD Connect testreszabott telepítése
Az Azure AD Connect **egyéni beállítások** használatos, ha azt szeretné, hogy további beállítások hello telepítéshez. Ha több erdővel rendelkezik, vagy ha azt szeretné, hogy nem tartalmazza az expressz telepítési hello tooconfigure választható szolgáltatások használható. Szerepel, minden esetben ha hello [ **Expressz telepítés** ](active-directory-aadconnect-get-started-express.md) beállítás nem megfelelő az üzemelő példányhoz vagy a topológia.

Az Azure AD Connect telepítése előtt győződjön meg arról, hogy túl[töltse le az Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) és teljes hello előzetesen szükséges lépések [az Azure AD Connect: hardver és előfeltételek](active-directory-aadconnect-prerequisites.md). Emellett győződjön meg róla, hogy a rendelkezésre állnak az [Azure AD Connect-fiókok és -engedélyek](active-directory-aadconnect-accounts-permissions.md) szakaszban ismertetett fiókok.

Ha testre szabott beállításokat nem egyezik meg a topológiának, például a DirSync tooupgrade lásd [vonatkozó dokumentációját](#related-documentation) az egyéb forgatókönyvek.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Az Azure AD Connect telepítése egyéni beállításokkal
### <a name="express-settings"></a>Express Settings (Gyorsbeállítások)
Ezen a lapon kattintson a **Testreszabás** toostart egy testreszabott beállításokkal végzett telepítések.

### <a name="install-required-components"></a>Szükséges összetevők telepítése
Hello szinkronizálási szolgáltatások telepítésekor hagyhatja hello opcionális konfigurációs szakaszban nincs bejelölve, és az Azure AD Connect automatikusan beállítja az mindent. Azt állítja be egy SQL Server 2012 Express LocalDB példány hello megfelelő csoportok létrehozása és engedélyeket. Ha toochange hello alapértelmezett beállításokat, a következő tábla toounderstand hello opcionális konfigurációs lehetőségek állnak rendelkezésre hello is használhatja.

![Szükséges összetevők](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

| Választható konfiguráció | Leírás |
| --- | --- |
| Meglévő SQL Server használata |Lehetővé teszi az toospecify hello SQL Server nevét és hello példány neve. Válassza ezt a beállítást, ha már van egy adatbázis-kiszolgálót, hogy szeretné-e toouse. Adja meg egy vesszőt és a port számát a hello példánynév **példánynév** az SQL Server nincs engedélyezve a Tallózás. |
| Meglévő szolgáltatásfiók használata |Alapértelmezés szerint az Azure AD Connect használja a virtuális szolgáltatásfiók hello szinkronizálási szolgáltatások toouse. Ha távoli SQL-kiszolgálót vagy egy hitelesítést igénylő proxyt használ, meg kell-e toouse egy **felügyelt szolgáltatásfiók** vagy a szolgáltatás fiók használata hello a tartományban, és hello jelszó ismerete. Ezekben az esetekben adja meg a hello fiók toouse. Ellenőrizze, hogy hello telepítést futtató hello felhasználó rendszergazda az SQL, egy bejelentkezési hello szolgáltatásfiók hozható létre. További információ: [Azure AD Connect-fiókok és -engedélyek](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) |
| Egyéni szinkronizálási csoportok megadása |Alapértelmezés szerint az Azure AD Connect hello szinkronizálási szolgáltatások telepítésekor létrehoz négy csoportok helyi toohello kiszolgáló. Ezek a csoportok: a Rendszergazdák csoport, a felelősök, a Tallózás csoport és a hello jelszó alaphelyzetbe állítása csoport. Itt megadhatja a saját csoportjait. hello csoportok hello kiszolgálón kell elhelyezkednie, és nem található a hello tartomány. |

### <a name="user-sign-in"></a>Felhasználói bejelentkezés
A rendszer felkéri tooselect hello szükséges összetevők telepítése után a felhasználók egyszeri bejelentkezésének módját. a következő táblázat hello hello rendelkezésre álló lehetőségek rövid leírását tartalmazza. Teljes hello bejelentkezési módszer, olvassa el [felhasználói bejelentkezés](active-directory-aadconnect-user-signin.md).

![Felhasználói bejelentkezés](./media/active-directory-aadconnect-get-started-custom/usersignin2.png)

| Egyszeri bejelentkezési beállítás | Leírás |
| --- | --- |
| Jelszó-szinkronizálás |Felhasználók is képes toosign tooMicrosoft felhőszolgáltatások, például az Office 365, a használatával hello ugyanazt a jelszót a helyi hálózatban lévő használnak. hello felhasználók jelszavak szinkronizált tooAzure AD szolgáltatásba jelszókivonatként, és hello felhőben történik hitelesítés. További információkért lásd: [Jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md). |
|Átmenő hitelesítés (előzetes verzió)|Felhasználók is képes toosign tooMicrosoft felhőszolgáltatások, például az Office 365, a használatával hello ugyanazt a jelszót a helyi hálózatban lévő használnak.  hello felhasználók jelszó áthalad toohello a helyszíni Active Directory tartományvezérlő toobe érvényesítve.
| Összevonás az AD FS rendszerrel |Felhasználók is képes toosign tooMicrosoft felhőszolgáltatások, például az Office 365, a használatával hello ugyanazt a jelszót a helyi hálózatban lévő használnak.  a rendszer átirányítja a felhasználókat hello tootheir a helyszíni Active Directory összevonási szolgáltatások példány toosign a és a helyszínen történik hitelesítés. |
| Nincs konfigurálás |Egyik szolgáltatás sincs telepítve és konfigurálva. Válassza ezt a lehetőséget, ha már rendelkezik külső fél által biztosított összevonási kiszolgálóval vagy más meglévő megoldással. |
|Egyszeri bejelentkezés engedélyezése|Ezen beállítások érhető el a jelszó-szinkronizálás és az áteresztő hitelesítés és egyszeri bejelentkezési élményt nyújt asztali felhasználóknak hello a vállalati hálózaton.  További információkért tekintse meg az [egyszeri bejelentkezést](active-directory-aadconnect-sso.md) ismertető témakört. </br>Megjegyzés: az AD FS-ügyfelek ezt a beállítást nem érhető el, mert az AD FS már ajánlatok hello azonos szintű egyszeri bejelentkezés.</br>(ha ESP kiadása nem történik: hello ugyanannyi időt vesz igénybe)
|Bejelentkezési beállítás|Ezen beállítások jelszó-szinkronizálás ügyfelek esetén érhető el, és egy egyszeri bejelentkezési élményt nyújt asztali felhasználóknak hello a vállalati hálózaton.  </br>További információkért tekintse meg az [egyszeri bejelentkezést](active-directory-aadconnect-sso.md) ismertető témakört. </br>Megjegyzés: az AD FS-ügyfelek ezt a beállítást nem érhető el, mert az AD FS már ajánlatok hello azonos szintű egyszeri bejelentkezés.


### <a name="connect-tooazure-ad"></a>TooAzure AD Connect
Hello csatlakozás tooAzure AD képernyőn adjon meg egy globális rendszergazdai fiókot és jelszót. Ha a kiválasztott **az AD FS összevonási** hello előző lapon, nem írja alá be olyan fiókkal, egy tartományban az összevonáshoz tooenable tervezi. Azt ajánljuk, toouse hello alapértelmezett fiók **onmicrosoft.com** tartomány, amely az Azure AD-címtár.

Ez a fiók csak a felhasznált toocreate szolgáltatásfiók az Azure AD- és nem használt hello varázsló befejezése után.  
![Felhasználói bejelentkezés](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Ha a globális rendszergazdai fiókon engedélyezve van az MFA, majd kell tooprovide hello jelszót újra hello bejelentkezési előugró ablakban és teljes hello MFA-kérdést. hello kérdés lehet egy ellenőrző kódot vagy egy telefonhívás megadása.  
![Felhasználói bejelentkezési MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

hello globális rendszergazdai fiókkal is beállíthatja, hogy [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md) engedélyezve van.

Ha hibaüzenetet kap, és problémák adódnak a kapcsolódással, tekintse meg a [Troubleshoot connectivity problems](active-directory-aadconnect-troubleshoot-connectivity.md) (Kapcsolati problémák elhárítása) szakaszt.

## <a name="pages-under-hello-section-sync"></a>Hello szakaszban szinkronizálás lap

### <a name="connect-your-directories"></a>Csatlakoztassa a címtárakat
tooconnect tooyour Active Directory tartományi szolgáltatások, az Azure AD Connect kell hello erdő nevét és egy megfelelő engedélyekkel rendelkező fiók hitelesítő adatait.

![Címtár csatlakoztatása](./media/active-directory-aadconnect-get-started-custom/connectdir01.png)

Hello erdő nevének megadása, és kattintson a **könyvtár hozzáadása**, egy felugró párbeszédpanel megjelenik, és felajánlja az alábbi beállítások hello:

| Beállítás | Leírás |
| --- | --- |
| Meglévő fiók használata | Válassza ezt a beállítást, ha azt szeretné, hogy egy meglévő Active Directory tartományi szolgáltatások tooprovide fiók toobe csatlakozáshoz toohello AD-erdő címtár-szinkronizálás során az Azure AD Connect használt. Hello tartományrészt megadhatja NetBios vagy FQDN formátumban, azaz FABRIKAM\syncuser vagy fabrikam.com\syncuser. Ezt a fiókot lehet egy normál felhasználói fiókot, mert csak olvasási engedéllyel a hello alapértelmezett. A forgatókönyvtől függően azonban más engedélyekre is szüksége lehet. További információkért lásd: [Azure AD Connect-fiókok és -engedélyek](active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account). |
| Új fiók létrehozása | Válassza ezt a beállítást, ha azt szeretné, hogy az Azure AD Connect varázsló toocreate hello Active Directory tartományi szolgáltatások fiók az Azure AD Connect toohello AD-erdő kapcsolódás során a címtár-szinkronizálás szükséges. Ha ezt a lehetőséget választja, meg hello felhasználónevet és jelszót egy vállalati rendszergazdai fiók. hello megadott vállalati rendszergazdai fiók használható az Azure AD Connect varázsló toocreate hello AD DS-fiókjához szükséges. Hello tartományrészt megadhatja NetBios vagy FQDN formátumban, azaz Fabrikam endszergazda vagy Fabrikam.com endszergazda alakban. |

![Címtár csatlakoztatása](./media/active-directory-aadconnect-get-started-custom/connectdir02.png)


### <a name="azure-ad-sign-in-configuration"></a>Azure AD-bejelentkezés konfigurálása
Ez a lap lehetővé teszi a tooreview hello UPN tartományok a helyszíni Active Directory tartományi Szolgáltatásokban, és amely az Azure AD szolgáltatásban ellenőrzött. Ez a lap továbbá lehetővé teszi tooconfigure hello attribútum toouse hello userPrincipalName.

![Nem ellenőrzött tartományok](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Tekintse át az összes **Not Added** (Hozzá nem adott) és **Not Verified** (Nem ellenőrzött) megjelöléssel rendelkező tartományt. Bizonyosodjon meg róla, hogy az Ön által használt tartományok ellenőrizve lettek az Azure AD szolgáltatásban. Kattintson a hello frissítés jelre, miután ellenőrizte a tartományokat. További információkért lásd: [hello tartományok hozzáadásának és hitelesítésének](../active-directory-add-domain.md)

**UserPrincipalName** -hello attribútum userPrincipalName hello attribútum felhasználók használja, amikor bejelentkeznek a tooAzure AD és az Office 365. hello tartományok, más néven UPN-utótagként hello használt, az Azure AD hello felhasználók vannak szinkronizálása előtt ellenőrizni kell. A Microsoft azt javasolja, hogy a tookeep hello alapértelmezett attribútum userPrincipalName. Ha ez az attribútum nem átirányítható és nem lehet ellenőrizni, akkor lehetséges tooselect egy másik attribútumot. Például kiválaszthatja az e-mailek hello attribútumaként okozó hello bejelentkezési azonosítót. A userPrincipalName attribútumtól eltérő attribútumot más néven **Másik azonosítóként** ismerjük. hello másik azonosító értékének hello szabványos RFC822 kell követnie. A Másik azonosító a jelszó-szinkronizálással és az összevonással egyaránt használható.

>[!NOTE]
> Áteresztő hitelesítés engedélyezése esetén a rendelés toocontinue hello varázsló lépéseinek legalább egy ellenőrzött tartomány kell rendelkeznie.

> [!WARNING]
> A Másik azonosító használata nem kompatibilis az összes Office 365 számítási feladattal. További információkért tekintse meg túl[másodlagos bejelentkezési azonosító beállítása](https://technet.microsoft.com/library/dn659436.aspx).
>
>

### <a name="domain-and-ou-filtering"></a>Tartományok és szervezeti egységek szűrése
Alapértelmezés szerint minden tartomány és szervezeti egység szinkronizálva van. Ha vannak olyan tartományok és szervezeti egységek toosynchronize tooAzure AD meg szeretné akadályozni, törölheti ezek.  
![Domain/OU-szűrés](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png)  
Ezen a lapon hello varázslóban konfigurálja tartományi és OU-alapú szűrés. Ha azt tervezi, toomake módosításokat, majd tekintse meg [tartományalapú szűrés](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) és [ou-alapú szűrés](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) módosítások elvégzése előtt. Néhány szervezeti egységek hello funkció elengedhetetlen, és ne legyen bejelölve.

Ha a szervezeti egység szerinti szűrést az Azure AD Connect 1.1.524.0 előtti verziójával használja, a rendszer alapértelmezés szerint szinkronizálja a később hozzáadott új szervezeti egységeket. Hello viselkedését, hogy új ou-k nem fognak szinkronizálódni, majd konfigurálhatja azt, Miután befejeződött a hello varázsló [ou-alapú szűrés](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). Az Azure AD Connect verzió 1.1.524.0 vagy után adhatja meg, hogy kívánja-e új szervezeti egységek toobe szinkronizálva, vagy nem.

Ha azt tervezi, hogy toouse [csoport alapú szűrés](#sync-filtering-based-on-groups), majd ellenőrizze, hogy hello OU hello csoporttal tartalmazza, és nem alapján szűrt OU-szűrés. A rendszer a szervezeti egység szerinti szűrést a csoportalapú szűrés előtt végzi el.

Lehetőség arra is, hogy egyes tartományok nem érhetők el toofirewall korlátozásai miatt. Ezeknek a tartományoknak a kijelölése alapértelmezés szerint törölve van, és egy figyelmeztető jelöléssel rendelkeznek.  
![Nem elérhető tartományok](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Ha ilyen figyelmeztetés jelenik meg, győződjön meg arról, hogy ezek a tartományok valóban nem érhetőek el és hello figyelmeztetés jogos.

### <a name="uniquely-identifying-your-users"></a>Felhasználók egyedi azonosítása

#### <a name="select-how-users-should-be-identified-in-your-on-premises-directories"></a>Válassza ki, hogyan történjen a felhasználók azonosítása a helyszíni címtárakban
hello erdők közötti megfeleltetés szolgáltatás lehetővé teszi az Azure ad-ben az AD DS-erdőkből származó felhasználók ábrázolása toodefine. A felhasználók vagy egyszer szerepelnek az összes erdőben, vagy engedélyezett és letiltott fiókok kombinációjával rendelkeznek. hello felhasználói előfordulhat, hogy is lehet kapcsolattartóként egyes erdőkben találhatók.

![Egyedi](./media/active-directory-aadconnect-get-started-custom/unique.png)

| Beállítás | Leírás |
| --- | --- |
| [A felhasználók csak egyszer szerepelnek az összes erdőben](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Minden felhasználó külön objektumként van létrehozva az Azure AD szolgáltatásban. hello objektumok nincsenek összekapcsolva a metaverzumban hello. |
| [Mail attribútum](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Ez a beállítás összekapcsolja a felhasználók és névjegyek ugyanezt az értéket a különböző erdők hello hello levél attribútum-e. Használja ezt a beállítást, ha a kapcsolattartók a GALSync használatával lettek létrehozva. Ha ezt a lehetőséget választja, amelynek levél attribútum nincsenek feltöltve felhasználói objektumok nem lesznek szinkronizált tooAzure AD. |
| [ObjectSID és msExchangeMasterAccountSID/ msRTCSIP-OriginatorSid](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Ez a beállítás összeköt egy fiókerdőben található engedélyezett felhasználót egy erőforráserdőben található letiltott felhasználóval. Az Exchange ezt a konfigurációt csatolt postaládaként ismeri. Ezt a lehetőséget is használható, ha Lync csak használ, és az Exchange nincs jelen a hello erőforráserdőben. |
| sAMAccountName and MailNickName (sAMAccountName és MailNickName) |Ez a beállítás összeköt attribútumok, ahol várt hello bejelentkezési azonosító a hello felhasználói találhatók. |
| A specific attribute (Egy adott attribútum) |Ez a beállítás lehetővé teszi tooselect a saját attribútumát. Ha ezt a lehetőséget választja, amelyek (kijelölt) attribútuma nem feltöltve felhasználói objektumok nem lesznek szinkronizált tooAzure AD. **Korlátozás:** győződjön meg arról, hogy toopick olyan attribútum, amely már hello metaverzumban található. Ha egy egyedi attribútumot választ (nem a hello metaverzumba), hello varázsló nem tudja elvégezni. |

#### <a name="select-how-users-should-be-identified-with-azure-ad---source-anchor"></a>Válassza ki, hogyan történjen a felhasználók azonosítása az Azure AD – Source Anchor (Forráshorgony) használatával
hello sourceAnchor egy olyan attribútum, amely nem módosítható hello felhasználóobjektum élettartama során. Hello összekötő elsődleges kulcs hello felhasználói hello helyszíni felhasználót az Azure ad-ben.

| Beállítás | Leírás |
| --- | --- |
| Hogy az Azure a számomra hello forráshorgony kezelése | Válassza ezt a lehetőséget, ha meg szeretné az Azure AD toopick hello attribútum. Ha ezt a lehetőséget választja, az Azure AD Connect varázsló vonatkozik-e a hello sourceAnchor attribútum lemezválasztási logika cikk szakaszban leírt [az Azure AD Connect: tervezési alapelvek - msDS-ConsistencyGuid használata sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). hello varázsló tájékoztatja, hogy mely attribútum válogatott hello Forráshorgony attribútumaként egyéni telepítés befejezése után. |
| A specific attribute (Egy adott attribútum) | Válassza ezt a lehetőséget, ha egy meglévő AD attribútum hello sourceAnchor attribútumaként toospecify kívánja. |

Hello attribútum nem módosítható, mert figyelembe kell vennie a megfelelő attribútumot toouse. Egy jó jelölt az objectGUID. Ez az attribútum nem változik, hacsak hello felhasználói fiókot nem helyezi át az erdők/tartományok között. Egy Többerdős környezetben, ahol az erdők közötti áthelyezése fiókok egy másik attribútumot kell használni, például a hello employeeID attribútumot. Ne használjon olyan attribútumokat, amelyek változhatnak, ha a felhasználó megházasodik vagy más pozícióba kerül. Nem használhat olyan attribútumokat, amelyek tartalmazzák a @-sign jelet, így az e-mail-cím vagy a userPrincipalName nem használható. hello attribútum is kis-és nagybetűket, amikor egy objektum erdők között, győződjön meg arról, hogy toopreserve hello felső/nagybetűk. A bináris attribútumok base64-kódolásúak, az egyéb típusú attribútumok azonban megmaradnak kódolatlan állapotban. Összevonási forgatókönyvekben és néhány Azure AD felületen ez az attribútum immutableID néven ismert. További információ a hello forráshorgony megtalálható hello [tervezési alapelvek](active-directory-aadconnect-design-concepts.md#sourceanchor).

### <a name="sync-filtering-based-on-groups"></a>Szinkronizálás szűrése csoportok alapján
hello csoportra szűrés szolgáltatás lehetővé teszi toosync csak egy szűk részhalmaza próbaüzem esetén. toouse ezt a beállítást, hozzon létre egy csoportot e célból a helyszíni Active Directoryban. Majd adja hozzá a felhasználókat és csoportokat, amelyeket közvetlen tagként szinkronizált tooAzure AD. Később hozzáadhat, és távolítsa el a felhasználók toothis csoport toomaintain hello objektumok listáját, amelyeket az Azure AD szolgáltatásban jelen kell lennie. Toosynchronize kívánt összes objektum hello csoport közvetlen tagjának kell lennie. A felhasználóknak, csoportoknak, kapcsolattartóknak és számítógépeknek/eszközöknek mind közvetlen tagnak kell lenniük. A beágyazott csoporttagság feloldása nem lehetséges. Tag, kizárólag hello csoport jelenik meg, amikor felvesz egy csoport, a tagjai nem.

![Szinkronizálás szűrése](./media/active-directory-aadconnect-get-started-custom/filter2.png)

> [!WARNING]
> Ez a funkció csak a megfelelő toosupport a próbatelepítések. Ne használja éles környezetben üzemelő teljes körű példányok esetében.
>
>

Egy üzemelő teljes körű példányok éles környezetben azt lesz toobe rögzített toomaintain egy különálló csoportot az összes objektum toosynchronize. Ehelyett használja a hello módszerek [szűrés konfigurálása](active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Optional Features (Választható szolgáltatások)
A képernyő segítségével beállíthatja tooselect hello választható szolgáltatásokat az egyedi forgatókönyvekhez.

![Választható szolgáltatások](./media/active-directory-aadconnect-get-started-custom/optional.png)

> [!WARNING]
> Ha jelenleg a DirSync vagy az Azure AD Sync aktív, ne aktiválja az Azure AD Connectben hello visszaírási szolgáltatások egyikét sem.
>
>

| Optional Features (Választható szolgáltatások) | Leírás |
| --- | --- |
| Exchange Hybrid Deployment (Exchange hibrid telepítés) |hello Exchange hibrid telepítés lehetővé teszi hello az Exchange-postaládák párhuzamos tárolását a helyszíni és az Office 365-ben. Az Azure AD Connect visszaszinkronizálja az [attribútumok](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) egy adott halmazát az Azure AD szolgáltatásból a helyszíni címtárba. |
| Exchange Mail Public Folders (Exchange-levelezés – nyilvános mappák) | hello Exchange E-mail nyilvános mappáiban funkcióval toosynchronize a nyilvános mappa objektumok levelezési a helyszíni Active Directory tooAzure AD. |
| Azure AD app and attribute filtering (Azure AD alkalmazás- és attribútumszűrés) |Az Azure AD alkalmazás- és attribútumszűrés engedélyezése esetén hello szinkronizált attribútumok halmaza testreszabható. Ez a beállítás a konfigurációs lapok két további toohello varázsló hozzáadja. További információkért lásd: [Azure AD alkalmazás- és attribútumszűrés](#azure-ad-app-and-attribute-filtering). |
| Jelszó-szinkronizálás |Ha összevonási hello bejelentkezési megoldáshoz választott ki, majd engedélyezheti ezt a beállítást. A jelszó-szinkronizálás ezt követően használható másodlagos beállításként. További információkért lásd: [Jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md). </br></br>Ha átmenő hitelesítést választotta Ez a beállítás által alapértelmezés szerint tooensure támogatja a hagyományos, mint ez a beállítás engedélyezve van. További információkért lásd: [Jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md).|
| Jelszóvisszaíró |A jelszóvisszaírás engedélyezése esetén végrehajtott jelszómódosítások visszaíródnak az Azure AD vissza íródik tooyour helyszíni címtárba. További részletekért lásd: [A jelszókezelés első lépései](../active-directory-passwords-getting-started.md). |
| Group writeback (Csoportvisszaíró) |Ha hello **Office 365-csoportok** szolgáltatást, akkor ezeket a csoportokat szerepeltetheti a helyszíni Active Directoryban. Ez a beállítás kizárólag akkor elérhető, ha az Exchange jelen van a helyszíni Active Directory szolgáltatásban. További információkért lásd: [Csoportvisszaíró](active-directory-aadconnect-feature-preview.md#group-writeback). |
| Eszközvisszaíró |Lehetővé teszi toowriteback eszközobjektumot az Azure AD tooyour a helyszíni Active Directory feltételes hozzáférési forgatókönyvek. További információkért lásd: [Eszközvisszaírás engedélyezése az Azure AD Connectben](active-directory-aadconnect-feature-device-writeback.md). |
| Directory extension attribute sync (Címtárbővítmény-attribútumok szinkronizálása) |Azáltal, hogy a könyvtár bővítmények attribútumok szinkronizálása, a megadott attribútumok szinkronizált tooAzure AD. További információkért lásd: [Címtárbővítmények](active-directory-aadconnectsync-feature-directory-extensions.md). |

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD app and attribute filtering (Azure AD alkalmazás- és attribútumszűrés)
Ha azt szeretné, hogy mely attribútumok toosynchronize tooAzure AD, kezdetnek használ szolgáltatások toolimit. Konfigurációmódosításokat végez ezen a lapon, ha egy új szolgáltatás a kijelölt explicit módon hello telepítővarázsló újrafuttatásával toobe rendelkezik.

![Választható szolgáltatások – Alkalmazások](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Hello előző lépésben kiválasztott hello szolgáltatások alapján, ezen a lapon látható minden szinkronizált attribútumok. Ez a lista az összes épp szinkronizált objektumtípus összesítése. Ha bizonyos attribútumokat toonot kell szinkronizálni, akkor törölje azok jelölését.

![Választható szolgáltatások – Attribútumok](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

> [!WARNING]
> Az attribútumok eltávolítása hatással lehet a funkcionalitásra. Az ajánlott eljárásokért és javaslatokért lásd: [szinkronizált attribútumok](active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).
>
>

### <a name="directory-extension-attribute-sync"></a>Címtárbővítmény-attribútumok szinkronizálása
Az Azure AD hello sémát kiterjesztheti a szervezet vagy más attribútumok az Active Directory által hozzáadott egyedi attribútumokkal. toouse Ez a szolgáltatás, válassza **Directory Címtárbővítmény-attribútumok szinkronizálása** a hello **választható szolgáltatások** lap. További attribútumok toosync ezen a lapon választhatja ki.

![Címtárbővítmények](./media/active-directory-aadconnect-get-started-custom/extension2.png)

További információkért lásd: [Címtárbővítmények](active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="enabling-single-sign-on-sso"></a>Egyszeri bejelentkezés (SSO) engedélyezése
Egy egyszerű folyamat, hogy elegendő toocomplete egyszer minden erdőre vonatkozik, folyamatban van a szinkronizált tooAzure AD konfigurálása egyszeri bejelentkezéshez használható a jelszó-szinkronizálás és az áteresztő hitelesítés. A konfiguráció az alábbi két lépésből áll:

1.  A helyszíni Active Directoryban hello szükséges számítógépfiókjának létrehozására.
2.  Hello intranet zóna hello ügyfél gépek toosupport egyszeri bejelentkezés beállítása.

#### <a name="create-hello-computer-account-in-active-directory"></a>Az Active Directory hello számítógépfiók létrehozása
Az egyes erdőkhöz, amely az Azure AD Connect lett hozzáadva szüksége lesz toosupply tartományi rendszergazda hitelesítő adatait, hogy hello számítógépfiók minden erdő hozhatók létre. hello hitelesítő adatok csak a felhasznált toocreate hello fiók és nem tárolja vagy bármely más művelethez használt. Egyszerűen adja hozzá a hello hitelesítő adatokat a hello **engedélyezése egyszeri bejelentkezéskor** hello Azure AD Connect varázsló látható módon:

![Egyszeri bejelentkezés engedélyezése](./media/active-directory-aadconnect-get-started-custom/enablesso.png)

>[!NOTE]
>Egy adott erdőben kihagyhatja, ha nem kíván toouse az egyszeri bejelentkezés az adott erdőben.

#### <a name="configure-hello-intranet-zone-for-client-machines"></a>Ügyfélszámítógépeknél hello Intranet zóna konfigurálása
hello ügyfél bejelentkezések automatikusan a hello intranet zóna, tooensure kell tooensure, hogy két URL-címek hello intranetes zóna részét képezik. Ez biztosítja, hogy hello tartományhoz csatlakoztatott számítógépen automatikusan elküldi a Kerberos jegy tooAzure AD a vállalati hálózathoz csatlakoztatott toohello.
Hello csoportházirend felügyeleti eszközökkel rendelkező számítógépen.

1.  Nyissa meg a hello csoportházirend-kezelő eszközök
2.  Alkalmazott tooall felhasználók lesz hello csoportházirend szerkesztése. Például hello alapértelmezett tartományi házirendé.
3.  Keresse meg a túl**Felhasználó konfigurációja\Felügyeleti sablonok\Windows összetevők\Internet Explorer\Internet vezérlő Panel\Security lap** válassza **hely tooZone társításának listája** / hello kép alább.
4.  Hello házirend engedélyezése, és írja be a következő két elem hello párbeszédpanelen hello.

        Value: `https://autologon.microsoftazuread-sso.com`  
        Data: 1  
        Value: `https://aadg.windows.net.nsatc.net`  
        Data: 1

5.  Az alábbihoz hasonló toohello következő:  
![Intranet zónák](./media/active-directory-aadconnect-get-started-custom/sitezone.png)

6.  Kattintson kétszer az **OK** gombra.

## <a name="configuring-federation-with-ad-fs"></a>AD FS-összevonás konfigurálása
Az AD FS konfigurálása az Azure AD Connecttel egyszerű feladat, amely mindössze néhány kattintást igényel. hello következő hello konfigurálása előtt meg kell adni.

* A Windows Server 2012 R2 server hello összevonási kiszolgáló távoli felügyelet engedélyezve van
* A Windows Server 2012 R2 server hello webalkalmazás-Proxy kiszolgáló távoli felügyelet engedélyezve van
* Egy SSL-tanúsítványa hello összevonási szolgáltatás neve szeretné toouse (például sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>Az AD FS konfigurálásának előfeltételei
tooconfigure az AD FS farm Azure AD Connect használatával, győződjön meg arról, WinRM engedélyezve van a távoli kiszolgálókon hello. Ezenkívül halad át hello portok követelmény felsorolt [3. táblázat – Azure AD Connect és az összevonási kiszolgálók/WAP](active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-ad-fs-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Új AD FS farm létrehozása vagy meglévő AD FS farm használata
Használhatja a meglévő AD FS-farm, vagy választhat toocreate egy új AD FS-farm. Ha úgy dönt, toocreate egy új, szükséges tooprovide hello SSL-tanúsítvány is. Ha hello SSL-tanúsítvány jelszóval védett, hello jelszót kéri.

![AD FS farm](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Ha úgy dönt, toouse meglévő AD FS-farm, megnyílik közvetlenül toohello közötti AD FS és az Azure AD képernyő hello megbízhatósági kapcsolat konfigurálása.

### <a name="specify-hello-ad-fs-servers"></a>Hello AD FS kiszolgálók megadása
Adja meg a hello kívánt kiszolgálókat AD FS tooinstall meg. Egy vagy több kiszolgálót is hozzáadhat kapacitástervezési igényeitől függően. Minden kiszolgálók tooActive Directory JOIN, mielőtt elvégezné ezt a konfigurációt. A Microsoft a teszt- és próbatelepítésekhez egyetlen AD FS-kiszolgáló üzembe helyezését javasolja. Majd adja hozzá, és a skálázási igények további kiszolgálók toomeet telepítése az Azure AD Connect futtassa újra a kezdeti konfigurálása után.

> [!NOTE]
> Gondoskodjon arról, hogy a kiszolgálók illesztett tooan AD-tartomány ahhoz a konfiguráció végrehajtását.
>
>

![AD FS-kiszolgálók](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-hello-web-application-proxy-servers"></a>Hello webalkalmazás-Proxy kiszolgálók megadása
Adja meg a webalkalmazás-proxy kiszolgálóként használni kívánt hello kiszolgálón. hello webalkalmazás-proxy kiszolgáló üzembe van helyezve a Szegélyhálózaton (az extranet oldalon), és hello extranetről érkező hitelesítési kéréseket támogatja. Egy vagy több kiszolgálót is hozzáadhat kapacitástervezési igényeitől függően. A Microsoft a teszt- és próbatelepítésekhez egyetlen webalkalmazás-proxykiszolgáló üzembe helyezését javasolja. Majd adja hozzá, és a skálázási igények további kiszolgálók toomeet telepítése az Azure AD Connect futtassa újra a kezdeti konfigurálása után. Javasoljuk, kiszolgálók toosatisfy proxyhitelesítés hello intranetről azonos számú.

> [!NOTE]
> <li> Ha hello használt fiók nem egy helyi rendszergazdai hello AD FS-kiszolgálókon, majd kéri rendszergazdai hitelesítő adatokat.</li>
> <li> Győződjön meg arról, hogy nincs HTTP/HTTPS kapcsolat hello Azure AD Connect-kiszolgáló és a hello webalkalmazás-Proxy kiszolgáló közt Ez a lépés futtatása előtt.</li>
> <li> Győződjön meg arról, hogy van-e HTTP/HTTPS kapcsolat hello webalkalmazás-kiszolgáló és a hello AD FS kiszolgáló tooallow hitelesítési kérelmek tooflow keresztül között.</li>
>

![Webalkalmazás](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Rákérdezéses tooenter hitelesítő adatokat, hogy hello webalkalmazás-kiszolgáló létrehozhasson egy biztonságos kapcsolatot toohello AD FS-kiszolgáló áll. Ezeket a hitelesítő adatokat kell toobe hello AD FS-kiszolgáló helyi rendszergazdája.

![Proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-hello-service-account-for-hello-ad-fs-service"></a>Adja meg az AD FS szolgáltatás hello hello szolgáltatásfiókot
hello AD FS szolgáltatással szükséges egy tartományi fiók tooauthenticate felhasználók és a felhasználók adatainak kikereséséhez az Active Directory szolgáltatás. A szolgáltatásfiókok két típusát tudja támogatni:

* **Csoportosan felügyelt szolgáltatásfiók** – Ez az Active Directory tartományi szolgáltatásokban a Windows Server 2012 kiadásával lett bevezetve. Ilyen típusú fiókok szolgáltatásokat, például az AD FS számára egy olyan fiók tooupdate hello fiók jelszava rendszeresen anélkül biztosít. Használja ezt a beállítást, ha már rendelkezik Windows Server 2012 rendszerű tartományvezérlők hello tartomány, amely az AD FS-kiszolgálók tartoznak.
* **Tartományi felhasználói fiók** -ilyen típusú fiókok meg tooprovide jelszót, és rendszeresen hello jelszófrissítési amikor hello változik vagy lejár. Használja ezt a beállítást csak akkor, ha nincs Windows Server 2012 rendszerű tartományvezérlők hello olyan tartományban, az AD FS-kiszolgálók tartoznak.

Amennyiben a Csoportosan felügyelt szolgáltatásfiók lehetőséget választotta, és ez a szolgáltatás még nem volt használva az Active Directoryban, a rendszer vállalati rendszergazdai hitelesítő adatokat kér. Ezeket a hitelesítő adatokat használt tooinitiate hello tárolt kulcs és az Active Directory hello szolgáltatás engedélyezése.

> [!NOTE]
> Az Azure AD Connect hajt végre egy ellenőrzés toodetect, ha a hello AD FS szolgáltatás már regisztrálva van, egy SPN hello tartományban.  Active Directory tartományi szolgáltatások nem teszi lehetővé ismétlődő SPN toobe regisztrált egyszerre.  Ha talál ismétlődő SPN, csak akkor tudja tooproceed további hello SPN eltávolításáig.

![AD FS-szolgáltatásfiók](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-hello-azure-ad-domain-that-you-wish-toofederate"></a>Válassza ki, hogy kívánja-e toofederate hello Azure AD-tartományt
Ez a konfiguráció használt toosetup hello a az AD FS és az Azure AD közti összevonási kapcsolat. Konfigurálja az AD FS tooissue biztonsági jogkivonatokat tooAzure AD, és konfigurálja az Azure AD tootrust hello jogkivonatok az adott AD FS-példányból. Ezen a lapon csak lehetővé teszi a kezdeti telepítés hello egyetlen tartományt tooconfigure. További kiszolgálókat később, az Azure AD Connect ismételt futtatásával konfigurálhat.

![Azure AD-tartomány](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-hello-azure-ad-domain-selected-for-federation"></a>Az összevonáshoz kiválasztott hello Azure AD-tartomány ellenőrzése
Ha összevont hello tartomány toobe választja, az Azure AD Connect biztosít szükséges információkat tooverify nem ellenőrzött tartományt. Lásd: [hozzáadása, és ellenőrizze a tartomány hello](../active-directory-add-domain.md) arról, hogyan toouse ezt az információt.

![Azure AD-tartomány](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

> [!NOTE]
> AD Connect záma tooverify hello tartományi során hello konfigurálása szakaszban. Ha tooconfigure hello szükséges DNS-rekordok hozzáadása nélkül folytatja, hello varázsló beállítás nem tud toocomplete hello.
>
>

## <a name="configure-and-verify-pages"></a>Configure (Konfigurálás) és Verify (Ellenőrzés) lap
hello konfigurálása ezen az oldalon történik.

> [!NOTE]
> Mielőtt folytatná a telepítést, ha konfigurálta az összevonást, ellenőrizze, hogy konfigurálta az [összevonási kiszolgálók névfeloldását is](active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).
>
>

![Készen áll a tooconfigure](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Átmeneti mód
Lehetséges toosetup egy új szinkronizálási kiszolgálót is átmeneti móddal párhuzamosan is. Csak támogatott toohave egyetlen szinkronizálási kiszolgálóról exportálása tooone directory hello felhőben. Azonban ha azt szeretné, hogy egy másik kiszolgáló, például egy futó DirSync, toomove, engedélyezheti az Azure AD Connectet átmeneti módban. Ha engedélyezve van, hello szinkronizálási motor importálása, és szinkronizálja az adatokat a szokásos módon, de nem exportál semmit tooAzure AD vagy ad szolgáltatásokba. hello szolgáltatások jelszó-szinkronizálás és jelszóvisszaíró átmeneti módban le vannak tiltva.

![Átmeneti mód](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

Átmeneti módban lehetséges toomake a szükséges változtatásokat toohello szinkronizálási motor, és áttekintheti az exportált toobe kapcsolatban. Amikor hello konfiguráció megfelelőnek tűnik, futtassa újra a hello telepítővarázslóját, és átmeneti környezetű üzemmód letiltása. Adatok mostantól exportált tooAzure AD erről a kiszolgálóról. Győződjön meg arról, hogy toodisable hello azonos idő ezért csak egy kiszolgáló exportáljon aktívan hello más kiszolgáló.

További információkért lásd: [Átmeneti mód](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Összevonási konfiguráció ellenőrzése
Az Azure AD Connect hello DNS-beállítások, ellenőrzi a hello ellenőrzése gombra kattintva.

**Intranetkapcsolat ellenőrzése**

* Összevonási FQDN feloldása: hello összevonási FQDN megoldhatók DNS tooensure kapcsolat az Azure AD Connect ellenőrzi. Az Azure AD Connect hello FQDN nem oldható fel, ha hello ellenőrzése sikertelen lesz. Gondoskodjon arról, hogy a DNS-rekord hello összevonási szolgáltatás teljes Tartományneve rendelés toosuccessfully teljes hello ellenőrzése.
* DNS A-rekord: Az Azure AD Connect ellenőrzi, hogy az összevonási szolgáltatáshoz létezik-e egy A-rekord. Az A rekord hello hiányában hello ellenőrzés sikertelen lesz. Hozzon létre egy A-CNAME rekord a összevonási FQDN a rendelés toosuccessfully hello igazolásához.

**Extranet kapcsolat ellenőrzése**

* Összevonási FQDN feloldása: hello összevonási FQDN megoldhatók DNS tooensure kapcsolat az Azure AD Connect ellenőrzi.

![Befejezve](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Ellenőrzés](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Ezenkívül hajtsa végre a következő témakörben leírt ellenőrző lépéseket hello:

* Ellenőrizze, hogy a tartományhoz csatlakozó gépről az intraneten hello böngészőből bejelentkezhet: csatlakozás toohttps://myapps.microsoft.com és hello jelentkezzen be a bejelentkezett fiók ellenőrzése. hello beépített Active Directory Tartományi rendszergazdai fiók nincs szinkronizálva, és nem használható az ellenőrzéshez.
* Ellenőrizze, hogy az eszközön az extranetes hello bejelentkezhet. Egy otthoni gépet vagy egy mobileszközön toohttps://myapps.microsoft.com csatlakozzon, és a hitelesítő adatok megadását.
* Ellenőrizze a gazdag ügyféllel való bejelentkezést. Toohttps://testconnectivity.microsoft.com csatlakozni, válassza a hello **Office 365** lapra, és válassza hello **Office 365 egyszeri bejelentkezés tesztelése**.

## <a name="next-steps"></a>Következő lépések
Miután hello telepítés befejeződött, jelentkezzen ki, és jelentkezzen be újra tooWindows, mielőtt a Synchronization Service Managert vagy a szinkronizálási szabály szerkesztő.

Most, hogy az Azure AD Connect telepítése is [hello telepítésének ellenőrzése és licencek hozzárendelése](active-directory-aadconnect-whats-next.md).

További információk, amelyek hello telepítéssel engedélyezett szolgáltatásokkal: [véletlen törlések megakadályozása](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) és [az Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Tudjon meg többet a következő általános témaköröket: [Feladatütemező, és hogyan tootrigger szinkronizálása](active-directory-aadconnectsync-feature-scheduler.md).

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

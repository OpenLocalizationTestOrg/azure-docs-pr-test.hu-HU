---
title: "Az Azure AD Connect: Támogatott topológiák |} Microsoft Docs"
description: "Ez a témakör a támogatott és nem támogatott topológiák részletezi az Azure AD Connect"
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 41632a54e8e85492fbf1a751ef4e618c8870abe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="topologies-for-azure-ad-connect"></a>Azure AD Connect-topológiák
Ez a cikk ismerteti a különböző helyszíni és az Azure AD Connect szinkronizálási szolgáltatás hello kulcs integrációs megoldásként használó Azure Active Directory (Azure AD) topológiákat. Ez a cikk egyaránt támogatott, és nem támogatott konfigurációkat tartalmazza.

Hello jelmagyarázat hello cikkben képek itt található:

| Leírás | Szimbólum |
| --- | --- |
| A helyszíni Active Directory-erdő |![A helyszíni Active Directory-erdő](./media/active-directory-aadconnect-topologies/LegendAD1.png) |
| A helyszíni Active Directory szűrt importálás |![Szűrt importálás Active Directory](./media/active-directory-aadconnect-topologies/LegendAD2.png) |
| Az Azure AD Connect sync-kiszolgáló |![Az Azure AD Connect sync-kiszolgáló](./media/active-directory-aadconnect-topologies/LegendSync1.png) |
| Az Azure AD Connect szinkronizálási kiszolgálót "átmeneti módban" |![Az Azure AD Connect szinkronizálási kiszolgálót "átmeneti módban"](./media/active-directory-aadconnect-topologies/LegendSync2.png) |
| A Forefront Identity Manager (FIM) 2010 vagy a Microsoft Identity Manager (MIM) 2016 GALSync |![A FIM 2010 vagy a MIM 2016 GALSync](./media/active-directory-aadconnect-topologies/LegendSync3.png) |
| Az Azure AD Connect szinkronizálási kiszolgálót, részletes |![Az Azure AD Connect szinkronizálási kiszolgálót, részletes](./media/active-directory-aadconnect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/active-directory-aadconnect-topologies/LegendAAD.png) |
| A forgatókönyv nem támogatott |![A forgatókönyv nem támogatott](./media/active-directory-aadconnect-topologies/LegendUnsupported.png) |

## <a name="single-forest-single-azure-ad-tenant"></a>Egyetlen erdő, egyetlen Azure AD-bérlő
![Egyetlen erdő esetén, és egyetlen bérlő topológia](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

hello leggyakrabban használt topológia egy helyi erdő, egy vagy több tartományt, és az egy egyetlen Azure AD bérlői. Az Azure AD-alapú hitelesítés a jelszó-szinkronizálás használatos. az Azure AD Connect hello gyorstelepítés csak ez a topológia támogatja.

### <a name="single-forest-multiple-sync-servers-tooone-azure-ad-tenant"></a>Egyetlen erdő, több szinkronizálási kiszolgálók tooone az Azure AD bérlő
![Egyetlen erdő esetén nem támogatott, a szűrt topológia](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Ha több Azure AD Connect szinkronizálási kiszolgálót is csatlakoztatott toohello ugyanazt az Azure AD bérlő nem támogatott, kivéve a [kiszolgáló átmeneti](#staging-server). Ez akkor is, ha ezek a kiszolgálók kölcsönösen kizárják egymást az objektumok készletével konfigurált toosynchronize nem támogatott. Akkor lehet, hogy tekintette ebben a topológiában Ha nem érhető el egy kiszolgálóról hello erdőben lévő összes tartományban, vagy ha több kiszolgáló között szeretne toodistribute betöltése.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Több erdő, egyetlen Azure AD-bérlő
![Több erdő és a egybérlős topológia](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Számos szervezet környezetekben a helyszíni Active Directory több erdővel rendelkezik. Nincsenek egynél több a helyszíni Active Directory-erdő rendelkező különböző okait. Tipikus többek között a fiók-erőforrás-erdők és hello eredménye egy összeolvadást vagy felvásárlást tervek.

Ha rendelkezik több erdő, az összes erdőben egyetlen elérhetőnek kell lennie az Azure AD Connect szinkronizálási kiszolgálót. Nincs toojoin hello tooa tartományában. Ha szükséges tooreach összes erdőben, hová kívánja helyezni a hello kiszolgálót a szegélyhálózaton (is néven DMZ demilitarizált zóna, és végezheti).

hello Azure AD Connect telepítővarázsló jelennek meg több erdő több beállítások tooconsolidate felhasználók kínál. hello célja, hogy a felhasználó Azure AD-ben csak egyszer képviseli. Nincsenek néhány gyakori topológiák hello egyéni telepítési útvonal hello telepítése varázslóban konfigurálható. A hello **felhasználók egyedi azonosítása** lapon, a topológia jelölő válassza hello a megfelelő beállítást. hello összevonása konfigurálta csak a felhasználók számára. Duplikált csoportok nem kell összevonni hello alapértelmezett konfigurációval.

Közös topológiák hello szakaszok esik szó kapcsolatos [topológiák külön](#multiple-forests-separate-topologies), [háló teljes](#multiple-forests-full-mesh-with-optional-galsync), és [fiók-erőforrás topológia hello](#multiple-forests-account-resource-forest).

feltételezi hello alapértelmezett konfiguráció a Azure AD Connect szinkronizálása:

* Minden felhasználó csak egy engedélyezett fiók rendelkezik, és használt tooauthenticate hello felhasználói hello erdőben, ahol ennek a fióknak-e. Ez feltételezi, a jelszó-szinkronizálás és a összevonási. UserPrincipalName és sourceAnchor/immutableID határozza meg az erdő.
* Minden felhasználónak csak egy postaláda van.
* egy felhasználó hello postaláda üzemeltető hello erdő hello ajánlott az adatminőségi attribútumok látható hello Exchange globális cím lista (GAL) rendelkezik. Ha nincs postaláda hello felhasználó, bármely erdőben lehet használt toocontribute ezek attribútumértékek.
* Ha rendelkezhetnek hivatkozott postafiókkal, akkor is fiók a bejelentkezéshez használt egy másik erdőben.

Ha a környezet nem egyezik meg az ilyen Előfeltevések, hello következő dolgokat okozhat:

* Ha egynél több aktív fiók vagy egynél több postaláda, hello szinkronizálási motor választja ki az egyik, és figyelmen kívül hagyja az egyéb hello.
* Nincs más aktív fiókkal hivatkozott postafiókkal nincs exportált tooAzure AD. hello felhasználói fiók nem szerepel minden csoport tagjaként. A DirSync hivatkozott postafiókkal mindig a normál postaláda jelzi. Ez a változás szándékosan egy másik viselkedés toobetter támogatási több erdőből példák.

További részletek találhatók [ismertetése hello alapértelmezett konfiguráció](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-tooone-azure-ad-tenant"></a>Több erdő, több szinkronizálási kiszolgálók tooone az Azure AD bérlő
![Több erdő és a több szinkronizálási kiszolgálót nem támogatott topológia](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Ha egynél több Azure AD Connect szinkronizálási kiszolgáló csatlakozott egy Azure AD-bérlő tooa nem támogatott. hello kivétel: hello használata egy [kiszolgáló átmeneti](#staging-server).

### <a name="multiple-forests-separate-topologies"></a>Több erdő, külön topológiák
![A felhasználók csak egyszer képviselő címtárak beállítás](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Több erdővel és külön topológiák ábrázolása](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

Ebben a környezetben az összes helyi erdőben külön entitásokként kezelik. Felhasználó nem szerepel a más erdőkben. Minden olyan erdőben, a saját Exchange-szervezet rendelkezik, és nem GALSync hello erdők között van. Ez a topológia lehet hello helyzet egy összeolvadást vagy felvásárlást követően, vagy olyan szervezetekben, ahol minden részleg egymástól függetlenül működik. Erdők az Azure AD-szervezethez hello és egy egységes globális Címlista együtt jelennek meg. A kép megelőző hello minden erdőben az egyes objektumok képviselt egyszer hello metaverse és hello cél az Azure AD bérlő összesíteni.

### <a name="multiple-forests-match-users"></a>Több erdő: felhasználók kereséséhez
Közös tooall forgatókönyvekben, hogy a terjesztési és biztonsági csoportok felhasználókat, névjegyeket és idegen rendszerbiztonsági (FSP) tartalmazhat. Active Directory tartományi szolgáltatások (AD DS) toorepresent tagok biztonsági csoportban lévő más erdőkből származó FSP használnak. Minden FSP megoldott toohello valódi objektum az Azure ad-ben.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Több erdő: opcionális GALSync ellátott teljes
![Az egyező, amikor a felhasználók identitásai több címtárban is léteznek hello levél attribútum használatára vonatkozó beállítás](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Teljes rácsot alkotó topológia több erdő](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

A teljes rácsot alkotó topológia lehetővé teszi a felhasználók és erőforrások toobe bármely erdőben található. Gyakran nincsenek kétirányú bizalmi kapcsolatok hello erdők között.

Ha Exchange megtalálható-e egynél több erdő, valószínűleg (opcionális) egy helyszíni GALSync megoldás. Minden felhasználó van majd kapcsolattartóként az összes többi erdője. FIM 2010 vagy a MIM 2016 segítségével gyakran használják a GALSync. Az Azure AD Connect helyszíni GALSync nem használható.

Ebben a forgatókönyvben identitásobjektumok csatlakozott hello levél attribútum használatával. Hello hello partnerek kapcsolódik egy postaládát egy erdő rendelkező felhasználó más erdőkben.

### <a name="multiple-forests-account-resource-forest"></a>Több erdő: fiók erőforráserdőben
![Amikor identitásai több címtárban is léteznek egyeztetéséhez hello ObjectSID és msExchMasterAccountSID attribútum használatára vonatkozó beállítás](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Fiók-erőforrás szolgának több erdő](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

Egy fiók-erőforrás szolgának, fel kell egy vagy több *fiók* erdőkbe tartozó aktív felhasználói fiókok. Akkor is rendelkezik, egy vagy több *erőforrás* erdők letiltott fiókok.

Ebben a forgatókönyvben egy (vagy több) erőforrás-erdő megbízhatónak tekinti minden fiókerdőben. hello Erőforráserdő jellemzően az Exchange és a Lync kiterjesztett Active Directory-sémát rendelkezik. Exchange és a Lync-szolgáltatások más megosztott szolgáltatások, valamint az erdőben találhatók. Felhasználók letiltott felhasználói fiók rendelkezik az erdőben, és hello postaláda kapcsolódó toohello fiókerdő.

## <a name="office-365-and-topology-considerations"></a>Az Office 365 és topológiai szempontok
Néhány Office 365 számítási feladattal bizonyos korlátozások a támogatott topológiák rendelkezik:

| Számítási feladat | Korlátozások |
--------- | ---------
| Exchange Online | Ha egynél több helyszíni Exchange-szervezet (Ez azt jelenti, hogy Exchange már telepített toomore, mint egy erdő), használnia kell az Exchange 2013 SP1 vagy újabb. További információkért lásd: [hibrid telepítések esetén több Active Directory-erdő](https://technet.microsoft.com/library/jj873754.aspx). |
| A Skype vállalati verzió | Több helyszíni erdővel használata esetén csak hello fiók-erőforrás szolgának használata támogatott. További információkért lásd: [környezeti követelményei a Skype vállalati Server 2015](https://technet.microsoft.com/library/dn933910.aspx). |


## <a name="staging-server"></a>Átmeneti kiszolgáló
![Átmeneti kiszolgálói topológia](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

A második kiszolgáló telepítése az Azure AD Connect támogatja *átmeneti módban*. Ebben a módban egy kiszolgáló olvassa be az adatokat az összes csatlakoztatott könyvtárak, de nem írja semmit tooconnected könyvtárak. Hello normál szinkronizálási ciklust használ, és ezért a hello identitásadatok frissített másolatát is.

Egy olyan vészhelyzet esetén, ahol a hello elsődleges kiszolgáló meghibásodása esetén, a kiszolgáló átmeneti toohello átveheti. Ehhez a hello Azure AD Connect varázsló. A második kiszolgáló is található egy ugyanabban az adatközpontban, mert meg van osztva infrastruktúrával nem rendelkező hello elsődleges kiszolgáló. A konfigurációs változásokat kiszolgálón végzett hello elsődleges kiszolgáló toohello második manuálisan át kell másolnia.

Használhat egy átmeneti kiszolgálón tootest egy új egyéni konfigurációs és hello azt gyakorolt hatása az adatokat. Hello módosításokat megtekintheti és hello konfigurációs beállítása. Ha már elégedett hello új konfigurációt, teheti a kiszolgáló hello aktív kiszolgáló átmeneti hello és hello régi aktív kiszolgáló toostaging üzemmód beállítása.

A metódus tooreplace hello aktív szinkronizálási kiszolgálót is használható. Készítsen elő hello új kiszolgálót, és toostaging üzemmód beállítása. Ellenőrizze, hogy jó állapotban, tiltsa le a átmeneti módban (aktiválás), és állítsa le a jelenleg aktív kiszolgálói hello.

Ha azt szeretné, toohave különböző adatközpontokban több biztonsági mentés lehetséges toohave több mint egy átmeneti kiszolgálón is.

## <a name="multiple-azure-ad-tenants"></a>Több Azure AD-bérlő
Javasoljuk, egyetlen bérlő szervezet Azure AD-ben.
Tervezi toouse több Azure AD-bérlőkkel, mielőtt cikke hello [az adminisztrációs egységek felügyelete az Azure AD](../active-directory-administrative-units-management.md). Gyakori forgatókönyvek, ahol ugyanúgy használhatók egy egybérlős magában foglalja.

![Több erdő és a több bérlő topológia](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Az Azure AD Connect szinkronizálási kiszolgálót és az Azure AD-bérlő közötti 1:1 kapcsolat van. Minden Azure AD-bérlő van szüksége egy Azure AD Connect szinkronizálási kiszolgáló telepítés. hello Azure AD-bérlő példányban vannak elszigetelt úgy lett kialakítva. Ez azt jelenti, hogy egy bérlő nem látnak hello felhasználók más bérlővel. Ha azt szeretné, hogy ez az elkülönítés, ez a beállítás támogatott. Ellenkező esetben használjon hello egyetlen Azure AD bérlő modell.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Az egyes objektumok csak egyszer az Azure AD-bérlő
![Egyetlen erdő esetén szűrt topológia](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

Ebben a topológiában egy Azure AD Connect szinkronizálási kiszolgálót az csatlakoztatott tooeach az Azure AD-bérlő. hello Azure AD Connect szinkronizálási kiszolgálók be kell állítani a szűréshez, hogy az egyes objektumok toooperate egymást kölcsönösen kizáró készletét. Hatókörét megadhatja a, például minden kiszolgáló tooa az egyes tartományokhoz vagy szervezeti egység.

A DNS-tartomány regisztrálni lehet egyetlen Azure AD-bérlő. hello UPN-EK hello felhasználók hello a helyszíni Active Directory-példányban is használnia kell a különálló névtereket. Például a fenti kép hello, három különálló UPN-utótagot regisztrált hello a helyszíni Active Directory-példányban: contoso.com, fabrikam.com és wingtiptoys.com. hello felhasználók minden a helyszíni Active Directory tartományban használja egy másik névtérből.

Nincs nincs GALSync hello Azure AD bérlő példányai között. hello címjegyzék az Exchange Online és Skype vállalati mutat be a csak a felhasználók hello ugyanannak a bérlőnek.

Ez a topológia rendelkezik hello következő egyéb korlátozások a támogatott forgatókönyveket:

* Hello Azure AD bérlők csak az egyik engedélyezheti az Exchange hibrid hello a helyszíni Active Directory-példánnyal.
* Windows 10-eszközöket csak egy Azure AD-bérlővel társítható.
* csak egy Azure AD-bérlő hello egyszeri bejelentkezés (SSO) lehetőséget a jelszó-szinkronizálás és az áteresztő hitelesítés használható.

az objektumok egymást kölcsönösen kizáró készletének hello követelmény is vonatkozik toowriteback. Néhány visszaíró szolgáltatását nem támogatottak a topológia, mert azok feltételezik, hogy egyetlen helyszíni konfigurációt. Ezek a funkciók a következők:

* Csoport visszaírási alapértelmezett konfigurációval.
* Eszközök visszaírásához.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Az egyes objektumok többször az Azure AD-bérlő
![Egyetlen erdő esetén, és több bérlő nem támogatott topológia](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Egyetlen erdő esetén, és több összekötő nem támogatott topológia](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

Ezek a feladatok nem támogatottak:

* Szinkronizálási hello azonos felhasználói toomultiple az Azure AD bérlők számára.
* Olyan konfigurációs módosítást, hogy a felhasználók számára egy Azure AD-bérlő frissítésként jelenik meg egy másik Azure AD-bérlő partnerek ellenőrizze.
* Az Azure AD Connect szinkronizálási tooconnect toomultiple az Azure AD-bérlő módosítása.

### <a name="galsync-by-using-writeback"></a>A visszaírási GALSync
![Több erdő és a több-címtárak esetén az Azure ad-val előtérbe GALSync nem támogatott topológia](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Több erdő, és több könyvtárak, GALSync előtérbe nem támogatott topológiát a helyszíni Active Directory](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Az Azure AD-bérlő munkakönyvtárral úgy lett kialakítva. Ezek a feladatok nem támogatottak:

* Az Azure AD Connect szinkronizálási tooread adatok egy másik Azure AD-bérlő hello konfigurációjának módosítása.
* Felhasználók exportálása névjegyek tooanother a helyszíni Active Directory példányt az Azure AD Connect szinkronizálási szolgáltatás használatával.

### <a name="galsync-with-on-premises-sync-server"></a>A helyszíni szinkronizálási kiszolgálóval GALSync
![Több erdő és a több könyvtárak topológiában GALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

FIM 2010 vagy a MIM 2016 helyszíni toosync felhasználókat (GALSync) két Exchange-szervezet között is használhatja. egy szervezet hello felhasználói frissítésként jelenik meg a külső felhasználók/contacts hello más szervezet. Ezek különböző a helyszíni Active Directory példányok majd szinkronizálhatja a saját Azure AD-bérlő.

## <a name="next-steps"></a>Következő lépések
Hogyan tooinstall, forgatókönyvek esetén az Azure AD Connect: toolearn [az Azure AD Connect egyéni telepítési](active-directory-aadconnect-get-started-custom.md).

További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

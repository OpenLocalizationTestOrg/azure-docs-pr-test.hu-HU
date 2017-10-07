---
title: "aaaAzure Active Directory – gyakori kérdések |} Microsoft Docs"
description: "Az Azure Active Directory GYIK hogyan tooaccess Azure és Azure Active Directory, a jelszókezelés és alkalmazás-hozzáférési kapcsolatos kérdésekre ad."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/16/2017
ms.author: markvi
ms.openlocfilehash: 63c30c4aeda4551bf02c6b968f98cded5a3b2c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-faq"></a>Azure Active Directory – gyakori kérdések
Az Azure Active Directory (Azure AD) egy átfogó szolgáltatott identitási (IDaaS) megoldás, amely az identitások, a hozzáférés-kezelés és a biztonság minden szempontját lefedi.

További információkért lásd: [Mi az az Azure Active Directory?](active-directory-whatis.md).


## <a name="access-azure-and-azure-active-directory"></a>Az Azure és az Azure Active Directory elérése
**K: Miért kapok "nem találhatók előfizetések" jelenik meg, hogy a klasszikus Azure portálon hello Azure AD tooaccess?**

**V:** tooaccess hello a klasszikus Azure portálon, minden felhasználó számára szükséges engedélyek Azure-előfizetés. Ha fizetős Office 365 vagy Azure AD-előfizetéssel rendelkezik, nyissa meg túl[http://aka.ms/accessAAD](http://aka.ms/accessAAD) egy egyszeri aktiváláshoz a. Ellenkező esetben szüksége lesz egy ingyenes tooactivate [Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) vagy egy fizetős előfizetést.

További információkért lásd:

* [How Azure subscriptions are associated with Active Directory? (Hogyan kapcsolódnak az Azure-előfizetések az Azure Active Directory-hoz?)](active-directory-how-subscriptions-associated-directory.md)
* [Az Office 365-előfizetéshez az Azure-ban hello címtár kezelése](active-directory-manage-o365-subscription.md)

- - -
**K: Mi az hello kapcsolat az Azure AD között az Office 365 és Azure?**

**V:** az Azure AD biztosít közös identitás- és hozzáférés lehetőségeket tooall webszolgáltatások. Akár az Office 365, Microsoft Azure, Intune-ban, vagy mások számára, akkor most már használja az Azure AD toohelp kapcsolja be a szolgáltatások a bejelentkezéshez és a hozzáférés-kezelés.

Minden felhasználó, aki toouse webszolgáltatások be vannak állítva az egy vagy több Azure AD-példányban található felhasználói fiókok vannak meghatározva. Beállíthatja ezeket a fiókokat az ingyenes Azure AD-képességekhez, például a felhőalkalmazások eléréséhez.

A fizetős Azure AD szolgáltatások, például az Enterprise Mobility + Security, átfogó vállalati méretű felügyeleti és biztonsági megoldásokkal egészítenek ki más webes szolgáltatásokat, például az Office 365-öt és a Microsoft Azure-t.
- - -
**K: miért lehet jelentkezzen be Azure-portálon toohello azonban nem hello a klasszikus Azure portálon?**

**V:** hello Azure-portál nem érvényes előfizetés szükséges, és a klasszikus portálon hello érvényes előfizetés szükséges.  Ha nem rendelkezik előfizetéssel, nem tud bejelentkezni a klasszikus portálon toohello.
- - -
**K: Mik előfizetési rendszergazda, és a Directory-rendszergazda hello különbségei?**

**V:** alapértelmezés szerint akkor hello előfizetés rendszergazdai szerepkörrel feliratkozás az Azure-bA. Előfizetés-adminisztrátor használhatja, vagy a Microsoft-fiókkal, vagy a munkahelyi vagy iskolai fiókkal, amely az Azure-előfizetés hello hello könyvtárból társítva.  A szerepkör engedélyezett toomanage szolgáltatások hello Azure-portálon.

Ha mások toosign az kell, és szolgáltatások által használatával hello ugyanahhoz az előfizetéshez, mint a társadminisztrátoroknak hozzáadhat. Ez a szerepkör rendelkezik hello azonos hello szolgáltatásadminisztrátoréval hozzáférési jogosultsággal, de nem módosíthatják az előfizetések tooAzure könyvtárak hello hozzárendelését.  Az előfizetés rendszergazdái további információkért lásd: [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](../billing-add-change-azure-subscription-administrator.md) és [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).


Az Azure AD rendszergazdai szerepkörök toomanage hello directory és az identitás-kapcsolatos funkciók különböző szabálykészleteket rendelkezik.  Ezeket a rendszergazdák a fog hello Azure-portálon hozzáférési toovarious funkciókat tartalmaz, vagy a klasszikus Azure portálon hello. Üdvözöljük a rendszergazdákat szerepkör meghatározza, hogy mit tehet, hasonló hozzon létre vagy szerkessze a felhasználók, tooothers rendszergazdai szerepkörök hozzárendelése, felhasználók új jelszavainak létrehozására, felhasználói licencek kezelése vagy tartományok kezelése.  Az Azure AD címtárrendszergazdáival és azok szerepköreivel kapcsolatos tovább információkért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles.md).

Ezenkívül a fizetős Azure AD szolgáltatások, például az Enterprise Mobility + Security, átfogó vállalati méretű felügyeleti és biztonsági megoldásokkal egészítenek ki más webes szolgáltatásokat, például az Office 365-öt és a Microsoft Azure-t.

- - -
**K: Létezik olyan jelentés, amely megmutatja, hogy mikor járnak le az Azure AD-beli felhasználói licenceim?**

**V.:** Nem.  Ez jelenleg nem érhető el.

- - -

## <a name="get-started-with-hybrid-azure-ad"></a>Első lépések a Hybrid Azure AD-ben


**K: Hogyan hagyhatok el egy bérlőt, amikor közreműködőként vagyok hozzáadva?**

**V:** tooanother szervezet bérlője kerülnek, közreműködő, használhatja hello "bérlői kapcsoló" hello felső jobb tooswitch a bérlők között.  Jelenleg nincs módja tooleave hello felkéri a szervezet van, és a Microsoft dolgozik a funkció.  Amíg ez a funkció nem érhető el, megkérheti a szervezet tooremove meghívása hello megtilthatja a bérlőben.
- - -
**K: hogyan kapcsolódhatnak a helyszíni címtár tooAzure AD?**

**V:** a helyszíni címtár tooAzure AD az Azure AD Connect használatával képes kapcsolódni.

További információkért lásd: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

- - -
**K: Hogyan állíthatok be egyszeri bejelentkezést a helyszíni címtár és a felhőalkalmazásaim között?**

**V:** csak akkor kell tooset be egyszeri bejelentkezést (SSO) a helyszíni címtár és az Azure AD között. Mindaddig, amíg az Azure AD keresztül fér hozzá a felhőalapú alkalmazásokhoz, a hello szolgáltatás automatikusan átirányítja a felhasználókat, a helyszíni hitelesítő adataikkal hitelesítést toocorrectly.

Az egyszeri bejelentkezés helyszínről végzett implementálása könnyen elérhető olyan összevonási megoldásokkal, mint az Active Directory összevonási szolgáltatások (AD FS), illetve a jelszókivonat-szinkronizálás konfigurálásával. Könnyedén telepítheti mindkét lehetőség hello Azure AD Connect konfigurációs varázsló használatával.

További információkért lásd: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

- - -
**K: Az Azure AD-ben elérhető önkiszolgáló portál a szervezetemben lévő felhasználók számára?**

**V:** Igen, az Azure AD biztosít hello [Azure AD hozzáférési Panel](http://myapps.microsoft.com) az önkiszolgáló felhasználók és az alkalmazás-hozzáférés. Ha Ön Office 365-felhasználó, található hello számos ugyanazokat a képességeket a hello Office 365 portál.

További információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).

- - -
**K: Az Azure AD segít a helyszíni infrastruktúrám kezelésében?**

**V:** Igen. hello Azure AD prémium kiadás tartalmazza az Azure AD Connect Health. Az Azure AD Connect Health segít a figyelheti, és betekintést a helyszíni identitás-infrastruktúra és hello szinkronizálási szolgáltatások.  

További információkért lásd: [figyelheti a helyszíni identitás infrastruktúra és a szinkronizálási szolgáltatások hello felhő](active-directory-aadconnect-health.md).  

- - -
## <a name="password-management"></a>Jelszókezelés
**K: Használhatom az Azure AD jelszóvisszaírást jelszó-szinkronizálás nélkül? (Ebben a forgatókönyvben az azt lehetséges toouse az Azure AD az önkiszolgáló jelszó-változtatási (SSPR) jelszó késleltetve visszaírt és tárolja-jelszavakkal működő hello felhő?)**

**V:** nem kell toosynchronize az Active Directory jelszavak tooAzure AD tooenable késleltetve visszaírt. Összevont környezetben az Azure AD az egyszeri bejelentkezés (SSO) támaszkodik hello a helyszíni címtár tooauthenticate hello felhasználó. Ez a forgatókönyv nem igényel hello helyszíni jelszó toobe nyomon követheti az Azure ad-ben.

- - -
**K: mennyi ideig tart a egy jelszó toobe visszaírását tooActive címtár a helyszínen?**

**V:** A jelszavak visszaírása valós időben történik.

További részletekért lásd: [A jelszókezelés első lépései](active-directory-passwords-getting-started.md).

- - -
**K: Használhatok jelszóvisszaírást rendszergazda által kezelt jelszavakhoz?**

**V:** Igen, ha jelszóvisszaírás engedélyezve van, egy rendszergazda által végrehajtott hello jelszó műveletek írt vissza tooyour helyszíni környezetben.  

Adott további válaszokért lásd toopassword kapcsolatos kérdések: [jelszókezeléssel kapcsolatos gyakori kérdések](active-directory-passwords-faq.md).
- - -
**K: Mi a teendő, ha a meglévő Office 365 vagy az Azure AD-jelszó nem emlékszem a jelszavam toochange közben?**

**V:** Ilyen helyzetben több lehetőség közül választhat.  Használjon önkiszolgáló jelszó-visszaállítást (SSPR), ha elérhető.  Az SSPR konfigurációjától függ, hogy működik-e.  További információkért lásd: [hogyan nem hello jelszó nullázza a munkahelyi portál](active-directory-passwords-best-practices.md).

Az Office 365-felhasználók, a rendszergazda is jelszó alaphelyzetbe állítása hello leírt lépéseket hello segítségével [felhasználói jelszavak átállítása](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).

Azure AD-fiókok, a rendszergazda alaphelyzetbe állíthatja a jelszavakat hello következő használatával:

- [Alaphelyzetbe állítja a fiókok a hello Azure-portálon](active-directory-users-reset-password-azure-portal.md)
- [A klasszikus portálon hello fiókok alaphelyzetbe állítása](active-directory-create-users-reset-password.md)
- [A PowerShell-lel](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a>Biztonság
**K: Zárolja a rendszer a fiókokat egy adott számú hibás kísérlet után, vagy ennél kifinomultabb stratégiát alkalmaz?**</br>
Egy kifinomultabb stratégia toolock fiókok használjuk.  Ez a hello IP hello kérelem és a beírt jelszavak hello alapul. hello kizárás időtartamát hello is növekszik hello valószínűsége, hogy a rendszer a támadás alapján.  

**K: bizonyos (általános) jelszavak beolvasása elutasítva hello üzenetek "ezt a jelszót használt toomany alkalommal újrapróbálkozott", nem ez tekintse meg a jelenlegi active Directoryban hello használt toopasswords?**</br>
Az adott globálisan, például a "Password" változatának és a "123456" toopasswords utal.

**K: Blokkolja a rendszer a kétes forrásokból (botnetek, TOR-végpontok) érkező bejelentkezési kéréseket a B2C-bérlőkön, vagy ehhez alap- vagy prémium szintű bérlőre van szükség?**</br>
Az átjárónk szűri a kéréseket és bizonyos fokú védelmet biztosít a botnetek ellen. Ez minden B2C-bérlőre vonatkozik.

## <a name="application-access"></a>Alkalmazás-hozzáférés
**K: Hol találom az Azure Ad-vel előre integrált alkalmazások és azok képességeinek listáját?**

**V:** Az Azure AD a Microsoft vállalat, az alkalmazásszolgáltatók és a partnerek több mint 2600 előre integrált alkalmazásával rendelkezik. Mindegyik előre integrált alkalmazás támogatja az egyszeri bejelentkezést (SSO). Egyszeri bejelentkezés lehetővé teszi a szervezeti hitelesítő adatok tooaccess az alkalmazásokat. Hello alkalmazások egy részének támogatja az automatizált üzembe helyezést és megszüntetést is.

Hello előre integrált alkalmazások teljes listáját lásd: hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).

- - -
**K: Mit tegyek, ha hello van szükségem az alkalmazás nincs hello Azure AD Marketplace-en?**

**V:** Az Azure AD Premiumban bármely alkalmazást felveheti és konfigurálhatja. Az alkalmazás képességeitől és a beállításaitól függően konfigurálhat egyszeri bejelentkezést és automatikus üzembe helyezést.  

További információkért lásd:

* [Egyszeri bejelentkezés tooapplications, amelyek nincsenek hello Azure Active Directory alkalmazáskatalógusában konfigurálása](active-directory-saas-custom-apps.md)
* [SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával](active-directory-scim-provisioning.md)

- - -
**K: hogyan tegye jelentkeznek be tooapplications Azure AD használatával?**

**V:** az Azure AD számos lehetőséget biztosít a felhasználók tooview, és az alkalmazások, például a eléréséhez:

* hello Azure AD hozzáférési panel
* hello Office 365 alkalmazásindító
* Közvetlen bejelentkezés toofederated alkalmazások
* Mélyhivatkozással toofederated, jelszóalapú, vagy meglévő alkalmazásokhoz

További információkért lásd: [üzembe helyezés az Azure AD integrált alkalmazások toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

- - -
**K: milyen módon hello Azure AD lehetővé teszi a hitelesítés és egyszeri bejelentkezés tooapplications?**

**V:** Az Azure AD számos szabványos protokollt támogat a hitelesítéshez és az engedélyezéshez, például ilyen a SAML 2.0, az OpenID Connect, az OAuth 2.0 és a WS-Federation. Az Azure AD a jelszótárolást és az automatikus bejelentkezési képességeket is támogatja olyan alkalmazásoknál, amelyek csak az űrlapalapú hitelesítést támogatják.  

További információkért lásd:

* [Hitelesítési forgatókönyvek az Azure AD-hez](active-directory-authentication-scenarios.md)
* [Az Active Directory hitelesítési protokolljai](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [Hogyan működik az egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
**K: Felvehetek helyszínen futtatott alkalmazásokat?**

**V:** az Azure AD-alkalmazásproxy egyszerű és biztonságos hozzáférést biztosít az Ön által tooon helyszíni webalkalmazások. Ezeket az alkalmazásokat hello végezheti el, hogy Ön a szoftver kiadva magát bejusson egy szolgáltatott szoftverként (SaaS) alkalmazások az Azure AD azonos módon. Nincs szükség a VPN-és toochange a hálózati infrastruktúrát.  

További információkért lásd: [hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md).

- - -
**K: Hogyan követelhetem meg a többtényezős hitelesítést az adott alkalmazásokhoz hozzáféréssel rendelkező felhasználóknál?**

**V:** Az Azure AD feltételes hozzáférésével minden alkalmazáshoz egyedi hozzáférési házirendet rendelhet. A házirend a multi-factor authentication mindig megkövetelheti, vagy ha a felhasználók nem rendelkeznek helyi hálózathoz csatlakoztatott toohello.  

További információkért lásd: [tooOffice 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele az Active Directory tooAzure csatlakoztatott](active-directory-conditional-access.md).

- - -
**K: Mi a SaaS-alkalmazások automatizált felhasználókiépítése?**

**V:** használja az Azure AD tooautomate hello létrehozását, karbantartási és eltávolítását számos népszerű felhőalapú Szolgáltatottszoftver-alkalmazásoknál felhasználói identitások.

További információkért lásd: [automatizálhatja a felhasználó kiépítésének és megszüntetésének biztosítása tooSaaS alkalmazásokat az Azure Active Directoryval](active-directory-saas-app-provisioning.md).

- - -
**K: Állíthatok be biztonságos LDAP-kapcsolatot az Azure AD-vel?**

**V.:** Nem. Az Azure AD nem támogatja a hello LDAP protokollt.

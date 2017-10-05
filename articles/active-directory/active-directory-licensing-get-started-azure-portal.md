---
title: "Ismerkedés az Azure Active Directory licencelése |} Microsoft Docs"
description: "Azure Active Directory licenckezelését, az ajánlott eljárások szerinti első lépések"
services: active-directory
keywords: "Az Azure AD-licencelés"
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: H1Hack27Feb2017;it-pro
ms.openlocfilehash: 6fa7cbbc452861870136482aa320d268e78fe3d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="license-yourself-and-your-users-in-azure-active-directory"></a>Licenc saját maga és a felhasználók az Azure Active Directoryban

> [!div class="op_single_selector"]
> * [Az Azure portál utasításokat](active-directory-licensing-get-started-azure-portal.md)
> * [Az Azure klasszikus portál adatok beolvasása](active-directory-licensing-what-is.md)
>
>

Azure Active Directory (Azure AD) identitás, mint a szolgáltatás (IDaaS) megoldás és a Microsoft platform. Az Azure AD különböző kiadásai tartományregisztráció:

* Az Azure Active Directory szabad, amelyik elérhető a Microsoft szolgáltatással, például az Office 365, Dynamics, a Microsoft Intune vagy Azure. Az Azure AD nem hoz létre a fogyasztás díjakat ebben a módban.

* Kiadás, például a fizetős Azure AD:
  - Enterprise Mobility + Security 
  - Prémium szintű Azure AD (P1 és P2)
  - Az Azure AD alapszintű
  - Azure Multi-Factor Authentication

Sok Microsoft online szolgáltatások, például az Azure AD fizetős verziója azonnal felhasználói jogosultságok keresztül Office 365, a Microsoft Intune és az Azure AD vannak. Ezekben az esetekben a szolgáltatás beszerzési képviseli egy vagy több előfizetés, és minden egyes előfizetés tartalmaz néhány prepurchased licencek az Ön bérelt szolgáltatásának. Felhasználói jogosultságok által érhetők el:

* A licenc hozzárendelése. 
* A felhasználó és a termék közötti kapcsolat létrehozásához.
* A szolgáltatás-összetevők, a felhasználó engedélyezése.
* Egyet, előre fel.

[Kipróbálom most az Azure AD prémium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

Az Azure AD szolgáltatás képességek az átfogó megismerésében, lásd: [Mi az Azure AD?](active-directory-whatis.md). További információkért lásd: a [szolgáltatói szerződések lap](https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/).

> [!NOTE]
> Azure-előfizetést használatalapú engedélyezte az Azure-erőforrások létrehozását, és hozzárendelheti a fizetési módot. Nincsenek nincsenek az előfizetéshez társított licencek száma. Engedélyeket a felhasználó az Azure-erőforrások rendelve az előfizetéshez való működésre, amikor azok az előfizetéshez társított, és kezelhetik az előfizetéshez kapcsolódó erőforrásokat.

## <a name="how-does-azure-active-directory-licensing-work"></a>Azure Active Directory licencelésének működése

Az Azure AD licencet-alapú munkahelyi szolgáltatások az Azure AD szolgáltatás bérlő előfizetés aktiválása. Után az előfizetése aktív, a szolgáltatás képességek az Azure AD-rendszergazdák által felügyelt, és a licenccel rendelkező felhasználók által használt.

### <a name="manage-subscription-information"></a>-Előfizetés adatainak kezelése

Nagyvállalati mobilitási + biztonsági, prémium szintű Azure AD vagy Azure AD alapvető megvásárolt, a bérlő frissül az előfizetést, beleértve a érvényességi időtartam és a fizetett licenccel. Az előfizetési adatai, beleértve a rendelt vagy rendelkezésre álló licencek számát az Azure portálon keresztül érhető el: A **Azure Active Directory**, nyissa meg a **licencek** az adott címtárhoz csempéjén. A **licencek** panel is a legjobb hely a licenc-hozzárendelések kezeléséhez.

Minden előfizetés áll egy vagy több service-csomagokról, például Azure AD, a multi-factor Authentication, Intune, az Exchange Online vagy SharePoint online-hoz.  Az Azure AD-Licenckezelés does *nem* igényelnek szolgáltatásiszint-terv-kezelést. Az Office 365 eltér, mivel a befoglalt szolgáltatásokhoz való hozzáférés kezelése a speciális konfigurációs mód alapul azt. Az Azure AD üzemelő konfiguráció szolgáltatások engedélyezése és kezeli az egyéni engedélyeket támaszkodik.

> [!IMPORTANT]
> Az Azure AD Premium, Azure AD alapvető és nagyvállalati mobilitási + biztonsági előfizetések a kiépített directory bérlőjének korlátozódnak. Előfizetések nem kell könyvtárak elosztva vagy más címtárakban lévő felhasználók jogosíthatók szolgál. Egy előfizetés közötti könyvtárak, lehetséges, de kell egy támogatási jegy elküldése, vagy törlését és az visszavásárlási közvetlen vásárlások.
>
> Ha az Azure AD vagy Enterprise Mobility + Security mennyiségi licencelés előfizetéssel értékesítik, és a szerződés magában foglalja a Microsoft Online más szolgáltatásaihoz (például Office 365), ha az aktiválás automatikusan történik. 

### <a name="assign-licenses"></a>Licencek hozzárendelése

Bár megszerezni egy előfizetés fizetett képességek konfigurálnia kell az összes, továbbra is kell osztania licenceket a felhasználók számára a szolgáltatások fizetős Azure ad. Minden olyan felhasználó, akik hozzáféréssel rendelkezhetnek vagy akik kezeli az Azure AD a szolgáltatás fizetős licencet kell hozzárendelni. A licenc-hozzárendelést a felhasználó és a megvásárolt szolgáltatás, például az Azure AD Premium, a Basic, vagy a nagyvállalati mobilitási + biztonsági közötti társítás.


Kezelése, a címtárban mely felhasználók rendelkezzenek a licenccel érhető el: 

* Licencek hozzárendelése a csoportok a [Azure-portálon](active-directory-licensing-whatis-azure-portal.md).
* Licencek hozzárendelésével közvetlenül a felhasználók számára a portálon, a PowerShell vagy az API-k. 

Ha most licencek hozzárendelése egy csoportot, csoport minden tagjára kapott licencet. Ha a felhasználók hozzáadásakor vagy eltávolításakor a csoportból, a megfelelő licenc hozzárendelt vagy eltávolítani. Csoport-hozzárendelés nyújthatnak a rendelkezésre álló csoportkezelés, és egységes alkalmazásokhoz történő Csoportalapú hozzárendelése.

Használhat [Csoportalapú licenc-hozzárendelést](active-directory-licensing-whatis-azure-portal.md) például a következő szabályok beállítása:
* A címtárban szereplő összes felhasználó automatikusan licenc beszerzése
* Rendelkezik a megfelelő beosztás lekérdezi a licenc
* A szervezet más kezelői döntési delegálhat (használatával [az önkiszolgáló csoportok](active-directory-accessmanagement-self-service-group-management.md))

Részletes leírását az csoportokhoz a licenc-hozzárendelést, beleértve a speciális forgatókönyvek és az Office 365 licencelési forgatókönyvek, lásd: [szükséges licencek kiosztása a felhasználók számára az Azure Active Directory csoport tagsága](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="get-started-with-azure-ad-licensing"></a>Ismerkedés az Azure AD-licenceléshez

Ismerkedés az Azure ad-vel is könnyen. A címtár regisztrációt részeként bármikor létrehozhat egy [Azure ingyenes próbát](https://azure.microsoft.com/offers/ms-azr-0044p/).

A következő gyakorlati tanácsok segítséget, győződjön meg arról, hogy a bérlő igazítva van-e, előfordulhat, hogy fel más Microsoft-szolgáltatásokkal és a célokat, a szolgáltatás:

- A Microsoft a szervezeti szolgáltatások segítségével, ha már rendelkezik az Azure AD-bérlő. Akkor célszerű használni ugyanannak a bérlőnek más szolgáltatásokkal, hogy a szolgáltatásban core Identitáskezelés, beleértve a kiépítés és hibrid egyszeri bejelentkezés (SSO) is használható. Az egyszeri bejelentkezés a felhasználók előnyt sokoldalú képességeit a szolgáltatásban. Így ha úgy dönt, hogy a dolgozók számára a szolgáltatás fizetős Azure AD vásárolni, ajánlott, hogy ugyanannak a bérlőnek újra.

- Javasoljuk, hogy egy új bérlőt az Azure portálon használjon el, ha azt tervezi, hogy:
  - Használhatja az Azure Active Directory különböző állítja be a felhasználók számára (például a partnerek vagy az ügyfelek).
  - Értékelje ki a termelési service elszigetelt Azure AD szolgáltatásokba.
  - A szolgáltatások egy védőfal mögötti környezet beállítása.

  Az új könyvtár hozza létre a fiók globális rendszergazdai jogosultságokkal rendelkező külső felhasználóként. Amikor bejelentkezik az Azure portálra, ezzel a fiókkal, tekintse meg ezt a bérlőt, és minden felügyeleti feladat eléréséhez.

> [!NOTE]
> Az Azure AD "vendégfelhasználók," felhasználói fiókok Azure AD-bérlő a Microsoft-fiókkal vagy egy Azure AD identity egy másik bérlőhöz keresztül létrehozott képező támogatja. Az Office 365 felügyeleti portál jelenleg nem támogatja ezeket a felhasználókat. A Microsoft-fiókokkal vendégfelhasználók nincsenek érhessék el az Office 365 felügyeleti portál, amíg más Azure AD bérlő vendégfelhasználók figyelmen kívül lesznek hagyva. Az utóbbi esetben csak a felhasználó helyi fiók, az Azure AD-ben, vagy ha a felhasználó eredetileg, Office 365-bérlő érhető el.

### <a name="select-one-or-more-license-trials"></a>Válasszon egy vagy több licenc próbaverzió

Aktiválhatja az Azure AD Premium vagy nagyvállalati mobilitási + biztonsági próba-előfizetést a **Azure Active Directory** &gt; **gyors üzembe helyezési**.

![Válassza ki a licencszerződés próbaverziójának](media/active-directory-licensing-get-started-azure-portal/select-a-license-trial.png)

Próbaverziós licenc érhetők el a **licencek** panelen.

### <a name="assign-licenses-to-users-and-groups"></a>Szükséges licencek kiosztása a felhasználók és csoportok

Miután az előfizetés aktív, saját magának kell rendel egy licencet. Ezt követően frissítse a böngészőt annak érdekében, hogy Ön a szolgáltatásokat. A következő lépésre licenceket rendelhet a felhasználókat, akiknek hozzáférésre van szükségük a fizetős Azure AD-funkciókat. A [licencek hozzárendelése](#assign-licenses), egy egyszerű licencek hozzárendelése módja a kívánt célközönség képviselő csoportnak, és a licenc hozzárendelése. Ezzel a módszerrel hozzáadásakor vagy annak életciklusa alatt a csoportból eltávolított felhasználók vannak kiosztani vagy megvonni a License kulcsattribútumokkal.

> [!NOTE]
> Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen. Mielőtt licenc rendelhet egy felhasználói, a rendszergazdának meg kell adnia a **felhasználási hely** tulajdonság a felhasználó számára. Beállíthatja, hogy ez a tulajdonság a **felhasználói** &gt; **profil** &gt; **beállítások** az Azure portálon. Licenc-hozzárendelést használatakor bármely felhasználó, akinek felhasználási hely nincs megadva örökli a könyvtár helye.

Kell hozzárendelnie, a **Azure Active Directory** &gt; **licencek** &gt; **összes**, egy vagy több terméket, majd válassza ki és **hozzárendelése** a parancssávon.

![Válassza ki a licenc hozzárendelése](media/active-directory-licensing-get-started-azure-portal/select-license-to-assign.png)

Használhatja a **felhasználók és csoportok** panel több felhasználó vagy csoport kiválasztása, vagy tiltsa le a termékben a service-csomagokról. Felhasználó- és csoportnevek kereséséhez használja a keresőmezőt felül.

![Válasszon egy felhasználót vagy csoportot a licenc-hozzárendelést](media/active-directory-licensing-get-started-azure-portal/select-user-for-license-assignment.png)

Ha még licenc hozzárendelése egy csoport, egy ideig, amíg az összes felhasználó öröklik a licencet, attól függően, hogy a csoportba tartozó felhasználók száma is igénybe vehet. Akkor is ellenőrizhesse a feldolgozási állapotát a **csoport** panel alatt a **licencek** csempére.

![Licenc-hozzárendelés állapota](media/active-directory-licensing-get-started-azure-portal/license-assignment-status.png)

Hozzárendelési hibák az Azure AD a licenc-hozzárendelést során fordulhatnak elő, de viszonylag ritkán kezelése az Azure AD és a nagyvállalati mobilitási + biztonsági termékeket. A lehetséges hozzárendelési hibák korlátozva:
- Hozzárendelés ütközés: amikor a felhasználó a korábban hozzárendelt a licencet, amely nem kompatibilis a jelenlegi licenc. Ebben az esetben a új licenc szükséges az aktuális példány eltávolítása.
- Túllépte a rendelkezésre álló licencek: hozzárendelt csoportokban lévő felhasználók száma meghaladja a rendelkezésre álló licencek, amikor a felhasználó-hozzárendelés állapotát tükrözi egy hiba miatt hiányzó licencek hozzárendelése.

#### <a name="azure-ad-b2b-collaboration-licensing"></a>Az Azure AD B2B együttműködés licencelés

B2B együttműködés lehetővé teszi az Azure AD-szolgáltatásokhoz való hozzáférés biztosításához az Azure AD-bérlő vendégfelhasználók meghívása, és bármely Azure-erőforrások elérhetővé tenni kívánt.  

Nincs B2B felhasználókat fióknevet és az Azure AD-alkalmazáshoz való hozzárendelése nem kell fizetni. Legfeljebb 10 / Vendég felhasználó és a 3 alapvető jelentés alkalmazásokat is szabadon B2B együttműködés felhasználók számára. Ha a Vendég felhasználó az Azure AD-bérlő a partner bármely megfelelő licenccel rendelkezik, akkor lesz licenccel kell rendelkezniük a saját is.

Nem kötelező, de ha lehetővé szeretné tenni a hozzáférést a fizetős Azure AD-funkciókat, e B2B vendégfelhasználók licenccel kell rendelkezniük az Azure AD licencet megfelelő. Az hívja fel egy licencet a fizetős Azure ad-bérlő számára egy további öt Vendég meghívót, hogy a bérlő felhasználói jogosultságokat rendelhet B2B együttműködés. A forgatókönyvek és a vonatkozó adatokat, lásd: [útmutatást licencelési B2B együttműködés](active-directory-b2b-licensing.md).

### <a name="view-assigned-licenses"></a>Licencek hozzárendelése megtekintése

A hozzárendelt és rendelkezésre álló licencek összefoglaló áttekintést jelenik meg **Azure Active Directory** &gt; **licencek** &gt; **összes**.

![Licenc összefoglaló megtekintése](media/active-directory-licensing-get-started-azure-portal/view-license-summary.png)

Hozzárendelt felhasználók és csoportok részletes listáját a termék kiválasztása esetén érhető el. A **licenccel rendelkező felhasználók** lista mutatja azokat az összes felhasználó jelenleg fel a licenc és e licenc közvetlenül lett hozzárendelve a felhasználó, vagy ha öröklés útján egy csoporttól származik.

![Licenc részleteinek megtekintése](media/active-directory-licensing-get-started-azure-portal/view-license-detail.png)

Hasonlóképpen a **licenccel rendelkező csoportok** lista mutatja azokat az összes csoport, amelyhez licencek van rendelve. Válasszon egy felhasználót vagy csoportot nyissa meg a **licencek** panel, amelyen látható, hogy az objektum összes licenccel.

### <a name="remove-a-license"></a>Távolítsa el a licenc

Licenc eltávolításához nyissa meg a felhasználó vagy csoport, és nyissa meg a **licencek** csempére. Jelölje ki a licencet, és kattintson a **eltávolítása**.

![Távolítsa el a licenc](media/active-directory-licensing-get-started-azure-portal/remove-license.png)

A felhasználó egy csoportból örökölt licencek közvetlenül nem távolítható el. Ehelyett távolítsa el a felhasználót a csoporthoz, amelyből a licenc történő öröklés.

### <a name="extend-trials"></a>Próbaverzió kiterjesztése

Az ügyfelek próba bővítmények érhetők el, önkiszolgáló regisztrációs az Office 365 portálon keresztül. Egy ügyfél rendszergazdai nyissa meg az Office-portál (az Office portál engedélyei függ access), és válassza ki az Azure AD prémium csomag próbaidőszaka. Kattintson a **hosszabbítani próbaidőszakot** hivatkozás a folyamat elindul. Hitelkártya szükséges, de nem kerül sor.

![Az Azure portálon próbaverzió meghosszabbítása](media/active-directory-licensing-get-started-azure-portal/extend-trial-beginning.png)

## <a name="next-steps"></a>Következő lépések

Speciális forgatókönyvek licenc felügyeleti csoportok használatával kapcsolatos további tudnivalókért olvassa el a cikk [licencek hozzárendelése egy csoport](active-directory-licensing-group-assignment-azure-portal.md).

Itt az információ arról, hogyan konfigurálhatja és használhatja más fizetős Azure AD-funkciókat:

* [Önkiszolgáló jelszóváltoztatás](active-directory-manage-passwords.md)
* [Önkiszolgáló csoportkezelés](active-directory-accessmanagement-self-service-group-management.md)
* [Az Azure AD Connect health](active-directory-aadconnect-health.md)
* [Az Azure többtényezős hitelesítés](../multi-factor-authentication/multi-factor-authentication.md)
* [Prémium szintű Azure AD licencet közvetlen beszerzési](http://aka.ms/buyaadp)

---
title: "az Azure Active Directoryban aaaLicense felhasználók |} Microsoft Docs"
description: "Megtudhatja, hogyan toolicense saját maga és a felhasználók az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
custom: it-pro
ms.openlocfilehash: ae0bc030fa02b79d1dd01ca961b4e96e6b9c470d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-license-users-in-azure-active-directory"></a>Gyors üzembe helyezés: Licenccel rendelkező felhasználók az Azure Active Directoryban
Az Azure AD licencet-alapú szolgáltatások munkahelyi egy Azure Active Directory (Azure AD) az előfizetéshez az Azure-bérlő aktiválása. Miután hello előfizetése aktív, a szolgáltatás képességei az Azure AD-rendszergazdák által kezelt és licenccel rendelkező felhasználók által használt. Nagyvállalati mobilitási + biztonsági, prémium szintű Azure AD vagy Azure AD alapvető, a bérlő vásárol hello előfizetéssel, beleértve az érvényességi frissül, és előre fizetett licenccel. Az előfizetési adatai, beleértve rendelt vagy rendelkezésre álló licencek számát hello hello Azure keresztül érhető el a portál **Azure Active Directory** által megnyitása hello **licencek** csempére. Hello **licencek** panel van is hello legjobb helyen toomanage a licenc-hozzárendelések.

Bár egy előfizetés beszerzése minden szükséges képességek fizetett tooconfigure, továbbra is kell osztania felhasználói licenceket szolgáltatások fizetős fizetős Azure ad. Minden olyan felhasználó, aki kell rendelkezik hozzáféréssel, vagy ki kezeli, az Azure AD a szolgáltatás fizetős hozzá kell rendelni a licencet. A licenc-hozzárendelést a felhasználó és a megvásárolt szolgáltatás, például az Azure AD Premium, a Basic, vagy a nagyvállalati mobilitási + biztonsági közötti társítás.

Használhat [Csoportalapú licenc-hozzárendelést](active-directory-licensing-whatis-azure-portal.md) tooset például hello következő szabályokat:
* A címtárban szereplő összes felhasználó automatikusan licenc beszerzése
* Mindenki hello megfelelő feladat címmel lekérdezi a licenc
* Hello döntési tooother kezelők hello szervezet delegálhat (használatával [az önkiszolgáló csoportok](active-directory-accessmanagement-self-service-group-management.md))

> [!TIP]
> A licenc hozzárendelése toogroups részletes leírását, beleértve a speciális forgatókönyvek és az Office 365 licencelési forgatókönyvek, lásd: [hozzárendelése az Azure Active Directory csoport tagsága toousers licencek](active-directory-licensing-group-assignment-azure-portal.md).

## <a name="assign-licenses-toousers-and-groups"></a>Toousers licencek és csoportok hozzárendelése
Aktív az előfizetése használja, érdemes először hozzárendelése a licenc tooyourself, és frissítse a böngésző tooensure, hogy az összes várt hello szolgáltatásai az előfizetéshez. következő lépés hello tooassign licencek toohello felhasználók, akik hozzáférést toopaid Azure AD-funkciókat. Egy egyszerű módot tooassign licencek tooassign licencek toogroups tooindividuals, hanem felhasználók. Licencek tooa csoport rendel csoport minden tagjára kapott licencet. Ha a felhasználók hozzáadásakor vagy eltávolításakor hello csoportból, hello megfelelő licencet automatikusan hozzárendelt vagy eltávolítani. 

> [!NOTE]
> Egyes Microsoft-szolgáltatások nem érhetők el az összes helyen. Licenc tooa felhasználói rendelhető, mielőtt hello rendszergazdának meg kell adnia hello **felhasználási hely** hello felhasználó tulajdonság. Beállíthatja, hogy ez a tulajdonság a **felhasználói** &gt; **profil** &gt; **beállítások** a hello Azure-portálon. Licenc-hozzárendelést használatakor bármely felhasználó, akinek felhasználási hely nincs megadva örökli hello könyvtár hello helye.

licenc tooassign a **Azure Active Directory** &gt; **licencek** &gt; **összes**, egy vagy több terméket, majd válassza ki és **Hozzárendelése** hello parancssávon.

![Válassza ki a licencszerződés tooassign](media/license-users-groups/select-license-to-assign.png)

Használhatja a hello **felhasználók és csoportok** panel toochoose több felhasználót vagy csoportot vagy toodisable service plans hello termékben. A felhasználó- és csoportnevek akkor használja a felső toosearch hello keresőmezőt.

![Válasszon egy felhasználót vagy csoportot a licenc-hozzárendelést](media/license-users-groups/select-user-for-license-assignment.png)

Licencek tooa csoport rendel egy ideig, amíg az összes felhasználó öröklése hello licenc hello csoport hello méretétől függően is igénybe vehet. Ellenőrizheti a hello feldolgozási állapot hello **csoport** panel alatt hello **licencek** csempére.

![Licenc-hozzárendelés állapota](media/license-users-groups/license-assignment-status.png)

Hozzárendelési hibák az Azure AD a licenc-hozzárendelést során fordulhatnak elő, de viszonylag ritkán kezelése az Azure AD és a nagyvállalati mobilitási + biztonsági termékeket. A lehetséges hozzárendelési hibák korlátozva:
- Hozzárendelés ütközés: amikor a felhasználó a korábban hozzárendelt a nem kompatibilis a jelenlegi licenc hello licenchez. Ebben az esetben hello új licenc hozzárendelése el kell távolítania jelenlegi hello.
- Túllépte a rendelkezésre álló licencek: hozzárendelt csoportokban lévő felhasználók hello száma meghaladja a rendelkezésre álló licencek hello, amikor a felhasználó-hozzárendelés állapotát tükrözi egy hiba tooassign toomissing licencek miatt.

### <a name="azure-ad-b2b-collaboration-licensing"></a>Az Azure AD B2B együttműködés licencelés

B2B együttműködés lehetővé teszi tooinvite vendégfelhasználók be az Azure AD bérlői tooprovide hozzáférés tooAzure AD szolgáltatásokba, és bármely Azure-erőforrások elérhetővé tenni kívánt.  

Nincs a B2B felhasználókat fióknevet és az Azure AD tooan alkalmazás hozzárendelése nem kell fizetni. Vendég / too10 alkalmazások fel felhasználói és 3 kapcsolatos alapszintű jelentések szabadon is B2B együttműködés felhasználók számára. Ha a Vendég felhasználó az Azure AD-bérlő hello partner bármely megfelelő licenccel rendelkezik, akkor lesz licenccel kell rendelkezniük a saját is.

Nem kötelező, de ha azt szeretné, hogy tooprovide hozzáférési toopaid az Azure AD-funkciókat, e B2B vendégfelhasználók licenccel kell rendelkezniük az Azure AD licencet megfelelő. Az hívja fel egy licencet a fizetős Azure ad-bérlő B2B együttműködés felhasználói jogosultságokat rendelhet meghívott tooan további öt vendégfelhasználók toohello bérlő. A forgatókönyvek és a vonatkozó adatokat, lásd: [útmutatást licencelési B2B együttműködés](active-directory-b2b-licensing.md).

## <a name="view-assigned-licenses"></a>Licencek hozzárendelése megtekintése

A hozzárendelt és rendelkezésre álló licencek összefoglaló áttekintést jelenik meg **Azure Active Directory** &gt; **licencek** &gt; **összes**.

![Licenc összefoglaló megtekintése](media/license-users-groups/view-license-summary.png)

Hozzárendelt felhasználók és csoportok részletes listáját a termék kiválasztása esetén érhető el. Hello **licenccel rendelkező felhasználók** lista mutatja azokat az összes felhasználó jelenleg fel a licencet, és hogy hello licenc lett hozzárendelve közvetlenül toohello felhasználó, vagy ha öröklés útján egy csoporttól származik.

![Licenc részleteinek megtekintése](media/license-users-groups/view-license-detail.png)

Ehhez hasonlóan hello **licenccel rendelkező csoportok** lista mutatja azokat az összes csoport toowhich licencek van rendelve. Válassza ki a felhasználó vagy csoport tooopen hello **licencek** panel, amelyen látható az összes licenc hozzárendelése toothat objektum.

## <a name="remove-a-license"></a>Távolítsa el a licenc

tooremove licencet, nyissa meg toohello felhasználót vagy csoportot, és nyissa meg a hello **licencek** csempére. Válassza ki a hello licenc, és kattintson a **eltávolítása**.

![Távolítsa el a licenc](media/license-users-groups/remove-license.png)

A csoportból hello felhasználó által örökölt licencek közvetlenül nem távolítható el. Ehelyett távolítsa el a hello felhasználót, ahol hello licenc történő öröklés hello csoportból.


## <a name="next-steps"></a>Következő lépések
A gyors üzembe helyezés mér megismerte, hogyan tooassign licencek toousers és a csoportokat az Azure AD-címtárban. 

Hello hivatkozás tooconfigure előfizetési licenc-hozzárendeléseivel követően az Azure AD hello Azure-portálon is használhatja.

> [!div class="nextstepaction"]
> [Az Azure AD-licencek hozzárendelése](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Overview) 
---
title: az Azure AD Privileged Identity Management aaaRoles |} Microsoft Docs
description: "Ismerje meg, milyen szerepkörök használt hello Azure Privileged Identity Management bővítmény kiemelt identitásokat."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: ac812ccc-cf4e-4ac2-b981-69598056c9ed
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/31/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: dc58d005489e3b51b3b3dbea4bf35bd795dbdfb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="different-administrative-role-in-azure-active-directory-pim"></a>Az Azure Active Directory PIM különböző rendszergazdai szerepkör
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Felhasználók a szervezet toodifferent rendszergazdai szerepköröket rendelhet az Azure ad-ben. A szerepkör-hozzárendelések szabályozása milyen feladatokat, például a felhasználók hozzáadása és eltávolítása, vagy a szolgáltatás beállításainak módosítása, hello felhasználók képes tooperform Azure ad-val, Office 365 és más Microsoft Online Services és a csatlakoztatott alkalmazások.  

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.

Egy globális rendszergazda frissítheti, amelyek felhasználók **véglegesen** tooroles hozzárendelése az Azure AD PowerShell-parancsmagok használatával, mint `Add-MsolRoleMember` és `Remove-MsolRoleMember`, vagy a klasszikus portálon hello leírtak [ rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md).

Az Azure AD Privileged Identity Management (PIM) a privilegizált hozzáférés érdekében a felhasználók házirendeket kezeli az Azure ad-ben. PIM hozzárendel felhasználók tooone vagy a további szerepkörök az Azure ad-ben, és hozzá lehet rendelni valaki toobe véglegesen a hello szerepkör, illetve a jogosult hello szerepkörhöz. Amikor a felhasználó véglegesen szerepét tooa, vagy egy erre jogosult szerepkör-hozzárendelés aktiválja kezelésére Azure Active Directory, az Office 365 és az egyéb alkalmazások hello engedélyekkel tootheir szerepkört, majd.

Nincs a megfelelő szerepkör-hozzárendeléssel és állandó toosomeone megadott hello Access különbség. hello egyetlen különbség, hogy néhány felhasználó nem kell elérheti az összes hello idő. Abban az esetben jogosult hello szerepkör történik, és kapcsolhatja be és ki, amikor szükség van.

## <a name="roles-managed-in-pim"></a>Szerepkörök kezelése a PIM
A privileged Identity Management lehetővé teszi a felhasználók toocommon rendszergazdai szerepköröket, beleértve:

* **Globális rendszergazda** (más néven a vállalati rendszergazda) rendelkezik hozzáférési tooall felügyeleti funkciókat. A szervezet csak egy globális rendszergazdai lehet. hello személy toopurchase Office 365 automatikusan válik egy globális rendszergazdához.
* **Kiemelt szerepkörű rendszergazda** Azure AD PIM kezeli és frissíti a szerepkör-hozzárendelések más felhasználók számára.  
* **Számlázási rendszergazda** lebonyolítja a vásárlásokat, kezeli az előfizetéseket, támogatási jegyeket, és figyeli a szolgáltatás állapotát.
* **Jelszókezelő** átállítja a jelszavakat, kezeli a szolgáltatáskérésekat és figyeli a szolgáltatás állapotát. Jelszó rendszergazdák korlátozott tooresetting jelszavak felhasználók.
* **Szolgáltatás-rendszergazda** kezeli a szolgáltatáskérésekat és figyeli a szolgáltatás állapotát.
  
  > [!NOTE]
  > Ha az Office 365 használ, majd hello szolgáltatás rendszergazdai szerepkör tooa felhasználó hozzárendelése előtt először hello felhasználó rendszergazdai jogosultságok hozzárendelése tooa szolgáltatás, például az Exchange online-hoz.
  > 
  > 
* **Felhasználókezelő rendszergazda** átállítja a jelszavakat, figyeli a szolgáltatás állapotát, és kezeli a felhasználói fiókok, a felhasználói csoportok és a szolgáltatáskéréseket. hello felhasználói felügyeleti admin nem lehet törölni egy globális rendszergazdai, egyéb rendszergazdai szerepköröket hozhat létre, vagy állítsa vissza a globális, számlázási és a szolgáltatás-rendszergazdák jelszavát.
* **Exchange-rendszergazda** keresztül hello Exchange felügyeleti központot (min.) rendszergazdai hozzáféréssel tooExchange Online rendelkezik, és szinte bármilyen feladatot végrehajthat Exchange Online-ban.
* **SharePoint-rendszergazda** rendelkezik rendszergazdai hozzáféréssel tooSharePoint Online hello SharePoint Online felügyeleti központon keresztül történik, és szinte bármilyen feladatot végrehajthat a SharePoint online-hoz.
* **Skype vállalati rendszergazda** rendelkezik rendszergazdai hozzáféréssel tooSkype üzleti hello Skype vállalati rendszergazda Center keresztül, és szinte bármilyen feladatot végrehajthat a a Skype vállalati online.

Ezek a cikkek további részletekért olvassa el [rendszergazdai szerepkörök hozzárendelése az Azure AD](active-directory-assign-admin-roles.md) és [rendszergazdai szerepkörök hozzárendelése az Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: hello above article may not be hello one we want since PIM gets roles from places other that Office 365**-->


A PIM, akkor [ezen szerepkörök tooa felhasználó](active-directory-privileged-identity-management-how-to-add-role-to-user.md) , hogy hello felhasználói is [szükség esetén hello szerepkör aktiválásához](active-directory-privileged-identity-management-how-to-activate-role.md).

Ha azt szeretné, toogive a PIM, amelyhez PIM hello felhasználói toohave részelemcímkék ismertetését további hello szerepköröket egy másik felhasználó hozzáférési toomanage [hogyan férhetnek hozzá a toogive tooPIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

<!-- ## hello PIM Security Administrator Role **PLACEHOLDER: Need description of hello Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>A PIM nem kezelt szerepkörök
Szerepkörök Exchange Online vagy SharePoint online-hoz, kivéve a fent említett nem találhatók az Azure ad-ben, és ezért nem láthatók a PIM. Ezek az Office 365-szolgáltatásokhoz részletes szerepkör-hozzárendelések módosításáról bővebben lásd: [engedélyek az Office 365-ben](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-előfizetések és -erőforráscsoportok is nem jelennek meg az Azure ad-ben. toomanage Azure előfizetések, lásd: [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](../billing/billing-add-change-azure-subscription-administrator.md) és további információ az Azure RBAC lásd [átruházásához hozzáférés-vezérlés](role-based-access-control-configure.md).

<!--**hello above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Felhasználói szerepkörök, és jelentkezzen be
Bizonyos Microsoft-szolgáltatások és alkalmazások, a felhasználó tooa szerepkör hozzárendelése előfordulhat, hogy nem megfelelő tooenable kell, hogy felhasználói toobe rendszergazdaként.

Hozzáférés toohello a klasszikus Azure portálon hello szükséges felhasználói kell egy szolgáltatási rendszergazdának vagy társadminisztrátornak az Azure-előfizetéssel, akkor is, ha hello felhasználónak nem kell toomanage hello Azure-előfizetések.  Toomanage beállításait az Azure AD hello klasszikus portál, a felhasználó például az Azure AD globális rendszergazda és előfizetés társadminisztrátoraként az Azure-előfizetéssel kell lennie.  Hogyan tooadd felhasználók tooAzure előfizetések: toolearn [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](../billing/billing-add-change-azure-subscription-administrator.md).

Szükség lehet az Online szolgáltatások hozzáférési tooMicrosoft hello felhasználói is hozzá kell rendelni a licencet hello szolgáltatás portál megnyitásához és felügyeleti feladatok.

## <a name="assign-a-license-tooa-user-in-azure-ad"></a>Licenc tooa felhasználó hozzárendelése az Azure ad-ben
1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com) vagy globális rendszergazdai fiók, vagy egy közös rendszergazdai fiók.
2. Válassza ki **minden elem** hello főmenüből.
3. Válassza ki a kívánt toowork az és, hogy rendelkezik licencek társítva hello könyvtár.
4. Válassza ki **licencek**. rendelkezésre álló licencek hello listája jelenik meg.
5. Válassza ki a megjeleníteni kívánt toodistribute hello licencek tartalmazó hello licenccsomag.
6. Válassza ki **felhasználók hozzárendelése**.
7. Megjeleníteni kívánt tooassign licenc válassza hello felhasználói számára.
8. Kattintson a hello **hozzárendelése** gombra.  hello felhasználói tooAzure mostantól bejelentkezhet.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


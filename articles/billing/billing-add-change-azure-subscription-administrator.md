---
title: "Hozzáadása vagy módosítása az Azure rendszergazdai előfizetés szerepkörök |} Microsoft Docs"
description: "Ismerteti, hogyan hozzáadása vagy módosítása az Azure Társadminisztrátoraként, a szolgáltatás-rendszergazda és a fiók rendszergazdájához"
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/04/2018
ms.author: genli
ms.openlocfilehash: dc09f29fec78d408e1560bfa0a943f16ab50c760
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/04/2018
---
# <a name="add-or-change-azure-subscription-administrators"></a>Hozzáadása vagy módosítása az Azure-előfizetés rendszergazdái

Klasszikus Azure-előfizetések rendszergazdái és az Azure [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-what-is.md) Azure erőforrásokhoz való hozzáférés kezelése a két rendszer:

* Hagyományos előfizetés rendszergazdai szerepkörök egyszerű kezelés és a fiók rendszergazdájához, szolgáltatás-rendszergazda és Társrendszergazdák.
    * Amikor regisztrál egy új Azure-előfizetéséhez, a fiók a a fiók rendszergazdájához, és a szolgáltatás rendszergazdája, alapértelmezés szerint van beállítva.
    * Társrendszergazdák után jelentkezzen lehet hozzáadni.
* Az RBAC egy újabb rendszerre, amely részletes hozzáféréskezelést számos beépített szerepkörök, hatókörű, és egyéni szerepkörök rugalmasságot biztosít.
    * Csak az RBAC-szerepkörök és a nem hagyományos előfizetés rendszergazdai szerepkörök rendelkező felhasználók azonban nem tudja kezelni az Azure klasszikus üzembe helyezés.

Jobban ellenőrizhető és kezelési egyszerűsítése, ajánlott RBAC használata az összes access felügyeleti igényeinek megfelelően. Ha lehetséges azt javasoljuk, hogy a meglévő hozzáférési házirendekkel RBAC újrakonfigurálása. 

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-admin-for-a-subscription-in-azure-portal"></a>Adja hozzá a Szerepalapú tulajdonos rendszergazda-előfizetéshez tartozó Azure-portálon 

Valaki hozzáadásához az Azure-előfizetés szolgáltatás-felügyeleti rendszergazdaként számukra az RBAC tulajdonosi szerepkört az előfizetéshez. A tulajdonosi szerepkört kezelheti az erőforrásokat az előfizetéshez társított, és nem rendelkezik hozzáférési jogosultság más előfizetésekkel is.

1. Látogasson el [ **előfizetések** Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Válassza ki az előfizetést, amelyet szeretne hozzáférést.
3. Válassza ki **hozzáférés-vezérlés (IAM)** a menüben.
4. Az a **szerepkör** mezőben válassza **tulajdonos**. 
5. Az a **való hozzáférés hozzárendelése** mezőben válassza **Azure AD-felhasználó, csoport vagy alkalmazás**. 
6. Az a **válasszon** mezőbe írja be a felhasználó hozzá szeretne adni a tulajdonos e-mail címe. A felhasználó, majd válassza ki és **mentése**.

    ![A tulajdonosi szerepkört a kiválasztott képernyőkép](./media/billing-add-change-azure-subscription-administrator/add-role.png)

A felhasználó számára teljes hozzáférést ad az összes erőforrást, beleértve a mások számára delegálása jobb. Hozzáférést egy másik hatókört, például egy erőforráscsoport, látogasson el a IAM menü, az adott hatókörnél. 

## <a name="add-or-change-co-administrator"></a>Hozzáadása vagy módosítása társadminisztrátoraként

Csak egy olyan tulajdonost társadminisztrátoraként adhatók hozzá. Szerepkörök, például közreműködői és ahhoz való olvasóra rendelkező más felhasználók társadminisztrátorként nem adható hozzá.

> [!TIP]
> Csak kell hozzáadnia a "Tulajdonos" fiók a közös rendszergazdaként, ha a felhasználónak van szüksége az Azure klasszikus üzembe helyezés kezelése. Minden más célra RBAC használatát javasoljuk.

1. Még nem tette meg, ha valaki egy olyan tulajdonost, a fenti utasításokat követve adja hozzá.
2. **Kattintson a jobb gombbal** a tulajdonosa, az előzőekben adott hozzá, és válassza **közös rendszergazdaként Hozzáadás**. Ha nem látja a **közös rendszergazdaként Hozzáadás** lehetőségét, frissítse az oldalt, vagy próbálkozzon egy másik böngészőben. 

    ![Képernyőkép a társadminisztrátoraként hozzáadása](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Eltávolítja a közös rendszergazdai jogosultsággal, **kattintson a jobb gombbal** "Közös rendszergazda" felhasználói és válassza ki **közös rendszergazda eltávolítása**.

    ![Képernyőkép a társadminisztrátoraként eltávolítása](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Módosítsa az Azure-előfizetés szolgáltatás-rendszergazda

Csak a Fiókadminisztrátor az előfizetés szolgáltatás-rendszergazda módosíthatja. Alapértelmezés szerint amikor regisztrál, a szolgáltatás-rendszergazda megegyezik a fiók-rendszergazdaként. Ha a szolgáltatás-rendszergazda egy másik felhasználó módosul, majd a Fiókadminisztrátor elveszítette a hozzáférését az Azure-portálhoz. Azonban a Fiókadminisztrátor mindig segítségével Account Center módosíthatja a szolgáltatás-rendszergazda vissza a saját magukat.

1. Győződjön meg arról, hogy a forgatókönyv által támogatott a [vonatkozó szolgáltatás-rendszergazdák módosítása](#limits).
1. Jelentkezzen be [Account Center](https://account.windowsazure.com/subscriptions) fiók rendszergazdaként.
1. Válasszon egy előfizetést.
1. A jobb oldalon válassza ki a **előfizetés részleteinek szerkesztése**.

    ![Képernyőfelvétel a Szerkesztés előfizetés gomb Account Center](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. Az a **szolgáltatás-rendszergazda** mezőbe írja be az e-mail cím a új szolgáltatás rendszergazdájával.

    ![Képernyőfelvétel a mezőbe a szolgáltatás-rendszergazdák e-mail módosítása](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Szolgáltatás-rendszergazdák módosítása vonatkozó korlátozások

* Az Azure AD-címtár minden előfizetés tartozik. A könyvtárat, az előfizetéshez társított találja, [ **előfizetések**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), majd válasszon ki egy előfizetést tekintse meg a könyvtárban.
* Ha jelentkezett be a munkahelyi vagy iskolai fiókkal, a más fiókokat is hozzáadhat a szolgáltatás-rendszergazdaként a szervezetében. Például abby@contoso.com adhat hozzá bob@contoso.com , szolgáltatás-rendszergazda nem adható hozzá, de john@notcontoso.com kivéve, ha john@notcontoso.com rendelkezik-e a contoso.com directory jelenlétét. Munkahelyi vagy iskolai fiókok bejelentkezve felhasználók továbbra is vegye fel a Microsoft Account felhasználók szolgáltatás-rendszergazdaként.

  | Bejelentkezési módszer | Adja hozzá a Microsoft Account felhasználó rendszergazdai (SA)? | Adja hozzá a munkahelyi vagy iskolai fiókjával társításként ugyanazon a szervezeten belül? | Adja hozzá a munkahelyi vagy iskolai fiókjával társításként másik vállalatnál? |
  | --- | --- | --- | --- |
  |  Microsoft-fiók |Igen |Nem |Nem |
  |  Munkahelyi vagy iskolai fiókkal |Igen |Igen |Nem |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>A Fiókadminisztrátor az Azure-előfizetés módosítása

A Fiókadminisztrátor a felhasználót, hogy kezdetben az Azure-előfizetésre iratkozott fel, és az előfizetés számlázási tulajdonosaként felelős. A Fiókadminisztrátor az előfizetés módosításához lásd [egy másik fiókot az Azure-előfizetés tulajdonjogának átruházása](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Nem biztos benne, aki a fiók rendszergazdájához?** Kövesse az alábbi lépéseket:

1. Látogasson el [ **előfizetések** Azure-portálon](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Válassza ki az előfizetést, és ellenőrizze, majd keresse meg a **beállítások**.
1. Válassza ki **tulajdonságok**. A Fiókadminisztrátor az előfizetés jelenik meg a **Fiókadminisztrátor** mezőbe.  

## <a name="types-of-classic-subscription-admins"></a>Klasszikus előfizetés rendszergazdái típusai

 Fiók rendszergazdájához, szolgáltatás-rendszergazda és társadminisztrátoraként háromféle a hagyományos előfizetés rendszergazdai szerepköröket az Azure-ban. A fiók, amellyel regisztráció az Azure automatikusan a a fiók rendszergazdájához, és a szolgáltatás rendszergazdája állítja. Ezt követően további Társrendszergazdák lehet hozzáadni. A következő táblázat ismerteti ezeket három rendszergazdai szerepkörei pontos különbségei. 

> [!TIP]
> A vezérlő jobb és a részletes hozzáféréskezelést azt javasoljuk, Azure szerepköralapú hozzáférés-vezérlés (RBAC), amely lehetővé teszi a felhasználóknak adható hozzá több szerepkört. További tudnivalókért lásd: [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md).

| Hagyományos előfizetés-adminisztrátor | Korlát | Leírás |
| --- | --- | --- |
| A Fiókadminisztrátor (AA) |1 / Azure-fiók |Ez az az Azure-előfizetésre iratkozott fel, és elérésére jogosult felhasználó a [Account Center](https://account.azure.com/Subscriptions) és különféle felügyeleti feladatok elvégzésére. Például hozzon létre új előfizetések, előfizetések megszakítja, módosítsa az előfizetés számlázási és módosítsa a szolgáltatás-rendszergazda. A Fiókadminisztrátor fogalmilag, az előfizetés számlázási tulajdonosa. Az RBAC a fiók rendszergazdájához nincs hozzárendelve szerepkör.|
| Szolgáltatás-rendszergazdai (SA) |1 / Azure-előfizetés |Ezt a szerepkört a szolgáltatások kezelésére jogosult a [Azure-portálon](https://portal.azure.com). Alapértelmezés szerint egy új előfizetés a fiók rendszergazdájához is a szolgáltatás rendszergazdájával. Az RBAC a tulajdonosi szerepkört kap a a szolgáltatás-rendszergazda az előfizetés hatókörben.|
| Társadminisztrátoraként (CA) |200 előfizetésenként |Ez a szerepkör ugyanazokkal a hozzáférési jogosultságokkal rendelkezik, mint a szolgáltatás-rendszergazda, de nem módosíthatja az előfizetések és az Azure-címtárak közötti társítást. Az RBAC a tulajdonosi szerepkört kap az előfizetési hatókört közös rendszergazdájának.|

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>További információ az erőforrás hozzáférés-vezérlés és az Active Directory

* Hogyan szabályozott erőforrások elérése a Microsoft Azure-ban kapcsolatos további információkért lásd: [az az Azure-erőforrások hozzáférésének megismerése](../active-directory/active-directory-understanding-resource-access.md).
* Azure Active Directoryval kapcsolatos további információkért lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) és [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](../active-directory/active-directory-assign-admin-roles-azure-portal.md).

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.

Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.

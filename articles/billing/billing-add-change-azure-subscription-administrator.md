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
ms.topic: article
ms.date: 07/20/2017
ms.author: genli
ms.openlocfilehash: da5995535d42ed52772cb09e0f4da51bbf878748
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-or-change-azure-administrator-roles-that-manage-the-subscription-or-services"></a>Hozzáadása vagy módosítása, hogy az előfizetés vagy a szolgáltatások kezelése az Azure rendszergazdai szerepkörök
Módosíthatja az Azure rendszergazdai, amely kezeli az Azure-előfizetéshez és nem is kezeli az előfizetésében használt Azure-szolgáltatásokhoz. Az Azure számlázási adatokat előfizetések megtekintése és kezelése, akkor be kell jelentkeznie a számára a [Account Center](https://account.windowsazure.com/Home/Index) fiók rendszergazdaként. 

## <a name="add-an-admin-for-a-subscription"></a>Adja hozzá a rendszergazda az előfizetéshez
Egy Azure rendszergazdát adhat hozzá, az Azure portálon vagy a klasszikus Azure portálon.

**Azure Portal**

Valaki hozzáadásához az előfizetéshez az Azure portálon rendszergazdaként kell adnia azokat a tulajdonosi szerepkört. A tulajdonosi szerepkört csak kezelheti az erőforrásokat, amelyet rendelt előfizetést. Nincs hozzáférési jogosultság más előfizetésekkel is. A tulajdonosok keresztül hozzáadhat a [Azure-portálon](https://portal.azure.com) erőforrás nem tudja kezelni a [a klasszikus Azure portálon](https://manage.windowsazure.com).

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. A központ menüben válassza ki a **előfizetés** > *az előfizetést, amelyet a rendszergazda eléréséhez*.

    ![Képernyőkép a kijelölt előfizetés](./media/billing-add-change-azure-subscription-administrator/newselectsub.png)

3. Az előfizetés panelen válassza ki a **hozzáférés-vezérlés (IAM)**.
4. Válassza ki **hozzáadása** > **szerepkör** > **tulajdonos**. Írja be a kívánt tulajdonosa adja, jelölje ki a felhasználót, és válassza ki a felhasználó e-mail címe **mentése**.

    ![A tulajdonosi szerepkört a kiválasztott képernyőkép](./media/billing-add-change-azure-subscription-administrator/add-role.png)

5. Ha szeretné-e a tulajdonos fiók hozzáadása közös rendszergazdaként a a **hozzáférés-vezérlés (IAM)** lapon, kattintson a jobb gombbal a felhasználó, és válassza ki **közös rendszergazdaként Hozzáadás**. Ez a funkció már elérhető a [Azure betekintő portál](https://preview.portal.azure.com/). 

     ![Képernyőkép a társadminisztrátoraként hozzáadása](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    >[!TIP]
    >Szüksége lesz a "Tulajdonos" felhasználó hozzáadása közös rendszergazdaként, ha a felhasználónak van szüksége az Azure-szolgáltatások kezelése [a klasszikus Azure portálon](https://manage.windowsazure.com/).

    A közös rendszergazdának eltávolításához kattintson a jobb gombbal a "közös rendszergazda" felhasználói, és válassza ki **közös rendszergazda eltávolítása**.

    ![Képernyőkép a társadminisztrátoraként eltávolítása](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)


**klasszikus Azure portál**

1. Jelentkezzen be a [klasszikus Azure portálra](https://manage.windowsazure.com/).
2. A navigációs ablaktáblán válassza ki a **beállítások**> **rendszergazdák**> **Hozzáadás**. </br>

    ![Képernyőkép a Hozzáadás gombra az beszerzése](./media/billing-add-change-azure-subscription-administrator/addcoadmin.png)
3. Írja be a kívánt hozzáadása közös rendszergazdaként, és válassza ki az előfizetést, a társadminisztrátoraként eléréséhez használni kívánt személy e-mail címét.</br>

    ![A kijelölt előfizetésben képernyőkép ](./media/billing-add-change-azure-subscription-administrator/addcoadmin2.png)</br>

A következő e-mail cím felveheti társadminisztrátoraként:

* **Microsoft Account** (korábbi nevén Windows Live ID Azonosítóval) </br>
  Jelentkezzen be a végfelhasználóra irányuló Microsoft termékekhez, és a felhőalapú szolgáltatások, például az Outlook (Hotmail), a Skype (MSN), a onedrive vállalati verzió, a Windows Phone és a Xbox LIVE használhatja Microsoft-Account.
* **Szervezeti fiókkal**</br>
  Egy szervezeti egy Azure Active Directory alatt létrehozott fiók. A szervezeti fiók címe van ebben a formátumban:

    @ felhasználó&lt;a tartomány&gt;. onmicrosoft.com

## <a name="change-service-administrator-for-a-subscription"></a>Módosítsa az előfizetés szolgáltatás-rendszergazda
Csak a Fiókadminisztrátor az előfizetés szolgáltatás-rendszergazda módosíthatja.

1. Jelentkezzen be [Azure Account Center](https://account.windowsazure.com/subscriptions) a Fiókadminisztrátor használatával.
2. Válassza ki a módosítani kívánt előfizetést.
3. A jobb oldalon válassza ki a **előfizetés szerkesztése** részleteit. </br>

    ![editsub](./media/billing-add-change-azure-subscription-administrator/editsub.png)
4. Az a **szolgáltatás-rendszergazda** mezőbe írja be az e-mail cím a új szolgáltatás rendszergazdájával. </br>

    ![changeSA](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

## <a name="change-the-account-administrator"></a>A Fiókadminisztrátor módosítása
Az Azure-fiók áthelyezni egy másik fiókot, lásd: [Azure-előfizetés átvitele tulajdonjogát](billing-subscription-transfer.md).

Határozottan javasoljuk, hogy ne törölje vagy nevezze át a Fiókadminisztrátor e-mail címet. Az Azure-fiók váratlan és nemkívánatos működését jelenhet meg. Akkor lehet, hogy nem tud bejelentkezés az Azure azzal a fiókkal, módosítja a fiókot, vagy kezelheti az erőforrásokat, azzal a fiókkal. 

## <a name="check-the-account-administrator-of-the-subscription"></a>A Fiókadminisztrátor az előfizetés ellenőrzése
Ha nem biztos, aki a fiókadminisztrátor az előfizetéséhez, tegye a következőket megállapítása.

  1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
  2. A központ menüben válassza ki a **előfizetés**.
  3. Válassza ki az előfizetést, és ellenőrizze, majd keresse meg a **beállítások**.
  4. Válassza ki **tulajdonságok**. A fiókadminisztrátor az előfizetés jelenik meg a **Fiókadminisztrátor** mezőbe.  

## <a name="types-of-azure-admin-accounts"></a>Az Azure rendszergazdai fiókok típusai
 Fiók rendszergazdájához, szolgáltatás-rendszergazda és társadminisztrátoraként háromféle a Microsoft Azure-ban rendszergazdai szerepkörrel. A következő táblázat ismerteti ezeket három rendszergazdai szerepkörök közötti különbség.

| Rendszergazdai szerepkör | Korlát | Leírás |
| --- | --- | --- |
| A Fiókadminisztrátor (AA) |1 / Azure-fiók |Ez az a személy, aki regisztrált a vásárolt Azure-előfizetések és elérésére jogosult a [Account Center](https://account.windowsazure.com/Home/Index) és különféle felügyeleti feladatok elvégzésére. Például előfizetések létrehozása, előfizetések megszakítja, módosítsa az előfizetés számlázási, és módosítsa a szolgáltatás-rendszergazda. |
| Szolgáltatás-rendszergazdai (SA) |1 / Azure-előfizetés |Ezt a szerepkört a szolgáltatások kezelésére jogosult a [Azure-portálon](https://portal.azure.com). Alapértelmezés szerint egy új előfizetés a fiók rendszergazdájához is a szolgáltatás rendszergazdájával. |
| A társadminisztrátoraként (CA) a [a klasszikus Azure portálon](https://manage.windowsazure.com) |200 előfizetésenként |Ez a szerepkör ugyanazokkal a hozzáférési jogosultságokkal rendelkezik, mint a szolgáltatás-rendszergazda, de nem módosíthatja az előfizetések és az Azure-címtárak közötti társítást. |

Az Azure Active Directory szerepköralapú hozzáférés-vezérlés (RBAC) lehetővé teszi a felhasználóknak adható hozzá több szerepkört. További információkért lásd: [Azure Active Directory szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

## <a name="limitations-and-restrictions-for-admin-accounts"></a>Korlátozások és rendszergazdai fiókok korlátozásai
* Minden előfizetés társítva az Azure AD-címtár (más néven az alapértelmezett könyvtár). Az előfizetés tartozik alapértelmezett címtárat találja, a [a klasszikus Azure portálon](https://manage.windowsazure.com/), jelölje be **beállítások** > **előfizetések**. Ellenőrizze az előfizetés-azonosító található a alapértelmezett címtárat.
* Ha a Microsoft-Account van bejelentkezve, csak adhat hozzá más Microsoft-Accounts vagy a felhasználók az alapértelmezett címtár közös rendszergazdaként.
* Ha szervezeti fiókkal van bejelentkezve, közös rendszergazdaként a szervezet más szervezeti fiókot is hozzáadhat. Például abby@contoso.com adhat hozzá bob@contoso.com , szolgáltatás-rendszergazdai vagy társadminisztrátori, nem adható hozzá, de john@notcontoso.com kivéve, ha john@notcontoso.com az alapértelmezett könyvtár. Bejelentkezve, munkahelyi és iskolai fiókok felhasználók továbbra is a Microsoft Account felhasználók hozzáadása a szolgáltatás rendszergazdájának vagy Társadminisztrátorának.
* Most, hogy lehet egy olyan szervezeti fiókkal jelentkezzen be Azure, az alábbiakban szolgáltatás-rendszergazda és a közös rendszergazdai fiókra vonatkozó követelmények változásai:

  | Bejelentkezés módszer | Adja hozzá a Microsoft Account vagy alapértelmezett könyvtárban lévő felhasználók vagy a rendszergazdai (SA)? | Adja hozzá a szervezeti fiók ugyanazon a szervezeten belül, vagy a rendszergazdai (SA)? | Szervezeti fiók hozzáadása a másik szervezet Hitelesítésszolgáltatói vagy a rendszergazdai (SA)? |
  | --- | --- | --- | --- |
  |  Microsoft-fiók |Igen |Nem |Nem |
  |  Szervezeti fiók |Igen |Igen |Nem |

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>További információ az erőforrás hozzáférés-vezérlés és az Active Directory
* Hogyan szabályozott erőforrások elérése a Microsoft Azure-ban kapcsolatos további információkért lásd: [az az Azure-erőforrások hozzáférésének megismerése](../active-directory/active-directory-understanding-resource-access.md).
* Azure Active Directoryval kapcsolatos további információkért lásd: [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) és [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.

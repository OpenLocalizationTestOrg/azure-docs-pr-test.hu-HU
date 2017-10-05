---
title: "Az Office 365-bérlő használata Azure-előfizetés |} Microsoft Docs"
description: "Ismerje meg az Office 365-címtár (bérlői) felvétele az Azure-előfizetésre."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: cjiang
ms.openlocfilehash: 7affb4a83cdb8ababef60e786a3c824e7477ed68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="associate-an-office-365-tenant-to-an-azure-subscription"></a>Az Office 365-bérlő egy Azure-előfizetések társítása
Csatolja az önálló Azure és az Office 365-előfizetések, hogy az Azure-előfizetéssel az Office 365-bérlő érhető el. Az előfizetések csatolásához jelentkezzen be az Azure-bA az Azure szolgáltatás-rendszergazdai fiók, adja hozzá a könyvtárát, és az Office 365 munkahelyi és iskolai fiókok hozzáadása az Azure Active Directory-bérlő.

Ha a felhasználók Office 365-előfizetéssel szeretne az Azure Active Directory-példányban, vagy az Office 365-fiókkal, de nem Azure-fiókkal rendelkezik, tekintse meg [regisztráció Office 365-fiókkal az Azure](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Előkészületek
* Az Azure-előfizetés szolgáltatás-rendszergazda hitelesítő adatait kell rendelkeznie. Közös rendszergazdai fiókok nem hajtható végre néhány cikkben leírt lépéseket. A szolgáltatás-rendszergazda módosításához lásd [hozzáadása vagy módosítása az Azure-rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Az Office 365-bérlő globális rendszergazdájának hitelesítő adatait kell rendelkeznie.
* A szolgáltatás-rendszergazda e-mail címe nem lehet az Office 365-bérlő.
* A szolgáltatás-rendszergazda e-mail címe nem egyezik, az Office 365-bérlő globális rendszergazdái.
* Ha egy e-mail címet, amely a Microsoft-fiók és a szervezeti fiók használatához, átmenetileg váltson egy másik Microsoft-fiók használata az Azure-előfizetéshez a szolgáltatás rendszergazdájával. A Microsoft-fiókjával a hozhat létre a [Microsoft-fiók bejelentkezési oldalának](https://signup.live.com/).

## <a name="link-office-365-tenant-to-azure-subscription"></a>Office 365-bérlő hivatkozás Azure-előfizetéshez
Rendelje hozzá a az Office 365-bérlő az Azure-előfizetéshez, kövesse az alábbi lépéseket:

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a>1. lépés: Az Office 365-bérlő hozzáadása az Azure-előfizetéshez

1. Jelentkezzen be a [a klasszikus Azure portálon](https://manage.windowsazure.com/) a szolgáltatás rendszergazdai hitelesítő adataival.

    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. A bal oldali panelen válassza ki a **ACTIVE DIRECTORY**. Ne tekintse meg az Office 365-bérlő. Ha azt látja, folytassa a [2. lépés: módosítsa a könyvtárat, az Azure-előfizetéshez társított](#Step2).
   
   ![Képernyőkép az Active Directory-bejegyzés](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. Válassza ki **új** > **DIRECTORY** > **egyéni létrehozás**.
   
    ![Képernyőfelvétel az Azure Active Directory egyéni létrehozása](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. Az a **Hozzáadás directory** lap **DIRECTORY**, jelölje be **meglévő címtár használata**. Válassza ki **készen állok a kijelentkezésre**, és válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Képernyőkép a "Meglévő könyvtár használata"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. Aláírása után jelentkezzen be az Office 365-bérlő globális rendszergazdájának hitelesítő adatait.
   
    ![Képernyőkép az Office 365 globális rendszergazdai bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. Válassza ki **továbbra is**.
   
    ![Képernyőkép a ellenőrzése](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. Válassza ki **azonnali Kijelentkezés**.
   
    ![Képernyőkép a kijelentkezési](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. Jelentkezzen be a [a klasszikus Azure portálon](https://manage.windowsazure.com/) a szolgáltatás rendszergazdai hitelesítő adataival.
   
    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. Meg kell jelennie az Office 365-bérlő az irányítópulton.
   
    ![Irányítópultjának képernyőképe](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>2. lépés: Módosítsa a könyvtárat, az Azure-előfizetéshez társított
   
1. Válassza ki **beállítások**.
   
    ![Képernyőfelvétel az Azure klasszikus portál beállítások ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. Az Azure-előfizetéshez, majd válassza ki és **könyvtár szerkesztése**.

    ![Képernyőfelvétel az Azure-előfizetés Szerkesztés könyvtára](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. Válassza ki **következő** ![tovább ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Képernyőkép a "Módosítása a társított könyvtár"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. Tekintse át az érintett fiókokat. Minden társrendszergazdák és [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) a meglévő erőforrás-csoportok a hozzárendelt hozzáféréssel rendelkező felhasználók törlődnek. A figyelmeztetést kap csak akkor említi, társrendszergazdák eltávolítása.
      
    ![Képernyőkép a a közös rendszergazdai fiókok, el kell távolítani.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Képernyőkép a egy példa-felhasználói fiókot el kell távolítani.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. Válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-to-the-azure-active-directory-tenant"></a>3. lépés: Az Office 365 szervezeti fiókokat hozzá társadminisztrátorként az Azure Active Directory-bérlő
   
1. Válassza ki a **RENDSZERGAZDÁK** lapra, majd válassza ki **hozzáadása**.
   
    ![Képernyőfelvétel az Azure klasszikus portál beállításai rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. Adja meg az Office 365-bérlő szervezeti fiókkal, az Azure-előfizetés, majd válassza ki és **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Képernyőfelvétel az Azure hozzáadása társadminisztrátoraként párbeszédpanel](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. Lépjen vissza a **RENDSZERGAZDÁK** fülre. Meg kell jelennie a szervezeti fiók társadminisztrátoraként jelenik meg.
   
    ![Képernyőkép a rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  A közös rendszergazdai fiókkal az Azure-hozzáférés tesztelése.
   
    a. Jelentkezzen ki a klasszikus Azure portálon.
   
    b. Nyissa meg az [Azure portált](https://portal.azure.com/).
   
    c. Adja meg a közös rendszergazdájának hitelesítő adatait, majd válassza ki **bejelentkezés**.
   
    ![Képernyőfelvétel az Azure bejelentkezési oldal](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.



---
title: "Azure-előfizetés aaaUse az Office 365 bérlői |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd az Office 365-könyvtár (bérlői) tooan Azure-előfizetés."
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
ms.openlocfilehash: e560370417bd074a7e37ceb7c60da45dbbbcf775
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="associate-an-office-365-tenant-tooan-azure-subscription"></a>Az Office 365 bérlői tooan Azure-előfizetés társítása
Csatolja az önálló Azure és az Office 365-előfizetések, hogy az Azure-előfizetéshez hello Office 365 bérlői érhető el. toolink előfizetése, jelentkezzen be az Azure szolgáltatás-rendszergazdai fiók, hello tooAzure adja hozzá a könyvtárát, és adja hozzá az Office 365 hello munkahelyi és iskolai fiókok toohello Azure Active Directory-bérlő.

Ha a felhasználók Office 365-előfizetéssel szeretne az Azure Active Directory-példányban, vagy az Office 365-fiókkal, de nem Azure-fiókkal rendelkezik, tekintse meg [regisztráció Office 365-fiókkal az Azure](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Előkészületek
* Hello hello Azure-előfizetés szolgáltatás-rendszergazda hitelesítő adataival kell rendelkeznie. Közös rendszergazdai fiókok nem hajtható végre néhány hello cikkben leírt lépéseket. toochange a szolgáltatás-rendszergazda, lásd: [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Hello hello Office 365-bérlő globális rendszergazdájának hitelesítő adatait kell rendelkeznie.
* hello hello szolgáltatás-rendszergazda e-mail címe nem lehet hello Office 365-bérlő.
* hello hello szolgáltatás-rendszergazda e-mail címe nem egyezik az hello Office 365-bérlő globális rendszergazdái.
* Egy e-mail címet, amely a Microsoft-fiók és a szervezeti fiók használatakor, átmenetileg váltson az Azure-előfizetés toouse hello szolgáltatás rendszergazdája másik Microsoft-fiókkal. Létrehozhat egy Microsoft-fiók: hello [Microsoft-fiók bejelentkezési oldalának](https://signup.live.com/).

## <a name="link-office-365-tenant-tooazure-subscription"></a>Office 365 bérlői tooAzure előfizetési hivatkozásra.
tooassociate hello Office 365 bérlői toohello Azure-előfizetésre, kövesse az alábbi lépéseket:

### <a name="step-1-add-office-365-tenant-tooyour-azure-subscription"></a>1. lépés: Az Office 365 bérlői tooyour Azure-előfizetés hozzáadása

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello szolgáltatás rendszergazdai hitelesítő adataival.

    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. Hello bal oldali ablaktáblában jelöljön ki **ACTIVE DIRECTORY**. Hello Office 365 bérlői nem látható. Ha azt látja, hagyja ki túl[2. lépés: hello Azure-előfizetéssel társított hello könyvtárváltás](#Step2).
   
   ![Képernyőkép az Active Directory-bejegyzés](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. Válassza ki **új** > **DIRECTORY** > **egyéni létrehozás**.
   
    ![Képernyőfelvétel az Azure Active Directory egyéni létrehozása](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. A hello **Hozzáadás directory** lap **DIRECTORY**, jelölje be **meglévő címtár használata**. Válassza ki **kijelentkezésre készen toobe vagyok**, és válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Képernyőkép a "Meglévő könyvtár használata"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. Aláírása után jelentkezzen be az Office 365-bérlő hello globális rendszergazda hitelesítő adatait.
   
    ![Képernyőkép az Office 365 globális rendszergazdai bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. Válassza ki **továbbra is**.
   
    ![Képernyőkép a ellenőrzése](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. Válassza ki **azonnali Kijelentkezés**.
   
    ![Képernyőkép a kijelentkezési](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) hello szolgáltatás rendszergazdai hitelesítő adataival.
   
    ![Képernyőfelvétel az Azure-bejelentkezés](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. Meg kell jelennie az Office 365-bérlő hello irányítópulton.
   
    ![Irányítópultjának képernyőképe](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>2. lépés: Hello Azure-előfizetéssel társított hello könyvtár módosítása
   
1. Válassza ki **beállítások**.
   
    ![Képernyőfelvétel az Azure klasszikus portál beállítások ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. Az Azure-előfizetéshez, majd válassza ki és **könyvtár szerkesztése**.

    ![Képernyőfelvétel az Azure-előfizetés Szerkesztés könyvtára](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. Válassza ki **következő** ![tovább ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Képernyőkép a "módosítás társított directory hello"szövegrészt](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. Tekintse át az érintett hello fiókok. Minden társrendszergazdák és [szerepköralapú hozzáférés-vezérlést (RBAC)](../active-directory/role-based-access-control-configure.md) hello meglévő erőforráscsoportok a hozzárendelt hozzáféréssel rendelkező felhasználók törlődnek. hello figyelmeztetést kap csak akkor említi, társrendszergazdák hello eltávolítását.
      
    ![Képernyőkép a hello közös rendszergazdai fiókok toobe eltávolítva.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Képernyőkép a egy példa felhasználói fiók toobe eltávolítva.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. Válassza ki **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-toohello-azure-active-directory-tenant"></a>3. lépés: Adja hozzá az Office 365 munkahelyi és iskolai fiókok társrendszergazdák toohello Azure Active Directory-bérlő
   
1. Jelölje be hello **RENDSZERGAZDÁK** lapra, majd válassza ki **hozzáadása**.
   
    ![Képernyőfelvétel az Azure klasszikus portál beállításai rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. Adja meg az Office 365-bérlő szervezeti fiókkal, hello Azure-előfizetésre, majd válassza ki és **Complete** ![befejezéséhez ikon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Képernyőfelvétel az Azure hozzáadása társadminisztrátoraként párbeszédpanel](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. Lépjen vissza toohello **RENDSZERGAZDÁK** fülre. Meg kell jelennie hello szervezeti fiók társadminisztrátoraként jelenik meg.
   
    ![Képernyőkép a rendszergazdák lap](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  Teszt hozzáférés tooAzure hello közös rendszergazdai fiókkal.
   
    a. Kijelentkezik hello a klasszikus Azure portálon.
   
    b. Nyissa meg hello [Azure-portálon](https://portal.azure.com/).
   
    c. Adja meg a hello hello közös rendszergazdájának hitelesítő adatait, majd válassza ki **bejelentkezés**.
   
    ![Képernyőfelvétel az Azure bejelentkezési oldal](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.



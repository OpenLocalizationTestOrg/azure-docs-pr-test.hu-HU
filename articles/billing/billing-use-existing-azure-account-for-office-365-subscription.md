---
title: "az Azure-fiók az Office 365 szolgáltatáshoz aaaSign |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate az Office 365-előfizetéshez az Azure-fiók használatával"
services: 
documentationcenter: 
author: JiangChen79
manager: adpick
editor: 
tags: billing,top-support-issue
ms.assetid: 
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: cjiang
ms.openlocfilehash: d19e1c1edff0b9658b639e796a72bbf4e87b9c3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a>Regisztráljon az Azure-fiókjába az Office 365-előfizetéssel
Ha Ön Azure-előfizető, használhatja az Azure-fiók toosign az Office 365-előfizetéssel szolgáltatáshoz. Ha egy szervezet, amely rendelkezik az Azure-előfizetés részét, a felhasználók az Office 365-előfizetés a meglévő Azure Active Directoryban (Azure AD) is létrehozhat. Iratkozzon fel tooOffice 365 egy olyan fiókkal, amely az Azure Active Directory-bérlő globális rendszergazdája és számlázási rendszergazda jogokkal rendelkezik. További információkért lásd: [ellenőrizze a fiók engedélyeit az Azure AD](#RoleInAzureAD) és [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).

Ha már rendelkezik Office 365-fiókkal, mind az Azure-előfizetéssel, akkor [az Office 365 bérlői tooan Azure-előfizetés társítása](billing-add-office-365-tenant-to-azure-subscription.md).

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a>Az Azure-fiók segítségével könnyebben nyerhet Office 365-előfizetéssel

1. Nyissa meg toohello [Office 365, termékoldala](https://products.office.com/business), és válassza ki a tervet.
2. Kattintson a **bejelentkezés** hello oldal jobb felső sarkában hello.

    ![Office 365 próba oldalát bemutató képernyőkép](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. Jelentkezzen be Azure-fiók hitelesítő adataival. Ha a szervezete egy előfizetést hoz létre, használja az Azure-fiókkal, amely tagja a hello globális felügyeleti vagy számlázási rendszergazda directory szerepkör az Azure Active Directory-bérlőben.

    ![Képernyőkép az Office 365-bejelentkezés](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. Kattintson a **Kipróbálom most**.

    ![Képernyőkép a megerősíti, hogy az Office 365 rendelés.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. Lapján hello rendelés fogadását, kattintson a **Folytatás**.

    ![Képernyőfelvétel a hello Office 365 rendelés visszaigazolást](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

Most már készen is. Ha a szervezet számára létrehozott hello Office 365-előfizetéssel, használja a következő lépéseket toocheck, amelyek az Azure AD-felhasználók az Office 365-ben közül hello.

1. Nyissa meg a hello Office 365 felügyeleti központot.
2. Bontsa ki a **felhasználók**, és kattintson a **aktív felhasználók**.

    ![Képernyőfelvétel a hello Office 365 felügyeleti központ felhasználók](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

A regisztrációt követően, Office 365-előfizetéssel hello szerepel-e toohello azonos Azure Active Directory-példányban, amely az Azure-előfizetéshez tartozik. További információkért lásd: [további információk az Office 365 és az Azure-előfizetések](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) és [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a id="RoleInAzureAD"></a>Ellenőrizze a fiók engedélyeit az Azure ad-ben
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **további szolgáltatások**, majd keresse meg a **Active Directory**.

    ![Képernyőkép az Active Directory-hello Azure-portálon](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. Kattintson a **felhasználók és csoportok** > **minden felhasználó**.
4. Válassza ki a hello felhasználói nevét. 

    ![Képernyőkép a hello Azure Active Directory-felhasználók](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. Kattintson a **Directory szerepkör**.
  
    ![Képernyőkép a hello Azure-portál címtár szerepkör](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  hello szerepkör **globális rendszergazda** vagy **korlátozott rendszergazdai** > **számlázási rendszergazda** van szükség toocreate felhasználók az Office 365-előfizetéssel a meglévő Azure Active Directoryban.

    ![Képernyőkép az Azure portál címtár szerepkörrel számlázási rendszergazda](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget. 

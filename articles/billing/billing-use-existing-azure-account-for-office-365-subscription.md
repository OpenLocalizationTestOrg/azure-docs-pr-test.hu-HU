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
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a><span data-ttu-id="39482-103">Regisztráljon az Azure-fiókjába az Office 365-előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="39482-103">Sign up for an Office 365 subscription with your Azure account</span></span>
<span data-ttu-id="39482-104">Ha Ön Azure-előfizető, használhatja az Azure-fiók toosign az Office 365-előfizetéssel szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="39482-104">If you're Azure subscriber, you can use your Azure account toosign up for an Office 365 subscription.</span></span> <span data-ttu-id="39482-105">Ha egy szervezet, amely rendelkezik az Azure-előfizetés részét, a felhasználók az Office 365-előfizetés a meglévő Azure Active Directoryban (Azure AD) is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="39482-105">If you're part of an organization that has an Azure subscription, you can create Office 365 subscriptions for users in your existing Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="39482-106">Iratkozzon fel tooOffice 365 egy olyan fiókkal, amely az Azure Active Directory-bérlő globális rendszergazdája és számlázási rendszergazda jogokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="39482-106">Sign up tooOffice 365 using an account that has Global Admin or Billing Admin permissions in your Azure Active Directory tenant.</span></span> <span data-ttu-id="39482-107">További információkért lásd: [ellenőrizze a fiók engedélyeit az Azure AD](#RoleInAzureAD) és [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="39482-107">For more information, see [Check my account permissions in Azure AD](#RoleInAzureAD) and [Assigning administrator roles in Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="39482-108">Ha már rendelkezik Office 365-fiókkal, mind az Azure-előfizetéssel, akkor [az Office 365 bérlői tooan Azure-előfizetés társítása](billing-add-office-365-tenant-to-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="39482-108">If you already have both an Office 365 account and an Azure subscription, you can [Associate an Office 365 tenant tooan Azure subscription](billing-add-office-365-tenant-to-azure-subscription.md).</span></span>

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a><span data-ttu-id="39482-109">Az Azure-fiók segítségével könnyebben nyerhet Office 365-előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="39482-109">Get an Office 365 subscription by using your Azure account</span></span>

1. <span data-ttu-id="39482-110">Nyissa meg toohello [Office 365, termékoldala](https://products.office.com/business), és válassza ki a tervet.</span><span class="sxs-lookup"><span data-stu-id="39482-110">Go toohello [Office 365 product page](https://products.office.com/business), and select a plan.</span></span>
2. <span data-ttu-id="39482-111">Kattintson a **bejelentkezés** hello oldal jobb felső sarkában hello.</span><span class="sxs-lookup"><span data-stu-id="39482-111">Click **Sign in** on hello upper-right corner of hello page.</span></span>

    ![Office 365 próba oldalát bemutató képernyőkép](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. <span data-ttu-id="39482-113">Jelentkezzen be Azure-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="39482-113">Sign in with your Azure account credentials.</span></span> <span data-ttu-id="39482-114">Ha a szervezete egy előfizetést hoz létre, használja az Azure-fiókkal, amely tagja a hello globális felügyeleti vagy számlázási rendszergazda directory szerepkör az Azure Active Directory-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="39482-114">If you're creating a subscription for your organization, use an Azure account that's a member of hello Global Admin or Billing Admin directory role in your Azure Active Directory tenant.</span></span>

    ![Képernyőkép az Office 365-bejelentkezés](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. <span data-ttu-id="39482-116">Kattintson a **Kipróbálom most**.</span><span class="sxs-lookup"><span data-stu-id="39482-116">Click **Try now**.</span></span>

    ![Képernyőkép a megerősíti, hogy az Office 365 rendelés.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. <span data-ttu-id="39482-118">Lapján hello rendelés fogadását, kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="39482-118">On hello order receipt page, click **Continue**.</span></span>

    ![Képernyőfelvétel a hello Office 365 rendelés visszaigazolást](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

<span data-ttu-id="39482-120">Most már készen is.</span><span class="sxs-lookup"><span data-stu-id="39482-120">Now you're all set.</span></span> <span data-ttu-id="39482-121">Ha a szervezet számára létrehozott hello Office 365-előfizetéssel, használja a következő lépéseket toocheck, amelyek az Azure AD-felhasználók az Office 365-ben közül hello.</span><span class="sxs-lookup"><span data-stu-id="39482-121">If you created hello Office 365 subscription for your organization, use hello following steps toocheck that your Azure AD users are now in Office 365.</span></span>

1. <span data-ttu-id="39482-122">Nyissa meg a hello Office 365 felügyeleti központot.</span><span class="sxs-lookup"><span data-stu-id="39482-122">Open hello Office 365 admin center.</span></span>
2. <span data-ttu-id="39482-123">Bontsa ki a **felhasználók**, és kattintson a **aktív felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="39482-123">Expand **USERS**, and then click **Active Users**.</span></span>

    ![Képernyőfelvétel a hello Office 365 felügyeleti központ felhasználók](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

<span data-ttu-id="39482-125">A regisztrációt követően, Office 365-előfizetéssel hello szerepel-e toohello azonos Azure Active Directory-példányban, amely az Azure-előfizetéshez tartozik.</span><span class="sxs-lookup"><span data-stu-id="39482-125">After you sign up, hello Office 365 subscription is added toohello same Azure Active Directory instance that your Azure subscription belongs to.</span></span> <span data-ttu-id="39482-126">További információkért lásd: [további információk az Office 365 és az Azure-előfizetések](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) és [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="39482-126">For more information, see [More about Azure and Office 365 subscriptions](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) and [How Azure subscriptions are associated with Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).</span></span>

## <span data-ttu-id="39482-127"><a id="RoleInAzureAD"></a>Ellenőrizze a fiók engedélyeit az Azure ad-ben</span><span class="sxs-lookup"><span data-stu-id="39482-127"><a id="RoleInAzureAD"></a>Check my account permissions in Azure AD</span></span>
1. <span data-ttu-id="39482-128">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="39482-128">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="39482-129">Kattintson a **további szolgáltatások**, majd keresse meg a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="39482-129">Click **More services**, and then search for **Active Directory**.</span></span>

    ![Képernyőkép az Active Directory-hello Azure-portálon](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. <span data-ttu-id="39482-131">Kattintson a **felhasználók és csoportok** > **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="39482-131">Click **Users and groups** > **All users**.</span></span>
4. <span data-ttu-id="39482-132">Válassza ki a hello felhasználói nevét.</span><span class="sxs-lookup"><span data-stu-id="39482-132">Select hello user name.</span></span> 

    ![Képernyőkép a hello Azure Active Directory-felhasználók](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. <span data-ttu-id="39482-134">Kattintson a **Directory szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="39482-134">Click **Directory role**.</span></span>
  
    ![Képernyőkép a hello Azure-portál címtár szerepkör](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  <span data-ttu-id="39482-136">hello szerepkör **globális rendszergazda** vagy **korlátozott rendszergazdai** > **számlázási rendszergazda** van szükség toocreate felhasználók az Office 365-előfizetéssel a meglévő Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="39482-136">hello role **Global administrator** or **Limited administrator** > **Billing administrator** is required toocreate an Office 365 subscription for users in your existing Azure Active Directory.</span></span>

    ![Képernyőkép az Azure portál címtár szerepkörrel számlázási rendszergazda](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="39482-138">Segítség</span><span class="sxs-lookup"><span data-stu-id="39482-138">Need help?</span></span> <span data-ttu-id="39482-139">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="39482-139">Contact support.</span></span>
<span data-ttu-id="39482-140">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.</span><span class="sxs-lookup"><span data-stu-id="39482-140">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span> 

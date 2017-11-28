---
title: "Regisztráljon az Azure-fiók az Office 365 |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre az Office 365-előfizetéssel az Azure-fiók használatával"
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
ms.openlocfilehash: 1c6e277e321980aaf30f821dbb41c7eaf296b4cf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="sign-up-for-an-office-365-subscription-with-your-azure-account"></a><span data-ttu-id="2347c-103">Regisztráljon az Azure-fiókjába az Office 365-előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="2347c-103">Sign up for an Office 365 subscription with your Azure account</span></span>
<span data-ttu-id="2347c-104">Ha Ön Azure-előfizető, az Azure-fiók segítségével előfizetés az Office 365-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="2347c-104">If you're Azure subscriber, you can use your Azure account to sign up for an Office 365 subscription.</span></span> <span data-ttu-id="2347c-105">Ha egy szervezet, amely rendelkezik az Azure-előfizetés részét, a felhasználók az Office 365-előfizetés a meglévő Azure Active Directoryban (Azure AD) is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="2347c-105">If you're part of an organization that has an Azure subscription, you can create Office 365 subscriptions for users in your existing Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2347c-106">Jelentkezzen egy olyan fiókkal, amely az Azure Active Directory-bérlő globális rendszergazdája és számlázási rendszergazda jogokkal rendelkezik Office 365.</span><span class="sxs-lookup"><span data-stu-id="2347c-106">Sign up to Office 365 using an account that has Global Admin or Billing Admin permissions in your Azure Active Directory tenant.</span></span> <span data-ttu-id="2347c-107">További információkért lásd: [ellenőrizze a fiók engedélyeit az Azure AD](#RoleInAzureAD) és [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="2347c-107">For more information, see [Check my account permissions in Azure AD](#RoleInAzureAD) and [Assigning administrator roles in Azure Active Directory](../active-directory/active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="2347c-108">Ha már rendelkezik Office 365-fiókkal, mind az Azure-előfizetéssel, akkor [társítsa Azure-előfizetéshez az Office 365-bérlő](billing-add-office-365-tenant-to-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="2347c-108">If you already have both an Office 365 account and an Azure subscription, you can [Associate an Office 365 tenant to an Azure subscription](billing-add-office-365-tenant-to-azure-subscription.md).</span></span>

## <a name="get-an-office-365-subscription-by-using-your-azure-account"></a><span data-ttu-id="2347c-109">Az Azure-fiók segítségével könnyebben nyerhet Office 365-előfizetéssel</span><span class="sxs-lookup"><span data-stu-id="2347c-109">Get an Office 365 subscription by using your Azure account</span></span>

1. <span data-ttu-id="2347c-110">Lépjen a [Office 365, termékoldala](https://products.office.com/business), és válassza ki a tervet.</span><span class="sxs-lookup"><span data-stu-id="2347c-110">Go to the [Office 365 product page](https://products.office.com/business), and select a plan.</span></span>
2. <span data-ttu-id="2347c-111">Kattintson a **bejelentkezés** a lap jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="2347c-111">Click **Sign in** on the upper-right corner of the page.</span></span>

    ![Office 365 próba oldalát bemutató képernyőkép](./media/billing-use-existing-azure-account-office-365-subscription/12-office-365-trial-page.png)
3. <span data-ttu-id="2347c-113">Jelentkezzen be Azure-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="2347c-113">Sign in with your Azure account credentials.</span></span> <span data-ttu-id="2347c-114">Ha a szervezete egy előfizetést hoz létre, használja az Azure-fiók, amely az az Azure Active Directory-bérlő globális rendszergazdája és számlázási rendszergazda szerepkör tagja.</span><span class="sxs-lookup"><span data-stu-id="2347c-114">If you're creating a subscription for your organization, use an Azure account that's a member of the Global Admin or Billing Admin directory role in your Azure Active Directory tenant.</span></span>

    ![Képernyőkép az Office 365-bejelentkezés](./media/billing-use-existing-azure-account-office-365-subscription/13-office-365-sign-in.png)
4. <span data-ttu-id="2347c-116">Kattintson a **Kipróbálom most**.</span><span class="sxs-lookup"><span data-stu-id="2347c-116">Click **Try now**.</span></span>

    ![Képernyőkép a megerősíti, hogy az Office 365 rendelés.](./media/billing-use-existing-azure-account-office-365-subscription/14-office-365-confirm-your-order.png)
5. <span data-ttu-id="2347c-118">A sorrend fogadását lapján kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="2347c-118">On the order receipt page, click **Continue**.</span></span>

    ![Az Office 365 rendelés fogadását képernyőképe](./media/billing-use-existing-azure-account-office-365-subscription/15-office-365-order-receipt.png)

<span data-ttu-id="2347c-120">Most már készen is.</span><span class="sxs-lookup"><span data-stu-id="2347c-120">Now you're all set.</span></span> <span data-ttu-id="2347c-121">Ha a szervezet hozott létre az Office 365-előfizetéssel, az alábbi lépések segítségével ellenőrizze, hogy az az Azure AD-felhasználók mostantól az Office 365-ben.</span><span class="sxs-lookup"><span data-stu-id="2347c-121">If you created the Office 365 subscription for your organization, use the following steps to check that your Azure AD users are now in Office 365.</span></span>

1. <span data-ttu-id="2347c-122">Nyissa meg az Office 365 felügyeleti központot.</span><span class="sxs-lookup"><span data-stu-id="2347c-122">Open the Office 365 admin center.</span></span>
2. <span data-ttu-id="2347c-123">Bontsa ki a **felhasználók**, és kattintson a **aktív felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="2347c-123">Expand **USERS**, and then click **Active Users**.</span></span>

    ![Az Office 365 felügyeleti központ felhasználók képernyőképe](./media/billing-use-existing-azure-account-office-365-subscription/16-office-365-admin-center-users.png)

<span data-ttu-id="2347c-125">A regisztrációt követően, az Office 365-előfizetéssel hozzáadódik az Azure Active Directory példányt, amely az Azure-előfizetéshez tartozik.</span><span class="sxs-lookup"><span data-stu-id="2347c-125">After you sign up, the Office 365 subscription is added to the same Azure Active Directory instance that your Azure subscription belongs to.</span></span> <span data-ttu-id="2347c-126">További információkért lásd: [további információk az Office 365 és az Azure-előfizetések](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) és [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="2347c-126">For more information, see [More about Azure and Office 365 subscriptions](billing-use-existing-office-365-account-azure-subscription.md#more-about-subs) and [How Azure subscriptions are associated with Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md).</span></span>

## <span data-ttu-id="2347c-127"><a id="RoleInAzureAD"></a>Ellenőrizze a fiók engedélyeit az Azure ad-ben</span><span class="sxs-lookup"><span data-stu-id="2347c-127"><a id="RoleInAzureAD"></a>Check my account permissions in Azure AD</span></span>
1. <span data-ttu-id="2347c-128">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2347c-128">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2347c-129">Kattintson a **további szolgáltatások**, majd keresse meg a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2347c-129">Click **More services**, and then search for **Active Directory**.</span></span>

    ![Képernyőkép az Active Directory az Azure-portálon](./media/billing-use-existing-azure-account-office-365-subscription/billing-more-services-active-directory.png)
3. <span data-ttu-id="2347c-131">Kattintson a **felhasználók és csoportok** > **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2347c-131">Click **Users and groups** > **All users**.</span></span>
4. <span data-ttu-id="2347c-132">Válassza ki a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="2347c-132">Select the user name.</span></span> 

    ![Képernyőkép az Azure Active Directory-felhasználók](./media/billing-use-existing-azure-account-office-365-subscription/billing-users-groups.png)

5. <span data-ttu-id="2347c-134">Kattintson a **Directory szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="2347c-134">Click **Directory role**.</span></span>
  
    ![Az Azure portál directory szerepkör képernyőkép](./media/billing-use-existing-azure-account-office-365-subscription/billing-user-directory-role.png)
6.  <span data-ttu-id="2347c-136">A szerepkör **globális rendszergazda** vagy **korlátozott rendszergazdai** > **számlázási rendszergazda** a meglévő Azure Active Directory szolgáltatásban a felhasználók Office 365-előfizetéssel létrehozásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="2347c-136">The role **Global administrator** or **Limited administrator** > **Billing administrator** is required to create an Office 365 subscription for users in your existing Azure Active Directory.</span></span>

    ![Képernyőkép az Azure portál címtár szerepkörrel számlázási rendszergazda](./media/billing-use-existing-azure-account-office-365-subscription/billing-directoryrole-limited.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="2347c-138">Segítség</span><span class="sxs-lookup"><span data-stu-id="2347c-138">Need help?</span></span> <span data-ttu-id="2347c-139">Forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="2347c-139">Contact support.</span></span>
<span data-ttu-id="2347c-140">Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma elhárítva gyors eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2347c-140">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span> 

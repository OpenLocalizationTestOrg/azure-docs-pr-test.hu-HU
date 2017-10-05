---
title: "Felhasználó hozzárendelése az Azure Active Directory rendszergazdai szerepkörök |} Microsoft Docs"
description: "Ismerteti, hogyan módosíthatja a felhasználói felügyeleti adatokat az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: bfadf133154488f9827cfbeaa98ddb0eb84b52f6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a><span data-ttu-id="cd18b-103">Felhasználó hozzárendelése az Azure Active Directory rendszergazdai szerepkörök</span><span class="sxs-lookup"><span data-stu-id="cd18b-103">Assign a user to administrator roles in Azure Active Directory</span></span>
<span data-ttu-id="cd18b-104">Ez a cikk azt ismerteti, hogyan egy rendszergazdai szerepkör hozzárendelése az Azure Active Directory (Azure AD) felhasználó.</span><span class="sxs-lookup"><span data-stu-id="cd18b-104">This article explains how to assign an administrative role to a user in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="cd18b-105">A szervezetben új felhasználók hozzáadásával kapcsolatos további információkért lásd: [új felhasználók hozzáadása az Azure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cd18b-105">For information about adding new users in your organization, see [Add new users to Azure Active Directory](active-directory-users-create-azure-portal.md).</span></span> <span data-ttu-id="cd18b-106">A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzájuk rendelhet szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="cd18b-106">Added users don't have administrator permissions by default, but you can assign roles to them at any time.</span></span>

## <a name="assign-a-role-to-a-user"></a><span data-ttu-id="cd18b-107">A szerepkör hozzárendelése felhasználóhoz</span><span class="sxs-lookup"><span data-stu-id="cd18b-107">Assign a role to a user</span></span>
1. <span data-ttu-id="cd18b-108">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="cd18b-108">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="cd18b-109">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="cd18b-109">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="cd18b-111">Az a **felhasználók és csoportok** panelen válassza **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cd18b-111">On the **Users and groups** blade, select **All users**.</span></span>

   ![Az összes felhasználók panel megnyitása](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="cd18b-113">Az a **felhasználók és csoportok - minden felhasználó** panelen válasszon ki egy felhasználót a listáról.</span><span class="sxs-lookup"><span data-stu-id="cd18b-113">On the **Users and groups - All users** blade, select a user from the list.</span></span>
5. <span data-ttu-id="cd18b-114">A kiválasztott felhasználó paneljén válassza **Directory szerepkör**, majd rendelje hozzá a felhasználót a szerepkörhöz a **Directory szerepkör** listája.</span><span class="sxs-lookup"><span data-stu-id="cd18b-114">On the blade for the selected user, select **Directory role**, and then assign the user to a role from the **Directory role** list.</span></span> <span data-ttu-id="cd18b-115">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="cd18b-115">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>

      ![A felhasználó hozzárendelése egy szerepkörhöz](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. <span data-ttu-id="cd18b-117">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="cd18b-117">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd18b-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd18b-118">Next steps</span></span>
* [<span data-ttu-id="cd18b-119">Felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cd18b-119">Add a user</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="cd18b-120">A felhasználó jelszavát az új Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cd18b-120">Reset a user's password in the new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="cd18b-121">A felhasználó munkahelyi adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="cd18b-121">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="cd18b-122">Felhasználói profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="cd18b-122">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="cd18b-123">Felhasználó törlése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cd18b-123">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)

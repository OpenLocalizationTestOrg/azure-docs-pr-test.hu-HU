---
title: "a felhasználó aaaAssign tooadministrator szerepkörök az Azure Active Directoryban |} Microsoft Docs"
description: "Azt ismerteti, hogyan toochange felhasználói felügyeleti adatokat az Azure Active Directoryban"
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
ms.openlocfilehash: 8bb6711c2570843962fe075b6026542a4bca525f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-tooadministrator-roles-in-azure-active-directory"></a><span data-ttu-id="4fec6-103">A felhasználó az Azure Active Directoryban tooadministrator szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4fec6-103">Assign a user tooadministrator roles in Azure Active Directory</span></span>
<span data-ttu-id="4fec6-104">Ez a cikk azt ismerteti, hogyan tooassign egy rendszergazdai szerepkör tooa felhasználó az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4fec6-104">This article explains how tooassign an administrative role tooa user in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="4fec6-105">A szervezetben új felhasználók hozzáadásával kapcsolatos további információkért lásd: [adja hozzá az új felhasználók tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4fec6-105">For information about adding new users in your organization, see [Add new users tooAzure Active Directory](active-directory-users-create-azure-portal.md).</span></span> <span data-ttu-id="4fec6-106">A hozzáadott felhasználók alapértelmezés szerint nem rendelkeznek rendszergazdai engedélyekkel, de bármikor hozzárendelheti a szerepkörökhöz toothem.</span><span class="sxs-lookup"><span data-stu-id="4fec6-106">Added users don't have administrator permissions by default, but you can assign roles toothem at any time.</span></span>

## <a name="assign-a-role-tooa-user"></a><span data-ttu-id="4fec6-107">A szerepkör tooa felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="4fec6-107">Assign a role tooa user</span></span>
1. <span data-ttu-id="4fec6-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="4fec6-108">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="4fec6-109">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="4fec6-109">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="4fec6-111">A hello **felhasználók és csoportok** panelen válassza **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="4fec6-111">On hello **Users and groups** blade, select **All users**.</span></span>

   ![Nyitó hello minden felhasználók panel](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="4fec6-113">A hello **felhasználók és csoportok - minden felhasználó** panelen válassza ki a megfelelő felhasználói hello listából.</span><span class="sxs-lookup"><span data-stu-id="4fec6-113">On hello **Users and groups - All users** blade, select a user from hello list.</span></span>
5. <span data-ttu-id="4fec6-114">A kiválasztott felhasználó hello hello panelen válassza ki a **Directory szerepkör**, és hozzárendelheti a hello hello felhasználói tooa szerepkör **címtár szerepkörrel** lista.</span><span class="sxs-lookup"><span data-stu-id="4fec6-114">On hello blade for hello selected user, select **Directory role**, and then assign hello user tooa role from hello **Directory role** list.</span></span> <span data-ttu-id="4fec6-115">A felhasználói és rendszergazdai szerepkörökről további információért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure AD-ben](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="4fec6-115">For more information about user and administrator roles, see [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles.md).</span></span>

      ![Felhasználói tooa szerepkör hozzárendelése](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. <span data-ttu-id="4fec6-117">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4fec6-117">Select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fec6-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4fec6-118">Next steps</span></span>
* [<span data-ttu-id="4fec6-119">Felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4fec6-119">Add a user</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="4fec6-120">A felhasználó jelszavát hello új Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4fec6-120">Reset a user's password in hello new Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="4fec6-121">A felhasználó munkahelyi adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="4fec6-121">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="4fec6-122">Felhasználói profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="4fec6-122">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="4fec6-123">Felhasználó törlése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4fec6-123">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)

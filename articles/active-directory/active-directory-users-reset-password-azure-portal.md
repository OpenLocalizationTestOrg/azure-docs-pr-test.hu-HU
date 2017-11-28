---
title: "Jelszó alaphelyzetbe állítása, az Azure Active Directoryban |} Microsoft Docs"
description: "Ismerteti az Azure Active Directoryban a felhasználó jelszavának visszaállítása"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8e7c38c7f0d40a310dd0b6bd0e866d2d55115550
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a><span data-ttu-id="203f2-103">Az Azure Active Directoryban a felhasználó jelszavának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="203f2-103">Reset the password for a user in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="203f2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="203f2-104">Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
> * [<span data-ttu-id="203f2-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="203f2-105">Azure classic portal</span></span>](active-directory-create-users-reset-password.md)
>
>

## <a name="how-to-reset-the-password-for-a-user"></a><span data-ttu-id="203f2-106">A felhasználó jelszavának alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="203f2-106">How to reset the password for a user</span></span>
1. <span data-ttu-id="203f2-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="203f2-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="203f2-108">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="203f2-108">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-users-reset-password-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="203f2-110">Az a **felhasználók és csoportok** panelen válassza **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="203f2-110">On the **Users and groups** blade, select **Users**.</span></span>

   ![A felhasználók panel megnyitása](./media/active-directory-users-reset-password-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="203f2-112">A **Felhasználók és csoportok – Felhasználók** panelen válasszon ki egy felhasználót a listából.</span><span class="sxs-lookup"><span data-stu-id="203f2-112">On the **Users and groups - Users** blade, select a user from the list.</span></span>
5. <span data-ttu-id="203f2-113">A kiválasztott felhasználó paneljén válassza az **Áttekintés** elemet, majd a parancssávon válassza ki a **Jelszó alaphelyzetbe állítása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="203f2-113">On the blade for the selected user, select **Overview**, and then in the command bar, select **Reset password**.</span></span>

    ![A jelszó alaphelyzetbe állítása paranccsal](./media/active-directory-users-reset-password-azure-portal/create-users-reset-password-command.png)
6. <span data-ttu-id="203f2-115">Az a **jelszó-átállítási** panelen válassza **jelszó-átállítási**.</span><span class="sxs-lookup"><span data-stu-id="203f2-115">On the **Reset password** blade, select **Reset password**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="203f2-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="203f2-116">Next steps</span></span>
* [<span data-ttu-id="203f2-117">Felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="203f2-117">Add a user</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="203f2-118">Felhasználó hozzárendelése egy szerepkörhöz az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="203f2-118">Assign a user to a role in your Azure AD</span></span>](active-directory-users-assign-role-azure-portal.md)
* [<span data-ttu-id="203f2-119">A felhasználó munkahelyi adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="203f2-119">Change a user's work information</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="203f2-120">Felhasználói profilok kezelése</span><span class="sxs-lookup"><span data-stu-id="203f2-120">Manage user profiles</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="203f2-121">Felhasználó törlése az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="203f2-121">Delete a user in your Azure AD</span></span>](active-directory-users-delete-user-azure-portal.md)

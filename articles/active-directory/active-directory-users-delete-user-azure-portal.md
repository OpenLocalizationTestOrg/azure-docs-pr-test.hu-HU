---
title: "Felhasználó törlése az Azure Active Directoryban olyan könyvtárból |} Microsoft Docs"
description: "Ismerteti az Azure Active Directory a felhasználók és az adatok törlése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bd1c9ccc-2d80-42bf-82cc-db37bb1a1d67
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: 19a4d2c37c785b3b56a2e0e1b6797ec56dad9835
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="delete-a-user-from-a-directory-in-azure-active-directory"></a><span data-ttu-id="2a030-103">A felhasználó töröl egy könyvtárat az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="2a030-103">Delete a user from a directory in Azure Active Directory</span></span>
<span data-ttu-id="2a030-104">Ez a cikk azt ismerteti, hogyan felhasználó törlése az Azure Active Directory (Azure AD) olyan könyvtárból.</span><span class="sxs-lookup"><span data-stu-id="2a030-104">This article explains how to delete a user from a directory in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2a030-105">A szervezetben új felhasználók hozzáadásával kapcsolatos további információkért lásd: [új felhasználók hozzáadása az Azure Active Directory](active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2a030-105">For information about adding new users to your organization, see [Add new users to Azure Active Directory](active-directory-users-create-azure-portal.md).</span></span>

## <a name="to-delete-a-user"></a><span data-ttu-id="2a030-106">Felhasználó törlése</span><span class="sxs-lookup"><span data-stu-id="2a030-106">To delete a user</span></span>
1. <span data-ttu-id="2a030-107">Jelentkezzen be [az Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="2a030-107">Sign in to [the Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="2a030-108">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="2a030-108">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-users-delete-user-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="2a030-110">Az a **felhasználók és csoportok** panelen válassza **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="2a030-110">On the **Users and groups** blade, select **Users**.</span></span>

   ![A felhasználók panel megnyitása](./media/active-directory-users-delete-user-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="2a030-112">A **Felhasználók és csoportok – Felhasználók** panelen válasszon ki egy felhasználót a listából.</span><span class="sxs-lookup"><span data-stu-id="2a030-112">On the **Users and groups - Users** blade, select a user from the list.</span></span>
5. <span data-ttu-id="2a030-113">A kiválasztott felhasználó paneljén válassza **áttekintése**, majd a parancssávon válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="2a030-113">On the blade for the selected user, select **Overview**, and then in the command bar, select **Delete**.</span></span>

    ![A Delete parancs kiválasztása](./media/active-directory-users-delete-user-azure-portal/create-users-delete-command.png)

## <a name="next-steps"></a><span data-ttu-id="2a030-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a030-115">Next steps</span></span>
* [<span data-ttu-id="2a030-116">Új felhasználók hozzáadása az Azure Active Directoryhoz</span><span class="sxs-lookup"><span data-stu-id="2a030-116">Add new users to Azure Active Directory</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="2a030-117">Az Azure Active Directoryban a felhasználó jelszavának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="2a030-117">Reset the password for a user in Azure Active Directory</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="2a030-118">Felhasználó hozzárendelése az Azure Active Directory rendszergazdai szerepkörök</span><span class="sxs-lookup"><span data-stu-id="2a030-118">Assign a user to administrator roles in Azure Active Directory</span></span>](active-directory-users-assign-role-azure-portal.md)
* [<span data-ttu-id="2a030-119">Adja hozzá, vagy egy Azure Active Directoryban lévő felhasználói profil adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="2a030-119">Add or change profile information for a user in Azure Active Directory</span></span>](active-directory-users-work-info-azure-portal.md)
* [<span data-ttu-id="2a030-120">A felhasználó töröl egy könyvtárat az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="2a030-120">Delete a user from a directory in Azure Active Directory</span></span>](active-directory-users-profile-azure-portal.md)

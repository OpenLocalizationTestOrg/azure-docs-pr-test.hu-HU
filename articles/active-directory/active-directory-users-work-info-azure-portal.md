---
title: "Az Azure Active Directoryban egy felhasználó a munkahelyi adatok hozzáadása vagy módosítása |} Microsoft Docs"
description: "Ismerteti, hogyan adja hozzá a telefonszámot, részleg nevek és más felhasználó adatait az Azure Active Directoryban"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a6f16152-53f4-491e-a1c3-157f41bc39fa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: 9f4031da7c6dfbd329d14c52f3a569084edacf20
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-or-change-work-information-for-a-user-in-azure-active-directory"></a><span data-ttu-id="088fb-103">Az Azure Active Directoryban egy felhasználó a munkahelyi adatok hozzáadása vagy módosítása</span><span class="sxs-lookup"><span data-stu-id="088fb-103">Add or change work information for a user in Azure Active Directory</span></span>
<span data-ttu-id="088fb-104">Ez a cikk ismerteti a munkahelyi adatok, például a telefonszámok és az Azure Active Directory (Azure AD) felhasználó számára részlege nevek hozzáadása vagy módosítása.</span><span class="sxs-lookup"><span data-stu-id="088fb-104">This article explains how to add or change work information such as phone numbers or department names for a user in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="088fb-105">A szervezetben új felhasználók hozzáadásával kapcsolatos további információkért lásd: [új felhasználók hozzáadása az Azure Active Directory](active-directory-users-create-external-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="088fb-105">For information about adding new users in your organization, see [Add new users to Azure Active Directory](active-directory-users-create-external-azure-portal.md).</span></span>

## <a name="to-change-work-information"></a><span data-ttu-id="088fb-106">A munkahelyi adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="088fb-106">To change work information</span></span>
1. <span data-ttu-id="088fb-107">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="088fb-107">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="088fb-108">Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** a szövegmezőbe, majd válassza ki azt a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="088fb-108">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Nyitó felhasználók kezelése](./media/active-directory-users-work-info-azure-portal/create-users-user-management.png)
3. <span data-ttu-id="088fb-110">Az a **felhasználók és csoportok** panelen válassza **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="088fb-110">On the **Users and groups** blade, select **Users**.</span></span>

   ![A felhasználók panel megnyitása](./media/active-directory-users-work-info-azure-portal/create-users-open-users-blade.png)
4. <span data-ttu-id="088fb-112">A **Felhasználók és csoportok – Felhasználók** panelen válasszon ki egy felhasználót a listából.</span><span class="sxs-lookup"><span data-stu-id="088fb-112">On the **Users and groups - Users** blade, select a user from the list.</span></span>
5. <span data-ttu-id="088fb-113">A kiválasztott felhasználó paneljén válassza **munkahelyi adatai**.</span><span class="sxs-lookup"><span data-stu-id="088fb-113">On the blade for the selected user, select **Work Info**.</span></span>

    ![Nyitó munkahelyi adatokat](./media/active-directory-users-work-info-azure-portal/active-directory-create-users-work-info.png)
6. <span data-ttu-id="088fb-115">Adja hozzá, vagy módosíthatja a munkahelyi adatait.</span><span class="sxs-lookup"><span data-stu-id="088fb-115">Add or change the work information.</span></span> <span data-ttu-id="088fb-116">Ezt követően a parancssávon válassza **mentése**.</span><span class="sxs-lookup"><span data-stu-id="088fb-116">Then, in the command bar, select **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="088fb-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="088fb-117">Next steps</span></span>
* [<span data-ttu-id="088fb-118">Új felhasználók hozzáadása az Azure Active Directoryhoz</span><span class="sxs-lookup"><span data-stu-id="088fb-118">Add new users to Azure Active Directory</span></span>](active-directory-users-create-azure-portal.md)
* [<span data-ttu-id="088fb-119">Az Azure Active Directoryban a felhasználó jelszavának visszaállítása</span><span class="sxs-lookup"><span data-stu-id="088fb-119">Reset the password for a user in Azure Active Directory</span></span>](active-directory-users-reset-password-azure-portal.md)
* [<span data-ttu-id="088fb-120">Felhasználó hozzárendelése az Azure Active Directory rendszergazdai szerepkörök</span><span class="sxs-lookup"><span data-stu-id="088fb-120">Assign a user to administrator roles in Azure Active Directory</span></span>](active-directory-users-assign-role-azure-portal.md)
* [<span data-ttu-id="088fb-121">Adja hozzá, vagy egy Azure Active Directoryban lévő felhasználói profil adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="088fb-121">Add or change profile information for a user in Azure Active Directory</span></span>](active-directory-users-profile-azure-portal.md)
* [<span data-ttu-id="088fb-122">A felhasználó töröl egy könyvtárat az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="088fb-122">Delete a user from a directory in Azure Active Directory</span></span>](active-directory-users-delete-user-azure-portal.md)

---
title: "Hogyan lehet hozzáadni vagy eltávolítani egy felhasználói szerepkört |} Microsoft Docs"
description: "Útmutató a szerepkörök hozzáadása a kiemelt jogosultságú identitások az Azure Active Directory Privileged Identity Management alkalmazással."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 6a47ced8-cf34-4ce8-bea2-e4fc548cfe22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim;oldportal;it-pro;
ms.openlocfilehash: 3ac07bb7b070f44595c099a454b3d0dbc66126c9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a><span data-ttu-id="c9a84-103">Azure AD Privileged Identity Management: Felhasználói szerepkör hozzáadása vagy eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c9a84-103">Azure AD Privileged Identity Management: How to add or remove a user role</span></span>
<span data-ttu-id="c9a84-104">Az Azure Active Directory (AD), egy globális rendszergazda (vagy vállalati rendszergazda) frissítheti felhasználók amelyek **véglegesen** rendelt szerepkörök az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="c9a84-104">With Azure Active Directory (AD), a global administrator (or company administrator) can update which users are **permanently** assigned to roles in Azure AD.</span></span> <span data-ttu-id="c9a84-105">Ez történik, például a PowerShell-parancsmagokkal `Add-MsolRoleMember` és `Remove-MsolRoleMember`.</span><span class="sxs-lookup"><span data-stu-id="c9a84-105">This is done with PowerShell cmdlets like `Add-MsolRoleMember` and `Remove-MsolRoleMember`.</span></span> <span data-ttu-id="c9a84-106">Vagy a klasszikus Azure portálra az használhatják a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="c9a84-106">Or they can use the Azure classic portal as described in [assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="c9a84-107">Az Azure AD Privileged Identity Management alkalmazás lehetővé teszi, hogy a kiemelt szerepkörű rendszergazda állandó szerepkör-hozzárendelések, valamint hogy.</span><span class="sxs-lookup"><span data-stu-id="c9a84-107">The Azure AD Privileged Identity Management application allows privileged role administrators to make permanent role assignments, as well.</span></span> <span data-ttu-id="c9a84-108">Emellett a kiemelt szerepkörű rendszergazda felhasználók tehet **jogosult** szabása rendszergazdai szerepkörökhöz.</span><span class="sxs-lookup"><span data-stu-id="c9a84-108">Additionally, privileged role administrators can make users **eligible** for admin roles.</span></span> <span data-ttu-id="c9a84-109">Jogosult rendszergazda is aktiválja a szerepkört, amikor szükség, majd rájuk vonatkozó engedélyek lejárnak, ha kész van.</span><span class="sxs-lookup"><span data-stu-id="c9a84-109">An eligible admin can activate the role when they need it, and then their permissions expire once they're done.</span></span>

## <a name="manage-roles-with-pim-in-the-azure-portal"></a><span data-ttu-id="c9a84-110">Az Azure-portálon a PIM szerepkörök kezelése</span><span class="sxs-lookup"><span data-stu-id="c9a84-110">Manage roles with PIM in the Azure portal</span></span>
<span data-ttu-id="c9a84-111">A szervezet rendelhet felhasználók különböző rendszergazdai szerepkörök az Azure AD, az Office 365 és más Microsoft szolgáltatásokat és alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="c9a84-111">In your organization, you can assign users to different administrative roles in Azure AD, Office 365, and other Microsoft services and applications.</span></span>  <span data-ttu-id="c9a84-112">További részleteket a következő szerepkörök található [szerepkörök az Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span><span class="sxs-lookup"><span data-stu-id="c9a84-112">More details on the available roles can be found at [Roles in Azure AD PIM](active-directory-privileged-identity-management-roles.md).</span></span>

<span data-ttu-id="c9a84-113">A felhasználó hozzáadása vagy eltávolítása a Privileged Identity Management használatával szerepkör, a PIM irányítópult elindítani.</span><span class="sxs-lookup"><span data-stu-id="c9a84-113">To add or remove a user in a role using Privileged Identity Management, bring up the PIM dashboard.</span></span> <span data-ttu-id="c9a84-114">Válassza a **rendszergazdai szerepkörök a felhasználók** gombra, vagy válasszon egy adott szerepkör (például a globális rendszergazda) a szerepkörök táblából.</span><span class="sxs-lookup"><span data-stu-id="c9a84-114">Then either click the **Users in Admin Roles** button, or select a specific role (such as Global Administrator) from the roles table.</span></span>

> [!NOTE]
> <span data-ttu-id="c9a84-115">Amennyiben a PIM még nem engedélyezte az Azure portálon, [Ismerkedés az Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="c9a84-115">If you haven't enabled PIM in the Azure portal yet, go to [Get started with Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) for details.</span></span>

<span data-ttu-id="c9a84-116">Ha azt szeretné, a másik felhasználó hozzáférésének PIM magát, a szerepkörök, amelyek a PIM a felhasználónak rendelkeznie kell részelemcímkék ismertetését további [a PIM hozzáférésének hogyan](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="c9a84-116">If you want to give another user access to PIM itself, the roles which PIM requires the user to have are described further in [how to give access to PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="add-a-user-to-a-role"></a><span data-ttu-id="c9a84-117">Adja hozzá a felhasználó egy szerepkörhöz</span><span class="sxs-lookup"><span data-stu-id="c9a84-117">Add a user to a role</span></span>
1. <span data-ttu-id="c9a84-118">Az a [Azure-portálon](https://portal.azure.com/), jelölje be a **Azure AD Privileged Identity Management** csempére az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="c9a84-118">In the [Azure portal](https://portal.azure.com/), select the **Azure AD Privileged Identity Management** tile on the dashboard.</span></span>
2. <span data-ttu-id="c9a84-119">Válassza ki **kiemelt szerepköröket kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c9a84-119">Select **Manage privileged roles**.</span></span>
3. <span data-ttu-id="c9a84-120">Az a **szerepkör összegzés** table, válassza ki a kezelni kívánt szerepkört.</span><span class="sxs-lookup"><span data-stu-id="c9a84-120">In the **Role summary** table, select the role you want to manage.</span></span>
4. <span data-ttu-id="c9a84-121">A szerepkör paneljén válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c9a84-121">In the role blade, select **Add**.</span></span>
5. <span data-ttu-id="c9a84-122">Kattintson a **válasszon ki egy felhasználót** és keresse meg a felhasználót a **válasszon ki egy felhasználót** panelen.</span><span class="sxs-lookup"><span data-stu-id="c9a84-122">Click **Select users** and search for the user on the **Select users** blade.</span></span>  
6. <span data-ttu-id="c9a84-123">A keresési eredmények listájáról válassza ki a felhasználót, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="c9a84-123">Select the user from the search results list, and click **Done**.</span></span>
7. <span data-ttu-id="c9a84-124">Kattintson a **OK** a mentéshez.</span><span class="sxs-lookup"><span data-stu-id="c9a84-124">Click **OK** to save your selection.</span></span> <span data-ttu-id="c9a84-125">A kiválasztott felhasználó megjelenik a listában a megfelelő a szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="c9a84-125">The user you have selected will appear in the list as eligible for the role.</span></span>

> [!NOTE]
> <span data-ttu-id="c9a84-126">Új felhasználói szerepkör a szerepkör alapértelmezés szerint csak jogosultak.</span><span class="sxs-lookup"><span data-stu-id="c9a84-126">New users in a role are only eligible for the role by default.</span></span> <span data-ttu-id="c9a84-127">Ha azt szeretné, hogy a szerepkör véglegessé, kattintson a felhasználónévre a listában.</span><span class="sxs-lookup"><span data-stu-id="c9a84-127">If you want to make the role permanent, click the user in the list.</span></span> <span data-ttu-id="c9a84-128">A felhasználói adatok egy új panelen megjelennek.</span><span class="sxs-lookup"><span data-stu-id="c9a84-128">The user's information will appear in a new blade.</span></span> <span data-ttu-id="c9a84-129">Válassza ki **ellenőrizze perm** a felhasználói adatokat menüben.</span><span class="sxs-lookup"><span data-stu-id="c9a84-129">Select **Make perm** in the user information menu.</span></span>  
> <span data-ttu-id="c9a84-130">Ha a felhasználó nem tudja regisztrálni Azure multi-factor Authentication (MFA), vagy egy Microsoft-fiókot használ (általában @outlook.com), meg kell győződnie állandó a szerepkört.</span><span class="sxs-lookup"><span data-stu-id="c9a84-130">If a user cannot register for Azure Multi-Factor Authentication (MFA), or is using a Microsoft account (usually @outlook.com), you need to make them permanent in all their roles.</span></span> <span data-ttu-id="c9a84-131">Regisztrálnia az MFA szolgáltatásra az aktiválás során meg kell adnia a megfelelő rendszergazdák.</span><span class="sxs-lookup"><span data-stu-id="c9a84-131">Eligible admins are asked to register for MFA during activation.</span></span>

<span data-ttu-id="c9a84-132">Most, hogy a felhasználó nem jogosult a szerepkörhöz, tájékoztathatja a felhasználókat, hogy akkor is aktiválhatja az utasításainak megfelelően [aktiválása vagy deaktiválása](active-directory-privileged-identity-management-how-to-activate-role.md).</span><span class="sxs-lookup"><span data-stu-id="c9a84-132">Now that the user is eligible for a role, let them know that they can activate it according to the instructions in [How to activate or deactivate a role](active-directory-privileged-identity-management-how-to-activate-role.md).</span></span>

## <a name="remove-a-user-from-a-role"></a><span data-ttu-id="c9a84-133">Felhasználó eltávolítása egy szerepkör</span><span class="sxs-lookup"><span data-stu-id="c9a84-133">Remove a user from a role</span></span>
<span data-ttu-id="c9a84-134">Távolítsa el a felhasználók a megfelelő szerepkör-hozzárendelések, de győződjön meg arról, hogy mindig van legalább egy felhasználót, aki egy állandó globális rendszergazdai.</span><span class="sxs-lookup"><span data-stu-id="c9a84-134">You can remove users from eligible role assignments, but make sure there is always at least one user who is a permanent global administrator.</span></span>

<span data-ttu-id="c9a84-135">Kövesse az alábbi lépéseket egy adott felhasználó eltávolítása egy szerepkört:</span><span class="sxs-lookup"><span data-stu-id="c9a84-135">Follow these steps to remove a specific user from a role:</span></span>

1. <span data-ttu-id="c9a84-136">Válasszon ki egy szerepkört az Azure AD PIM irányítópult vagy kattintson a a szerepkört a szerepkör listában keresse meg a **rendszergazdai szerepkörök a felhasználók** gombra.</span><span class="sxs-lookup"><span data-stu-id="c9a84-136">Navigate to the role in the role list either by selecting a role in the Azure AD PIM dashboard or by clicking on the **Users in Admin Roles** button.</span></span>
2. <span data-ttu-id="c9a84-137">Kattintson a felhasználót a felhasználói listáról.</span><span class="sxs-lookup"><span data-stu-id="c9a84-137">Click on the user in the user list.</span></span>
3. <span data-ttu-id="c9a84-138">Kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="c9a84-138">Click **Remove**.</span></span> <span data-ttu-id="c9a84-139">Üzenet kéri megerősítését.</span><span class="sxs-lookup"><span data-stu-id="c9a84-139">A message will ask you to confirm.</span></span>
4. <span data-ttu-id="c9a84-140">Kattintson a **Igen** eltávolítja a szerepkört a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="c9a84-140">Click **Yes** to remove the role from the user.</span></span>

<span data-ttu-id="c9a84-141">Ha nem biztos abban, hogy mely felhasználók továbbra is szükséges a szerepkör-hozzárendeléseket, akkor is [a szerepkör egy hozzáférés-ellenőrzés indítása](active-directory-privileged-identity-management-how-to-start-security-review.md).</span><span class="sxs-lookup"><span data-stu-id="c9a84-141">If you're not sure which users still need their role assignments, then you can [start an access review for the role](active-directory-privileged-identity-management-how-to-start-security-review.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9a84-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9a84-142">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


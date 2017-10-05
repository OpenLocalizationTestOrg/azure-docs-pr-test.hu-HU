---
title: "Zónázással biztosítson hozzáférést a Privileged Identity Management - Azure hogyan |} Microsoft Docs"
description: "Útmutató: a szerepkörök hozzáadása az Azure Active Directory Privileged Identity Management bővítmény rendelkező felhasználók részére, így a PIM kezelésére."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: aeaefb484b29da6e89c2c3c650a79a881b3fa5b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="giving-access-to-manage-azure-ad-privileged-identity-management"></a><span data-ttu-id="c2f71-103">Adjon hozzáférést a Azure AD Privileged Identity Management kezelésére</span><span class="sxs-lookup"><span data-stu-id="c2f71-103">Giving access to manage Azure AD Privileged Identity Management</span></span>
<span data-ttu-id="c2f71-104">A globális rendszergazda Azure AD Privileged Identity Management (PIM) egy olyan szervezet automatikusan lehetővé teszi, hogy le a szerepkör-hozzárendelések és a PIM elérésére.</span><span class="sxs-lookup"><span data-stu-id="c2f71-104">The global administrator who enables Azure AD Privileged Identity Management (PIM) for an organization automatically get role assignments and access to PIM.</span></span> <span data-ttu-id="c2f71-105">Más írási hozzáféréssel alapértelmezés szerint kap, azonban más globális rendszergazdákat is.</span><span class="sxs-lookup"><span data-stu-id="c2f71-105">No one else gets write access by default, though, including other global administrators.</span></span> <span data-ttu-id="c2f71-106">Más globális rendszergazdák, biztonsági rendszergazdák és biztonsági olvasók rendelkezik az Azure AD PIM olvasási hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="c2f71-106">Other global adminstrators, security administrators, and security readers have read-only access to Azure AD PIM.</span></span> <span data-ttu-id="c2f71-107">A hozzáférésének PIM, az első felhasználó rendelhet másoknak a **kiemelt szerepkörű rendszergazda** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="c2f71-107">To give access to PIM, the first user can assign others to the **Privileged role administrator** role.</span></span> <span data-ttu-id="c2f71-108">Ehhez a hozzárendeléshez maga PIM belül kell végrehajtani, és a PowerShell vagy más portálokon keresztül nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="c2f71-108">This assignment must be done from within PIM itself, and cannot be changed via PowerShell or other portals.</span></span>

> [!NOTE]
> <span data-ttu-id="c2f71-109">Az Azure MFA kezelése az Azure AD PIM van szükség.</span><span class="sxs-lookup"><span data-stu-id="c2f71-109">Managing Azure AD PIM requires Azure MFA.</span></span> <span data-ttu-id="c2f71-110">Microsoft-fiókok az Azure MFA nem regisztrálható, mert a Microsoft-fiókkal jelentkezik be egy felhasználó nem fér hozzá a Azure AD PIM.</span><span class="sxs-lookup"><span data-stu-id="c2f71-110">Since Microsoft accounts cannot register for Azure MFA, a user who signs in with a Microsoft account cannot access Azure AD PIM.</span></span>
> 
> 

<span data-ttu-id="c2f71-111">Győződjön meg arról, hogy mindig legalább két felhasználók egy kiemelt szerepkörű rendszergazda, abban az esetben, ha egy felhasználó ki van zárva, vagy a fiókot törölték.</span><span class="sxs-lookup"><span data-stu-id="c2f71-111">Make sure there are always at least two users in a privileged role administrator role, in case one user is locked out or their account is deleted.</span></span>

## <a name="give-another-user-access-to-manage-pim"></a><span data-ttu-id="c2f71-112">Egy másik felhasználó hozzáférést PIM kezelése</span><span class="sxs-lookup"><span data-stu-id="c2f71-112">Give another user access to manage PIM</span></span>
1. <span data-ttu-id="c2f71-113">Jelentkezzen be a [Azure-portálon](https://portal.azure.com/) válassza ki a **Azure AD Privileged Identity Management** alkalmazást az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="c2f71-113">Sign in to the [Azure portal](https://portal.azure.com/) and select the **Azure AD Privileged Identity Management** app on the dashboard.</span></span>
2. <span data-ttu-id="c2f71-114">Válassza ki **kiemelt szerepköröket kezelése** > **kiemelt szerepkörű rendszergazda** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c2f71-114">Select **Manage privileged roles** > **Privileged role administrator** > **Add**.</span></span>
   
    ![Adja hozzá a kiemelt szerepkörű rendszergazda – képernyőkép][1]
3. <span data-ttu-id="c2f71-116">A felügyelt felhasználók hozzáadása paneljét 1. lépés már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c2f71-116">On the Add managed users blade, step 1 is already complete.</span></span> <span data-ttu-id="c2f71-117">Válassza ki a 2 **válasszon ki egy felhasználót** és keresse meg a hozzáadni kívánt felhasználót.</span><span class="sxs-lookup"><span data-stu-id="c2f71-117">Select step 2, **Select users** and search for the user you want to add.</span></span>
   
    ![Válassza ki a felhasználók – képernyőkép][2]
4. <span data-ttu-id="c2f71-119">Jelölje ki a felhasználó a keresési eredmények között, és kattintson **végzett**.</span><span class="sxs-lookup"><span data-stu-id="c2f71-119">Select the user from the search results, and click **Done**.</span></span>
5. <span data-ttu-id="c2f71-120">Kattintson a **OK** a mentéshez.</span><span class="sxs-lookup"><span data-stu-id="c2f71-120">Click **OK** to save your selection.</span></span> <span data-ttu-id="c2f71-121">A kiválasztott felhasználó megjelenik a kiemelt szerepkörű rendszergazdák listáját.</span><span class="sxs-lookup"><span data-stu-id="c2f71-121">The user you have selected will appear in the list of Privileged role administrators.</span></span>
   
   * <span data-ttu-id="c2f71-122">Amikor egy új szerepkör hozzárendelése valakinek, azok automatikusan be vannak állítva, jogosult a szerepkör aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="c2f71-122">Whenever you assign a new role to someone, they are automatically set up as eligible to activate the role.</span></span> <span data-ttu-id="c2f71-123">Ha engedélyezni szeretné azokat a szerepkör állandó, kattintson a felhasználónévre a listában.</span><span class="sxs-lookup"><span data-stu-id="c2f71-123">If you want to make them permanent in the role, click the user in the list.</span></span> <span data-ttu-id="c2f71-124">Válassza ki **perm ellenőrizze** a felhasználói adatokat menüben.</span><span class="sxs-lookup"><span data-stu-id="c2f71-124">Select **make perm** in the user information menu.</span></span>
6. <span data-ttu-id="c2f71-125">A felhasználó mutató hivatkozást küld [Ismerkedés az Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c2f71-125">Send the user a link to [Getting started with Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).</span></span>

## <a name="remove-another-users-access-rights-for-managing-pim"></a><span data-ttu-id="c2f71-126">Egy másik felhasználói hozzáférési jogosultságokat PIM kezelésére szolgáló eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c2f71-126">Remove another user's access rights for managing PIM</span></span>
<span data-ttu-id="c2f71-127">Mielőtt törli valakit a kiemelt szerepkörű rendszergazda, ügyeljen arra, hogy még mindig lesz két hozzárendelve felhasználók.</span><span class="sxs-lookup"><span data-stu-id="c2f71-127">Before you remove someone from the privileged role administrator role, always make sure there will still be two users assigned to it.</span></span>

1. <span data-ttu-id="c2f71-128">A PIM irányítópulton kattintson a szerepkör **kiemelt szerepkörű rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="c2f71-128">In the PIM dashboard, click on the role **Privileged role administrator**.</span></span>  <span data-ttu-id="c2f71-129">Jelenleg a szerepet betöltő felhasználók listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c2f71-129">The list of users currently in that role will be displayed.</span></span>
2. <span data-ttu-id="c2f71-130">Kattintson a felhasználót a felhasználói listáról.</span><span class="sxs-lookup"><span data-stu-id="c2f71-130">Click on the user in the user list.</span></span>
3. <span data-ttu-id="c2f71-131">Kattintson a **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="c2f71-131">Click on **Remove**.</span></span>  <span data-ttu-id="c2f71-132">Egy megerősítő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c2f71-132">You are presented with a confirmation message.</span></span>
4. <span data-ttu-id="c2f71-133">Kattintson a **Igen** eltávolítása a felhasználói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="c2f71-133">Click **Yes** to remove the user from the role.</span></span>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a><span data-ttu-id="c2f71-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2f71-134">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png

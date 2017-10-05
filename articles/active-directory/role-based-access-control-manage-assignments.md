---
title: "Az Azure erőforrás-hozzáférési hozzárendelések megtekintése |} Microsoft Docs"
description: "Megtekintéséhez és kezeléséhez minden a szerepköralapú hozzáférés-vezérlés hozzárendelések bármely felhasználó vagy csoport az Azure-portálon"
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: 72c695d08bdf5de003d51ffb0768184e1e4d00ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal"></a><span data-ttu-id="e60cb-103">A felhasználók és csoportok az Azure portálon access-hozzárendelések megtekintése</span><span class="sxs-lookup"><span data-stu-id="e60cb-103">View access assignments for users and groups in the Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e60cb-104">Hozzáférés kezelése felhasználó vagy csoport alapján</span><span class="sxs-lookup"><span data-stu-id="e60cb-104">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="e60cb-105">Hozzáférés kezelése erőforrás alapján</span><span class="sxs-lookup"><span data-stu-id="e60cb-105">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="e60cb-106">Szerepköralapú hozzáférés-vezérléssel (RBAC) az Azure Active Directory (Azure AD) az Azure-erőforrások hozzáférés kezelhető.</span><span class="sxs-lookup"><span data-stu-id="e60cb-106">With role-based access control (RBAC) in the Azure Active Directory (Azure AD), you can manage access to your Azure resources.</span></span> 

<span data-ttu-id="e60cb-107">Rendelni, amelyben a Szerepalapú hozzáférés nem részletes, mert az engedélyeket korlátozhatja két módja van:</span><span class="sxs-lookup"><span data-stu-id="e60cb-107">Access assigned with RBAC is fine-grained because there are two ways you can restrict the permissions:</span></span>

* <span data-ttu-id="e60cb-108">**Hatókör:** RBAC szerepkör-hozzárendelések hatóköre egy adott előfizetés, a csoport vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e60cb-108">**Scope:** RBAC role assignments are scoped to a specific subscription, resource group, or resource.</span></span> <span data-ttu-id="e60cb-109">A felhasználó által megadott hozzáférési egyetlen erőforrásra nem fér hozzá az összes erőforrását ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e60cb-109">A user given access to a single resource cannot access any other resources in the same subscription.</span></span>
* <span data-ttu-id="e60cb-110">**Szerepkör:** a hozzárendelés hatókörébe hozzáférés szűkíthető van akár további által szerepkör hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="e60cb-110">**Role:** Within the scope of the assignment, access is narrowed even further by assigning a role.</span></span> <span data-ttu-id="e60cb-111">Szerepkörök magas szintű, például a tulajdonos vagy adott, például a virtuális gép olvasó lehet.</span><span class="sxs-lookup"><span data-stu-id="e60cb-111">Roles can be high-level, like owner, or specific, like virtual machine reader.</span></span>

<span data-ttu-id="e60cb-112">Szerepkörök csak rendelhetők hozzá a az előfizetés, az erőforráscsoport vagy a erőforrást, a hozzárendelés hatókörét.</span><span class="sxs-lookup"><span data-stu-id="e60cb-112">Roles can only be assigned from within the subscription, resource group, or resource that is the scope for the assignment.</span></span> <span data-ttu-id="e60cb-113">De megtekintheti egy adott felhasználó vagy csoport access-hozzárendelések egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="e60cb-113">But you can view all the access assignments for a given user or group in a single place.</span></span> <span data-ttu-id="e60cb-114">Akkor is, legfeljebb 2000 szerepkör-hozzárendelések minden előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="e60cb-114">You can have up to 2000 role assignments in each subscription.</span></span> 

<span data-ttu-id="e60cb-115">További információért arról, hogy hogyan [az Azure-előfizetés erőforrásokhoz való hozzáférés kezelése a szerepkör-hozzárendelések segítségével](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="e60cb-115">Get more information about how to [Use role assignments to manage access to your Azure subscription resources](role-based-access-control-configure.md).</span></span>

## <a name="view-access-assignments"></a><span data-ttu-id="e60cb-116">Hozzáférés-hozzárendelések megtekintése</span><span class="sxs-lookup"><span data-stu-id="e60cb-116">View access assignments</span></span>
<span data-ttu-id="e60cb-117">A hozzáférés-hozzárendeléseinek egyetlen felhasználó vagy csoport kereséséhez indítsa el az Azure Active Directoryban a [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e60cb-117">To look up the access assignments for a single user or group, start in Azure Active Directory in the [Azure portal](http://portal.azure.com).</span></span>

1. <span data-ttu-id="e60cb-118">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e60cb-118">Select **Azure Active Directory**.</span></span> <span data-ttu-id="e60cb-119">Ha ezt a beállítást nem a navigációs listája látható, válassza ki a **több szolgáltatások** majd görgessen lefelé a Keresés **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e60cb-119">If this option is not visible on your navigation list, select **More Services** and then scroll down to find **Azure Active Directory**.</span></span>
2. <span data-ttu-id="e60cb-120">Válassza ki **felhasználók és csoportok**, majd **minden felhasználó** vagy **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="e60cb-120">Select **Users and Groups**, and then either **All users** or **All groups**.</span></span> <span data-ttu-id="e60cb-121">Ehhez a példához azt összpontosítani egyéni felhasználók számára is.</span><span class="sxs-lookup"><span data-stu-id="e60cb-121">For this example, we focus on individual users.</span></span>
    <span data-ttu-id="e60cb-122">![Kezelheti a felhasználókat és csoportokat az Azure Active Directory – képernyőkép](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span><span class="sxs-lookup"><span data-stu-id="e60cb-122">![Manage users and groups in Azure Active Directory - screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span></span>
3. <span data-ttu-id="e60cb-123">Keresse meg a felhasználói név vagy felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="e60cb-123">Search for the user by name or username.</span></span>
4. <span data-ttu-id="e60cb-124">Válassza ki az **Azure-erőforrások** lehetőséget a felhasználói panelen.</span><span class="sxs-lookup"><span data-stu-id="e60cb-124">Select **Azure resources** on the user blade.</span></span> <span data-ttu-id="e60cb-125">Ekkor a felhasználóhoz tartozó összes hozzáférési hozzárendelés megjelenik.</span><span class="sxs-lookup"><span data-stu-id="e60cb-125">All the access assignments for that user appear.</span></span>

### <a name="read-permissions-to-view-assignments"></a><span data-ttu-id="e60cb-126">Olvasási engedélyek hozzárendelések megtekintése</span><span class="sxs-lookup"><span data-stu-id="e60cb-126">Read permissions to view assignments</span></span>
<span data-ttu-id="e60cb-127">Ezen az oldalon csak az, hogy rendelkezik olvasási engedélye hozzáférés hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="e60cb-127">This page only shows the access assignments that you have permission to read.</span></span> <span data-ttu-id="e60cb-128">Például A előfizetés olvasási hozzáféréssel rendelkezik, és nyissa meg az Azure-erőforrások paneljét, ellenőrizze a felhasználó-hozzárendeléseket.</span><span class="sxs-lookup"><span data-stu-id="e60cb-128">For example, you have read access to subscription A and go to the Azure resources blade to check a user's assignments.</span></span> <span data-ttu-id="e60cb-129">Az előfizetés A hozzáférési hozzárendeléseinek látható, de nem látja, hogy ő is van a hozzáférés-hozzárendelések az előfizetésben a b kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="e60cb-129">You can see her access assignments for subscription A, but can't see that she also has access assignments on subscription B.</span></span>

## <a name="delete-access-assignments"></a><span data-ttu-id="e60cb-130">Hozzáférés-hozzárendelések törlése</span><span class="sxs-lookup"><span data-stu-id="e60cb-130">Delete access assignments</span></span>
<span data-ttu-id="e60cb-131">Ezen a panelen törölheti a közvetlenül hozzá egy felhasználóhoz vagy csoporthoz rendelt access-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="e60cb-131">From this blade, you can delete access assignments that were assigned directly to a user or group.</span></span> <span data-ttu-id="e60cb-132">A hozzáférés hozzárendelése egy szülőcsoportot öröklődött, ha az erőforrás vagy az előfizetés, és kezelheti a van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e60cb-132">If the access assignment was inherited from a parent group, you need to go to the resource or subscription and manage the assignment there.</span></span>

1. <span data-ttu-id="e60cb-133">A listát az összes a hozzáférés egy felhasználó vagy csoport számára jelölje ki a megfelelőt.</span><span class="sxs-lookup"><span data-stu-id="e60cb-133">From the list of all the access assignments for a user or group, select the one you want to delete.</span></span>
2. <span data-ttu-id="e60cb-134">Válassza ki **eltávolítása** , majd **Igen** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="e60cb-134">Select **Remove** and then **Yes** to confirm.</span></span>
    <span data-ttu-id="e60cb-135">![Távolítsa el a hozzáférés hozzárendelése – képernyőkép](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span><span class="sxs-lookup"><span data-stu-id="e60cb-135">![Remove access assignment - screenshot](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="e60cb-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e60cb-136">Next steps</span></span>

* <span data-ttu-id="e60cb-137">Ismerkedés a szerepköralapú hozzáférés-vezérlést az [szerepkör-hozzárendelések segítségével az Azure-előfizetés erőforrásokhoz való hozzáférés kezelése](role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="e60cb-137">Get started with Role-Based Access Control to [Use role assignments to manage access to your Azure subscription resources](role-based-access-control-configure.md)</span></span>
* <span data-ttu-id="e60cb-138">Lásd: [Beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="e60cb-138">See the [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>


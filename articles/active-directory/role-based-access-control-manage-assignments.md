---
title: "az Azure erőforrás-hozzáférési hozzárendelések aaaView |} Microsoft Docs"
description: "Megtekintéséhez és kezeléséhez minden hello szerepköralapú hozzáférés-vezérlés hozzárendeléseinek bármely felhasználónak vagy csoportnak a hello Azure-portálon"
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
ms.openlocfilehash: ec96c9d4b9e2cb4925949b8bac78767bd564c8ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-hello-azure-portal"></a><span data-ttu-id="1de35-103">A felhasználók és csoportok hello Azure-portálon a hozzáférés-hozzárendelések megtekintése</span><span class="sxs-lookup"><span data-stu-id="1de35-103">View access assignments for users and groups in hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1de35-104">Hozzáférés kezelése felhasználó vagy csoport alapján</span><span class="sxs-lookup"><span data-stu-id="1de35-104">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="1de35-105">Hozzáférés kezelése erőforrás alapján</span><span class="sxs-lookup"><span data-stu-id="1de35-105">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="1de35-106">A szerepköralapú hozzáférés-vezérlés (RBAC) hello Azure Active Directory (Azure AD), kezelheti a hozzáférést tooyour Azure erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1de35-106">With role-based access control (RBAC) in hello Azure Active Directory (Azure AD), you can manage access tooyour Azure resources.</span></span> 

<span data-ttu-id="1de35-107">Rendelni, amelyben a Szerepalapú hozzáférés nem részletes, mert hello engedélyeket korlátozhatja két módja van:</span><span class="sxs-lookup"><span data-stu-id="1de35-107">Access assigned with RBAC is fine-grained because there are two ways you can restrict hello permissions:</span></span>

* <span data-ttu-id="1de35-108">**Hatókör:** RBAC szerepkör-hozzárendeléseket a rendszer hatókörön belüli tooa előfizetéshez, erőforrás vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="1de35-108">**Scope:** RBAC role assignments are scoped tooa specific subscription, resource group, or resource.</span></span> <span data-ttu-id="1de35-109">Egy felhasználó által megadott hozzáférési tooa egyetlen erőforrás nem érhető el bármilyen egyéb erőforrások hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="1de35-109">A user given access tooa single resource cannot access any other resources in hello same subscription.</span></span>
* <span data-ttu-id="1de35-110">**Szerepkör:** hello hozzárendelés hello hatókörbe hozzáférés szűkíthető van akár további által szerepkör hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="1de35-110">**Role:** Within hello scope of hello assignment, access is narrowed even further by assigning a role.</span></span> <span data-ttu-id="1de35-111">Szerepkörök magas szintű, például a tulajdonos vagy adott, például a virtuális gép olvasó lehet.</span><span class="sxs-lookup"><span data-stu-id="1de35-111">Roles can be high-level, like owner, or specific, like virtual machine reader.</span></span>

<span data-ttu-id="1de35-112">Szerepkörök csak rendelhetők hozzá a hello előfizetés, erőforráscsoportból vagy erőforrása, amely hello hatókör hello hozzárendelés belül.</span><span class="sxs-lookup"><span data-stu-id="1de35-112">Roles can only be assigned from within hello subscription, resource group, or resource that is hello scope for hello assignment.</span></span> <span data-ttu-id="1de35-113">De megtekintheti egy adott felhasználó vagy csoport összes hello hozzáférés hozzárendelését egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="1de35-113">But you can view all hello access assignments for a given user or group in a single place.</span></span> <span data-ttu-id="1de35-114">Akkor is fel too2000 szerepkör-hozzárendelések minden előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="1de35-114">You can have up too2000 role assignments in each subscription.</span></span> 

<span data-ttu-id="1de35-115">További információért arról, hogyan túl[szerepkör hozzárendelések toomanage tooyour Azure-előfizetés erőforrások eléréséhez használjon](role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1de35-115">Get more information about how too[Use role assignments toomanage access tooyour Azure subscription resources](role-based-access-control-configure.md).</span></span>

## <a name="view-access-assignments"></a><span data-ttu-id="1de35-116">Hozzáférés-hozzárendelések megtekintése</span><span class="sxs-lookup"><span data-stu-id="1de35-116">View access assignments</span></span>
<span data-ttu-id="1de35-117">toolook mentése hello hozzáférés hozzárendelése egy felhasználót vagy csoportot, indítsa el az Azure Active Directoryban hello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1de35-117">toolook up hello access assignments for a single user or group, start in Azure Active Directory in hello [Azure portal](http://portal.azure.com).</span></span>

1. <span data-ttu-id="1de35-118">Válassza ki **az Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1de35-118">Select **Azure Active Directory**.</span></span> <span data-ttu-id="1de35-119">Ha ezt a beállítást nem a navigációs listája látható, válassza ki a **több szolgáltatások** majd görgessen lefelé toofind **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1de35-119">If this option is not visible on your navigation list, select **More Services** and then scroll down toofind **Azure Active Directory**.</span></span>
2. <span data-ttu-id="1de35-120">Válassza ki **felhasználók és csoportok**, majd **minden felhasználó** vagy **összes csoport**.</span><span class="sxs-lookup"><span data-stu-id="1de35-120">Select **Users and Groups**, and then either **All users** or **All groups**.</span></span> <span data-ttu-id="1de35-121">Ehhez a példához azt összpontosítani egyéni felhasználók számára is.</span><span class="sxs-lookup"><span data-stu-id="1de35-121">For this example, we focus on individual users.</span></span>
    <span data-ttu-id="1de35-122">![Kezelheti a felhasználókat és csoportokat az Azure Active Directory – képernyőkép](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span><span class="sxs-lookup"><span data-stu-id="1de35-122">![Manage users and groups in Azure Active Directory - screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)</span></span>
3. <span data-ttu-id="1de35-123">Keresés név vagy felhasználónév hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="1de35-123">Search for hello user by name or username.</span></span>
4. <span data-ttu-id="1de35-124">Válassza ki **Azure-erőforrások** hello felhasználói panelen.</span><span class="sxs-lookup"><span data-stu-id="1de35-124">Select **Azure resources** on hello user blade.</span></span> <span data-ttu-id="1de35-125">Az adott felhasználó összes hello hozzáférés hozzárendelést jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1de35-125">All hello access assignments for that user appear.</span></span>

### <a name="read-permissions-tooview-assignments"></a><span data-ttu-id="1de35-126">Olvasási engedéllyel tooview hozzárendelések</span><span class="sxs-lookup"><span data-stu-id="1de35-126">Read permissions tooview assignments</span></span>
<span data-ttu-id="1de35-127">Ezen az oldalon csak az hello hozzáférés hozzárendelések, hogy rendelkezik-e engedéllyel tooread.</span><span class="sxs-lookup"><span data-stu-id="1de35-127">This page only shows hello access assignments that you have permission tooread.</span></span> <span data-ttu-id="1de35-128">Például A olvasási hozzáférés toosubscription rendelkezik, és toohello Azure-erőforrások panel toocheck nyissa meg a felhasználó-hozzárendeléseket.</span><span class="sxs-lookup"><span data-stu-id="1de35-128">For example, you have read access toosubscription A and go toohello Azure resources blade toocheck a user's assignments.</span></span> <span data-ttu-id="1de35-129">Az előfizetés A hozzáférési hozzárendeléseinek látható, de nem látja, hogy ő is van a hozzáférés-hozzárendelések az előfizetésben a b kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="1de35-129">You can see her access assignments for subscription A, but can't see that she also has access assignments on subscription B.</span></span>

## <a name="delete-access-assignments"></a><span data-ttu-id="1de35-130">Hozzáférés-hozzárendelések törlése</span><span class="sxs-lookup"><span data-stu-id="1de35-130">Delete access assignments</span></span>
<span data-ttu-id="1de35-131">Ezen a panelen törölheti a hozzáférési hozzárendelések közvetlenül hozzárendelt tooa felhasználó vagy csoport.</span><span class="sxs-lookup"><span data-stu-id="1de35-131">From this blade, you can delete access assignments that were assigned directly tooa user or group.</span></span> <span data-ttu-id="1de35-132">Ha hello hozzáférés hozzárendelése egy szülőcsoportot öröklődött, toogo toohello erőforrás vagy előfizetés szükséges, és nincs hello hozzárendelés kezelése.</span><span class="sxs-lookup"><span data-stu-id="1de35-132">If hello access assignment was inherited from a parent group, you need toogo toohello resource or subscription and manage hello assignment there.</span></span>

1. <span data-ttu-id="1de35-133">Az összes hello hozzáférés egy felhasználó vagy csoport hello listáról válassza ki a kívánt toodelete egy hello.</span><span class="sxs-lookup"><span data-stu-id="1de35-133">From hello list of all hello access assignments for a user or group, select hello one you want toodelete.</span></span>
2. <span data-ttu-id="1de35-134">Válassza ki **eltávolítása** , majd **Igen** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="1de35-134">Select **Remove** and then **Yes** tooconfirm.</span></span>
    <span data-ttu-id="1de35-135">![Távolítsa el a hozzáférés hozzárendelése – képernyőkép](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span><span class="sxs-lookup"><span data-stu-id="1de35-135">![Remove access assignment - screenshot](./media/role-based-access-control-manage-assignments/delete_assignment.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="1de35-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1de35-136">Next steps</span></span>

* <span data-ttu-id="1de35-137">Ismerkedés a szerepköralapú hozzáférés-vezérlés túl[szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához](role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="1de35-137">Get started with Role-Based Access Control too[Use role assignments toomanage access tooyour Azure subscription resources](role-based-access-control-configure.md)</span></span>
* <span data-ttu-id="1de35-138">Lásd: hello [beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="1de35-138">See hello [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>


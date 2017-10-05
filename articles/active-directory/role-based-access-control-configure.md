---
title: "Szerepköralapú hozzáférés-vezérlés az Azure Portalon | Microsoft Docs"
description: "Megismerheti a hozzáférés kezelését az Azure Portal szerepköralapú hozzáférés-vezérlése segítségével. Szerepkör-hozzárendelésekkel rendelhet engedélyeket az erőforrásokhoz."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 9df7f7851ef1fc6b4ed03b981aa5062d6b0913ad
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-role-based-access-control-to-manage-access-to-your-azure-subscription-resources"></a><span data-ttu-id="d0fdf-104">Az Azure-előfizetések erőforrásaihoz való hozzáférés kezelése szerepköralapú hozzáférés-vezérléssel</span><span class="sxs-lookup"><span data-stu-id="d0fdf-104">Use Role-Based Access Control to manage access to your Azure subscription resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d0fdf-105">Hozzáférés kezelése felhasználó vagy csoport alapján</span><span class="sxs-lookup"><span data-stu-id="d0fdf-105">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="d0fdf-106">Hozzáférés kezelése erőforrás alapján</span><span class="sxs-lookup"><span data-stu-id="d0fdf-106">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="d0fdf-107">Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-107">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure.</span></span> <span data-ttu-id="d0fdf-108">Az RBAC használata lehetővé teszi, hogy csak olyan mértékű hozzáférést biztosítson, ami a felhasználóknak a feladataik elvégzéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-108">Using RBAC, you can grant only the amount of access that users need to perform their jobs.</span></span> <span data-ttu-id="d0fdf-109">Ez a cikk segít az RBAC beállításában és használatában az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-109">This article helps you get up and running with RBAC in the Azure portal.</span></span> <span data-ttu-id="d0fdf-110">Ha további részleteket szeretne arról, hogy hogyan segít az RBAC a hozzáférések kezelésében, tekintse meg [a szerepköralapú hozzáférés-vezérlést](role-based-access-control-what-is.md) ismertető szakaszt.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-110">If you want more details about how RBAC helps you manage access, see [What is Role-Based Access Control](role-based-access-control-what-is.md).</span></span>

<span data-ttu-id="d0fdf-111">Előfizetésenként 2000 szerepkör-hozzárendelés osztható ki.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-111">Within each subscription, you can grant up to 2000 role assignments.</span></span> 

## <a name="view-access"></a><span data-ttu-id="d0fdf-112">Hozzáférés megtekintése</span><span class="sxs-lookup"><span data-stu-id="d0fdf-112">View access</span></span>
<span data-ttu-id="d0fdf-113">Az [Azure Portal](https://portal.azure.com) fő paneljén láthatja, hogy kinek van hozzáférése egy adott erőforráshoz, erőforráscsoporthoz vagy előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-113">You can see who has access to a resource, resource group, or subscription from its main blade in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d0fdf-114">Tegyük fel, hogy szeretnénk megnézni, hogy kinek van hozzáférése egy erőforráscsoporthoz:</span><span class="sxs-lookup"><span data-stu-id="d0fdf-114">For example, we want to see who has access to one of our resource groups:</span></span>

1. <span data-ttu-id="d0fdf-115">Válassza az **Erőforráscsoportok** elemet a bal oldali navigációs sávban.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-115">Select **Resource groups** in the navigation bar on the left.</span></span>  
    <span data-ttu-id="d0fdf-116">![Erőforráscsoportok – ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span><span class="sxs-lookup"><span data-stu-id="d0fdf-116">![Resource groups - icon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span></span>
2. <span data-ttu-id="d0fdf-117">Válassza ki az erőforráscsoport nevét az **Erőforráscsoportok** panelen.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-117">Select the name of the resource group from the **Resource groups** blade.</span></span>
3. <span data-ttu-id="d0fdf-118">A bal oldali menüben válassza az **Access control (IAM)** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-118">Select **Access control (IAM)** from the left menu.</span></span>  
4. <span data-ttu-id="d0fdf-119">Az Access control panel az összes olyan felhasználót, csoportot és alkalmazást felsorolja, amely hozzáféréssel rendelkezik az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-119">The Access control blade lists all users, groups, and applications that have been granted access to the resource group.</span></span>  
   
    ![Képernyőfelvétel a felhasználók panelről – örökölt vagy hozzárendelt hozzáférés](./media/role-based-access-control-configure/view-access.png)

<span data-ttu-id="d0fdf-121">Figyelje meg, hogy egyes szerepkör hatóköre **erre az erőforrásra** érvényes, míg mások más hatókörökből **öröklődnek**.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-121">Notice that some roles are scoped to **This resource** while others are **Inherited** it from another scope.</span></span> <span data-ttu-id="d0fdf-122">A hozzáférés lehet kifejezetten az erőforráscsoporthoz rendelt, vagy a szülő előfizetés egyik hozzárendeléséből örökölt.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-122">Access is either assigned specifically to the resource group or inherited from an assignment to the parent subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="d0fdf-123">Az új RBAC-modellben a hagyományos előfizetés-adminisztrátorok és társadminisztrátorok minősülnek tulajdonosoknak.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-123">Classic subscription admins and co-admins are considered owners of the subscription in the new RBAC model.</span></span>

## <a name="add-access"></a><span data-ttu-id="d0fdf-124">Hozzáférés felvétele</span><span class="sxs-lookup"><span data-stu-id="d0fdf-124">Add Access</span></span>
<span data-ttu-id="d0fdf-125">A hozzáférés a szerepkör-hozzárendelés hatókörébe tartozó erőforrásból, erőforráscsoportból vagy előfizetésből biztosítható.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-125">You grant access from within the resource, resource group, or subscription that is the scope of the role assignment.</span></span>

1. <span data-ttu-id="d0fdf-126">Kattintson a **Hozzáadás** gombra az Access control panelen.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-126">Select **Add** on the Access control blade.</span></span>  
2. <span data-ttu-id="d0fdf-127">A **Szerepkör kiválasztása** panelen válassza ki a felvenni kívánt szerepkört.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-127">Select the role that you wish to assign from the **Select a role** blade.</span></span>
3. <span data-ttu-id="d0fdf-128">Válassza ki azt a felhasználót, csoportot vagy alkalmazást a címtárában, amely számára hozzáférést kíván biztosítani.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-128">Select the user, group, or application in your directory that you wish to grant access to.</span></span> <span data-ttu-id="d0fdf-129">A címtárban rákereshet megjelenítendő nevekre, e-mail-címekre és objektumazonosítókra.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-129">You can search the directory with display names, email addresses, and object identifiers.</span></span>  
   
    ![Felhasználók hozzáadása panel – keresés – képernyőfelvétel](./media/role-based-access-control-configure/grant-access2.png)
4. <span data-ttu-id="d0fdf-131">A hozzárendelés létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-131">Select **OK** to create the assignment.</span></span> <span data-ttu-id="d0fdf-132">A **Felhasználó felvétele** előugró ablak nyomon követi a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-132">The **Adding user** popup tracks the progress.</span></span>  
    <span data-ttu-id="d0fdf-133">![Felhasználó felvétele folyamatjelző sáv – képernyőfelvétel](./media/role-based-access-control-configure/addinguser_popup.png)</span><span class="sxs-lookup"><span data-stu-id="d0fdf-133">![Adding user progress bar - screenshot](./media/role-based-access-control-configure/addinguser_popup.png)</span></span>

<span data-ttu-id="d0fdf-134">A szerepkör-hozzárendelés a sikeres felvétel után megjelenik a **Felhasználók** panelen.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-134">After successfully adding a role assignment, it will appear on the **Users** blade.</span></span>

## <a name="remove-access"></a><span data-ttu-id="d0fdf-135">Hozzáférés megszüntetése</span><span class="sxs-lookup"><span data-stu-id="d0fdf-135">Remove Access</span></span>
1. <span data-ttu-id="d0fdf-136">Vigye az egérmutatót az eltávolítani kívánt hozzárendelés fölé.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-136">Hover your cursor over the name of the assignment that you want to remove.</span></span> <span data-ttu-id="d0fdf-137">Ekkor megjelenik egy jelölőnégyzet a név mellett.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-137">A check box appears next to the name.</span></span>
2. <span data-ttu-id="d0fdf-138">A jelölőnégyzetek segítségével válasszon ki egy vagy több szerepkör-hozzárendelést.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-138">Use the check boxes to select one or more role assignments.</span></span>
2. <span data-ttu-id="d0fdf-139">Válassza az **Eltávolítás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-139">Select **Remove**.</span></span>  
3. <span data-ttu-id="d0fdf-140">Válassza az **Igen** lehetőséget az eltávolítás megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-140">Select **Yes** to confirm the removal.</span></span>

<span data-ttu-id="d0fdf-141">Az örökölt hozzárendeléseket nem lehet eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-141">Inherited assignments cannot be removed.</span></span> <span data-ttu-id="d0fdf-142">Ha egy örökölt hozzárendelést kell eltávolítania, azt abban a hatókörben kell megtennie, ahol a szerepkör-hozzárendelés létrejött.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-142">If you need to remove an inherited assignment, you need to do it at the scope where the role assignment was created.</span></span> <span data-ttu-id="d0fdf-143">A **Hatókör** oszlopban az **Örökölt** elem melletti hivatkozás azokra az erőforrásokra mutat, ahol az adott szerepkör hozzárendelése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-143">In the **Scope** column, next to **Inherited** there is a link that takes you to the resources where this role was assigned.</span></span> <span data-ttu-id="d0fdf-144">Lépjen az ott szereplő erőforrásra a szerepkör-hozzárendelés eltávolításához.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-144">Go to the resource listed there to remove the role assignment.</span></span>

![Felhasználók panel – az örökölt hozzáférés letiltja az eltávolítás gombot – képernyőfelvétel](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a><span data-ttu-id="d0fdf-146">Más eszközök a hozzáférés kezelésére</span><span class="sxs-lookup"><span data-stu-id="d0fdf-146">Other tools to manage access</span></span>
<span data-ttu-id="d0fdf-147">A szerepkörök hozzárendelését és a hozzáférések kezelését más eszközökön is elvégezheti az Azure RBAC-parancsokkal, nem csak az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-147">You can assign roles and manage access with Azure RBAC commands in tools other than the Azure portal.</span></span>  <span data-ttu-id="d0fdf-148">Az alábbi hivatkozásokat követve további információkat szerezhet az előfeltételekről, és megismerkedhet az Azure RBAC-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="d0fdf-148">Follow the links to learn more about the prerequisites and get started with the Azure RBAC commands.</span></span>

* [<span data-ttu-id="d0fdf-149">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d0fdf-149">Azure PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
* [<span data-ttu-id="d0fdf-150">Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="d0fdf-150">Azure Command-Line Interface</span></span>](role-based-access-control-manage-access-azure-cli.md)
* [<span data-ttu-id="d0fdf-151">REST API</span><span class="sxs-lookup"><span data-stu-id="d0fdf-151">REST API</span></span>](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a><span data-ttu-id="d0fdf-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0fdf-152">Next Steps</span></span>
* [<span data-ttu-id="d0fdf-153">Jelentés létrehozása a hozzáférés-módosítások előzményeiről</span><span class="sxs-lookup"><span data-stu-id="d0fdf-153">Create an access change history report</span></span>](role-based-access-control-access-change-history-report.md)
* <span data-ttu-id="d0fdf-154">Lásd: [Beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="d0fdf-154">See the [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>
* <span data-ttu-id="d0fdf-155">Saját [egyéni szerepkörök az Azure RBAC-ben](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="d0fdf-155">Define your own [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>


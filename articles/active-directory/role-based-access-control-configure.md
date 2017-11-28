---
title: "Hozzáférés-vezérlés aaaRole alapú hello Azure portálon |} Microsoft Docs"
description: "Ismerkedés a hozzáférés-kezelés a szerepköralapú hozzáférés-vezérlés az Azure portál hello. Szerepkör hozzárendelések tooassign engedélyek tooyour erőforrások használatára."
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
ms.openlocfilehash: b87e00089b0fc93fb212b318330a6f22bfbf59e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-access-tooyour-azure-subscription-resources"></a><span data-ttu-id="6a540-104">Szerepköralapú hozzáférés-vezérlés toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához</span><span class="sxs-lookup"><span data-stu-id="6a540-104">Use Role-Based Access Control toomanage access tooyour Azure subscription resources</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a540-105">Hozzáférés kezelése felhasználó vagy csoport alapján</span><span class="sxs-lookup"><span data-stu-id="6a540-105">Manage access by user or group</span></span>](role-based-access-control-manage-assignments.md)
> * [<span data-ttu-id="6a540-106">Hozzáférés kezelése erőforrás alapján</span><span class="sxs-lookup"><span data-stu-id="6a540-106">Manage access by resource</span></span>](role-based-access-control-configure.md)

<span data-ttu-id="6a540-107">Az Azure Szerepköralapú hozzáférés-vezérlés (RBAC) részletes hozzáférés-vezérlést biztosít az Azure-hoz.</span><span class="sxs-lookup"><span data-stu-id="6a540-107">Azure Role-Based Access Control (RBAC) enables fine-grained access management for Azure.</span></span> <span data-ttu-id="6a540-108">Az RBAC használata, biztosíthat csak hello mértékű hozzáférést a felhasználóknak frissíteniük kell tooperform a munkájukat.</span><span class="sxs-lookup"><span data-stu-id="6a540-108">Using RBAC, you can grant only hello amount of access that users need tooperform their jobs.</span></span> <span data-ttu-id="6a540-109">Ez a cikk segít, amelyekből megismerheti az RBAC a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6a540-109">This article helps you get up and running with RBAC in hello Azure portal.</span></span> <span data-ttu-id="6a540-110">Ha további részleteket szeretne arról, hogy hogyan segít az RBAC a hozzáférések kezelésében, tekintse meg [a szerepköralapú hozzáférés-vezérlést](role-based-access-control-what-is.md) ismertető szakaszt.</span><span class="sxs-lookup"><span data-stu-id="6a540-110">If you want more details about how RBAC helps you manage access, see [What is Role-Based Access Control](role-based-access-control-what-is.md).</span></span>

<span data-ttu-id="6a540-111">Minden előfizetésen belül biztosíthat a szerepkör-hozzárendelések too2000 fel.</span><span class="sxs-lookup"><span data-stu-id="6a540-111">Within each subscription, you can grant up too2000 role assignments.</span></span> 

## <a name="view-access"></a><span data-ttu-id="6a540-112">Hozzáférés megtekintése</span><span class="sxs-lookup"><span data-stu-id="6a540-112">View access</span></span>
<span data-ttu-id="6a540-113">Láthatja, hogy hozzáférési tooa erőforrás, erőforráscsoportból vagy előfizetés fő paneljén kik hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a540-113">You can see who has access tooa resource, resource group, or subscription from its main blade in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6a540-114">Például azt szeretnénk, toosee rendelkező hozzáférés tooone erőforráscsoporthoz:</span><span class="sxs-lookup"><span data-stu-id="6a540-114">For example, we want toosee who has access tooone of our resource groups:</span></span>

1. <span data-ttu-id="6a540-115">Válassza ki **erőforráscsoportok** hello bal oldali navigációs sávján hello.</span><span class="sxs-lookup"><span data-stu-id="6a540-115">Select **Resource groups** in hello navigation bar on hello left.</span></span>  
    <span data-ttu-id="6a540-116">![Erőforráscsoportok – ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span><span class="sxs-lookup"><span data-stu-id="6a540-116">![Resource groups - icon](./media/role-based-access-control-configure/resourcegroups_icon.png)</span></span>
2. <span data-ttu-id="6a540-117">Hello hello erőforráscsoport nevét válassza hello **erőforráscsoportok** panelen.</span><span class="sxs-lookup"><span data-stu-id="6a540-117">Select hello name of hello resource group from hello **Resource groups** blade.</span></span>
3. <span data-ttu-id="6a540-118">Válassza ki **hozzáférés-vezérlés (IAM)** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="6a540-118">Select **Access control (IAM)** from hello left menu.</span></span>  
4. <span data-ttu-id="6a540-119">hello Access control panel összes felhasználók, csoportok és alkalmazások, amely rendelkezik hozzáférési toohello erőforráscsoport sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="6a540-119">hello Access control blade lists all users, groups, and applications that have been granted access toohello resource group.</span></span>  
   
    ![Képernyőfelvétel a felhasználók panelről – örökölt vagy hozzárendelt hozzáférés](./media/role-based-access-control-configure/view-access.png)

<span data-ttu-id="6a540-121">Figyelje meg, hogy egyes szerepkörök hatóköre túl**ehhez az erőforráshoz** vannak **örökölt** azt egy másik hatókörben.</span><span class="sxs-lookup"><span data-stu-id="6a540-121">Notice that some roles are scoped too**This resource** while others are **Inherited** it from another scope.</span></span> <span data-ttu-id="6a540-122">Hozzáférés hozzárendelt kifejezetten toohello erőforráscsoport vagy egy hozzárendelés toohello szülő előfizetés öröklődik.</span><span class="sxs-lookup"><span data-stu-id="6a540-122">Access is either assigned specifically toohello resource group or inherited from an assignment toohello parent subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="6a540-123">Hagyományos előfizetés rendszergazdák és a társadminisztrátoroknak minősülnek tulajdonosoknak hello előfizetés hello új RBAC-modellben.</span><span class="sxs-lookup"><span data-stu-id="6a540-123">Classic subscription admins and co-admins are considered owners of hello subscription in hello new RBAC model.</span></span>

## <a name="add-access"></a><span data-ttu-id="6a540-124">Hozzáférés felvétele</span><span class="sxs-lookup"><span data-stu-id="6a540-124">Add Access</span></span>
<span data-ttu-id="6a540-125">Megadja a hozzáférés hello erőforrás, erőforráscsoportból vagy hello szerepkör-hozzárendelés hello hatókörében van.</span><span class="sxs-lookup"><span data-stu-id="6a540-125">You grant access from within hello resource, resource group, or subscription that is hello scope of hello role assignment.</span></span>

1. <span data-ttu-id="6a540-126">Válassza ki **Hozzáadás** hello Access control panelen.</span><span class="sxs-lookup"><span data-stu-id="6a540-126">Select **Add** on hello Access control blade.</span></span>  
2. <span data-ttu-id="6a540-127">Jelölje be hello szerepkör, hogy kívánja-e a hello tooassign **Szerepkörválasztás** panelen.</span><span class="sxs-lookup"><span data-stu-id="6a540-127">Select hello role that you wish tooassign from hello **Select a role** blade.</span></span>
3. <span data-ttu-id="6a540-128">Válassza ki a hello felhasználó, csoport vagy alkalmazást a címtárában, amely toogrant elérésére kívánja.</span><span class="sxs-lookup"><span data-stu-id="6a540-128">Select hello user, group, or application in your directory that you wish toogrant access to.</span></span> <span data-ttu-id="6a540-129">A megjelenített nevek, e-mail címekre és objektumazonosítókra hello directory kereshet.</span><span class="sxs-lookup"><span data-stu-id="6a540-129">You can search hello directory with display names, email addresses, and object identifiers.</span></span>  
   
    ![Felhasználók hozzáadása panel – keresés – képernyőfelvétel](./media/role-based-access-control-configure/grant-access2.png)
4. <span data-ttu-id="6a540-131">Válassza ki **OK** toocreate hello hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="6a540-131">Select **OK** toocreate hello assignment.</span></span> <span data-ttu-id="6a540-132">Hello **felhasználó felvétele** előugró ablak nyomon követi a hello folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="6a540-132">hello **Adding user** popup tracks hello progress.</span></span>  
    <span data-ttu-id="6a540-133">![Felhasználó felvétele folyamatjelző sáv – képernyőfelvétel](./media/role-based-access-control-configure/addinguser_popup.png)</span><span class="sxs-lookup"><span data-stu-id="6a540-133">![Adding user progress bar - screenshot](./media/role-based-access-control-configure/addinguser_popup.png)</span></span>

<span data-ttu-id="6a540-134">Sikerült a szerepkör-hozzárendelés felvett, az megjelenik hello **felhasználók** panelen.</span><span class="sxs-lookup"><span data-stu-id="6a540-134">After successfully adding a role assignment, it will appear on hello **Users** blade.</span></span>

## <a name="remove-access"></a><span data-ttu-id="6a540-135">Hozzáférés megszüntetése</span><span class="sxs-lookup"><span data-stu-id="6a540-135">Remove Access</span></span>
1. <span data-ttu-id="6a540-136">A kurzorral rámutat hello nevét, amelyet az tooremove hello hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="6a540-136">Hover your cursor over hello name of hello assignment that you want tooremove.</span></span> <span data-ttu-id="6a540-137">A jelölőnégyzet következő toohello neve.</span><span class="sxs-lookup"><span data-stu-id="6a540-137">A check box appears next toohello name.</span></span>
2. <span data-ttu-id="6a540-138">Hello jelölőnégyzetek tooselect használja egy vagy több szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="6a540-138">Use hello check boxes tooselect one or more role assignments.</span></span>
2. <span data-ttu-id="6a540-139">Válassza az **Eltávolítás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6a540-139">Select **Remove**.</span></span>  
3. <span data-ttu-id="6a540-140">Válassza ki **Igen** tooconfirm hello eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="6a540-140">Select **Yes** tooconfirm hello removal.</span></span>

<span data-ttu-id="6a540-141">Az örökölt hozzárendeléseket nem lehet eltávolítani.</span><span class="sxs-lookup"><span data-stu-id="6a540-141">Inherited assignments cannot be removed.</span></span> <span data-ttu-id="6a540-142">Ha tooremove örökölt hozzárendelés van szüksége, meg kell toodo hello a hatókör, ahol a hello szerepkör-hozzárendelés létrehozása történt.</span><span class="sxs-lookup"><span data-stu-id="6a540-142">If you need tooremove an inherited assignment, you need toodo it at hello scope where hello role assignment was created.</span></span> <span data-ttu-id="6a540-143">A hello **hatókör** oszlop, a következő túl**örökölt** egy hivatkozás, amely toohello erőforrások ahol ezt a szerepkört rendelték.</span><span class="sxs-lookup"><span data-stu-id="6a540-143">In hello **Scope** column, next too**Inherited** there is a link that takes you toohello resources where this role was assigned.</span></span> <span data-ttu-id="6a540-144">Lépjen az ott szereplő erőforrásra toohello tooremove hello szerepkör-hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="6a540-144">Go toohello resource listed there tooremove hello role assignment.</span></span>

![Felhasználók panel – az örökölt hozzáférés letiltja az eltávolítás gombot – képernyőfelvétel](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-toomanage-access"></a><span data-ttu-id="6a540-146">Más eszközök toomanage hozzáférés</span><span class="sxs-lookup"><span data-stu-id="6a540-146">Other tools toomanage access</span></span>
<span data-ttu-id="6a540-147">Szerepkörök hozzárendelése, és hozzáférés az Azure RBAC-parancsokkal hello Azure-portálon kívül eszközök kezelése.</span><span class="sxs-lookup"><span data-stu-id="6a540-147">You can assign roles and manage access with Azure RBAC commands in tools other than hello Azure portal.</span></span>  <span data-ttu-id="6a540-148">Hajtsa végre a hello hivatkozások toolearn hello előfeltételeivel kapcsolatos további, valamint Ismerkedés a hello Azure RBAC-parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="6a540-148">Follow hello links toolearn more about hello prerequisites and get started with hello Azure RBAC commands.</span></span>

* [<span data-ttu-id="6a540-149">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6a540-149">Azure PowerShell</span></span>](role-based-access-control-manage-access-powershell.md)
* [<span data-ttu-id="6a540-150">Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="6a540-150">Azure Command-Line Interface</span></span>](role-based-access-control-manage-access-azure-cli.md)
* [<span data-ttu-id="6a540-151">REST API</span><span class="sxs-lookup"><span data-stu-id="6a540-151">REST API</span></span>](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a><span data-ttu-id="6a540-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a540-152">Next Steps</span></span>
* [<span data-ttu-id="6a540-153">Jelentés létrehozása a hozzáférés-módosítások előzményeiről</span><span class="sxs-lookup"><span data-stu-id="6a540-153">Create an access change history report</span></span>](role-based-access-control-access-change-history-report.md)
* <span data-ttu-id="6a540-154">Lásd: hello [beépített RBAC-szerepkörök](role-based-access-built-in-roles.md)</span><span class="sxs-lookup"><span data-stu-id="6a540-154">See hello [RBAC built-in roles](role-based-access-built-in-roles.md)</span></span>
* <span data-ttu-id="6a540-155">Saját [egyéni szerepkörök az Azure RBAC-ben](role-based-access-control-custom-roles.md)</span><span class="sxs-lookup"><span data-stu-id="6a540-155">Define your own [Custom roles in Azure RBAC](role-based-access-control-custom-roles.md)</span></span>


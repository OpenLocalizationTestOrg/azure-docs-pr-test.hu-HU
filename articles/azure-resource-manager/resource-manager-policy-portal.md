---
title: "az erőforrás-házirendek aaaAzure portál |} Microsoft Docs"
description: "Ismerteti, hogyan toouse az Azure portál toocreate és erőforrás-kezelő házirendek kezelése. Szabályzatok alkalmazhatók a hello előfizetés vagy az erőforrás-csoportok."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: ce6413386317e532b63761a24458b85c996af4a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-portal-tooassign-and-manage-resource-policies"></a><span data-ttu-id="88d5e-104">Az Azure portál tooassign használata és erőforrás-házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="88d5e-104">Use Azure portal tooassign and manage resource policies</span></span>
<span data-ttu-id="88d5e-105">hello Azure-portálon lehetővé teszi, hogy tooassign házirendek tooresource erőforráscsoportok és előfizetések.</span><span class="sxs-lookup"><span data-stu-id="88d5e-105">hello Azure portal enables you tooassign resource policies tooresource groups and subscriptions.</span></span> <span data-ttu-id="88d5e-106">hello felhasználói felület segítségével könnyen tooselect hello házirend, hogy szeretné, hogy tooassign, és adja meg, hogy a házirend toocustomize hello házirend-beállítások paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="88d5e-106">hello user interface makes it easy tooselect hello policy that you want tooassign, and specify parameter values for that policy toocustomize hello policy settings.</span></span> 

<span data-ttu-id="88d5e-107">egy házirend hello portálon keresztül tooassign, hello házirend-definíció már léteznie kell az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="88d5e-107">tooassign a policy through hello portal, hello policy definition must already exist in your subscription.</span></span> <span data-ttu-id="88d5e-108">Az előfizetés rendelkezik, amely készen áll az Ön tooassign tooresource csoportok vagy előfizetések számos beépített házirend-definíciók.</span><span class="sxs-lookup"><span data-stu-id="88d5e-108">Your subscription has several built-in policy definitions that are ready for you tooassign tooresource groups or subscriptions.</span></span> <span data-ttu-id="88d5e-109">Láthatja, hogy a beépített házirendek és hello portál tooassign házirendek használatakor definiált egyéni házirendekben.</span><span class="sxs-lookup"><span data-stu-id="88d5e-109">You see these built-in policies and any custom policies you have defined when using hello portal tooassign policies.</span></span> <span data-ttu-id="88d5e-110">Egy bevezető toopolicies és hogyan toodefine testreszabott házirendet: [erőforrás házirendek – áttekintés](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="88d5e-110">For an introduction toopolicies and how toodefine customized policy, see [Resource policy overview](resource-manager-policy.md).</span></span>

<span data-ttu-id="88d5e-111">Összes gyermek-erőforrás által örökölt házirendek.</span><span class="sxs-lookup"><span data-stu-id="88d5e-111">Policies are inherited by all child resources.</span></span> <span data-ttu-id="88d5e-112">Ezért, ha a házirend alkalmazott tooa erőforráscsoportban, ezt a megfelelő tooall hello erőforrások az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="88d5e-112">So, if a policy is applied tooa resource group, it is applicable tooall hello resources in that resource group.</span></span> <span data-ttu-id="88d5e-113">Ebben a cikkben hello kifejezés **hatókör** toohello erőforráscsoportba vagy előfizetésbe, hozzárendelt hello házirend hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="88d5e-113">In this article, hello term **scope** refers toohello resource group or subscription that is assigned hello policy.</span></span> 

<span data-ttu-id="88d5e-114">Házirendek létrehozása, és az erőforrások (PUT és műveletek javítás) frissítésekor kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="88d5e-114">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

## <a name="assign-a-policy"></a><span data-ttu-id="88d5e-115">A házirend kijelölése</span><span class="sxs-lookup"><span data-stu-id="88d5e-115">Assign a policy</span></span>

1. <span data-ttu-id="88d5e-116">egy olyan erőforráscsoport házirend tooeither tooassign vagy előfizetés, válassza ki, hogy erőforráscsoportba vagy előfizetésbe.</span><span class="sxs-lookup"><span data-stu-id="88d5e-116">tooassign a policy tooeither a resource group or subscription, select that resource group or subscription.</span></span> <span data-ttu-id="88d5e-117">Hello beállításaiban, válassza ki a **házirendek**.</span><span class="sxs-lookup"><span data-stu-id="88d5e-117">In hello settings, select **Policies**.</span></span>

   ![Jelölje ki a házirendek](./media/resource-manager-policy-portal/select-policies.png)

2. <span data-ttu-id="88d5e-119">toocreate egy házirend-hozzárendelést a hatókör kiválasztása **adja hozzá a hozzárendelés**.</span><span class="sxs-lookup"><span data-stu-id="88d5e-119">toocreate a policy assignment for this scope, select **Add assignment**.</span></span>

   ![hozzárendelés hozzáadása](./media/resource-manager-policy-portal/add-assignment.png)

3. <span data-ttu-id="88d5e-121">Válassza ki a kívánt tooassign hello házirendet.</span><span class="sxs-lookup"><span data-stu-id="88d5e-121">Select hello policy you want tooassign.</span></span> <span data-ttu-id="88d5e-122">Akkor is, ha még nem hozzá egyetlen házirend-definíciók tooyour előfizetéshez, lásd: hello beépített házirendek kiosztására használható.</span><span class="sxs-lookup"><span data-stu-id="88d5e-122">Even if you have not added any policy definitions tooyour subscription, you see hello built-in policies that are available for assignment.</span></span> <span data-ttu-id="88d5e-123">A beépített házirendek számos gyakori helyzetek foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="88d5e-123">These built-in policies cover many common scenarios.</span></span>

   ![Válassza ki a definíció](./media/resource-manager-policy-portal/select-definition.png)

4. <span data-ttu-id="88d5e-125">A házirend kijelölése után megjelenik a hello házirend leírását, és a házirendhez tartozó paramétereket.</span><span class="sxs-lookup"><span data-stu-id="88d5e-125">After selecting a policy, you see a description of hello policy, and any parameters for that policy.</span></span> <span data-ttu-id="88d5e-126">Például hello következő kép bemutatja hello **engedélyezett helyek** paraméter, amely korlátozza a hello elérhető helyről hello házirend szükséges.</span><span class="sxs-lookup"><span data-stu-id="88d5e-126">For example, hello following image shows hello **Allowed locations** parameter, which is required for hello policy that restricts hello available locations.</span></span>

   ![paraméterek megjelenítése](./media/resource-manager-policy-portal/show-parameters.png)

5. <span data-ttu-id="88d5e-128">Hello felhasználói felületen válassza ki a hello értékek toospecify hello házirend paraméterek (például a központi telepítéshez használható hello helyek).</span><span class="sxs-lookup"><span data-stu-id="88d5e-128">Through hello user interface, select hello values toospecify for hello policy parameters (such as hello locations that can be used for deployment).</span></span>

   ![Válassza ki a paramétert értékek](./media/resource-manager-policy-portal/select-parameters.png)

6. <span data-ttu-id="88d5e-130">Adja meg hello más paramétereket.</span><span class="sxs-lookup"><span data-stu-id="88d5e-130">Provide values for hello other parameters.</span></span> <span data-ttu-id="88d5e-131">hello hatókör automatikusan hello házirend-hozzárendelés indításakor kijelölt hello panel alapján.</span><span class="sxs-lookup"><span data-stu-id="88d5e-131">hello scope is automatically assigned based on hello blade you selected when starting hello policy assignment.</span></span> <span data-ttu-id="88d5e-132">Kattintson az **OK** gombra, amikor végzett.</span><span class="sxs-lookup"><span data-stu-id="88d5e-132">Select **OK** when done.</span></span>

   ![Paraméterek megadása](./media/resource-manager-policy-portal/define-parameters.png)

  <span data-ttu-id="88d5e-134">Hello házirend hozzárendelt toohello megadott hatókörben.</span><span class="sxs-lookup"><span data-stu-id="88d5e-134">You have assigned hello policy toohello specified scope.</span></span>

## <a name="view-policy-assignments"></a><span data-ttu-id="88d5e-135">Házirend-hozzárendelések megtekintése</span><span class="sxs-lookup"><span data-stu-id="88d5e-135">View policy assignments</span></span>

<span data-ttu-id="88d5e-136">Házirend hozzárendelése után látható az adott hatókörnél házirendek hello listája.</span><span class="sxs-lookup"><span data-stu-id="88d5e-136">After assigning a policy, you see it in hello list of policies for that scope.</span></span> <span data-ttu-id="88d5e-137">Hello **részletek** lapon hello házirend-hozzárendelés összegzését jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="88d5e-137">hello **Details** tab shows a summary of hello policy assignment.</span></span>

![Részletek megjelenítése](./media/resource-manager-policy-portal/show-details.png)

<span data-ttu-id="88d5e-139">Hello **hozzárendelési szabály** lapon látható hello JSON hello házirend-definíció.</span><span class="sxs-lookup"><span data-stu-id="88d5e-139">hello **Assignment rule** tab shows hello JSON for hello policy definition.</span></span>

![hozzárendelési szabály megjelenítése](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a><span data-ttu-id="88d5e-141">Egy meglévő házirend-hozzárendelés módosítása</span><span class="sxs-lookup"><span data-stu-id="88d5e-141">Change an existing policy assignment</span></span>

<span data-ttu-id="88d5e-142">toochange egy házirendet, válassza ki **hozzárendelés szerkesztése** vagy **törlése**</span><span class="sxs-lookup"><span data-stu-id="88d5e-142">toochange a policy, select **Edit assignment** or **Delete**</span></span>

![szerkesztheti és törölheti a hozzárendelés](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a><span data-ttu-id="88d5e-144">Rendelje hozzá az egyéni házirendek</span><span class="sxs-lookup"><span data-stu-id="88d5e-144">Assign custom policies</span></span>

<span data-ttu-id="88d5e-145">Egyéni házirendeket az előfizetés adta meg, ha ezek a házirendek legyenek hello portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="88d5e-145">If you have defined custom policies in your subscription, those policies are available for assignment through hello portal.</span></span> <span data-ttu-id="88d5e-146">Ezek a házirendek vannak végrehajtásával kerüli meg a **[egyéni]**</span><span class="sxs-lookup"><span data-stu-id="88d5e-146">Those policies are prefaced with **[Custom]**</span></span>

![Egyéni házirendek](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a><span data-ttu-id="88d5e-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="88d5e-148">Next steps</span></span>
* <span data-ttu-id="88d5e-149">toolearn kapcsolatos hello JSON szintaxisának meghatározó házirendek, lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="88d5e-149">toolearn about hello JSON syntax for defining policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="88d5e-150">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="88d5e-150">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="88d5e-151">hello házirend séma közzé van téve a [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="88d5e-151">hello policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 


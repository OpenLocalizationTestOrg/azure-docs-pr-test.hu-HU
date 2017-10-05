---
title: "Erőforrás-házirendek az Azure portálon |} Microsoft Docs"
description: "Ismerteti az erőforrás-kezelő szabályzatok létrehozása és kezelése az Azure portál használatával. Házirendek: az előfizetés vagy az erőforrás-csoportok alkalmazhatók."
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
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a><span data-ttu-id="5aff4-104">Az Azure portál használatával és kezelheti az erőforrás-házirendekkel</span><span class="sxs-lookup"><span data-stu-id="5aff4-104">Use Azure portal to assign and manage resource policies</span></span>
<span data-ttu-id="5aff4-105">Az Azure-portál lehetővé teszi, hogy rendelheti hozzá az erőforrás-házirendek erőforráscsoport-sablonok és előfizetések.</span><span class="sxs-lookup"><span data-stu-id="5aff4-105">The Azure portal enables you to assign resource policies to resource groups and subscriptions.</span></span> <span data-ttu-id="5aff4-106">A felhasználói felület segítségével egyszerűen válassza ki a házirendet, amelyet szeretne hozzárendelni, és adja meg a házirend-beállítások testreszabása házirendhez tartozó paraméterértékeket.</span><span class="sxs-lookup"><span data-stu-id="5aff4-106">The user interface makes it easy to select the policy that you want to assign, and specify parameter values for that policy to customize the policy settings.</span></span> 

<span data-ttu-id="5aff4-107">A portálon keresztül egy házirend hozzárendelése, a házirend-definíció már léteznie kell az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5aff4-107">To assign a policy through the portal, the policy definition must already exist in your subscription.</span></span> <span data-ttu-id="5aff4-108">Az előfizetés rendelkezik, amely készen rendelheti hozzá az erőforráscsoport-sablonok és előfizetések számos beépített házirend-definíciók.</span><span class="sxs-lookup"><span data-stu-id="5aff4-108">Your subscription has several built-in policy definitions that are ready for you to assign to resource groups or subscriptions.</span></span> <span data-ttu-id="5aff4-109">Láthatja, hogy a beépített házirendek és a portál használata házirendek rendelhetők definiált egyéni házirendekben.</span><span class="sxs-lookup"><span data-stu-id="5aff4-109">You see these built-in policies and any custom policies you have defined when using the portal to assign policies.</span></span> <span data-ttu-id="5aff4-110">A házirendek és hogyan adhat meg egyéni házirend bevezető, lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="5aff4-110">For an introduction to policies and how to define customized policy, see [Resource policy overview](resource-manager-policy.md).</span></span>

<span data-ttu-id="5aff4-111">Összes gyermek-erőforrás által örökölt házirendek.</span><span class="sxs-lookup"><span data-stu-id="5aff4-111">Policies are inherited by all child resources.</span></span> <span data-ttu-id="5aff4-112">Igen a házirend vonatkozik egy erőforráscsoport, esetén alkalmazandó az erőforráscsoport összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="5aff4-112">So, if a policy is applied to a resource group, it is applicable to all the resources in that resource group.</span></span> <span data-ttu-id="5aff4-113">Ebben a cikkben kifejezés **hatókör** az erőforráscsoport vagy -előfizetéssel, amely hozzá van rendelve a házirend vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="5aff4-113">In this article, the term **scope** refers to the resource group or subscription that is assigned the policy.</span></span> 

<span data-ttu-id="5aff4-114">Házirendek létrehozása, és az erőforrások (PUT és műveletek javítás) frissítésekor kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="5aff4-114">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

## <a name="assign-a-policy"></a><span data-ttu-id="5aff4-115">A házirend kijelölése</span><span class="sxs-lookup"><span data-stu-id="5aff4-115">Assign a policy</span></span>

1. <span data-ttu-id="5aff4-116">Egy házirend hozzárendelése egy erőforráscsoport vagy egy előfizetés, válassza ki, hogy erőforráscsoportba vagy előfizetésbe.</span><span class="sxs-lookup"><span data-stu-id="5aff4-116">To assign a policy to either a resource group or subscription, select that resource group or subscription.</span></span> <span data-ttu-id="5aff4-117">Válassza a beállítások **házirendek**.</span><span class="sxs-lookup"><span data-stu-id="5aff4-117">In the settings, select **Policies**.</span></span>

   ![Jelölje ki a házirendek](./media/resource-manager-policy-portal/select-policies.png)

2. <span data-ttu-id="5aff4-119">A hatókör házirend-hozzárendelés létrehozásához válassza **adja hozzá a hozzárendelés**.</span><span class="sxs-lookup"><span data-stu-id="5aff4-119">To create a policy assignment for this scope, select **Add assignment**.</span></span>

   ![hozzárendelés hozzáadása](./media/resource-manager-policy-portal/add-assignment.png)

3. <span data-ttu-id="5aff4-121">Válassza ki a hozzárendelni kívánt házirendet.</span><span class="sxs-lookup"><span data-stu-id="5aff4-121">Select the policy you want to assign.</span></span> <span data-ttu-id="5aff4-122">Akkor is, ha az előfizetés nem hozzáadott összes házirend-definíciót, tekintse meg a beépített házirendek, amelyek rendelhető hozzá.</span><span class="sxs-lookup"><span data-stu-id="5aff4-122">Even if you have not added any policy definitions to your subscription, you see the built-in policies that are available for assignment.</span></span> <span data-ttu-id="5aff4-123">A beépített házirendek számos gyakori helyzetek foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="5aff4-123">These built-in policies cover many common scenarios.</span></span>

   ![Válassza ki a definíció](./media/resource-manager-policy-portal/select-definition.png)

4. <span data-ttu-id="5aff4-125">A házirend kijelölése után tekintse meg a házirendet, és a paramétereket, hogy a házirend leírását.</span><span class="sxs-lookup"><span data-stu-id="5aff4-125">After selecting a policy, you see a description of the policy, and any parameters for that policy.</span></span> <span data-ttu-id="5aff4-126">Például az alábbi képen látható a **engedélyezett helyek** paraméter, amely pedig szükséges a házirend, amely korlátozza a helyeket.</span><span class="sxs-lookup"><span data-stu-id="5aff4-126">For example, the following image shows the **Allowed locations** parameter, which is required for the policy that restricts the available locations.</span></span>

   ![paraméterek megjelenítése](./media/resource-manager-policy-portal/show-parameters.png)

5. <span data-ttu-id="5aff4-128">A felhasználói felületen jelölje ki az értékeket adja meg a házirend paraméterek (például a hely központi telepítéshez használható).</span><span class="sxs-lookup"><span data-stu-id="5aff4-128">Through the user interface, select the values to specify for the policy parameters (such as the locations that can be used for deployment).</span></span>

   ![Válassza ki a paramétert értékek](./media/resource-manager-policy-portal/select-parameters.png)

6. <span data-ttu-id="5aff4-130">Adja meg a többi paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="5aff4-130">Provide values for the other parameters.</span></span> <span data-ttu-id="5aff4-131">A hatókör automatikusan a panelen, a házirend-hozzárendelés indításakor kijelölt alapján.</span><span class="sxs-lookup"><span data-stu-id="5aff4-131">The scope is automatically assigned based on the blade you selected when starting the policy assignment.</span></span> <span data-ttu-id="5aff4-132">Kattintson az **OK** gombra, amikor végzett.</span><span class="sxs-lookup"><span data-stu-id="5aff4-132">Select **OK** when done.</span></span>

   ![Paraméterek megadása](./media/resource-manager-policy-portal/define-parameters.png)

  <span data-ttu-id="5aff4-134">A házirend rendelt-e a megadott hatókörben.</span><span class="sxs-lookup"><span data-stu-id="5aff4-134">You have assigned the policy to the specified scope.</span></span>

## <a name="view-policy-assignments"></a><span data-ttu-id="5aff4-135">Házirend-hozzárendelések megtekintése</span><span class="sxs-lookup"><span data-stu-id="5aff4-135">View policy assignments</span></span>

<span data-ttu-id="5aff4-136">Házirend hozzárendelése után azt látja, a házirendek az adott hatókörnél listájában.</span><span class="sxs-lookup"><span data-stu-id="5aff4-136">After assigning a policy, you see it in the list of policies for that scope.</span></span> <span data-ttu-id="5aff4-137">A **részletek** lapján a házirend-hozzárendelés összegzését jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5aff4-137">The **Details** tab shows a summary of the policy assignment.</span></span>

![Részletek megjelenítése](./media/resource-manager-policy-portal/show-details.png)

<span data-ttu-id="5aff4-139">A **hozzárendelési szabály** lapon láthatók a JSON a házirend-definíció.</span><span class="sxs-lookup"><span data-stu-id="5aff4-139">The **Assignment rule** tab shows the JSON for the policy definition.</span></span>

![hozzárendelési szabály megjelenítése](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a><span data-ttu-id="5aff4-141">Egy meglévő házirend-hozzárendelés módosítása</span><span class="sxs-lookup"><span data-stu-id="5aff4-141">Change an existing policy assignment</span></span>

<span data-ttu-id="5aff4-142">Ha módosítani szeretné a házirendet, válassza ki **hozzárendelés szerkesztése** vagy **törlése**</span><span class="sxs-lookup"><span data-stu-id="5aff4-142">To change a policy, select **Edit assignment** or **Delete**</span></span>

![szerkesztheti és törölheti a hozzárendelés](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a><span data-ttu-id="5aff4-144">Rendelje hozzá az egyéni házirendek</span><span class="sxs-lookup"><span data-stu-id="5aff4-144">Assign custom policies</span></span>

<span data-ttu-id="5aff4-145">Egyéni házirendeket az előfizetés adta meg, ha ezek a házirendek legyenek a portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="5aff4-145">If you have defined custom policies in your subscription, those policies are available for assignment through the portal.</span></span> <span data-ttu-id="5aff4-146">Ezek a házirendek vannak végrehajtásával kerüli meg a **[egyéni]**</span><span class="sxs-lookup"><span data-stu-id="5aff4-146">Those policies are prefaced with **[Custom]**</span></span>

![Egyéni házirendek](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a><span data-ttu-id="5aff4-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5aff4-148">Next steps</span></span>
* <span data-ttu-id="5aff4-149">A házirendek definiálása a JSON-szintaxissal kapcsolatos további tudnivalókért lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="5aff4-149">To learn about the JSON syntax for defining policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="5aff4-150">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="5aff4-150">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="5aff4-151">A házirend séma közzé van téve a [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="5aff4-151">The policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 


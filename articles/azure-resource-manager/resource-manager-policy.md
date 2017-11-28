---
title: "Az Azure erőforrás-házirendekkel |} Microsoft Docs"
description: "Azure Resource Manager-házirendek használata a központi telepítése során beállításának egységes erőforrás-tulajdonságokat ismerteti. Házirendek: az előfizetés vagy az erőforrás-csoportok alkalmazhatók."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2017
ms.author: tomfitz
ms.openlocfilehash: 0ee2624f45a1de0c23cae4538a38ae3e302eedd3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="resource-policy-overview"></a><span data-ttu-id="1279e-104">Erőforrás-házirendek – áttekintés</span><span class="sxs-lookup"><span data-stu-id="1279e-104">Resource policy overview</span></span>
<span data-ttu-id="1279e-105">Erőforrás-házirendek lehetővé teszik a szervezet egyezmények erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1279e-105">Resource policies enable you to establish conventions for resources in your organization.</span></span> <span data-ttu-id="1279e-106">Egyezmények meghatározásával szabályozhatja költségeit, és több könnyen kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1279e-106">By defining conventions, you can control costs and more easily manage your resources.</span></span> <span data-ttu-id="1279e-107">Megadhatja például, hogy engedélyezve legyenek-e a virtuális gépek csak bizonyos típusú.</span><span class="sxs-lookup"><span data-stu-id="1279e-107">For example, you can specify that only certain types of virtual machines are allowed.</span></span> <span data-ttu-id="1279e-108">Vagy megkövetelheti, hogy minden erőforrás egy bizonyos címkével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1279e-108">Or, you can require that all resources have a particular tag.</span></span> <span data-ttu-id="1279e-109">Összes gyermek-erőforrás által örökölt házirendek.</span><span class="sxs-lookup"><span data-stu-id="1279e-109">Policies are inherited by all child resources.</span></span> <span data-ttu-id="1279e-110">Igen a házirend vonatkozik egy erőforráscsoport, esetén alkalmazandó az erőforráscsoport összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="1279e-110">So, if a policy is applied to a resource group, it is applicable to all the resources in that resource group.</span></span>

<span data-ttu-id="1279e-111">Nincsenek két fogalom megértéséhez házirendek:</span><span class="sxs-lookup"><span data-stu-id="1279e-111">There are two concepts to understand about policies:</span></span>

* <span data-ttu-id="1279e-112">házirend-definíció - ismerteti Ha a házirend érvényesítve van-e, és milyen műveletet kell végrehajtani</span><span class="sxs-lookup"><span data-stu-id="1279e-112">policy definition - you describe when the policy is enforced and what action to take</span></span>
* <span data-ttu-id="1279e-113">házirend-hozzárendelés - alkalmazza a házirend-definíció a hatókör (előfizetés vagy az erőforrás csoport)</span><span class="sxs-lookup"><span data-stu-id="1279e-113">policy assignment - you apply the policy definition to a scope (subscription or resource group)</span></span>

<span data-ttu-id="1279e-114">Ez a témakör ismerteti a házirend-definíció.</span><span class="sxs-lookup"><span data-stu-id="1279e-114">This topic focuses on policy definition.</span></span> <span data-ttu-id="1279e-115">További információ a házirend-hozzárendelést: [hozzárendelésére és kezelésére erőforrás-házirendek használata Azure-portálon](resource-manager-policy-portal.md) vagy [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="1279e-115">For information about policy assignment, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md) or [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>

<span data-ttu-id="1279e-116">Házirendek létrehozása, és az erőforrások (PUT és műveletek javítás) frissítésekor kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="1279e-116">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

> [!NOTE]
> <span data-ttu-id="1279e-117">Házirend jelenleg nem értékelhetők erőforrástípusok esetében, amelyek nem támogatják a címkék, típusa és helye, például a Microsoft.Resources/deployments erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="1279e-117">Currently, policy does not evaluate resource types that do not support tags, kind, and location, such as the Microsoft.Resources/deployments resource type.</span></span> <span data-ttu-id="1279e-118">Ez a támogatás a későbbiekben lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="1279e-118">This support will be added at a future time.</span></span> <span data-ttu-id="1279e-119">Visszafelé kompatibilitási problémák elkerülése érdekében meg kell explicit módon adja meg házirendek runboookok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1279e-119">To avoid backward compatibility issues, you should explicitly specify type when authoring policies.</span></span> <span data-ttu-id="1279e-120">Például egy címke házirend, amely nem határoz meg típusok minden alkalmazástípus esetében érvényes.</span><span class="sxs-lookup"><span data-stu-id="1279e-120">For example, a tag policy that does not specify types is applied for all types.</span></span> <span data-ttu-id="1279e-121">Ebben az esetben egy sablon telepítés sikertelen lehet, ha egy beágyazott erőforrást, amely nem támogatja a címkék, és a központi telepítés erőforrástípus házirend értékelésekor hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="1279e-121">In that case, a template deployment may fail if there is a nested resource that doesn't support tags, and the deployment resource type has been added to policy evaluation.</span></span> 
> 
> 

## <a name="how-is-it-different-from-rbac"></a><span data-ttu-id="1279e-122">Miben különbözik az RBAC?</span><span class="sxs-lookup"><span data-stu-id="1279e-122">How is it different from RBAC?</span></span>
<span data-ttu-id="1279e-123">Házirend és a szerepköralapú hozzáférés-vezérlést (RBAC) közötti néhány fontos különbség van.</span><span class="sxs-lookup"><span data-stu-id="1279e-123">There are a few key differences between policy and role-based access control (RBAC).</span></span> <span data-ttu-id="1279e-124">Az RBAC-ra összpontosít **felhasználói** különböző hatóköröket műveletek.</span><span class="sxs-lookup"><span data-stu-id="1279e-124">RBAC focuses on **user** actions at different scopes.</span></span> <span data-ttu-id="1279e-125">Például akkor kerülnek a közreműködő szerepkört az egy erőforráscsoport, a kívánt hatókörben, erőforrás csoporthoz módosításokat végezheti el.</span><span class="sxs-lookup"><span data-stu-id="1279e-125">For example, you are added to the contributor role for a resource group at the desired scope, so you can make changes to that resource group.</span></span> <span data-ttu-id="1279e-126">Házirend összpontosít **erőforrás** tulajdonságok üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="1279e-126">Policy focuses on **resource** properties during deployment.</span></span> <span data-ttu-id="1279e-127">Például szabályzatokkal, szabályozhatja a telepíthető erőforrások típusú.</span><span class="sxs-lookup"><span data-stu-id="1279e-127">For example, through policies, you can control the types of resources that can be provisioned.</span></span> <span data-ttu-id="1279e-128">Vagy a helyek, amelyben az erőforrások kiépítése korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="1279e-128">Or, you can restrict the locations in which the resources can be provisioned.</span></span> <span data-ttu-id="1279e-129">Szerepalapú, eltérően házirend egy alapértelmezett engedélyezése és explicit megtagadja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="1279e-129">Unlike RBAC, policy is a default allow and explicit deny system.</span></span> 

<span data-ttu-id="1279e-130">Házirendek használatára kell hitelesíteni RBAC keresztül.</span><span class="sxs-lookup"><span data-stu-id="1279e-130">To use policies, you must be authenticated through RBAC.</span></span> <span data-ttu-id="1279e-131">Pontosabban, a fiókot kell-e a:</span><span class="sxs-lookup"><span data-stu-id="1279e-131">Specifically, your account needs the:</span></span>

* <span data-ttu-id="1279e-132">`Microsoft.Authorization/policydefinitions/write`egy házirend meghatározásához engedély</span><span class="sxs-lookup"><span data-stu-id="1279e-132">`Microsoft.Authorization/policydefinitions/write` permission to define a policy</span></span>
* <span data-ttu-id="1279e-133">`Microsoft.Authorization/policyassignments/write`egy házirend hozzárendelése engedély</span><span class="sxs-lookup"><span data-stu-id="1279e-133">`Microsoft.Authorization/policyassignments/write` permission to assign a policy</span></span> 

<span data-ttu-id="1279e-134">Ezek az engedélyek nem szerepelnek a **közreműködő** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="1279e-134">These permissions are not included in the **Contributor** role.</span></span>

## <a name="built-in-policies"></a><span data-ttu-id="1279e-135">Beépített házirendek</span><span class="sxs-lookup"><span data-stu-id="1279e-135">Built-in policies</span></span>

<span data-ttu-id="1279e-136">Azure biztosít néhány beépített házirend-definíciókat tartalmazzon, előfordulhat, hogy kevesebb házirendek meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="1279e-136">Azure provides some built-in policy definitions that may reduce the number of policies you have to define.</span></span> <span data-ttu-id="1279e-137">Házirend-definíciók a folytatás előtt érdemes lehet akár egy beépített házirend már használatával kell definition.</span><span class="sxs-lookup"><span data-stu-id="1279e-137">Before proceeding with policy definitions, you should consider whether a built-in policy already provides the definition you need.</span></span> <span data-ttu-id="1279e-138">A beépített házirend-definíciók száma a következők:</span><span class="sxs-lookup"><span data-stu-id="1279e-138">The built-in policy definitions are:</span></span>

* <span data-ttu-id="1279e-139">Engedélyezett helyek</span><span class="sxs-lookup"><span data-stu-id="1279e-139">Allowed locations</span></span>
* <span data-ttu-id="1279e-140">Engedélyezett erőforrástípusok</span><span class="sxs-lookup"><span data-stu-id="1279e-140">Allowed resource types</span></span>
* <span data-ttu-id="1279e-141">A tárfiók termékváltozatok engedélyezett</span><span class="sxs-lookup"><span data-stu-id="1279e-141">Allowed storage account SKUs</span></span>
* <span data-ttu-id="1279e-142">Virtuális gép termékváltozatok engedélyezett</span><span class="sxs-lookup"><span data-stu-id="1279e-142">Allowed virtual machine SKUs</span></span>
* <span data-ttu-id="1279e-143">Címke és az alapértelmezett érték alkalmazása</span><span class="sxs-lookup"><span data-stu-id="1279e-143">Apply tag and default value</span></span>
* <span data-ttu-id="1279e-144">Címke és érték</span><span class="sxs-lookup"><span data-stu-id="1279e-144">Enforce tag and value</span></span>
* <span data-ttu-id="1279e-145">Nem engedélyezett típusú erőforrások</span><span class="sxs-lookup"><span data-stu-id="1279e-145">Not allowed resource types</span></span>
* <span data-ttu-id="1279e-146">SQL Server verziója 12.0 megkövetelése</span><span class="sxs-lookup"><span data-stu-id="1279e-146">Require SQL Server version 12.0</span></span>
* <span data-ttu-id="1279e-147">Tárolási fiók titkosításának kötelezővé tétele</span><span class="sxs-lookup"><span data-stu-id="1279e-147">Require storage account encryption</span></span>

<span data-ttu-id="1279e-148">Említett házirendjeinek keresztül rendelhet a [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), vagy [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1279e-148">You can assign any of these policies through the [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), or [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span></span>

## <a name="policy-definition-structure"></a><span data-ttu-id="1279e-149">Házirend szerkezete</span><span class="sxs-lookup"><span data-stu-id="1279e-149">Policy definition structure</span></span>
<span data-ttu-id="1279e-150">JSON házirend-definíció létrehozására használhatja.</span><span class="sxs-lookup"><span data-stu-id="1279e-150">You use JSON to create a policy definition.</span></span> <span data-ttu-id="1279e-151">A házirend-definíció a elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="1279e-151">The policy definition contains elements for:</span></span>

* <span data-ttu-id="1279e-152">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1279e-152">parameters</span></span>
* <span data-ttu-id="1279e-153">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="1279e-153">display name</span></span>
* <span data-ttu-id="1279e-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-154">description</span></span>
* <span data-ttu-id="1279e-155">Házirend szabályai</span><span class="sxs-lookup"><span data-stu-id="1279e-155">policy rule</span></span>
  * <span data-ttu-id="1279e-156">logikai kiértékelése</span><span class="sxs-lookup"><span data-stu-id="1279e-156">logical evaluation</span></span>
  * <span data-ttu-id="1279e-157">hatása</span><span class="sxs-lookup"><span data-stu-id="1279e-157">effect</span></span>

<span data-ttu-id="1279e-158">A következő példa bemutatja egy házirendet, amely korlátozza, amelyekre központilag telepítették erőforrások:</span><span class="sxs-lookup"><span data-stu-id="1279e-158">The following example shows a policy that limits where resources are deployed:</span></span>

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="parameters"></a><span data-ttu-id="1279e-159">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1279e-159">Parameters</span></span>
<span data-ttu-id="1279e-160">Paraméterek használatával egyszerűbbé teszi a Csoportházirend kezelése a házirend-definíciók számának csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="1279e-160">Using parameters helps simplify your policy management by reducing the number of policy definitions.</span></span> <span data-ttu-id="1279e-161">Adja meg a házirendet (például korlátozza a hely, ahol erőforrásokat is telepíthető) erőforrás-tulajdonságot, és paraméterek bele.</span><span class="sxs-lookup"><span data-stu-id="1279e-161">You define a policy for a resource property (such as limiting the locations where resources can be deployed), and include parameters in the definition.</span></span> <span data-ttu-id="1279e-162">Ezt követően többször használ, hogy a házirend-definíció különböző helyzetek kezelésére történő különböző értékeket (például megadják az előfizetéshez tartozó helyek egy set) Ha a házirend hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="1279e-162">Then, you reuse that policy definition for different scenarios by passing in different values (such as specifying one set of locations for a subscription) when assigning the policy.</span></span>

<span data-ttu-id="1279e-163">Ha a házirend-meghatározások létrehozása deklarálhatja paraméterek.</span><span class="sxs-lookup"><span data-stu-id="1279e-163">You declare parameters when you create policy definitions.</span></span>

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

<span data-ttu-id="1279e-164">A paraméter típusa karakterlánc vagy tömb lehet.</span><span class="sxs-lookup"><span data-stu-id="1279e-164">The type of a parameter can be either string or array.</span></span> <span data-ttu-id="1279e-165">A metaadat-tulajdonságnak felhasználóbarát információk megjelenítésére szolgál például az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1279e-165">The metadata property is used for tools like Azure portal to display user-friendly information.</span></span> 

<span data-ttu-id="1279e-166">A házirend szabályban hivatkozási paraméter a következő szintaxissal:</span><span class="sxs-lookup"><span data-stu-id="1279e-166">In the policy rule, you reference parameters with the following syntax:</span></span> 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a><span data-ttu-id="1279e-167">Megjelenített név és leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-167">Display name and description</span></span>

<span data-ttu-id="1279e-168">Használja a **displayName** és **leírás** azonosíthatja a házirend-definíció, és adja meg a környezetben használat esetén.</span><span class="sxs-lookup"><span data-stu-id="1279e-168">You use the **displayName** and **description** to identify the policy definition, and provide context for when it is used.</span></span>

## <a name="policy-rule"></a><span data-ttu-id="1279e-169">Házirend szabályai</span><span class="sxs-lookup"><span data-stu-id="1279e-169">Policy rule</span></span>

<span data-ttu-id="1279e-170">A házirendszabály áll **Ha** és **majd** blokkolja.</span><span class="sxs-lookup"><span data-stu-id="1279e-170">The policy rule consists of **If** and **Then** blocks.</span></span> <span data-ttu-id="1279e-171">Az a **Ha** blokk, megadhatja, hogy egy vagy több feltételt, adja meg, ha a házirend érvényesítve van-e.</span><span class="sxs-lookup"><span data-stu-id="1279e-171">In the **If** block, you define one or more conditions that specify when the policy is enforced.</span></span> <span data-ttu-id="1279e-172">Logikai operátorok ezek a feltételek pontosan meghatározni a forgatókönyvhöz, amelyben egy házirendet alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1279e-172">You can apply logical operators to these conditions to precisely define the scenario for a policy.</span></span> <span data-ttu-id="1279e-173">Az a **majd** blokk, megadhatja a hatást, amelyet akkor fordul elő, amikor a **Ha** feltételek teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="1279e-173">In the **Then** block, you define the effect that happens when the **If** conditions are fulfilled.</span></span>

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a><span data-ttu-id="1279e-174">Logikai operátorok</span><span class="sxs-lookup"><span data-stu-id="1279e-174">Logical operators</span></span>
<span data-ttu-id="1279e-175">A támogatott logikai operátorok a következők:</span><span class="sxs-lookup"><span data-stu-id="1279e-175">The supported logical operators are:</span></span>

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

<span data-ttu-id="1279e-176">A **nem** szintaxis invertálja a feltétel eredménye.</span><span class="sxs-lookup"><span data-stu-id="1279e-176">The **not** syntax inverts the result of the condition.</span></span> <span data-ttu-id="1279e-177">A **allOf** szintaxis (a logikai hasonló **és** művelet) igényel minden feltétel igaz értékű lesz.</span><span class="sxs-lookup"><span data-stu-id="1279e-177">The **allOf** syntax (similar to the logical **And** operation) requires all conditions to be true.</span></span> <span data-ttu-id="1279e-178">A **anyOf** szintaxis (a logikai hasonló **vagy** művelet) szükséges egy vagy több feltétel igaz értékű lesz.</span><span class="sxs-lookup"><span data-stu-id="1279e-178">The **anyOf** syntax (similar to the logical **Or** operation) requires one or more conditions to be true.</span></span>

<span data-ttu-id="1279e-179">Logikai operátorok ágyazhatja be.</span><span class="sxs-lookup"><span data-stu-id="1279e-179">You can nest logical operators.</span></span> <span data-ttu-id="1279e-180">Az alábbi példa mutatja egy **nem** művelet, amelynek van beágyazva egy **allOf** műveletet.</span><span class="sxs-lookup"><span data-stu-id="1279e-180">The following example shows a **not** operation that is nested within an **allOf** operation.</span></span> 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a><span data-ttu-id="1279e-181">Feltételek</span><span class="sxs-lookup"><span data-stu-id="1279e-181">Conditions</span></span>
<span data-ttu-id="1279e-182">A feltétel e egy **mező** meghatározott feltételeknek eleget.</span><span class="sxs-lookup"><span data-stu-id="1279e-182">The condition evaluates whether a **field** meets certain criteria.</span></span> <span data-ttu-id="1279e-183">A támogatott feltételek esetén:</span><span class="sxs-lookup"><span data-stu-id="1279e-183">The supported conditions are:</span></span>

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

<span data-ttu-id="1279e-184">Használatakor a **például** feltétel, megadhatja a helyettesítő karakter (*) értékét.</span><span class="sxs-lookup"><span data-stu-id="1279e-184">When using the **like** condition, you can provide a wildcard (*) in the value.</span></span>

<span data-ttu-id="1279e-185">Használatakor a **megfelelő** feltétel, adja meg `#` képviselő számjegy, `?` betűvel, és bármely más karaktert adott tényleges karakter helyettesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-185">When using the **match** condition, provide `#` to represent a digit, `?` for a letter, and any other character to represent that actual character.</span></span> <span data-ttu-id="1279e-186">Tekintse meg a [nevét és az erőforrás-szabályzatok alkalmazása](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="1279e-186">For examples, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>

### <a name="fields"></a><span data-ttu-id="1279e-187">Mezők</span><span class="sxs-lookup"><span data-stu-id="1279e-187">Fields</span></span>
<span data-ttu-id="1279e-188">Feltételek mezőkkel jöttek létre.</span><span class="sxs-lookup"><span data-stu-id="1279e-188">Conditions are formed by using fields.</span></span> <span data-ttu-id="1279e-189">A mező képviseli tulajdonság az erőforrás-kérések forgalma, amellyel ismertetik az erőforrás állapotát.</span><span class="sxs-lookup"><span data-stu-id="1279e-189">A field represents properties in the resource request payload that is used to describe the state of the resource.</span></span>  

<span data-ttu-id="1279e-190">A következő mezők támogatottak:</span><span class="sxs-lookup"><span data-stu-id="1279e-190">The following fields are supported:</span></span>

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* <span data-ttu-id="1279e-191">tulajdonságának aliasokat - listájáért lásd: [aliasok](#aliases).</span><span class="sxs-lookup"><span data-stu-id="1279e-191">property aliases - for a list, see [Aliases](#aliases).</span></span>

### <a name="effect"></a><span data-ttu-id="1279e-192">Következmény</span><span class="sxs-lookup"><span data-stu-id="1279e-192">Effect</span></span>
<span data-ttu-id="1279e-193">Házirend érvénybe - három típusú támogat `deny`, `audit`, és `append`.</span><span class="sxs-lookup"><span data-stu-id="1279e-193">Policy supports three types of effect - `deny`, `audit`, and `append`.</span></span> 

* <span data-ttu-id="1279e-194">**Megtagadási** generál egy eseményt, a biztonsági naplóban, és a kérelem sikertelen</span><span class="sxs-lookup"><span data-stu-id="1279e-194">**Deny** generates an event in the audit log and fails the request</span></span>
* <span data-ttu-id="1279e-195">**Naplózási** naplót hoz létre a figyelmeztető üzenet, de nem tudja teljesíteni a kérést</span><span class="sxs-lookup"><span data-stu-id="1279e-195">**Audit** generates a warning event in audit log but does not fail the request</span></span>
* <span data-ttu-id="1279e-196">**Hozzáfűzendő** a definiált mezők halmaza alapján ad a kéréshez</span><span class="sxs-lookup"><span data-stu-id="1279e-196">**Append** adds the defined set of fields to the request</span></span> 

<span data-ttu-id="1279e-197">A **hozzáfűzése**, meg kell adnia a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="1279e-197">For **append**, you must provide the following details:</span></span>

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

<span data-ttu-id="1279e-198">Az érték lehet egy karakterlánc vagy egy JSON-formátumú objektum.</span><span class="sxs-lookup"><span data-stu-id="1279e-198">The value can be either a string or a JSON format object.</span></span> 

## <a name="aliases"></a><span data-ttu-id="1279e-199">Aliasok</span><span class="sxs-lookup"><span data-stu-id="1279e-199">Aliases</span></span>

<span data-ttu-id="1279e-200">Egy erőforrás típusára vonatkozó tulajdonságokat eléréséhez használt tulajdonságának aliasokat.</span><span class="sxs-lookup"><span data-stu-id="1279e-200">You use property aliases to access specific properties for a resource type.</span></span> <span data-ttu-id="1279e-201">Aliasok engedélyezi, hogy milyen értékeket, vagy a feltételek engedélyezve van a erőforrás-tulajdonságok korlátozása.</span><span class="sxs-lookup"><span data-stu-id="1279e-201">Aliases enable you to restrict what values or conditions are permitted for a property on a resource.</span></span> <span data-ttu-id="1279e-202">Mindegyik aliasnak elérési utak a különböző API-verziók egy adott erőforrástípusra van leképezve.</span><span class="sxs-lookup"><span data-stu-id="1279e-202">Each alias maps to paths in different API versions for a given resource type.</span></span> <span data-ttu-id="1279e-203">Házirend kiértékelése közben a házirendmotor lekérdezi az adott API-verzió útvonal.</span><span class="sxs-lookup"><span data-stu-id="1279e-203">During policy evaluation, the policy engine gets the property path for that API version.</span></span>

<span data-ttu-id="1279e-204">**Microsoft.Cache/Redis**</span><span class="sxs-lookup"><span data-stu-id="1279e-204">**Microsoft.Cache/Redis**</span></span>

| <span data-ttu-id="1279e-205">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-205">Alias</span></span> | <span data-ttu-id="1279e-206">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-206">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-207">Microsoft.Cache/Redis/enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="1279e-207">Microsoft.Cache/Redis/enableNonSslPort</span></span> | <span data-ttu-id="1279e-208">Állítsa e a nem ssl-Redis-kiszolgáló port (6379) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="1279e-208">Set whether the non-ssl Redis server port (6379) is enabled.</span></span> |
| <span data-ttu-id="1279e-209">Microsoft.Cache/Redis/shardCount</span><span class="sxs-lookup"><span data-stu-id="1279e-209">Microsoft.Cache/Redis/shardCount</span></span> | <span data-ttu-id="1279e-210">Adjon meg a szilánkok prémium fürt gyorsítótárat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1279e-210">Set the number of shards to be created on a Premium Cluster Cache.</span></span>  |
| <span data-ttu-id="1279e-211">Microsoft.Cache/Redis/sku.capacity</span><span class="sxs-lookup"><span data-stu-id="1279e-211">Microsoft.Cache/Redis/sku.capacity</span></span> | <span data-ttu-id="1279e-212">A telepítendő Redis gyorsítótár méretének beállítása.</span><span class="sxs-lookup"><span data-stu-id="1279e-212">Set the size of the Redis cache to deploy.</span></span>  |
| <span data-ttu-id="1279e-213">Microsoft.Cache/Redis/sku.family</span><span class="sxs-lookup"><span data-stu-id="1279e-213">Microsoft.Cache/Redis/sku.family</span></span> | <span data-ttu-id="1279e-214">Állítsa be az SKU-család használható.</span><span class="sxs-lookup"><span data-stu-id="1279e-214">Set the SKU family to use.</span></span> |
| <span data-ttu-id="1279e-215">Microsoft.Cache/Redis/sku.name</span><span class="sxs-lookup"><span data-stu-id="1279e-215">Microsoft.Cache/Redis/sku.name</span></span> | <span data-ttu-id="1279e-216">Állítsa be a Redis Cache telepítendő típusú.</span><span class="sxs-lookup"><span data-stu-id="1279e-216">Set the type of Redis Cache to deploy.</span></span> |

<span data-ttu-id="1279e-217">**Microsoft.Cdn/profiles**</span><span class="sxs-lookup"><span data-stu-id="1279e-217">**Microsoft.Cdn/profiles**</span></span>

| <span data-ttu-id="1279e-218">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-218">Alias</span></span> | <span data-ttu-id="1279e-219">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-219">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-220">Microsoft.CDN/profiles/sku.name</span><span class="sxs-lookup"><span data-stu-id="1279e-220">Microsoft.CDN/profiles/sku.name</span></span> | <span data-ttu-id="1279e-221">Állítsa be az árképzési szint nevét.</span><span class="sxs-lookup"><span data-stu-id="1279e-221">Set the name of the pricing tier.</span></span> |

<span data-ttu-id="1279e-222">**Microsoft.Compute/disks**</span><span class="sxs-lookup"><span data-stu-id="1279e-222">**Microsoft.Compute/disks**</span></span>

| <span data-ttu-id="1279e-223">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-223">Alias</span></span> | <span data-ttu-id="1279e-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-224">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-225">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="1279e-225">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="1279e-226">Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-226">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-227">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="1279e-227">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="1279e-228">Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-228">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-229">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="1279e-229">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="1279e-230">Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-230">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-231">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="1279e-231">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="1279e-232">Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját.</span><span class="sxs-lookup"><span data-stu-id="1279e-232">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |


<span data-ttu-id="1279e-233">**Microsoft.Compute/virtualMachines**</span><span class="sxs-lookup"><span data-stu-id="1279e-233">**Microsoft.Compute/virtualMachines**</span></span>

| <span data-ttu-id="1279e-234">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-234">Alias</span></span> | <span data-ttu-id="1279e-235">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-235">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-236">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="1279e-236">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="1279e-237">Állítsa be a virtuális gép létrehozásához használt kép azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1279e-237">Set the identifier of the image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-238">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="1279e-238">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="1279e-239">Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-239">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-240">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="1279e-240">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="1279e-241">Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-241">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-242">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="1279e-242">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="1279e-243">Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-243">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-244">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="1279e-244">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="1279e-245">Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját.</span><span class="sxs-lookup"><span data-stu-id="1279e-245">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-246">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="1279e-246">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="1279e-247">Állítsa be, hogy a lemezkép vagy lemez-e a licencelt helyszíni.</span><span class="sxs-lookup"><span data-stu-id="1279e-247">Set that the image or disk is licensed on-premises.</span></span> <span data-ttu-id="1279e-248">Ez az érték csak a Windows Server operációs rendszert tartalmazó lemezképek használatos.</span><span class="sxs-lookup"><span data-stu-id="1279e-248">This value is only used for images that contain the Windows Server operating system.</span></span>  |
| <span data-ttu-id="1279e-249">Microsoft.Compute/virtualMachines/imageOffer</span><span class="sxs-lookup"><span data-stu-id="1279e-249">Microsoft.Compute/virtualMachines/imageOffer</span></span> | <span data-ttu-id="1279e-250">Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-250">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-251">Microsoft.Compute/virtualMachines/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="1279e-251">Microsoft.Compute/virtualMachines/imagePublisher</span></span> | <span data-ttu-id="1279e-252">Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-252">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-253">Microsoft.Compute/virtualMachines/imageSku</span><span class="sxs-lookup"><span data-stu-id="1279e-253">Microsoft.Compute/virtualMachines/imageSku</span></span> | <span data-ttu-id="1279e-254">Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-254">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-255">Microsoft.Compute/virtualMachines/imageVersion</span><span class="sxs-lookup"><span data-stu-id="1279e-255">Microsoft.Compute/virtualMachines/imageVersion</span></span> | <span data-ttu-id="1279e-256">Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját.</span><span class="sxs-lookup"><span data-stu-id="1279e-256">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span><span class="sxs-lookup"><span data-stu-id="1279e-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span></span> | <span data-ttu-id="1279e-258">Állítsa be a virtuális merevlemez URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1279e-258">Set the vhd URI.</span></span> |
| <span data-ttu-id="1279e-259">Microsoft.Compute/virtualMachines/sku.name</span><span class="sxs-lookup"><span data-stu-id="1279e-259">Microsoft.Compute/virtualMachines/sku.name</span></span> | <span data-ttu-id="1279e-260">A virtuális gép méretének beállítása.</span><span class="sxs-lookup"><span data-stu-id="1279e-260">Set the size of the virtual machine.</span></span> |

<span data-ttu-id="1279e-261">**Microsoft.Compute/virtualMachines/extensions**</span><span class="sxs-lookup"><span data-stu-id="1279e-261">**Microsoft.Compute/virtualMachines/extensions**</span></span>

| <span data-ttu-id="1279e-262">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-262">Alias</span></span> | <span data-ttu-id="1279e-263">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-263">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-264">Microsoft.Compute/virtualMachines/extensions/publisher</span><span class="sxs-lookup"><span data-stu-id="1279e-264">Microsoft.Compute/virtualMachines/extensions/publisher</span></span> | <span data-ttu-id="1279e-265">Állítsa be a kiadó nevét: a bővítményt.</span><span class="sxs-lookup"><span data-stu-id="1279e-265">Set the name of the extension’s publisher.</span></span> |
| <span data-ttu-id="1279e-266">Microsoft.Compute/virtualMachines/extensions/type</span><span class="sxs-lookup"><span data-stu-id="1279e-266">Microsoft.Compute/virtualMachines/extensions/type</span></span> | <span data-ttu-id="1279e-267">Állítsa be a kiterjesztés típusú.</span><span class="sxs-lookup"><span data-stu-id="1279e-267">Set the type of extension.</span></span> |
| <span data-ttu-id="1279e-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="1279e-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span></span> | <span data-ttu-id="1279e-269">A verzió megadása</span><span class="sxs-lookup"><span data-stu-id="1279e-269">Set the version of the extension.</span></span> |

<span data-ttu-id="1279e-270">**Microsoft.Compute/virtualMachineScaleSets**</span><span class="sxs-lookup"><span data-stu-id="1279e-270">**Microsoft.Compute/virtualMachineScaleSets**</span></span>

| <span data-ttu-id="1279e-271">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-271">Alias</span></span> | <span data-ttu-id="1279e-272">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-272">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-273">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="1279e-273">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="1279e-274">Állítsa be a virtuális gép létrehozásához használt kép azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1279e-274">Set the identifier of the image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-275">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="1279e-275">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="1279e-276">Állítsa be az ajánlat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-276">Set the offer of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-277">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="1279e-277">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="1279e-278">Állítsa be a közzétevő platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-278">Set the publisher of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-279">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="1279e-279">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="1279e-280">Állítsa be a Termékváltozat platformlemezképet vagy a virtuális gép létrehozásához használt Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="1279e-280">Set the SKU of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-281">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="1279e-281">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="1279e-282">Állítsa be a platformlemezképet vagy a Piactéri lemezképhez a virtuális gép létrehozásához használt verzióját.</span><span class="sxs-lookup"><span data-stu-id="1279e-282">Set the version of the platform image or marketplace image used to create the virtual machine.</span></span> |
| <span data-ttu-id="1279e-283">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="1279e-283">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="1279e-284">Állítsa be, hogy a lemezkép vagy lemez-e a licencelt helyszíni.</span><span class="sxs-lookup"><span data-stu-id="1279e-284">Set that the image or disk is licensed on-premises.</span></span> <span data-ttu-id="1279e-285">Ez az érték csak a Windows Server operációs rendszert tartalmazó lemezképek használatos.</span><span class="sxs-lookup"><span data-stu-id="1279e-285">This value is only used for images that contain the Windows Server operating system.</span></span> |
| <span data-ttu-id="1279e-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="1279e-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span></span> | <span data-ttu-id="1279e-287">A méretezési csoportban lévő összes virtuális gépet a számítógép nevének előtagját beállítása</span><span class="sxs-lookup"><span data-stu-id="1279e-287">Set the computer name prefix for all  the virtual machines in the scale set.</span></span> |
| <span data-ttu-id="1279e-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span><span class="sxs-lookup"><span data-stu-id="1279e-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span></span> | <span data-ttu-id="1279e-289">Állítsa be a blob URI felhasználói lemezkép.</span><span class="sxs-lookup"><span data-stu-id="1279e-289">Set the blob URI for user image.</span></span> |
| <span data-ttu-id="1279e-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span><span class="sxs-lookup"><span data-stu-id="1279e-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span></span> | <span data-ttu-id="1279e-291">A méretezési az operációs rendszer lemezek tárolására használt tároló URL-címeinek beállítása.</span><span class="sxs-lookup"><span data-stu-id="1279e-291">Set the container URLs that are used to store operating system disks for the scale set.</span></span> |
| <span data-ttu-id="1279e-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span><span class="sxs-lookup"><span data-stu-id="1279e-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span></span> | <span data-ttu-id="1279e-293">Méretezési csoportban lévő virtuális gépek méretének beállítása.</span><span class="sxs-lookup"><span data-stu-id="1279e-293">Set the size of virtual machines in a scale set.</span></span> |
| <span data-ttu-id="1279e-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span><span class="sxs-lookup"><span data-stu-id="1279e-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span></span> | <span data-ttu-id="1279e-295">Méretezési csoportban lévő virtuális gépek szintjének beállítása</span><span class="sxs-lookup"><span data-stu-id="1279e-295">Set the tier of virtual machines in a scale set.</span></span> |
  
<span data-ttu-id="1279e-296">**Microsoft.Network/applicationGateways**</span><span class="sxs-lookup"><span data-stu-id="1279e-296">**Microsoft.Network/applicationGateways**</span></span>

| <span data-ttu-id="1279e-297">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-297">Alias</span></span> | <span data-ttu-id="1279e-298">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-298">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-299">Microsoft.Network/applicationGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="1279e-299">Microsoft.Network/applicationGateways/sku.name</span></span> | <span data-ttu-id="1279e-300">Az átjáró méretének beállítása.</span><span class="sxs-lookup"><span data-stu-id="1279e-300">Set the size of the gateway.</span></span> |

<span data-ttu-id="1279e-301">**Microsoft.Network/virtualNetworkGateways**</span><span class="sxs-lookup"><span data-stu-id="1279e-301">**Microsoft.Network/virtualNetworkGateways**</span></span>

| <span data-ttu-id="1279e-302">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-302">Alias</span></span> | <span data-ttu-id="1279e-303">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-303">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span><span class="sxs-lookup"><span data-stu-id="1279e-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span></span> | <span data-ttu-id="1279e-305">Állítsa be a virtuális hálózati átjáró típusát.</span><span class="sxs-lookup"><span data-stu-id="1279e-305">Set the type of this virtual network gateway.</span></span> |
| <span data-ttu-id="1279e-306">Microsoft.Network/virtualNetworkGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="1279e-306">Microsoft.Network/virtualNetworkGateways/sku.name</span></span> | <span data-ttu-id="1279e-307">Állítsa be az átjáró-termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="1279e-307">Set the gateway SKU name.</span></span> |

<span data-ttu-id="1279e-308">**Microsoft.Sql/servers**</span><span class="sxs-lookup"><span data-stu-id="1279e-308">**Microsoft.Sql/servers**</span></span>

| <span data-ttu-id="1279e-309">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-309">Alias</span></span> | <span data-ttu-id="1279e-310">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-310">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-311">Microsoft.Sql/servers/version</span><span class="sxs-lookup"><span data-stu-id="1279e-311">Microsoft.Sql/servers/version</span></span> | <span data-ttu-id="1279e-312">Állítsa be a kiszolgáló verziója.</span><span class="sxs-lookup"><span data-stu-id="1279e-312">Set the version of the server.</span></span> |

<span data-ttu-id="1279e-313">**Microsoft.Sql/databases**</span><span class="sxs-lookup"><span data-stu-id="1279e-313">**Microsoft.Sql/databases**</span></span>

| <span data-ttu-id="1279e-314">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-314">Alias</span></span> | <span data-ttu-id="1279e-315">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-315">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-316">Microsoft.Sql/servers/databases/edition</span><span class="sxs-lookup"><span data-stu-id="1279e-316">Microsoft.Sql/servers/databases/edition</span></span> | <span data-ttu-id="1279e-317">Állítsa be az adatbázis-kiadás.</span><span class="sxs-lookup"><span data-stu-id="1279e-317">Set the edition of the database.</span></span> |
| <span data-ttu-id="1279e-318">Microsoft.Sql/servers/databases/elasticPoolName</span><span class="sxs-lookup"><span data-stu-id="1279e-318">Microsoft.Sql/servers/databases/elasticPoolName</span></span> | <span data-ttu-id="1279e-319">Állítsa be a rugalmas készlet az adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="1279e-319">Set the name of the elastic pool the database is in.</span></span> |
| <span data-ttu-id="1279e-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span><span class="sxs-lookup"><span data-stu-id="1279e-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span></span> | <span data-ttu-id="1279e-321">Állítsa be az adatbázis konfigurált szolgáltatási szint célkitűzésének azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1279e-321">Set the configured service level objective ID of the database.</span></span> |
| <span data-ttu-id="1279e-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span><span class="sxs-lookup"><span data-stu-id="1279e-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span></span> | <span data-ttu-id="1279e-323">Állítsa be a konfigurált szolgáltatásiszint-célkitűzés az adatbázis nevét.</span><span class="sxs-lookup"><span data-stu-id="1279e-323">Set the name of the configured service level objective of the database.</span></span>  |

<span data-ttu-id="1279e-324">**Microsoft.Sql/elasticpools**</span><span class="sxs-lookup"><span data-stu-id="1279e-324">**Microsoft.Sql/elasticpools**</span></span>

| <span data-ttu-id="1279e-325">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-325">Alias</span></span> | <span data-ttu-id="1279e-326">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-326">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-327">kiszolgálók/elasticpools</span><span class="sxs-lookup"><span data-stu-id="1279e-327">servers/elasticpools</span></span> | <span data-ttu-id="1279e-328">Microsoft.Sql/servers/elasticPools/dtu</span><span class="sxs-lookup"><span data-stu-id="1279e-328">Microsoft.Sql/servers/elasticPools/dtu</span></span> | <span data-ttu-id="1279e-329">Állítsa be a rugalmas adatbáziskészlet az összes megosztott DTU.</span><span class="sxs-lookup"><span data-stu-id="1279e-329">Set the total shared DTU for the database elastic pool.</span></span> |
| <span data-ttu-id="1279e-330">kiszolgálók/elasticpools</span><span class="sxs-lookup"><span data-stu-id="1279e-330">servers/elasticpools</span></span> | <span data-ttu-id="1279e-331">Microsoft.Sql/servers/elasticPools/edition</span><span class="sxs-lookup"><span data-stu-id="1279e-331">Microsoft.Sql/servers/elasticPools/edition</span></span> | <span data-ttu-id="1279e-332">Állítsa be a rugalmas készlet kiadása.</span><span class="sxs-lookup"><span data-stu-id="1279e-332">Set the edition of the elastic pool.</span></span> |

<span data-ttu-id="1279e-333">**Microsoft.Storage/storageAccounts**</span><span class="sxs-lookup"><span data-stu-id="1279e-333">**Microsoft.Storage/storageAccounts**</span></span>

| <span data-ttu-id="1279e-334">Alias</span><span class="sxs-lookup"><span data-stu-id="1279e-334">Alias</span></span> | <span data-ttu-id="1279e-335">Leírás</span><span class="sxs-lookup"><span data-stu-id="1279e-335">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="1279e-336">Microsoft.Storage/storageAccounts/accessTier</span><span class="sxs-lookup"><span data-stu-id="1279e-336">Microsoft.Storage/storageAccounts/accessTier</span></span> | <span data-ttu-id="1279e-337">Állítsa be a hozzáférési réteg számlázási használatos.</span><span class="sxs-lookup"><span data-stu-id="1279e-337">Set the access tier used for billing.</span></span> |
| <span data-ttu-id="1279e-338">Microsoft.Storage/storageAccounts/accountType</span><span class="sxs-lookup"><span data-stu-id="1279e-338">Microsoft.Storage/storageAccounts/accountType</span></span> | <span data-ttu-id="1279e-339">Állítsa be a termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="1279e-339">Set the SKU name.</span></span> |
| <span data-ttu-id="1279e-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span><span class="sxs-lookup"><span data-stu-id="1279e-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span></span> | <span data-ttu-id="1279e-341">Állítsa be, hogy a szolgáltatás titkosítja az adatokat a blob storage szolgáltatásban tárolt.</span><span class="sxs-lookup"><span data-stu-id="1279e-341">Set whether the service encrypts the data as it is stored in the blob storage service.</span></span> |
| <span data-ttu-id="1279e-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span><span class="sxs-lookup"><span data-stu-id="1279e-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span></span> | <span data-ttu-id="1279e-343">Állítsa be, hogy a szolgáltatás titkosítja az adatokat, a fájl storage szolgáltatásban tárolt.</span><span class="sxs-lookup"><span data-stu-id="1279e-343">Set whether the service encrypts the data as it is stored in the file storage service.</span></span> |
| <span data-ttu-id="1279e-344">Microsoft.Storage/storageAccounts/sku.name</span><span class="sxs-lookup"><span data-stu-id="1279e-344">Microsoft.Storage/storageAccounts/sku.name</span></span> | <span data-ttu-id="1279e-345">Állítsa be a termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="1279e-345">Set the SKU name.</span></span> |
| <span data-ttu-id="1279e-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span><span class="sxs-lookup"><span data-stu-id="1279e-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span></span> | <span data-ttu-id="1279e-347">Beállítása, hogy a csak https-forgalom tároló szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1279e-347">Set to allow only https traffic to storage service.</span></span> |


## <a name="policy-examples"></a><span data-ttu-id="1279e-348">Házirend példák</span><span class="sxs-lookup"><span data-stu-id="1279e-348">Policy examples</span></span>

<span data-ttu-id="1279e-349">A következő témakörök tartalmaznak házirend példák:</span><span class="sxs-lookup"><span data-stu-id="1279e-349">The following topics contain policy examples:</span></span>

* <span data-ttu-id="1279e-350">Példák címke házirendek, lásd: [címkék erőforrás házirendek alkalmazását](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="1279e-350">For examples of tag polices, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>
* <span data-ttu-id="1279e-351">Annak a kiosztási és a szöveg, tekintse meg a [nevét és az erőforrás-szabályzatok alkalmazása](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="1279e-351">For examples of naming and text patterns, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>
* <span data-ttu-id="1279e-352">A tárolási házirendek, tekintse meg a [erőforrás házirendek alkalmazása a storage-fiókok](resource-manager-policy-storage.md).</span><span class="sxs-lookup"><span data-stu-id="1279e-352">For examples of storage policies, see [Apply resource policies to storage accounts](resource-manager-policy-storage.md).</span></span>
* <span data-ttu-id="1279e-353">A virtuális gép házirendek, tekintse meg a [Linux virtuális gépek erőforrás-szabályzatok alkalmazása](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) és [Windows virtuális gépek erőforrás-szabályzatok alkalmazása](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="1279e-353">For examples of virtual machine policies, see [Apply resource policies to Linux VMs](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) and [Apply resource policies to Windows VMs](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span></span>


## <a name="next-steps"></a><span data-ttu-id="1279e-354">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1279e-354">Next steps</span></span>
* <span data-ttu-id="1279e-355">Házirend szabály megadása után rendelje hozzá hatókör.</span><span class="sxs-lookup"><span data-stu-id="1279e-355">After defining a policy rule, assign it to a scope.</span></span> <span data-ttu-id="1279e-356">A portálon keresztül házirendek rendeléséhez lásd: [hozzárendelésére és kezelésére erőforrás-házirendek használata Azure-portálon](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1279e-356">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="1279e-357">REST API-t, a PowerShell vagy az Azure CLI-házirendeket rendeléséhez lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="1279e-357">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="1279e-358">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="1279e-358">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="1279e-359">A házirend séma közzé van téve a [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="1279e-359">The policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 


---
title: "erőforrás-házirendek aaaAzure |} Microsoft Docs"
description: "Ismerteti, üzembe helyezése során toouse Azure Resource Manager házirendek tooensure egységes erőforrás-tulajdonságok beállításának módját. Szabályzatok alkalmazhatók a hello előfizetés vagy az erőforrás-csoportok."
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
ms.openlocfilehash: f1b0bbb5f838f6bb70721e1040ad3eac2d881cea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-policy-overview"></a><span data-ttu-id="8bb57-104">Erőforrás-házirendek – áttekintés</span><span class="sxs-lookup"><span data-stu-id="8bb57-104">Resource policy overview</span></span>
<span data-ttu-id="8bb57-105">Erőforrás-házirendek lehetővé teszik a szervezet erőforrásaihoz tooestablish szabályai.</span><span class="sxs-lookup"><span data-stu-id="8bb57-105">Resource policies enable you tooestablish conventions for resources in your organization.</span></span> <span data-ttu-id="8bb57-106">Egyezmények meghatározásával szabályozhatja költségeit, és több könnyen kezelheti az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8bb57-106">By defining conventions, you can control costs and more easily manage your resources.</span></span> <span data-ttu-id="8bb57-107">Megadhatja például, hogy engedélyezve legyenek-e a virtuális gépek csak bizonyos típusú.</span><span class="sxs-lookup"><span data-stu-id="8bb57-107">For example, you can specify that only certain types of virtual machines are allowed.</span></span> <span data-ttu-id="8bb57-108">Vagy megkövetelheti, hogy minden erőforrás egy bizonyos címkével rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8bb57-108">Or, you can require that all resources have a particular tag.</span></span> <span data-ttu-id="8bb57-109">Összes gyermek-erőforrás által örökölt házirendek.</span><span class="sxs-lookup"><span data-stu-id="8bb57-109">Policies are inherited by all child resources.</span></span> <span data-ttu-id="8bb57-110">Ezért, ha a házirend alkalmazott tooa erőforráscsoportban, ezt a megfelelő tooall hello erőforrások az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="8bb57-110">So, if a policy is applied tooa resource group, it is applicable tooall hello resources in that resource group.</span></span>

<span data-ttu-id="8bb57-111">Nincsenek két fogalmak toounderstand szabályzataival kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="8bb57-111">There are two concepts toounderstand about policies:</span></span>

* <span data-ttu-id="8bb57-112">házirend-definíció - ismerteti hello szabályzat érvényes mikor és milyen művelet tootake</span><span class="sxs-lookup"><span data-stu-id="8bb57-112">policy definition - you describe when hello policy is enforced and what action tootake</span></span>
* <span data-ttu-id="8bb57-113">házirend-hozzárendelés - alkalmazása hello házirend definition tooa hatókör (előfizetéshez vagy erőforráscsoporthoz)</span><span class="sxs-lookup"><span data-stu-id="8bb57-113">policy assignment - you apply hello policy definition tooa scope (subscription or resource group)</span></span>

<span data-ttu-id="8bb57-114">Ez a témakör ismerteti a házirend-definíció.</span><span class="sxs-lookup"><span data-stu-id="8bb57-114">This topic focuses on policy definition.</span></span> <span data-ttu-id="8bb57-115">További információ a házirend-hozzárendelést: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md) vagy [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-115">For information about policy assignment, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md) or [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>

<span data-ttu-id="8bb57-116">Házirendek létrehozása, és az erőforrások (PUT és műveletek javítás) frissítésekor kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="8bb57-116">Policies are evaluated when creating and updating resources (PUT and PATCH operations).</span></span>

> [!NOTE]
> <span data-ttu-id="8bb57-117">Házirend jelenleg nem értékelhetők erőforrástípusok esetében, amelyek nem támogatják a címkék, típusa és helyre, például hello Microsoft.Resources/deployments erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="8bb57-117">Currently, policy does not evaluate resource types that do not support tags, kind, and location, such as hello Microsoft.Resources/deployments resource type.</span></span> <span data-ttu-id="8bb57-118">Ez a támogatás a későbbiekben lesz hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="8bb57-118">This support will be added at a future time.</span></span> <span data-ttu-id="8bb57-119">tooavoid visszafelé kompatibilitási problémák, explicit módon adjon meg típusú házirendek runboookok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="8bb57-119">tooavoid backward compatibility issues, you should explicitly specify type when authoring policies.</span></span> <span data-ttu-id="8bb57-120">Például egy címke házirend, amely nem határoz meg típusok minden alkalmazástípus esetében érvényes.</span><span class="sxs-lookup"><span data-stu-id="8bb57-120">For example, a tag policy that does not specify types is applied for all types.</span></span> <span data-ttu-id="8bb57-121">Ebben az esetben egy sablon telepítés sikertelen lehet, ha egy beágyazott erőforrást, amely nem támogatja a címkék, és hello telepítési erőforrástípus toopolicy értékelési hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="8bb57-121">In that case, a template deployment may fail if there is a nested resource that doesn't support tags, and hello deployment resource type has been added toopolicy evaluation.</span></span> 
> 
> 

## <a name="how-is-it-different-from-rbac"></a><span data-ttu-id="8bb57-122">Miben különbözik az RBAC?</span><span class="sxs-lookup"><span data-stu-id="8bb57-122">How is it different from RBAC?</span></span>
<span data-ttu-id="8bb57-123">Házirend és a szerepköralapú hozzáférés-vezérlést (RBAC) közötti néhány fontos különbség van.</span><span class="sxs-lookup"><span data-stu-id="8bb57-123">There are a few key differences between policy and role-based access control (RBAC).</span></span> <span data-ttu-id="8bb57-124">Az RBAC-ra összpontosít **felhasználói** különböző hatóköröket műveletek.</span><span class="sxs-lookup"><span data-stu-id="8bb57-124">RBAC focuses on **user** actions at different scopes.</span></span> <span data-ttu-id="8bb57-125">Például akkor kerülnek toohello közreműködő szerepkört az egy erőforráscsoport szükséges hello hatókörben, hogy módosításokat toothat erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="8bb57-125">For example, you are added toohello contributor role for a resource group at hello desired scope, so you can make changes toothat resource group.</span></span> <span data-ttu-id="8bb57-126">Házirend összpontosít **erőforrás** tulajdonságok üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="8bb57-126">Policy focuses on **resource** properties during deployment.</span></span> <span data-ttu-id="8bb57-127">Például szabályzatokkal, szabályozhatja hello típusú erőforrások, amelyek létesíthetők.</span><span class="sxs-lookup"><span data-stu-id="8bb57-127">For example, through policies, you can control hello types of resources that can be provisioned.</span></span> <span data-ttu-id="8bb57-128">Vagy létesíthetők hello erőforrások hello helyek korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-128">Or, you can restrict hello locations in which hello resources can be provisioned.</span></span> <span data-ttu-id="8bb57-129">Szerepalapú, eltérően házirend egy alapértelmezett engedélyezése és explicit megtagadja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="8bb57-129">Unlike RBAC, policy is a default allow and explicit deny system.</span></span> 

<span data-ttu-id="8bb57-130">toouse házirendek, akkor RBAC lehet hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="8bb57-130">toouse policies, you must be authenticated through RBAC.</span></span> <span data-ttu-id="8bb57-131">Pontosabban, a fiókot kell-e a:</span><span class="sxs-lookup"><span data-stu-id="8bb57-131">Specifically, your account needs the:</span></span>

* <span data-ttu-id="8bb57-132">`Microsoft.Authorization/policydefinitions/write`engedély toodefine házirend</span><span class="sxs-lookup"><span data-stu-id="8bb57-132">`Microsoft.Authorization/policydefinitions/write` permission toodefine a policy</span></span>
* <span data-ttu-id="8bb57-133">`Microsoft.Authorization/policyassignments/write`engedély tooassign házirend</span><span class="sxs-lookup"><span data-stu-id="8bb57-133">`Microsoft.Authorization/policyassignments/write` permission tooassign a policy</span></span> 

<span data-ttu-id="8bb57-134">Ezek az engedélyek nem szerepelnek a hello **közreműködő** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="8bb57-134">These permissions are not included in hello **Contributor** role.</span></span>

## <a name="built-in-policies"></a><span data-ttu-id="8bb57-135">Beépített házirendek</span><span class="sxs-lookup"><span data-stu-id="8bb57-135">Built-in policies</span></span>

<span data-ttu-id="8bb57-136">Azure biztosít néhány beépített házirend-definíciókat tartalmazzon, hello számát csökkentheti a házirendek toodefine rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8bb57-136">Azure provides some built-in policy definitions that may reduce hello number of policies you have toodefine.</span></span> <span data-ttu-id="8bb57-137">Házirend-definíciók a folytatás előtt érdemes lehet akár egy beépített házirend már használatával hello definíciója van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8bb57-137">Before proceeding with policy definitions, you should consider whether a built-in policy already provides hello definition you need.</span></span> <span data-ttu-id="8bb57-138">hello beépített házirend-definíciók száma a következők:</span><span class="sxs-lookup"><span data-stu-id="8bb57-138">hello built-in policy definitions are:</span></span>

* <span data-ttu-id="8bb57-139">Engedélyezett helyek</span><span class="sxs-lookup"><span data-stu-id="8bb57-139">Allowed locations</span></span>
* <span data-ttu-id="8bb57-140">Engedélyezett erőforrástípusok</span><span class="sxs-lookup"><span data-stu-id="8bb57-140">Allowed resource types</span></span>
* <span data-ttu-id="8bb57-141">A tárfiók termékváltozatok engedélyezett</span><span class="sxs-lookup"><span data-stu-id="8bb57-141">Allowed storage account SKUs</span></span>
* <span data-ttu-id="8bb57-142">Virtuális gép termékváltozatok engedélyezett</span><span class="sxs-lookup"><span data-stu-id="8bb57-142">Allowed virtual machine SKUs</span></span>
* <span data-ttu-id="8bb57-143">Címke és az alapértelmezett érték alkalmazása</span><span class="sxs-lookup"><span data-stu-id="8bb57-143">Apply tag and default value</span></span>
* <span data-ttu-id="8bb57-144">Címke és érték</span><span class="sxs-lookup"><span data-stu-id="8bb57-144">Enforce tag and value</span></span>
* <span data-ttu-id="8bb57-145">Nem engedélyezett típusú erőforrások</span><span class="sxs-lookup"><span data-stu-id="8bb57-145">Not allowed resource types</span></span>
* <span data-ttu-id="8bb57-146">SQL Server verziója 12.0 megkövetelése</span><span class="sxs-lookup"><span data-stu-id="8bb57-146">Require SQL Server version 12.0</span></span>
* <span data-ttu-id="8bb57-147">Tárolási fiók titkosításának kötelezővé tétele</span><span class="sxs-lookup"><span data-stu-id="8bb57-147">Require storage account encryption</span></span>

<span data-ttu-id="8bb57-148">Említett házirendjeinek hello keresztül rendelhet [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), vagy [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8bb57-148">You can assign any of these policies through hello [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), or [Azure CLI](resource-manager-policy-create-assign.md#azure-cli).</span></span>

## <a name="policy-definition-structure"></a><span data-ttu-id="8bb57-149">Házirend szerkezete</span><span class="sxs-lookup"><span data-stu-id="8bb57-149">Policy definition structure</span></span>
<span data-ttu-id="8bb57-150">Házirend-definíció JSON toocreate használhatja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-150">You use JSON toocreate a policy definition.</span></span> <span data-ttu-id="8bb57-151">hello házirend-definíció a elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8bb57-151">hello policy definition contains elements for:</span></span>

* <span data-ttu-id="8bb57-152">paraméterek</span><span class="sxs-lookup"><span data-stu-id="8bb57-152">parameters</span></span>
* <span data-ttu-id="8bb57-153">Megjelenített név</span><span class="sxs-lookup"><span data-stu-id="8bb57-153">display name</span></span>
* <span data-ttu-id="8bb57-154">leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-154">description</span></span>
* <span data-ttu-id="8bb57-155">Házirend szabályai</span><span class="sxs-lookup"><span data-stu-id="8bb57-155">policy rule</span></span>
  * <span data-ttu-id="8bb57-156">logikai kiértékelése</span><span class="sxs-lookup"><span data-stu-id="8bb57-156">logical evaluation</span></span>
  * <span data-ttu-id="8bb57-157">hatása</span><span class="sxs-lookup"><span data-stu-id="8bb57-157">effect</span></span>

<span data-ttu-id="8bb57-158">a következő példa hello egy házirendet, amely korlátozza, amelyekre központilag telepítették erőforrások láthatók:</span><span class="sxs-lookup"><span data-stu-id="8bb57-158">hello following example shows a policy that limits where resources are deployed:</span></span>

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
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

## <a name="parameters"></a><span data-ttu-id="8bb57-159">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="8bb57-159">Parameters</span></span>
<span data-ttu-id="8bb57-160">Paraméterek használatával egyszerűbbé teszi a Csoportházirend kezelése házirend-definíciók hello számának csökkentésével.</span><span class="sxs-lookup"><span data-stu-id="8bb57-160">Using parameters helps simplify your policy management by reducing hello number of policy definitions.</span></span> <span data-ttu-id="8bb57-161">Adja meg a házirendet (például korlátozza a hello helyeken, ahol erőforrásokat is telepíthető) erőforrás-tulajdonságot, és paraméterek közé tartoznak a hello definícióban.</span><span class="sxs-lookup"><span data-stu-id="8bb57-161">You define a policy for a resource property (such as limiting hello locations where resources can be deployed), and include parameters in hello definition.</span></span> <span data-ttu-id="8bb57-162">Ezt követően többször használ, hogy a házirend-definíció különböző helyzetek kezelésére különböző értékeket (például megadják az előfizetéshez tartozó helyek egy set) történő amikor hello házirend hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="8bb57-162">Then, you reuse that policy definition for different scenarios by passing in different values (such as specifying one set of locations for a subscription) when assigning hello policy.</span></span>

<span data-ttu-id="8bb57-163">Ha a házirend-meghatározások létrehozása deklarálhatja paraméterek.</span><span class="sxs-lookup"><span data-stu-id="8bb57-163">You declare parameters when you create policy definitions.</span></span>

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "hello list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

<span data-ttu-id="8bb57-164">egy paraméter hello típusú karakterlánc vagy tömb lehet.</span><span class="sxs-lookup"><span data-stu-id="8bb57-164">hello type of a parameter can be either string or array.</span></span> <span data-ttu-id="8bb57-165">például az Azure portál toodisplay felhasználóbarát információk hello metaadat-tulajdonságnak szolgál.</span><span class="sxs-lookup"><span data-stu-id="8bb57-165">hello metadata property is used for tools like Azure portal toodisplay user-friendly information.</span></span> 

<span data-ttu-id="8bb57-166">Hello házirendszabály, a paraméterek a hello szintaxisa a következő hivatkozik:</span><span class="sxs-lookup"><span data-stu-id="8bb57-166">In hello policy rule, you reference parameters with hello following syntax:</span></span> 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a><span data-ttu-id="8bb57-167">Megjelenített név és leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-167">Display name and description</span></span>

<span data-ttu-id="8bb57-168">Hello használata **displayName** és **leírás** tooidentify hello házirend-definíció, és adja meg a környezetben használat esetén.</span><span class="sxs-lookup"><span data-stu-id="8bb57-168">You use hello **displayName** and **description** tooidentify hello policy definition, and provide context for when it is used.</span></span>

## <a name="policy-rule"></a><span data-ttu-id="8bb57-169">Házirend szabályai</span><span class="sxs-lookup"><span data-stu-id="8bb57-169">Policy rule</span></span>

<span data-ttu-id="8bb57-170">hello házirendszabály áll **Ha** és **majd** blokkolja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-170">hello policy rule consists of **If** and **Then** blocks.</span></span> <span data-ttu-id="8bb57-171">A hello **Ha** blokk, megadhatja, hogy egy vagy több feltételt, adja meg, amikor a hello házirend van érvényben.</span><span class="sxs-lookup"><span data-stu-id="8bb57-171">In hello **If** block, you define one or more conditions that specify when hello policy is enforced.</span></span> <span data-ttu-id="8bb57-172">Logikai operátorok toothese feltételek alkalmazása tooprecisely hello forgatókönyv egy házirend határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8bb57-172">You can apply logical operators toothese conditions tooprecisely define hello scenario for a policy.</span></span> <span data-ttu-id="8bb57-173">A hello **majd** blokk, megadhatja, hogy történik, ha hello hello hatással **Ha** feltételek teljesülnek.</span><span class="sxs-lookup"><span data-stu-id="8bb57-173">In hello **Then** block, you define hello effect that happens when hello **If** conditions are fulfilled.</span></span>

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

### <a name="logical-operators"></a><span data-ttu-id="8bb57-174">Logikai operátorok</span><span class="sxs-lookup"><span data-stu-id="8bb57-174">Logical operators</span></span>
<span data-ttu-id="8bb57-175">hello támogatott logikai operátorok a következők:</span><span class="sxs-lookup"><span data-stu-id="8bb57-175">hello supported logical operators are:</span></span>

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

<span data-ttu-id="8bb57-176">Hello **nem** szintaxis invertálja hello feltétel hello eredményét.</span><span class="sxs-lookup"><span data-stu-id="8bb57-176">hello **not** syntax inverts hello result of hello condition.</span></span> <span data-ttu-id="8bb57-177">Hello **allOf** szintaxis (logikai hasonló toohello **és** művelet) igényel minden feltételek toobe teljesül.</span><span class="sxs-lookup"><span data-stu-id="8bb57-177">hello **allOf** syntax (similar toohello logical **And** operation) requires all conditions toobe true.</span></span> <span data-ttu-id="8bb57-178">Hello **anyOf** szintaxis (logikai hasonló toohello **vagy** művelet) egy vagy több feltételek toobe true igényel.</span><span class="sxs-lookup"><span data-stu-id="8bb57-178">hello **anyOf** syntax (similar toohello logical **Or** operation) requires one or more conditions toobe true.</span></span>

<span data-ttu-id="8bb57-179">Logikai operátorok ágyazhatja be.</span><span class="sxs-lookup"><span data-stu-id="8bb57-179">You can nest logical operators.</span></span> <span data-ttu-id="8bb57-180">a következő példa azt mutatja meg hello egy **nem** művelet, amelynek van beágyazva egy **allOf** műveletet.</span><span class="sxs-lookup"><span data-stu-id="8bb57-180">hello following example shows a **not** operation that is nested within an **allOf** operation.</span></span> 

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

### <a name="conditions"></a><span data-ttu-id="8bb57-181">Feltételek</span><span class="sxs-lookup"><span data-stu-id="8bb57-181">Conditions</span></span>
<span data-ttu-id="8bb57-182">hello feltétel e egy **mező** meghatározott feltételeknek eleget.</span><span class="sxs-lookup"><span data-stu-id="8bb57-182">hello condition evaluates whether a **field** meets certain criteria.</span></span> <span data-ttu-id="8bb57-183">hello támogatott feltételek esetén:</span><span class="sxs-lookup"><span data-stu-id="8bb57-183">hello supported conditions are:</span></span>

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

<span data-ttu-id="8bb57-184">Hello használatakor **például** feltétel, megadhatja a helyettesítő karakter (*) hello érték.</span><span class="sxs-lookup"><span data-stu-id="8bb57-184">When using hello **like** condition, you can provide a wildcard (*) in hello value.</span></span>

<span data-ttu-id="8bb57-185">Hello használata esetén **megfelelő** feltétel, adja meg `#` toorepresent számjegy, `?` a betű, és bármilyen más karakter toorepresent Ez a tényleges karakter.</span><span class="sxs-lookup"><span data-stu-id="8bb57-185">When using hello **match** condition, provide `#` toorepresent a digit, `?` for a letter, and any other character toorepresent that actual character.</span></span> <span data-ttu-id="8bb57-186">Tekintse meg a [nevét és az erőforrás-szabályzatok alkalmazása](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-186">For examples, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>

### <a name="fields"></a><span data-ttu-id="8bb57-187">Mezők</span><span class="sxs-lookup"><span data-stu-id="8bb57-187">Fields</span></span>
<span data-ttu-id="8bb57-188">Feltételek mezőkkel jöttek létre.</span><span class="sxs-lookup"><span data-stu-id="8bb57-188">Conditions are formed by using fields.</span></span> <span data-ttu-id="8bb57-189">A mező képviseli tulajdonságokat az erőforrás-kérések forgalma hello hello erőforrás, amely használt toodescribe hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="8bb57-189">A field represents properties in hello resource request payload that is used toodescribe hello state of hello resource.</span></span>  

<span data-ttu-id="8bb57-190">a következő mezők hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="8bb57-190">hello following fields are supported:</span></span>

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* <span data-ttu-id="8bb57-191">tulajdonságának aliasokat - listájáért lásd: [aliasok](#aliases).</span><span class="sxs-lookup"><span data-stu-id="8bb57-191">property aliases - for a list, see [Aliases](#aliases).</span></span>

### <a name="effect"></a><span data-ttu-id="8bb57-192">Következmény</span><span class="sxs-lookup"><span data-stu-id="8bb57-192">Effect</span></span>
<span data-ttu-id="8bb57-193">Házirend érvénybe - három típusú támogat `deny`, `audit`, és `append`.</span><span class="sxs-lookup"><span data-stu-id="8bb57-193">Policy supports three types of effect - `deny`, `audit`, and `append`.</span></span> 

* <span data-ttu-id="8bb57-194">**Megtagadási** hello naplóban generál egy eseményt, és nem sikerül hello kérelem</span><span class="sxs-lookup"><span data-stu-id="8bb57-194">**Deny** generates an event in hello audit log and fails hello request</span></span>
* <span data-ttu-id="8bb57-195">**Naplózási** naplót hoz létre a figyelmeztető üzenet, de nem sikertelen hello kérelem</span><span class="sxs-lookup"><span data-stu-id="8bb57-195">**Audit** generates a warning event in audit log but does not fail hello request</span></span>
* <span data-ttu-id="8bb57-196">**Hozzáfűzendő** mezők toohello kérelem hello meghatározott készletét hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8bb57-196">**Append** adds hello defined set of fields toohello request</span></span> 

<span data-ttu-id="8bb57-197">A **hozzáfűzése**, meg kell adnia a következő adatok hello:</span><span class="sxs-lookup"><span data-stu-id="8bb57-197">For **append**, you must provide hello following details:</span></span>

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of hello field"
  }
]
```

<span data-ttu-id="8bb57-198">hello érték lehet egy karakterlánc vagy egy JSON-formátumú objektum.</span><span class="sxs-lookup"><span data-stu-id="8bb57-198">hello value can be either a string or a JSON format object.</span></span> 

## <a name="aliases"></a><span data-ttu-id="8bb57-199">Aliasok</span><span class="sxs-lookup"><span data-stu-id="8bb57-199">Aliases</span></span>

<span data-ttu-id="8bb57-200">Az erőforrástípus tulajdonságának aliasokat tooaccess tulajdonságokat használhatja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-200">You use property aliases tooaccess specific properties for a resource type.</span></span> <span data-ttu-id="8bb57-201">Aliasok toorestrict milyen értékeket, vagy a feltételek engedélyezve van a erőforrás-tulajdonságok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8bb57-201">Aliases enable you toorestrict what values or conditions are permitted for a property on a resource.</span></span> <span data-ttu-id="8bb57-202">Mindegyik aliasnak maps toopaths egy adott erőforrástípusra különböző API-verziókban.</span><span class="sxs-lookup"><span data-stu-id="8bb57-202">Each alias maps toopaths in different API versions for a given resource type.</span></span> <span data-ttu-id="8bb57-203">Házirend kiértékelése közben hello házirendmotor kap, az API-verzióhoz tartozó hello útvonal.</span><span class="sxs-lookup"><span data-stu-id="8bb57-203">During policy evaluation, hello policy engine gets hello property path for that API version.</span></span>

<span data-ttu-id="8bb57-204">**Microsoft.Cache/Redis**</span><span class="sxs-lookup"><span data-stu-id="8bb57-204">**Microsoft.Cache/Redis**</span></span>

| <span data-ttu-id="8bb57-205">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-205">Alias</span></span> | <span data-ttu-id="8bb57-206">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-206">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-207">Microsoft.Cache/Redis/enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="8bb57-207">Microsoft.Cache/Redis/enableNonSslPort</span></span> | <span data-ttu-id="8bb57-208">Állítsa be e hello Redis-kiszolgáló-ssl port (6379) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="8bb57-208">Set whether hello non-ssl Redis server port (6379) is enabled.</span></span> |
| <span data-ttu-id="8bb57-209">Microsoft.Cache/Redis/shardCount</span><span class="sxs-lookup"><span data-stu-id="8bb57-209">Microsoft.Cache/Redis/shardCount</span></span> | <span data-ttu-id="8bb57-210">Prémium szintű fürt gyorsítótárat létrehozni szilánkok toobe hello számának megadása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-210">Set hello number of shards toobe created on a Premium Cluster Cache.</span></span>  |
| <span data-ttu-id="8bb57-211">Microsoft.Cache/Redis/sku.capacity</span><span class="sxs-lookup"><span data-stu-id="8bb57-211">Microsoft.Cache/Redis/sku.capacity</span></span> | <span data-ttu-id="8bb57-212">Hello Redis gyorsítótár toodeploy hello méret megadása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-212">Set hello size of hello Redis cache toodeploy.</span></span>  |
| <span data-ttu-id="8bb57-213">Microsoft.Cache/Redis/sku.family</span><span class="sxs-lookup"><span data-stu-id="8bb57-213">Microsoft.Cache/Redis/sku.family</span></span> | <span data-ttu-id="8bb57-214">Hello SKU-család toouse beállítása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-214">Set hello SKU family toouse.</span></span> |
| <span data-ttu-id="8bb57-215">Microsoft.Cache/Redis/sku.name</span><span class="sxs-lookup"><span data-stu-id="8bb57-215">Microsoft.Cache/Redis/sku.name</span></span> | <span data-ttu-id="8bb57-216">Állítsa be a Redis Cache toodeploy hello típusú.</span><span class="sxs-lookup"><span data-stu-id="8bb57-216">Set hello type of Redis Cache toodeploy.</span></span> |

<span data-ttu-id="8bb57-217">**Microsoft.Cdn/profiles**</span><span class="sxs-lookup"><span data-stu-id="8bb57-217">**Microsoft.Cdn/profiles**</span></span>

| <span data-ttu-id="8bb57-218">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-218">Alias</span></span> | <span data-ttu-id="8bb57-219">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-219">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-220">Microsoft.CDN/profiles/sku.name</span><span class="sxs-lookup"><span data-stu-id="8bb57-220">Microsoft.CDN/profiles/sku.name</span></span> | <span data-ttu-id="8bb57-221">IP-címek hello hello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-221">Set hello name of hello pricing tier.</span></span> |

<span data-ttu-id="8bb57-222">**Microsoft.Compute/disks**</span><span class="sxs-lookup"><span data-stu-id="8bb57-222">**Microsoft.Compute/disks**</span></span>

| <span data-ttu-id="8bb57-223">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-223">Alias</span></span> | <span data-ttu-id="8bb57-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-224">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-225">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="8bb57-225">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="8bb57-226">Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-226">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-227">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="8bb57-227">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="8bb57-228">Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-228">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-229">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="8bb57-229">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="8bb57-230">Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-230">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-231">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="8bb57-231">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="8bb57-232">Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-232">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |


<span data-ttu-id="8bb57-233">**Microsoft.Compute/virtualMachines**</span><span class="sxs-lookup"><span data-stu-id="8bb57-233">**Microsoft.Compute/virtualMachines**</span></span>

| <span data-ttu-id="8bb57-234">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-234">Alias</span></span> | <span data-ttu-id="8bb57-235">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-235">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-236">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="8bb57-236">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="8bb57-237">Hello hello használt kép toocreate hello virtuális gép azonosítójának megadása</span><span class="sxs-lookup"><span data-stu-id="8bb57-237">Set hello identifier of hello image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-238">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="8bb57-238">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="8bb57-239">Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-239">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-240">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="8bb57-240">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="8bb57-241">Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-241">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-242">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="8bb57-242">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="8bb57-243">Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-243">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-244">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="8bb57-244">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="8bb57-245">Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-245">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-246">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="8bb57-246">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="8bb57-247">Állítsa be, hogy hello lemezkép vagy lemez-e a licencelt helyszíni.</span><span class="sxs-lookup"><span data-stu-id="8bb57-247">Set that hello image or disk is licensed on-premises.</span></span> <span data-ttu-id="8bb57-248">Ez az érték csak a hello Windows Server operációs rendszert tartalmazó lemezképek használatos.</span><span class="sxs-lookup"><span data-stu-id="8bb57-248">This value is only used for images that contain hello Windows Server operating system.</span></span>  |
| <span data-ttu-id="8bb57-249">Microsoft.Compute/virtualMachines/imageOffer</span><span class="sxs-lookup"><span data-stu-id="8bb57-249">Microsoft.Compute/virtualMachines/imageOffer</span></span> | <span data-ttu-id="8bb57-250">Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-250">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-251">Microsoft.Compute/virtualMachines/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="8bb57-251">Microsoft.Compute/virtualMachines/imagePublisher</span></span> | <span data-ttu-id="8bb57-252">Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-252">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-253">Microsoft.Compute/virtualMachines/imageSku</span><span class="sxs-lookup"><span data-stu-id="8bb57-253">Microsoft.Compute/virtualMachines/imageSku</span></span> | <span data-ttu-id="8bb57-254">Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-254">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-255">Microsoft.Compute/virtualMachines/imageVersion</span><span class="sxs-lookup"><span data-stu-id="8bb57-255">Microsoft.Compute/virtualMachines/imageVersion</span></span> | <span data-ttu-id="8bb57-256">Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-256">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span><span class="sxs-lookup"><span data-stu-id="8bb57-257">Microsoft.Compute/virtualMachines/osDisk.Uri</span></span> | <span data-ttu-id="8bb57-258">Állítsa be a hello vhd URI.</span><span class="sxs-lookup"><span data-stu-id="8bb57-258">Set hello vhd URI.</span></span> |
| <span data-ttu-id="8bb57-259">Microsoft.Compute/virtualMachines/sku.name</span><span class="sxs-lookup"><span data-stu-id="8bb57-259">Microsoft.Compute/virtualMachines/sku.name</span></span> | <span data-ttu-id="8bb57-260">Virtuális gép hello hello méretének beállítása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-260">Set hello size of hello virtual machine.</span></span> |

<span data-ttu-id="8bb57-261">**Microsoft.Compute/virtualMachines/extensions**</span><span class="sxs-lookup"><span data-stu-id="8bb57-261">**Microsoft.Compute/virtualMachines/extensions**</span></span>

| <span data-ttu-id="8bb57-262">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-262">Alias</span></span> | <span data-ttu-id="8bb57-263">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-263">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-264">Microsoft.Compute/virtualMachines/extensions/publisher</span><span class="sxs-lookup"><span data-stu-id="8bb57-264">Microsoft.Compute/virtualMachines/extensions/publisher</span></span> | <span data-ttu-id="8bb57-265">Hello bővítmény publisher hello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-265">Set hello name of hello extension’s publisher.</span></span> |
| <span data-ttu-id="8bb57-266">Microsoft.Compute/virtualMachines/extensions/type</span><span class="sxs-lookup"><span data-stu-id="8bb57-266">Microsoft.Compute/virtualMachines/extensions/type</span></span> | <span data-ttu-id="8bb57-267">Állítsa be a bővítmény hello típusát.</span><span class="sxs-lookup"><span data-stu-id="8bb57-267">Set hello type of extension.</span></span> |
| <span data-ttu-id="8bb57-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span><span class="sxs-lookup"><span data-stu-id="8bb57-268">Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion</span></span> | <span data-ttu-id="8bb57-269">Állítsa be a hello bővítmény hello verziója.</span><span class="sxs-lookup"><span data-stu-id="8bb57-269">Set hello version of hello extension.</span></span> |

<span data-ttu-id="8bb57-270">**Microsoft.Compute/virtualMachineScaleSets**</span><span class="sxs-lookup"><span data-stu-id="8bb57-270">**Microsoft.Compute/virtualMachineScaleSets**</span></span>

| <span data-ttu-id="8bb57-271">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-271">Alias</span></span> | <span data-ttu-id="8bb57-272">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-272">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-273">Microsoft.Compute/imageId</span><span class="sxs-lookup"><span data-stu-id="8bb57-273">Microsoft.Compute/imageId</span></span> | <span data-ttu-id="8bb57-274">Hello hello használt kép toocreate hello virtuális gép azonosítójának megadása</span><span class="sxs-lookup"><span data-stu-id="8bb57-274">Set hello identifier of hello image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-275">Microsoft.Compute/imageOffer</span><span class="sxs-lookup"><span data-stu-id="8bb57-275">Microsoft.Compute/imageOffer</span></span> | <span data-ttu-id="8bb57-276">Set hello ajánlat hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-276">Set hello offer of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-277">Microsoft.Compute/imagePublisher</span><span class="sxs-lookup"><span data-stu-id="8bb57-277">Microsoft.Compute/imagePublisher</span></span> | <span data-ttu-id="8bb57-278">Set hello publisher hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-278">Set hello publisher of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-279">Microsoft.Compute/imageSku</span><span class="sxs-lookup"><span data-stu-id="8bb57-279">Microsoft.Compute/imageSku</span></span> | <span data-ttu-id="8bb57-280">Set hello hello platformlemezképet vagy Piactéri lemezképhez Termékváltozata toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-280">Set hello SKU of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-281">Microsoft.Compute/imageVersion</span><span class="sxs-lookup"><span data-stu-id="8bb57-281">Microsoft.Compute/imageVersion</span></span> | <span data-ttu-id="8bb57-282">Hello verzió beállítása hello platformlemezképet vagy Piactéri lemezképhez toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="8bb57-282">Set hello version of hello platform image or marketplace image used toocreate hello virtual machine.</span></span> |
| <span data-ttu-id="8bb57-283">Microsoft.Compute/licenseType</span><span class="sxs-lookup"><span data-stu-id="8bb57-283">Microsoft.Compute/licenseType</span></span> | <span data-ttu-id="8bb57-284">Állítsa be, hogy hello lemezkép vagy lemez-e a licencelt helyszíni.</span><span class="sxs-lookup"><span data-stu-id="8bb57-284">Set that hello image or disk is licensed on-premises.</span></span> <span data-ttu-id="8bb57-285">Ez az érték csak a hello Windows Server operációs rendszert tartalmazó lemezképek használatos.</span><span class="sxs-lookup"><span data-stu-id="8bb57-285">This value is only used for images that contain hello Windows Server operating system.</span></span> |
| <span data-ttu-id="8bb57-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="8bb57-286">Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix</span></span> | <span data-ttu-id="8bb57-287">Hello méretezési csoportban lévő hello gépek nevének előtagja hello virtuális gép beállítása</span><span class="sxs-lookup"><span data-stu-id="8bb57-287">Set hello computer name prefix for all  hello virtual machines in hello scale set.</span></span> |
| <span data-ttu-id="8bb57-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span><span class="sxs-lookup"><span data-stu-id="8bb57-288">Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl</span></span> | <span data-ttu-id="8bb57-289">Állítsa be a hello blob URI felhasználói lemezkép.</span><span class="sxs-lookup"><span data-stu-id="8bb57-289">Set hello blob URI for user image.</span></span> |
| <span data-ttu-id="8bb57-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span><span class="sxs-lookup"><span data-stu-id="8bb57-290">Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers</span></span> | <span data-ttu-id="8bb57-291">Hello tároló URL-címeinek beállítása, amelyek használt toostore operációs rendszer lemezek hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="8bb57-291">Set hello container URLs that are used toostore operating system disks for hello scale set.</span></span> |
| <span data-ttu-id="8bb57-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span><span class="sxs-lookup"><span data-stu-id="8bb57-292">Microsoft.Compute/VirtualMachineScaleSets/sku.name</span></span> | <span data-ttu-id="8bb57-293">Méretezési csoportban lévő virtuális gépek méretét hello beállítása</span><span class="sxs-lookup"><span data-stu-id="8bb57-293">Set hello size of virtual machines in a scale set.</span></span> |
| <span data-ttu-id="8bb57-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span><span class="sxs-lookup"><span data-stu-id="8bb57-294">Microsoft.Compute/VirtualMachineScaleSets/sku.tier</span></span> | <span data-ttu-id="8bb57-295">Méretezési csoportban lévő virtuális gépek hello szintjének beállítása</span><span class="sxs-lookup"><span data-stu-id="8bb57-295">Set hello tier of virtual machines in a scale set.</span></span> |
  
<span data-ttu-id="8bb57-296">**Microsoft.Network/applicationGateways**</span><span class="sxs-lookup"><span data-stu-id="8bb57-296">**Microsoft.Network/applicationGateways**</span></span>

| <span data-ttu-id="8bb57-297">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-297">Alias</span></span> | <span data-ttu-id="8bb57-298">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-298">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-299">Microsoft.Network/applicationGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="8bb57-299">Microsoft.Network/applicationGateways/sku.name</span></span> | <span data-ttu-id="8bb57-300">Hello átjáró hello méret megadása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-300">Set hello size of hello gateway.</span></span> |

<span data-ttu-id="8bb57-301">**Microsoft.Network/virtualNetworkGateways**</span><span class="sxs-lookup"><span data-stu-id="8bb57-301">**Microsoft.Network/virtualNetworkGateways**</span></span>

| <span data-ttu-id="8bb57-302">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-302">Alias</span></span> | <span data-ttu-id="8bb57-303">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-303">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span><span class="sxs-lookup"><span data-stu-id="8bb57-304">Microsoft.Network/virtualNetworkGateways/gatewayType</span></span> | <span data-ttu-id="8bb57-305">Állítsa be a virtuális hálózati átjáró hello típusú.</span><span class="sxs-lookup"><span data-stu-id="8bb57-305">Set hello type of this virtual network gateway.</span></span> |
| <span data-ttu-id="8bb57-306">Microsoft.Network/virtualNetworkGateways/sku.name</span><span class="sxs-lookup"><span data-stu-id="8bb57-306">Microsoft.Network/virtualNetworkGateways/sku.name</span></span> | <span data-ttu-id="8bb57-307">Átjáró-termékváltozat hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-307">Set hello gateway SKU name.</span></span> |

<span data-ttu-id="8bb57-308">**Microsoft.Sql/servers**</span><span class="sxs-lookup"><span data-stu-id="8bb57-308">**Microsoft.Sql/servers**</span></span>

| <span data-ttu-id="8bb57-309">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-309">Alias</span></span> | <span data-ttu-id="8bb57-310">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-310">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-311">Microsoft.Sql/servers/version</span><span class="sxs-lookup"><span data-stu-id="8bb57-311">Microsoft.Sql/servers/version</span></span> | <span data-ttu-id="8bb57-312">Állítsa be a hello hello server verziója.</span><span class="sxs-lookup"><span data-stu-id="8bb57-312">Set hello version of hello server.</span></span> |

<span data-ttu-id="8bb57-313">**Microsoft.Sql/databases**</span><span class="sxs-lookup"><span data-stu-id="8bb57-313">**Microsoft.Sql/databases**</span></span>

| <span data-ttu-id="8bb57-314">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-314">Alias</span></span> | <span data-ttu-id="8bb57-315">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-315">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-316">Microsoft.Sql/servers/databases/edition</span><span class="sxs-lookup"><span data-stu-id="8bb57-316">Microsoft.Sql/servers/databases/edition</span></span> | <span data-ttu-id="8bb57-317">Állítsa be a hello adatbázis hello kiadásában.</span><span class="sxs-lookup"><span data-stu-id="8bb57-317">Set hello edition of hello database.</span></span> |
| <span data-ttu-id="8bb57-318">Microsoft.Sql/servers/databases/elasticPoolName</span><span class="sxs-lookup"><span data-stu-id="8bb57-318">Microsoft.Sql/servers/databases/elasticPoolName</span></span> | <span data-ttu-id="8bb57-319">Hello rugalmas készlet hello adatbázis hello neve van.</span><span class="sxs-lookup"><span data-stu-id="8bb57-319">Set hello name of hello elastic pool hello database is in.</span></span> |
| <span data-ttu-id="8bb57-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span><span class="sxs-lookup"><span data-stu-id="8bb57-320">Microsoft.Sql/servers/databases/requestedServiceObjectiveId</span></span> | <span data-ttu-id="8bb57-321">Hello beállítása a szolgáltatási szint célkitűzésének azonosítója hello adatbázis konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="8bb57-321">Set hello configured service level objective ID of hello database.</span></span> |
| <span data-ttu-id="8bb57-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span><span class="sxs-lookup"><span data-stu-id="8bb57-322">Microsoft.Sql/servers/databases/requestedServiceObjectiveName</span></span> | <span data-ttu-id="8bb57-323">Hello konfigurált szolgáltatási szint célkitűzésének hello adatbázis hello nevének beállítása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-323">Set hello name of hello configured service level objective of hello database.</span></span>  |

<span data-ttu-id="8bb57-324">**Microsoft.Sql/elasticpools**</span><span class="sxs-lookup"><span data-stu-id="8bb57-324">**Microsoft.Sql/elasticpools**</span></span>

| <span data-ttu-id="8bb57-325">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-325">Alias</span></span> | <span data-ttu-id="8bb57-326">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-326">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-327">kiszolgálók/elasticpools</span><span class="sxs-lookup"><span data-stu-id="8bb57-327">servers/elasticpools</span></span> | <span data-ttu-id="8bb57-328">Microsoft.Sql/servers/elasticPools/dtu</span><span class="sxs-lookup"><span data-stu-id="8bb57-328">Microsoft.Sql/servers/elasticPools/dtu</span></span> | <span data-ttu-id="8bb57-329">Set hello összesen hello database rugalmas készlet DTU megosztott.</span><span class="sxs-lookup"><span data-stu-id="8bb57-329">Set hello total shared DTU for hello database elastic pool.</span></span> |
| <span data-ttu-id="8bb57-330">kiszolgálók/elasticpools</span><span class="sxs-lookup"><span data-stu-id="8bb57-330">servers/elasticpools</span></span> | <span data-ttu-id="8bb57-331">Microsoft.Sql/servers/elasticPools/edition</span><span class="sxs-lookup"><span data-stu-id="8bb57-331">Microsoft.Sql/servers/elasticPools/edition</span></span> | <span data-ttu-id="8bb57-332">Állítsa be a rugalmas készlet hello hello kiadása.</span><span class="sxs-lookup"><span data-stu-id="8bb57-332">Set hello edition of hello elastic pool.</span></span> |

<span data-ttu-id="8bb57-333">**Microsoft.Storage/storageAccounts**</span><span class="sxs-lookup"><span data-stu-id="8bb57-333">**Microsoft.Storage/storageAccounts**</span></span>

| <span data-ttu-id="8bb57-334">Alias</span><span class="sxs-lookup"><span data-stu-id="8bb57-334">Alias</span></span> | <span data-ttu-id="8bb57-335">Leírás</span><span class="sxs-lookup"><span data-stu-id="8bb57-335">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="8bb57-336">Microsoft.Storage/storageAccounts/accessTier</span><span class="sxs-lookup"><span data-stu-id="8bb57-336">Microsoft.Storage/storageAccounts/accessTier</span></span> | <span data-ttu-id="8bb57-337">Állítsa a számlázási használt hello hozzáférési szintet.</span><span class="sxs-lookup"><span data-stu-id="8bb57-337">Set hello access tier used for billing.</span></span> |
| <span data-ttu-id="8bb57-338">Microsoft.Storage/storageAccounts/accountType</span><span class="sxs-lookup"><span data-stu-id="8bb57-338">Microsoft.Storage/storageAccounts/accountType</span></span> | <span data-ttu-id="8bb57-339">Állítsa be a hello termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="8bb57-339">Set hello SKU name.</span></span> |
| <span data-ttu-id="8bb57-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span><span class="sxs-lookup"><span data-stu-id="8bb57-340">Microsoft.Storage/storageAccounts/enableBlobEncryption</span></span> | <span data-ttu-id="8bb57-341">Állítsa be, hogy hello blob storage szolgáltatásban tárolt hello szolgáltatás titkosítja az hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="8bb57-341">Set whether hello service encrypts hello data as it is stored in hello blob storage service.</span></span> |
| <span data-ttu-id="8bb57-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span><span class="sxs-lookup"><span data-stu-id="8bb57-342">Microsoft.Storage/storageAccounts/enableFileEncryption</span></span> | <span data-ttu-id="8bb57-343">Állítsa be, hogy hello storage szolgáltatásban tárolt hello szolgáltatás titkosítja az hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="8bb57-343">Set whether hello service encrypts hello data as it is stored in hello file storage service.</span></span> |
| <span data-ttu-id="8bb57-344">Microsoft.Storage/storageAccounts/sku.name</span><span class="sxs-lookup"><span data-stu-id="8bb57-344">Microsoft.Storage/storageAccounts/sku.name</span></span> | <span data-ttu-id="8bb57-345">Állítsa be a hello termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="8bb57-345">Set hello SKU name.</span></span> |
| <span data-ttu-id="8bb57-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span><span class="sxs-lookup"><span data-stu-id="8bb57-346">Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly</span></span> | <span data-ttu-id="8bb57-347">Adja meg tooallow csak https-forgalom toostorage szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8bb57-347">Set tooallow only https traffic toostorage service.</span></span> |


## <a name="policy-examples"></a><span data-ttu-id="8bb57-348">Házirend példák</span><span class="sxs-lookup"><span data-stu-id="8bb57-348">Policy examples</span></span>

<span data-ttu-id="8bb57-349">a következő témakörök hello házirend példákat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="8bb57-349">hello following topics contain policy examples:</span></span>

* <span data-ttu-id="8bb57-350">Példák címke házirendek, lásd: [címkék erőforrás házirendek alkalmazását](resource-manager-policy-tags.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-350">For examples of tag polices, see [Apply resource policies for tags](resource-manager-policy-tags.md).</span></span>
* <span data-ttu-id="8bb57-351">Annak a kiosztási és a szöveg, tekintse meg a [nevét és az erőforrás-szabályzatok alkalmazása](resource-manager-policy-naming-convention.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-351">For examples of naming and text patterns, see [Apply resource policies for names and text](resource-manager-policy-naming-convention.md).</span></span>
* <span data-ttu-id="8bb57-352">A tárolási házirendek, tekintse meg a [alkalmazni házirendeket toostorage erőforrásfiókok](resource-manager-policy-storage.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-352">For examples of storage policies, see [Apply resource policies toostorage accounts](resource-manager-policy-storage.md).</span></span>
* <span data-ttu-id="8bb57-353">A virtuális gép házirendek, tekintse meg a [alkalmazni az erőforrás-házirendek tooLinux virtuális gépek](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) és [alkalmazni az erőforrás-házirendek tooWindows virtuális gépek](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="8bb57-353">For examples of virtual machine policies, see [Apply resource policies tooLinux VMs](../virtual-machines/linux/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json) and [Apply resource policies tooWindows VMs](../virtual-machines/windows/policy.md?toc=%2fazure%2fazure-resource-manager%2ftoc.json)</span></span>


## <a name="next-steps"></a><span data-ttu-id="8bb57-354">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8bb57-354">Next steps</span></span>
* <span data-ttu-id="8bb57-355">Házirend szabály megadása után rendelje hozzá tooa hatókör.</span><span class="sxs-lookup"><span data-stu-id="8bb57-355">After defining a policy rule, assign it tooa scope.</span></span> <span data-ttu-id="8bb57-356">hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-356">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="8bb57-357">REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-357">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="8bb57-358">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="8bb57-358">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>
* <span data-ttu-id="8bb57-359">hello házirend séma közzé van téve a [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span><span class="sxs-lookup"><span data-stu-id="8bb57-359">hello policy schema is published at [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json).</span></span> 


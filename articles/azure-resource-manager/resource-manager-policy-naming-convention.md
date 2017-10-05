---
title: "Az Azure erőforrás-házirendek az elnevezési konvenciókat |} Microsoft Docs"
description: "Azure Resource Manager házirendeket erőforrás elnevezési konvenciókat ismerteti."
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
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 51b3519bbba8cb4c768bfdd7dadf92fced434f22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a><span data-ttu-id="5761b-103">Nevét és az erőforrás-szabályzatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="5761b-103">Apply resource policies for names and text</span></span>
<span data-ttu-id="5761b-104">Ez a témakör bemutatja a több [erőforrás-házirendek](resource-manager-policy.md) elnevezésekor és a szöveg egyezmények létrehozására is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5761b-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply to establish naming and text conventions.</span></span> <span data-ttu-id="5761b-105">Ezek a házirendek biztosítják a konzisztenciát erőforrásnevek és az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5761b-105">These policies ensure consistency for resource names and tag values.</span></span> 

## <a name="set-naming-convention-with-wildcard"></a><span data-ttu-id="5761b-106">A helyettesítő karakteres elnevezési beállítása</span><span class="sxs-lookup"><span data-stu-id="5761b-106">Set naming convention with wildcard</span></span>
<span data-ttu-id="5761b-107">A következő példa bemutatja támogatja-e helyettesítő karakter használatát a **például** feltétel.</span><span class="sxs-lookup"><span data-stu-id="5761b-107">The following example shows the use of wildcard, which is supported by the **like** condition.</span></span> <span data-ttu-id="5761b-108">A feltétel, amely jelzi, ha a név nem egyezik meg az említett mintát (namePrefix\*nameSuffix) visszautasítja a kérelmet, majd:</span><span class="sxs-lookup"><span data-stu-id="5761b-108">The condition states that if the name does match the mentioned pattern (namePrefix\*nameSuffix) then deny the request:</span></span>

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a><span data-ttu-id="5761b-109">Állítsa be a mintával elnevezési egyezmény</span><span class="sxs-lookup"><span data-stu-id="5761b-109">Set naming convention with pattern</span></span>

<span data-ttu-id="5761b-110">Megadhatja, hogy erőforrásnevek mintát, a match feltétel használatához.</span><span class="sxs-lookup"><span data-stu-id="5761b-110">To specify that resource names match a pattern, use the match condition.</span></span> <span data-ttu-id="5761b-111">Az alábbi példa szükséges nevek kezdődnie `contoso` hat további betűket és:</span><span class="sxs-lookup"><span data-stu-id="5761b-111">The following example requires names to start with `contoso` and contain six additional letters:</span></span>

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a><span data-ttu-id="5761b-112">Címke értéke dátum szabály beállítása</span><span class="sxs-lookup"><span data-stu-id="5761b-112">Set date pattern for tag value</span></span>

<span data-ttu-id="5761b-113">A dátum minta kétjegyű, dash, három betű, dash vagy négy számjegy, használati megkövetelése:</span><span class="sxs-lookup"><span data-stu-id="5761b-113">To require a date pattern of two digits, dash, three letters, dash, and four digits, use:</span></span>

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="5761b-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5761b-114">Next steps</span></span>
* <span data-ttu-id="5761b-115">(A fenti példákban szerint) házirend szabály megadása után kell a házirend-definíció létrehozása, és rendelje hozzá hatókör.</span><span class="sxs-lookup"><span data-stu-id="5761b-115">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="5761b-116">A hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5761b-116">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="5761b-117">A portálon keresztül házirendek rendeléséhez lásd: [hozzárendelésére és kezelésére erőforrás-házirendek használata Azure-portálon](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5761b-117">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="5761b-118">REST API-t, a PowerShell vagy az Azure CLI-házirendeket rendeléséhez lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="5761b-118">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="5761b-119">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="5761b-119">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


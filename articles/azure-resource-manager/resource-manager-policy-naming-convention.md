---
title: "aaaAzure erőforrás-házirendek elnevezési konvenciókat |} Microsoft Docs"
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
ms.openlocfilehash: c8384b231263fb694aed8b936a953d5c0ca31e71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a><span data-ttu-id="b94a8-103">Nevét és az erőforrás-szabályzatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="b94a8-103">Apply resource policies for names and text</span></span>
<span data-ttu-id="b94a8-104">Ez a témakör bemutatja a több [erőforrás-házirendek](resource-manager-policy.md) tooestablish elnevezésekor és a szöveg konvenciókat is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="b94a8-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply tooestablish naming and text conventions.</span></span> <span data-ttu-id="b94a8-105">Ezek a házirendek biztosítják a konzisztenciát erőforrásnevek és az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b94a8-105">These policies ensure consistency for resource names and tag values.</span></span> 

## <a name="set-naming-convention-with-wildcard"></a><span data-ttu-id="b94a8-106">A helyettesítő karakteres elnevezési beállítása</span><span class="sxs-lookup"><span data-stu-id="b94a8-106">Set naming convention with wildcard</span></span>
<span data-ttu-id="b94a8-107">hello alábbi példa bemutatja helyettesítő, amelyet már támogat hello hello használata **például** feltétel.</span><span class="sxs-lookup"><span data-stu-id="b94a8-107">hello following example shows hello use of wildcard, which is supported by hello **like** condition.</span></span> <span data-ttu-id="b94a8-108">hello feltételt jelzi, hogy ha hello neve egyezik a hello említett mintát (namePrefix\*nameSuffix) hello majd visszautasítsa:</span><span class="sxs-lookup"><span data-stu-id="b94a8-108">hello condition states that if hello name does match hello mentioned pattern (namePrefix\*nameSuffix) then deny hello request:</span></span>

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

## <a name="set-naming-convention-with-pattern"></a><span data-ttu-id="b94a8-109">Állítsa be a mintával elnevezési egyezmény</span><span class="sxs-lookup"><span data-stu-id="b94a8-109">Set naming convention with pattern</span></span>

<span data-ttu-id="b94a8-110">hogy erőforrás megfelel egy olyan mintát, használjon hello toospecify feltétel felel meg.</span><span class="sxs-lookup"><span data-stu-id="b94a8-110">toospecify that resource names match a pattern, use hello match condition.</span></span> <span data-ttu-id="b94a8-111">hello következő példánál az szükséges a nevek toostart `contoso` hat további betűket és:</span><span class="sxs-lookup"><span data-stu-id="b94a8-111">hello following example requires names toostart with `contoso` and contain six additional letters:</span></span>

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

## <a name="set-date-pattern-for-tag-value"></a><span data-ttu-id="b94a8-112">Címke értéke dátum szabály beállítása</span><span class="sxs-lookup"><span data-stu-id="b94a8-112">Set date pattern for tag value</span></span>

<span data-ttu-id="b94a8-113">a dátum minta kétjegyű, dash, három betű, dash vagy négy számjegy, használati toorequire:</span><span class="sxs-lookup"><span data-stu-id="b94a8-113">toorequire a date pattern of two digits, dash, three letters, dash, and four digits, use:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b94a8-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b94a8-114">Next steps</span></span>
* <span data-ttu-id="b94a8-115">(A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör.</span><span class="sxs-lookup"><span data-stu-id="b94a8-115">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="b94a8-116">hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b94a8-116">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="b94a8-117">hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b94a8-117">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="b94a8-118">REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="b94a8-118">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="b94a8-119">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="b94a8-119">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


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
# <a name="apply-resource-policies-for-names-and-text"></a>Nevét és az erőforrás-szabályzatok alkalmazása
Ez a témakör bemutatja a több [erőforrás-házirendek](resource-manager-policy.md) elnevezésekor és a szöveg egyezmények létrehozására is alkalmazhat. Ezek a házirendek biztosítják a konzisztenciát erőforrásnevek és az értékeket. 

## <a name="set-naming-convention-with-wildcard"></a>A helyettesítő karakteres elnevezési beállítása
A következő példa bemutatja támogatja-e helyettesítő karakter használatát a **például** feltétel. A feltétel, amely jelzi, ha a név nem egyezik meg az említett mintát (namePrefix\*nameSuffix) visszautasítja a kérelmet, majd:

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

## <a name="set-naming-convention-with-pattern"></a>Állítsa be a mintával elnevezési egyezmény

Megadhatja, hogy erőforrásnevek mintát, a match feltétel használatához. Az alábbi példa szükséges nevek kezdődnie `contoso` hat további betűket és:

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

## <a name="set-date-pattern-for-tag-value"></a>Címke értéke dátum szabály beállítása

A dátum minta kétjegyű, dash, három betű, dash vagy négy számjegy, használati megkövetelése:

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

## <a name="next-steps"></a>Következő lépések
* (A fenti példákban szerint) házirend szabály megadása után kell a házirend-definíció létrehozása, és rendelje hozzá hatókör. A hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás. A portálon keresztül házirendek rendeléséhez lásd: [hozzárendelésére és kezelésére erőforrás-házirendek használata Azure-portálon](resource-manager-policy-portal.md). REST API-t, a PowerShell vagy az Azure CLI-házirendeket rendeléséhez lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md). 
* Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.


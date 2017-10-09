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
# <a name="apply-resource-policies-for-names-and-text"></a>Nevét és az erőforrás-szabályzatok alkalmazása
Ez a témakör bemutatja a több [erőforrás-házirendek](resource-manager-policy.md) tooestablish elnevezésekor és a szöveg konvenciókat is alkalmazhat. Ezek a házirendek biztosítják a konzisztenciát erőforrásnevek és az értékeket. 

## <a name="set-naming-convention-with-wildcard"></a>A helyettesítő karakteres elnevezési beállítása
hello alábbi példa bemutatja helyettesítő, amelyet már támogat hello hello használata **például** feltétel. hello feltételt jelzi, hogy ha hello neve egyezik a hello említett mintát (namePrefix\*nameSuffix) hello majd visszautasítsa:

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

hogy erőforrás megfelel egy olyan mintát, használjon hello toospecify feltétel felel meg. hello következő példánál az szükséges a nevek toostart `contoso` hat további betűket és:

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

a dátum minta kétjegyű, dash, három betű, dash vagy négy számjegy, használati toorequire:

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
* (A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör. hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás. hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md). REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md). 
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).


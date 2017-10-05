---
title: "Az Azure által felügyelt alkalmazás StorageAccountSelector felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Storage.StorageAccountSelector felhasználói felületi elem Azure által felügyelt alkalmazások"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 15e69c0deb4bce64b7413b557eb69db5165bde73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Microsoft.Storage.StorageAccountSelector felhasználói felületi elem
Válasszon egy új vagy meglévő tárfiókot tartozó vezérlőelem. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- Ha meg van adva, `defaultValue.name` egyedi-e automatikusan érvényesítése. Ha a tárfiók neve nem egyedi, a felhasználó adjon meg egy másik nevet, vagy egy meglévő tárfiókot.
- Az alapértelmezett érték `defaultValue.type` van **Premium_LRS**.
- Nincs megadva a bármilyen `constraints.allowedTypes` rejtett, és nincs megadva a bármilyen `constraints.excludedTypes` jelenik meg.
`constraints.allowedTypes`és `constraints.excludedTypes` mindkettő nem kötelező, de nem használható egyszerre.
- Ha `options.hideExisting` van **igaz**, a felhasználó egy meglévő tárfiók nem választható. Az alapértelmezett érték **hamis**.


## <a name="sample-output"></a>Minta kimenet
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a>Következő lépések
* Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).

---
title: "Az Azure által felügyelt alkalmazás OptionsGroup felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Common.OptionsGroup felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 6e147ed28c8248f7f17cb36fd7ae13468141dced
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a>Microsoft.Common.OptionsGroup felhasználói felületi elem
Rendelkezésre álló lehetőségek sor kijelölés vezérlőt. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.OptionsGroup",
  "label": "Some options group",
  "defaultValue": "Foo",
  "toolTip": "",
  "constraints": {
    "allowedValues": [
      {
        "label": "Foo",
        "value": "Bar"
      },
      {
        "label": "Baz",
        "value": "Qux"
      }
    ]
  },
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- A címke `constraints.allowedValues` a megjelenített szöveg-e egy elem, és a kimeneti értéket, az elem kiválasztásakor értéke.
- Ha meg van adva, az alapértelmezett érték lehet egy címke szerepel `constraints.allowedValues`. Ha nincs megadva, az első elem az `constraints.allowedValues` alapértelmezés szerint engedélyezett. Az alapértelmezett érték **null**.
- `constraints.allowedValues`legalább egy elemet kell tartalmaznia.
- Ez az elem nem támogatja a `constraints.required` tulajdonság; elem ellenőrzése sikeresen meg kell adni.

## <a name="sample-output"></a>Minta kimenet
```json
"Bar"
```

## <a name="next-steps"></a>Következő lépések
* Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).

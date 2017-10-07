---
title: "aaaAzure felügyelt alkalmazás legördülő felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Common.DropDown felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 1c07a48ad66b8e8b7fd8f59561776ecb1fc6224f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommondropdown-ui-element"></a>Microsoft.Common.DropDown felhasználói felületi elem
A legördülő lista kijelölési vezérlőt. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.DropDown",
  "label": "Some drop down",
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
- hello címkéjét `constraints.allowedValues` hello megjelenített szöveg-e egy elem, és hello elem kiválasztásakor hello kimeneti értéke érvénytelen.
- Ha meg van adva, a hello alapértelmezett értéket kell lennie egy címke szerepel `constraints.allowedValues`. Ha nincs megadva, az első elem hello `constraints.allowedValues` van kiválasztva. hello alapértelmezett értéke **null**.
- `constraints.allowedValues`legalább egy elemet kell tartalmaznia.
- Ez az elem nem támogatja a hello `constraints.required` tulajdonság. tooemulate ezt a viselkedést, vegyen fel egy elemet egy címke és értékének `""` (üres karakterlánc) túl`constraints.allowedValues`.

## <a name="sample-output"></a>Példa kimenet
```json
"Bar"
```

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).

---
title: "aaaAzure felügyelt alkalmazás szövegmező felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Common.TextBox felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 11771cd1d689b720384df98b8d1465703068af37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommontextbox-ui-element"></a>Microsoft.Common.TextBox felhasználói felületi elem
A vezérlő kijelölhető használt tooedit formázatlan szöveges. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Halp!",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- Ha `constraints.required` értéke túl**igaz**, majd hello szövegmező tartalmaznia kell egy érték toovalidate sikeresen megtörtént. hello alapértelmezett értéke **hamis**.
- `constraints.regex`a JavaScript reguláris kifejezési minta van. Ha meg van adva, majd hello beviteli mező értékét meg kell egyeznie hello mintát toovalidate sikeresen megtörtént. Az alapértelmezett érték **null**.
- `constraints.validationMessage`egy karakterlánc toodisplay esetén hello beviteli mező értéke érvényesítése sikertelen. Ha nincs megadva, az üzenetek használt beviteli mező beépített ellenőrzési hello. hello alapértelmezett értéke **null**.
- Az esetleges toospecify értéket `constraints.regex` amikor `constraints.required` értéke túl**hamis**. Ebben a forgatókönyvben egy érték nincs szükség hello szöveg mezőben toovalidate sikeresen megtörtént. Ha egy meg van adva, akkor meg kell egyeznie a hello reguláris kifejezési minta.

## <a name="sample-output"></a>Példa kimenet

```json
"foobar"
```

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).

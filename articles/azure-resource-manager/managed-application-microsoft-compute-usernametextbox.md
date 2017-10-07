---
title: "aaaAzure felügyelt alkalmazás UserNameTextBox felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Compute.UserNameTextBox felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 33092014e804c4aabd56ba49144d9cd4d6a5fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Microsoft.Compute.UserNameTextBox felhasználói felületi elem
A szövegmező-vezérlőt a Windows és Linux-felhasználónevek beépített érvényesítéssel. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and hello value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- Ha `constraints.required` értéke túl**igaz**, majd hello szövegmező tartalmaznia kell egy érték toovalidate sikeresen megtörtént. hello alapértelmezett értéke **igaz**.
- `osPlatform`meg kell adni, és lehet **Windows** vagy **Linux**.
- `constraints.regex`a JavaScript reguláris kifejezési minta van. Ha meg van adva, majd hello beviteli mező értékét meg kell egyeznie hello mintát toovalidate sikeresen megtörtént. Az alapértelmezett érték **null**.
- `constraints.validationMessage`egy karakterlánc toodisplay esetén hello beviteli mező értéke által meghatározott hello érvényesítése sikertelen `constraints.regex`. Ha nincs megadva, az üzenetek használt beviteli mező beépített ellenőrzési hello. hello alapértelmezett értéke **null**.
- Ezzel az elemmel rendelkezik beépített ellenőrzési hello számára megadott érték alapján `osPlatform`. hello beépített ellenőrzési együtt egy egyéni reguláris kifejezést is használható.
Ha értéket `constraints.regex` van megadva, akkor mindkét hello beépített és egyéni érvényesítést által kiváltott.

## <a name="sample-output"></a>Példa kimenet
```json
"tabrezm"
```

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).

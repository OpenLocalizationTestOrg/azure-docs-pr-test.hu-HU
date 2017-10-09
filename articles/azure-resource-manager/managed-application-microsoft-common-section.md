---
title: "aaaAzure felügyelt alkalmazás szakasz felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Common.Section felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: d20b365b12fab66177e1a12db2ebbeefe507212e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonsection-ui-element"></a>Microsoft.Common.Section felhasználói felületi elem
A vezérlő, amely csoportosítja a fejléc alatt egy vagy több elemet. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Séma
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- `elements`legalább egy elemet kell tartalmaznia, és kivételével az összes elem típust is tartalmazhatnak `Microsoft.Common.Section`.
- Ez az elem nem támogatja a hello `toolTip` tulajdonság.

## <a name="sample-output"></a>Példa kimenet
tooaccess hello kimeneti eleme értékének `elements`, használja a hello [basics()](managed-application-createuidefinition-functions.md#basics) vagy [steps()](managed-application-createuidefinition-functions.md#steps) funkciók és felépítését:

```json
basics('section1').element1
```

Típusú elemek `Microsoft.Common.Section` nincs kimeneti értékűek magukat.

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).

---
title: "aaaAzure felügyelt alkalmazás SizeSelector felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Compute.SizeSelector felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: d93306135d9c6f9a83692766ce1ca7ea2b688086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector felhasználói felületi elem
Egy vagy több virtuálisgép-példányok méretét kiválasztására szolgáló vezérlő. Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).

## <a name="ui-sample"></a>Felhasználói felület minta
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a>Séma
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Megjegyzések
- `recommendedSizes`tartalmaznia kell legalább egy méretét. hello először ajánlott méret hello alapértelmezésben szerepel.
- Ha az ajánlott mérete hello kiválasztott helyen nem érhető el, hello mérete automatikusan a rendszer kihagyja. Ehelyett hello következő ajánlott méret szolgál.
- Nincs megadva a hello bármilyen méretű `constraints.allowedSizes` rejtett, és nincs megadva a tetszőleges méretű `constraints.excludedSizes` jelenik meg.
`constraints.allowedSizes`és `constraints.excludedSizes` mindkettő nem kötelező, de nem használható egyszerre. elérhető méretek listáját hello meghívásával lehet meghatározni [érhető el virtuális gépek méretét az előfizetéshez listában](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).
- `osPlatform`meg kell adni, és lehet **Windows** vagy **Linux**. Hogy toodetermine hello hardverköltségek hello virtuális gépek használják.
- `imageReference`Nincs megadva a belső lemezképek, de a megadott külső képek. Hogy toodetermine hello származó szoftverek költségeit hello virtuális gépek használják.
- `count`használt tooset hello megfelelő szorzószáma hello elem van. Egy állandó érték, például támogatja **2**, vagy egy másik elem dinamikus értéket, például `[steps('step1').vmCount]`. hello alapértelmezett értéke **1**.

## <a name="sample-output"></a>Példa kimenet
```json
"Standard_D1"
```

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).
* Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).

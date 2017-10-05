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
# <a name="microsoftcommonoptionsgroup-ui-element"></a><span data-ttu-id="d4d38-103">Microsoft.Common.OptionsGroup felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="d4d38-103">Microsoft.Common.OptionsGroup UI element</span></span>
<span data-ttu-id="d4d38-104">Rendelkezésre álló lehetőségek sor kijelölés vezérlőt.</span><span class="sxs-lookup"><span data-stu-id="d4d38-104">A selection control with a row of available options.</span></span> <span data-ttu-id="d4d38-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d4d38-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d4d38-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="d4d38-106">UI sample</span></span>
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a><span data-ttu-id="d4d38-108">Séma</span><span class="sxs-lookup"><span data-stu-id="d4d38-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="d4d38-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d4d38-109">Remarks</span></span>
- <span data-ttu-id="d4d38-110">A címke `constraints.allowedValues` a megjelenített szöveg-e egy elem, és a kimeneti értéket, az elem kiválasztásakor értéke.</span><span class="sxs-lookup"><span data-stu-id="d4d38-110">The label for `constraints.allowedValues` is the display text for an item, and its value is the output value of the element when selected.</span></span>
- <span data-ttu-id="d4d38-111">Ha meg van adva, az alapértelmezett érték lehet egy címke szerepel `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="d4d38-111">If specified, the default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="d4d38-112">Ha nincs megadva, az első elem az `constraints.allowedValues` alapértelmezés szerint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="d4d38-112">If not specified, the first item in `constraints.allowedValues` is selected by default.</span></span> <span data-ttu-id="d4d38-113">Az alapértelmezett érték **null**.</span><span class="sxs-lookup"><span data-stu-id="d4d38-113">The default value is **null**.</span></span>
- <span data-ttu-id="d4d38-114">`constraints.allowedValues`legalább egy elemet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="d4d38-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="d4d38-115">Ez az elem nem támogatja a `constraints.required` tulajdonság; elem ellenőrzése sikeresen meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="d4d38-115">This element doesn't support the `constraints.required` property; an item must be selected to validate successfully.</span></span>

## <a name="sample-output"></a><span data-ttu-id="d4d38-116">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="d4d38-116">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="d4d38-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d4d38-117">Next steps</span></span>
* <span data-ttu-id="d4d38-118">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4d38-118">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d4d38-119">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d4d38-119">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d4d38-120">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d4d38-120">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

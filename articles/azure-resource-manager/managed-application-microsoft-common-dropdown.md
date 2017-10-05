---
title: "Az Azure által felügyelt alkalmazás legördülő felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Common.DropDown felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: a769e14efbae928b811fa1f1b1c2d4fba3c7692b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommondropdown-ui-element"></a><span data-ttu-id="0ac73-103">Microsoft.Common.DropDown felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="0ac73-103">Microsoft.Common.DropDown UI element</span></span>
<span data-ttu-id="0ac73-104">A legördülő lista kijelölési vezérlőt.</span><span class="sxs-lookup"><span data-stu-id="0ac73-104">A selection control with a dropdown list.</span></span> <span data-ttu-id="0ac73-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="0ac73-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="0ac73-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="0ac73-106">UI sample</span></span>
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a><span data-ttu-id="0ac73-108">Séma</span><span class="sxs-lookup"><span data-stu-id="0ac73-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="0ac73-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0ac73-109">Remarks</span></span>
- <span data-ttu-id="0ac73-110">A címke `constraints.allowedValues` a megjelenített szöveg-e egy elem, és a kimeneti értéket, az elem kiválasztásakor értéke.</span><span class="sxs-lookup"><span data-stu-id="0ac73-110">The label for `constraints.allowedValues` is the display text for an item, and its value is the output value of the element when selected.</span></span>
- <span data-ttu-id="0ac73-111">Ha meg van adva, az alapértelmezett érték lehet egy címke szerepel `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="0ac73-111">If specified, the default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="0ac73-112">Ha nincs megadva, az első elem az `constraints.allowedValues` van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0ac73-112">If not specified, the first item in `constraints.allowedValues` is selected.</span></span> <span data-ttu-id="0ac73-113">Az alapértelmezett érték **null**.</span><span class="sxs-lookup"><span data-stu-id="0ac73-113">The default value is **null**.</span></span>
- <span data-ttu-id="0ac73-114">`constraints.allowedValues`legalább egy elemet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="0ac73-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="0ac73-115">Ez az elem nem támogatja a `constraints.required` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="0ac73-115">This element doesn't support the `constraints.required` property.</span></span> <span data-ttu-id="0ac73-116">Ez viselkedésének emulációjához, vegyen fel egy elemet, label és értékének `""` (üres karakterlánc) való `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="0ac73-116">To emulate this behavior, add an item with a label and value of `""` (empty string) to `constraints.allowedValues`.</span></span>

## <a name="sample-output"></a><span data-ttu-id="0ac73-117">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="0ac73-117">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="0ac73-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0ac73-118">Next steps</span></span>
* <span data-ttu-id="0ac73-119">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0ac73-119">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="0ac73-120">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0ac73-120">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="0ac73-121">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="0ac73-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

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
# <a name="microsoftcommondropdown-ui-element"></a><span data-ttu-id="5c9ab-103">Microsoft.Common.DropDown felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="5c9ab-103">Microsoft.Common.DropDown UI element</span></span>
<span data-ttu-id="5c9ab-104">A legördülő lista kijelölési vezérlőt.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-104">A selection control with a dropdown list.</span></span> <span data-ttu-id="5c9ab-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="5c9ab-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="5c9ab-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="5c9ab-106">UI sample</span></span>
![Microsoft.Common.DropDown](./media/managed-application-elements/microsoft.common.dropdown.png)

## <a name="schema"></a><span data-ttu-id="5c9ab-108">Séma</span><span class="sxs-lookup"><span data-stu-id="5c9ab-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="5c9ab-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5c9ab-109">Remarks</span></span>
- <span data-ttu-id="5c9ab-110">hello címkéjét `constraints.allowedValues` hello megjelenített szöveg-e egy elem, és hello elem kiválasztásakor hello kimeneti értéke érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-110">hello label for `constraints.allowedValues` is hello display text for an item, and its value is hello output value of hello element when selected.</span></span>
- <span data-ttu-id="5c9ab-111">Ha meg van adva, a hello alapértelmezett értéket kell lennie egy címke szerepel `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-111">If specified, hello default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="5c9ab-112">Ha nincs megadva, az első elem hello `constraints.allowedValues` van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-112">If not specified, hello first item in `constraints.allowedValues` is selected.</span></span> <span data-ttu-id="5c9ab-113">hello alapértelmezett értéke **null**.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-113">hello default value is **null**.</span></span>
- <span data-ttu-id="5c9ab-114">`constraints.allowedValues`legalább egy elemet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="5c9ab-115">Ez az elem nem támogatja a hello `constraints.required` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-115">This element doesn't support hello `constraints.required` property.</span></span> <span data-ttu-id="5c9ab-116">tooemulate ezt a viselkedést, vegyen fel egy elemet egy címke és értékének `""` (üres karakterlánc) túl`constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="5c9ab-116">tooemulate this behavior, add an item with a label and value of `""` (empty string) too`constraints.allowedValues`.</span></span>

## <a name="sample-output"></a><span data-ttu-id="5c9ab-117">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="5c9ab-117">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="5c9ab-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c9ab-118">Next steps</span></span>
* <span data-ttu-id="5c9ab-119">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5c9ab-119">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="5c9ab-120">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5c9ab-120">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="5c9ab-121">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="5c9ab-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

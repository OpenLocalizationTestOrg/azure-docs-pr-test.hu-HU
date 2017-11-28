---
title: "Az Azure által felügyelt alkalmazás szakasz felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Common.Section felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 34c0f2f88e6af5a0f822ec116e7e2334e4e29e8d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommonsection-ui-element"></a><span data-ttu-id="0a089-103">Microsoft.Common.Section felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="0a089-103">Microsoft.Common.Section UI element</span></span>
<span data-ttu-id="0a089-104">A vezérlő, amely csoportosítja a fejléc alatt egy vagy több elemet.</span><span class="sxs-lookup"><span data-stu-id="0a089-104">A control that groups one or more elements under a heading.</span></span> <span data-ttu-id="0a089-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="0a089-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="0a089-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="0a089-106">UI sample</span></span>
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a><span data-ttu-id="0a089-108">Séma</span><span class="sxs-lookup"><span data-stu-id="0a089-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="0a089-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0a089-109">Remarks</span></span>
- <span data-ttu-id="0a089-110">`elements`legalább egy elemet kell tartalmaznia, és kivételével az összes elem típust is tartalmazhatnak `Microsoft.Common.Section`.</span><span class="sxs-lookup"><span data-stu-id="0a089-110">`elements` must contain at least one element, and can contain all element types except `Microsoft.Common.Section`.</span></span>
- <span data-ttu-id="0a089-111">Ez az elem nem támogatja a `toolTip` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="0a089-111">This element doesn't support the `toolTip` property.</span></span>

## <a name="sample-output"></a><span data-ttu-id="0a089-112">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="0a089-112">Sample output</span></span>
<span data-ttu-id="0a089-113">A kimeneti értékek elemek eléréséhez `elements`, használja a [basics()](managed-application-createuidefinition-functions.md#basics) vagy [steps()](managed-application-createuidefinition-functions.md#steps) funkciók és felépítését:</span><span class="sxs-lookup"><span data-stu-id="0a089-113">To access the output values of elements in `elements`, use the [basics()](managed-application-createuidefinition-functions.md#basics) or [steps()](managed-application-createuidefinition-functions.md#steps) functions and dot notation:</span></span>

```json
basics('section1').element1
```

<span data-ttu-id="0a089-114">Típusú elemek `Microsoft.Common.Section` nincs kimeneti értékűek magukat.</span><span class="sxs-lookup"><span data-stu-id="0a089-114">Elements of type `Microsoft.Common.Section` have no output values themselves.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a089-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a089-115">Next steps</span></span>
* <span data-ttu-id="0a089-116">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0a089-116">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="0a089-117">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0a089-117">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="0a089-118">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="0a089-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

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
# <a name="microsoftcommonsection-ui-element"></a><span data-ttu-id="6aa0e-103">Microsoft.Common.Section felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="6aa0e-103">Microsoft.Common.Section UI element</span></span>
<span data-ttu-id="6aa0e-104">A vezérlő, amely csoportosítja a fejléc alatt egy vagy több elemet.</span><span class="sxs-lookup"><span data-stu-id="6aa0e-104">A control that groups one or more elements under a heading.</span></span> <span data-ttu-id="6aa0e-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="6aa0e-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="6aa0e-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="6aa0e-106">UI sample</span></span>
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a><span data-ttu-id="6aa0e-108">Séma</span><span class="sxs-lookup"><span data-stu-id="6aa0e-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="6aa0e-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6aa0e-109">Remarks</span></span>
- <span data-ttu-id="6aa0e-110">`elements`legalább egy elemet kell tartalmaznia, és kivételével az összes elem típust is tartalmazhatnak `Microsoft.Common.Section`.</span><span class="sxs-lookup"><span data-stu-id="6aa0e-110">`elements` must contain at least one element, and can contain all element types except `Microsoft.Common.Section`.</span></span>
- <span data-ttu-id="6aa0e-111">Ez az elem nem támogatja a hello `toolTip` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="6aa0e-111">This element doesn't support hello `toolTip` property.</span></span>

## <a name="sample-output"></a><span data-ttu-id="6aa0e-112">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="6aa0e-112">Sample output</span></span>
<span data-ttu-id="6aa0e-113">tooaccess hello kimeneti eleme értékének `elements`, használja a hello [basics()](managed-application-createuidefinition-functions.md#basics) vagy [steps()](managed-application-createuidefinition-functions.md#steps) funkciók és felépítését:</span><span class="sxs-lookup"><span data-stu-id="6aa0e-113">tooaccess hello output values of elements in `elements`, use hello [basics()](managed-application-createuidefinition-functions.md#basics) or [steps()](managed-application-createuidefinition-functions.md#steps) functions and dot notation:</span></span>

```json
basics('section1').element1
```

<span data-ttu-id="6aa0e-114">Típusú elemek `Microsoft.Common.Section` nincs kimeneti értékűek magukat.</span><span class="sxs-lookup"><span data-stu-id="6aa0e-114">Elements of type `Microsoft.Common.Section` have no output values themselves.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6aa0e-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6aa0e-115">Next steps</span></span>
* <span data-ttu-id="6aa0e-116">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6aa0e-116">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="6aa0e-117">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6aa0e-117">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="6aa0e-118">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="6aa0e-118">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

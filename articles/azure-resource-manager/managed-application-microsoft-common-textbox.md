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
# <a name="microsoftcommontextbox-ui-element"></a><span data-ttu-id="43edb-103">Microsoft.Common.TextBox felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="43edb-103">Microsoft.Common.TextBox UI element</span></span>
<span data-ttu-id="43edb-104">A vezérlő kijelölhető használt tooedit formázatlan szöveges.</span><span class="sxs-lookup"><span data-stu-id="43edb-104">A control that can be used tooedit unformatted text.</span></span> <span data-ttu-id="43edb-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="43edb-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="43edb-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="43edb-106">UI sample</span></span>
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a><span data-ttu-id="43edb-108">Séma</span><span class="sxs-lookup"><span data-stu-id="43edb-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="43edb-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="43edb-109">Remarks</span></span>
- <span data-ttu-id="43edb-110">Ha `constraints.required` értéke túl**igaz**, majd hello szövegmező tartalmaznia kell egy érték toovalidate sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="43edb-110">If `constraints.required` is set too**true**, then hello text box must contain a value toovalidate successfully.</span></span> <span data-ttu-id="43edb-111">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="43edb-111">hello default value is **false**.</span></span>
- <span data-ttu-id="43edb-112">`constraints.regex`a JavaScript reguláris kifejezési minta van.</span><span class="sxs-lookup"><span data-stu-id="43edb-112">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="43edb-113">Ha meg van adva, majd hello beviteli mező értékét meg kell egyeznie hello mintát toovalidate sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="43edb-113">If specified, then hello text box's value must match hello pattern toovalidate successfully.</span></span> <span data-ttu-id="43edb-114">Az alapértelmezett érték **null**.</span><span class="sxs-lookup"><span data-stu-id="43edb-114">The default value is **null**.</span></span>
- <span data-ttu-id="43edb-115">`constraints.validationMessage`egy karakterlánc toodisplay esetén hello beviteli mező értéke érvényesítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="43edb-115">`constraints.validationMessage` is a string toodisplay when hello text box's value fails validation.</span></span> <span data-ttu-id="43edb-116">Ha nincs megadva, az üzenetek használt beviteli mező beépített ellenőrzési hello.</span><span class="sxs-lookup"><span data-stu-id="43edb-116">If not specified, then hello text box's built-in validation messages are used.</span></span> <span data-ttu-id="43edb-117">hello alapértelmezett értéke **null**.</span><span class="sxs-lookup"><span data-stu-id="43edb-117">hello default value is **null**.</span></span>
- <span data-ttu-id="43edb-118">Az esetleges toospecify értéket `constraints.regex` amikor `constraints.required` értéke túl**hamis**.</span><span class="sxs-lookup"><span data-stu-id="43edb-118">It's possible toospecify a value for `constraints.regex` when `constraints.required` is set too**false**.</span></span> <span data-ttu-id="43edb-119">Ebben a forgatókönyvben egy érték nincs szükség hello szöveg mezőben toovalidate sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="43edb-119">In this scenario, a value is not required for hello text box toovalidate successfully.</span></span> <span data-ttu-id="43edb-120">Ha egy meg van adva, akkor meg kell egyeznie a hello reguláris kifejezési minta.</span><span class="sxs-lookup"><span data-stu-id="43edb-120">If one is specified, it must match hello regular expression pattern.</span></span>

## <a name="sample-output"></a><span data-ttu-id="43edb-121">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="43edb-121">Sample output</span></span>

```json
"foobar"
```

## <a name="next-steps"></a><span data-ttu-id="43edb-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="43edb-122">Next steps</span></span>
* <span data-ttu-id="43edb-123">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="43edb-123">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="43edb-124">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="43edb-124">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="43edb-125">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="43edb-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

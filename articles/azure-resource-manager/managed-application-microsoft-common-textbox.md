---
title: "Az Azure által felügyelt alkalmazás szövegmező felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Common.TextBox felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 5a0ac5b811812c8c03f7f63aae12b8699d248ebf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcommontextbox-ui-element"></a><span data-ttu-id="fd450-103">Microsoft.Common.TextBox felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="fd450-103">Microsoft.Common.TextBox UI element</span></span>
<span data-ttu-id="fd450-104">Segítségével szerkesztheti a formázatlan szöveges vezérlő.</span><span class="sxs-lookup"><span data-stu-id="fd450-104">A control that can be used to edit unformatted text.</span></span> <span data-ttu-id="fd450-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="fd450-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="fd450-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="fd450-106">UI sample</span></span>
![Microsoft.Common.TextBox](./media/managed-application-elements/microsoft.common.textbox.png)

## <a name="schema"></a><span data-ttu-id="fd450-108">Séma</span><span class="sxs-lookup"><span data-stu-id="fd450-108">Schema</span></span>
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
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="fd450-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fd450-109">Remarks</span></span>
- <span data-ttu-id="fd450-110">Ha `constraints.required` értéke **igaz**, majd a szövegmező sikeresen érvényesíthető értéket kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="fd450-110">If `constraints.required` is set to **true**, then the text box must contain a value to validate successfully.</span></span> <span data-ttu-id="fd450-111">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="fd450-111">The default value is **false**.</span></span>
- <span data-ttu-id="fd450-112">`constraints.regex`a JavaScript reguláris kifejezési minta van.</span><span class="sxs-lookup"><span data-stu-id="fd450-112">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="fd450-113">Ha meg van adva, majd a szövegmező értékét kell megfelel a mintának sikeresen érvényesíthető.</span><span class="sxs-lookup"><span data-stu-id="fd450-113">If specified, then the text box's value must match the pattern to validate successfully.</span></span> <span data-ttu-id="fd450-114">Az alapértelmezett érték **null**.</span><span class="sxs-lookup"><span data-stu-id="fd450-114">The default value is **null**.</span></span>
- <span data-ttu-id="fd450-115">`constraints.validationMessage`egy olyan karakterlánc jelenítsen meg, ha a szövegmező értékét érvényesítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="fd450-115">`constraints.validationMessage` is a string to display when the text box's value fails validation.</span></span> <span data-ttu-id="fd450-116">Ha nincs megadva, akkor a szövegmező beépített ellenőrzési üzenetek használja.</span><span class="sxs-lookup"><span data-stu-id="fd450-116">If not specified, then the text box's built-in validation messages are used.</span></span> <span data-ttu-id="fd450-117">Az alapértelmezett érték **null**.</span><span class="sxs-lookup"><span data-stu-id="fd450-117">The default value is **null**.</span></span>
- <span data-ttu-id="fd450-118">Adjon meg egy értéket lehet `constraints.regex` amikor `constraints.required` értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="fd450-118">It's possible to specify a value for `constraints.regex` when `constraints.required` is set to **false**.</span></span> <span data-ttu-id="fd450-119">Ebben a forgatókönyvben egy érték nincs szükség a szövegmező sikeresen érvényesíthető.</span><span class="sxs-lookup"><span data-stu-id="fd450-119">In this scenario, a value is not required for the text box to validate successfully.</span></span> <span data-ttu-id="fd450-120">Ha egy meg van adva, akkor meg kell egyeznie a reguláris kifejezési minta.</span><span class="sxs-lookup"><span data-stu-id="fd450-120">If one is specified, it must match the regular expression pattern.</span></span>

## <a name="sample-output"></a><span data-ttu-id="fd450-121">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="fd450-121">Sample output</span></span>

```json
"foobar"
```

## <a name="next-steps"></a><span data-ttu-id="fd450-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fd450-122">Next steps</span></span>
* <span data-ttu-id="fd450-123">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fd450-123">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="fd450-124">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fd450-124">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="fd450-125">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="fd450-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

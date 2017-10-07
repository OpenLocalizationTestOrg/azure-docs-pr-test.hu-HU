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
# <a name="microsoftcomputeusernametextbox-ui-element"></a><span data-ttu-id="fcf72-103">Microsoft.Compute.UserNameTextBox felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="fcf72-103">Microsoft.Compute.UserNameTextBox UI element</span></span>
<span data-ttu-id="fcf72-104">A szövegmező-vezérlőt a Windows és Linux-felhasználónevek beépített érvényesítéssel.</span><span class="sxs-lookup"><span data-stu-id="fcf72-104">A text box control with built-in validation for Windows and Linux user names.</span></span> <span data-ttu-id="fcf72-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="fcf72-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="fcf72-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="fcf72-106">UI sample</span></span>
![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a><span data-ttu-id="fcf72-108">Séma</span><span class="sxs-lookup"><span data-stu-id="fcf72-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="fcf72-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fcf72-109">Remarks</span></span>
- <span data-ttu-id="fcf72-110">Ha `constraints.required` értéke túl**igaz**, majd hello szövegmező tartalmaznia kell egy érték toovalidate sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="fcf72-110">If `constraints.required` is set too**true**, then hello text box must contain a value toovalidate successfully.</span></span> <span data-ttu-id="fcf72-111">hello alapértelmezett értéke **igaz**.</span><span class="sxs-lookup"><span data-stu-id="fcf72-111">hello default value is **true**.</span></span>
- <span data-ttu-id="fcf72-112">`osPlatform`meg kell adni, és lehet **Windows** vagy **Linux**.</span><span class="sxs-lookup"><span data-stu-id="fcf72-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="fcf72-113">`constraints.regex`a JavaScript reguláris kifejezési minta van.</span><span class="sxs-lookup"><span data-stu-id="fcf72-113">`constraints.regex` is a JavaScript regular expression pattern.</span></span> <span data-ttu-id="fcf72-114">Ha meg van adva, majd hello beviteli mező értékét meg kell egyeznie hello mintát toovalidate sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="fcf72-114">If specified, then hello text box's value must match hello pattern toovalidate successfully.</span></span> <span data-ttu-id="fcf72-115">Az alapértelmezett érték **null**.</span><span class="sxs-lookup"><span data-stu-id="fcf72-115">The default value is **null**.</span></span>
- <span data-ttu-id="fcf72-116">`constraints.validationMessage`egy karakterlánc toodisplay esetén hello beviteli mező értéke által meghatározott hello érvényesítése sikertelen `constraints.regex`.</span><span class="sxs-lookup"><span data-stu-id="fcf72-116">`constraints.validationMessage` is a string toodisplay when hello text box's value fails hello validation specified by `constraints.regex`.</span></span> <span data-ttu-id="fcf72-117">Ha nincs megadva, az üzenetek használt beviteli mező beépített ellenőrzési hello.</span><span class="sxs-lookup"><span data-stu-id="fcf72-117">If not specified, then hello text box's built-in validation messages are used.</span></span> <span data-ttu-id="fcf72-118">hello alapértelmezett értéke **null**.</span><span class="sxs-lookup"><span data-stu-id="fcf72-118">hello default value is **null**.</span></span>
- <span data-ttu-id="fcf72-119">Ezzel az elemmel rendelkezik beépített ellenőrzési hello számára megadott érték alapján `osPlatform`.</span><span class="sxs-lookup"><span data-stu-id="fcf72-119">This element has built-in validation that is based on hello value specified for `osPlatform`.</span></span> <span data-ttu-id="fcf72-120">hello beépített ellenőrzési együtt egy egyéni reguláris kifejezést is használható.</span><span class="sxs-lookup"><span data-stu-id="fcf72-120">hello built-in validation can be used along with a custom regular expression.</span></span>
<span data-ttu-id="fcf72-121">Ha értéket `constraints.regex` van megadva, akkor mindkét hello beépített és egyéni érvényesítést által kiváltott.</span><span class="sxs-lookup"><span data-stu-id="fcf72-121">If a value for `constraints.regex` is specified, then both hello built-in and custom validations are triggered.</span></span>

## <a name="sample-output"></a><span data-ttu-id="fcf72-122">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="fcf72-122">Sample output</span></span>
```json
"tabrezm"
```

## <a name="next-steps"></a><span data-ttu-id="fcf72-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fcf72-123">Next steps</span></span>
* <span data-ttu-id="fcf72-124">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fcf72-124">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="fcf72-125">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fcf72-125">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="fcf72-126">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="fcf72-126">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

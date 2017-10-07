---
title: "aaaAzure felügyelt alkalmazás OptionsGroup felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Common.OptionsGroup felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: e222468009c8db567bdde9f42836a48fb791da00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcommonoptionsgroup-ui-element"></a><span data-ttu-id="13be7-103">Microsoft.Common.OptionsGroup felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="13be7-103">Microsoft.Common.OptionsGroup UI element</span></span>
<span data-ttu-id="13be7-104">Rendelkezésre álló lehetőségek sor kijelölés vezérlőt.</span><span class="sxs-lookup"><span data-stu-id="13be7-104">A selection control with a row of available options.</span></span> <span data-ttu-id="13be7-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="13be7-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="13be7-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="13be7-106">UI sample</span></span>
![Microsoft.Common.OptionsGroup](./media/managed-application-elements/microsoft.common.optionsgroup.png)

## <a name="schema"></a><span data-ttu-id="13be7-108">Séma</span><span class="sxs-lookup"><span data-stu-id="13be7-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="13be7-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="13be7-109">Remarks</span></span>
- <span data-ttu-id="13be7-110">hello címkéjét `constraints.allowedValues` hello megjelenített szöveg-e egy elem, és hello elem kiválasztásakor hello kimeneti értéke érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="13be7-110">hello label for `constraints.allowedValues` is hello display text for an item, and its value is hello output value of hello element when selected.</span></span>
- <span data-ttu-id="13be7-111">Ha meg van adva, a hello alapértelmezett értéket kell lennie egy címke szerepel `constraints.allowedValues`.</span><span class="sxs-lookup"><span data-stu-id="13be7-111">If specified, hello default value must be a label present in `constraints.allowedValues`.</span></span> <span data-ttu-id="13be7-112">Ha nincs megadva, az első elem hello `constraints.allowedValues` alapértelmezés szerint engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="13be7-112">If not specified, hello first item in `constraints.allowedValues` is selected by default.</span></span> <span data-ttu-id="13be7-113">hello alapértelmezett értéke **null**.</span><span class="sxs-lookup"><span data-stu-id="13be7-113">hello default value is **null**.</span></span>
- <span data-ttu-id="13be7-114">`constraints.allowedValues`legalább egy elemet kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="13be7-114">`constraints.allowedValues` must contain at least one item.</span></span>
- <span data-ttu-id="13be7-115">Ez az elem nem támogatja a hello `constraints.required` tulajdonság; csak elemek kijelölt toovalidate sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="13be7-115">This element doesn't support hello `constraints.required` property; an item must be selected toovalidate successfully.</span></span>

## <a name="sample-output"></a><span data-ttu-id="13be7-116">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="13be7-116">Sample output</span></span>
```json
"Bar"
```

## <a name="next-steps"></a><span data-ttu-id="13be7-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13be7-117">Next steps</span></span>
* <span data-ttu-id="13be7-118">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="13be7-118">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="13be7-119">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="13be7-119">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="13be7-120">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="13be7-120">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

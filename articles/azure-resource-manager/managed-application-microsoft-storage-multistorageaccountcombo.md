---
title: "aaaAzure felügyelt alkalmazás MultiStorageAccountCombo felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Storage.MultiStorageAccountCombo felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 765be145b61c3dbf0a035a7a00aa18eee464a3eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragemultistorageaccountcombo-ui-element"></a><span data-ttu-id="c52f7-103">Microsoft.Storage.MultiStorageAccountCombo felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="c52f7-103">Microsoft.Storage.MultiStorageAccountCombo UI element</span></span>
<span data-ttu-id="c52f7-104">Több tárfiókot, a közös előtaggal kezdődő neveket való létrehozásának vezérlők egy csoportja.</span><span class="sxs-lookup"><span data-stu-id="c52f7-104">A group of controls for creating multiple storage accounts, with names that start with a common prefix.</span></span> <span data-ttu-id="c52f7-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c52f7-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="c52f7-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="c52f7-106">UI sample</span></span>
![Microsoft.Storage.MultiStorageAccountCombo](./media/managed-application-elements/microsoft.storage.multistorageaccountcombo.png)

## <a name="schema"></a><span data-ttu-id="c52f7-108">Séma</span><span class="sxs-lookup"><span data-stu-id="c52f7-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.MultiStorageAccountCombo",
  "label": {
    "prefix": "Storage account prefix",
    "type": "Storage account type"
  },
  "toolTip": {
    "prefix": "",
    "type": ""
  },
  "defaultValue": {
    "prefix": "sa",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="c52f7-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c52f7-109">Remarks</span></span>
- <span data-ttu-id="c52f7-110">a következő hello `defaultValue.prefix` egy vagy több egész számok toogenerate hello sorozatát tárfiókneveket van kibővítve.</span><span class="sxs-lookup"><span data-stu-id="c52f7-110">hello value for `defaultValue.prefix` is concatenated with one or more integers toogenerate hello sequence of storage account names.</span></span> <span data-ttu-id="c52f7-111">Például ha `defaultValue.prefix` van **foobar** és `count` van **2**, majd tárfiókok neve **foobar1** és **foobar2** jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="c52f7-111">For example, if `defaultValue.prefix` is **foobar** and `count` is **2**, then storage account names **foobar1** and **foobar2** are generated.</span></span> <span data-ttu-id="c52f7-112">Létrehozott tárfiókok neve automatikusan érvényesíti egyedi-e.</span><span class="sxs-lookup"><span data-stu-id="c52f7-112">Generated storage account names are validated for uniqueness automatically.</span></span>
- <span data-ttu-id="c52f7-113">hello tárfiókok neve lexicographically alapján generált `count`.</span><span class="sxs-lookup"><span data-stu-id="c52f7-113">hello storage account names are generated lexicographically based on `count`.</span></span> <span data-ttu-id="c52f7-114">Például ha `count` 10, majd 2 számjegyből álló egész számok végén hello tárfiókok neve (01, 02, 03, stb.).</span><span class="sxs-lookup"><span data-stu-id="c52f7-114">For example, if `count` is 10, then hello storage account names end with 2-digit integers (01, 02, 03, etc.).</span></span>
- <span data-ttu-id="c52f7-115">az alapértelmezett érték hello `defaultValue.prefix` van **null**, és a `defaultValue.type` van **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="c52f7-115">hello default value for `defaultValue.prefix` is **null**, and for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="c52f7-116">Nincs megadva a bármilyen `constraints.allowedTypes` rejtett, és nincs megadva a bármilyen `constraints.excludedTypes` jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c52f7-116">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="c52f7-117">`constraints.allowedTypes`és `constraints.excludedTypes` mindkettő nem kötelező, de nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="c52f7-117">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="c52f7-118">Továbbá toogenerating tárfiókok neve, a `count` használt tooset van a megfelelő többszöröző hello elemhez.</span><span class="sxs-lookup"><span data-stu-id="c52f7-118">In addition toogenerating storage account names, `count` is used tooset the appropriate multiplier for hello element.</span></span> <span data-ttu-id="c52f7-119">Egy állandó érték, például támogatja **2**, vagy egy másik elem dinamikus értéket, például `[steps('step1').storageAccountCount]`.</span><span class="sxs-lookup"><span data-stu-id="c52f7-119">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').storageAccountCount]`.</span></span> <span data-ttu-id="c52f7-120">hello alapértelmezett értéke **1**.</span><span class="sxs-lookup"><span data-stu-id="c52f7-120">hello default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="c52f7-121">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="c52f7-121">Sample output</span></span>
```json
{
  "prefix": "sa",
  "count": 2,
  "resourceGroup": "rg01",
  "type": "Premium_LRS"
}
```

## <a name="next-steps"></a><span data-ttu-id="c52f7-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c52f7-122">Next steps</span></span>
* <span data-ttu-id="c52f7-123">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c52f7-123">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="c52f7-124">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c52f7-124">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="c52f7-125">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="c52f7-125">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

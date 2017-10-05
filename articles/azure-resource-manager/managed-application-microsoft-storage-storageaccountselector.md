---
title: "Az Azure által felügyelt alkalmazás StorageAccountSelector felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Storage.StorageAccountSelector felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 15e69c0deb4bce64b7413b557eb69db5165bde73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a><span data-ttu-id="d3910-103">Microsoft.Storage.StorageAccountSelector felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="d3910-103">Microsoft.Storage.StorageAccountSelector UI element</span></span>
<span data-ttu-id="d3910-104">Válasszon egy új vagy meglévő tárfiókot tartozó vezérlőelem.</span><span class="sxs-lookup"><span data-stu-id="d3910-104">A control for selecting a new or existing storage account.</span></span> <span data-ttu-id="d3910-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="d3910-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="d3910-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="d3910-106">UI sample</span></span>
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a><span data-ttu-id="d3910-108">Séma</span><span class="sxs-lookup"><span data-stu-id="d3910-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="d3910-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d3910-109">Remarks</span></span>
- <span data-ttu-id="d3910-110">Ha meg van adva, `defaultValue.name` egyedi-e automatikusan érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="d3910-110">If specified, `defaultValue.name` is automatically validated for uniqueness.</span></span> <span data-ttu-id="d3910-111">Ha a tárfiók neve nem egyedi, a felhasználó adjon meg egy másik nevet, vagy egy meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="d3910-111">If the storage account name is not unique, the user must specify a different name or choose an existing storage account.</span></span>
- <span data-ttu-id="d3910-112">Az alapértelmezett érték `defaultValue.type` van **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="d3910-112">The default value for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="d3910-113">Nincs megadva a bármilyen `constraints.allowedTypes` rejtett, és nincs megadva a bármilyen `constraints.excludedTypes` jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d3910-113">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="d3910-114">`constraints.allowedTypes`és `constraints.excludedTypes` mindkettő nem kötelező, de nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="d3910-114">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="d3910-115">Ha `options.hideExisting` van **igaz**, a felhasználó egy meglévő tárfiók nem választható.</span><span class="sxs-lookup"><span data-stu-id="d3910-115">If `options.hideExisting` is **true**, the user can't choose an existing storage account.</span></span> <span data-ttu-id="d3910-116">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="d3910-116">The default value is **false**.</span></span>


## <a name="sample-output"></a><span data-ttu-id="d3910-117">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="d3910-117">Sample output</span></span>
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a><span data-ttu-id="d3910-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3910-118">Next steps</span></span>
* <span data-ttu-id="d3910-119">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3910-119">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="d3910-120">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d3910-120">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="d3910-121">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="d3910-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

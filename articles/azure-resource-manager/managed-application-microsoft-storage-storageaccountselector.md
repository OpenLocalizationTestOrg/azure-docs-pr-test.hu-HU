---
title: "aaaAzure felügyelt alkalmazás StorageAccountSelector felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Storage.StorageAccountSelector felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: a2c9545feed4c4afb3c64b30b42c94d5382a108d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a><span data-ttu-id="8125e-103">Microsoft.Storage.StorageAccountSelector felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="8125e-103">Microsoft.Storage.StorageAccountSelector UI element</span></span>
<span data-ttu-id="8125e-104">Válasszon egy új vagy meglévő tárfiókot tartozó vezérlőelem.</span><span class="sxs-lookup"><span data-stu-id="8125e-104">A control for selecting a new or existing storage account.</span></span> <span data-ttu-id="8125e-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="8125e-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="8125e-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="8125e-106">UI sample</span></span>
![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft.storage.storageaccountselector.png)

## <a name="schema"></a><span data-ttu-id="8125e-108">Séma</span><span class="sxs-lookup"><span data-stu-id="8125e-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="8125e-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8125e-109">Remarks</span></span>
- <span data-ttu-id="8125e-110">Ha meg van adva, `defaultValue.name` egyedi-e automatikusan érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="8125e-110">If specified, `defaultValue.name` is automatically validated for uniqueness.</span></span> <span data-ttu-id="8125e-111">Ha hello tárfiók neve nem egyedi, hello felhasználói adjon meg másik nevet, vagy egy meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="8125e-111">If hello storage account name is not unique, hello user must specify a different name or choose an existing storage account.</span></span>
- <span data-ttu-id="8125e-112">az alapértelmezett érték hello `defaultValue.type` van **Premium_LRS**.</span><span class="sxs-lookup"><span data-stu-id="8125e-112">hello default value for `defaultValue.type` is **Premium_LRS**.</span></span>
- <span data-ttu-id="8125e-113">Nincs megadva a bármilyen `constraints.allowedTypes` rejtett, és nincs megadva a bármilyen `constraints.excludedTypes` jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8125e-113">Any type not specified in `constraints.allowedTypes` is hidden, and any type not specified in `constraints.excludedTypes` is shown.</span></span>
<span data-ttu-id="8125e-114">`constraints.allowedTypes`és `constraints.excludedTypes` mindkettő nem kötelező, de nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="8125e-114">`constraints.allowedTypes` and `constraints.excludedTypes` are both optional, but cannot be used simultaneously.</span></span>
- <span data-ttu-id="8125e-115">Ha `options.hideExisting` van **igaz**, hello felhasználói egy meglévő tárfiók nem választható.</span><span class="sxs-lookup"><span data-stu-id="8125e-115">If `options.hideExisting` is **true**, hello user can't choose an existing storage account.</span></span> <span data-ttu-id="8125e-116">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="8125e-116">hello default value is **false**.</span></span>


## <a name="sample-output"></a><span data-ttu-id="8125e-117">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="8125e-117">Sample output</span></span>
```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="next-steps"></a><span data-ttu-id="8125e-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8125e-118">Next steps</span></span>
* <span data-ttu-id="8125e-119">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8125e-119">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="8125e-120">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8125e-120">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="8125e-121">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="8125e-121">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

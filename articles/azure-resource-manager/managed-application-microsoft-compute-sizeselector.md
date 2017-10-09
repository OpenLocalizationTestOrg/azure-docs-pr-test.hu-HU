---
title: "aaaAzure felügyelt alkalmazás SizeSelector felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Compute.SizeSelector felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: d93306135d9c6f9a83692766ce1ca7ea2b688086
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a><span data-ttu-id="3be4d-103">Microsoft.Compute.SizeSelector felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="3be4d-103">Microsoft.Compute.SizeSelector UI element</span></span>
<span data-ttu-id="3be4d-104">Egy vagy több virtuálisgép-példányok méretét kiválasztására szolgáló vezérlő.</span><span class="sxs-lookup"><span data-stu-id="3be4d-104">A control for selecting a size for one or more virtual machine instances.</span></span> <span data-ttu-id="3be4d-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3be4d-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="3be4d-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="3be4d-106">UI sample</span></span>
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a><span data-ttu-id="3be4d-108">Séma</span><span class="sxs-lookup"><span data-stu-id="3be4d-108">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": []
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="3be4d-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3be4d-109">Remarks</span></span>
- <span data-ttu-id="3be4d-110">`recommendedSizes`tartalmaznia kell legalább egy méretét.</span><span class="sxs-lookup"><span data-stu-id="3be4d-110">`recommendedSizes` should contain at least one size.</span></span> <span data-ttu-id="3be4d-111">hello először ajánlott méret hello alapértelmezésben szerepel.</span><span class="sxs-lookup"><span data-stu-id="3be4d-111">hello first recommended size is used as hello default.</span></span>
- <span data-ttu-id="3be4d-112">Ha az ajánlott mérete hello kiválasztott helyen nem érhető el, hello mérete automatikusan a rendszer kihagyja.</span><span class="sxs-lookup"><span data-stu-id="3be4d-112">If a recommended size is not available in hello selected location, hello size is automatically skipped.</span></span> <span data-ttu-id="3be4d-113">Ehelyett hello következő ajánlott méret szolgál.</span><span class="sxs-lookup"><span data-stu-id="3be4d-113">Instead, hello next recommended size is used.</span></span>
- <span data-ttu-id="3be4d-114">Nincs megadva a hello bármilyen méretű `constraints.allowedSizes` rejtett, és nincs megadva a tetszőleges méretű `constraints.excludedSizes` jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3be4d-114">Any size not specified in hello `constraints.allowedSizes` is hidden, and any size not specified in `constraints.excludedSizes` is shown.</span></span>
<span data-ttu-id="3be4d-115">`constraints.allowedSizes`és `constraints.excludedSizes` mindkettő nem kötelező, de nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="3be4d-115">`constraints.allowedSizes` and `constraints.excludedSizes` are both optional, but cannot be used simultaneously.</span></span> <span data-ttu-id="3be4d-116">elérhető méretek listáját hello meghívásával lehet meghatározni [érhető el virtuális gépek méretét az előfizetéshez listában](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span><span class="sxs-lookup"><span data-stu-id="3be4d-116">hello list of available sizes can be determined by calling [List available virtual machine sizes for a subscription](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span></span>
- <span data-ttu-id="3be4d-117">`osPlatform`meg kell adni, és lehet **Windows** vagy **Linux**.</span><span class="sxs-lookup"><span data-stu-id="3be4d-117">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span> <span data-ttu-id="3be4d-118">Hogy toodetermine hello hardverköltségek hello virtuális gépek használják.</span><span class="sxs-lookup"><span data-stu-id="3be4d-118">It's used toodetermine hello hardware costs of hello virtual machines.</span></span>
- <span data-ttu-id="3be4d-119">`imageReference`Nincs megadva a belső lemezképek, de a megadott külső képek.</span><span class="sxs-lookup"><span data-stu-id="3be4d-119">`imageReference` is omitted for first-party images, but provided for third-party images.</span></span> <span data-ttu-id="3be4d-120">Hogy toodetermine hello származó szoftverek költségeit hello virtuális gépek használják.</span><span class="sxs-lookup"><span data-stu-id="3be4d-120">It's used toodetermine hello software costs of hello virtual machines.</span></span>
- <span data-ttu-id="3be4d-121">`count`használt tooset hello megfelelő szorzószáma hello elem van.</span><span class="sxs-lookup"><span data-stu-id="3be4d-121">`count` is used tooset hello appropriate multiplier for hello element.</span></span> <span data-ttu-id="3be4d-122">Egy állandó érték, például támogatja **2**, vagy egy másik elem dinamikus értéket, például `[steps('step1').vmCount]`.</span><span class="sxs-lookup"><span data-stu-id="3be4d-122">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').vmCount]`.</span></span> <span data-ttu-id="3be4d-123">hello alapértelmezett értéke **1**.</span><span class="sxs-lookup"><span data-stu-id="3be4d-123">hello default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="3be4d-124">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="3be4d-124">Sample output</span></span>
```json
"Standard_D1"
```

## <a name="next-steps"></a><span data-ttu-id="3be4d-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3be4d-125">Next steps</span></span>
* <span data-ttu-id="3be4d-126">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3be4d-126">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="3be4d-127">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3be4d-127">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="3be4d-128">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="3be4d-128">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

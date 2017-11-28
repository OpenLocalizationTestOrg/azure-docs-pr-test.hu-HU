---
title: "Az Azure által felügyelt alkalmazás SizeSelector felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Compute.SizeSelector felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: e54962f73028ada258a7faef271d66f0fbcef649
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcomputesizeselector-ui-element"></a><span data-ttu-id="499e7-103">Microsoft.Compute.SizeSelector felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="499e7-103">Microsoft.Compute.SizeSelector UI element</span></span>
<span data-ttu-id="499e7-104">Egy vagy több virtuálisgép-példányok méretét kiválasztására szolgáló vezérlő.</span><span class="sxs-lookup"><span data-stu-id="499e7-104">A control for selecting a size for one or more virtual machine instances.</span></span> <span data-ttu-id="499e7-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="499e7-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="499e7-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="499e7-106">UI sample</span></span>
![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

## <a name="schema"></a><span data-ttu-id="499e7-108">Séma</span><span class="sxs-lookup"><span data-stu-id="499e7-108">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="499e7-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="499e7-109">Remarks</span></span>
- <span data-ttu-id="499e7-110">`recommendedSizes`tartalmaznia kell legalább egy méretét.</span><span class="sxs-lookup"><span data-stu-id="499e7-110">`recommendedSizes` should contain at least one size.</span></span> <span data-ttu-id="499e7-111">Az első ajánlott mérete alapértelmezés szerint használt.</span><span class="sxs-lookup"><span data-stu-id="499e7-111">The first recommended size is used as the default.</span></span>
- <span data-ttu-id="499e7-112">Ha az ajánlott méretet a kiválasztott helyen nem érhető el, a mérete automatikusan a rendszer kihagyja.</span><span class="sxs-lookup"><span data-stu-id="499e7-112">If a recommended size is not available in the selected location, the size is automatically skipped.</span></span> <span data-ttu-id="499e7-113">Ehelyett a következő ajánlott méret szolgál.</span><span class="sxs-lookup"><span data-stu-id="499e7-113">Instead, the next recommended size is used.</span></span>
- <span data-ttu-id="499e7-114">Nincs megadva a tetszőleges méretű a `constraints.allowedSizes` rejtett, és nincs megadva a tetszőleges méretű `constraints.excludedSizes` látható.</span><span class="sxs-lookup"><span data-stu-id="499e7-114">Any size not specified in the `constraints.allowedSizes` is hidden, and any size not specified in `constraints.excludedSizes` is shown.</span></span>
<span data-ttu-id="499e7-115">`constraints.allowedSizes`és `constraints.excludedSizes` mindkettő nem kötelező, de nem használható egyszerre.</span><span class="sxs-lookup"><span data-stu-id="499e7-115">`constraints.allowedSizes` and `constraints.excludedSizes` are both optional, but cannot be used simultaneously.</span></span> <span data-ttu-id="499e7-116">Az elérhető méretek listáját meghívásával lehet meghatározni [érhető el virtuális gépek méretét az előfizetéshez listában](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span><span class="sxs-lookup"><span data-stu-id="499e7-116">The list of available sizes can be determined by calling [List available virtual machine sizes for a subscription](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region).</span></span>
- <span data-ttu-id="499e7-117">`osPlatform`meg kell adni, és lehet **Windows** vagy **Linux**.</span><span class="sxs-lookup"><span data-stu-id="499e7-117">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span> <span data-ttu-id="499e7-118">Határozza meg a virtuális gépek hardverköltségek szolgál.</span><span class="sxs-lookup"><span data-stu-id="499e7-118">It's used to determine the hardware costs of the virtual machines.</span></span>
- <span data-ttu-id="499e7-119">`imageReference`Nincs megadva a belső lemezképek, de a megadott külső képek.</span><span class="sxs-lookup"><span data-stu-id="499e7-119">`imageReference` is omitted for first-party images, but provided for third-party images.</span></span> <span data-ttu-id="499e7-120">A virtuális gépek szoftverek költségeit meghatározására szolgál.</span><span class="sxs-lookup"><span data-stu-id="499e7-120">It's used to determine the software costs of the virtual machines.</span></span>
- <span data-ttu-id="499e7-121">`count`az elem a megfelelő szorzószáma beállítására használatos.</span><span class="sxs-lookup"><span data-stu-id="499e7-121">`count` is used to set the appropriate multiplier for the element.</span></span> <span data-ttu-id="499e7-122">Egy állandó érték, például támogatja **2**, vagy egy másik elem dinamikus értéket, például `[steps('step1').vmCount]`.</span><span class="sxs-lookup"><span data-stu-id="499e7-122">It supports a static value, like **2**, or a dynamic value from another element, like `[steps('step1').vmCount]`.</span></span> <span data-ttu-id="499e7-123">Az alapértelmezett érték **1**.</span><span class="sxs-lookup"><span data-stu-id="499e7-123">The default value is **1**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="499e7-124">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="499e7-124">Sample output</span></span>
```json
"Standard_D1"
```

## <a name="next-steps"></a><span data-ttu-id="499e7-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="499e7-125">Next steps</span></span>
* <span data-ttu-id="499e7-126">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="499e7-126">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="499e7-127">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="499e7-127">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="499e7-128">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="499e7-128">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

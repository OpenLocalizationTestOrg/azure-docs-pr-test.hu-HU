---
title: "Az Azure által felügyelt alkalmazás VirtualNetworkCombo felhasználói felületi elem |} Microsoft Docs"
description: "A témakör ismerteti a Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 8bb255b76ac5c3de570fa569a1cfb3ee953f9687
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a><span data-ttu-id="935ef-103">Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="935ef-103">Microsoft.Network.VirtualNetworkCombo UI element</span></span>
<span data-ttu-id="935ef-104">Egy új vagy meglévő virtuális hálózat kiválasztásához vezérlők egy csoportja.</span><span class="sxs-lookup"><span data-stu-id="935ef-104">A group of controls for selecting a new or existing virtual network.</span></span> <span data-ttu-id="935ef-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="935ef-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="935ef-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="935ef-106">UI sample</span></span>
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- <span data-ttu-id="935ef-108">A felső Drótvázdiagram, az a felhasználó kiválasztotta egy új virtuális hálózat, a felhasználó minden alhálózat nevét és címét előtag szabhatja testre.</span><span class="sxs-lookup"><span data-stu-id="935ef-108">In the top wireframe, the user has picked a new virtual network, so the user can customize each subnet's name and address prefix.</span></span> <span data-ttu-id="935ef-109">Alhálózatok konfigurálása ebben az esetben nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="935ef-109">Configuring subnets in this case is optional.</span></span>
- <span data-ttu-id="935ef-110">Az alsó Drótvázdiagram a felhasználó kiválasztotta meglévő virtuális hálózat, így a felhasználó a központi telepítési sablont igényel minden egyes alhálózatot kell társítani egy létező alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="935ef-110">In the bottom wireframe, the user has picked an existing virtual network, so the user must map each subnet the deployment template requires to an existing subnet.</span></span> <span data-ttu-id="935ef-111">Alhálózatok konfigurálása ebben az esetben szükség.</span><span class="sxs-lookup"><span data-stu-id="935ef-111">Configuring subnets in this case is required.</span></span>

## <a name="schema"></a><span data-ttu-id="935ef-112">Séma</span><span class="sxs-lookup"><span data-stu-id="935ef-112">Schema</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="935ef-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="935ef-113">Remarks</span></span>
- <span data-ttu-id="935ef-114">Ha meg van adva, az első mozaikként, átfedés nélkül cím előtag méretű `defaultValue.addressPrefixSize` a felhasználó az előfizetéshez létező virtuális hálózatok automatikusan alapján történik.</span><span class="sxs-lookup"><span data-stu-id="935ef-114">If specified, the first non-overlapping address prefix of size `defaultValue.addressPrefixSize` is determined automatically based on the existing virtual networks in the user's subscription.</span></span>
- <span data-ttu-id="935ef-115">Az alapértelmezett érték `defaultValue.name` és `defaultValue.addressPrefixSize` van **null**.</span><span class="sxs-lookup"><span data-stu-id="935ef-115">The default value for `defaultValue.name` and `defaultValue.addressPrefixSize` is **null**.</span></span>
- <span data-ttu-id="935ef-116">`constraints.minAddressPrefixSize`meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="935ef-116">`constraints.minAddressPrefixSize` must be specified.</span></span> <span data-ttu-id="935ef-117">Meglévő virtuális hálózatok egy kisebb, mint a megadott érték címtartománnyal rendelkező kijelölésnél érhetők el.</span><span class="sxs-lookup"><span data-stu-id="935ef-117">Any existing virtual networks with an address space smaller than the specified value are unavailable for selection.</span></span>
- <span data-ttu-id="935ef-118">`subnets`meg kell adni, és `constraints.minAddressPrefixSize` meg kell adni az egyes alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="935ef-118">`subnets` must be specified, and `constraints.minAddressPrefixSize` must be specified for each subnet.</span></span>
- <span data-ttu-id="935ef-119">Amikor új virtuális hálózat létrehozása minden alhálózati cím előtag alapján van kiszámítva automatikusan a virtuális hálózat címelőtagjához és a megfelelő `addressPrefixSize`.</span><span class="sxs-lookup"><span data-stu-id="935ef-119">When creating a new virtual network, each subnet's address prefix is calculated automatically based on the virtual network's address prefix and the respective `addressPrefixSize`.</span></span>
- <span data-ttu-id="935ef-120">Ha egy meglévő virtuális hálózat, kisebb, mint a megfelelő alhálózatok `constraints.minAddressPrefixSize` kijelölés nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="935ef-120">When using an existing virtual network, any subnets smaller than the respective `constraints.minAddressPrefixSize` are unavailable for selection.</span></span> <span data-ttu-id="935ef-121">Továbbá ha meg van adva, alhálózatok, amelyek nem tartalmaznak legalább `minAddressCount` elérhető címei nem lehet kiválasztani.</span><span class="sxs-lookup"><span data-stu-id="935ef-121">Additionally, if specified, subnets that do not contain at least `minAddressCount` available addresses are unavailable for selection.</span></span>
<span data-ttu-id="935ef-122">Az alapértelmezett érték **0**.</span><span class="sxs-lookup"><span data-stu-id="935ef-122">The default value is **0**.</span></span> <span data-ttu-id="935ef-123">Győződjön meg arról, hogy a rendelkezésre álló címek összefüggő, adja meg a **igaz** a `requireContiguousAddresses`.</span><span class="sxs-lookup"><span data-stu-id="935ef-123">To ensure that the available addresses are contiguous, specify **true** for `requireContiguousAddresses`.</span></span> <span data-ttu-id="935ef-124">Az alapértelmezett érték **igaz**.</span><span class="sxs-lookup"><span data-stu-id="935ef-124">The default value is **true**.</span></span>
- <span data-ttu-id="935ef-125">A meglévő virtuális hálózat alhálózatok létrehozása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="935ef-125">Creating subnets in an existing virtual network is not supported.</span></span>
- <span data-ttu-id="935ef-126">Ha `options.hideExisting` van **igaz**, a felhasználó egy meglévő virtuális hálózat nem választható.</span><span class="sxs-lookup"><span data-stu-id="935ef-126">If `options.hideExisting` is **true**, the user can't choose an existing virtual network.</span></span> <span data-ttu-id="935ef-127">Az alapértelmezett érték **hamis**.</span><span class="sxs-lookup"><span data-stu-id="935ef-127">The default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="935ef-128">Minta kimenet</span><span class="sxs-lookup"><span data-stu-id="935ef-128">Sample output</span></span>
```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="935ef-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="935ef-129">Next steps</span></span>
* <span data-ttu-id="935ef-130">Felügyelt alkalmazások bemutatása, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="935ef-130">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="935ef-131">A bevezetést UI-definíciók létrehozásáról lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="935ef-131">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="935ef-132">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="935ef-132">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

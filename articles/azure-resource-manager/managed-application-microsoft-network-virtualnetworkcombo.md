---
title: "aaaAzure felügyelt alkalmazás VirtualNetworkCombo felhasználói felületi elem |} Microsoft Docs"
description: "Hello Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem ismerteti az Azure által felügyelt alkalmazások"
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
ms.openlocfilehash: 1b0fa5360d93306f7a814723f77e42540bdaaa9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a><span data-ttu-id="6a29d-103">Microsoft.Network.VirtualNetworkCombo felhasználói felületi elem</span><span class="sxs-lookup"><span data-stu-id="6a29d-103">Microsoft.Network.VirtualNetworkCombo UI element</span></span>
<span data-ttu-id="6a29d-104">Egy új vagy meglévő virtuális hálózat kiválasztásához vezérlők egy csoportja.</span><span class="sxs-lookup"><span data-stu-id="6a29d-104">A group of controls for selecting a new or existing virtual network.</span></span> <span data-ttu-id="6a29d-105">Ez az elem használata során [Azure által felügyelt alkalmazások létrehozására](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="6a29d-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="6a29d-106">Felhasználói felület minta</span><span class="sxs-lookup"><span data-stu-id="6a29d-106">UI sample</span></span>
![Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo.png)

- <span data-ttu-id="6a29d-108">Hello felső Drótvázdiagram, a hello felhasználói kiválasztotta az új virtuális hálózat, így hello felhasználói testre szabhatja az egyes alhálózatok nevét és címét előtag.</span><span class="sxs-lookup"><span data-stu-id="6a29d-108">In hello top wireframe, hello user has picked a new virtual network, so hello user can customize each subnet's name and address prefix.</span></span> <span data-ttu-id="6a29d-109">Alhálózatok konfigurálása ebben az esetben nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="6a29d-109">Configuring subnets in this case is optional.</span></span>
- <span data-ttu-id="6a29d-110">A hello alsó Drótvázdiagram hello felhasználói kiválasztotta a meglévő virtuális hálózat, a hello felhasználói kell társítani, minden alhálózati hello központi telepítési sablont megköveteli-e a tooan létező alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="6a29d-110">In hello bottom wireframe, hello user has picked an existing virtual network, so hello user must map each subnet hello deployment template requires tooan existing subnet.</span></span> <span data-ttu-id="6a29d-111">Alhálózatok konfigurálása ebben az esetben szükség.</span><span class="sxs-lookup"><span data-stu-id="6a29d-111">Configuring subnets in this case is required.</span></span>

## <a name="schema"></a><span data-ttu-id="6a29d-112">Séma</span><span class="sxs-lookup"><span data-stu-id="6a29d-112">Schema</span></span>
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

## <a name="remarks"></a><span data-ttu-id="6a29d-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6a29d-113">Remarks</span></span>
- <span data-ttu-id="6a29d-114">Ha meg van adva, hello első mozaikként, átfedés nélkül címelőtag méretű `defaultValue.addressPrefixSize` hello felhasználó az előfizetéshez létező virtuális hálózatok automatikusan alapján történik.</span><span class="sxs-lookup"><span data-stu-id="6a29d-114">If specified, hello first non-overlapping address prefix of size `defaultValue.addressPrefixSize` is determined automatically based on the existing virtual networks in hello user's subscription.</span></span>
- <span data-ttu-id="6a29d-115">az alapértelmezett érték hello `defaultValue.name` és `defaultValue.addressPrefixSize` van **null**.</span><span class="sxs-lookup"><span data-stu-id="6a29d-115">hello default value for `defaultValue.name` and `defaultValue.addressPrefixSize` is **null**.</span></span>
- <span data-ttu-id="6a29d-116">`constraints.minAddressPrefixSize`meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="6a29d-116">`constraints.minAddressPrefixSize` must be specified.</span></span> <span data-ttu-id="6a29d-117">Az-címtér kisebb, mint hello megadott érték nem lehet kiválasztani a meglévő virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="6a29d-117">Any existing virtual networks with an address space smaller than hello specified value are unavailable for selection.</span></span>
- <span data-ttu-id="6a29d-118">`subnets`meg kell adni, és `constraints.minAddressPrefixSize` meg kell adni az egyes alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="6a29d-118">`subnets` must be specified, and `constraints.minAddressPrefixSize` must be specified for each subnet.</span></span>
- <span data-ttu-id="6a29d-119">Amikor új virtuális hálózat létrehozása minden alhálózati cím előtag alapján van kiszámítva automatikusan hello virtuális hálózat címelőtagjához és a megfelelő `addressPrefixSize`.</span><span class="sxs-lookup"><span data-stu-id="6a29d-119">When creating a new virtual network, each subnet's address prefix is calculated automatically based on hello virtual network's address prefix and the respective `addressPrefixSize`.</span></span>
- <span data-ttu-id="6a29d-120">Ha egy meglévő virtuális hálózat, kisebb, mint a megfelelő alhálózatok `constraints.minAddressPrefixSize` kijelölés nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6a29d-120">When using an existing virtual network, any subnets smaller than the respective `constraints.minAddressPrefixSize` are unavailable for selection.</span></span> <span data-ttu-id="6a29d-121">Továbbá ha meg van adva, alhálózatok, amelyek nem tartalmaznak legalább `minAddressCount` elérhető címei nem lehet kiválasztani.</span><span class="sxs-lookup"><span data-stu-id="6a29d-121">Additionally, if specified, subnets that do not contain at least `minAddressCount` available addresses are unavailable for selection.</span></span>
<span data-ttu-id="6a29d-122">hello alapértelmezett értéke **0**.</span><span class="sxs-lookup"><span data-stu-id="6a29d-122">hello default value is **0**.</span></span> <span data-ttu-id="6a29d-123">amely a rendelkezésre álló címek hello tooensure összefüggő, adja meg **igaz** a `requireContiguousAddresses`.</span><span class="sxs-lookup"><span data-stu-id="6a29d-123">tooensure that hello available addresses are contiguous, specify **true** for `requireContiguousAddresses`.</span></span> <span data-ttu-id="6a29d-124">hello alapértelmezett értéke **igaz**.</span><span class="sxs-lookup"><span data-stu-id="6a29d-124">hello default value is **true**.</span></span>
- <span data-ttu-id="6a29d-125">A meglévő virtuális hálózat alhálózatok létrehozása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6a29d-125">Creating subnets in an existing virtual network is not supported.</span></span>
- <span data-ttu-id="6a29d-126">Ha `options.hideExisting` van **igaz**, hello felhasználói egy meglévő virtuális hálózat nem választható.</span><span class="sxs-lookup"><span data-stu-id="6a29d-126">If `options.hideExisting` is **true**, hello user can't choose an existing virtual network.</span></span> <span data-ttu-id="6a29d-127">hello alapértelmezett értéke **hamis**.</span><span class="sxs-lookup"><span data-stu-id="6a29d-127">hello default value is **false**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="6a29d-128">Példa kimenet</span><span class="sxs-lookup"><span data-stu-id="6a29d-128">Sample output</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="6a29d-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a29d-129">Next steps</span></span>
* <span data-ttu-id="6a29d-130">Egy bevezető toomanaged alkalmazások, lásd: [Azure kezelt alkalmazás – áttekintés](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a29d-130">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="6a29d-131">Egy bevezető toocreating UI-definíciók, lásd: [Ismerkedés a CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6a29d-131">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="6a29d-132">Általános tulajdonságok felhasználói felületi elemei ismertetését lásd: [CreateUiDefinition elemek](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="6a29d-132">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>

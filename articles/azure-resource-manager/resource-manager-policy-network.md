---
title: "A hálózati erőforrások az Azure erőforrás-házirendek |} Microsoft Docs"
description: "Azure Resource Manager házirendeket a központi telepítés, a hálózati erőforrások kezelését ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/05/2017
ms.author: tomfitz
ms.openlocfilehash: bca66bbdd9da9b3e4099d0d961f42c9368a17f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="apply-resource-policies-to-network-resources"></a><span data-ttu-id="2e34b-103">Erőforrás-házirendek alkalmazása a hálózati erőforrások</span><span class="sxs-lookup"><span data-stu-id="2e34b-103">Apply resource policies to network resources</span></span>
<span data-ttu-id="2e34b-104">Ez a cikk egy példát mutat be, [erőforrás házirend](resource-manager-policy.md) Azure virtuális hálózati átjárók alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="2e34b-104">This article shows an example [resource policy](resource-manager-policy.md) you can apply to Azure virtual network gateways.</span></span> <span data-ttu-id="2e34b-105">Ez a házirend a szervezetben telepített átjárók konzisztencia biztosítja.</span><span class="sxs-lookup"><span data-stu-id="2e34b-105">This policy ensures consistency for gateways deployed in your organization.</span></span> 

## <a name="define-permitted-virtual-network-gateway-sku"></a><span data-ttu-id="2e34b-106">Adja meg az engedélyezett virtuális hálózati átjáró Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="2e34b-106">Define permitted virtual network gateway SKU</span></span>

<span data-ttu-id="2e34b-107">A következő házirend korlátozza a virtuális hálózati átjárók mely termékváltozatok telepíthető:</span><span class="sxs-lookup"><span data-stu-id="2e34b-107">The following policy restricts which SKUs can be deployed for virtual network gateways:</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Network/virtualNetworkGateways"
      },
      {
        "not": {
          "field": "Microsoft.Network/virtualNetworkGateways/sku.name",
          "in": [
            "Basic",
            "VpnGw1"
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="2e34b-108">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e34b-108">Next steps</span></span>
* <span data-ttu-id="2e34b-109">(A fenti példákban szerint) házirend szabály megadása után kell a házirend-definíció létrehozása, és rendelje hozzá hatókör.</span><span class="sxs-lookup"><span data-stu-id="2e34b-109">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="2e34b-110">A hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="2e34b-110">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="2e34b-111">A portálon keresztül házirendek rendeléséhez lásd: [hozzárendelésére és kezelésére erőforrás-házirendek használata Azure-portálon](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2e34b-111">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="2e34b-112">REST API-t, a PowerShell vagy az Azure CLI-házirendeket rendeléséhez lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="2e34b-112">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="2e34b-113">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="2e34b-113">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


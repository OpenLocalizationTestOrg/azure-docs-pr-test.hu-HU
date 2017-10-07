---
title: "a hálózati erőforrások erőforrás-házirendek aaaAzure |} Microsoft Docs"
description: "Azure Resource Manager házirendek hello telepítési hálózati erőforrások kezelését ismerteti."
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
ms.openlocfilehash: a6072c1c30db0a4e4a1cae04efc7828d14069709
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-toonetwork-resources"></a><span data-ttu-id="61d25-103">Erőforrás-házirendek toonetwork erőforrások alkalmazása</span><span class="sxs-lookup"><span data-stu-id="61d25-103">Apply resource policies toonetwork resources</span></span>
<span data-ttu-id="61d25-104">Ez a cikk egy példát mutat be, [erőforrás házirend](resource-manager-policy.md) tooAzure virtuális hálózati átjárók is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="61d25-104">This article shows an example [resource policy](resource-manager-policy.md) you can apply tooAzure virtual network gateways.</span></span> <span data-ttu-id="61d25-105">Ez a házirend a szervezetben telepített átjárók konzisztencia biztosítja.</span><span class="sxs-lookup"><span data-stu-id="61d25-105">This policy ensures consistency for gateways deployed in your organization.</span></span> 

## <a name="define-permitted-virtual-network-gateway-sku"></a><span data-ttu-id="61d25-106">Adja meg az engedélyezett virtuális hálózati átjáró Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="61d25-106">Define permitted virtual network gateway SKU</span></span>

<span data-ttu-id="61d25-107">a következő házirend hello korlátozza, mely termékváltozatok telepíthető virtuális hálózati átjárók:</span><span class="sxs-lookup"><span data-stu-id="61d25-107">hello following policy restricts which SKUs can be deployed for virtual network gateways:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="61d25-108">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61d25-108">Next steps</span></span>
* <span data-ttu-id="61d25-109">(A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör.</span><span class="sxs-lookup"><span data-stu-id="61d25-109">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="61d25-110">hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="61d25-110">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="61d25-111">hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="61d25-111">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="61d25-112">REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="61d25-112">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="61d25-113">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="61d25-113">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


---
title: "erőforrás-házirendek aaaAzure storage-fiókok |} Microsoft Docs"
description: "Azure Resource Manager házirendek hello telepítési tárfiókok kezelését ismerteti."
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
ms.openlocfilehash: d37fc4bcf7cdec71b0e14f6231fc138bfb6a7893
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-toostorage-accounts"></a><span data-ttu-id="94725-103">Alkalmazza a házirendeket toostorage erőforrásfiókokhoz</span><span class="sxs-lookup"><span data-stu-id="94725-103">Apply resource policies toostorage accounts</span></span>
<span data-ttu-id="94725-104">Ez a témakör bemutatja a több [erőforrás-házirendek](resource-manager-policy.md) tooAzure storage-fiókok is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="94725-104">This topic shows several [resource policies](resource-manager-policy.md) you can apply tooAzure storage accounts.</span></span> <span data-ttu-id="94725-105">Ezek a házirendek biztosítják a konzisztenciát a szervezetben telepített hello storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="94725-105">These policies ensure consistency for hello storage accounts deployed in your organization.</span></span> 

## <a name="define-permitted-storage-account-types"></a><span data-ttu-id="94725-106">Engedélyezett tárolási fiók típusainak definiálása</span><span class="sxs-lookup"><span data-stu-id="94725-106">Define permitted storage account types</span></span>

<span data-ttu-id="94725-107">hello következő házirend korlátozza a amely [tárfióktípusokat](../storage/common/storage-redundancy.md) telepíthető:</span><span class="sxs-lookup"><span data-stu-id="94725-107">hello following policy restricts which [storage account types](../storage/common/storage-redundancy.md) can be deployed:</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/sku.name",
          "in": [
            "Standard_LRS",
            "Standard_GRS"
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

<span data-ttu-id="94725-108">Egy paraméterrel termékváltozatok engedélyezett hello elfogadása hasonló házirendszabály, beépített házirend-definíció érhető el.</span><span class="sxs-lookup"><span data-stu-id="94725-108">A similar policy rule with a parameter for accepting hello allowed SKUs is available as a built-in policy definition.</span></span> <span data-ttu-id="94725-109">beépített hello-házirendben engedélyezve van az erőforrás-azonosító hello `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span><span class="sxs-lookup"><span data-stu-id="94725-109">hello built-in policy has hello resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`.</span></span> 

## <a name="define-permitted-access-tier"></a><span data-ttu-id="94725-110">Engedélyezett hozzáférési szint meghatározása</span><span class="sxs-lookup"><span data-stu-id="94725-110">Define permitted access tier</span></span>

<span data-ttu-id="94725-111">hello következő szabályzat előírja hello típusú [hozzáférési szint](../storage/blobs/storage-blob-storage-tiers.md) storage-fiókok, amelyek adható meg:</span><span class="sxs-lookup"><span data-stu-id="94725-111">hello following policy specifies hello type of [access tier](../storage/blobs/storage-blob-storage-tiers.md) that can be specified for storage accounts:</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "field": "kind",
        "equals": "BlobStorage"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/accessTier",
          "equals": "cool"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="ensure-encryption-is-enabled"></a><span data-ttu-id="94725-112">Győződjön meg róla, engedélyezve van</span><span class="sxs-lookup"><span data-stu-id="94725-112">Ensure encryption is enabled</span></span>

<span data-ttu-id="94725-113">hello következő a házirendhez szükséges összes storage-fiókok tooenable [Storage szolgáltatás titkosítási](../storage/common/storage-service-encryption.md):</span><span class="sxs-lookup"><span data-stu-id="94725-113">hello following policy requires all storage accounts tooenable [Storage service encryption](../storage/common/storage-service-encryption.md):</span></span>

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "equals": "Microsoft.Storage/storageAccounts"
      },
      {
        "not": {
          "field": "Microsoft.Storage/storageAccounts/enableBlobEncryption",
          "equals": "true"
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

<span data-ttu-id="94725-114">A házirendszabály is rendelkezésre áll, mint az erőforrás-azonosító hello beépített házirend-definíció `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span><span class="sxs-lookup"><span data-stu-id="94725-114">This policy rule is also available as a built-in policy definition with hello resource ID of `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94725-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94725-115">Next steps</span></span>
* <span data-ttu-id="94725-116">(A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör.</span><span class="sxs-lookup"><span data-stu-id="94725-116">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="94725-117">hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="94725-117">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="94725-118">hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="94725-118">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="94725-119">REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="94725-119">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span> 
* <span data-ttu-id="94725-120">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="94725-120">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


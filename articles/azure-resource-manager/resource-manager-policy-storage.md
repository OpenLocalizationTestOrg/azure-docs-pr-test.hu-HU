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
# <a name="apply-resource-policies-toostorage-accounts"></a>Alkalmazza a házirendeket toostorage erőforrásfiókokhoz
Ez a témakör bemutatja a több [erőforrás-házirendek](resource-manager-policy.md) tooAzure storage-fiókok is alkalmazhat. Ezek a házirendek biztosítják a konzisztenciát a szervezetben telepített hello storage-fiókok. 

## <a name="define-permitted-storage-account-types"></a>Engedélyezett tárolási fiók típusainak definiálása

hello következő házirend korlátozza a amely [tárfióktípusokat](../storage/common/storage-redundancy.md) telepíthető:

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

Egy paraméterrel termékváltozatok engedélyezett hello elfogadása hasonló házirendszabály, beépített házirend-definíció érhető el. beépített hello-házirendben engedélyezve van az erőforrás-azonosító hello `/providers/Microsoft.Authorization/policyDefinitions/7433c107-6db4-4ad1-b57a-a76dce0154a1`. 

## <a name="define-permitted-access-tier"></a>Engedélyezett hozzáférési szint meghatározása

hello következő szabályzat előírja hello típusú [hozzáférési szint](../storage/blobs/storage-blob-storage-tiers.md) storage-fiókok, amelyek adható meg:

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

## <a name="ensure-encryption-is-enabled"></a>Győződjön meg róla, engedélyezve van

hello következő a házirendhez szükséges összes storage-fiókok tooenable [Storage szolgáltatás titkosítási](../storage/common/storage-service-encryption.md):

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

A házirendszabály is rendelkezésre áll, mint az erőforrás-azonosító hello beépített házirend-definíció `/providers/Microsoft.Authorization/policyDefinitions/7c5a74bf-ae94-4a74-8fcf-644d1e0e6e6f`.

## <a name="next-steps"></a>Következő lépések
* (A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör. hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás. hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md). REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md). 
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).


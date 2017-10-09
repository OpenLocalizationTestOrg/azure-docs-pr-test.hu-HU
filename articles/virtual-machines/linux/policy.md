---
title: "a házirendek az Azure Linux virtuális gépeken aaaEnforce biztonsági |} Microsoft Docs"
description: "Hogyan tooapply egy házirend tooan Azure Resource Manager Linux virtuális gép"
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: singhkay
ms.openlocfilehash: 5abd0c937578aba7e72b62c65b4eef9a9737aa2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-policies-toolinux-vms-with-azure-resource-manager"></a>Házirendek tooLinux virtuális gépeket az Azure Resource Manager alkalmazása
Házirendek segítségével a szervezetek kényszerítheti a különböző egyezmények és szabályok hello vállalaton belül. Szükséges hello viselkedés végrehajtását segítségével mérsékelhetik a kockázatokat toohello sikeres hello szervezet hozzájárul. Ez a cikk azt ismerteti hogyan használható az Azure Resource Manager házirendek toodefine szükséges hello viselkedését a szervezet virtuális gépekhez.

Egy bevezető toopolicies, lásd: [vezérlési hozzáférési és használati feltételei toomanage erőforrások](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Engedélyezett virtuális gépek
tooensure, hogy egy alkalmazás kompatibilisek legyenek-e a szervezet virtuális gépek, operációs rendszerek engedélyezett hello korlátozhatja. A házirend például a következő hello, csak az Ubuntu 14.04.2-LTS virtuális gépek engedélyezése toobe létrehozni.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
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

Egy házirend tooallow megelőző Ubuntu LTS képet helyettesítő toomodify hello használata: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

Házirend mezőkkel kapcsolatos információkért lásd: [házirend aliasok](../../azure-resource-manager/resource-manager-policy.md#aliases).

## <a name="managed-disks"></a>Felügyelt lemezek

toorequire hello használata felügyelt lemezek használatát hello házirendet a következő:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>A virtuális gépek lemezképek

Biztonsági okokból megkövetelheti, hogy csak a jóváhagyott egyéni lemezképek telepítve vannak-e a környezetében. Hello jóváhagyott lemezképeket tartalmazó erőforráscsoportot hello vagy hello adott jóváhagyott lemezképeket is megadhat.

a következő példa hello jóváhagyott erőforráscsoportból képek van szükség:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

hello alábbi példa meghatározza, hogy jóvá hello kép azonosítók:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Virtuálisgép-bővítmények

Érdemes lehet tooforbid használati bővítmények bizonyos típusú. Egy bővítmény például nem lehet kompatibilis bizonyos egyéni virtuálisgép-lemezképeket. a következő példa azt mutatja meg hogyan hello tooblock egy adott kiterjesztéssel. Gyártó és típus toodetermine mely bővítmény tooblock használ.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="next-steps"></a>Következő lépések
* (A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör. hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás. hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](../../azure-resource-manager/resource-manager-policy-portal.md). REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](../../azure-resource-manager/resource-manager-policy-create-assign.md).
* Egy bevezető tooresource házirendek, lásd: [erőforrás házirendek – áttekintés](../../azure-resource-manager/resource-manager-policy.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](../../azure-resource-manager/resource-manager-subscription-governance.md).

---
title: "a házirendek a Windows-alapú virtuális gépek Azure-ban aaaEnforce biztonsági |} Microsoft Docs"
description: "Hogyan tooapply egy házirend tooan Azure Resource Manager Windows rendszerű virtuális gép"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: kasing
ms.openlocfilehash: b31c8a03ecf8eed6a929f97fe4146ea14364404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-policies-toowindows-vms-with-azure-resource-manager"></a>Házirendek tooWindows virtuális gépeket az Azure Resource Manager alkalmazása
Házirendek segítségével a szervezetek kényszerítheti a különböző egyezmények és szabályok hello vállalaton belül. Szükséges hello viselkedés végrehajtását segítségével mérsékelhetik a kockázatokat toohello sikeres hello szervezet hozzájárul. Ez a cikk azt ismerteti hogyan használható az Azure Resource Manager házirendek toodefine szükséges hello viselkedését a szervezet virtuális gépekhez.

Egy bevezető toopolicies, lásd: [vezérlési hozzáférési és használati feltételei toomanage erőforrások](../../azure-resource-manager/resource-manager-policy.md).

## <a name="permitted-virtual-machines"></a>Engedélyezett virtuális gépek
tooensure, hogy egy alkalmazás kompatibilisek legyenek-e a szervezet virtuális gépek, operációs rendszerek engedélyezett hello korlátozhatja. A házirend például a következő hello csak a Windows Server 2012 R2 Datacenter virtuális gépek toobe létrehozott engedélyezése:

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
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-R2-Datacenter"
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

Egy házirend tooallow megelőző Windows Server Datacenter képet helyettesítő toomodify hello használata:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Házirend tooallow megelőző bármilyen Windows Server 2012 R2 Datacenter vagy magasabb kép anyOf toomodify hello használata:

```json
{
  "anyOf": [
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2012-R2-Datacenter*"
    },
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2016-Datacenter*"
    }
  ]
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


## <a name="azure-hybrid-use-benefit"></a>Azure hibrid használata juttatás

Ha a helyszíni licenccel rendelkezik, a virtuális gépek hello licenc díj is mentheti. Hello licenccel nem rendelkező, meg kell megtiltják hello lehetőséget. a következő házirend hello tiltja használati Azure hibrid használata juttatás (AHUB):

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in":[ "Microsoft.Compute/virtualMachines","Microsoft.Compute/VirtualMachineScaleSets"]
            },
            {
                "field": "Microsoft.Compute/licenseType",
                "exists": true
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

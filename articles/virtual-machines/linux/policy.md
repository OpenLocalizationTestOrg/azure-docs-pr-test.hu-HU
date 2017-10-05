---
title: "A házirendek az Azure Linux virtuális gépeken biztonság kényszerítése |} Microsoft Docs"
description: "Egy házirend alkalmazása az Azure Resource Manager Linux virtuális gépeket"
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
ms.openlocfilehash: 58eaab4fa03afc1e6a5e38bef691cce62a921ea9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a><span data-ttu-id="bafcf-103">Linux virtuális gépek az Azure Resource Manager-szabályzatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="bafcf-103">Apply policies to Linux VMs with Azure Resource Manager</span></span>
<span data-ttu-id="bafcf-104">Házirendek segítségével a szervezetek kényszerítheti a különböző egyezmények és szabályok a vállalaton belül.</span><span class="sxs-lookup"><span data-stu-id="bafcf-104">By using policies, an organization can enforce various conventions and rules throughout the enterprise.</span></span> <span data-ttu-id="bafcf-105">A kívánt viselkedés végrehajtását segítségével mérsékelhetik a kockázatokat hozzájárul a szervezet sikeres.</span><span class="sxs-lookup"><span data-stu-id="bafcf-105">Enforcement of the desired behavior can help mitigate risk while contributing to the success of the organization.</span></span> <span data-ttu-id="bafcf-106">Ez a cikk azt ismerteti használatát Azure Resource Manager-házirendek megadhatók a kívánt viselkedés a szervezet virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="bafcf-106">In this article, we describe how you can use Azure Resource Manager policies to define the desired behavior for your organization's Virtual Machines.</span></span>

<span data-ttu-id="bafcf-107">Házirendek bemutatása, lásd: [kezelheti az erőforrásokat, és hozzáférés szabályozása házirendekkel](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="bafcf-107">For an introduction to policies, see [Use Policy to manage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="bafcf-108">Engedélyezett virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="bafcf-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="bafcf-109">Győződjön meg arról, hogy a szervezet virtuális gépek kompatibilisek egy alkalmazás, korlátozhatja az engedélyezett operációs rendszerek.</span><span class="sxs-lookup"><span data-stu-id="bafcf-109">To ensure that virtual machines for your organization are compatible with an application, you can restrict the permitted operating systems.</span></span> <span data-ttu-id="bafcf-110">A következő házirend-példa engedélyezése csak Ubuntu 14.04.2-LTS virtuális gépeket létrehozni.</span><span class="sxs-lookup"><span data-stu-id="bafcf-110">In the following policy example, you allow only Ubuntu 14.04.2-LTS Virtual Machines to be created.</span></span>

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

<span data-ttu-id="bafcf-111">Egy helyettesítő karakter használatával módosíthatja az előző házirendben engedélyezze az Ubuntu LTS képet:</span><span class="sxs-lookup"><span data-stu-id="bafcf-111">Use a wild card to modify the preceding policy to allow any Ubuntu LTS image:</span></span> 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

<span data-ttu-id="bafcf-112">Házirend mezőkkel kapcsolatos információkért lásd: [házirend aliasok](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="bafcf-112">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="bafcf-113">Felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="bafcf-113">Managed disks</span></span>

<span data-ttu-id="bafcf-114">Felügyelt lemezek használata szükséges, használja a következő házirendet:</span><span class="sxs-lookup"><span data-stu-id="bafcf-114">To require the use of managed disks, use the following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="bafcf-115">A virtuális gépek lemezképek</span><span class="sxs-lookup"><span data-stu-id="bafcf-115">Images for Virtual Machines</span></span>

<span data-ttu-id="bafcf-116">Biztonsági okokból megkövetelheti, hogy csak a jóváhagyott egyéni lemezképek telepítve vannak-e a környezetében.</span><span class="sxs-lookup"><span data-stu-id="bafcf-116">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="bafcf-117">Vagy az erőforráscsoport, amely tartalmazza a jóváhagyott lemezképeket is megadhat, vagy a specifikus jóváhagyott lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="bafcf-117">You can specify either the resource group that contains the approved images, or the specific approved images.</span></span>

<span data-ttu-id="bafcf-118">A következő példa egy jóváhagyott erőforráscsoportból képek van szükség:</span><span class="sxs-lookup"><span data-stu-id="bafcf-118">The following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="bafcf-119">A következő példa meghatározza, hogy a jóváhagyott lemezkép-azonosítók:</span><span class="sxs-lookup"><span data-stu-id="bafcf-119">The following example specifies the approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="bafcf-120">Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="bafcf-120">Virtual Machine extensions</span></span>

<span data-ttu-id="bafcf-121">Érdemes lehet megtiltják bizonyos típusú bővítmények használatát.</span><span class="sxs-lookup"><span data-stu-id="bafcf-121">You may want to forbid usage of certain types of extensions.</span></span> <span data-ttu-id="bafcf-122">Egy bővítmény például nem lehet kompatibilis bizonyos egyéni virtuálisgép-lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="bafcf-122">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="bafcf-123">A következő példa bemutatja, hogyan blokkolja egy adott kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="bafcf-123">The following example shows how to block a specific extension.</span></span> <span data-ttu-id="bafcf-124">Gyártó és típus használatával határozza meg, melyik bővítmény letiltása.</span><span class="sxs-lookup"><span data-stu-id="bafcf-124">It uses publisher and type to determine which extension to block.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="bafcf-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bafcf-125">Next steps</span></span>
* <span data-ttu-id="bafcf-126">(A fenti példákban szerint) házirend szabály megadása után kell a házirend-definíció létrehozása, és rendelje hozzá hatókör.</span><span class="sxs-lookup"><span data-stu-id="bafcf-126">After defining a policy rule (as shown in the preceding examples), you need to create the policy definition and assign it to a scope.</span></span> <span data-ttu-id="bafcf-127">A hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bafcf-127">The scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="bafcf-128">A portálon keresztül házirendek rendeléséhez lásd: [hozzárendelésére és kezelésére erőforrás-házirendek használata Azure-portálon](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bafcf-128">To assign policies through the portal, see [Use Azure portal to assign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="bafcf-129">REST API-t, a PowerShell vagy az Azure CLI-házirendeket rendeléséhez lásd: [meg és kezelheti a parancsfájl-házirendeket](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="bafcf-129">To assign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="bafcf-130">Megismerkedhet az erőforrás-házirendek, lásd: [erőforrás házirendek – áttekintés](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="bafcf-130">For an introduction to resource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="bafcf-131">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="bafcf-131">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>

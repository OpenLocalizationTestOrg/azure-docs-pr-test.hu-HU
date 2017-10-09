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
# <a name="apply-policies-toolinux-vms-with-azure-resource-manager"></a><span data-ttu-id="33ff7-103">Házirendek tooLinux virtuális gépeket az Azure Resource Manager alkalmazása</span><span class="sxs-lookup"><span data-stu-id="33ff7-103">Apply policies tooLinux VMs with Azure Resource Manager</span></span>
<span data-ttu-id="33ff7-104">Házirendek segítségével a szervezetek kényszerítheti a különböző egyezmények és szabályok hello vállalaton belül.</span><span class="sxs-lookup"><span data-stu-id="33ff7-104">By using policies, an organization can enforce various conventions and rules throughout hello enterprise.</span></span> <span data-ttu-id="33ff7-105">Szükséges hello viselkedés végrehajtását segítségével mérsékelhetik a kockázatokat toohello sikeres hello szervezet hozzájárul.</span><span class="sxs-lookup"><span data-stu-id="33ff7-105">Enforcement of hello desired behavior can help mitigate risk while contributing toohello success of hello organization.</span></span> <span data-ttu-id="33ff7-106">Ez a cikk azt ismerteti hogyan használható az Azure Resource Manager házirendek toodefine szükséges hello viselkedését a szervezet virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="33ff7-106">In this article, we describe how you can use Azure Resource Manager policies toodefine hello desired behavior for your organization's Virtual Machines.</span></span>

<span data-ttu-id="33ff7-107">Egy bevezető toopolicies, lásd: [vezérlési hozzáférési és használati feltételei toomanage erőforrások](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="33ff7-107">For an introduction toopolicies, see [Use Policy toomanage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="33ff7-108">Engedélyezett virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="33ff7-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="33ff7-109">tooensure, hogy egy alkalmazás kompatibilisek legyenek-e a szervezet virtuális gépek, operációs rendszerek engedélyezett hello korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="33ff7-109">tooensure that virtual machines for your organization are compatible with an application, you can restrict hello permitted operating systems.</span></span> <span data-ttu-id="33ff7-110">A házirend például a következő hello, csak az Ubuntu 14.04.2-LTS virtuális gépek engedélyezése toobe létrehozni.</span><span class="sxs-lookup"><span data-stu-id="33ff7-110">In hello following policy example, you allow only Ubuntu 14.04.2-LTS Virtual Machines toobe created.</span></span>

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

<span data-ttu-id="33ff7-111">Egy házirend tooallow megelőző Ubuntu LTS képet helyettesítő toomodify hello használata:</span><span class="sxs-lookup"><span data-stu-id="33ff7-111">Use a wild card toomodify hello preceding policy tooallow any Ubuntu LTS image:</span></span> 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
}
```

<span data-ttu-id="33ff7-112">Házirend mezőkkel kapcsolatos információkért lásd: [házirend aliasok](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="33ff7-112">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="33ff7-113">Felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="33ff7-113">Managed disks</span></span>

<span data-ttu-id="33ff7-114">toorequire hello használata felügyelt lemezek használatát hello házirendet a következő:</span><span class="sxs-lookup"><span data-stu-id="33ff7-114">toorequire hello use of managed disks, use hello following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="33ff7-115">A virtuális gépek lemezképek</span><span class="sxs-lookup"><span data-stu-id="33ff7-115">Images for Virtual Machines</span></span>

<span data-ttu-id="33ff7-116">Biztonsági okokból megkövetelheti, hogy csak a jóváhagyott egyéni lemezképek telepítve vannak-e a környezetében.</span><span class="sxs-lookup"><span data-stu-id="33ff7-116">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="33ff7-117">Hello jóváhagyott lemezképeket tartalmazó erőforráscsoportot hello vagy hello adott jóváhagyott lemezképeket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="33ff7-117">You can specify either hello resource group that contains hello approved images, or hello specific approved images.</span></span>

<span data-ttu-id="33ff7-118">a következő példa hello jóváhagyott erőforráscsoportból képek van szükség:</span><span class="sxs-lookup"><span data-stu-id="33ff7-118">hello following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="33ff7-119">hello alábbi példa meghatározza, hogy jóvá hello kép azonosítók:</span><span class="sxs-lookup"><span data-stu-id="33ff7-119">hello following example specifies hello approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="33ff7-120">Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="33ff7-120">Virtual Machine extensions</span></span>

<span data-ttu-id="33ff7-121">Érdemes lehet tooforbid használati bővítmények bizonyos típusú.</span><span class="sxs-lookup"><span data-stu-id="33ff7-121">You may want tooforbid usage of certain types of extensions.</span></span> <span data-ttu-id="33ff7-122">Egy bővítmény például nem lehet kompatibilis bizonyos egyéni virtuálisgép-lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="33ff7-122">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="33ff7-123">a következő példa azt mutatja meg hogyan hello tooblock egy adott kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="33ff7-123">hello following example shows how tooblock a specific extension.</span></span> <span data-ttu-id="33ff7-124">Gyártó és típus toodetermine mely bővítmény tooblock használ.</span><span class="sxs-lookup"><span data-stu-id="33ff7-124">It uses publisher and type toodetermine which extension tooblock.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="33ff7-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33ff7-125">Next steps</span></span>
* <span data-ttu-id="33ff7-126">(A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör.</span><span class="sxs-lookup"><span data-stu-id="33ff7-126">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="33ff7-127">hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="33ff7-127">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="33ff7-128">hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="33ff7-128">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="33ff7-129">REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="33ff7-129">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="33ff7-130">Egy bevezető tooresource házirendek, lásd: [erőforrás házirendek – áttekintés](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="33ff7-130">For an introduction tooresource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="33ff7-131">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="33ff7-131">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>

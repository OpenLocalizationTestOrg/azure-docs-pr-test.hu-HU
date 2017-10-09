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
# <a name="apply-policies-toowindows-vms-with-azure-resource-manager"></a><span data-ttu-id="e7679-103">Házirendek tooWindows virtuális gépeket az Azure Resource Manager alkalmazása</span><span class="sxs-lookup"><span data-stu-id="e7679-103">Apply policies tooWindows VMs with Azure Resource Manager</span></span>
<span data-ttu-id="e7679-104">Házirendek segítségével a szervezetek kényszerítheti a különböző egyezmények és szabályok hello vállalaton belül.</span><span class="sxs-lookup"><span data-stu-id="e7679-104">By using policies, an organization can enforce various conventions and rules throughout hello enterprise.</span></span> <span data-ttu-id="e7679-105">Szükséges hello viselkedés végrehajtását segítségével mérsékelhetik a kockázatokat toohello sikeres hello szervezet hozzájárul.</span><span class="sxs-lookup"><span data-stu-id="e7679-105">Enforcement of hello desired behavior can help mitigate risk while contributing toohello success of hello organization.</span></span> <span data-ttu-id="e7679-106">Ez a cikk azt ismerteti hogyan használható az Azure Resource Manager házirendek toodefine szükséges hello viselkedését a szervezet virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="e7679-106">In this article, we describe how you can use Azure Resource Manager policies toodefine hello desired behavior for your organization’s Virtual Machines.</span></span>

<span data-ttu-id="e7679-107">Egy bevezető toopolicies, lásd: [vezérlési hozzáférési és használati feltételei toomanage erőforrások](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="e7679-107">For an introduction toopolicies, see [Use Policy toomanage resources and control access](../../azure-resource-manager/resource-manager-policy.md).</span></span>

## <a name="permitted-virtual-machines"></a><span data-ttu-id="e7679-108">Engedélyezett virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e7679-108">Permitted Virtual Machines</span></span>
<span data-ttu-id="e7679-109">tooensure, hogy egy alkalmazás kompatibilisek legyenek-e a szervezet virtuális gépek, operációs rendszerek engedélyezett hello korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="e7679-109">tooensure that virtual machines for your organization are compatible with an application, you can restrict hello permitted operating systems.</span></span> <span data-ttu-id="e7679-110">A házirend például a következő hello csak a Windows Server 2012 R2 Datacenter virtuális gépek toobe létrehozott engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="e7679-110">In hello following policy example, you allow only Windows Server 2012 R2 Datacenter Virtual Machines toobe created:</span></span>

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

<span data-ttu-id="e7679-111">Egy házirend tooallow megelőző Windows Server Datacenter képet helyettesítő toomodify hello használata:</span><span class="sxs-lookup"><span data-stu-id="e7679-111">Use a wild card toomodify hello preceding policy tooallow any Windows Server Datacenter image:</span></span>

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

<span data-ttu-id="e7679-112">Házirend tooallow megelőző bármilyen Windows Server 2012 R2 Datacenter vagy magasabb kép anyOf toomodify hello használata:</span><span class="sxs-lookup"><span data-stu-id="e7679-112">Use anyOf toomodify hello preceding policy tooallow any Windows Server 2012 R2 Datacenter or higher image:</span></span>

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

<span data-ttu-id="e7679-113">Házirend mezőkkel kapcsolatos információkért lásd: [házirend aliasok](../../azure-resource-manager/resource-manager-policy.md#aliases).</span><span class="sxs-lookup"><span data-stu-id="e7679-113">For information about policy fields, see [Policy aliases](../../azure-resource-manager/resource-manager-policy.md#aliases).</span></span>

## <a name="managed-disks"></a><span data-ttu-id="e7679-114">Felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="e7679-114">Managed disks</span></span>

<span data-ttu-id="e7679-115">toorequire hello használata felügyelt lemezek használatát hello házirendet a következő:</span><span class="sxs-lookup"><span data-stu-id="e7679-115">toorequire hello use of managed disks, use hello following policy:</span></span>

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

## <a name="images-for-virtual-machines"></a><span data-ttu-id="e7679-116">A virtuális gépek lemezképek</span><span class="sxs-lookup"><span data-stu-id="e7679-116">Images for Virtual Machines</span></span>

<span data-ttu-id="e7679-117">Biztonsági okokból megkövetelheti, hogy csak a jóváhagyott egyéni lemezképek telepítve vannak-e a környezetében.</span><span class="sxs-lookup"><span data-stu-id="e7679-117">For security reasons, you can require that only approved custom images are deployed in your environment.</span></span> <span data-ttu-id="e7679-118">Hello jóváhagyott lemezképeket tartalmazó erőforráscsoportot hello vagy hello adott jóváhagyott lemezképeket is megadhat.</span><span class="sxs-lookup"><span data-stu-id="e7679-118">You can specify either hello resource group that contains hello approved images, or hello specific approved images.</span></span>

<span data-ttu-id="e7679-119">a következő példa hello jóváhagyott erőforráscsoportból képek van szükség:</span><span class="sxs-lookup"><span data-stu-id="e7679-119">hello following example requires images from an approved resource group:</span></span>

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

<span data-ttu-id="e7679-120">hello alábbi példa meghatározza, hogy jóvá hello kép azonosítók:</span><span class="sxs-lookup"><span data-stu-id="e7679-120">hello following example specifies hello approved image IDs:</span></span>

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a><span data-ttu-id="e7679-121">Virtuálisgép-bővítmények</span><span class="sxs-lookup"><span data-stu-id="e7679-121">Virtual Machine extensions</span></span>

<span data-ttu-id="e7679-122">Érdemes lehet tooforbid használati bővítmények bizonyos típusú.</span><span class="sxs-lookup"><span data-stu-id="e7679-122">You may want tooforbid usage of certain types of extensions.</span></span> <span data-ttu-id="e7679-123">Egy bővítmény például nem lehet kompatibilis bizonyos egyéni virtuálisgép-lemezképeket.</span><span class="sxs-lookup"><span data-stu-id="e7679-123">For example, an extension may not be compatible with certain custom virtual machine images.</span></span> <span data-ttu-id="e7679-124">a következő példa azt mutatja meg hogyan hello tooblock egy adott kiterjesztéssel.</span><span class="sxs-lookup"><span data-stu-id="e7679-124">hello following example shows how tooblock a specific extension.</span></span> <span data-ttu-id="e7679-125">Gyártó és típus toodetermine mely bővítmény tooblock használ.</span><span class="sxs-lookup"><span data-stu-id="e7679-125">It uses publisher and type toodetermine which extension tooblock.</span></span>

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


## <a name="azure-hybrid-use-benefit"></a><span data-ttu-id="e7679-126">Azure hibrid használata juttatás</span><span class="sxs-lookup"><span data-stu-id="e7679-126">Azure Hybrid Use Benefit</span></span>

<span data-ttu-id="e7679-127">Ha a helyszíni licenccel rendelkezik, a virtuális gépek hello licenc díj is mentheti.</span><span class="sxs-lookup"><span data-stu-id="e7679-127">When you have an on-premise license, you can save hello license fee on your virtual machines.</span></span> <span data-ttu-id="e7679-128">Hello licenccel nem rendelkező, meg kell megtiltják hello lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e7679-128">When you don't have hello license, you should forbid hello option.</span></span> <span data-ttu-id="e7679-129">a következő házirend hello tiltja használati Azure hibrid használata juttatás (AHUB):</span><span class="sxs-lookup"><span data-stu-id="e7679-129">hello following policy forbids usage of Azure Hybrid Use Benefit (AHUB):</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e7679-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7679-130">Next steps</span></span>
* <span data-ttu-id="e7679-131">(A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör.</span><span class="sxs-lookup"><span data-stu-id="e7679-131">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="e7679-132">hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e7679-132">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="e7679-133">hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](../../azure-resource-manager/resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e7679-133">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](../../azure-resource-manager/resource-manager-policy-portal.md).</span></span> <span data-ttu-id="e7679-134">REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="e7679-134">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](../../azure-resource-manager/resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="e7679-135">Egy bevezető tooresource házirendek, lásd: [erőforrás házirendek – áttekintés](../../azure-resource-manager/resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="e7679-135">For an introduction tooresource policies, see [Resource policy overview](../../azure-resource-manager/resource-manager-policy.md).</span></span>
* <span data-ttu-id="e7679-136">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](../../azure-resource-manager/resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="e7679-136">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](../../azure-resource-manager/resource-manager-subscription-governance.md).</span></span>

---
title: "erőforrás-házirendek aaaAzure címkék |} Microsoft Docs"
description: "Erőforrás-házirendekre talál példákat nyújt címkéket az erőforrások kezelése"
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
ms.date: 08/24/2017
ms.author: tomfitz
ms.openlocfilehash: 5a5b3d5ed52b47544b397694b9da0070f61b1faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apply-resource-policies-for-tags"></a><span data-ttu-id="cb681-103">Címkék erőforrás-szabályzatok alkalmazása</span><span class="sxs-lookup"><span data-stu-id="cb681-103">Apply resource policies for tags</span></span>

<span data-ttu-id="cb681-104">Ez a témakör általános házirend szabályok tooensure következetes használatával címkéket alkalmazhatja erőforrások.</span><span class="sxs-lookup"><span data-stu-id="cb681-104">This topic provides common policy rules you can apply tooensure consistent use of tags on resources.</span></span>

<span data-ttu-id="cb681-105">Egy címke házirend tooa erőforráscsoport vagy a meglévő erőforrásokkal folytatott előfizetés alkalmazása visszamenőleges vonatkozik hello házirend toothose erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cb681-105">Applying a tag policy tooa resource group or subscription with existing resources does not retroactively apply hello policy toothose resources.</span></span> <span data-ttu-id="cb681-106">tooenforce hello házirendeket az olyan erőforrásokhoz, egy meglévő erőforrások frissítés toohello indítható el.</span><span class="sxs-lookup"><span data-stu-id="cb681-106">tooenforce hello policies on those resources, trigger an update toohello existing resources.</span></span> <span data-ttu-id="cb681-107">A cikk egy PowerShell-példa indítására frissítést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cb681-107">This article includes a PowerShell example for triggering an update.</span></span>

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a><span data-ttu-id="cb681-108">Győződjön meg arról, összes erőforrást erőforráscsoportban kell egy címke</span><span class="sxs-lookup"><span data-stu-id="cb681-108">Ensure all resources in a resource group have a tag/value</span></span>

<span data-ttu-id="cb681-109">Általános követelmény, egy erőforráscsoportban található összes erőforrást rendelkezik-e egy adott címke és értéket.</span><span class="sxs-lookup"><span data-stu-id="cb681-109">A common requirement is that all resources in a resource group have a particular tag and value.</span></span> <span data-ttu-id="cb681-110">Ez a követelmény legtöbbször szükséges tootrack költségek részleg által.</span><span class="sxs-lookup"><span data-stu-id="cb681-110">This requirement is often needed tootrack costs by department.</span></span> <span data-ttu-id="cb681-111">hello a következő feltételeknek kell teljesülniük:</span><span class="sxs-lookup"><span data-stu-id="cb681-111">hello following conditions must be met:</span></span>

* <span data-ttu-id="cb681-112">hello címke értéke hozzáfűzött toonew és erőforrásokat, amelyek nem rendelkeznek hello címke frissítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb681-112">hello required tag and value are appended toonew and updated resources that do not have hello tag.</span></span>
* <span data-ttu-id="cb681-113">hello szükséges címkét, és érték nem lehet eltávolítani a meglévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cb681-113">hello required tag and value cannot be removed from any existing resources.</span></span>

<span data-ttu-id="cb681-114">Két beépített házirendek tooa erőforráscsoport alkalmazásával megfelelhet ennek a követelménynek.</span><span class="sxs-lookup"><span data-stu-id="cb681-114">You accomplish this requirement by applying two built-in policies tooa resource group.</span></span>

| <span data-ttu-id="cb681-115">ID (Azonosító)</span><span class="sxs-lookup"><span data-stu-id="cb681-115">ID</span></span> | <span data-ttu-id="cb681-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb681-116">Description</span></span> |
| ---- | ---- |
| <span data-ttu-id="cb681-117">2a0e14a6-b0a6-4fab-991a-187a4f81c498</span><span class="sxs-lookup"><span data-stu-id="cb681-117">2a0e14a6-b0a6-4fab-991a-187a4f81c498</span></span> | <span data-ttu-id="cb681-118">Egy szükséges kód és az alapértelmezett érték érvényes hello felhasználó nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="cb681-118">Applies a required tag and its default value when it is not specified by hello user.</span></span> |
| <span data-ttu-id="cb681-119">1e30110a-5ceb-460c-a204-c1c3969c6d62</span><span class="sxs-lookup"><span data-stu-id="cb681-119">1e30110a-5ceb-460c-a204-c1c3969c6d62</span></span> | <span data-ttu-id="cb681-120">Kikényszeríti egy szükséges kód és az értéke.</span><span class="sxs-lookup"><span data-stu-id="cb681-120">Enforces a required tag and its value.</span></span> |

### <a name="powershell"></a><span data-ttu-id="cb681-121">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cb681-121">PowerShell</span></span>

<span data-ttu-id="cb681-122">a következő PowerShell-parancsfájl hello hello két beépített házirend definíciók tooa erőforráscsoport rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="cb681-122">hello following PowerShell script assigns hello two built-in policy definitions tooa resource group.</span></span> <span data-ttu-id="cb681-123">Hello parancsprogram futtatása előtt rendelje hozzá az összes szükséges címkéket toohello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="cb681-123">Before running hello script, assign all required tags toohello resource group.</span></span> <span data-ttu-id="cb681-124">Minden címke hello erőforráscsoport hello erőforrások hello csoport szükség.</span><span class="sxs-lookup"><span data-stu-id="cb681-124">Each tag on hello resource group is required for hello resources in hello group.</span></span> <span data-ttu-id="cb681-125">az előfizetés tooassign tooall erőforráscsoport-sablonok nem nyújtanak hello `-Name` paraméter hello erőforráscsoportok beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="cb681-125">tooassign tooall resource groups in your subscription, do not provide hello `-Name` parameter when getting hello resource groups.</span></span>

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

<span data-ttu-id="cb681-126">Hello szabályzatok hozzárendelését követően egy frissítés tooall erőforrások tooenforce hello címke házirendek hozzáadása meglévő indíthat el.</span><span class="sxs-lookup"><span data-stu-id="cb681-126">After assigning hello policies, you can trigger an update tooall existing resources tooenforce hello tag policies you have added.</span></span> <span data-ttu-id="cb681-127">hello következő parancsfájl megőrzi más címkék, hogy hello erőforrástól függ:</span><span class="sxs-lookup"><span data-stu-id="cb681-127">hello following script retains any other tags that existed on hello resources:</span></span>

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="require-tags-for-a-resource-type"></a><span data-ttu-id="cb681-128">Címkék kérése erőforrástípus</span><span class="sxs-lookup"><span data-stu-id="cb681-128">Require tags for a resource type</span></span>
<span data-ttu-id="cb681-129">hello a következő példa bemutatja, hogyan toonest logikai operátorok toorequire alkalmazás címke csak a megadott erőforrástípus (az ebben az esetben storage-fiókok).</span><span class="sxs-lookup"><span data-stu-id="cb681-129">hello following example shows how toonest logical operators toorequire an application tag for only a specified resource type (in this case, storage accounts).</span></span>

```json
{
    "if": {
        "allOf": [
          {
            "not": {
              "field": "tags",
              "containsKey": "application"
            }
          },
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a><span data-ttu-id="cb681-130">Címke megkövetelése</span><span class="sxs-lookup"><span data-stu-id="cb681-130">Require tag</span></span>
<span data-ttu-id="cb681-131">hello következő házirend megtagadja, amelyek nem rendelkeznek a címke (minden érték alkalmazható) "costCenter" kulcsot tartalmazó:</span><span class="sxs-lookup"><span data-stu-id="cb681-131">hello following policy denies requests that don't have a tag containing "costCenter" key (any value can be applied):</span></span>

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="next-steps"></a><span data-ttu-id="cb681-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb681-132">Next steps</span></span>
* <span data-ttu-id="cb681-133">(A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör.</span><span class="sxs-lookup"><span data-stu-id="cb681-133">After defining a policy rule (as shown in hello preceding examples), you need toocreate hello policy definition and assign it tooa scope.</span></span> <span data-ttu-id="cb681-134">hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="cb681-134">hello scope can be a subscription, resource group, or resource.</span></span> <span data-ttu-id="cb681-135">hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cb681-135">tooassign policies through hello portal, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="cb681-136">REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).</span><span class="sxs-lookup"><span data-stu-id="cb681-136">tooassign policies through REST API, PowerShell or Azure CLI, see [Assign and manage policies through script](resource-manager-policy-create-assign.md).</span></span>
* <span data-ttu-id="cb681-137">Egy bevezető tooresource házirendek, lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="cb681-137">For an introduction tooresource policies, see [Resource policy overview](resource-manager-policy.md).</span></span>
* <span data-ttu-id="cb681-138">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="cb681-138">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


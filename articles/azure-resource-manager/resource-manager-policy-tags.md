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
# <a name="apply-resource-policies-for-tags"></a>Címkék erőforrás-szabályzatok alkalmazása

Ez a témakör általános házirend szabályok tooensure következetes használatával címkéket alkalmazhatja erőforrások.

Egy címke házirend tooa erőforráscsoport vagy a meglévő erőforrásokkal folytatott előfizetés alkalmazása visszamenőleges vonatkozik hello házirend toothose erőforrásokat. tooenforce hello házirendeket az olyan erőforrásokhoz, egy meglévő erőforrások frissítés toohello indítható el. A cikk egy PowerShell-példa indítására frissítést tartalmaz.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Győződjön meg arról, összes erőforrást erőforráscsoportban kell egy címke

Általános követelmény, egy erőforráscsoportban található összes erőforrást rendelkezik-e egy adott címke és értéket. Ez a követelmény legtöbbször szükséges tootrack költségek részleg által. hello a következő feltételeknek kell teljesülniük:

* hello címke értéke hozzáfűzött toonew és erőforrásokat, amelyek nem rendelkeznek hello címke frissítése szükséges.
* hello szükséges címkét, és érték nem lehet eltávolítani a meglévő erőforrásokat.

Két beépített házirendek tooa erőforráscsoport alkalmazásával megfelelhet ennek a követelménynek.

| ID (Azonosító) | Leírás |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Egy szükséges kód és az alapértelmezett érték érvényes hello felhasználó nincs megadva. |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Kikényszeríti egy szükséges kód és az értéke. |

### <a name="powershell"></a>PowerShell

a következő PowerShell-parancsfájl hello hello két beépített házirend definíciók tooa erőforráscsoport rendeli hozzá. Hello parancsprogram futtatása előtt rendelje hozzá az összes szükséges címkéket toohello erőforráscsoportot. Minden címke hello erőforráscsoport hello erőforrások hello csoport szükség. az előfizetés tooassign tooall erőforráscsoport-sablonok nem nyújtanak hello `-Name` paraméter hello erőforráscsoportok beolvasásakor.

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

Hello szabályzatok hozzárendelését követően egy frissítés tooall erőforrások tooenforce hello címke házirendek hozzáadása meglévő indíthat el. hello következő parancsfájl megőrzi más címkék, hogy hello erőforrástól függ:

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

## <a name="require-tags-for-a-resource-type"></a>Címkék kérése erőforrástípus
hello a következő példa bemutatja, hogyan toonest logikai operátorok toorequire alkalmazás címke csak a megadott erőforrástípus (az ebben az esetben storage-fiókok).

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

## <a name="require-tag"></a>Címke megkövetelése
hello következő házirend megtagadja, amelyek nem rendelkeznek a címke (minden érték alkalmazható) "costCenter" kulcsot tartalmazó:

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

## <a name="next-steps"></a>Következő lépések
* (A fenti példák hello) házirend szabály megadása után kell toocreate hello házirend-definíció, és rendelje hozzá tooa hatókör. hello hatókör lehet egy előfizetés, az erőforráscsoportot, vagy az erőforrás. hello portálon keresztül tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md). REST API-t, a PowerShell vagy Azure CLI tooassign házirendek, lásd: [meg és kezelheti a parancsfájl-házirendeket](resource-manager-policy-create-assign.md).
* Egy bevezető tooresource házirendek, lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).


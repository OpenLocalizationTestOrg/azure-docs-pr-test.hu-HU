---
title: "aaaAssign és az Azure erőforrás-házirendek kezelése |} Microsoft Docs"
description: "Ismerteti, hogyan tooapply Azure erőforráscsoport házirendek toosubscriptions és erőforrás-sablonok, és hogyan tooview erőforrás-házirendekkel."
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
ms.date: 07/26/2017
ms.author: tomfitz
ms.openlocfilehash: b6999b43bbcc80d2fde9911352fd4352fa453443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-and-manage-resource-policies"></a>Rendelje hozzá, és erőforrás-házirendek kezelése

egy házirend tooimplement, végezze el ezeket a lépéseket:

1. Ellenőrizze a házirend-definíciók (beleértve az Azure által biztosított beépített házirendek) toosee, ha már létezik ilyen az előfizetés, amely megfelel a követelményeknek.
2. Ha van ilyen, annak neve olvasható.
3. Ha még nem létezik, határozza meg a JSON hello házirendszabály, és adja hozzá az előfizetés a házirend-definíció. Ez a lépés hatására hello házirend rendelhető hozzá, de nem alkalmazza a hello szabályok tooyour előfizetés.
4. Mindkét esetben rendelni hello házirend tooa hatáskörüket (például egy előfizetés vagy az erőforrás csoportot). hello szabályok hello házirend kényszerítése megtörtént.

Ez a cikk egy házirend-definíció hello lépéseket toocreate összpontosít, és rendelje hozzá a REST API-t, a PowerShell vagy az Azure CLI tooa definition hatókörnek. Ha jobban szeret toouse hello portál tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md). Ez a cikk nem hello házirend-definíció létrehozása hello szintaxisának összpontosítanak. Házirendet a szintaxissal kapcsolatos információkért lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).

## <a name="rest-api"></a>REST API

### <a name="create-policy-definition"></a>Házirend-definíció létrehozása

Létrehozhat egy házirendet hello [REST API-t a házirend-definíciók](/rest/api/resources/policydefinitions). hello REST API lehetővé teszi a toocreate és törlése, házirend-definíciók, meglévő definíciók adatainak beolvasása.

a házirend-definíció toocreate futtatása:

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

A kérelem törzsében hasonló toohello a következő példa a következők:

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

### <a name="assign-policy"></a>Házirend hozzárendelése

Hello házirend-definíció szükséges hello hatókör hello keresztül is alkalmazhat [REST API-t a házirend-hozzárendelések](/rest/api/resources/policyassignments). hello REST API lehetővé teszi a toocreate házirend-hozzárendelést, és törlése meglévő hozzárendelések adatainak beolvasása.

egy házirend-hozzárendelést toocreate futtatása:

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

{házirend-hozzárendelés} hello hello hello házirend-hozzárendelés neve.

A kérelem törzsében hasonló toohello a következő példa a következők:

```json
{
  "properties":{
    "displayName":"West US only policy assignment on hello subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a>Házirend megtekintése
egy házirend tooget hello használja [házirend-definíció beolvasása](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) műveletet.

### <a name="get-aliases"></a>Aliasok beolvasása
Hello REST API-n keresztül aliasok le:

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

a következő példa hello alias definíciójának jeleníti meg. Ahogy látja, alias határoz meg elérési utak a különböző API-verziók, akkor is, amikor egy tulajdonság nevének módosítása. 

```json
"aliases": [
    {
      "name": "Microsoft.Storage/storageAccounts/sku.name",
      "paths": [
        {
          "path": "properties.accountType",
          "apiVersions": [
            "2015-06-15",
            "2015-05-01-preview"
          ]
        },
        {
          "path": "sku.name",
          "apiVersions": [
            "2016-01-01"
          ]
        }
      ]
    }
]
```

## <a name="powershell"></a>PowerShell

Hello PowerShell-példákkal a folytatás előtt győződjön meg arról, hogy [hello legújabb verziójának telepítése](/powershell/azure/install-azurerm-ps) az Azure PowerShell. Házirend-paraméterek 3.6.0 verzióban lettek hozzáadva. Ha egy korábbi, hello példák vissza, hibát jelző hello paraméter nem található.

### <a name="view-policy-definitions"></a>Házirend-definíciók megtekintése
toosee összes házirend-definíciók az előfizetéshez használatban hello a következő parancsot:

```powershell
Get-AzureRmPolicyDefinition
```

Összes elérhető, házirend-beállítást, többek között beépített házirendek adja vissza. Minden egyes házirend eredmény abban az esetben a következő formátumban hello:

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict hello locations your organization can specify when deploying resources. Use tooenforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

Mielőtt továbblép toocreate házirend-definíció nézze meg a hello beépített házirendek. Ha egy beépített házirendet érvényes hello korlátok van szüksége, kihagyhatja a házirend-definíció létrehozása. Ehelyett hozzárendelése hello beépített házirend szükséges toohello hatókör.

### <a name="create-policy-definition"></a>Házirend-definíció létrehozása
A házirend-definíció hello segítségével hozhat létre `New-AzureRmPolicyDefinition` parancsmag.

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy '{
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
}'
```            

hello kimeneti tárolja egy `$definition` objektum, amely házirend-hozzárendelés során használatos. 

Ahelyett, hogy adja meg a hello JSON paraméterként, megadhatja a hello elérési tooa .JSON kiterjesztésű fájlt tartalmazó hello házirendszabályt.

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy "c:\policies\coolAccessTier.json"
```

hello alábbi példa létrehoz egy házirend-definíció, amely tartalmazza a Paraméterek:

```powershell
$policy = '{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
            },
            {
                "not": {
                    "field": "location",
                    "in": "[parameters(''allowedLocations'')]"
                }
            }
        ]
    },
    "then": {
        "effect": "Deny"
    }
}'

$parameters = '{
    "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "hello list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy toospecify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a>Házirend hozzárendelése

Hello futtatásával kell alkalmazni kívánt hello hatókörből hello házirend `New-AzureRmPolicyAssignment` parancsmag. a következő példa hello hello házirend tooa erőforráscsoport rendeli hozzá.

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

tooassign egy házirendet a szükséges paraméter, hozzon létre, és ezen értékekkel rendelkező objektum. hello alábbi példa lekérdezi egy beépített házirend, és átadja a paraméterek értékeit:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a>Házirend-hozzárendelés megtekintése

egy adott házirend-hozzárendelést tooget használja:

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

tooview hello házirend szabály a házirend-definíció, használja:

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a>Távolítsa el a házirend-hozzárendelés 

egy házirend-hozzárendelést tooremove használja:

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a>Azure CLI

### <a name="view-policy-definitions"></a>Házirend-definíciók megtekintése
toosee összes házirend-definíciók az előfizetéshez használatban hello a következő parancsot:

```azurecli
az policy definition list
```

Összes elérhető, házirend-beállítást, többek között beépített házirendek adja vissza. Minden egyes házirend eredmény abban az esetben a következő formátumban hello:

```azurecli
{                                                            
  "description": "This policy enables you toorestrict hello locations your organization can specify when deploying resources. Use tooenforce your geo-compliance requirements.",                      
  "displayName": "Allowed locations",
  "id": "/providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "name": "e56962a6-4747-49cd-b67b-bf8b01975c4c",
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "in": "[parameters('listOfAllowedLocations')]"
      }
    },
    "then": {
      "effect": "Deny"
    }
  },
  "policyType": "BuiltIn"
}
```

Mielőtt továbblép toocreate házirend-definíció nézze meg a hello beépített házirendek. Ha egy beépített házirendet érvényes hello korlátok van szüksége, kihagyhatja a házirend-definíció létrehozása. Ehelyett hozzárendelése hello beépített házirend szükséges toohello hatókör.

### <a name="create-policy-definition"></a>Házirend-definíció létrehozása

A házirend-definíció Azure parancssori felület használatával hello házirend-definíció paranccsal hozhat létre.

```azurecli
az policy definition create --name coolAccessTier --description "Policy toospecify access tier." --rules '{
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
}'    
```

### <a name="assign-policy"></a>Házirend hozzárendelése

Hello házirend szükséges toohello hatókör hello házirend-hozzárendelés paranccsal is alkalmazhat. a következő példa hello házirend tooa erőforráscsoport rendeli hozzá.

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a>Házirend-hozzárendelés megtekintése

tooview egy házirend-hozzárendelést adjon meg, hello házirend-hozzárendelés neve és hello hatókör:

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a>Távolítsa el a házirend-hozzárendelés 

egy házirend-hozzárendelést tooremove használja:

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a>Következő lépések
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).


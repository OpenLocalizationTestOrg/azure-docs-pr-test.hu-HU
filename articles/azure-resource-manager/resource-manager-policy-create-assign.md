---
title: "Rendelje hozzá, és az Azure erőforrás-házirendek kezelése |} Microsoft Docs"
description: "Az Azure erőforrás-házirendek alkalmazása előfizetésekhez és erőforráscsoportokhoz, és erőforrás-házirendekkel megtekintéséhez módját ismerteti."
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
ms.openlocfilehash: b204cffa8fab0ad27a9f78a81c04f0a0225d95f5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="assign-and-manage-resource-policies"></a><span data-ttu-id="25681-103">Rendelje hozzá, és erőforrás-házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="25681-103">Assign and manage resource policies</span></span>

<span data-ttu-id="25681-104">A házirend megvalósítása, hajtsa végre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="25681-104">To implement a policy, you must perform these steps:</span></span>

1. <span data-ttu-id="25681-105">Ellenőrizze a házirend-definíciók száma (beleértve az Azure által biztosított beépített házirendek) az egyik már létezik-e az előfizetés, amely megfelel a követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="25681-105">Check policy definitions (including built-in policies provided by Azure) to see if one already exists in your subscription that fulfills your requirements.</span></span>
2. <span data-ttu-id="25681-106">Ha van ilyen, annak neve olvasható.</span><span class="sxs-lookup"><span data-stu-id="25681-106">If one exists, get its name.</span></span>
3. <span data-ttu-id="25681-107">Ha még nem létezik, adja meg a házirend JSON szabály, és adja hozzá az előfizetés a házirend-definíció.</span><span class="sxs-lookup"><span data-stu-id="25681-107">If one does not exist, define the policy rule with JSON, and add it as a policy definition in your subscription.</span></span> <span data-ttu-id="25681-108">Ebben a lépésben elérhetővé teszi a házirend a hozzárendeléshez, de nem felel meg a szabályokat az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="25681-108">This step makes the policy available for assignment but does not apply the rules to your subscription.</span></span>
4. <span data-ttu-id="25681-109">Mindkét esetben rendelje hozzá a házirendet (például egy előfizetés vagy az erőforrás csoport) hatókörbe.</span><span class="sxs-lookup"><span data-stu-id="25681-109">For either case, assign the policy to a scope (such as a subscription or resource group).</span></span> <span data-ttu-id="25681-110">A szabályok a házirend kényszerítése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="25681-110">The rules of the policy are now enforced.</span></span>

<span data-ttu-id="25681-111">Ez a cikk foglalkozik a házirend-definíció létrehozása, és rendelje hozzá, hogy definíció REST API-t, a PowerShell vagy az Azure CLI hatókör lépéseket.</span><span class="sxs-lookup"><span data-stu-id="25681-111">This article focuses on the steps to create a policy definition and assign that definition to a scope through REST API, PowerShell, or Azure CLI.</span></span> <span data-ttu-id="25681-112">Ha inkább a portál használatával rendelje hozzá a házirendek, lásd: [hozzárendelésére és kezelésére erőforrás-házirendek használata Azure-portálon](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="25681-112">If you prefer to use the portal to assign policies, see [Use Azure portal to assign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="25681-113">Ez a cikk nem a szintaxist a házirend-definíció létrehozására összpontosít.</span><span class="sxs-lookup"><span data-stu-id="25681-113">This article does not focus on the syntax for creating the policy definition.</span></span> <span data-ttu-id="25681-114">Házirendet a szintaxissal kapcsolatos információkért lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="25681-114">For information about policy syntax, see [Resource policy overview](resource-manager-policy.md).</span></span>

## <a name="rest-api"></a><span data-ttu-id="25681-115">REST API</span><span class="sxs-lookup"><span data-stu-id="25681-115">REST API</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="25681-116">Házirend-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="25681-116">Create policy definition</span></span>

<span data-ttu-id="25681-117">A házirendet hozhat létre a [REST API-t a házirend-definíciók](/rest/api/resources/policydefinitions).</span><span class="sxs-lookup"><span data-stu-id="25681-117">You can create a policy with the [REST API for Policy Definitions](/rest/api/resources/policydefinitions).</span></span> <span data-ttu-id="25681-118">A REST API lehetővé teszi létrehozása és törlése a házirend-definíciók és meglévő definíciók adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="25681-118">The REST API enables you to create and delete policy definitions, and get information about existing definitions.</span></span>

<span data-ttu-id="25681-119">Házirend-definíció létrehozásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="25681-119">To create a policy definition, run:</span></span>

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

<span data-ttu-id="25681-120">Az alábbi példához hasonló egy kérelemtörzset a következők:</span><span class="sxs-lookup"><span data-stu-id="25681-120">Include a request body similar to the following example:</span></span>

```json
{
  "properties": {
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
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

### <a name="assign-policy"></a><span data-ttu-id="25681-121">Házirend hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="25681-121">Assign policy</span></span>

<span data-ttu-id="25681-122">A házirend-definíció a kívánt hatóköre keresztül is alkalmazhat a [REST API-t a házirend-hozzárendelések](/rest/api/resources/policyassignments).</span><span class="sxs-lookup"><span data-stu-id="25681-122">You can apply the policy definition at the desired scope through the [REST API for policy assignments](/rest/api/resources/policyassignments).</span></span> <span data-ttu-id="25681-123">A REST API lehetővé teszi, hogy hozhat létre és a házirend-hozzárendelések törlése és a meglévő hozzárendelés adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="25681-123">The REST API enables you to create and delete policy assignments, and get information about existing assignments.</span></span>

<span data-ttu-id="25681-124">Házirend-hozzárendelés létrehozásához futtassa:</span><span class="sxs-lookup"><span data-stu-id="25681-124">To create a policy assignment, run:</span></span>

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

<span data-ttu-id="25681-125">A {házirend-hozzárendelés} a házirend-hozzárendelés neve.</span><span class="sxs-lookup"><span data-stu-id="25681-125">The {policy-assignment} is the name of the policy assignment.</span></span>

<span data-ttu-id="25681-126">Az alábbi példához hasonló egy kérelemtörzset a következők:</span><span class="sxs-lookup"><span data-stu-id="25681-126">Include a request body similar to the following example:</span></span>

```json
{
  "properties":{
    "displayName":"West US only policy assignment on the subscription ",
    "description":"Resources can only be provisioned in West US regions",
    "parameters": {
      "allowedLocations": { "value": ["northeurope", "westus"] }
     },
    "policyDefinitionId":"/subscriptions/{subscription-id}/providers/Microsoft.Authorization/policyDefinitions/{definition-name}",
      "scope":"/subscriptions/{subscription-id}"
  },
}
```

### <a name="view-policy"></a><span data-ttu-id="25681-127">Házirend megtekintése</span><span class="sxs-lookup"><span data-stu-id="25681-127">View policy</span></span>
<span data-ttu-id="25681-128">A házirend beszerzéséhez használja a [házirend-definíció beolvasása](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="25681-128">To get a policy, use the [Get policy definition](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operation.</span></span>

### <a name="get-aliases"></a><span data-ttu-id="25681-129">Aliasok beolvasása</span><span class="sxs-lookup"><span data-stu-id="25681-129">Get aliases</span></span>
<span data-ttu-id="25681-130">A REST API-n keresztül aliasok le:</span><span class="sxs-lookup"><span data-stu-id="25681-130">You can retrieve aliases through the REST API:</span></span>

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

<span data-ttu-id="25681-131">Az alábbi példában az alias definíciójának.</span><span class="sxs-lookup"><span data-stu-id="25681-131">The following example shows a definition of an alias.</span></span> <span data-ttu-id="25681-132">Ahogy látja, alias határoz meg elérési utak a különböző API-verziók, akkor is, amikor egy tulajdonság nevének módosítása.</span><span class="sxs-lookup"><span data-stu-id="25681-132">As you can see, an alias defines paths in different API versions, even when there is a property name change.</span></span> 

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

## <a name="powershell"></a><span data-ttu-id="25681-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="25681-133">PowerShell</span></span>

<span data-ttu-id="25681-134">A PowerShell-példákkal a folytatás előtt győződjön meg arról, hogy [telepítve a legújabb verzió](/powershell/azure/install-azurerm-ps) az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="25681-134">Before proceeding with the PowerShell examples, make sure you have [installed the latest version](/powershell/azure/install-azurerm-ps) of Azure PowerShell.</span></span> <span data-ttu-id="25681-135">Házirend-paraméterek 3.6.0 verzióban lettek hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="25681-135">Policy parameters were added in version 3.6.0.</span></span> <span data-ttu-id="25681-136">Ha egy korábbi, a példák, hibaüzenetet jelzi, hogy a paraméter nem található.</span><span class="sxs-lookup"><span data-stu-id="25681-136">If you have an earlier version, the examples return an error indicating the parameter cannot be found.</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="25681-137">Házirend-definíciók megtekintése</span><span class="sxs-lookup"><span data-stu-id="25681-137">View policy definitions</span></span>
<span data-ttu-id="25681-138">Az előfizetés az összes házirend-definíciók, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="25681-138">To see all policy definitions in your subscription, use the following command:</span></span>

```powershell
Get-AzureRmPolicyDefinition
```

<span data-ttu-id="25681-139">Összes elérhető, házirend-beállítást, többek között beépített házirendek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="25681-139">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="25681-140">Minden egyes házirend eredmény abban az esetben a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="25681-140">Each policy is returned in the following format:</span></span>

```powershell
Name               : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceId         : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceName       : e56962a6-4747-49cd-b67b-bf8b01975c4c
ResourceType       : Microsoft.Authorization/policyDefinitions
Properties         : @{displayName=Allowed locations; policyType=BuiltIn; description=This policy enables you to
                     restrict the locations your organization can specify when deploying resources. Use to enforce
                     your geo-compliance requirements.; parameters=; policyRule=}
PolicyDefinitionId : /providers/Microsoft.Authorization/policyDefinitions/e56962a6-4747-49cd-b67b-bf8b01975c4c
```

<span data-ttu-id="25681-141">Mielőtt továbblép a házirend-definíció létrehozása, tekintse meg a beépített házirendek.</span><span class="sxs-lookup"><span data-stu-id="25681-141">Before proceeding to create a policy definition, look at the built-in policies.</span></span> <span data-ttu-id="25681-142">Ha egy beépített házirendet kell korlátok érvényes, kihagyhatja a házirend-definíció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="25681-142">If you find a built-in policy that applies the limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="25681-143">Ehelyett a beépített házirend hozzárendelése a kívánt hatóköre.</span><span class="sxs-lookup"><span data-stu-id="25681-143">Instead, assign the built-in policy to the desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="25681-144">Házirend-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="25681-144">Create policy definition</span></span>
<span data-ttu-id="25681-145">A szabályzat definíciója használatával hozhat létre a `New-AzureRmPolicyDefinition` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="25681-145">You can create a policy definition using the `New-AzureRmPolicyDefinition` cmdlet.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy to specify access tier." -Policy '{
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

<span data-ttu-id="25681-146">A kimeneti tárolja egy `$definition` objektum, amely házirend-hozzárendelés során használatos.</span><span class="sxs-lookup"><span data-stu-id="25681-146">The output is stored in a `$definition` object, which is used during policy assignment.</span></span> 

<span data-ttu-id="25681-147">Ahelyett, hogy adja meg a JSON-paraméterként, megadhatja a házirendszabály tartalmazó .JSON kiterjesztésű fájl elérési útja.</span><span class="sxs-lookup"><span data-stu-id="25681-147">Rather than specifying the JSON as a parameter, you can provide the path to a .json file containing the policy rule.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy to specify access tier." -Policy "c:\policies\coolAccessTier.json"
```

<span data-ttu-id="25681-148">Az alábbi példakód létrehozza a házirend-definíció paramétereket tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="25681-148">The following example creates a policy definition that includes parameters:</span></span>

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
          "description": "The list of locations that can be specified when deploying storage accounts.",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
    }
}' 

$definition = New-AzureRmPolicyDefinition -Name storageLocations -Description "Policy to specify locations for storage accounts." -Policy $policy -Parameter $parameters 
```

### <a name="assign-policy"></a><span data-ttu-id="25681-149">Házirend hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="25681-149">Assign policy</span></span>

<span data-ttu-id="25681-150">A szabályzatot a kívánt hatókörben futtatásával kell alkalmazni a `New-AzureRmPolicyAssignment` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="25681-150">You apply the policy at the desired scope by using the `New-AzureRmPolicyAssignment` cmdlet.</span></span> <span data-ttu-id="25681-151">A következő példa egy erőforráscsoportot a szabályzat rendeli.</span><span class="sxs-lookup"><span data-stu-id="25681-151">The following example assigns the policy to a resource group.</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

<span data-ttu-id="25681-152">Rendelje hozzá egy házirendet, mely paraméterek szükségesek, hozzon létre, és ezen értékekkel rendelkező objektum.</span><span class="sxs-lookup"><span data-stu-id="25681-152">To assign a policy that requires parameters, create and object with those values.</span></span> <span data-ttu-id="25681-153">Az alábbi példa lekérdezi a beépített házirend, és átadja a paraméterek értékeit:</span><span class="sxs-lookup"><span data-stu-id="25681-153">The following example retrieves a built-in policy and passes in parameters values:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a><span data-ttu-id="25681-154">Házirend-hozzárendelés megtekintése</span><span class="sxs-lookup"><span data-stu-id="25681-154">View policy assignment</span></span>

<span data-ttu-id="25681-155">Egy adott házirend-hozzárendelés használatához:</span><span class="sxs-lookup"><span data-stu-id="25681-155">To get a specific policy assignment, use:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

<span data-ttu-id="25681-156">Házirend-definíció a házirend-szabálya megtekintéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="25681-156">To view the policy rule for a policy definition, use:</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="25681-157">Távolítsa el a házirend-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="25681-157">Remove policy assignment</span></span> 

<span data-ttu-id="25681-158">Házirend-hozzárendelés eltávolításához használja:</span><span class="sxs-lookup"><span data-stu-id="25681-158">To remove a policy assignment, use:</span></span>

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a><span data-ttu-id="25681-159">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="25681-159">Azure CLI</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="25681-160">Házirend-definíciók megtekintése</span><span class="sxs-lookup"><span data-stu-id="25681-160">View policy definitions</span></span>
<span data-ttu-id="25681-161">Az előfizetés az összes házirend-definíciók, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="25681-161">To see all policy definitions in your subscription, use the following command:</span></span>

```azurecli
az policy definition list
```

<span data-ttu-id="25681-162">Összes elérhető, házirend-beállítást, többek között beépített házirendek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="25681-162">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="25681-163">Minden egyes házirend eredmény abban az esetben a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="25681-163">Each policy is returned in the following format:</span></span>

```azurecli
{                                                            
  "description": "This policy enables you to restrict the locations your organization can specify when deploying resources. Use to enforce your geo-compliance requirements.",                      
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

<span data-ttu-id="25681-164">Mielőtt továbblép a házirend-definíció létrehozása, tekintse meg a beépített házirendek.</span><span class="sxs-lookup"><span data-stu-id="25681-164">Before proceeding to create a policy definition, look at the built-in policies.</span></span> <span data-ttu-id="25681-165">Ha egy beépített házirendet kell korlátok érvényes, kihagyhatja a házirend-definíció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="25681-165">If you find a built-in policy that applies the limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="25681-166">Ehelyett a beépített házirend hozzárendelése a kívánt hatóköre.</span><span class="sxs-lookup"><span data-stu-id="25681-166">Instead, assign the built-in policy to the desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="25681-167">Házirend-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="25681-167">Create policy definition</span></span>

<span data-ttu-id="25681-168">A házirend-definíció a házirend-definíció parancs az Azure parancssori felület használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="25681-168">You can create a policy definition using Azure CLI with the policy definition command.</span></span>

```azurecli
az policy definition create --name coolAccessTier --description "Policy to specify access tier." --rules '{
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

### <a name="assign-policy"></a><span data-ttu-id="25681-169">Házirend hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="25681-169">Assign policy</span></span>

<span data-ttu-id="25681-170">A házirend-hozzárendelés paranccsal alkalmazhatja a szabályzatot a kívánt hatókörbe.</span><span class="sxs-lookup"><span data-stu-id="25681-170">You can apply the policy to the desired scope by using the policy assignment command.</span></span> <span data-ttu-id="25681-171">A következő példa egy házirend rendel egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="25681-171">The following example assigns a policy to a resource group.</span></span>

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a><span data-ttu-id="25681-172">Házirend-hozzárendelés megtekintése</span><span class="sxs-lookup"><span data-stu-id="25681-172">View policy assignment</span></span>

<span data-ttu-id="25681-173">Házirend-hozzárendelés megtekinteni, adja meg a házirend-hozzárendelés neve és a hatókör:</span><span class="sxs-lookup"><span data-stu-id="25681-173">To view a policy assignment, provide the policy assignment name and the scope:</span></span>

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="25681-174">Távolítsa el a házirend-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="25681-174">Remove policy assignment</span></span> 

<span data-ttu-id="25681-175">Házirend-hozzárendelés eltávolításához használja:</span><span class="sxs-lookup"><span data-stu-id="25681-175">To remove a policy assignment, use:</span></span>

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a><span data-ttu-id="25681-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25681-176">Next steps</span></span>
* <span data-ttu-id="25681-177">Nagyvállalatoknak az [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Azure nagyvállalati struktúra - előíró előfizetés-irányítás) című cikk nyújt útmutatást az előfizetéseknek a Resource Managerrel való hatékony kezeléséről.</span><span class="sxs-lookup"><span data-stu-id="25681-177">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


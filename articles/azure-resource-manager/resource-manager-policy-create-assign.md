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
# <a name="assign-and-manage-resource-policies"></a><span data-ttu-id="1f161-103">Rendelje hozzá, és erőforrás-házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="1f161-103">Assign and manage resource policies</span></span>

<span data-ttu-id="1f161-104">egy házirend tooimplement, végezze el ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1f161-104">tooimplement a policy, you must perform these steps:</span></span>

1. <span data-ttu-id="1f161-105">Ellenőrizze a házirend-definíciók (beleértve az Azure által biztosított beépített házirendek) toosee, ha már létezik ilyen az előfizetés, amely megfelel a követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="1f161-105">Check policy definitions (including built-in policies provided by Azure) toosee if one already exists in your subscription that fulfills your requirements.</span></span>
2. <span data-ttu-id="1f161-106">Ha van ilyen, annak neve olvasható.</span><span class="sxs-lookup"><span data-stu-id="1f161-106">If one exists, get its name.</span></span>
3. <span data-ttu-id="1f161-107">Ha még nem létezik, határozza meg a JSON hello házirendszabály, és adja hozzá az előfizetés a házirend-definíció.</span><span class="sxs-lookup"><span data-stu-id="1f161-107">If one does not exist, define hello policy rule with JSON, and add it as a policy definition in your subscription.</span></span> <span data-ttu-id="1f161-108">Ez a lépés hatására hello házirend rendelhető hozzá, de nem alkalmazza a hello szabályok tooyour előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1f161-108">This step makes hello policy available for assignment but does not apply hello rules tooyour subscription.</span></span>
4. <span data-ttu-id="1f161-109">Mindkét esetben rendelni hello házirend tooa hatáskörüket (például egy előfizetés vagy az erőforrás csoportot).</span><span class="sxs-lookup"><span data-stu-id="1f161-109">For either case, assign hello policy tooa scope (such as a subscription or resource group).</span></span> <span data-ttu-id="1f161-110">hello szabályok hello házirend kényszerítése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="1f161-110">hello rules of hello policy are now enforced.</span></span>

<span data-ttu-id="1f161-111">Ez a cikk egy házirend-definíció hello lépéseket toocreate összpontosít, és rendelje hozzá a REST API-t, a PowerShell vagy az Azure CLI tooa definition hatókörnek.</span><span class="sxs-lookup"><span data-stu-id="1f161-111">This article focuses on hello steps toocreate a policy definition and assign that definition tooa scope through REST API, PowerShell, or Azure CLI.</span></span> <span data-ttu-id="1f161-112">Ha jobban szeret toouse hello portál tooassign házirendek, lásd: [használata Azure-portál tooassign és erőforrás-házirendek kezeléséhez](resource-manager-policy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1f161-112">If you prefer toouse hello portal tooassign policies, see [Use Azure portal tooassign and manage resource policies](resource-manager-policy-portal.md).</span></span> <span data-ttu-id="1f161-113">Ez a cikk nem hello házirend-definíció létrehozása hello szintaxisának összpontosítanak.</span><span class="sxs-lookup"><span data-stu-id="1f161-113">This article does not focus on hello syntax for creating hello policy definition.</span></span> <span data-ttu-id="1f161-114">Házirendet a szintaxissal kapcsolatos információkért lásd: [erőforrás házirendek – áttekintés](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="1f161-114">For information about policy syntax, see [Resource policy overview](resource-manager-policy.md).</span></span>

## <a name="rest-api"></a><span data-ttu-id="1f161-115">REST API</span><span class="sxs-lookup"><span data-stu-id="1f161-115">REST API</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="1f161-116">Házirend-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f161-116">Create policy definition</span></span>

<span data-ttu-id="1f161-117">Létrehozhat egy házirendet hello [REST API-t a házirend-definíciók](/rest/api/resources/policydefinitions).</span><span class="sxs-lookup"><span data-stu-id="1f161-117">You can create a policy with hello [REST API for Policy Definitions](/rest/api/resources/policydefinitions).</span></span> <span data-ttu-id="1f161-118">hello REST API lehetővé teszi a toocreate és törlése, házirend-definíciók, meglévő definíciók adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="1f161-118">hello REST API enables you toocreate and delete policy definitions, and get information about existing definitions.</span></span>

<span data-ttu-id="1f161-119">a házirend-definíció toocreate futtatása:</span><span class="sxs-lookup"><span data-stu-id="1f161-119">toocreate a policy definition, run:</span></span>

```HTTP
PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}
```

<span data-ttu-id="1f161-120">A kérelem törzsében hasonló toohello a következő példa a következők:</span><span class="sxs-lookup"><span data-stu-id="1f161-120">Include a request body similar toohello following example:</span></span>

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

### <a name="assign-policy"></a><span data-ttu-id="1f161-121">Házirend hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1f161-121">Assign policy</span></span>

<span data-ttu-id="1f161-122">Hello házirend-definíció szükséges hello hatókör hello keresztül is alkalmazhat [REST API-t a házirend-hozzárendelések](/rest/api/resources/policyassignments).</span><span class="sxs-lookup"><span data-stu-id="1f161-122">You can apply hello policy definition at hello desired scope through hello [REST API for policy assignments](/rest/api/resources/policyassignments).</span></span> <span data-ttu-id="1f161-123">hello REST API lehetővé teszi a toocreate házirend-hozzárendelést, és törlése meglévő hozzárendelések adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="1f161-123">hello REST API enables you toocreate and delete policy assignments, and get information about existing assignments.</span></span>

<span data-ttu-id="1f161-124">egy házirend-hozzárendelést toocreate futtatása:</span><span class="sxs-lookup"><span data-stu-id="1f161-124">toocreate a policy assignment, run:</span></span>

```HTTP
PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}
```

<span data-ttu-id="1f161-125">{házirend-hozzárendelés} hello hello hello házirend-hozzárendelés neve.</span><span class="sxs-lookup"><span data-stu-id="1f161-125">hello {policy-assignment} is hello name of hello policy assignment.</span></span>

<span data-ttu-id="1f161-126">A kérelem törzsében hasonló toohello a következő példa a következők:</span><span class="sxs-lookup"><span data-stu-id="1f161-126">Include a request body similar toohello following example:</span></span>

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

### <a name="view-policy"></a><span data-ttu-id="1f161-127">Házirend megtekintése</span><span class="sxs-lookup"><span data-stu-id="1f161-127">View policy</span></span>
<span data-ttu-id="1f161-128">egy házirend tooget hello használja [házirend-definíció beolvasása](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) műveletet.</span><span class="sxs-lookup"><span data-stu-id="1f161-128">tooget a policy, use hello [Get policy definition](https://docs.microsoft.com/rest/api/resources/policydefinitions#PolicyDefinitions_Get) operation.</span></span>

### <a name="get-aliases"></a><span data-ttu-id="1f161-129">Aliasok beolvasása</span><span class="sxs-lookup"><span data-stu-id="1f161-129">Get aliases</span></span>
<span data-ttu-id="1f161-130">Hello REST API-n keresztül aliasok le:</span><span class="sxs-lookup"><span data-stu-id="1f161-130">You can retrieve aliases through hello REST API:</span></span>

```HTTP
GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
```

<span data-ttu-id="1f161-131">a következő példa hello alias definíciójának jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1f161-131">hello following example shows a definition of an alias.</span></span> <span data-ttu-id="1f161-132">Ahogy látja, alias határoz meg elérési utak a különböző API-verziók, akkor is, amikor egy tulajdonság nevének módosítása.</span><span class="sxs-lookup"><span data-stu-id="1f161-132">As you can see, an alias defines paths in different API versions, even when there is a property name change.</span></span> 

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

## <a name="powershell"></a><span data-ttu-id="1f161-133">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1f161-133">PowerShell</span></span>

<span data-ttu-id="1f161-134">Hello PowerShell-példákkal a folytatás előtt győződjön meg arról, hogy [hello legújabb verziójának telepítése](/powershell/azure/install-azurerm-ps) az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1f161-134">Before proceeding with hello PowerShell examples, make sure you have [installed hello latest version](/powershell/azure/install-azurerm-ps) of Azure PowerShell.</span></span> <span data-ttu-id="1f161-135">Házirend-paraméterek 3.6.0 verzióban lettek hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="1f161-135">Policy parameters were added in version 3.6.0.</span></span> <span data-ttu-id="1f161-136">Ha egy korábbi, hello példák vissza, hibát jelző hello paraméter nem található.</span><span class="sxs-lookup"><span data-stu-id="1f161-136">If you have an earlier version, hello examples return an error indicating hello parameter cannot be found.</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="1f161-137">Házirend-definíciók megtekintése</span><span class="sxs-lookup"><span data-stu-id="1f161-137">View policy definitions</span></span>
<span data-ttu-id="1f161-138">toosee összes házirend-definíciók az előfizetéshez használatban hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1f161-138">toosee all policy definitions in your subscription, use hello following command:</span></span>

```powershell
Get-AzureRmPolicyDefinition
```

<span data-ttu-id="1f161-139">Összes elérhető, házirend-beállítást, többek között beépített házirendek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1f161-139">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="1f161-140">Minden egyes házirend eredmény abban az esetben a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="1f161-140">Each policy is returned in hello following format:</span></span>

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

<span data-ttu-id="1f161-141">Mielőtt továbblép toocreate házirend-definíció nézze meg a hello beépített házirendek.</span><span class="sxs-lookup"><span data-stu-id="1f161-141">Before proceeding toocreate a policy definition, look at hello built-in policies.</span></span> <span data-ttu-id="1f161-142">Ha egy beépített házirendet érvényes hello korlátok van szüksége, kihagyhatja a házirend-definíció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1f161-142">If you find a built-in policy that applies hello limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="1f161-143">Ehelyett hozzárendelése hello beépített házirend szükséges toohello hatókör.</span><span class="sxs-lookup"><span data-stu-id="1f161-143">Instead, assign hello built-in policy toohello desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="1f161-144">Házirend-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f161-144">Create policy definition</span></span>
<span data-ttu-id="1f161-145">A házirend-definíció hello segítségével hozhat létre `New-AzureRmPolicyDefinition` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1f161-145">You can create a policy definition using hello `New-AzureRmPolicyDefinition` cmdlet.</span></span>

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

<span data-ttu-id="1f161-146">hello kimeneti tárolja egy `$definition` objektum, amely házirend-hozzárendelés során használatos.</span><span class="sxs-lookup"><span data-stu-id="1f161-146">hello output is stored in a `$definition` object, which is used during policy assignment.</span></span> 

<span data-ttu-id="1f161-147">Ahelyett, hogy adja meg a hello JSON paraméterként, megadhatja a hello elérési tooa .JSON kiterjesztésű fájlt tartalmazó hello házirendszabályt.</span><span class="sxs-lookup"><span data-stu-id="1f161-147">Rather than specifying hello JSON as a parameter, you can provide hello path tooa .json file containing hello policy rule.</span></span>

```powershell
$definition = New-AzureRmPolicyDefinition -Name coolAccessTier -Description "Policy toospecify access tier." -Policy "c:\policies\coolAccessTier.json"
```

<span data-ttu-id="1f161-148">hello alábbi példa létrehoz egy házirend-definíció, amely tartalmazza a Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="1f161-148">hello following example creates a policy definition that includes parameters:</span></span>

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

### <a name="assign-policy"></a><span data-ttu-id="1f161-149">Házirend hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1f161-149">Assign policy</span></span>

<span data-ttu-id="1f161-150">Hello futtatásával kell alkalmazni kívánt hello hatókörből hello házirend `New-AzureRmPolicyAssignment` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1f161-150">You apply hello policy at hello desired scope by using hello `New-AzureRmPolicyAssignment` cmdlet.</span></span> <span data-ttu-id="1f161-151">a következő példa hello hello házirend tooa erőforráscsoport rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="1f161-151">hello following example assigns hello policy tooa resource group.</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
New-AzureRMPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId -PolicyDefinition $definition
```

<span data-ttu-id="1f161-152">tooassign egy házirendet a szükséges paraméter, hozzon létre, és ezen értékekkel rendelkező objektum.</span><span class="sxs-lookup"><span data-stu-id="1f161-152">tooassign a policy that requires parameters, create and object with those values.</span></span> <span data-ttu-id="1f161-153">hello alábbi példa lekérdezi egy beépített házirend, és átadja a paraméterek értékeit:</span><span class="sxs-lookup"><span data-stu-id="1f161-153">hello following example retrieves a built-in policy and passes in parameters values:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/e5662a6-4747-49cd-b67b-bf8b01975c4c
$array = @("West US", "West US 2")
$param = @{"listOfAllowedLocations"=$array}
New-AzureRMPolicyAssignment -Name locationAssignment -Scope $rg.ResourceId -PolicyDefinition $definition -PolicyParameterObject $param
```

### <a name="view-policy-assignment"></a><span data-ttu-id="1f161-154">Házirend-hozzárendelés megtekintése</span><span class="sxs-lookup"><span data-stu-id="1f161-154">View policy assignment</span></span>

<span data-ttu-id="1f161-155">egy adott házirend-hozzárendelést tooget használja:</span><span class="sxs-lookup"><span data-stu-id="1f161-155">tooget a specific policy assignment, use:</span></span>

```powershell
$rg = Get-AzureRmResourceGroup -Name "ExampleGroup"
(Get-AzureRmPolicyAssignment -Name accessTierAssignment -Scope $rg.ResourceId
```

<span data-ttu-id="1f161-156">tooview hello házirend szabály a házirend-definíció, használja:</span><span class="sxs-lookup"><span data-stu-id="1f161-156">tooview hello policy rule for a policy definition, use:</span></span>

```powershell
(Get-AzureRmPolicyDefinition -Name coolAccessTier).Properties.policyRule | ConvertTo-Json
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="1f161-157">Távolítsa el a házirend-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="1f161-157">Remove policy assignment</span></span> 

<span data-ttu-id="1f161-158">egy házirend-hozzárendelést tooremove használja:</span><span class="sxs-lookup"><span data-stu-id="1f161-158">tooremove a policy assignment, use:</span></span>

```powershell
Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="azure-cli"></a><span data-ttu-id="1f161-159">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1f161-159">Azure CLI</span></span>

### <a name="view-policy-definitions"></a><span data-ttu-id="1f161-160">Házirend-definíciók megtekintése</span><span class="sxs-lookup"><span data-stu-id="1f161-160">View policy definitions</span></span>
<span data-ttu-id="1f161-161">toosee összes házirend-definíciók az előfizetéshez használatban hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="1f161-161">toosee all policy definitions in your subscription, use hello following command:</span></span>

```azurecli
az policy definition list
```

<span data-ttu-id="1f161-162">Összes elérhető, házirend-beállítást, többek között beépített házirendek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1f161-162">It returns all available policy definitions, including built-in policies.</span></span> <span data-ttu-id="1f161-163">Minden egyes házirend eredmény abban az esetben a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="1f161-163">Each policy is returned in hello following format:</span></span>

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

<span data-ttu-id="1f161-164">Mielőtt továbblép toocreate házirend-definíció nézze meg a hello beépített házirendek.</span><span class="sxs-lookup"><span data-stu-id="1f161-164">Before proceeding toocreate a policy definition, look at hello built-in policies.</span></span> <span data-ttu-id="1f161-165">Ha egy beépített házirendet érvényes hello korlátok van szüksége, kihagyhatja a házirend-definíció létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1f161-165">If you find a built-in policy that applies hello limits you need, you can skip creating a policy definition.</span></span> <span data-ttu-id="1f161-166">Ehelyett hozzárendelése hello beépített házirend szükséges toohello hatókör.</span><span class="sxs-lookup"><span data-stu-id="1f161-166">Instead, assign hello built-in policy toohello desired scope.</span></span>

### <a name="create-policy-definition"></a><span data-ttu-id="1f161-167">Házirend-definíció létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f161-167">Create policy definition</span></span>

<span data-ttu-id="1f161-168">A házirend-definíció Azure parancssori felület használatával hello házirend-definíció paranccsal hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="1f161-168">You can create a policy definition using Azure CLI with hello policy definition command.</span></span>

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

### <a name="assign-policy"></a><span data-ttu-id="1f161-169">Házirend hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1f161-169">Assign policy</span></span>

<span data-ttu-id="1f161-170">Hello házirend szükséges toohello hatókör hello házirend-hozzárendelés paranccsal is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1f161-170">You can apply hello policy toohello desired scope by using hello policy assignment command.</span></span> <span data-ttu-id="1f161-171">a következő példa hello házirend tooa erőforráscsoport rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="1f161-171">hello following example assigns a policy tooa resource group.</span></span>

```azurecli
az policy assignment create --name coolAccessTierAssignment --policy coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

### <a name="view-policy-assignment"></a><span data-ttu-id="1f161-172">Házirend-hozzárendelés megtekintése</span><span class="sxs-lookup"><span data-stu-id="1f161-172">View policy assignment</span></span>

<span data-ttu-id="1f161-173">tooview egy házirend-hozzárendelést adjon meg, hello házirend-hozzárendelés neve és hello hatókör:</span><span class="sxs-lookup"><span data-stu-id="1f161-173">tooview a policy assignment, provide hello policy assignment name and hello scope:</span></span>

```azurecli
az policy assignment show --name coolAccessTierAssignment --scope "/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}"
```

### <a name="remove-policy-assignment"></a><span data-ttu-id="1f161-174">Távolítsa el a házirend-hozzárendelés</span><span class="sxs-lookup"><span data-stu-id="1f161-174">Remove policy assignment</span></span> 

<span data-ttu-id="1f161-175">egy házirend-hozzárendelést tooremove használja:</span><span class="sxs-lookup"><span data-stu-id="1f161-175">tooremove a policy assignment, use:</span></span>

```azurecli
az policy assignment delete --name coolAccessTier --scope /subscriptions/{subscription-id}/resourceGroups/{resource-group-name}
```

## <a name="next-steps"></a><span data-ttu-id="1f161-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f161-176">Next steps</span></span>
* <span data-ttu-id="1f161-177">A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="1f161-177">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>


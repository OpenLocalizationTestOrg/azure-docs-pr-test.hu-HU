---
title: "aaaAzure Resource Manager sablonfüggvényei - erőforrások |} Microsoft Docs"
description: "Erőforrások hello funkciók toouse az Azure Resource Manager sablon tooretrieve értékeket ismerteti."
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
ms.date: 08/09/2017
ms.author: tomfitz
ms.openlocfilehash: c9d524b338b8b7ea6d8c9e0135d48e4fb8f167c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="ecba3-103">Az Azure Resource Manager sablonokhoz erőforrás-funkciók</span><span class="sxs-lookup"><span data-stu-id="ecba3-103">Resource functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="ecba3-104">A Resource Manager biztosít a következő funkciók az erőforrás-értékek első hello:</span><span class="sxs-lookup"><span data-stu-id="ecba3-104">Resource Manager provides hello following functions for getting resource values:</span></span>

* [<span data-ttu-id="ecba3-105">listKeys és a {Value} lista</span><span class="sxs-lookup"><span data-stu-id="ecba3-105">listKeys and list{Value}</span></span>](#listkeys)
* [<span data-ttu-id="ecba3-106">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="ecba3-106">providers</span></span>](#providers)
* [<span data-ttu-id="ecba3-107">hivatkozás</span><span class="sxs-lookup"><span data-stu-id="ecba3-107">reference</span></span>](#reference)
* [<span data-ttu-id="ecba3-108">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="ecba3-108">resourceGroup</span></span>](#resourcegroup)
* [<span data-ttu-id="ecba3-109">resourceId</span><span class="sxs-lookup"><span data-stu-id="ecba3-109">resourceId</span></span>](#resourceid)
* [<span data-ttu-id="ecba3-110">előfizetést</span><span class="sxs-lookup"><span data-stu-id="ecba3-110">subscription</span></span>](#subscription)

<span data-ttu-id="ecba3-111">tooget értékeket a paraméterek, a változók vagy a hello jelenlegi üzemelő példány, lásd: [központi telepítési érték funkciók](resource-group-template-functions-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="ecba3-111">tooget values from parameters, variables, or hello current deployment, see [Deployment value functions](resource-group-template-functions-deployment.md).</span></span>

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a><span data-ttu-id="ecba3-112">listKeys és a {Value} lista</span><span class="sxs-lookup"><span data-stu-id="ecba3-112">listKeys and list{Value}</span></span>
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

<span data-ttu-id="ecba3-113">Beolvasása hello minden erőforrástípus, amely támogatja a hello list művelet értékeit.</span><span class="sxs-lookup"><span data-stu-id="ecba3-113">Returns hello values for any resource type that supports hello list operation.</span></span> <span data-ttu-id="ecba3-114">hello leggyakoribb használata `listKeys`.</span><span class="sxs-lookup"><span data-stu-id="ecba3-114">hello most common usage is `listKeys`.</span></span> 

### <a name="parameters"></a><span data-ttu-id="ecba3-115">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="ecba3-115">Parameters</span></span>

| <span data-ttu-id="ecba3-116">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ecba3-116">Parameter</span></span> | <span data-ttu-id="ecba3-117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ecba3-117">Required</span></span> | <span data-ttu-id="ecba3-118">Típus</span><span class="sxs-lookup"><span data-stu-id="ecba3-118">Type</span></span> | <span data-ttu-id="ecba3-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="ecba3-119">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ecba3-120">resourceName vagy resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="ecba3-120">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="ecba3-121">Igen</span><span class="sxs-lookup"><span data-stu-id="ecba3-121">Yes</span></span> |<span data-ttu-id="ecba3-122">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-122">string</span></span> |<span data-ttu-id="ecba3-123">Hello erőforrás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ecba3-123">Unique identifier for hello resource.</span></span> |
| <span data-ttu-id="ecba3-124">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ecba3-124">apiVersion</span></span> |<span data-ttu-id="ecba3-125">Igen</span><span class="sxs-lookup"><span data-stu-id="ecba3-125">Yes</span></span> |<span data-ttu-id="ecba3-126">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-126">string</span></span> |<span data-ttu-id="ecba3-127">API-verzió erőforrás futásidejű állapot.</span><span class="sxs-lookup"><span data-stu-id="ecba3-127">API version of resource runtime state.</span></span> <span data-ttu-id="ecba3-128">Általában a hello formátumban **éééé-hh-nn**.</span><span class="sxs-lookup"><span data-stu-id="ecba3-128">Typically, in hello format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="ecba3-129">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="ecba3-129">Return value</span></span>

<span data-ttu-id="ecba3-130">hello visszaadott listKeys objektum rendelkezik hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ecba3-130">hello returned object from listKeys has hello following format:</span></span>

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

<span data-ttu-id="ecba3-131">Más lista függvények visszatérési formátumuk eltérő.</span><span class="sxs-lookup"><span data-stu-id="ecba3-131">Other list functions have different return formats.</span></span> <span data-ttu-id="ecba3-132">toosee hello formátum egy függvény figyelembevétel hello kimenetek szakaszban látható módon hello példa sablon.</span><span class="sxs-lookup"><span data-stu-id="ecba3-132">toosee hello format of a function, include it in hello outputs section as shown in hello example template.</span></span> 

### <a name="remarks"></a><span data-ttu-id="ecba3-133">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ecba3-133">Remarks</span></span>

<span data-ttu-id="ecba3-134">Bármely művelet kezdetű **lista** használható legyen a sablon egy függvényt.</span><span class="sxs-lookup"><span data-stu-id="ecba3-134">Any operation that starts with **list** can be used as a function in your template.</span></span> <span data-ttu-id="ecba3-135">hello elérhető műveletek nem csak listKeys többek között is műveletek, például `list`, `listAdminKeys`, és `listStatus`.</span><span class="sxs-lookup"><span data-stu-id="ecba3-135">hello available operations include not only listKeys, but also operations like `list`, `listAdminKeys`, and `listStatus`.</span></span> <span data-ttu-id="ecba3-136">Azonban nem használható **lista** hello értékeket igénylő műveletekhez kérelem törzse.</span><span class="sxs-lookup"><span data-stu-id="ecba3-136">However, you cannot use **list** operations that require values in hello request body.</span></span> <span data-ttu-id="ecba3-137">Például hello [lista fiók SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) kérelemtörzs-paraméterrel, például a művelethez *signedExpiry*, így a sablonon belül nem használható.</span><span class="sxs-lookup"><span data-stu-id="ecba3-137">For example, hello [List Account SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operation requires request body parameters like *signedExpiry*, so you cannot use it within a template.</span></span>

<span data-ttu-id="ecba3-138">toodetermine, mely rendelkezik a list művelet, a következő beállítások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="ecba3-138">toodetermine which resource types have a list operation, you have hello following options:</span></span>

* <span data-ttu-id="ecba3-139">Nézet hello [REST API-műveleteket](/rest/api/) az erőforrás-szolgáltató és listázási műveletei keressen.</span><span class="sxs-lookup"><span data-stu-id="ecba3-139">View hello [REST API operations](/rest/api/) for a resource provider, and look for list operations.</span></span> <span data-ttu-id="ecba3-140">Például, a storage-fiókok vannak hello [listKeys művelet](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span><span class="sxs-lookup"><span data-stu-id="ecba3-140">For example, storage accounts have hello [listKeys operation](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span></span>
* <span data-ttu-id="ecba3-141">Használjon hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="ecba3-141">Use hello [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet.</span></span> <span data-ttu-id="ecba3-142">hello alábbi példa lekérdezi valamennyi listázási műveletei storage-fiókok:</span><span class="sxs-lookup"><span data-stu-id="ecba3-142">hello following example gets all list operations for storage accounts:</span></span>

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* <span data-ttu-id="ecba3-143">A következő Azure CLI parancs toofilter csak hello listázási műveletei hello használata:</span><span class="sxs-lookup"><span data-stu-id="ecba3-143">Use hello following Azure CLI command toofilter only hello list operations:</span></span>

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

<span data-ttu-id="ecba3-144">Adja meg a hello erőforrás vagy hello használatával [resourceId függvény](#resourceid), vagy hello formátum `{providerNamespace}/{resourceType}/{resourceName}`.</span><span class="sxs-lookup"><span data-stu-id="ecba3-144">Specify hello resource by using either hello [resourceId function](#resourceid), or hello format `{providerNamespace}/{resourceType}/{resourceName}`.</span></span>


### <a name="example"></a><span data-ttu-id="ecba3-145">Példa</span><span class="sxs-lookup"><span data-stu-id="ecba3-145">Example</span></span>

<span data-ttu-id="ecba3-146">hello következő példa bemutatja, hogyan tooreturn hello elsődleges és másodlagos hello tárfiókból kulcsok kimenete szakasz.</span><span class="sxs-lookup"><span data-stu-id="ecba3-146">hello following example shows how tooreturn hello primary and secondary keys from a storage account in hello outputs section.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountId": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "storageKeysOutput": {
            "value": "[listKeys(parameters('storageAccountId'), '2016-01-01')]",
            "type" : "object"
        }
    }
}
``` 

<a id="providers" />

## <a name="providers"></a><span data-ttu-id="ecba3-147">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="ecba3-147">providers</span></span>
`providers(providerNamespace, [resourceType])`

<span data-ttu-id="ecba3-148">Egy erőforrás-szolgáltató és a támogatott erőforrástípusai információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="ecba3-148">Returns information about a resource provider and its supported resource types.</span></span> <span data-ttu-id="ecba3-149">Ha nem ad meg egy erőforrás típusa, a hello függvény hello erőforrás-szolgáltató az összes hello támogatott típus adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ecba3-149">If you do not provide a resource type, hello function returns all hello supported types for hello resource provider.</span></span>

### <a name="parameters"></a><span data-ttu-id="ecba3-150">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="ecba3-150">Parameters</span></span>

| <span data-ttu-id="ecba3-151">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ecba3-151">Parameter</span></span> | <span data-ttu-id="ecba3-152">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ecba3-152">Required</span></span> | <span data-ttu-id="ecba3-153">Típus</span><span class="sxs-lookup"><span data-stu-id="ecba3-153">Type</span></span> | <span data-ttu-id="ecba3-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="ecba3-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ecba3-155">providerNamespace</span><span class="sxs-lookup"><span data-stu-id="ecba3-155">providerNamespace</span></span> |<span data-ttu-id="ecba3-156">Igen</span><span class="sxs-lookup"><span data-stu-id="ecba3-156">Yes</span></span> |<span data-ttu-id="ecba3-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-157">string</span></span> |<span data-ttu-id="ecba3-158">Namespace hello szolgáltató</span><span class="sxs-lookup"><span data-stu-id="ecba3-158">Namespace of hello provider</span></span> |
| <span data-ttu-id="ecba3-159">a resourceType</span><span class="sxs-lookup"><span data-stu-id="ecba3-159">resourceType</span></span> |<span data-ttu-id="ecba3-160">Nem</span><span class="sxs-lookup"><span data-stu-id="ecba3-160">No</span></span> |<span data-ttu-id="ecba3-161">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-161">string</span></span> |<span data-ttu-id="ecba3-162">a megadott névtér hello típusú erőforrás hello belül.</span><span class="sxs-lookup"><span data-stu-id="ecba3-162">hello type of resource within hello specified namespace.</span></span> |

### <a name="return-value"></a><span data-ttu-id="ecba3-163">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="ecba3-163">Return value</span></span>

<span data-ttu-id="ecba3-164">Minden támogatott típus eredmény abban az esetben a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="ecba3-164">Each supported type is returned in hello following format:</span></span> 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

<span data-ttu-id="ecba3-165">Tömb sorrendje hello visszaadott értékek nem garantált.</span><span class="sxs-lookup"><span data-stu-id="ecba3-165">Array ordering of hello returned values is not guaranteed.</span></span>

### <a name="example"></a><span data-ttu-id="ecba3-166">Példa</span><span class="sxs-lookup"><span data-stu-id="ecba3-166">Example</span></span>

<span data-ttu-id="ecba3-167">hello a következő példa bemutatja, hogyan toouse hello szolgáltató funkció:</span><span class="sxs-lookup"><span data-stu-id="ecba3-167">hello following example shows how toouse hello provider function:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers('Microsoft.Web', 'sites')]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="ecba3-168">hello előző példa visszaad egy objektumot a hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ecba3-168">hello preceding example returns an object in hello following format:</span></span>

```json
{
  "resourceType": "sites",
  "locations": [
    "South Central US",
    "North Europe",
    "West Europe",
    "Southeast Asia",
    ...
  ],
  "apiVersions": [
    "2016-08-01",
    "2016-03-01",
    "2015-08-01-preview",
    "2015-08-01",
    ...
  ]
}
```

<a id="reference" />

## <a name="reference"></a><span data-ttu-id="ecba3-169">Hivatkozás</span><span class="sxs-lookup"><span data-stu-id="ecba3-169">reference</span></span>
`reference(resourceName or resourceIdentifier, [apiVersion])`

<span data-ttu-id="ecba3-170">Az erőforrás futásidejű állapot képviselő objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ecba3-170">Returns an object representing a resource's runtime state.</span></span>

### <a name="parameters"></a><span data-ttu-id="ecba3-171">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="ecba3-171">Parameters</span></span>

| <span data-ttu-id="ecba3-172">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ecba3-172">Parameter</span></span> | <span data-ttu-id="ecba3-173">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ecba3-173">Required</span></span> | <span data-ttu-id="ecba3-174">Típus</span><span class="sxs-lookup"><span data-stu-id="ecba3-174">Type</span></span> | <span data-ttu-id="ecba3-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="ecba3-175">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ecba3-176">resourceName vagy resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="ecba3-176">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="ecba3-177">Igen</span><span class="sxs-lookup"><span data-stu-id="ecba3-177">Yes</span></span> |<span data-ttu-id="ecba3-178">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-178">string</span></span> |<span data-ttu-id="ecba3-179">Név vagy egy erőforrás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ecba3-179">Name or unique identifier of a resource.</span></span> |
| <span data-ttu-id="ecba3-180">apiVersion</span><span class="sxs-lookup"><span data-stu-id="ecba3-180">apiVersion</span></span> |<span data-ttu-id="ecba3-181">Nem</span><span class="sxs-lookup"><span data-stu-id="ecba3-181">No</span></span> |<span data-ttu-id="ecba3-182">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-182">string</span></span> |<span data-ttu-id="ecba3-183">A megadott erőforrás hello API-verzióját.</span><span class="sxs-lookup"><span data-stu-id="ecba3-183">API version of hello specified resource.</span></span> <span data-ttu-id="ecba3-184">Ez a paraméter kihagyása hello erőforrás nincs kiépítve belül ugyanazt a sablont.</span><span class="sxs-lookup"><span data-stu-id="ecba3-184">Include this parameter when hello resource is not provisioned within same template.</span></span> <span data-ttu-id="ecba3-185">Általában a hello formátumban **éééé-hh-nn**.</span><span class="sxs-lookup"><span data-stu-id="ecba3-185">Typically, in hello format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="ecba3-186">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="ecba3-186">Return value</span></span>

<span data-ttu-id="ecba3-187">Minden erőforrástípus hello hivatkozás függvény különböző tulajdonságait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ecba3-187">Every resource type returns different properties for hello reference function.</span></span> <span data-ttu-id="ecba3-188">hello függvény nem ad vissza egy egyetlen, előre meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="ecba3-188">hello function does not return a single, predefined format.</span></span> <span data-ttu-id="ecba3-189">toosee hello tulajdonságok erőforrástípusra, térjen vissza hello hello objektum kimenete szakasz hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="ecba3-189">toosee hello properties for a resource type, return hello object in hello outputs section as shown in hello example.</span></span>

### <a name="remarks"></a><span data-ttu-id="ecba3-190">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ecba3-190">Remarks</span></span>

<span data-ttu-id="ecba3-191">hello hivatkozás függvény az értékét a futásidejű állapot osztályból származik, és ezért nem használható hello változók szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ecba3-191">hello reference function derives its value from a runtime state, and therefore cannot be used in hello variables section.</span></span> <span data-ttu-id="ecba3-192">A sablon kimenetének részében használható.</span><span class="sxs-lookup"><span data-stu-id="ecba3-192">It can be used in outputs section of a template.</span></span> 

<span data-ttu-id="ecba3-193">Hello hivatkozás funkcióval, akkor implicit módon deklarálja, hogy egy erőforrás függ-e egy másik erőforrás, ha a hivatkozott hello erőforrás ki van építve belül ugyanazt a sablont.</span><span class="sxs-lookup"><span data-stu-id="ecba3-193">By using hello reference function, you implicitly declare that one resource depends on another resource if hello referenced resource is provisioned within same template.</span></span> <span data-ttu-id="ecba3-194">Nem kell tooalso használata hello dependsOn tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ecba3-194">You do not need tooalso use hello dependsOn property.</span></span> <span data-ttu-id="ecba3-195">hello függvény nem kerül kiértékelésre hello hivatkozott erőforrás amíg nem fejeződik be telepítési.</span><span class="sxs-lookup"><span data-stu-id="ecba3-195">hello function is not evaluated until hello referenced resource has completed deployment.</span></span>

<span data-ttu-id="ecba3-196">toosee hello nevét és értékeit erőforrástípusra, hozzon létre egy sablont, amely hello kimenetek szakaszban hello objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ecba3-196">toosee hello property names and values for a resource type, create a template that returns hello object in hello outputs section.</span></span> <span data-ttu-id="ecba3-197">Ha az adott típusú erőforrással rendelkezik, a sablon bármely új erőforrások telepítése nélkül hello objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ecba3-197">If you have an existing resource of that type, your template returns hello object without deploying any new resources.</span></span> 

<span data-ttu-id="ecba3-198">Általában akkor használják hello **hivatkozás** működéséhez tooreturn egy adott értéket az objektumot, például hello blob végpont URI vagy teljesen minősített tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="ecba3-198">Typically, you use hello **reference** function tooreturn a particular value from an object, such as hello blob endpoint URI or fully qualified domain name.</span></span>

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

### <a name="example"></a><span data-ttu-id="ecba3-199">Példa</span><span class="sxs-lookup"><span data-stu-id="ecba3-199">Example</span></span>

<span data-ttu-id="ecba3-200">hello toodeploy és a hivatkozás hello erőforrása ugyanazt a sablont, használja:</span><span class="sxs-lookup"><span data-stu-id="ecba3-200">toodeploy and reference hello resource in hello same template, use:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      }
    }
}
``` 

<span data-ttu-id="ecba3-201">hello előző példa visszaad egy objektumot a hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ecba3-201">hello preceding example returns an object in hello following format:</span></span>

```json
{
   "creationTime": "2017-06-13T21:24:46.618364Z",
   "primaryEndpoints": {
     "blob": "https://examplestorage.blob.core.windows.net/",
     "file": "https://examplestorage.file.core.windows.net/",
     "queue": "https://examplestorage.queue.core.windows.net/",
     "table": "https://examplestorage.table.core.windows.net/"
   },
   "primaryLocation": "southcentralus",
   "provisioningState": "Succeeded",
   "statusOfPrimary": "available",
   "supportsHttpsTrafficOnly": false
}
```

<span data-ttu-id="ecba3-202">hello alábbi példa hivatkozik, amelyek a sablonban nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="ecba3-202">hello following example references a storage account that is not deployed in this template.</span></span> <span data-ttu-id="ecba3-203">hello tárfiók már létezik hello belül azonos erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="ecba3-203">hello storage account already exists within hello same resource group.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

<a id="resourcegroup" />

## <a name="resourcegroup"></a><span data-ttu-id="ecba3-204">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="ecba3-204">resourceGroup</span></span>
`resourceGroup()`

<span data-ttu-id="ecba3-205">Hello jelenlegi erőforráscsoportban képviselő objektumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ecba3-205">Returns an object that represents hello current resource group.</span></span> 

### <a name="return-value"></a><span data-ttu-id="ecba3-206">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="ecba3-206">Return value</span></span>

<span data-ttu-id="ecba3-207">hello visszaadott objektum van hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ecba3-207">hello returned object is in hello following format:</span></span>

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

### <a name="remarks"></a><span data-ttu-id="ecba3-208">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ecba3-208">Remarks</span></span>

<span data-ttu-id="ecba3-209">A közös hello resourceGroup függvény használata hello toocreate erőforrások hello erőforráscsoport és ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="ecba3-209">A common use of hello resourceGroup function is toocreate resources in hello same location as hello resource group.</span></span> <span data-ttu-id="ecba3-210">hello alábbi példa hello erőforráscsoport helye tooassign hello helye webhelyhez.</span><span class="sxs-lookup"><span data-stu-id="ecba3-210">hello following example uses hello resource group location tooassign hello location for a web site.</span></span>

```json
"resources": [
   {
      "apiVersion": "2016-08-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="example"></a><span data-ttu-id="ecba3-211">Példa</span><span class="sxs-lookup"><span data-stu-id="ecba3-211">Example</span></span>

<span data-ttu-id="ecba3-212">hello következő sablon adja vissza hello erőforráscsoport hello tulajdonságainak.</span><span class="sxs-lookup"><span data-stu-id="ecba3-212">hello following template returns hello properties of hello resource group.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="ecba3-213">hello előző példa visszaad egy objektumot a hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ecba3-213">hello preceding example returns an object in hello following format:</span></span>

```json
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/examplegroup",
  "name": "examplegroup",
  "location": "southcentralus",
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

<a id="resourceid" />

## <a name="resourceid"></a><span data-ttu-id="ecba3-214">resourceId</span><span class="sxs-lookup"><span data-stu-id="ecba3-214">resourceId</span></span>
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

<span data-ttu-id="ecba3-215">Beolvasása hello erőforrás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ecba3-215">Returns hello unique identifier of a resource.</span></span> <span data-ttu-id="ecba3-216">Ezt a funkciót használja, ha hello erőforrás neve nem egyértelmű, illetve nem kiépített hello belül ugyanazt a sablont.</span><span class="sxs-lookup"><span data-stu-id="ecba3-216">You use this function when hello resource name is ambiguous or not provisioned within hello same template.</span></span> 

### <a name="parameters"></a><span data-ttu-id="ecba3-217">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="ecba3-217">Parameters</span></span>

| <span data-ttu-id="ecba3-218">Paraméter</span><span class="sxs-lookup"><span data-stu-id="ecba3-218">Parameter</span></span> | <span data-ttu-id="ecba3-219">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ecba3-219">Required</span></span> | <span data-ttu-id="ecba3-220">Típus</span><span class="sxs-lookup"><span data-stu-id="ecba3-220">Type</span></span> | <span data-ttu-id="ecba3-221">Leírás</span><span class="sxs-lookup"><span data-stu-id="ecba3-221">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="ecba3-222">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="ecba3-222">subscriptionId</span></span> |<span data-ttu-id="ecba3-223">Nem</span><span class="sxs-lookup"><span data-stu-id="ecba3-223">No</span></span> |<span data-ttu-id="ecba3-224">karakterlánc (a GUID formátumban)</span><span class="sxs-lookup"><span data-stu-id="ecba3-224">string (In GUID format)</span></span> |<span data-ttu-id="ecba3-225">Alapértelmezett érték: hello előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="ecba3-225">Default value is hello current subscription.</span></span> <span data-ttu-id="ecba3-226">Adjon meg ezt az értéket, amikor egy erőforrást egy másik előfizetésben tooretrieve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ecba3-226">Specify this value when you need tooretrieve a resource in another subscription.</span></span> |
| <span data-ttu-id="ecba3-227">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="ecba3-227">resourceGroupName</span></span> |<span data-ttu-id="ecba3-228">Nem</span><span class="sxs-lookup"><span data-stu-id="ecba3-228">No</span></span> |<span data-ttu-id="ecba3-229">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-229">string</span></span> |<span data-ttu-id="ecba3-230">Alapértelmezett érték: a jelenlegi erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="ecba3-230">Default value is current resource group.</span></span> <span data-ttu-id="ecba3-231">Adja meg ezt az értéket, ha tooretrieve egy erőforrást egy másik erőforráscsoportban van szükség.</span><span class="sxs-lookup"><span data-stu-id="ecba3-231">Specify this value when you need tooretrieve a resource in another resource group.</span></span> |
| <span data-ttu-id="ecba3-232">a resourceType</span><span class="sxs-lookup"><span data-stu-id="ecba3-232">resourceType</span></span> |<span data-ttu-id="ecba3-233">Igen</span><span class="sxs-lookup"><span data-stu-id="ecba3-233">Yes</span></span> |<span data-ttu-id="ecba3-234">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-234">string</span></span> |<span data-ttu-id="ecba3-235">Beleértve az erőforrás-szolgáltató névtere erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="ecba3-235">Type of resource including resource provider namespace.</span></span> |
| <span data-ttu-id="ecba3-236">resourceName1</span><span class="sxs-lookup"><span data-stu-id="ecba3-236">resourceName1</span></span> |<span data-ttu-id="ecba3-237">Igen</span><span class="sxs-lookup"><span data-stu-id="ecba3-237">Yes</span></span> |<span data-ttu-id="ecba3-238">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-238">string</span></span> |<span data-ttu-id="ecba3-239">Erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="ecba3-239">Name of resource.</span></span> |
| <span data-ttu-id="ecba3-240">resourceName2</span><span class="sxs-lookup"><span data-stu-id="ecba3-240">resourceName2</span></span> |<span data-ttu-id="ecba3-241">Nem</span><span class="sxs-lookup"><span data-stu-id="ecba3-241">No</span></span> |<span data-ttu-id="ecba3-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-242">string</span></span> |<span data-ttu-id="ecba3-243">Következő neve erőforrásszegmensre. Ha az erőforrás van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="ecba3-243">Next resource name segment if resource is nested.</span></span> |

### <a name="return-value"></a><span data-ttu-id="ecba3-244">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="ecba3-244">Return value</span></span>

<span data-ttu-id="ecba3-245">hello azonosító eredmény abban az esetben a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="ecba3-245">hello identifier is returned in hello following format:</span></span>

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a><span data-ttu-id="ecba3-246">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ecba3-246">Remarks</span></span>

<span data-ttu-id="ecba3-247">hello megadott paraméterértékek függnek, hogy hello erőforrás hello a jelenlegi telepítésben hello azonos előfizetésbe és erőforráscsoportba csoportban.</span><span class="sxs-lookup"><span data-stu-id="ecba3-247">hello parameter values you specify depend on whether hello resource is in hello same subscription and resource group as hello current deployment.</span></span>

<span data-ttu-id="ecba3-248">ugyanaz a tárfiókon lévő hello azonosító tooget hello erőforrás előfizetés és az erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="ecba3-248">tooget hello resource ID for a storage account in hello same subscription and resource group, use:</span></span>

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="ecba3-249">tooget hello erőforrás-azonosító a tárfiók ugyanahhoz az előfizetéshez, de egy másik erőforráscsoportban található, használja hello:</span><span class="sxs-lookup"><span data-stu-id="ecba3-249">tooget hello resource ID for a storage account in hello same subscription but a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="ecba3-250">tooget hello erőforrás-azonosító egy másik előfizetést, és az erőforráscsoportot, egy tárfiókot használja:</span><span class="sxs-lookup"><span data-stu-id="ecba3-250">tooget hello resource ID for a storage account in a different subscription and resource group, use:</span></span>

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="ecba3-251">tooget hello erőforrás-azonosító egy másik erőforráscsoportban található adatbázis használata:</span><span class="sxs-lookup"><span data-stu-id="ecba3-251">tooget hello resource ID for a database in a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

<span data-ttu-id="ecba3-252">Gyakran kell toouse Ez a függvény egy tárfiókhoz vagy a virtuális hálózat használata egy másik erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="ecba3-252">Often, you need toouse this function when using a storage account or virtual network in an alternate resource group.</span></span> <span data-ttu-id="ecba3-253">hello következő példa bemutatja, hogyan könnyen használható egy külső erőforráscsoportból erőforrás:</span><span class="sxs-lookup"><span data-stu-id="ecba3-253">hello following example shows how a resource from an external resource group can easily be used:</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="example"></a><span data-ttu-id="ecba3-254">Példa</span><span class="sxs-lookup"><span data-stu-id="ecba3-254">Example</span></span>

<span data-ttu-id="ecba3-255">hello alábbi példa hello erőforrás-Azonosítót adja vissza egy tárfiók hello erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="ecba3-255">hello following example returns hello resource ID for a storage account in hello resource group:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "sameRGOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentRGOutput": {
            "value": "[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "differentSubOutput": {
            "value": "[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        },
        "nestedResourceOutput": {
            "value": "[resourceId('Microsoft.SQL/servers/databases', 'serverName', 'databaseName')]",
            "type" : "string"
        }
    }
}
```

<span data-ttu-id="ecba3-256">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="ecba3-256">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="ecba3-257">Név</span><span class="sxs-lookup"><span data-stu-id="ecba3-257">Name</span></span> | <span data-ttu-id="ecba3-258">Típus</span><span class="sxs-lookup"><span data-stu-id="ecba3-258">Type</span></span> | <span data-ttu-id="ecba3-259">Érték</span><span class="sxs-lookup"><span data-stu-id="ecba3-259">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="ecba3-260">sameRGOutput</span><span class="sxs-lookup"><span data-stu-id="ecba3-260">sameRGOutput</span></span> | <span data-ttu-id="ecba3-261">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-261">String</span></span> | <span data-ttu-id="ecba3-262">/Subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/Providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="ecba3-262">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="ecba3-263">differentRGOutput</span><span class="sxs-lookup"><span data-stu-id="ecba3-263">differentRGOutput</span></span> | <span data-ttu-id="ecba3-264">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-264">String</span></span> | <span data-ttu-id="ecba3-265">/Subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="ecba3-265">/subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="ecba3-266">differentSubOutput</span><span class="sxs-lookup"><span data-stu-id="ecba3-266">differentSubOutput</span></span> | <span data-ttu-id="ecba3-267">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-267">String</span></span> | <span data-ttu-id="ecba3-268">/Subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="ecba3-268">/subscriptions/{different-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="ecba3-269">nestedResourceOutput</span><span class="sxs-lookup"><span data-stu-id="ecba3-269">nestedResourceOutput</span></span> | <span data-ttu-id="ecba3-270">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ecba3-270">String</span></span> | <span data-ttu-id="ecba3-271">/Subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/Providers/Microsoft.SQL/Servers/serverName/Databases/databaseName</span><span class="sxs-lookup"><span data-stu-id="ecba3-271">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName</span></span> |

<a id="subscription" />

## <a name="subscription"></a><span data-ttu-id="ecba3-272">előfizetést</span><span class="sxs-lookup"><span data-stu-id="ecba3-272">subscription</span></span>
`subscription()`

<span data-ttu-id="ecba3-273">Hello előfizetés hello aktuális központi telepítés részleteit adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ecba3-273">Returns details about hello subscription for hello current deployment.</span></span> 

### <a name="return-value"></a><span data-ttu-id="ecba3-274">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="ecba3-274">Return value</span></span>

<span data-ttu-id="ecba3-275">hello függvény hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ecba3-275">hello function returns hello following format:</span></span>

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a><span data-ttu-id="ecba3-276">Példa</span><span class="sxs-lookup"><span data-stu-id="ecba3-276">Example</span></span>

<span data-ttu-id="ecba3-277">hello következő példa bemutatja hello előfizetés függvény hívása hello kimenetek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ecba3-277">hello following example shows hello subscription function called in hello outputs section.</span></span> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="ecba3-278">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecba3-278">Next steps</span></span>
* <span data-ttu-id="ecba3-279">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ecba3-279">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="ecba3-280">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ecba3-280">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="ecba3-281">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="ecba3-281">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="ecba3-282">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ecba3-282">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


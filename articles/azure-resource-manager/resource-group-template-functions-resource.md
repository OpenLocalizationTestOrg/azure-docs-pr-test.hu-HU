---
title: "Az Azure Resource Manager sablonfüggvényei - erőforrások |} Microsoft Docs"
description: "Az Azure Resource Manager-sablonok segítségével erőforrásokra vonatkozó értékek lekérését funkcióit ismerteti."
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
ms.openlocfilehash: 494ade55f21c19d9c68d5cc52756528401d9bb77
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="resource-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="cb98d-103">Az Azure Resource Manager sablonokhoz erőforrás-funkciók</span><span class="sxs-lookup"><span data-stu-id="cb98d-103">Resource functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="cb98d-104">Erőforrás-kezelő a következő funkciókat biztosít erőforrás értékek beolvasása:</span><span class="sxs-lookup"><span data-stu-id="cb98d-104">Resource Manager provides the following functions for getting resource values:</span></span>

* [<span data-ttu-id="cb98d-105">listKeys és a {Value} lista</span><span class="sxs-lookup"><span data-stu-id="cb98d-105">listKeys and list{Value}</span></span>](#listkeys)
* [<span data-ttu-id="cb98d-106">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="cb98d-106">providers</span></span>](#providers)
* [<span data-ttu-id="cb98d-107">hivatkozás</span><span class="sxs-lookup"><span data-stu-id="cb98d-107">reference</span></span>](#reference)
* [<span data-ttu-id="cb98d-108">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="cb98d-108">resourceGroup</span></span>](#resourcegroup)
* [<span data-ttu-id="cb98d-109">resourceId</span><span class="sxs-lookup"><span data-stu-id="cb98d-109">resourceId</span></span>](#resourceid)
* [<span data-ttu-id="cb98d-110">előfizetést</span><span class="sxs-lookup"><span data-stu-id="cb98d-110">subscription</span></span>](#subscription)

<span data-ttu-id="cb98d-111">Ahhoz, hogy az értékeket a paraméterek, változók vagy a jelenlegi üzemelő példány, lásd: [központi telepítési érték funkciók](resource-group-template-functions-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="cb98d-111">To get values from parameters, variables, or the current deployment, see [Deployment value functions](resource-group-template-functions-deployment.md).</span></span>

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a><span data-ttu-id="cb98d-112">listKeys és a {Value} lista</span><span class="sxs-lookup"><span data-stu-id="cb98d-112">listKeys and list{Value}</span></span>
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

<span data-ttu-id="cb98d-113">Minden erőforrástípus, amely támogatja a list művelet értékeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cb98d-113">Returns the values for any resource type that supports the list operation.</span></span> <span data-ttu-id="cb98d-114">A leggyakoribb használata `listKeys`.</span><span class="sxs-lookup"><span data-stu-id="cb98d-114">The most common usage is `listKeys`.</span></span> 

### <a name="parameters"></a><span data-ttu-id="cb98d-115">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="cb98d-115">Parameters</span></span>

| <span data-ttu-id="cb98d-116">Paraméter</span><span class="sxs-lookup"><span data-stu-id="cb98d-116">Parameter</span></span> | <span data-ttu-id="cb98d-117">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb98d-117">Required</span></span> | <span data-ttu-id="cb98d-118">Típus</span><span class="sxs-lookup"><span data-stu-id="cb98d-118">Type</span></span> | <span data-ttu-id="cb98d-119">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb98d-119">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="cb98d-120">resourceName vagy resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="cb98d-120">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="cb98d-121">Igen</span><span class="sxs-lookup"><span data-stu-id="cb98d-121">Yes</span></span> |<span data-ttu-id="cb98d-122">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-122">string</span></span> |<span data-ttu-id="cb98d-123">Az erőforrás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cb98d-123">Unique identifier for the resource.</span></span> |
| <span data-ttu-id="cb98d-124">apiVersion</span><span class="sxs-lookup"><span data-stu-id="cb98d-124">apiVersion</span></span> |<span data-ttu-id="cb98d-125">Igen</span><span class="sxs-lookup"><span data-stu-id="cb98d-125">Yes</span></span> |<span data-ttu-id="cb98d-126">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-126">string</span></span> |<span data-ttu-id="cb98d-127">API-verzió erőforrás futásidejű állapot.</span><span class="sxs-lookup"><span data-stu-id="cb98d-127">API version of resource runtime state.</span></span> <span data-ttu-id="cb98d-128">Általában a következő formátumban **éééé-hh-nn**.</span><span class="sxs-lookup"><span data-stu-id="cb98d-128">Typically, in the format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="cb98d-129">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="cb98d-129">Return value</span></span>

<span data-ttu-id="cb98d-130">A visszaadott objektumot listKeys formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="cb98d-130">The returned object from listKeys has the following format:</span></span>

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

<span data-ttu-id="cb98d-131">Más lista függvények visszatérési formátumuk eltérő.</span><span class="sxs-lookup"><span data-stu-id="cb98d-131">Other list functions have different return formats.</span></span> <span data-ttu-id="cb98d-132">Egy függvény formátum megtekintéséhez foglalja bele a kimenetek szakaszban látható módon a példa sablont.</span><span class="sxs-lookup"><span data-stu-id="cb98d-132">To see the format of a function, include it in the outputs section as shown in the example template.</span></span> 

### <a name="remarks"></a><span data-ttu-id="cb98d-133">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cb98d-133">Remarks</span></span>

<span data-ttu-id="cb98d-134">Bármely művelet kezdetű **lista** használható legyen a sablon egy függvényt.</span><span class="sxs-lookup"><span data-stu-id="cb98d-134">Any operation that starts with **list** can be used as a function in your template.</span></span> <span data-ttu-id="cb98d-135">A rendelkezésre álló műveletek közé tartozik, nem csak listKeys, de az is, például műveletek `list`, `listAdminKeys`, és `listStatus`.</span><span class="sxs-lookup"><span data-stu-id="cb98d-135">The available operations include not only listKeys, but also operations like `list`, `listAdminKeys`, and `listStatus`.</span></span> <span data-ttu-id="cb98d-136">Azonban nem használható **lista** a kérelem törzsében szereplő értékeket igénylő műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="cb98d-136">However, you cannot use **list** operations that require values in the request body.</span></span> <span data-ttu-id="cb98d-137">Például a [lista fiók SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) kérelemtörzs-paraméterrel, például a művelethez *signedExpiry*, így a sablonon belül nem használható.</span><span class="sxs-lookup"><span data-stu-id="cb98d-137">For example, the [List Account SAS](/rest/api/storagerp/storageaccounts#StorageAccounts_ListAccountSAS) operation requires request body parameters like *signedExpiry*, so you cannot use it within a template.</span></span>

<span data-ttu-id="cb98d-138">Annak meghatározásához, mely rendelkezik a list művelet, a következő lehetőségei vannak:</span><span class="sxs-lookup"><span data-stu-id="cb98d-138">To determine which resource types have a list operation, you have the following options:</span></span>

* <span data-ttu-id="cb98d-139">Nézet a [REST API-műveleteket](/rest/api/) az erőforrás-szolgáltató és listázási műveletei keressen.</span><span class="sxs-lookup"><span data-stu-id="cb98d-139">View the [REST API operations](/rest/api/) for a resource provider, and look for list operations.</span></span> <span data-ttu-id="cb98d-140">Például, storage-fiókok vannak a [listKeys művelet](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span><span class="sxs-lookup"><span data-stu-id="cb98d-140">For example, storage accounts have the [listKeys operation](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys).</span></span>
* <span data-ttu-id="cb98d-141">Használja a [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="cb98d-141">Use the [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) PowerShell cmdlet.</span></span> <span data-ttu-id="cb98d-142">Az alábbi példa lekérdezi valamennyi listázási műveletei storage-fiókok:</span><span class="sxs-lookup"><span data-stu-id="cb98d-142">The following example gets all list operations for storage accounts:</span></span>

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* <span data-ttu-id="cb98d-143">A következő Azure CLI paranccsal csak a lista műveletek szűrése:</span><span class="sxs-lookup"><span data-stu-id="cb98d-143">Use the following Azure CLI command to filter only the list operations:</span></span>

  ```azurecli
  az provider operation show --namespace Microsoft.Storage --query "resourceTypes[?name=='storageAccounts'].operations[].name | [?contains(@, 'list')]"
  ```

<span data-ttu-id="cb98d-144">Adja meg az erőforrás használatával vagy a [resourceId függvény](#resourceid), vagy a formátum `{providerNamespace}/{resourceType}/{resourceName}`.</span><span class="sxs-lookup"><span data-stu-id="cb98d-144">Specify the resource by using either the [resourceId function](#resourceid), or the format `{providerNamespace}/{resourceType}/{resourceName}`.</span></span>


### <a name="example"></a><span data-ttu-id="cb98d-145">Példa</span><span class="sxs-lookup"><span data-stu-id="cb98d-145">Example</span></span>

<span data-ttu-id="cb98d-146">A következő példa bemutatja, hogyan vissza az elsődleges és másodlagos kulcsok a kimenetek szakaszban tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="cb98d-146">The following example shows how to return the primary and secondary keys from a storage account in the outputs section.</span></span>

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

## <a name="providers"></a><span data-ttu-id="cb98d-147">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="cb98d-147">providers</span></span>
`providers(providerNamespace, [resourceType])`

<span data-ttu-id="cb98d-148">Egy erőforrás-szolgáltató és a támogatott erőforrástípusai információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="cb98d-148">Returns information about a resource provider and its supported resource types.</span></span> <span data-ttu-id="cb98d-149">Erőforrástípus nincs megadva, a függvény a támogatott típusok esetében az erőforrás-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="cb98d-149">If you do not provide a resource type, the function returns all the supported types for the resource provider.</span></span>

### <a name="parameters"></a><span data-ttu-id="cb98d-150">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="cb98d-150">Parameters</span></span>

| <span data-ttu-id="cb98d-151">Paraméter</span><span class="sxs-lookup"><span data-stu-id="cb98d-151">Parameter</span></span> | <span data-ttu-id="cb98d-152">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb98d-152">Required</span></span> | <span data-ttu-id="cb98d-153">Típus</span><span class="sxs-lookup"><span data-stu-id="cb98d-153">Type</span></span> | <span data-ttu-id="cb98d-154">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb98d-154">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="cb98d-155">providerNamespace</span><span class="sxs-lookup"><span data-stu-id="cb98d-155">providerNamespace</span></span> |<span data-ttu-id="cb98d-156">Igen</span><span class="sxs-lookup"><span data-stu-id="cb98d-156">Yes</span></span> |<span data-ttu-id="cb98d-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-157">string</span></span> |<span data-ttu-id="cb98d-158">A szolgáltató Namespace</span><span class="sxs-lookup"><span data-stu-id="cb98d-158">Namespace of the provider</span></span> |
| <span data-ttu-id="cb98d-159">a resourceType</span><span class="sxs-lookup"><span data-stu-id="cb98d-159">resourceType</span></span> |<span data-ttu-id="cb98d-160">Nem</span><span class="sxs-lookup"><span data-stu-id="cb98d-160">No</span></span> |<span data-ttu-id="cb98d-161">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-161">string</span></span> |<span data-ttu-id="cb98d-162">A típusú erőforrás a megadott névtérben.</span><span class="sxs-lookup"><span data-stu-id="cb98d-162">The type of resource within the specified namespace.</span></span> |

### <a name="return-value"></a><span data-ttu-id="cb98d-163">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="cb98d-163">Return value</span></span>

<span data-ttu-id="cb98d-164">Minden támogatott típust ad vissza a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="cb98d-164">Each supported type is returned in the following format:</span></span> 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

<span data-ttu-id="cb98d-165">Tömb által visszaadott érték sorrendje nem garantált.</span><span class="sxs-lookup"><span data-stu-id="cb98d-165">Array ordering of the returned values is not guaranteed.</span></span>

### <a name="example"></a><span data-ttu-id="cb98d-166">Példa</span><span class="sxs-lookup"><span data-stu-id="cb98d-166">Example</span></span>

<span data-ttu-id="cb98d-167">A következő példa bemutatja, hogyan használja a szolgáltató funkciót:</span><span class="sxs-lookup"><span data-stu-id="cb98d-167">The following example shows how to use the provider function:</span></span>

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

<span data-ttu-id="cb98d-168">A fenti példában egy objektum a következő formátumban adja vissza:</span><span class="sxs-lookup"><span data-stu-id="cb98d-168">The preceding example returns an object in the following format:</span></span>

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

## <a name="reference"></a><span data-ttu-id="cb98d-169">Hivatkozás</span><span class="sxs-lookup"><span data-stu-id="cb98d-169">reference</span></span>
`reference(resourceName or resourceIdentifier, [apiVersion])`

<span data-ttu-id="cb98d-170">Az erőforrás futásidejű állapot képviselő objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cb98d-170">Returns an object representing a resource's runtime state.</span></span>

### <a name="parameters"></a><span data-ttu-id="cb98d-171">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="cb98d-171">Parameters</span></span>

| <span data-ttu-id="cb98d-172">Paraméter</span><span class="sxs-lookup"><span data-stu-id="cb98d-172">Parameter</span></span> | <span data-ttu-id="cb98d-173">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb98d-173">Required</span></span> | <span data-ttu-id="cb98d-174">Típus</span><span class="sxs-lookup"><span data-stu-id="cb98d-174">Type</span></span> | <span data-ttu-id="cb98d-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb98d-175">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="cb98d-176">resourceName vagy resourceIdentifier</span><span class="sxs-lookup"><span data-stu-id="cb98d-176">resourceName or resourceIdentifier</span></span> |<span data-ttu-id="cb98d-177">Igen</span><span class="sxs-lookup"><span data-stu-id="cb98d-177">Yes</span></span> |<span data-ttu-id="cb98d-178">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-178">string</span></span> |<span data-ttu-id="cb98d-179">Név vagy egy erőforrás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cb98d-179">Name or unique identifier of a resource.</span></span> |
| <span data-ttu-id="cb98d-180">apiVersion</span><span class="sxs-lookup"><span data-stu-id="cb98d-180">apiVersion</span></span> |<span data-ttu-id="cb98d-181">Nem</span><span class="sxs-lookup"><span data-stu-id="cb98d-181">No</span></span> |<span data-ttu-id="cb98d-182">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-182">string</span></span> |<span data-ttu-id="cb98d-183">A megadott erőforrás API-verzió.</span><span class="sxs-lookup"><span data-stu-id="cb98d-183">API version of the specified resource.</span></span> <span data-ttu-id="cb98d-184">Ez a paraméter tartalmazza, amikor az erőforrás nincs kiépítve belül ugyanazt a sablont.</span><span class="sxs-lookup"><span data-stu-id="cb98d-184">Include this parameter when the resource is not provisioned within same template.</span></span> <span data-ttu-id="cb98d-185">Általában a következő formátumban **éééé-hh-nn**.</span><span class="sxs-lookup"><span data-stu-id="cb98d-185">Typically, in the format, **yyyy-mm-dd**.</span></span> |

### <a name="return-value"></a><span data-ttu-id="cb98d-186">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="cb98d-186">Return value</span></span>

<span data-ttu-id="cb98d-187">Minden erőforrástípus adja vissza a hivatkozás függvény különböző tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="cb98d-187">Every resource type returns different properties for the reference function.</span></span> <span data-ttu-id="cb98d-188">A függvény nem ad vissza egy egyetlen, előre meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="cb98d-188">The function does not return a single, predefined format.</span></span> <span data-ttu-id="cb98d-189">Erőforrástípus tulajdonságainak megtekintéséhez a kimenetek szakaszában, a példában látható módon adja vissza az objektum.</span><span class="sxs-lookup"><span data-stu-id="cb98d-189">To see the properties for a resource type, return the object in the outputs section as shown in the example.</span></span>

### <a name="remarks"></a><span data-ttu-id="cb98d-190">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cb98d-190">Remarks</span></span>

<span data-ttu-id="cb98d-191">A hivatkozás függvény az értékét a futásidejű állapot osztályból származik, és ezért nem használható a változók szakaszban.</span><span class="sxs-lookup"><span data-stu-id="cb98d-191">The reference function derives its value from a runtime state, and therefore cannot be used in the variables section.</span></span> <span data-ttu-id="cb98d-192">A sablon kimenetének részében használható.</span><span class="sxs-lookup"><span data-stu-id="cb98d-192">It can be used in outputs section of a template.</span></span> 

<span data-ttu-id="cb98d-193">A hivatkozás függvény használatával, akkor implicit módon deklarálja, hogy egy erőforrás függ-e egy másik erőforrás, ha a hivatkozott erőforrás ugyanazt a sablont belül lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="cb98d-193">By using the reference function, you implicitly declare that one resource depends on another resource if the referenced resource is provisioned within same template.</span></span> <span data-ttu-id="cb98d-194">Nem kell a dependsOn tulajdonság is használhatja.</span><span class="sxs-lookup"><span data-stu-id="cb98d-194">You do not need to also use the dependsOn property.</span></span> <span data-ttu-id="cb98d-195">A függvény a rendszer nem értékeli ki, a hivatkozott erőforrás telepítés befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="cb98d-195">The function is not evaluated until the referenced resource has completed deployment.</span></span>

<span data-ttu-id="cb98d-196">Lásd: a tulajdonság és erőforrástípus tartozó értékek, hozzon létre egy sablont, amely a kimenetek szakaszban adja vissza az objektum.</span><span class="sxs-lookup"><span data-stu-id="cb98d-196">To see the property names and values for a resource type, create a template that returns the object in the outputs section.</span></span> <span data-ttu-id="cb98d-197">Ha az adott típusú erőforrással rendelkezik, a sablon bármely új erőforrások telepítése nélkül a objektum beállítása/beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cb98d-197">If you have an existing resource of that type, your template returns the object without deploying any new resources.</span></span> 

<span data-ttu-id="cb98d-198">Általában akkor használják a **hivatkozás** működnek, mint egy adott értéket visszaadásának egy objektumot, például a blob-végpont URI vagy teljesen minősített tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="cb98d-198">Typically, you use the **reference** function to return a particular value from an object, such as the blob endpoint URI or fully qualified domain name.</span></span>

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

### <a name="example"></a><span data-ttu-id="cb98d-199">Példa</span><span class="sxs-lookup"><span data-stu-id="cb98d-199">Example</span></span>

<span data-ttu-id="cb98d-200">Központilag telepíti, és ugyanazt a sablont az erőforrásra hivatkozik, használja:</span><span class="sxs-lookup"><span data-stu-id="cb98d-200">To deploy and reference the resource in the same template, use:</span></span>

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

<span data-ttu-id="cb98d-201">A fenti példában egy objektum a következő formátumban adja vissza:</span><span class="sxs-lookup"><span data-stu-id="cb98d-201">The preceding example returns an object in the following format:</span></span>

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

<span data-ttu-id="cb98d-202">A következő példa egy tárfiókot, amely nincs telepítve Ez a sablon hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cb98d-202">The following example references a storage account that is not deployed in this template.</span></span> <span data-ttu-id="cb98d-203">A tárfiók már létezik az ugyanazon erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="cb98d-203">The storage account already exists within the same resource group.</span></span>

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

## <a name="resourcegroup"></a><span data-ttu-id="cb98d-204">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="cb98d-204">resourceGroup</span></span>
`resourceGroup()`

<span data-ttu-id="cb98d-205">A jelenlegi erőforráscsoportban képviselő objektumot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cb98d-205">Returns an object that represents the current resource group.</span></span> 

### <a name="return-value"></a><span data-ttu-id="cb98d-206">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="cb98d-206">Return value</span></span>

<span data-ttu-id="cb98d-207">A visszaadott objektumot a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="cb98d-207">The returned object is in the following format:</span></span>

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

### <a name="remarks"></a><span data-ttu-id="cb98d-208">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cb98d-208">Remarks</span></span>

<span data-ttu-id="cb98d-209">A közös a resourceGroup függvény használata erőforrások létrehozásához és az erőforráscsoport ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="cb98d-209">A common use of the resourceGroup function is to create resources in the same location as the resource group.</span></span> <span data-ttu-id="cb98d-210">Az alábbi példában az erőforráscsoport helye rendelje hozzá a webhelyhez tartozó hely.</span><span class="sxs-lookup"><span data-stu-id="cb98d-210">The following example uses the resource group location to assign the location for a web site.</span></span>

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

### <a name="example"></a><span data-ttu-id="cb98d-211">Példa</span><span class="sxs-lookup"><span data-stu-id="cb98d-211">Example</span></span>

<span data-ttu-id="cb98d-212">A következő sablon az erőforráscsoport tulajdonságainak adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cb98d-212">The following template returns the properties of the resource group.</span></span>

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

<span data-ttu-id="cb98d-213">A fenti példában egy objektum a következő formátumban adja vissza:</span><span class="sxs-lookup"><span data-stu-id="cb98d-213">The preceding example returns an object in the following format:</span></span>

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

## <a name="resourceid"></a><span data-ttu-id="cb98d-214">resourceId</span><span class="sxs-lookup"><span data-stu-id="cb98d-214">resourceId</span></span>
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

<span data-ttu-id="cb98d-215">Az erőforrás egyedi azonosítójának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="cb98d-215">Returns the unique identifier of a resource.</span></span> <span data-ttu-id="cb98d-216">Ezt a funkciót használja, ha az erőforrás neve nem egyértelmű, vagy nem kiépített ugyanabban a sablonban.</span><span class="sxs-lookup"><span data-stu-id="cb98d-216">You use this function when the resource name is ambiguous or not provisioned within the same template.</span></span> 

### <a name="parameters"></a><span data-ttu-id="cb98d-217">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="cb98d-217">Parameters</span></span>

| <span data-ttu-id="cb98d-218">Paraméter</span><span class="sxs-lookup"><span data-stu-id="cb98d-218">Parameter</span></span> | <span data-ttu-id="cb98d-219">Szükséges</span><span class="sxs-lookup"><span data-stu-id="cb98d-219">Required</span></span> | <span data-ttu-id="cb98d-220">Típus</span><span class="sxs-lookup"><span data-stu-id="cb98d-220">Type</span></span> | <span data-ttu-id="cb98d-221">Leírás</span><span class="sxs-lookup"><span data-stu-id="cb98d-221">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="cb98d-222">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="cb98d-222">subscriptionId</span></span> |<span data-ttu-id="cb98d-223">Nem</span><span class="sxs-lookup"><span data-stu-id="cb98d-223">No</span></span> |<span data-ttu-id="cb98d-224">karakterlánc (a GUID formátumban)</span><span class="sxs-lookup"><span data-stu-id="cb98d-224">string (In GUID format)</span></span> |<span data-ttu-id="cb98d-225">Alapértelmezett érték az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="cb98d-225">Default value is the current subscription.</span></span> <span data-ttu-id="cb98d-226">Adja meg ezt az értéket, ha szüksége van egy másik előfizetésben található erőforrás lekérése.</span><span class="sxs-lookup"><span data-stu-id="cb98d-226">Specify this value when you need to retrieve a resource in another subscription.</span></span> |
| <span data-ttu-id="cb98d-227">erőforráscsoport-név</span><span class="sxs-lookup"><span data-stu-id="cb98d-227">resourceGroupName</span></span> |<span data-ttu-id="cb98d-228">Nem</span><span class="sxs-lookup"><span data-stu-id="cb98d-228">No</span></span> |<span data-ttu-id="cb98d-229">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-229">string</span></span> |<span data-ttu-id="cb98d-230">Alapértelmezett érték: a jelenlegi erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="cb98d-230">Default value is current resource group.</span></span> <span data-ttu-id="cb98d-231">Adja meg ezt az értéket, ha erőforrást egy másik erőforráscsoportban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="cb98d-231">Specify this value when you need to retrieve a resource in another resource group.</span></span> |
| <span data-ttu-id="cb98d-232">a resourceType</span><span class="sxs-lookup"><span data-stu-id="cb98d-232">resourceType</span></span> |<span data-ttu-id="cb98d-233">Igen</span><span class="sxs-lookup"><span data-stu-id="cb98d-233">Yes</span></span> |<span data-ttu-id="cb98d-234">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-234">string</span></span> |<span data-ttu-id="cb98d-235">Beleértve az erőforrás-szolgáltató névtere erőforrás típusát.</span><span class="sxs-lookup"><span data-stu-id="cb98d-235">Type of resource including resource provider namespace.</span></span> |
| <span data-ttu-id="cb98d-236">resourceName1</span><span class="sxs-lookup"><span data-stu-id="cb98d-236">resourceName1</span></span> |<span data-ttu-id="cb98d-237">Igen</span><span class="sxs-lookup"><span data-stu-id="cb98d-237">Yes</span></span> |<span data-ttu-id="cb98d-238">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-238">string</span></span> |<span data-ttu-id="cb98d-239">Erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="cb98d-239">Name of resource.</span></span> |
| <span data-ttu-id="cb98d-240">resourceName2</span><span class="sxs-lookup"><span data-stu-id="cb98d-240">resourceName2</span></span> |<span data-ttu-id="cb98d-241">Nem</span><span class="sxs-lookup"><span data-stu-id="cb98d-241">No</span></span> |<span data-ttu-id="cb98d-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-242">string</span></span> |<span data-ttu-id="cb98d-243">Következő neve erőforrásszegmensre. Ha az erőforrás van beágyazva.</span><span class="sxs-lookup"><span data-stu-id="cb98d-243">Next resource name segment if resource is nested.</span></span> |

### <a name="return-value"></a><span data-ttu-id="cb98d-244">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="cb98d-244">Return value</span></span>

<span data-ttu-id="cb98d-245">Az azonosító eredmény abban az esetben a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="cb98d-245">The identifier is returned in the following format:</span></span>

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

### <a name="remarks"></a><span data-ttu-id="cb98d-246">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cb98d-246">Remarks</span></span>

<span data-ttu-id="cb98d-247">A megadott paraméterértékek függnek, hogy az erőforrás azonos előfizetésbe és erőforráscsoportba tartozik, mint a jelenlegi üzemelő példány.</span><span class="sxs-lookup"><span data-stu-id="cb98d-247">The parameter values you specify depend on whether the resource is in the same subscription and resource group as the current deployment.</span></span>

<span data-ttu-id="cb98d-248">Az erőforrás-azonosítója egy tárfiók ugyanahhoz az előfizetéshez és erőforráscsoport megtekintéséhez használja:</span><span class="sxs-lookup"><span data-stu-id="cb98d-248">To get the resource ID for a storage account in the same subscription and resource group, use:</span></span>

```json
"[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="cb98d-249">Az erőforrás-azonosítója egy tárfiók ugyanahhoz az előfizetéshez, de egy másik erőforráscsoportban található, amelyet:</span><span class="sxs-lookup"><span data-stu-id="cb98d-249">To get the resource ID for a storage account in the same subscription but a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="cb98d-250">Az erőforrás-azonosítója egy tárfiók egy másik előfizetésben és erőforráscsoportban használatához:</span><span class="sxs-lookup"><span data-stu-id="cb98d-250">To get the resource ID for a storage account in a different subscription and resource group, use:</span></span>

```json
"[resourceId('xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', 'otherResourceGroup', 'Microsoft.Storage/storageAccounts','examplestorage')]"
```

<span data-ttu-id="cb98d-251">Az erőforrás-azonosító egy másik erőforráscsoportban található adatbázis használatához:</span><span class="sxs-lookup"><span data-stu-id="cb98d-251">To get the resource ID for a database in a different resource group, use:</span></span>

```json
"[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]"
```

<span data-ttu-id="cb98d-252">Gyakran kell használnia a függvény egy tárfiókhoz vagy a virtuális hálózat használata egy másik erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="cb98d-252">Often, you need to use this function when using a storage account or virtual network in an alternate resource group.</span></span> <span data-ttu-id="cb98d-253">A következő példa bemutatja, hogyan könnyen használható egy külső erőforráscsoportból erőforrás:</span><span class="sxs-lookup"><span data-stu-id="cb98d-253">The following example shows how a resource from an external resource group can easily be used:</span></span>

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

### <a name="example"></a><span data-ttu-id="cb98d-254">Példa</span><span class="sxs-lookup"><span data-stu-id="cb98d-254">Example</span></span>

<span data-ttu-id="cb98d-255">A következő példa az erőforrás-azonosítója egy tárfiók erőforráscsoportban adja vissza:</span><span class="sxs-lookup"><span data-stu-id="cb98d-255">The following example returns the resource ID for a storage account in the resource group:</span></span>

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

<span data-ttu-id="cb98d-256">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="cb98d-256">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="cb98d-257">Név</span><span class="sxs-lookup"><span data-stu-id="cb98d-257">Name</span></span> | <span data-ttu-id="cb98d-258">Típus</span><span class="sxs-lookup"><span data-stu-id="cb98d-258">Type</span></span> | <span data-ttu-id="cb98d-259">Érték</span><span class="sxs-lookup"><span data-stu-id="cb98d-259">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="cb98d-260">sameRGOutput</span><span class="sxs-lookup"><span data-stu-id="cb98d-260">sameRGOutput</span></span> | <span data-ttu-id="cb98d-261">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-261">String</span></span> | <span data-ttu-id="cb98d-262">/Subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/Providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="cb98d-262">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="cb98d-263">differentRGOutput</span><span class="sxs-lookup"><span data-stu-id="cb98d-263">differentRGOutput</span></span> | <span data-ttu-id="cb98d-264">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-264">String</span></span> | <span data-ttu-id="cb98d-265">/Subscriptions/{Current-Sub-ID}/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="cb98d-265">/subscriptions/{current-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="cb98d-266">differentSubOutput</span><span class="sxs-lookup"><span data-stu-id="cb98d-266">differentSubOutput</span></span> | <span data-ttu-id="cb98d-267">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-267">String</span></span> | <span data-ttu-id="cb98d-268">/Subscriptions/{different-Sub-ID}/resourceGroups/otherResourceGroup/Providers/Microsoft.Storage/storageAccounts/examplestorage</span><span class="sxs-lookup"><span data-stu-id="cb98d-268">/subscriptions/{different-sub-id}/resourceGroups/otherResourceGroup/providers/Microsoft.Storage/storageAccounts/examplestorage</span></span> |
| <span data-ttu-id="cb98d-269">nestedResourceOutput</span><span class="sxs-lookup"><span data-stu-id="cb98d-269">nestedResourceOutput</span></span> | <span data-ttu-id="cb98d-270">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="cb98d-270">String</span></span> | <span data-ttu-id="cb98d-271">/Subscriptions/{Current-Sub-ID}/resourceGroups/examplegroup/Providers/Microsoft.SQL/Servers/serverName/Databases/databaseName</span><span class="sxs-lookup"><span data-stu-id="cb98d-271">/subscriptions/{current-sub-id}/resourceGroups/examplegroup/providers/Microsoft.SQL/servers/serverName/databases/databaseName</span></span> |

<a id="subscription" />

## <a name="subscription"></a><span data-ttu-id="cb98d-272">előfizetést</span><span class="sxs-lookup"><span data-stu-id="cb98d-272">subscription</span></span>
`subscription()`

<span data-ttu-id="cb98d-273">Az előfizetés, a jelenlegi üzemelő példány részleteit adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cb98d-273">Returns details about the subscription for the current deployment.</span></span> 

### <a name="return-value"></a><span data-ttu-id="cb98d-274">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="cb98d-274">Return value</span></span>

<span data-ttu-id="cb98d-275">A függvény a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="cb98d-275">The function returns the following format:</span></span>

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

### <a name="example"></a><span data-ttu-id="cb98d-276">Példa</span><span class="sxs-lookup"><span data-stu-id="cb98d-276">Example</span></span>

<span data-ttu-id="cb98d-277">A következő példa bemutatja a előfizetés függvény hívása a kimenetek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="cb98d-277">The following example shows the subscription function called in the outputs section.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="cb98d-278">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb98d-278">Next steps</span></span>
* <span data-ttu-id="cb98d-279">A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="cb98d-279">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="cb98d-280">Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="cb98d-280">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="cb98d-281">Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="cb98d-281">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="cb98d-282">A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="cb98d-282">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


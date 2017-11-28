---
title: "Az Azure Resource Manager sablonfüggvényei - telepítés |} Microsoft Docs"
description: "A központi telepítési információk beolvasása az Azure Resource Manager-sablonok használatára funkcióit ismerteti."
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
ms.date: 06/13/2017
ms.author: tomfitz
ms.openlocfilehash: d7e6bcd669d40cb19de44b646505856ecd8f51a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="4908d-103">Központi telepítési funkciók az Azure Resource Manager sablonokhoz</span><span class="sxs-lookup"><span data-stu-id="4908d-103">Deployment functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="4908d-104">Erőforrás-kezelő a következő funkciókat nyújt értékek lekérése a sablon és a központi telepítéshez kapcsolódó értékek szakaszait:</span><span class="sxs-lookup"><span data-stu-id="4908d-104">Resource Manager provides the following functions for getting values from sections of the template and values related to the deployment:</span></span>

* [<span data-ttu-id="4908d-105">központi telepítés</span><span class="sxs-lookup"><span data-stu-id="4908d-105">deployment</span></span>](#deployment)
* [<span data-ttu-id="4908d-106">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="4908d-106">parameters</span></span>](#parameters)
* [<span data-ttu-id="4908d-107">változók</span><span class="sxs-lookup"><span data-stu-id="4908d-107">variables</span></span>](#variables)

<span data-ttu-id="4908d-108">Értékek erőforrásokat, erőforráscsoport-sablonok vagy előfizetések megtekinteni [erőforrás funkciók](resource-group-template-functions-resource.md).</span><span class="sxs-lookup"><span data-stu-id="4908d-108">To get values from resources, resource groups, or subscriptions, see [Resource functions](resource-group-template-functions-resource.md).</span></span>

<a id="deployment" />

## <a name="deployment"></a><span data-ttu-id="4908d-109">üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="4908d-109">deployment</span></span>
`deployment()`

<span data-ttu-id="4908d-110">Az aktuális központi telepítési művelet információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4908d-110">Returns information about the current deployment operation.</span></span>

### <a name="return-value"></a><span data-ttu-id="4908d-111">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="4908d-111">Return value</span></span>

<span data-ttu-id="4908d-112">Ez a funkció a telepítés során átadott objektum adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4908d-112">This function returns the object that is passed during deployment.</span></span> <span data-ttu-id="4908d-113">A tulajdonságokat a visszaadott objektumot az attól függően változnak, hogy a központi telepítési objektum közlekednek hivatkozásként vagy beágyazott objektumként.</span><span class="sxs-lookup"><span data-stu-id="4908d-113">The properties in the returned object differ based on whether the deployment object is passed as a link or as an in-line object.</span></span> <span data-ttu-id="4908d-114">Ha a központi telepítési objektum átadása egysoros, többek között használata esetén a **- TemplateFile** az Azure PowerShell helyi fájlra mutat, a visszaadott objektumot paraméternek a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="4908d-114">When the deployment object is passed in-line, such as when using the **-TemplateFile** parameter in Azure PowerShell to point to a local file, the returned object has the following format:</span></span>

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

<span data-ttu-id="4908d-115">Ha az objektum átadása hivatkozásként, például a használatakor a **- TemplateUri** paraméterrel a távoli objektumra mutat az objektumot ad vissza a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="4908d-115">When the object is passed as a link, such as when using the **-TemplateUri** parameter to point to a remote object, the object is returned in the following format:</span></span> 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a><span data-ttu-id="4908d-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4908d-116">Remarks</span></span>

<span data-ttu-id="4908d-117">Deployment() használatával kapcsolódik egy másik sablont, az URI a szülő-sablon alapján.</span><span class="sxs-lookup"><span data-stu-id="4908d-117">You can use deployment() to link to another template based on the URI of the parent template.</span></span>

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a><span data-ttu-id="4908d-118">Példa</span><span class="sxs-lookup"><span data-stu-id="4908d-118">Example</span></span>

<span data-ttu-id="4908d-119">A következő példa a központi telepítési objektum beállítása/beolvasása:</span><span class="sxs-lookup"><span data-stu-id="4908d-119">The following example returns the deployment object:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="4908d-120">A fenti példa adja a következő objektumot:</span><span class="sxs-lookup"><span data-stu-id="4908d-120">The preceding example returns the following object:</span></span>

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

<a id="parameters" />

## <a name="parameters"></a><span data-ttu-id="4908d-121">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="4908d-121">parameters</span></span>
`parameters(parameterName)`

<span data-ttu-id="4908d-122">A paraméter értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4908d-122">Returns a parameter value.</span></span> <span data-ttu-id="4908d-123">A megadott paraméternév definiálni kell a sablon a Paraméterek szakaszban.</span><span class="sxs-lookup"><span data-stu-id="4908d-123">The specified parameter name must be defined in the parameters section of the template.</span></span>

### <a name="parameters"></a><span data-ttu-id="4908d-124">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="4908d-124">Parameters</span></span>

| <span data-ttu-id="4908d-125">Paraméter</span><span class="sxs-lookup"><span data-stu-id="4908d-125">Parameter</span></span> | <span data-ttu-id="4908d-126">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4908d-126">Required</span></span> | <span data-ttu-id="4908d-127">Típus</span><span class="sxs-lookup"><span data-stu-id="4908d-127">Type</span></span> | <span data-ttu-id="4908d-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="4908d-128">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4908d-129">parameterName</span><span class="sxs-lookup"><span data-stu-id="4908d-129">parameterName</span></span> |<span data-ttu-id="4908d-130">Igen</span><span class="sxs-lookup"><span data-stu-id="4908d-130">Yes</span></span> |<span data-ttu-id="4908d-131">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4908d-131">string</span></span> |<span data-ttu-id="4908d-132">Vissza a paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="4908d-132">The name of the parameter to return.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4908d-133">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="4908d-133">Return value</span></span>

<span data-ttu-id="4908d-134">A megadott paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="4908d-134">The value of the specified parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="4908d-135">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4908d-135">Remarks</span></span>

<span data-ttu-id="4908d-136">Általában használja a paraméterek erőforrás értékeinek beállításához.</span><span class="sxs-lookup"><span data-stu-id="4908d-136">Typically, you use parameters to set resource values.</span></span> <span data-ttu-id="4908d-137">A következő példa a telepítés során az átadott paraméter értéke a webhely nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="4908d-137">The following example sets the name of web site to the parameter value passed in during deployment.</span></span>

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a><span data-ttu-id="4908d-138">Példa</span><span class="sxs-lookup"><span data-stu-id="4908d-138">Example</span></span>

<span data-ttu-id="4908d-139">A következő példa bemutatja a paraméterek függvény egy egyszerűsített használata.</span><span class="sxs-lookup"><span data-stu-id="4908d-139">The following example shows a simplified use of the parameters function.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

<span data-ttu-id="4908d-140">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="4908d-140">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4908d-141">Név</span><span class="sxs-lookup"><span data-stu-id="4908d-141">Name</span></span> | <span data-ttu-id="4908d-142">Típus</span><span class="sxs-lookup"><span data-stu-id="4908d-142">Type</span></span> | <span data-ttu-id="4908d-143">Érték</span><span class="sxs-lookup"><span data-stu-id="4908d-143">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4908d-144">stringOutput</span><span class="sxs-lookup"><span data-stu-id="4908d-144">stringOutput</span></span> | <span data-ttu-id="4908d-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4908d-145">String</span></span> | <span data-ttu-id="4908d-146">1. lehetőséget</span><span class="sxs-lookup"><span data-stu-id="4908d-146">option 1</span></span> |
| <span data-ttu-id="4908d-147">intOutput</span><span class="sxs-lookup"><span data-stu-id="4908d-147">intOutput</span></span> | <span data-ttu-id="4908d-148">int</span><span class="sxs-lookup"><span data-stu-id="4908d-148">Int</span></span> | <span data-ttu-id="4908d-149">1</span><span class="sxs-lookup"><span data-stu-id="4908d-149">1</span></span> |
| <span data-ttu-id="4908d-150">objectOutput</span><span class="sxs-lookup"><span data-stu-id="4908d-150">objectOutput</span></span> | <span data-ttu-id="4908d-151">Objektum</span><span class="sxs-lookup"><span data-stu-id="4908d-151">Object</span></span> | <span data-ttu-id="4908d-152">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="4908d-152">{"one": "a", "two": "b"}</span></span> |
| <span data-ttu-id="4908d-153">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="4908d-153">arrayOutput</span></span> | <span data-ttu-id="4908d-154">A tömb</span><span class="sxs-lookup"><span data-stu-id="4908d-154">Array</span></span> | <span data-ttu-id="4908d-155">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="4908d-155">[1, 2, 3]</span></span> |
| <span data-ttu-id="4908d-156">crossOutput</span><span class="sxs-lookup"><span data-stu-id="4908d-156">crossOutput</span></span> | <span data-ttu-id="4908d-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4908d-157">String</span></span> | <span data-ttu-id="4908d-158">1. lehetőséget</span><span class="sxs-lookup"><span data-stu-id="4908d-158">option 1</span></span> |

<a id="variables" />

## <a name="variables"></a><span data-ttu-id="4908d-159">változók</span><span class="sxs-lookup"><span data-stu-id="4908d-159">variables</span></span>
`variables(variableName)`

<span data-ttu-id="4908d-160">A változó értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4908d-160">Returns the value of variable.</span></span> <span data-ttu-id="4908d-161">A változók szakaszban a sablon a megadott változónév definiálni kell.</span><span class="sxs-lookup"><span data-stu-id="4908d-161">The specified variable name must be defined in the variables section of the template.</span></span>

### <a name="parameters"></a><span data-ttu-id="4908d-162">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="4908d-162">Parameters</span></span>

| <span data-ttu-id="4908d-163">Paraméter</span><span class="sxs-lookup"><span data-stu-id="4908d-163">Parameter</span></span> | <span data-ttu-id="4908d-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="4908d-164">Required</span></span> | <span data-ttu-id="4908d-165">Típus</span><span class="sxs-lookup"><span data-stu-id="4908d-165">Type</span></span> | <span data-ttu-id="4908d-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="4908d-166">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="4908d-167">variableName</span><span class="sxs-lookup"><span data-stu-id="4908d-167">variableName</span></span> |<span data-ttu-id="4908d-168">Igen</span><span class="sxs-lookup"><span data-stu-id="4908d-168">Yes</span></span> |<span data-ttu-id="4908d-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4908d-169">String</span></span> |<span data-ttu-id="4908d-170">Vissza a változó neve.</span><span class="sxs-lookup"><span data-stu-id="4908d-170">The name of the variable to return.</span></span> |

### <a name="return-value"></a><span data-ttu-id="4908d-171">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="4908d-171">Return value</span></span>

<span data-ttu-id="4908d-172">A megadott változó értékét.</span><span class="sxs-lookup"><span data-stu-id="4908d-172">The value of the specified variable.</span></span>

### <a name="remarks"></a><span data-ttu-id="4908d-173">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4908d-173">Remarks</span></span>

<span data-ttu-id="4908d-174">Általában a változókkal egyszerűbbé teheti a sablont hozhat létre csak egyszer összetett értékeket.</span><span class="sxs-lookup"><span data-stu-id="4908d-174">Typically, you use variables to simplify your template by constructing complex values only once.</span></span> <span data-ttu-id="4908d-175">A következő példa egy egyedi nevet a tárfiók hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4908d-175">The following example constructs a unique name for a storage account.</span></span>

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a><span data-ttu-id="4908d-176">Példa</span><span class="sxs-lookup"><span data-stu-id="4908d-176">Example</span></span>

<span data-ttu-id="4908d-177">A példa sablon más változók értékeinek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4908d-177">The example template returns different variable values.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

<span data-ttu-id="4908d-178">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="4908d-178">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="4908d-179">Név</span><span class="sxs-lookup"><span data-stu-id="4908d-179">Name</span></span> | <span data-ttu-id="4908d-180">Típus</span><span class="sxs-lookup"><span data-stu-id="4908d-180">Type</span></span> | <span data-ttu-id="4908d-181">Érték</span><span class="sxs-lookup"><span data-stu-id="4908d-181">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="4908d-182">exampleOutput1</span><span class="sxs-lookup"><span data-stu-id="4908d-182">exampleOutput1</span></span> | <span data-ttu-id="4908d-183">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4908d-183">String</span></span> | <span data-ttu-id="4908d-184">myVariable</span><span class="sxs-lookup"><span data-stu-id="4908d-184">myVariable</span></span> |
| <span data-ttu-id="4908d-185">exampleOutput2</span><span class="sxs-lookup"><span data-stu-id="4908d-185">exampleOutput2</span></span> | <span data-ttu-id="4908d-186">A tömb</span><span class="sxs-lookup"><span data-stu-id="4908d-186">Array</span></span> | <span data-ttu-id="4908d-187">[1, 2, 3, 4]</span><span class="sxs-lookup"><span data-stu-id="4908d-187">[1, 2, 3, 4]</span></span> |
| <span data-ttu-id="4908d-188">exampleOutput3</span><span class="sxs-lookup"><span data-stu-id="4908d-188">exampleOutput3</span></span> | <span data-ttu-id="4908d-189">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4908d-189">String</span></span> | <span data-ttu-id="4908d-190">myVariable</span><span class="sxs-lookup"><span data-stu-id="4908d-190">myVariable</span></span> |
| <span data-ttu-id="4908d-191">exampleOutput4</span><span class="sxs-lookup"><span data-stu-id="4908d-191">exampleOutput4</span></span> |  <span data-ttu-id="4908d-192">Objektum</span><span class="sxs-lookup"><span data-stu-id="4908d-192">Object</span></span> | <span data-ttu-id="4908d-193">{"Tulajdonság1": "érték1", "Tulajdonság2": "érték2"}</span><span class="sxs-lookup"><span data-stu-id="4908d-193">{"property1": "value1", "property2": "value2"}</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4908d-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4908d-194">Next steps</span></span>
* <span data-ttu-id="4908d-195">A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4908d-195">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="4908d-196">Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="4908d-196">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="4908d-197">Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="4908d-197">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="4908d-198">A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="4908d-198">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


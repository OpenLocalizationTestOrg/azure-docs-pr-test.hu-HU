---
title: "aaaAzure Resource Manager sablonfüggvényei - telepítés |} Microsoft Docs"
description: "Az Azure Resource Manager sablon tooretrieve telepítési információi hello funkciók toouse ismerteti."
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
ms.openlocfilehash: 458c3f740504fdd6799ed24cc386219726737636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="7c0d4-103">Központi telepítési funkciók az Azure Resource Manager sablonokhoz</span><span class="sxs-lookup"><span data-stu-id="7c0d4-103">Deployment functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="7c0d4-104">A Resource Manager biztosít hello alábbi az értékek lekérése hello sablon szakaszok működik, és kapcsolódó toohello telepítési értékeket:</span><span class="sxs-lookup"><span data-stu-id="7c0d4-104">Resource Manager provides hello following functions for getting values from sections of hello template and values related toohello deployment:</span></span>

* [<span data-ttu-id="7c0d4-105">központi telepítés</span><span class="sxs-lookup"><span data-stu-id="7c0d4-105">deployment</span></span>](#deployment)
* [<span data-ttu-id="7c0d4-106">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="7c0d4-106">parameters</span></span>](#parameters)
* [<span data-ttu-id="7c0d4-107">változók</span><span class="sxs-lookup"><span data-stu-id="7c0d4-107">variables</span></span>](#variables)

<span data-ttu-id="7c0d4-108">tooget értékek erőforrásokat, erőforráscsoport-sablonok vagy előfizetések, lásd: [erőforrás funkciók](resource-group-template-functions-resource.md).</span><span class="sxs-lookup"><span data-stu-id="7c0d4-108">tooget values from resources, resource groups, or subscriptions, see [Resource functions](resource-group-template-functions-resource.md).</span></span>

<a id="deployment" />

## <a name="deployment"></a><span data-ttu-id="7c0d4-109">üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="7c0d4-109">deployment</span></span>
`deployment()`

<span data-ttu-id="7c0d4-110">Hello aktuális telepítési művelet információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-110">Returns information about hello current deployment operation.</span></span>

### <a name="return-value"></a><span data-ttu-id="7c0d4-111">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="7c0d4-111">Return value</span></span>

<span data-ttu-id="7c0d4-112">Ez a funkció a telepítés során átadott hello-objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-112">This function returns hello object that is passed during deployment.</span></span> <span data-ttu-id="7c0d4-113">hello objektumot adott vissza a hello tulajdonságok attól függően változnak, hogy hello központi telepítési objektum közlekednek hivatkozásként vagy beágyazott objektumként.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-113">hello properties in hello returned object differ based on whether hello deployment object is passed as a link or as an in-line object.</span></span> <span data-ttu-id="7c0d4-114">Ha hello központi telepítési objektum átadása egysoros, többek között hello használatakor **- TemplateFile** paraméter az Azure PowerShell toopoint tooa helyi fájlban hello visszaadott objektumnak hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="7c0d4-114">When hello deployment object is passed in-line, such as when using hello **-TemplateFile** parameter in Azure PowerShell toopoint tooa local file, hello returned object has hello following format:</span></span>

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

<span data-ttu-id="7c0d4-115">Ha hello objektum átadása hivatkozásként, például amikor használatával hello **- TemplateUri** paraméter toopoint tooa távoli objektum hello objektum eredmény abban az esetben a következő formátumban hello:</span><span class="sxs-lookup"><span data-stu-id="7c0d4-115">When hello object is passed as a link, such as when using hello **-TemplateUri** parameter toopoint tooa remote object, hello object is returned in hello following format:</span></span> 

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

### <a name="remarks"></a><span data-ttu-id="7c0d4-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7c0d4-116">Remarks</span></span>

<span data-ttu-id="7c0d4-117">Deployment() toolink tooanother sablon hello URI hello szülő sablon alapján is használhatja.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-117">You can use deployment() toolink tooanother template based on hello URI of hello parent template.</span></span>

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a><span data-ttu-id="7c0d4-118">Példa</span><span class="sxs-lookup"><span data-stu-id="7c0d4-118">Example</span></span>

<span data-ttu-id="7c0d4-119">hello alábbi példa hello központi telepítési objektum beállítása/beolvasása:</span><span class="sxs-lookup"><span data-stu-id="7c0d4-119">hello following example returns hello deployment object:</span></span>

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

<span data-ttu-id="7c0d4-120">hello előző példa adja vissza a következő objektum hello:</span><span class="sxs-lookup"><span data-stu-id="7c0d4-120">hello preceding example returns hello following object:</span></span>

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

## <a name="parameters"></a><span data-ttu-id="7c0d4-121">paraméterek</span><span class="sxs-lookup"><span data-stu-id="7c0d4-121">parameters</span></span>
`parameters(parameterName)`

<span data-ttu-id="7c0d4-122">A paraméter értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-122">Returns a parameter value.</span></span> <span data-ttu-id="7c0d4-123">hello megadott paraméternév hello paraméterek szakaszban hello sablon definiálni kell.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-123">hello specified parameter name must be defined in hello parameters section of hello template.</span></span>

### <a name="parameters"></a><span data-ttu-id="7c0d4-124">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="7c0d4-124">Parameters</span></span>

| <span data-ttu-id="7c0d4-125">Paraméter</span><span class="sxs-lookup"><span data-stu-id="7c0d4-125">Parameter</span></span> | <span data-ttu-id="7c0d4-126">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7c0d4-126">Required</span></span> | <span data-ttu-id="7c0d4-127">Típus</span><span class="sxs-lookup"><span data-stu-id="7c0d4-127">Type</span></span> | <span data-ttu-id="7c0d4-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="7c0d4-128">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="7c0d4-129">parameterName</span><span class="sxs-lookup"><span data-stu-id="7c0d4-129">parameterName</span></span> |<span data-ttu-id="7c0d4-130">Igen</span><span class="sxs-lookup"><span data-stu-id="7c0d4-130">Yes</span></span> |<span data-ttu-id="7c0d4-131">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c0d4-131">string</span></span> |<span data-ttu-id="7c0d4-132">hello paraméter tooreturn hello neve.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-132">hello name of hello parameter tooreturn.</span></span> |

### <a name="return-value"></a><span data-ttu-id="7c0d4-133">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="7c0d4-133">Return value</span></span>

<span data-ttu-id="7c0d4-134">hello hello értéket adott paraméter.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-134">hello value of hello specified parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="7c0d4-135">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7c0d4-135">Remarks</span></span>

<span data-ttu-id="7c0d4-136">Általában akkor használják tooset erőforrás paraméterértéket.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-136">Typically, you use parameters tooset resource values.</span></span> <span data-ttu-id="7c0d4-137">hello alábbi mintakód webhely toohello paraméter értéke üzembe helyezése során átadott hello nevét.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-137">hello following example sets hello name of web site toohello parameter value passed in during deployment.</span></span>

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

### <a name="example"></a><span data-ttu-id="7c0d4-138">Példa</span><span class="sxs-lookup"><span data-stu-id="7c0d4-138">Example</span></span>

<span data-ttu-id="7c0d4-139">hello következő példa bemutatja egy egyszerűsített hello paraméterek függvény használata.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-139">hello following example shows a simplified use of hello parameters function.</span></span>

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

<span data-ttu-id="7c0d4-140">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="7c0d4-140">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="7c0d4-141">Név</span><span class="sxs-lookup"><span data-stu-id="7c0d4-141">Name</span></span> | <span data-ttu-id="7c0d4-142">Típus</span><span class="sxs-lookup"><span data-stu-id="7c0d4-142">Type</span></span> | <span data-ttu-id="7c0d4-143">Érték</span><span class="sxs-lookup"><span data-stu-id="7c0d4-143">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7c0d4-144">stringOutput</span><span class="sxs-lookup"><span data-stu-id="7c0d4-144">stringOutput</span></span> | <span data-ttu-id="7c0d4-145">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c0d4-145">String</span></span> | <span data-ttu-id="7c0d4-146">1. lehetőséget</span><span class="sxs-lookup"><span data-stu-id="7c0d4-146">option 1</span></span> |
| <span data-ttu-id="7c0d4-147">intOutput</span><span class="sxs-lookup"><span data-stu-id="7c0d4-147">intOutput</span></span> | <span data-ttu-id="7c0d4-148">int</span><span class="sxs-lookup"><span data-stu-id="7c0d4-148">Int</span></span> | <span data-ttu-id="7c0d4-149">1</span><span class="sxs-lookup"><span data-stu-id="7c0d4-149">1</span></span> |
| <span data-ttu-id="7c0d4-150">objectOutput</span><span class="sxs-lookup"><span data-stu-id="7c0d4-150">objectOutput</span></span> | <span data-ttu-id="7c0d4-151">Objektum</span><span class="sxs-lookup"><span data-stu-id="7c0d4-151">Object</span></span> | <span data-ttu-id="7c0d4-152">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="7c0d4-152">{"one": "a", "two": "b"}</span></span> |
| <span data-ttu-id="7c0d4-153">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="7c0d4-153">arrayOutput</span></span> | <span data-ttu-id="7c0d4-154">Tömb</span><span class="sxs-lookup"><span data-stu-id="7c0d4-154">Array</span></span> | <span data-ttu-id="7c0d4-155">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="7c0d4-155">[1, 2, 3]</span></span> |
| <span data-ttu-id="7c0d4-156">crossOutput</span><span class="sxs-lookup"><span data-stu-id="7c0d4-156">crossOutput</span></span> | <span data-ttu-id="7c0d4-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c0d4-157">String</span></span> | <span data-ttu-id="7c0d4-158">1. lehetőséget</span><span class="sxs-lookup"><span data-stu-id="7c0d4-158">option 1</span></span> |

<a id="variables" />

## <a name="variables"></a><span data-ttu-id="7c0d4-159">változók</span><span class="sxs-lookup"><span data-stu-id="7c0d4-159">variables</span></span>
`variables(variableName)`

<span data-ttu-id="7c0d4-160">Beolvasása hello változó értékét.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-160">Returns hello value of variable.</span></span> <span data-ttu-id="7c0d4-161">hello megadott változónév hello változók szakaszban hello sablon definiálni kell.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-161">hello specified variable name must be defined in hello variables section of hello template.</span></span>

### <a name="parameters"></a><span data-ttu-id="7c0d4-162">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="7c0d4-162">Parameters</span></span>

| <span data-ttu-id="7c0d4-163">Paraméter</span><span class="sxs-lookup"><span data-stu-id="7c0d4-163">Parameter</span></span> | <span data-ttu-id="7c0d4-164">Szükséges</span><span class="sxs-lookup"><span data-stu-id="7c0d4-164">Required</span></span> | <span data-ttu-id="7c0d4-165">Típus</span><span class="sxs-lookup"><span data-stu-id="7c0d4-165">Type</span></span> | <span data-ttu-id="7c0d4-166">Leírás</span><span class="sxs-lookup"><span data-stu-id="7c0d4-166">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="7c0d4-167">variableName</span><span class="sxs-lookup"><span data-stu-id="7c0d4-167">variableName</span></span> |<span data-ttu-id="7c0d4-168">Igen</span><span class="sxs-lookup"><span data-stu-id="7c0d4-168">Yes</span></span> |<span data-ttu-id="7c0d4-169">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c0d4-169">String</span></span> |<span data-ttu-id="7c0d4-170">hello változó tooreturn hello neve.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-170">hello name of hello variable tooreturn.</span></span> |

### <a name="return-value"></a><span data-ttu-id="7c0d4-171">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="7c0d4-171">Return value</span></span>

<span data-ttu-id="7c0d4-172">hello hello megadott változó értékét.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-172">hello value of hello specified variable.</span></span>

### <a name="remarks"></a><span data-ttu-id="7c0d4-173">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7c0d4-173">Remarks</span></span>

<span data-ttu-id="7c0d4-174">Általában akkor használják változók toosimplify a sablont hozhat létre csak egyszer összetett értékeket.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-174">Typically, you use variables toosimplify your template by constructing complex values only once.</span></span> <span data-ttu-id="7c0d4-175">hello alábbi példa hoz létre egy egyedi nevet a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-175">hello following example constructs a unique name for a storage account.</span></span>

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

### <a name="example"></a><span data-ttu-id="7c0d4-176">Példa</span><span class="sxs-lookup"><span data-stu-id="7c0d4-176">Example</span></span>

<span data-ttu-id="7c0d4-177">hello példa sablon más változók értékeinek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7c0d4-177">hello example template returns different variable values.</span></span>

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

<span data-ttu-id="7c0d4-178">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="7c0d4-178">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="7c0d4-179">Név</span><span class="sxs-lookup"><span data-stu-id="7c0d4-179">Name</span></span> | <span data-ttu-id="7c0d4-180">Típus</span><span class="sxs-lookup"><span data-stu-id="7c0d4-180">Type</span></span> | <span data-ttu-id="7c0d4-181">Érték</span><span class="sxs-lookup"><span data-stu-id="7c0d4-181">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="7c0d4-182">exampleOutput1</span><span class="sxs-lookup"><span data-stu-id="7c0d4-182">exampleOutput1</span></span> | <span data-ttu-id="7c0d4-183">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c0d4-183">String</span></span> | <span data-ttu-id="7c0d4-184">myVariable</span><span class="sxs-lookup"><span data-stu-id="7c0d4-184">myVariable</span></span> |
| <span data-ttu-id="7c0d4-185">exampleOutput2</span><span class="sxs-lookup"><span data-stu-id="7c0d4-185">exampleOutput2</span></span> | <span data-ttu-id="7c0d4-186">Tömb</span><span class="sxs-lookup"><span data-stu-id="7c0d4-186">Array</span></span> | <span data-ttu-id="7c0d4-187">[1, 2, 3, 4]</span><span class="sxs-lookup"><span data-stu-id="7c0d4-187">[1, 2, 3, 4]</span></span> |
| <span data-ttu-id="7c0d4-188">exampleOutput3</span><span class="sxs-lookup"><span data-stu-id="7c0d4-188">exampleOutput3</span></span> | <span data-ttu-id="7c0d4-189">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7c0d4-189">String</span></span> | <span data-ttu-id="7c0d4-190">myVariable</span><span class="sxs-lookup"><span data-stu-id="7c0d4-190">myVariable</span></span> |
| <span data-ttu-id="7c0d4-191">exampleOutput4</span><span class="sxs-lookup"><span data-stu-id="7c0d4-191">exampleOutput4</span></span> |  <span data-ttu-id="7c0d4-192">Objektum</span><span class="sxs-lookup"><span data-stu-id="7c0d4-192">Object</span></span> | <span data-ttu-id="7c0d4-193">{"Tulajdonság1": "érték1", "Tulajdonság2": "érték2"}</span><span class="sxs-lookup"><span data-stu-id="7c0d4-193">{"property1": "value1", "property2": "value2"}</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7c0d4-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c0d4-194">Next steps</span></span>
* <span data-ttu-id="7c0d4-195">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7c0d4-195">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="7c0d4-196">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7c0d4-196">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="7c0d4-197">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="7c0d4-197">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="7c0d4-198">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="7c0d4-198">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


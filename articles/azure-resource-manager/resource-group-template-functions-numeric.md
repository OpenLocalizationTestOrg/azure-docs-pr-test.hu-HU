---
title: "Az Azure Resource Manager sablonfüggvényei - numerikus |} Microsoft Docs"
description: "Az Azure Resource Manager-sablonok segítségével számok dolgozni funkcióit ismerteti."
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
ms.openlocfilehash: ae0261134b8d4a934048f58d6c679a48a904950b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="13be1-103">Az Azure Resource Manager sablonokhoz numerikus funkciók</span><span class="sxs-lookup"><span data-stu-id="13be1-103">Numeric functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="13be1-104">Erőforrás-kezelő a következő funkciókat nyújt egész számok használata:</span><span class="sxs-lookup"><span data-stu-id="13be1-104">Resource Manager provides the following functions for working with integers:</span></span>

* [<span data-ttu-id="13be1-105">hozzáadása</span><span class="sxs-lookup"><span data-stu-id="13be1-105">add</span></span>](#add)
* [<span data-ttu-id="13be1-106">copyIndex</span><span class="sxs-lookup"><span data-stu-id="13be1-106">copyIndex</span></span>](#copyindex)
* [<span data-ttu-id="13be1-107">DIV</span><span class="sxs-lookup"><span data-stu-id="13be1-107">div</span></span>](#div)
* [<span data-ttu-id="13be1-108">lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="13be1-108">float</span></span>](#float)
* [<span data-ttu-id="13be1-109">int</span><span class="sxs-lookup"><span data-stu-id="13be1-109">int</span></span>](#int)
* [<span data-ttu-id="13be1-110">perc</span><span class="sxs-lookup"><span data-stu-id="13be1-110">min</span></span>](#min)
* [<span data-ttu-id="13be1-111">maximális</span><span class="sxs-lookup"><span data-stu-id="13be1-111">max</span></span>](#max)
* [<span data-ttu-id="13be1-112">MOD</span><span class="sxs-lookup"><span data-stu-id="13be1-112">mod</span></span>](#mod)
* [<span data-ttu-id="13be1-113">MUL számú</span><span class="sxs-lookup"><span data-stu-id="13be1-113">mul</span></span>](#mul)
* [<span data-ttu-id="13be1-114">Sub</span><span class="sxs-lookup"><span data-stu-id="13be1-114">sub</span></span>](#sub)

<a id="add" />

## <a name="add"></a><span data-ttu-id="13be1-115">Hozzáadása</span><span class="sxs-lookup"><span data-stu-id="13be1-115">add</span></span>
`add(operand1, operand2)`

<span data-ttu-id="13be1-116">A két megadott egész számok összegét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-116">Returns the sum of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-117">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-117">Parameters</span></span>

| <span data-ttu-id="13be1-118">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-118">Parameter</span></span> | <span data-ttu-id="13be1-119">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-119">Required</span></span> | <span data-ttu-id="13be1-120">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-120">Type</span></span> | <span data-ttu-id="13be1-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-121">Description</span></span> |
|:--- |:--- |:--- |:--- | 
|<span data-ttu-id="13be1-122">operand1</span><span class="sxs-lookup"><span data-stu-id="13be1-122">operand1</span></span> |<span data-ttu-id="13be1-123">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-123">Yes</span></span> |<span data-ttu-id="13be1-124">int</span><span class="sxs-lookup"><span data-stu-id="13be1-124">int</span></span> |<span data-ttu-id="13be1-125">Első számú hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="13be1-125">First number to add.</span></span> |
|<span data-ttu-id="13be1-126">operand2</span><span class="sxs-lookup"><span data-stu-id="13be1-126">operand2</span></span> |<span data-ttu-id="13be1-127">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-127">Yes</span></span> |<span data-ttu-id="13be1-128">int</span><span class="sxs-lookup"><span data-stu-id="13be1-128">int</span></span> |<span data-ttu-id="13be1-129">Adja hozzá a második szám.</span><span class="sxs-lookup"><span data-stu-id="13be1-129">Second number to add.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-130">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-130">Return value</span></span>

<span data-ttu-id="13be1-131">Egész szám, amely tartalmazza a paraméterek számának összege.</span><span class="sxs-lookup"><span data-stu-id="13be1-131">An integer that contains the sum of the parameters.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-132">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-132">Example</span></span>

<span data-ttu-id="13be1-133">A következő példakóddal felveheti a két paramétert.</span><span class="sxs-lookup"><span data-stu-id="13be1-133">The following example adds two parameters.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to add"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to add"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

<span data-ttu-id="13be1-134">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-134">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-135">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-135">Name</span></span> | <span data-ttu-id="13be1-136">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-136">Type</span></span> | <span data-ttu-id="13be1-137">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-137">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-138">addResult</span><span class="sxs-lookup"><span data-stu-id="13be1-138">addResult</span></span> | <span data-ttu-id="13be1-139">int</span><span class="sxs-lookup"><span data-stu-id="13be1-139">Int</span></span> | <span data-ttu-id="13be1-140">8</span><span class="sxs-lookup"><span data-stu-id="13be1-140">8</span></span> |

<a id="copyindex" />

## <a name="copyindex"></a><span data-ttu-id="13be1-141">copyIndex</span><span class="sxs-lookup"><span data-stu-id="13be1-141">copyIndex</span></span>
`copyIndex(loopName, offset)`

<span data-ttu-id="13be1-142">Egy iteráció hurok indexét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-142">Returns the index of an iteration loop.</span></span> 

### <a name="parameters"></a><span data-ttu-id="13be1-143">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-143">Parameters</span></span>

| <span data-ttu-id="13be1-144">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-144">Parameter</span></span> | <span data-ttu-id="13be1-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-145">Required</span></span> | <span data-ttu-id="13be1-146">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-146">Type</span></span> | <span data-ttu-id="13be1-147">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-147">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-148">loopName</span><span class="sxs-lookup"><span data-stu-id="13be1-148">loopName</span></span> | <span data-ttu-id="13be1-149">Nem</span><span class="sxs-lookup"><span data-stu-id="13be1-149">No</span></span> | <span data-ttu-id="13be1-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="13be1-150">string</span></span> | <span data-ttu-id="13be1-151">Neve a ciklus ismétléseinek beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="13be1-151">The name of the loop for getting the iteration.</span></span> |
| <span data-ttu-id="13be1-152">Az offset</span><span class="sxs-lookup"><span data-stu-id="13be1-152">offset</span></span> |<span data-ttu-id="13be1-153">Nem</span><span class="sxs-lookup"><span data-stu-id="13be1-153">No</span></span> |<span data-ttu-id="13be1-154">int</span><span class="sxs-lookup"><span data-stu-id="13be1-154">int</span></span> |<span data-ttu-id="13be1-155">Az a szám, a nulla alapú ismétlési érték hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="13be1-155">The number to add to the zero-based iteration value.</span></span> |

### <a name="remarks"></a><span data-ttu-id="13be1-156">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="13be1-156">Remarks</span></span>

<span data-ttu-id="13be1-157">Ez a funkció mindig használatos a **másolási** objektum.</span><span class="sxs-lookup"><span data-stu-id="13be1-157">This function is always used with a **copy** object.</span></span> <span data-ttu-id="13be1-158">Ha nincs érték megadva, a **eltolás**, az aktuális iterációs értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-158">If no value is provided for **offset**, the current iteration value is returned.</span></span> <span data-ttu-id="13be1-159">Az ismétlési érték nulla kezdődik.</span><span class="sxs-lookup"><span data-stu-id="13be1-159">The iteration value starts at zero.</span></span>

<span data-ttu-id="13be1-160">A **loopName** tulajdonság lehetővé teszi adja meg, hogy copyIndex erőforrás iterációs vagy tulajdonság iterációs hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="13be1-160">The **loopName** property enables you to specify whether copyIndex is referring to a resource iteration or property iteration.</span></span> <span data-ttu-id="13be1-161">Ha nincs érték megadva, a **loopName**, az aktuális erőforrás-típus iteráció szolgál.</span><span class="sxs-lookup"><span data-stu-id="13be1-161">If no value is provided for **loopName**, the current resource type iteration is used.</span></span> <span data-ttu-id="13be1-162">Adjon meg egy értéket a **loopName** amikor léptetés tulajdonság alapján.</span><span class="sxs-lookup"><span data-stu-id="13be1-162">Provide a value for **loopName** when iterating on a property.</span></span> 
 
<span data-ttu-id="13be1-163">Teljes leírását az használatának **copyIndex**, lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="13be1-163">For a complete description of how you use **copyIndex**, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

### <a name="example"></a><span data-ttu-id="13be1-164">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-164">Example</span></span>

<span data-ttu-id="13be1-165">A következő példa bemutatja a másolási ciklust és az értéket a neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="13be1-165">The following example shows a copy loop and the index value included in the name.</span></span> 

```json
"resources": [ 
  { 
    "name": "[concat('examplecopy-', copyIndex())]", 
    "type": "Microsoft.Web/sites", 
    "copy": { 
      "name": "websitescopy", 
      "count": "[parameters('count')]" 
    }, 
    ...
  }
]
```

### <a name="return-value"></a><span data-ttu-id="13be1-166">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-166">Return value</span></span>

<span data-ttu-id="13be1-167">Jelző egész számot az iteráció aktuális indexét.</span><span class="sxs-lookup"><span data-stu-id="13be1-167">An integer representing the current index of the iteration.</span></span>

<a id="div" />

## <a name="div"></a><span data-ttu-id="13be1-168">DIV</span><span class="sxs-lookup"><span data-stu-id="13be1-168">div</span></span>
`div(operand1, operand2)`

<span data-ttu-id="13be1-169">A két megadott egész számok egész szám hányadosának adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-169">Returns the integer division of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-170">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-170">Parameters</span></span>

| <span data-ttu-id="13be1-171">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-171">Parameter</span></span> | <span data-ttu-id="13be1-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-172">Required</span></span> | <span data-ttu-id="13be1-173">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-173">Type</span></span> | <span data-ttu-id="13be1-174">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-174">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-175">operand1</span><span class="sxs-lookup"><span data-stu-id="13be1-175">operand1</span></span> |<span data-ttu-id="13be1-176">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-176">Yes</span></span> |<span data-ttu-id="13be1-177">int</span><span class="sxs-lookup"><span data-stu-id="13be1-177">int</span></span> |<span data-ttu-id="13be1-178">Az a szám felosztják.</span><span class="sxs-lookup"><span data-stu-id="13be1-178">The number being divided.</span></span> |
| <span data-ttu-id="13be1-179">operand2</span><span class="sxs-lookup"><span data-stu-id="13be1-179">operand2</span></span> |<span data-ttu-id="13be1-180">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-180">Yes</span></span> |<span data-ttu-id="13be1-181">int</span><span class="sxs-lookup"><span data-stu-id="13be1-181">int</span></span> |<span data-ttu-id="13be1-182">Az a szám, amellyel osztani.</span><span class="sxs-lookup"><span data-stu-id="13be1-182">The number that is used to divide.</span></span> <span data-ttu-id="13be1-183">Nem lehet 0.</span><span class="sxs-lookup"><span data-stu-id="13be1-183">Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-184">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-184">Return value</span></span>

<span data-ttu-id="13be1-185">Egy osztás jelző egész számot.</span><span class="sxs-lookup"><span data-stu-id="13be1-185">An integer representing the division.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-186">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-186">Example</span></span>

<span data-ttu-id="13be1-187">A következő példa egy másik paraméterrel egy paraméter osztja.</span><span class="sxs-lookup"><span data-stu-id="13be1-187">The following example divides one parameter by another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

<span data-ttu-id="13be1-188">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-188">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-189">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-189">Name</span></span> | <span data-ttu-id="13be1-190">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-190">Type</span></span> | <span data-ttu-id="13be1-191">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-191">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-192">divResult</span><span class="sxs-lookup"><span data-stu-id="13be1-192">divResult</span></span> | <span data-ttu-id="13be1-193">int</span><span class="sxs-lookup"><span data-stu-id="13be1-193">Int</span></span> | <span data-ttu-id="13be1-194">2</span><span class="sxs-lookup"><span data-stu-id="13be1-194">2</span></span> |

<a id="float" />

## <a name="float"></a><span data-ttu-id="13be1-195">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="13be1-195">float</span></span>
`float(arg1)`

<span data-ttu-id="13be1-196">Konvertálja az értéket lebegőpontos számnak.</span><span class="sxs-lookup"><span data-stu-id="13be1-196">Converts the value to a floating point number.</span></span> <span data-ttu-id="13be1-197">Ez a függvény csak ha egyéni paraméterek átadása egy alkalmazást, például a logikai alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="13be1-197">You only use this function when passing custom parameters to an application, such as a Logic App.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-198">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-198">Parameters</span></span>

| <span data-ttu-id="13be1-199">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-199">Parameter</span></span> | <span data-ttu-id="13be1-200">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-200">Required</span></span> | <span data-ttu-id="13be1-201">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-201">Type</span></span> | <span data-ttu-id="13be1-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-202">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-203">arg1</span><span class="sxs-lookup"><span data-stu-id="13be1-203">arg1</span></span> |<span data-ttu-id="13be1-204">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-204">Yes</span></span> |<span data-ttu-id="13be1-205">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="13be1-205">string or int</span></span> |<span data-ttu-id="13be1-206">Az érték átalakítása lebegőpontos számnak.</span><span class="sxs-lookup"><span data-stu-id="13be1-206">The value to convert to a floating point number.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-207">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-207">Return value</span></span>
<span data-ttu-id="13be1-208">Lebegőpontos szám.</span><span class="sxs-lookup"><span data-stu-id="13be1-208">A floating point number.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-209">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-209">Example</span></span>

<span data-ttu-id="13be1-210">A következő példa bemutatja, hogyan lebegőpontos használandó paraméterek átadása egy logikai alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="13be1-210">The following example shows how to use float to pass parameters to a Logic App:</span></span>

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
        "custom1": {
            "value": "[float('3.0')]"
        },
        "custom2": {
            "value": "[float(3)]"
        },
```

<a id="int" />

## <a name="int"></a><span data-ttu-id="13be1-211">int</span><span class="sxs-lookup"><span data-stu-id="13be1-211">int</span></span>
`int(valueToConvert)`

<span data-ttu-id="13be1-212">A megadott érték konvertálása egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="13be1-212">Converts the specified value to an integer.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-213">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-213">Parameters</span></span>

| <span data-ttu-id="13be1-214">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-214">Parameter</span></span> | <span data-ttu-id="13be1-215">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-215">Required</span></span> | <span data-ttu-id="13be1-216">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-216">Type</span></span> | <span data-ttu-id="13be1-217">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-217">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-218">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="13be1-218">valueToConvert</span></span> |<span data-ttu-id="13be1-219">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-219">Yes</span></span> |<span data-ttu-id="13be1-220">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="13be1-220">string or int</span></span> |<span data-ttu-id="13be1-221">Az érték egész számra konvertálni.</span><span class="sxs-lookup"><span data-stu-id="13be1-221">The value to convert to an integer.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-222">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-222">Return value</span></span>

<span data-ttu-id="13be1-223">Az átalakított érték egész szám.</span><span class="sxs-lookup"><span data-stu-id="13be1-223">An integer of the converted value.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-224">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-224">Example</span></span>

<span data-ttu-id="13be1-225">Az alábbi példa a felhasználó által megadott paraméter értékének egész számra konvertál.</span><span class="sxs-lookup"><span data-stu-id="13be1-225">The following example converts the user-provided parameter value to integer.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": { 
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

<span data-ttu-id="13be1-226">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-226">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-227">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-227">Name</span></span> | <span data-ttu-id="13be1-228">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-228">Type</span></span> | <span data-ttu-id="13be1-229">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-229">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-230">intEredmeny</span><span class="sxs-lookup"><span data-stu-id="13be1-230">intResult</span></span> | <span data-ttu-id="13be1-231">int</span><span class="sxs-lookup"><span data-stu-id="13be1-231">Int</span></span> | <span data-ttu-id="13be1-232">4</span><span class="sxs-lookup"><span data-stu-id="13be1-232">4</span></span> |


<a id="min" />

## <a name="min"></a><span data-ttu-id="13be1-233">perc</span><span class="sxs-lookup"><span data-stu-id="13be1-233">min</span></span>
`min (arg1)`

<span data-ttu-id="13be1-234">Egy számokból álló tömb vagy egészek vesszővel elválasztott listáját a minimális értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-234">Returns the minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-235">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-235">Parameters</span></span>

| <span data-ttu-id="13be1-236">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-236">Parameter</span></span> | <span data-ttu-id="13be1-237">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-237">Required</span></span> | <span data-ttu-id="13be1-238">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-238">Type</span></span> | <span data-ttu-id="13be1-239">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-239">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-240">arg1</span><span class="sxs-lookup"><span data-stu-id="13be1-240">arg1</span></span> |<span data-ttu-id="13be1-241">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-241">Yes</span></span> |<span data-ttu-id="13be1-242">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="13be1-242">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="13be1-243">A gyűjteményt, amelyben a minimális érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="13be1-243">The collection to get the minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-244">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-244">Return value</span></span>

<span data-ttu-id="13be1-245">Jelző egész számot minimális érték a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="13be1-245">An integer representing minimum value from the collection.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-246">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-246">Example</span></span>

<span data-ttu-id="13be1-247">A következő példa bemutatja, hogyan használható min tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="13be1-247">The following example shows how to use min with an array and a list of integers:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

<span data-ttu-id="13be1-248">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-248">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-249">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-249">Name</span></span> | <span data-ttu-id="13be1-250">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-250">Type</span></span> | <span data-ttu-id="13be1-251">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-251">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-252">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="13be1-252">arrayOutput</span></span> | <span data-ttu-id="13be1-253">int</span><span class="sxs-lookup"><span data-stu-id="13be1-253">Int</span></span> | <span data-ttu-id="13be1-254">0</span><span class="sxs-lookup"><span data-stu-id="13be1-254">0</span></span> |
| <span data-ttu-id="13be1-255">intOutput</span><span class="sxs-lookup"><span data-stu-id="13be1-255">intOutput</span></span> | <span data-ttu-id="13be1-256">int</span><span class="sxs-lookup"><span data-stu-id="13be1-256">Int</span></span> | <span data-ttu-id="13be1-257">0</span><span class="sxs-lookup"><span data-stu-id="13be1-257">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="13be1-258">maximális</span><span class="sxs-lookup"><span data-stu-id="13be1-258">max</span></span>
`max (arg1)`

<span data-ttu-id="13be1-259">A maximális érték egész számok tömb vagy egészek vesszővel elválasztott listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-259">Returns the maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-260">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-260">Parameters</span></span>

| <span data-ttu-id="13be1-261">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-261">Parameter</span></span> | <span data-ttu-id="13be1-262">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-262">Required</span></span> | <span data-ttu-id="13be1-263">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-263">Type</span></span> | <span data-ttu-id="13be1-264">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-264">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-265">arg1</span><span class="sxs-lookup"><span data-stu-id="13be1-265">arg1</span></span> |<span data-ttu-id="13be1-266">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-266">Yes</span></span> |<span data-ttu-id="13be1-267">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="13be1-267">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="13be1-268">A gyűjteményt, amelyben a legnagyobb érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="13be1-268">The collection to get the maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-269">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-269">Return value</span></span>

<span data-ttu-id="13be1-270">Jelző egész számot a maximális érték a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="13be1-270">An integer representing the maximum value from the collection.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-271">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-271">Example</span></span>

<span data-ttu-id="13be1-272">A következő példa bemutatja, hogyan használható maximum tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="13be1-272">The following example shows how to use max with an array and a list of integers:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

<span data-ttu-id="13be1-273">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-273">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-274">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-274">Name</span></span> | <span data-ttu-id="13be1-275">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-275">Type</span></span> | <span data-ttu-id="13be1-276">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-276">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-277">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="13be1-277">arrayOutput</span></span> | <span data-ttu-id="13be1-278">int</span><span class="sxs-lookup"><span data-stu-id="13be1-278">Int</span></span> | <span data-ttu-id="13be1-279">5</span><span class="sxs-lookup"><span data-stu-id="13be1-279">5</span></span> |
| <span data-ttu-id="13be1-280">intOutput</span><span class="sxs-lookup"><span data-stu-id="13be1-280">intOutput</span></span> | <span data-ttu-id="13be1-281">int</span><span class="sxs-lookup"><span data-stu-id="13be1-281">Int</span></span> | <span data-ttu-id="13be1-282">5</span><span class="sxs-lookup"><span data-stu-id="13be1-282">5</span></span> |

<a id="mod" />

## <a name="mod"></a><span data-ttu-id="13be1-283">MOD</span><span class="sxs-lookup"><span data-stu-id="13be1-283">mod</span></span>
`mod(operand1, operand2)`

<span data-ttu-id="13be1-284">Használja a két megadott egész szám hányadosának egész a maradékot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-284">Returns the remainder of the integer division using the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-285">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-285">Parameters</span></span>

| <span data-ttu-id="13be1-286">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-286">Parameter</span></span> | <span data-ttu-id="13be1-287">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-287">Required</span></span> | <span data-ttu-id="13be1-288">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-288">Type</span></span> | <span data-ttu-id="13be1-289">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-289">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-290">operand1</span><span class="sxs-lookup"><span data-stu-id="13be1-290">operand1</span></span> |<span data-ttu-id="13be1-291">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-291">Yes</span></span> |<span data-ttu-id="13be1-292">int</span><span class="sxs-lookup"><span data-stu-id="13be1-292">int</span></span> |<span data-ttu-id="13be1-293">Az a szám felosztják.</span><span class="sxs-lookup"><span data-stu-id="13be1-293">The number being divided.</span></span> |
| <span data-ttu-id="13be1-294">operand2</span><span class="sxs-lookup"><span data-stu-id="13be1-294">operand2</span></span> |<span data-ttu-id="13be1-295">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-295">Yes</span></span> |<span data-ttu-id="13be1-296">int</span><span class="sxs-lookup"><span data-stu-id="13be1-296">int</span></span> |<span data-ttu-id="13be1-297">A szám, amellyel osztani, nem lehet 0.</span><span class="sxs-lookup"><span data-stu-id="13be1-297">The number that is used to divide, Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-298">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-298">Return value</span></span>
<span data-ttu-id="13be1-299">Egy további jelző egész számot.</span><span class="sxs-lookup"><span data-stu-id="13be1-299">An integer representing the remainder.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-300">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-300">Example</span></span>

<span data-ttu-id="13be1-301">A következő példa egy másik paraméterrel egy paraméter felosztása adja eredményül.</span><span class="sxs-lookup"><span data-stu-id="13be1-301">The following example returns the remainder of dividing one parameter by another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used to divide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

<span data-ttu-id="13be1-302">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-302">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-303">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-303">Name</span></span> | <span data-ttu-id="13be1-304">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-304">Type</span></span> | <span data-ttu-id="13be1-305">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-305">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-306">modResult</span><span class="sxs-lookup"><span data-stu-id="13be1-306">modResult</span></span> | <span data-ttu-id="13be1-307">int</span><span class="sxs-lookup"><span data-stu-id="13be1-307">Int</span></span> | <span data-ttu-id="13be1-308">1</span><span class="sxs-lookup"><span data-stu-id="13be1-308">1</span></span> |

<a id="mul" />

## <a name="mul"></a><span data-ttu-id="13be1-309">MUL számú</span><span class="sxs-lookup"><span data-stu-id="13be1-309">mul</span></span>
`mul(operand1, operand2)`

<span data-ttu-id="13be1-310">A két megadott egész számok szorzás adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-310">Returns the multiplication of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-311">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-311">Parameters</span></span>

| <span data-ttu-id="13be1-312">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-312">Parameter</span></span> | <span data-ttu-id="13be1-313">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-313">Required</span></span> | <span data-ttu-id="13be1-314">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-314">Type</span></span> | <span data-ttu-id="13be1-315">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-315">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-316">operand1</span><span class="sxs-lookup"><span data-stu-id="13be1-316">operand1</span></span> |<span data-ttu-id="13be1-317">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-317">Yes</span></span> |<span data-ttu-id="13be1-318">int</span><span class="sxs-lookup"><span data-stu-id="13be1-318">int</span></span> |<span data-ttu-id="13be1-319">A szorzási első szám.</span><span class="sxs-lookup"><span data-stu-id="13be1-319">First number to multiply.</span></span> |
| <span data-ttu-id="13be1-320">operand2</span><span class="sxs-lookup"><span data-stu-id="13be1-320">operand2</span></span> |<span data-ttu-id="13be1-321">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-321">Yes</span></span> |<span data-ttu-id="13be1-322">int</span><span class="sxs-lookup"><span data-stu-id="13be1-322">int</span></span> |<span data-ttu-id="13be1-323">A szorzási második szám.</span><span class="sxs-lookup"><span data-stu-id="13be1-323">Second number to multiply.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-324">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-324">Return value</span></span>

<span data-ttu-id="13be1-325">A szorzás jelölő egész.</span><span class="sxs-lookup"><span data-stu-id="13be1-325">An integer representing the multiplication.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-326">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-326">Example</span></span>

<span data-ttu-id="13be1-327">Az alábbi példa szorozza meg egy másik paraméterrel egy paramétert.</span><span class="sxs-lookup"><span data-stu-id="13be1-327">The following example multiplies one parameter by another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer to multiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer to multiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

<span data-ttu-id="13be1-328">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-328">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-329">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-329">Name</span></span> | <span data-ttu-id="13be1-330">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-330">Type</span></span> | <span data-ttu-id="13be1-331">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-331">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-332">mulResult</span><span class="sxs-lookup"><span data-stu-id="13be1-332">mulResult</span></span> | <span data-ttu-id="13be1-333">int</span><span class="sxs-lookup"><span data-stu-id="13be1-333">Int</span></span> | <span data-ttu-id="13be1-334">15</span><span class="sxs-lookup"><span data-stu-id="13be1-334">15</span></span> |

<a id="sub" />

## <a name="sub"></a><span data-ttu-id="13be1-335">Sub</span><span class="sxs-lookup"><span data-stu-id="13be1-335">sub</span></span>
`sub(operand1, operand2)`

<span data-ttu-id="13be1-336">A kivonás a két megadott egész számokat adja vissza.</span><span class="sxs-lookup"><span data-stu-id="13be1-336">Returns the subtraction of the two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="13be1-337">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="13be1-337">Parameters</span></span>

| <span data-ttu-id="13be1-338">Paraméter</span><span class="sxs-lookup"><span data-stu-id="13be1-338">Parameter</span></span> | <span data-ttu-id="13be1-339">Szükséges</span><span class="sxs-lookup"><span data-stu-id="13be1-339">Required</span></span> | <span data-ttu-id="13be1-340">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-340">Type</span></span> | <span data-ttu-id="13be1-341">Leírás</span><span class="sxs-lookup"><span data-stu-id="13be1-341">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="13be1-342">operand1</span><span class="sxs-lookup"><span data-stu-id="13be1-342">operand1</span></span> |<span data-ttu-id="13be1-343">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-343">Yes</span></span> |<span data-ttu-id="13be1-344">int</span><span class="sxs-lookup"><span data-stu-id="13be1-344">int</span></span> |<span data-ttu-id="13be1-345">A szám, amelyet a program levonja az.</span><span class="sxs-lookup"><span data-stu-id="13be1-345">The number that is subtracted from.</span></span> |
| <span data-ttu-id="13be1-346">operand2</span><span class="sxs-lookup"><span data-stu-id="13be1-346">operand2</span></span> |<span data-ttu-id="13be1-347">Igen</span><span class="sxs-lookup"><span data-stu-id="13be1-347">Yes</span></span> |<span data-ttu-id="13be1-348">int</span><span class="sxs-lookup"><span data-stu-id="13be1-348">int</span></span> |<span data-ttu-id="13be1-349">A szám, amelyet a program levonja.</span><span class="sxs-lookup"><span data-stu-id="13be1-349">The number that is subtracted.</span></span> |

### <a name="return-value"></a><span data-ttu-id="13be1-350">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="13be1-350">Return value</span></span>
<span data-ttu-id="13be1-351">Egy a kivonásnak jelző egész számot.</span><span class="sxs-lookup"><span data-stu-id="13be1-351">An integer representing the subtraction.</span></span>

### <a name="example"></a><span data-ttu-id="13be1-352">Példa</span><span class="sxs-lookup"><span data-stu-id="13be1-352">Example</span></span>

<span data-ttu-id="13be1-353">A következő példa egy paraméter és egy másik paraméter kivonja.</span><span class="sxs-lookup"><span data-stu-id="13be1-353">The following example subtracts one parameter from another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer to subtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

<span data-ttu-id="13be1-354">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="13be1-354">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="13be1-355">Név</span><span class="sxs-lookup"><span data-stu-id="13be1-355">Name</span></span> | <span data-ttu-id="13be1-356">Típus</span><span class="sxs-lookup"><span data-stu-id="13be1-356">Type</span></span> | <span data-ttu-id="13be1-357">Érték</span><span class="sxs-lookup"><span data-stu-id="13be1-357">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="13be1-358">subResult</span><span class="sxs-lookup"><span data-stu-id="13be1-358">subResult</span></span> | <span data-ttu-id="13be1-359">int</span><span class="sxs-lookup"><span data-stu-id="13be1-359">Int</span></span> | <span data-ttu-id="13be1-360">4</span><span class="sxs-lookup"><span data-stu-id="13be1-360">4</span></span> |

## <a name="next-steps"></a><span data-ttu-id="13be1-361">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13be1-361">Next steps</span></span>
* <span data-ttu-id="13be1-362">A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="13be1-362">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="13be1-363">Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="13be1-363">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="13be1-364">Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="13be1-364">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="13be1-365">A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="13be1-365">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


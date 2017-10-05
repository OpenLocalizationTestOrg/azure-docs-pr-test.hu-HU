---
title: "Az Azure Resource Manager-sablon működik - tömbállandó és objektumok |} Microsoft Docs"
description: "A tömbök és objektumok használata az Azure Resource Manager sablon használandó funkcióit ismerteti."
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
ms.date: 06/12/2017
ms.author: tomfitz
ms.openlocfilehash: 0bd9ec41761c9ce575f3bcf4d1f8e8578b83e01c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="10562-103">Az Azure Resource Manager sablonokhoz tárolótömböt és az objektum funkciók</span><span class="sxs-lookup"><span data-stu-id="10562-103">Array and object functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="10562-104">Erőforrás-kezelő számos funkciókat nyújt, tömbök és objektumok.</span><span class="sxs-lookup"><span data-stu-id="10562-104">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="10562-105">a tömb</span><span class="sxs-lookup"><span data-stu-id="10562-105">array</span></span>](#array)
* [<span data-ttu-id="10562-106">Egyesítés</span><span class="sxs-lookup"><span data-stu-id="10562-106">coalesce</span></span>](#coalesce)
* [<span data-ttu-id="10562-107">Concat</span><span class="sxs-lookup"><span data-stu-id="10562-107">concat</span></span>](#concat)
* [<span data-ttu-id="10562-108">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="10562-108">contains</span></span>](#contains)
* [<span data-ttu-id="10562-109">createArray</span><span class="sxs-lookup"><span data-stu-id="10562-109">createArray</span></span>](#createarray)
* [<span data-ttu-id="10562-110">üres</span><span class="sxs-lookup"><span data-stu-id="10562-110">empty</span></span>](#empty)
* [<span data-ttu-id="10562-111">első</span><span class="sxs-lookup"><span data-stu-id="10562-111">first</span></span>](#first)
* [<span data-ttu-id="10562-112">metszetének</span><span class="sxs-lookup"><span data-stu-id="10562-112">intersection</span></span>](#intersection)
* [<span data-ttu-id="10562-113">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="10562-113">json</span></span>](#json)
* [<span data-ttu-id="10562-114">utolsó</span><span class="sxs-lookup"><span data-stu-id="10562-114">last</span></span>](#last)
* [<span data-ttu-id="10562-115">hossza</span><span class="sxs-lookup"><span data-stu-id="10562-115">length</span></span>](#length)
* [<span data-ttu-id="10562-116">perc</span><span class="sxs-lookup"><span data-stu-id="10562-116">min</span></span>](#min)
* [<span data-ttu-id="10562-117">maximális</span><span class="sxs-lookup"><span data-stu-id="10562-117">max</span></span>](#max)
* [<span data-ttu-id="10562-118">tartomány</span><span class="sxs-lookup"><span data-stu-id="10562-118">range</span></span>](#range)
* [<span data-ttu-id="10562-119">hagyja ki</span><span class="sxs-lookup"><span data-stu-id="10562-119">skip</span></span>](#skip)
* [<span data-ttu-id="10562-120">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="10562-120">take</span></span>](#take)
* [<span data-ttu-id="10562-121">a UNION</span><span class="sxs-lookup"><span data-stu-id="10562-121">union</span></span>](#union)

<span data-ttu-id="10562-122">Ahhoz, hogy egy érték elválasztott karakterlánc tömböt, lásd: [vágási](resource-group-template-functions-string.md#split).</span><span class="sxs-lookup"><span data-stu-id="10562-122">To get an array of string values delimited by a value, see [split](resource-group-template-functions-string.md#split).</span></span>

<a id="array" />

## <a name="array"></a><span data-ttu-id="10562-123">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-123">array</span></span>
`array(convertToArray)`

<span data-ttu-id="10562-124">Az érték alakít át tömbbé.</span><span class="sxs-lookup"><span data-stu-id="10562-124">Converts the value to an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-125">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-125">Parameters</span></span>

| <span data-ttu-id="10562-126">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-126">Parameter</span></span> | <span data-ttu-id="10562-127">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-127">Required</span></span> | <span data-ttu-id="10562-128">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-128">Type</span></span> | <span data-ttu-id="10562-129">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-129">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-130">convertToArray</span><span class="sxs-lookup"><span data-stu-id="10562-130">convertToArray</span></span> |<span data-ttu-id="10562-131">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-131">Yes</span></span> |<span data-ttu-id="10562-132">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-132">int, string, array, or object</span></span> |<span data-ttu-id="10562-133">A tömb átalakítása érték.</span><span class="sxs-lookup"><span data-stu-id="10562-133">The value to convert to an array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-134">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-134">Return value</span></span>

<span data-ttu-id="10562-135">Egy tömb.</span><span class="sxs-lookup"><span data-stu-id="10562-135">An array.</span></span>

### <a name="example"></a><span data-ttu-id="10562-136">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-136">Example</span></span>

<span data-ttu-id="10562-137">A következő példa bemutatja, hogyan különböző típusú tömb funkcióval.</span><span class="sxs-lookup"><span data-stu-id="10562-137">The following example shows how to use the array function with different types.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "intToConvert": {
            "type": "int",
            "defaultValue": 1
        },
        "stringToConvert": {
            "type": "string",
            "defaultValue": "a"
        },
        "objectToConvert": {
            "type": "object",
            "defaultValue": {"a": "b", "c": "d"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "intOutput": {
            "type": "array",
            "value": "[array(parameters('intToConvert'))]"
        },
        "stringOutput": {
            "type": "array",
            "value": "[array(parameters('stringToConvert'))]"
        },
        "objectOutput": {
            "type": "array",
            "value": "[array(parameters('objectToConvert'))]"
        }
    }
}
```

<span data-ttu-id="10562-138">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-138">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-139">Név</span><span class="sxs-lookup"><span data-stu-id="10562-139">Name</span></span> | <span data-ttu-id="10562-140">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-140">Type</span></span> | <span data-ttu-id="10562-141">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-141">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-142">intOutput</span><span class="sxs-lookup"><span data-stu-id="10562-142">intOutput</span></span> | <span data-ttu-id="10562-143">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-143">Array</span></span> | <span data-ttu-id="10562-144">[1]</span><span class="sxs-lookup"><span data-stu-id="10562-144">[1]</span></span> |
| <span data-ttu-id="10562-145">stringOutput</span><span class="sxs-lookup"><span data-stu-id="10562-145">stringOutput</span></span> | <span data-ttu-id="10562-146">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-146">Array</span></span> | <span data-ttu-id="10562-147">["a"]</span><span class="sxs-lookup"><span data-stu-id="10562-147">["a"]</span></span> |
| <span data-ttu-id="10562-148">objectOutput</span><span class="sxs-lookup"><span data-stu-id="10562-148">objectOutput</span></span> | <span data-ttu-id="10562-149">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-149">Array</span></span> | <span data-ttu-id="10562-150">[{"a": "b", "c": "d"}]</span><span class="sxs-lookup"><span data-stu-id="10562-150">[{"a": "b", "c": "d"}]</span></span> |

<a id="coalesce" />

## <a name="coalesce"></a><span data-ttu-id="10562-151">Egyesítés</span><span class="sxs-lookup"><span data-stu-id="10562-151">coalesce</span></span>
`coalesce(arg1, arg2, arg3, ...)`

<span data-ttu-id="10562-152">A paraméterek első nem null értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-152">Returns first non-null value from the parameters.</span></span> <span data-ttu-id="10562-153">Üres karakterláncokat, üres tömbök és üres objektumok csak NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="10562-153">Empty strings, empty arrays, and empty objects are not null.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-154">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-154">Parameters</span></span>

| <span data-ttu-id="10562-155">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-155">Parameter</span></span> | <span data-ttu-id="10562-156">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-156">Required</span></span> | <span data-ttu-id="10562-157">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-157">Type</span></span> | <span data-ttu-id="10562-158">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-158">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-159">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-159">arg1</span></span> |<span data-ttu-id="10562-160">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-160">Yes</span></span> |<span data-ttu-id="10562-161">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-161">int, string, array, or object</span></span> |<span data-ttu-id="10562-162">Az első érték teszteléséhez a NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="10562-162">The first value to test for null.</span></span> |
| <span data-ttu-id="10562-163">További argumentum</span><span class="sxs-lookup"><span data-stu-id="10562-163">additional args</span></span> |<span data-ttu-id="10562-164">Nem</span><span class="sxs-lookup"><span data-stu-id="10562-164">No</span></span> |<span data-ttu-id="10562-165">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-165">int, string, array, or object</span></span> |<span data-ttu-id="10562-166">További értékek tesztelésére null értékű.</span><span class="sxs-lookup"><span data-stu-id="10562-166">Additional values to test for null.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-167">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-167">Return value</span></span>

<span data-ttu-id="10562-168">Az érték első null értékű paramétert, amely egy karakterlánc, int, tömb vagy objektum lehet.</span><span class="sxs-lookup"><span data-stu-id="10562-168">The value of the first non-null parameters, which can be a string, int, array, or object.</span></span> <span data-ttu-id="10562-169">NULL értékű, ha az összes paraméterei null értékű.</span><span class="sxs-lookup"><span data-stu-id="10562-169">Null if all parameters are null.</span></span> 

### <a name="example"></a><span data-ttu-id="10562-170">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-170">Example</span></span>

<span data-ttu-id="10562-171">A következő példa bemutatja a kimenetét a coalesce különböző használatát.</span><span class="sxs-lookup"><span data-stu-id="10562-171">The following example shows the output from different uses of coalesce.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "null1": null, 
                "null2": null,
                "string": "default",
                "int": 1,
                "object": {"first": "default"},
                "array": [1]
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').string)]"
        },
        "intOutput": {
            "type": "int",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').int)]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').object)]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').array)]"
        },
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2))]"
        }
    }
}
```

<span data-ttu-id="10562-172">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-172">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-173">Név</span><span class="sxs-lookup"><span data-stu-id="10562-173">Name</span></span> | <span data-ttu-id="10562-174">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-174">Type</span></span> | <span data-ttu-id="10562-175">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-175">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-176">stringOutput</span><span class="sxs-lookup"><span data-stu-id="10562-176">stringOutput</span></span> | <span data-ttu-id="10562-177">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-177">String</span></span> | <span data-ttu-id="10562-178">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="10562-178">default</span></span> |
| <span data-ttu-id="10562-179">intOutput</span><span class="sxs-lookup"><span data-stu-id="10562-179">intOutput</span></span> | <span data-ttu-id="10562-180">int</span><span class="sxs-lookup"><span data-stu-id="10562-180">Int</span></span> | <span data-ttu-id="10562-181">1</span><span class="sxs-lookup"><span data-stu-id="10562-181">1</span></span> |
| <span data-ttu-id="10562-182">objectOutput</span><span class="sxs-lookup"><span data-stu-id="10562-182">objectOutput</span></span> | <span data-ttu-id="10562-183">Objektum</span><span class="sxs-lookup"><span data-stu-id="10562-183">Object</span></span> | <span data-ttu-id="10562-184">{"első": "alapértelmezett"}</span><span class="sxs-lookup"><span data-stu-id="10562-184">{"first": "default"}</span></span> |
| <span data-ttu-id="10562-185">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-185">arrayOutput</span></span> | <span data-ttu-id="10562-186">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-186">Array</span></span> | <span data-ttu-id="10562-187">[1]</span><span class="sxs-lookup"><span data-stu-id="10562-187">[1]</span></span> |
| <span data-ttu-id="10562-188">emptyOutput</span><span class="sxs-lookup"><span data-stu-id="10562-188">emptyOutput</span></span> | <span data-ttu-id="10562-189">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-189">Bool</span></span> | <span data-ttu-id="10562-190">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-190">True</span></span> |

<a id="concat" />

## <a name="concat"></a><span data-ttu-id="10562-191">Concat</span><span class="sxs-lookup"><span data-stu-id="10562-191">concat</span></span>
`concat(arg1, arg2, arg3, ...)`

<span data-ttu-id="10562-192">Több tömbök egyesíti, és a összefűzött tömböt ad vissza, vagy több karakterlánc-értékek egyesíti, és a összefűzött karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-192">Combines multiple arrays and returns the concatenated array, or combines multiple string values and returns the concatenated string.</span></span> 

### <a name="parameters"></a><span data-ttu-id="10562-193">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-193">Parameters</span></span>

| <span data-ttu-id="10562-194">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-194">Parameter</span></span> | <span data-ttu-id="10562-195">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-195">Required</span></span> | <span data-ttu-id="10562-196">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-196">Type</span></span> | <span data-ttu-id="10562-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-197">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-198">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-198">arg1</span></span> |<span data-ttu-id="10562-199">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-199">Yes</span></span> |<span data-ttu-id="10562-200">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-200">array or string</span></span> |<span data-ttu-id="10562-201">Az első tömb vagy kapott karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="10562-201">The first array or string for concatenation.</span></span> |
| <span data-ttu-id="10562-202">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="10562-202">additional arguments</span></span> |<span data-ttu-id="10562-203">Nem</span><span class="sxs-lookup"><span data-stu-id="10562-203">No</span></span> |<span data-ttu-id="10562-204">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-204">array or string</span></span> |<span data-ttu-id="10562-205">További tömbök vagy karakterláncok kapott a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="10562-205">Additional arrays or strings in sequential order for concatenation.</span></span> |

<span data-ttu-id="10562-206">Ez a funkció tetszőleges számú argumentumot is igénybe vehet, és fogadhat, karakterláncok vagy a tömbök a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="10562-206">This function can take any number of arguments, and can accept either strings or arrays for the parameters.</span></span>

### <a name="return-value"></a><span data-ttu-id="10562-207">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-207">Return value</span></span>
<span data-ttu-id="10562-208">A karakterlánc vagy tömb összefűzött.</span><span class="sxs-lookup"><span data-stu-id="10562-208">A string or array of concatenated values.</span></span>

### <a name="example"></a><span data-ttu-id="10562-209">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-209">Example</span></span>

<span data-ttu-id="10562-210">A következő példa bemutatja, hogyan kombinálhatók két tömb.</span><span class="sxs-lookup"><span data-stu-id="10562-210">The following example shows how to combine two arrays.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

<span data-ttu-id="10562-211">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-211">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-212">Név</span><span class="sxs-lookup"><span data-stu-id="10562-212">Name</span></span> | <span data-ttu-id="10562-213">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-213">Type</span></span> | <span data-ttu-id="10562-214">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-214">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-215">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="10562-215">return</span></span> | <span data-ttu-id="10562-216">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-216">Array</span></span> | <span data-ttu-id="10562-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="10562-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<span data-ttu-id="10562-218">A következő példa bemutatja, hogyan kombinálhatja a két karakterlánc-értékeket, és olyan összefűzött karakterláncot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-218">The following example shows how to combine two string values and return a concatenated string.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

<span data-ttu-id="10562-219">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-219">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-220">Név</span><span class="sxs-lookup"><span data-stu-id="10562-220">Name</span></span> | <span data-ttu-id="10562-221">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-221">Type</span></span> | <span data-ttu-id="10562-222">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-222">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-223">concatOutput</span><span class="sxs-lookup"><span data-stu-id="10562-223">concatOutput</span></span> | <span data-ttu-id="10562-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-224">String</span></span> | <span data-ttu-id="10562-225">előtag-5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="10562-225">prefix-5yj4yjf5mbg72</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="10562-226">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="10562-226">contains</span></span>
`contains(container, itemToFind)`

<span data-ttu-id="10562-227">Ellenőrzi, hogy egy tömb értéket tartalmaz, objektum kulcsot tartalmaz, vagy egy karakterláncot egy substring tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="10562-227">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-228">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-228">Parameters</span></span>

| <span data-ttu-id="10562-229">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-229">Parameter</span></span> | <span data-ttu-id="10562-230">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-230">Required</span></span> | <span data-ttu-id="10562-231">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-231">Type</span></span> | <span data-ttu-id="10562-232">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-232">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-233">Tároló</span><span class="sxs-lookup"><span data-stu-id="10562-233">container</span></span> |<span data-ttu-id="10562-234">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-234">Yes</span></span> |<span data-ttu-id="10562-235">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-235">array, object, or string</span></span> |<span data-ttu-id="10562-236">Az érték, amely tartalmazza a keresendő érték.</span><span class="sxs-lookup"><span data-stu-id="10562-236">The value that contains the value to find.</span></span> |
| <span data-ttu-id="10562-237">itemToFind</span><span class="sxs-lookup"><span data-stu-id="10562-237">itemToFind</span></span> |<span data-ttu-id="10562-238">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-238">Yes</span></span> |<span data-ttu-id="10562-239">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="10562-239">string or int</span></span> |<span data-ttu-id="10562-240">Az érték kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="10562-240">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-241">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-241">Return value</span></span>

<span data-ttu-id="10562-242">**Igaz** Ha az elem található; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="10562-242">**True** if the item is found; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="10562-243">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-243">Example</span></span>

<span data-ttu-id="10562-244">A következő példa bemutatja, hogyan használható különböző típusú tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="10562-244">The following example shows how to use contains with different types:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

<span data-ttu-id="10562-245">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-245">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-246">Név</span><span class="sxs-lookup"><span data-stu-id="10562-246">Name</span></span> | <span data-ttu-id="10562-247">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-247">Type</span></span> | <span data-ttu-id="10562-248">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-249">stringTrue</span><span class="sxs-lookup"><span data-stu-id="10562-249">stringTrue</span></span> | <span data-ttu-id="10562-250">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-250">Bool</span></span> | <span data-ttu-id="10562-251">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-251">True</span></span> |
| <span data-ttu-id="10562-252">stringFalse</span><span class="sxs-lookup"><span data-stu-id="10562-252">stringFalse</span></span> | <span data-ttu-id="10562-253">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-253">Bool</span></span> | <span data-ttu-id="10562-254">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="10562-254">False</span></span> |
| <span data-ttu-id="10562-255">objectTrue</span><span class="sxs-lookup"><span data-stu-id="10562-255">objectTrue</span></span> | <span data-ttu-id="10562-256">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-256">Bool</span></span> | <span data-ttu-id="10562-257">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-257">True</span></span> |
| <span data-ttu-id="10562-258">objectFalse</span><span class="sxs-lookup"><span data-stu-id="10562-258">objectFalse</span></span> | <span data-ttu-id="10562-259">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-259">Bool</span></span> | <span data-ttu-id="10562-260">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="10562-260">False</span></span> |
| <span data-ttu-id="10562-261">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="10562-261">arrayTrue</span></span> | <span data-ttu-id="10562-262">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-262">Bool</span></span> | <span data-ttu-id="10562-263">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-263">True</span></span> |
| <span data-ttu-id="10562-264">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="10562-264">arrayFalse</span></span> | <span data-ttu-id="10562-265">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-265">Bool</span></span> | <span data-ttu-id="10562-266">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="10562-266">False</span></span> |

<a id="createarray" />

## <a name="createarray"></a><span data-ttu-id="10562-267">createarray</span><span class="sxs-lookup"><span data-stu-id="10562-267">createarray</span></span>
`createArray (arg1, arg2, arg3, ...)`

<span data-ttu-id="10562-268">A paraméter egy tömb jön létre.</span><span class="sxs-lookup"><span data-stu-id="10562-268">Creates an array from the parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-269">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-269">Parameters</span></span>

| <span data-ttu-id="10562-270">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-270">Parameter</span></span> | <span data-ttu-id="10562-271">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-271">Required</span></span> | <span data-ttu-id="10562-272">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-272">Type</span></span> | <span data-ttu-id="10562-273">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-273">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-274">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-274">arg1</span></span> |<span data-ttu-id="10562-275">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-275">Yes</span></span> |<span data-ttu-id="10562-276">Karakterlánc, egész szám, a tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-276">String, Integer, Array, or Object</span></span> |<span data-ttu-id="10562-277">A tömb első értékét.</span><span class="sxs-lookup"><span data-stu-id="10562-277">The first value in the array.</span></span> |
| <span data-ttu-id="10562-278">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="10562-278">additional arguments</span></span> |<span data-ttu-id="10562-279">Nem</span><span class="sxs-lookup"><span data-stu-id="10562-279">No</span></span> |<span data-ttu-id="10562-280">Karakterlánc, egész szám, a tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-280">String, Integer, Array, or Object</span></span> |<span data-ttu-id="10562-281">További a tömbben szereplő értékeket.</span><span class="sxs-lookup"><span data-stu-id="10562-281">Additional values in the array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-282">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-282">Return value</span></span>

<span data-ttu-id="10562-283">Egy tömb.</span><span class="sxs-lookup"><span data-stu-id="10562-283">An array.</span></span>

### <a name="example"></a><span data-ttu-id="10562-284">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-284">Example</span></span>

<span data-ttu-id="10562-285">A következő példa bemutatja, hogyan különböző createArray használata:</span><span class="sxs-lookup"><span data-stu-id="10562-285">The following example shows how to use createArray with different types:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringArray": {
            "type": "array",
            "value": "[createArray('a', 'b', 'c')]"
        },
        "intArray": {
            "type": "array",
            "value": "[createArray(1, 2, 3)]"
        },
        "objectArray": {
            "type": "array",
            "value": "[createArray(parameters('objectToTest'))]"
        },
        "arrayArray": {
            "type": "array",
            "value": "[createArray(parameters('arrayToTest'))]"
        }
    }
}
```

<span data-ttu-id="10562-286">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-286">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-287">Név</span><span class="sxs-lookup"><span data-stu-id="10562-287">Name</span></span> | <span data-ttu-id="10562-288">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-288">Type</span></span> | <span data-ttu-id="10562-289">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-289">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-290">stringArray</span><span class="sxs-lookup"><span data-stu-id="10562-290">stringArray</span></span> | <span data-ttu-id="10562-291">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-291">Array</span></span> | <span data-ttu-id="10562-292">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="10562-292">["a", "b", "c"]</span></span> |
| <span data-ttu-id="10562-293">intArray</span><span class="sxs-lookup"><span data-stu-id="10562-293">intArray</span></span> | <span data-ttu-id="10562-294">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-294">Array</span></span> | <span data-ttu-id="10562-295">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="10562-295">[1, 2, 3]</span></span> |
| <span data-ttu-id="10562-296">objectArray</span><span class="sxs-lookup"><span data-stu-id="10562-296">objectArray</span></span> | <span data-ttu-id="10562-297">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-297">Array</span></span> | <span data-ttu-id="10562-298">[{"egy": "a", "2": "b", "három": "c"}]</span><span class="sxs-lookup"><span data-stu-id="10562-298">[{"one": "a", "two": "b", "three": "c"}]</span></span> |
| <span data-ttu-id="10562-299">arrayArray</span><span class="sxs-lookup"><span data-stu-id="10562-299">arrayArray</span></span> | <span data-ttu-id="10562-300">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-300">Array</span></span> | <span data-ttu-id="10562-301">[["egy", "két", "három"]]</span><span class="sxs-lookup"><span data-stu-id="10562-301">[["one", "two", "three"]]</span></span> |

<a id="empty" />

## <a name="empty"></a><span data-ttu-id="10562-302">üres</span><span class="sxs-lookup"><span data-stu-id="10562-302">empty</span></span>

`empty(itemToTest)`

<span data-ttu-id="10562-303">Meghatározza, hogy egy tömb, az objektum, vagy a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="10562-303">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-304">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-304">Parameters</span></span>

| <span data-ttu-id="10562-305">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-305">Parameter</span></span> | <span data-ttu-id="10562-306">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-306">Required</span></span> | <span data-ttu-id="10562-307">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-307">Type</span></span> | <span data-ttu-id="10562-308">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-308">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-309">itemToTest</span><span class="sxs-lookup"><span data-stu-id="10562-309">itemToTest</span></span> |<span data-ttu-id="10562-310">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-310">Yes</span></span> |<span data-ttu-id="10562-311">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-311">array, object, or string</span></span> |<span data-ttu-id="10562-312">Ellenőrizze a esetén üres érték.</span><span class="sxs-lookup"><span data-stu-id="10562-312">The value to check if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-313">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-313">Return value</span></span>

<span data-ttu-id="10562-314">Beolvasása **igaz** értéke üres, ha sikertelen, ha **hamis**.</span><span class="sxs-lookup"><span data-stu-id="10562-314">Returns **True** if the value is empty; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="10562-315">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-315">Example</span></span>

<span data-ttu-id="10562-316">A következő példa ellenőrzi, hogy egy tömb, az objektumot, és a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="10562-316">The following example checks whether an array, object, and string are empty.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

<span data-ttu-id="10562-317">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-317">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-318">Név</span><span class="sxs-lookup"><span data-stu-id="10562-318">Name</span></span> | <span data-ttu-id="10562-319">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-319">Type</span></span> | <span data-ttu-id="10562-320">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-320">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-321">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="10562-321">arrayEmpty</span></span> | <span data-ttu-id="10562-322">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-322">Bool</span></span> | <span data-ttu-id="10562-323">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-323">True</span></span> |
| <span data-ttu-id="10562-324">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="10562-324">objectEmpty</span></span> | <span data-ttu-id="10562-325">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-325">Bool</span></span> | <span data-ttu-id="10562-326">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-326">True</span></span> |
| <span data-ttu-id="10562-327">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="10562-327">stringEmpty</span></span> | <span data-ttu-id="10562-328">logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-328">Bool</span></span> | <span data-ttu-id="10562-329">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-329">True</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="10562-330">első</span><span class="sxs-lookup"><span data-stu-id="10562-330">first</span></span>
`first(arg1)`

<span data-ttu-id="10562-331">A tömb első eleme, vagy a karakterlánc első karaktere adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-331">Returns the first element of the array, or first character of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-332">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-332">Parameters</span></span>

| <span data-ttu-id="10562-333">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-333">Parameter</span></span> | <span data-ttu-id="10562-334">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-334">Required</span></span> | <span data-ttu-id="10562-335">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-335">Type</span></span> | <span data-ttu-id="10562-336">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-336">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-337">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-337">arg1</span></span> |<span data-ttu-id="10562-338">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-338">Yes</span></span> |<span data-ttu-id="10562-339">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-339">array or string</span></span> |<span data-ttu-id="10562-340">Az érték első karakter vagy elem lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="10562-340">The value to retrieve the first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-341">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-341">Return value</span></span>

<span data-ttu-id="10562-342">A típus (karakterlánc, int, tömb vagy objektum) az első elem a tömb vagy karakterlánc első karaktere.</span><span class="sxs-lookup"><span data-stu-id="10562-342">The type (string, int, array, or object) of the first element in an array, or the first character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="10562-343">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-343">Example</span></span>

<span data-ttu-id="10562-344">A következő példa bemutatja, hogyan használható az első függvényét egy tömb és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="10562-344">The following example shows how to use the first function with an array and string.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

<span data-ttu-id="10562-345">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-345">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-346">Név</span><span class="sxs-lookup"><span data-stu-id="10562-346">Name</span></span> | <span data-ttu-id="10562-347">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-347">Type</span></span> | <span data-ttu-id="10562-348">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-348">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-349">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-349">arrayOutput</span></span> | <span data-ttu-id="10562-350">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-350">String</span></span> | <span data-ttu-id="10562-351">egy</span><span class="sxs-lookup"><span data-stu-id="10562-351">one</span></span> |
| <span data-ttu-id="10562-352">stringOutput</span><span class="sxs-lookup"><span data-stu-id="10562-352">stringOutput</span></span> | <span data-ttu-id="10562-353">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-353">String</span></span> | <span data-ttu-id="10562-354">O</span><span class="sxs-lookup"><span data-stu-id="10562-354">O</span></span> |

<a id="intersection" />

## <a name="intersection"></a><span data-ttu-id="10562-355">metszetének</span><span class="sxs-lookup"><span data-stu-id="10562-355">intersection</span></span>
`intersection(arg1, arg2, arg3, ...)`

<span data-ttu-id="10562-356">A paraméterek egy egyetlen tömb vagy objektum a szokványos elemeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-356">Returns a single array or object with the common elements from the parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-357">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-357">Parameters</span></span>

| <span data-ttu-id="10562-358">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-358">Parameter</span></span> | <span data-ttu-id="10562-359">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-359">Required</span></span> | <span data-ttu-id="10562-360">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-360">Type</span></span> | <span data-ttu-id="10562-361">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-361">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-362">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-362">arg1</span></span> |<span data-ttu-id="10562-363">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-363">Yes</span></span> |<span data-ttu-id="10562-364">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-364">array or object</span></span> |<span data-ttu-id="10562-365">Az első értéket közös elemek kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="10562-365">The first value to use for finding common elements.</span></span> |
| <span data-ttu-id="10562-366">Arg2</span><span class="sxs-lookup"><span data-stu-id="10562-366">arg2</span></span> |<span data-ttu-id="10562-367">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-367">Yes</span></span> |<span data-ttu-id="10562-368">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-368">array or object</span></span> |<span data-ttu-id="10562-369">A második érték közös elemek kereséséhez használja.</span><span class="sxs-lookup"><span data-stu-id="10562-369">The second value to use for finding common elements.</span></span> |
| <span data-ttu-id="10562-370">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="10562-370">additional arguments</span></span> |<span data-ttu-id="10562-371">Nem</span><span class="sxs-lookup"><span data-stu-id="10562-371">No</span></span> |<span data-ttu-id="10562-372">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-372">array or object</span></span> |<span data-ttu-id="10562-373">Közös elemek kereséséhez használni kívánt további értékeket.</span><span class="sxs-lookup"><span data-stu-id="10562-373">Additional values to use for finding common elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-374">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-374">Return value</span></span>

<span data-ttu-id="10562-375">Tömb vagy objektum a szokványos elemeket.</span><span class="sxs-lookup"><span data-stu-id="10562-375">An array or object with the common elements.</span></span>

### <a name="example"></a><span data-ttu-id="10562-376">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-376">Example</span></span>

<span data-ttu-id="10562-377">A következő példa bemutatja, hogyan használható metszetének tömbök és objektumok:</span><span class="sxs-lookup"><span data-stu-id="10562-377">The following example shows how to use intersection with arrays and objects:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "z", "three": "c"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[intersection(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[intersection(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

<span data-ttu-id="10562-378">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-378">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-379">Név</span><span class="sxs-lookup"><span data-stu-id="10562-379">Name</span></span> | <span data-ttu-id="10562-380">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-380">Type</span></span> | <span data-ttu-id="10562-381">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-381">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-382">objectOutput</span><span class="sxs-lookup"><span data-stu-id="10562-382">objectOutput</span></span> | <span data-ttu-id="10562-383">Objektum</span><span class="sxs-lookup"><span data-stu-id="10562-383">Object</span></span> | <span data-ttu-id="10562-384">{"egy": "a", "három": "c"}</span><span class="sxs-lookup"><span data-stu-id="10562-384">{"one": "a", "three": "c"}</span></span> |
| <span data-ttu-id="10562-385">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-385">arrayOutput</span></span> | <span data-ttu-id="10562-386">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-386">Array</span></span> | <span data-ttu-id="10562-387">["két", "három"]</span><span class="sxs-lookup"><span data-stu-id="10562-387">["two", "three"]</span></span> |


## <a name="json"></a><span data-ttu-id="10562-388">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="10562-388">json</span></span>
`json(arg1)`

<span data-ttu-id="10562-389">A JSON-objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-389">Returns a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-390">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-390">Parameters</span></span>

| <span data-ttu-id="10562-391">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-391">Parameter</span></span> | <span data-ttu-id="10562-392">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-392">Required</span></span> | <span data-ttu-id="10562-393">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-393">Type</span></span> | <span data-ttu-id="10562-394">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-394">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-395">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-395">arg1</span></span> |<span data-ttu-id="10562-396">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-396">Yes</span></span> |<span data-ttu-id="10562-397">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-397">string</span></span> |<span data-ttu-id="10562-398">Az érték átalakítása JSON.</span><span class="sxs-lookup"><span data-stu-id="10562-398">The value to convert to JSON.</span></span> |


### <a name="return-value"></a><span data-ttu-id="10562-399">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-399">Return value</span></span>

<span data-ttu-id="10562-400">A JSON-objektum a megadott karakterlánc vagy egy üres objektum amikor **null** van megadva.</span><span class="sxs-lookup"><span data-stu-id="10562-400">The JSON object from the specified string, or an empty object when **null** is specified.</span></span>

### <a name="example"></a><span data-ttu-id="10562-401">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-401">Example</span></span>

<span data-ttu-id="10562-402">A következő példa bemutatja, hogyan használható metszetének tömbök és objektumok:</span><span class="sxs-lookup"><span data-stu-id="10562-402">The following example shows how to use intersection with arrays and objects:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "jsonOutput": {
            "type": "object",
            "value": "[json('{\"a\": \"b\"}')]"
        },
        "nullOutput": {
            "type": "bool",
            "value": "[empty(json('null'))]"
        }
    }
}
```

<span data-ttu-id="10562-403">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-403">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-404">Név</span><span class="sxs-lookup"><span data-stu-id="10562-404">Name</span></span> | <span data-ttu-id="10562-405">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-405">Type</span></span> | <span data-ttu-id="10562-406">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-406">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-407">jsonOutput</span><span class="sxs-lookup"><span data-stu-id="10562-407">jsonOutput</span></span> | <span data-ttu-id="10562-408">Objektum</span><span class="sxs-lookup"><span data-stu-id="10562-408">Object</span></span> | <span data-ttu-id="10562-409">{"a": "b"}</span><span class="sxs-lookup"><span data-stu-id="10562-409">{"a": "b"}</span></span> |
| <span data-ttu-id="10562-410">nullOutput</span><span class="sxs-lookup"><span data-stu-id="10562-410">nullOutput</span></span> | <span data-ttu-id="10562-411">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="10562-411">Boolean</span></span> | <span data-ttu-id="10562-412">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10562-412">True</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="10562-413">utolsó</span><span class="sxs-lookup"><span data-stu-id="10562-413">last</span></span>
`last (arg1)`

<span data-ttu-id="10562-414">A tömb utolsó eleme, vagy a karakterlánc utolsó karakterét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-414">Returns the last element of the array, or last character of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-415">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-415">Parameters</span></span>

| <span data-ttu-id="10562-416">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-416">Parameter</span></span> | <span data-ttu-id="10562-417">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-417">Required</span></span> | <span data-ttu-id="10562-418">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-418">Type</span></span> | <span data-ttu-id="10562-419">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-420">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-420">arg1</span></span> |<span data-ttu-id="10562-421">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-421">Yes</span></span> |<span data-ttu-id="10562-422">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-422">array or string</span></span> |<span data-ttu-id="10562-423">Az érték utolsó karakter vagy elem lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="10562-423">The value to retrieve the last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-424">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-424">Return value</span></span>

<span data-ttu-id="10562-425">A típus (karakterlánc, int, tömb vagy objektum) utolsó elemének tömb vagy karakterlánc az utolsó karakter.</span><span class="sxs-lookup"><span data-stu-id="10562-425">The type (string, int, array, or object) of the last element in an array, or the last character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="10562-426">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-426">Example</span></span>

<span data-ttu-id="10562-427">A következő példa bemutatja, hogyan használható az utolsó függvény egy tömb és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="10562-427">The following example shows how to use the last function with an array and string.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

<span data-ttu-id="10562-428">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-428">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-429">Név</span><span class="sxs-lookup"><span data-stu-id="10562-429">Name</span></span> | <span data-ttu-id="10562-430">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-430">Type</span></span> | <span data-ttu-id="10562-431">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-432">arrayOutput</span></span> | <span data-ttu-id="10562-433">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-433">String</span></span> | <span data-ttu-id="10562-434">három</span><span class="sxs-lookup"><span data-stu-id="10562-434">three</span></span> |
| <span data-ttu-id="10562-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="10562-435">stringOutput</span></span> | <span data-ttu-id="10562-436">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-436">String</span></span> | <span data-ttu-id="10562-437">E</span><span class="sxs-lookup"><span data-stu-id="10562-437">e</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="10562-438">Hossza</span><span class="sxs-lookup"><span data-stu-id="10562-438">length</span></span>
`length(arg1)`

<span data-ttu-id="10562-439">A tömb, vagy egy karakterlánc karaktereinek elemek számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-439">Returns the number of elements in an array, or characters in a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-440">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-440">Parameters</span></span>

| <span data-ttu-id="10562-441">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-441">Parameter</span></span> | <span data-ttu-id="10562-442">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-442">Required</span></span> | <span data-ttu-id="10562-443">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-443">Type</span></span> | <span data-ttu-id="10562-444">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-444">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-445">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-445">arg1</span></span> |<span data-ttu-id="10562-446">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-446">Yes</span></span> |<span data-ttu-id="10562-447">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-447">array or string</span></span> |<span data-ttu-id="10562-448">A tömb használni az első elemek, vagy a karakterlánc első karakterek használata.</span><span class="sxs-lookup"><span data-stu-id="10562-448">The array to use for getting the number of elements, or the string to use for getting the number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-449">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-449">Return value</span></span>

<span data-ttu-id="10562-450">Egy egész szám.</span><span class="sxs-lookup"><span data-stu-id="10562-450">An int.</span></span> 

### <a name="example"></a><span data-ttu-id="10562-451">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-451">Example</span></span>

<span data-ttu-id="10562-452">A következő példa bemutatja, hogyan egy tömb és a karakterlánc hossza használhat:</span><span class="sxs-lookup"><span data-stu-id="10562-452">The following example shows how to use length with an array and string:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

<span data-ttu-id="10562-453">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-453">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-454">Név</span><span class="sxs-lookup"><span data-stu-id="10562-454">Name</span></span> | <span data-ttu-id="10562-455">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-455">Type</span></span> | <span data-ttu-id="10562-456">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-456">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-457">arrayLength</span><span class="sxs-lookup"><span data-stu-id="10562-457">arrayLength</span></span> | <span data-ttu-id="10562-458">int</span><span class="sxs-lookup"><span data-stu-id="10562-458">Int</span></span> | <span data-ttu-id="10562-459">3</span><span class="sxs-lookup"><span data-stu-id="10562-459">3</span></span> |
| <span data-ttu-id="10562-460">stringLength</span><span class="sxs-lookup"><span data-stu-id="10562-460">stringLength</span></span> | <span data-ttu-id="10562-461">int</span><span class="sxs-lookup"><span data-stu-id="10562-461">Int</span></span> | <span data-ttu-id="10562-462">13</span><span class="sxs-lookup"><span data-stu-id="10562-462">13</span></span> |

<span data-ttu-id="10562-463">Ez a funkció a tömb segítségével adja meg az ismétlések száma erőforrások létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="10562-463">You can use this function with an array to specify the number of iterations when creating resources.</span></span> <span data-ttu-id="10562-464">A következő példában a paraméter **siteNames** hivatkozna webhelyek létrehozásakor használandó tömbjét.</span><span class="sxs-lookup"><span data-stu-id="10562-464">In the following example, the parameter **siteNames** would refer to an array of names to use when creating the web sites.</span></span>

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

<span data-ttu-id="10562-465">Ez a függvény egy tömb használatával kapcsolatban további információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="10562-465">For more information about using this function with an array, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<a id="min" />

## <a name="min"></a><span data-ttu-id="10562-466">perc</span><span class="sxs-lookup"><span data-stu-id="10562-466">min</span></span>
`min(arg1)`

<span data-ttu-id="10562-467">Egy számokból álló tömb vagy egészek vesszővel elválasztott listáját a minimális értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-467">Returns the minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-468">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-468">Parameters</span></span>

| <span data-ttu-id="10562-469">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-469">Parameter</span></span> | <span data-ttu-id="10562-470">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-470">Required</span></span> | <span data-ttu-id="10562-471">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-471">Type</span></span> | <span data-ttu-id="10562-472">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-472">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-473">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-473">arg1</span></span> |<span data-ttu-id="10562-474">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-474">Yes</span></span> |<span data-ttu-id="10562-475">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="10562-475">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="10562-476">A gyűjteményt, amelyben a minimális érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="10562-476">The collection to get the minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-477">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-477">Return value</span></span>

<span data-ttu-id="10562-478">Az int minimális értékét képviselő.</span><span class="sxs-lookup"><span data-stu-id="10562-478">An int representing the minimum value.</span></span>

### <a name="example"></a><span data-ttu-id="10562-479">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-479">Example</span></span>

<span data-ttu-id="10562-480">A következő példa bemutatja, hogyan használható min tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="10562-480">The following example shows how to use min with an array and a list of integers:</span></span>

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

<span data-ttu-id="10562-481">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-481">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-482">Név</span><span class="sxs-lookup"><span data-stu-id="10562-482">Name</span></span> | <span data-ttu-id="10562-483">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-483">Type</span></span> | <span data-ttu-id="10562-484">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-484">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-485">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-485">arrayOutput</span></span> | <span data-ttu-id="10562-486">int</span><span class="sxs-lookup"><span data-stu-id="10562-486">Int</span></span> | <span data-ttu-id="10562-487">0</span><span class="sxs-lookup"><span data-stu-id="10562-487">0</span></span> |
| <span data-ttu-id="10562-488">intOutput</span><span class="sxs-lookup"><span data-stu-id="10562-488">intOutput</span></span> | <span data-ttu-id="10562-489">int</span><span class="sxs-lookup"><span data-stu-id="10562-489">Int</span></span> | <span data-ttu-id="10562-490">0</span><span class="sxs-lookup"><span data-stu-id="10562-490">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="10562-491">maximális</span><span class="sxs-lookup"><span data-stu-id="10562-491">max</span></span>
`max(arg1)`

<span data-ttu-id="10562-492">A maximális érték egész számok tömb vagy egészek vesszővel elválasztott listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-492">Returns the maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-493">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-493">Parameters</span></span>

| <span data-ttu-id="10562-494">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-494">Parameter</span></span> | <span data-ttu-id="10562-495">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-495">Required</span></span> | <span data-ttu-id="10562-496">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-496">Type</span></span> | <span data-ttu-id="10562-497">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-497">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-498">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-498">arg1</span></span> |<span data-ttu-id="10562-499">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-499">Yes</span></span> |<span data-ttu-id="10562-500">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="10562-500">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="10562-501">A gyűjteményt, amelyben a legnagyobb érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="10562-501">The collection to get the maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-502">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-502">Return value</span></span>

<span data-ttu-id="10562-503">Az int maximális értékét képviselő.</span><span class="sxs-lookup"><span data-stu-id="10562-503">An int representing the maximum value.</span></span>

### <a name="example"></a><span data-ttu-id="10562-504">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-504">Example</span></span>

<span data-ttu-id="10562-505">A következő példa bemutatja, hogyan használható maximum tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="10562-505">The following example shows how to use max with an array and a list of integers:</span></span>

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

<span data-ttu-id="10562-506">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-506">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-507">Név</span><span class="sxs-lookup"><span data-stu-id="10562-507">Name</span></span> | <span data-ttu-id="10562-508">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-508">Type</span></span> | <span data-ttu-id="10562-509">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-509">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-510">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-510">arrayOutput</span></span> | <span data-ttu-id="10562-511">int</span><span class="sxs-lookup"><span data-stu-id="10562-511">Int</span></span> | <span data-ttu-id="10562-512">5</span><span class="sxs-lookup"><span data-stu-id="10562-512">5</span></span> |
| <span data-ttu-id="10562-513">intOutput</span><span class="sxs-lookup"><span data-stu-id="10562-513">intOutput</span></span> | <span data-ttu-id="10562-514">int</span><span class="sxs-lookup"><span data-stu-id="10562-514">Int</span></span> | <span data-ttu-id="10562-515">5</span><span class="sxs-lookup"><span data-stu-id="10562-515">5</span></span> |

<a id="range" />

## <a name="range"></a><span data-ttu-id="10562-516">tartomány</span><span class="sxs-lookup"><span data-stu-id="10562-516">range</span></span>
`range(startingInteger, numberOfElements)`

<span data-ttu-id="10562-517">Egy számokból álló tömb egy egész kezdő- és néhány elemet tartalmazó jön létre.</span><span class="sxs-lookup"><span data-stu-id="10562-517">Creates an array of integers from a starting integer and containing a number of items.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-518">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-518">Parameters</span></span>

| <span data-ttu-id="10562-519">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-519">Parameter</span></span> | <span data-ttu-id="10562-520">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-520">Required</span></span> | <span data-ttu-id="10562-521">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-521">Type</span></span> | <span data-ttu-id="10562-522">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-522">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-523">startingInteger</span><span class="sxs-lookup"><span data-stu-id="10562-523">startingInteger</span></span> |<span data-ttu-id="10562-524">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-524">Yes</span></span> |<span data-ttu-id="10562-525">int</span><span class="sxs-lookup"><span data-stu-id="10562-525">int</span></span> |<span data-ttu-id="10562-526">A tömb első egész szám.</span><span class="sxs-lookup"><span data-stu-id="10562-526">The first integer in the array.</span></span> |
| <span data-ttu-id="10562-527">numberofElements</span><span class="sxs-lookup"><span data-stu-id="10562-527">numberofElements</span></span> |<span data-ttu-id="10562-528">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-528">Yes</span></span> |<span data-ttu-id="10562-529">int</span><span class="sxs-lookup"><span data-stu-id="10562-529">int</span></span> |<span data-ttu-id="10562-530">A tömb egész számok száma.</span><span class="sxs-lookup"><span data-stu-id="10562-530">The number of integers in the array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-531">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-531">Return value</span></span>

<span data-ttu-id="10562-532">Egy számokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="10562-532">An array of integers.</span></span>

### <a name="example"></a><span data-ttu-id="10562-533">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-533">Example</span></span>

<span data-ttu-id="10562-534">A következő példa bemutatja, hogyan használja a tartomány funkciót:</span><span class="sxs-lookup"><span data-stu-id="10562-534">The following example shows how to use the range function:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "startingInt": {
            "type": "int",
            "defaultValue": 5
        },
        "numberOfElements": {
            "type": "int",
            "defaultValue": 3
        }
    },
    "resources": [],
    "outputs": {
        "rangeOutput": {
            "type": "array",
            "value": "[range(parameters('startingInt'),parameters('numberOfElements'))]"
        }
    }
}
```

<span data-ttu-id="10562-535">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-535">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-536">Név</span><span class="sxs-lookup"><span data-stu-id="10562-536">Name</span></span> | <span data-ttu-id="10562-537">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-537">Type</span></span> | <span data-ttu-id="10562-538">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-538">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-539">rangeOutput</span><span class="sxs-lookup"><span data-stu-id="10562-539">rangeOutput</span></span> | <span data-ttu-id="10562-540">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-540">Array</span></span> | <span data-ttu-id="10562-541">[5, 6, 7]</span><span class="sxs-lookup"><span data-stu-id="10562-541">[5, 6, 7]</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="10562-542">Kihagyása</span><span class="sxs-lookup"><span data-stu-id="10562-542">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="10562-543">A tömb a megadott szám után az összes elem tömböt ad vissza, vagy az összes karakter karakterláncot ad vissza a megadott szám a karakterlánc után.</span><span class="sxs-lookup"><span data-stu-id="10562-543">Returns an array with all the elements after the specified number in the array, or returns a string with all the characters after the specified number in the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-544">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-544">Parameters</span></span>

| <span data-ttu-id="10562-545">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-545">Parameter</span></span> | <span data-ttu-id="10562-546">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-546">Required</span></span> | <span data-ttu-id="10562-547">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-547">Type</span></span> | <span data-ttu-id="10562-548">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-548">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-549">originalValue</span><span class="sxs-lookup"><span data-stu-id="10562-549">originalValue</span></span> |<span data-ttu-id="10562-550">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-550">Yes</span></span> |<span data-ttu-id="10562-551">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-551">array or string</span></span> |<span data-ttu-id="10562-552">A tömb vagy karakterlánc kihagyása használandó.</span><span class="sxs-lookup"><span data-stu-id="10562-552">The array or string to use for skipping.</span></span> |
| <span data-ttu-id="10562-553">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="10562-553">numberToSkip</span></span> |<span data-ttu-id="10562-554">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-554">Yes</span></span> |<span data-ttu-id="10562-555">int</span><span class="sxs-lookup"><span data-stu-id="10562-555">int</span></span> |<span data-ttu-id="10562-556">Elemek vagy kihagyását karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="10562-556">The number of elements or characters to skip.</span></span> <span data-ttu-id="10562-557">Ha ez az érték 0 vagy kisebb, a elemek vagy az karaktere adott vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-557">If this value is 0 or less, all the elements or characters in the value are returned.</span></span> <span data-ttu-id="10562-558">Ha a tömb vagy karakterlánc hossza nagyobb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-558">If it is larger than the length of the array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-559">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-559">Return value</span></span>

<span data-ttu-id="10562-560">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="10562-560">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="10562-561">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-561">Example</span></span>

<span data-ttu-id="10562-562">Az alábbi példa kihagyja a megadott számú elemet a tömbben, és a megadott számú karaktert egy karakterláncon belül.</span><span class="sxs-lookup"><span data-stu-id="10562-562">The following example skips the specified number of elements in the array, and the specified number of characters in a string.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

<span data-ttu-id="10562-563">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-563">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-564">Név</span><span class="sxs-lookup"><span data-stu-id="10562-564">Name</span></span> | <span data-ttu-id="10562-565">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-565">Type</span></span> | <span data-ttu-id="10562-566">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-566">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-567">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-567">arrayOutput</span></span> | <span data-ttu-id="10562-568">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-568">Array</span></span> | <span data-ttu-id="10562-569">["három"]</span><span class="sxs-lookup"><span data-stu-id="10562-569">["three"]</span></span> |
| <span data-ttu-id="10562-570">stringOutput</span><span class="sxs-lookup"><span data-stu-id="10562-570">stringOutput</span></span> | <span data-ttu-id="10562-571">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-571">String</span></span> | <span data-ttu-id="10562-572">két három</span><span class="sxs-lookup"><span data-stu-id="10562-572">two three</span></span> |

<a id="take" />

## <a name="take"></a><span data-ttu-id="10562-573">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="10562-573">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="10562-574">A tömb, vagy a megadott számú karaktert a karakterlánc elejéről. a karakterlánc kezdetét adja vissza egy tömb a megadott számú elemet.</span><span class="sxs-lookup"><span data-stu-id="10562-574">Returns an array with the specified number of elements from the start of the array, or a string with the specified number of characters from the start of the string.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-575">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-575">Parameters</span></span>

| <span data-ttu-id="10562-576">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-576">Parameter</span></span> | <span data-ttu-id="10562-577">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-577">Required</span></span> | <span data-ttu-id="10562-578">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-578">Type</span></span> | <span data-ttu-id="10562-579">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-579">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-580">originalValue</span><span class="sxs-lookup"><span data-stu-id="10562-580">originalValue</span></span> |<span data-ttu-id="10562-581">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-581">Yes</span></span> |<span data-ttu-id="10562-582">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-582">array or string</span></span> |<span data-ttu-id="10562-583">A tömb vagy karakterlánc az elemek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="10562-583">The array or string to take the elements from.</span></span> |
| <span data-ttu-id="10562-584">numberToTake</span><span class="sxs-lookup"><span data-stu-id="10562-584">numberToTake</span></span> |<span data-ttu-id="10562-585">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-585">Yes</span></span> |<span data-ttu-id="10562-586">int</span><span class="sxs-lookup"><span data-stu-id="10562-586">int</span></span> |<span data-ttu-id="10562-587">Elemek és érvénybe karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="10562-587">The number of elements or characters to take.</span></span> <span data-ttu-id="10562-588">Ha ez az érték 0 vagy kisebb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-588">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="10562-589">Ha a megadott tömb vagy karakterlánc hossza nagyobb, a tömb vagy karakterlánc összes elemet visszaadja a.</span><span class="sxs-lookup"><span data-stu-id="10562-589">If it is larger than the length of the given array or string, all the elements in the array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-590">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-590">Return value</span></span>

<span data-ttu-id="10562-591">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="10562-591">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="10562-592">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-592">Example</span></span>

<span data-ttu-id="10562-593">A következő példa a tömb elemei, és a karakterek egy karakterlánc megadott számú vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="10562-593">The following example takes the specified number of elements from the array, and characters from a string.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

<span data-ttu-id="10562-594">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-594">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-595">Név</span><span class="sxs-lookup"><span data-stu-id="10562-595">Name</span></span> | <span data-ttu-id="10562-596">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-596">Type</span></span> | <span data-ttu-id="10562-597">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-597">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-598">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-598">arrayOutput</span></span> | <span data-ttu-id="10562-599">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-599">Array</span></span> | <span data-ttu-id="10562-600">["egy", "két"]</span><span class="sxs-lookup"><span data-stu-id="10562-600">["one", "two"]</span></span> |
| <span data-ttu-id="10562-601">stringOutput</span><span class="sxs-lookup"><span data-stu-id="10562-601">stringOutput</span></span> | <span data-ttu-id="10562-602">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="10562-602">String</span></span> | <span data-ttu-id="10562-603">a</span><span class="sxs-lookup"><span data-stu-id="10562-603">on</span></span> |

<a id="union" />

## <a name="union"></a><span data-ttu-id="10562-604">a UNION</span><span class="sxs-lookup"><span data-stu-id="10562-604">union</span></span>
`union(arg1, arg2, arg3, ...)`

<span data-ttu-id="10562-605">A paraméterek egy egyetlen tömb vagy objektum minden elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="10562-605">Returns a single array or object with all elements from the parameters.</span></span> <span data-ttu-id="10562-606">Ismétlődő vagy kulcsok vannak csak egyszer tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="10562-606">Duplicate values or keys are only included once.</span></span>

### <a name="parameters"></a><span data-ttu-id="10562-607">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="10562-607">Parameters</span></span>

| <span data-ttu-id="10562-608">Paraméter</span><span class="sxs-lookup"><span data-stu-id="10562-608">Parameter</span></span> | <span data-ttu-id="10562-609">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10562-609">Required</span></span> | <span data-ttu-id="10562-610">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-610">Type</span></span> | <span data-ttu-id="10562-611">Leírás</span><span class="sxs-lookup"><span data-stu-id="10562-611">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="10562-612">arg1</span><span class="sxs-lookup"><span data-stu-id="10562-612">arg1</span></span> |<span data-ttu-id="10562-613">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-613">Yes</span></span> |<span data-ttu-id="10562-614">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-614">array or object</span></span> |<span data-ttu-id="10562-615">Az első érték való csatlakozás elemek.</span><span class="sxs-lookup"><span data-stu-id="10562-615">The first value to use for joining elements.</span></span> |
| <span data-ttu-id="10562-616">Arg2</span><span class="sxs-lookup"><span data-stu-id="10562-616">arg2</span></span> |<span data-ttu-id="10562-617">Igen</span><span class="sxs-lookup"><span data-stu-id="10562-617">Yes</span></span> |<span data-ttu-id="10562-618">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-618">array or object</span></span> |<span data-ttu-id="10562-619">A második érték való csatlakozás elemek.</span><span class="sxs-lookup"><span data-stu-id="10562-619">The second value to use for joining elements.</span></span> |
| <span data-ttu-id="10562-620">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="10562-620">additional arguments</span></span> |<span data-ttu-id="10562-621">Nem</span><span class="sxs-lookup"><span data-stu-id="10562-621">No</span></span> |<span data-ttu-id="10562-622">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="10562-622">array or object</span></span> |<span data-ttu-id="10562-623">További értékeket való csatlakozás elemek.</span><span class="sxs-lookup"><span data-stu-id="10562-623">Additional values to use for joining elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="10562-624">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="10562-624">Return value</span></span>

<span data-ttu-id="10562-625">Tömb vagy objektum.</span><span class="sxs-lookup"><span data-stu-id="10562-625">An array or object.</span></span>

### <a name="example"></a><span data-ttu-id="10562-626">Példa</span><span class="sxs-lookup"><span data-stu-id="10562-626">Example</span></span>

<span data-ttu-id="10562-627">A következő példa bemutatja, hogyan használható az union tömbök és objektumok:</span><span class="sxs-lookup"><span data-stu-id="10562-627">The following example shows how to use union with arrays and objects:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"three": "c", "four": "d", "five": "e"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["three", "four"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[union(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[union(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

<span data-ttu-id="10562-628">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="10562-628">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="10562-629">Név</span><span class="sxs-lookup"><span data-stu-id="10562-629">Name</span></span> | <span data-ttu-id="10562-630">Típus</span><span class="sxs-lookup"><span data-stu-id="10562-630">Type</span></span> | <span data-ttu-id="10562-631">Érték</span><span class="sxs-lookup"><span data-stu-id="10562-631">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="10562-632">objectOutput</span><span class="sxs-lookup"><span data-stu-id="10562-632">objectOutput</span></span> | <span data-ttu-id="10562-633">Objektum</span><span class="sxs-lookup"><span data-stu-id="10562-633">Object</span></span> | <span data-ttu-id="10562-634">{"egy": "a", "2": "b", "három": "c", "négy": "d", "5": "e"}</span><span class="sxs-lookup"><span data-stu-id="10562-634">{"one": "a", "two": "b", "three": "c", "four": "d", "five": "e"}</span></span> |
| <span data-ttu-id="10562-635">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="10562-635">arrayOutput</span></span> | <span data-ttu-id="10562-636">A tömb</span><span class="sxs-lookup"><span data-stu-id="10562-636">Array</span></span> | <span data-ttu-id="10562-637">["egy", "két", "három", "négy"]</span><span class="sxs-lookup"><span data-stu-id="10562-637">["one", "two", "three", "four"]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="10562-638">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10562-638">Next steps</span></span>
* <span data-ttu-id="10562-639">A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="10562-639">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="10562-640">Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="10562-640">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="10562-641">Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="10562-641">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="10562-642">A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="10562-642">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


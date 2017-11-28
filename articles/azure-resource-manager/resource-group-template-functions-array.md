---
title: "aaaAzure Resource Manager-sablon működik - tömbállandó és objektumok |} Microsoft Docs"
description: "Hello funkciók toouse tömbök és objektumok használata az Azure Resource Manager sablon ismerteti."
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
ms.openlocfilehash: e5f1a9b2a71039562eae7e48c2474a1fa59a7bea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="2f84f-103">Az Azure Resource Manager sablonokhoz tárolótömböt és az objektum funkciók</span><span class="sxs-lookup"><span data-stu-id="2f84f-103">Array and object functions for Azure Resource Manager templates</span></span> 

<span data-ttu-id="2f84f-104">Erőforrás-kezelő számos funkciókat nyújt, tömbök és objektumok.</span><span class="sxs-lookup"><span data-stu-id="2f84f-104">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="2f84f-105">a tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-105">array</span></span>](#array)
* [<span data-ttu-id="2f84f-106">Egyesítés</span><span class="sxs-lookup"><span data-stu-id="2f84f-106">coalesce</span></span>](#coalesce)
* [<span data-ttu-id="2f84f-107">Concat</span><span class="sxs-lookup"><span data-stu-id="2f84f-107">concat</span></span>](#concat)
* [<span data-ttu-id="2f84f-108">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="2f84f-108">contains</span></span>](#contains)
* [<span data-ttu-id="2f84f-109">createArray</span><span class="sxs-lookup"><span data-stu-id="2f84f-109">createArray</span></span>](#createarray)
* [<span data-ttu-id="2f84f-110">üres</span><span class="sxs-lookup"><span data-stu-id="2f84f-110">empty</span></span>](#empty)
* [<span data-ttu-id="2f84f-111">első</span><span class="sxs-lookup"><span data-stu-id="2f84f-111">first</span></span>](#first)
* [<span data-ttu-id="2f84f-112">metszetének</span><span class="sxs-lookup"><span data-stu-id="2f84f-112">intersection</span></span>](#intersection)
* [<span data-ttu-id="2f84f-113">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="2f84f-113">json</span></span>](#json)
* [<span data-ttu-id="2f84f-114">utolsó</span><span class="sxs-lookup"><span data-stu-id="2f84f-114">last</span></span>](#last)
* [<span data-ttu-id="2f84f-115">hossza</span><span class="sxs-lookup"><span data-stu-id="2f84f-115">length</span></span>](#length)
* [<span data-ttu-id="2f84f-116">perc</span><span class="sxs-lookup"><span data-stu-id="2f84f-116">min</span></span>](#min)
* [<span data-ttu-id="2f84f-117">maximális</span><span class="sxs-lookup"><span data-stu-id="2f84f-117">max</span></span>](#max)
* [<span data-ttu-id="2f84f-118">tartomány</span><span class="sxs-lookup"><span data-stu-id="2f84f-118">range</span></span>](#range)
* [<span data-ttu-id="2f84f-119">hagyja ki</span><span class="sxs-lookup"><span data-stu-id="2f84f-119">skip</span></span>](#skip)
* [<span data-ttu-id="2f84f-120">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="2f84f-120">take</span></span>](#take)
* [<span data-ttu-id="2f84f-121">a UNION</span><span class="sxs-lookup"><span data-stu-id="2f84f-121">union</span></span>](#union)

<span data-ttu-id="2f84f-122">egy érték elválasztott karakterlánc-értékek tömbje tooget lásd: [vágási](resource-group-template-functions-string.md#split).</span><span class="sxs-lookup"><span data-stu-id="2f84f-122">tooget an array of string values delimited by a value, see [split](resource-group-template-functions-string.md#split).</span></span>

<a id="array" />

## <a name="array"></a><span data-ttu-id="2f84f-123">A tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-123">array</span></span>
`array(convertToArray)`

<span data-ttu-id="2f84f-124">Hello tooan értéktömb alakítja.</span><span class="sxs-lookup"><span data-stu-id="2f84f-124">Converts hello value tooan array.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-125">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-125">Parameters</span></span>

| <span data-ttu-id="2f84f-126">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-126">Parameter</span></span> | <span data-ttu-id="2f84f-127">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-127">Required</span></span> | <span data-ttu-id="2f84f-128">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-128">Type</span></span> | <span data-ttu-id="2f84f-129">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-129">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-130">convertToArray</span><span class="sxs-lookup"><span data-stu-id="2f84f-130">convertToArray</span></span> |<span data-ttu-id="2f84f-131">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-131">Yes</span></span> |<span data-ttu-id="2f84f-132">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-132">int, string, array, or object</span></span> |<span data-ttu-id="2f84f-133">hello értéktömb tooconvert tooan.</span><span class="sxs-lookup"><span data-stu-id="2f84f-133">hello value tooconvert tooan array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-134">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-134">Return value</span></span>

<span data-ttu-id="2f84f-135">Egy tömb.</span><span class="sxs-lookup"><span data-stu-id="2f84f-135">An array.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-136">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-136">Example</span></span>

<span data-ttu-id="2f84f-137">hello következő példa bemutatja, hogyan toouse hello különböző típusú tömb függvény.</span><span class="sxs-lookup"><span data-stu-id="2f84f-137">hello following example shows how toouse hello array function with different types.</span></span>

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

<span data-ttu-id="2f84f-138">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-138">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-139">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-139">Name</span></span> | <span data-ttu-id="2f84f-140">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-140">Type</span></span> | <span data-ttu-id="2f84f-141">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-141">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-142">intOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-142">intOutput</span></span> | <span data-ttu-id="2f84f-143">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-143">Array</span></span> | <span data-ttu-id="2f84f-144">[1]</span><span class="sxs-lookup"><span data-stu-id="2f84f-144">[1]</span></span> |
| <span data-ttu-id="2f84f-145">stringOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-145">stringOutput</span></span> | <span data-ttu-id="2f84f-146">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-146">Array</span></span> | <span data-ttu-id="2f84f-147">["a"]</span><span class="sxs-lookup"><span data-stu-id="2f84f-147">["a"]</span></span> |
| <span data-ttu-id="2f84f-148">objectOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-148">objectOutput</span></span> | <span data-ttu-id="2f84f-149">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-149">Array</span></span> | <span data-ttu-id="2f84f-150">[{"a": "b", "c": "d"}]</span><span class="sxs-lookup"><span data-stu-id="2f84f-150">[{"a": "b", "c": "d"}]</span></span> |

<a id="coalesce" />

## <a name="coalesce"></a><span data-ttu-id="2f84f-151">Egyesítés</span><span class="sxs-lookup"><span data-stu-id="2f84f-151">coalesce</span></span>
`coalesce(arg1, arg2, arg3, ...)`

<span data-ttu-id="2f84f-152">Hello paraméterek első nem null értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-152">Returns first non-null value from hello parameters.</span></span> <span data-ttu-id="2f84f-153">Üres karakterláncokat, üres tömbök és üres objektumok csak NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="2f84f-153">Empty strings, empty arrays, and empty objects are not null.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-154">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-154">Parameters</span></span>

| <span data-ttu-id="2f84f-155">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-155">Parameter</span></span> | <span data-ttu-id="2f84f-156">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-156">Required</span></span> | <span data-ttu-id="2f84f-157">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-157">Type</span></span> | <span data-ttu-id="2f84f-158">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-158">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-159">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-159">arg1</span></span> |<span data-ttu-id="2f84f-160">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-160">Yes</span></span> |<span data-ttu-id="2f84f-161">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-161">int, string, array, or object</span></span> |<span data-ttu-id="2f84f-162">hello első érték tootest a NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="2f84f-162">hello first value tootest for null.</span></span> |
| <span data-ttu-id="2f84f-163">További argumentum</span><span class="sxs-lookup"><span data-stu-id="2f84f-163">additional args</span></span> |<span data-ttu-id="2f84f-164">Nem</span><span class="sxs-lookup"><span data-stu-id="2f84f-164">No</span></span> |<span data-ttu-id="2f84f-165">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-165">int, string, array, or object</span></span> |<span data-ttu-id="2f84f-166">További értékeket tootest a NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="2f84f-166">Additional values tootest for null.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-167">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-167">Return value</span></span>

<span data-ttu-id="2f84f-168">hello érték hello első null értékű paramétert, amely egy karakterlánc, int, tömb vagy objektum lehet.</span><span class="sxs-lookup"><span data-stu-id="2f84f-168">hello value of hello first non-null parameters, which can be a string, int, array, or object.</span></span> <span data-ttu-id="2f84f-169">NULL értékű, ha az összes paraméterei null értékű.</span><span class="sxs-lookup"><span data-stu-id="2f84f-169">Null if all parameters are null.</span></span> 

### <a name="example"></a><span data-ttu-id="2f84f-170">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-170">Example</span></span>

<span data-ttu-id="2f84f-171">hello következő példa bemutatja, coalesce különböző használatát hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="2f84f-171">hello following example shows hello output from different uses of coalesce.</span></span>

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

<span data-ttu-id="2f84f-172">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-172">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-173">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-173">Name</span></span> | <span data-ttu-id="2f84f-174">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-174">Type</span></span> | <span data-ttu-id="2f84f-175">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-175">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-176">stringOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-176">stringOutput</span></span> | <span data-ttu-id="2f84f-177">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-177">String</span></span> | <span data-ttu-id="2f84f-178">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="2f84f-178">default</span></span> |
| <span data-ttu-id="2f84f-179">intOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-179">intOutput</span></span> | <span data-ttu-id="2f84f-180">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-180">Int</span></span> | <span data-ttu-id="2f84f-181">1</span><span class="sxs-lookup"><span data-stu-id="2f84f-181">1</span></span> |
| <span data-ttu-id="2f84f-182">objectOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-182">objectOutput</span></span> | <span data-ttu-id="2f84f-183">Objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-183">Object</span></span> | <span data-ttu-id="2f84f-184">{"első": "alapértelmezett"}</span><span class="sxs-lookup"><span data-stu-id="2f84f-184">{"first": "default"}</span></span> |
| <span data-ttu-id="2f84f-185">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-185">arrayOutput</span></span> | <span data-ttu-id="2f84f-186">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-186">Array</span></span> | <span data-ttu-id="2f84f-187">[1]</span><span class="sxs-lookup"><span data-stu-id="2f84f-187">[1]</span></span> |
| <span data-ttu-id="2f84f-188">emptyOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-188">emptyOutput</span></span> | <span data-ttu-id="2f84f-189">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-189">Bool</span></span> | <span data-ttu-id="2f84f-190">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-190">True</span></span> |

<a id="concat" />

## <a name="concat"></a><span data-ttu-id="2f84f-191">Concat</span><span class="sxs-lookup"><span data-stu-id="2f84f-191">concat</span></span>
`concat(arg1, arg2, arg3, ...)`

<span data-ttu-id="2f84f-192">Több tömbök és összefűzendő hello tömb értéket ad vissza, vagy több karakterlánc-értékek egyesíti, és összefűzendő hello karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-192">Combines multiple arrays and returns hello concatenated array, or combines multiple string values and returns hello concatenated string.</span></span> 

### <a name="parameters"></a><span data-ttu-id="2f84f-193">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-193">Parameters</span></span>

| <span data-ttu-id="2f84f-194">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-194">Parameter</span></span> | <span data-ttu-id="2f84f-195">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-195">Required</span></span> | <span data-ttu-id="2f84f-196">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-196">Type</span></span> | <span data-ttu-id="2f84f-197">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-197">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-198">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-198">arg1</span></span> |<span data-ttu-id="2f84f-199">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-199">Yes</span></span> |<span data-ttu-id="2f84f-200">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-200">array or string</span></span> |<span data-ttu-id="2f84f-201">hello első tömb vagy kapott karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2f84f-201">hello first array or string for concatenation.</span></span> |
| <span data-ttu-id="2f84f-202">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="2f84f-202">additional arguments</span></span> |<span data-ttu-id="2f84f-203">Nem</span><span class="sxs-lookup"><span data-stu-id="2f84f-203">No</span></span> |<span data-ttu-id="2f84f-204">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-204">array or string</span></span> |<span data-ttu-id="2f84f-205">További tömbök vagy karakterláncok kapott a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="2f84f-205">Additional arrays or strings in sequential order for concatenation.</span></span> |

<span data-ttu-id="2f84f-206">Ez a funkció tetszőleges számú argumentumot is igénybe vehet, és fogadhat, karakterláncok vagy a tömbök hello paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="2f84f-206">This function can take any number of arguments, and can accept either strings or arrays for hello parameters.</span></span>

### <a name="return-value"></a><span data-ttu-id="2f84f-207">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-207">Return value</span></span>
<span data-ttu-id="2f84f-208">A karakterlánc vagy tömb összefűzött.</span><span class="sxs-lookup"><span data-stu-id="2f84f-208">A string or array of concatenated values.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-209">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-209">Example</span></span>

<span data-ttu-id="2f84f-210">hello a következő példa bemutatja, hogyan két toocombine tömbállandó.</span><span class="sxs-lookup"><span data-stu-id="2f84f-210">hello following example shows how toocombine two arrays.</span></span>

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

<span data-ttu-id="2f84f-211">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-211">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-212">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-212">Name</span></span> | <span data-ttu-id="2f84f-213">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-213">Type</span></span> | <span data-ttu-id="2f84f-214">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-214">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-215">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="2f84f-215">return</span></span> | <span data-ttu-id="2f84f-216">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-216">Array</span></span> | <span data-ttu-id="2f84f-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="2f84f-217">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<span data-ttu-id="2f84f-218">hello a következő példa bemutatja, hogyan toocombine két karakterlánc-értékeket, és térjen vissza olyan összefűzött karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2f84f-218">hello following example shows how toocombine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="2f84f-219">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-219">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-220">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-220">Name</span></span> | <span data-ttu-id="2f84f-221">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-221">Type</span></span> | <span data-ttu-id="2f84f-222">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-222">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-223">concatOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-223">concatOutput</span></span> | <span data-ttu-id="2f84f-224">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-224">String</span></span> | <span data-ttu-id="2f84f-225">előtag-5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="2f84f-225">prefix-5yj4yjf5mbg72</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="2f84f-226">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="2f84f-226">contains</span></span>
`contains(container, itemToFind)`

<span data-ttu-id="2f84f-227">Ellenőrzi, hogy egy tömb értéket tartalmaz, objektum kulcsot tartalmaz, vagy egy karakterláncot egy substring tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-227">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-228">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-228">Parameters</span></span>

| <span data-ttu-id="2f84f-229">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-229">Parameter</span></span> | <span data-ttu-id="2f84f-230">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-230">Required</span></span> | <span data-ttu-id="2f84f-231">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-231">Type</span></span> | <span data-ttu-id="2f84f-232">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-232">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-233">Tároló</span><span class="sxs-lookup"><span data-stu-id="2f84f-233">container</span></span> |<span data-ttu-id="2f84f-234">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-234">Yes</span></span> |<span data-ttu-id="2f84f-235">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-235">array, object, or string</span></span> |<span data-ttu-id="2f84f-236">hello érték, amely hello érték toofind tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-236">hello value that contains hello value toofind.</span></span> |
| <span data-ttu-id="2f84f-237">itemToFind</span><span class="sxs-lookup"><span data-stu-id="2f84f-237">itemToFind</span></span> |<span data-ttu-id="2f84f-238">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-238">Yes</span></span> |<span data-ttu-id="2f84f-239">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="2f84f-239">string or int</span></span> |<span data-ttu-id="2f84f-240">hello érték toofind.</span><span class="sxs-lookup"><span data-stu-id="2f84f-240">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-241">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-241">Return value</span></span>

<span data-ttu-id="2f84f-242">**Igaz** Ha hello elem található; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="2f84f-242">**True** if hello item is found; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-243">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-243">Example</span></span>

<span data-ttu-id="2f84f-244">hello következő példa bemutatja, hogyan toouse különböző típusú tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2f84f-244">hello following example shows how toouse contains with different types:</span></span>

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

<span data-ttu-id="2f84f-245">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-245">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-246">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-246">Name</span></span> | <span data-ttu-id="2f84f-247">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-247">Type</span></span> | <span data-ttu-id="2f84f-248">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-249">stringTrue</span><span class="sxs-lookup"><span data-stu-id="2f84f-249">stringTrue</span></span> | <span data-ttu-id="2f84f-250">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-250">Bool</span></span> | <span data-ttu-id="2f84f-251">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-251">True</span></span> |
| <span data-ttu-id="2f84f-252">stringFalse</span><span class="sxs-lookup"><span data-stu-id="2f84f-252">stringFalse</span></span> | <span data-ttu-id="2f84f-253">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-253">Bool</span></span> | <span data-ttu-id="2f84f-254">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2f84f-254">False</span></span> |
| <span data-ttu-id="2f84f-255">objectTrue</span><span class="sxs-lookup"><span data-stu-id="2f84f-255">objectTrue</span></span> | <span data-ttu-id="2f84f-256">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-256">Bool</span></span> | <span data-ttu-id="2f84f-257">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-257">True</span></span> |
| <span data-ttu-id="2f84f-258">objectFalse</span><span class="sxs-lookup"><span data-stu-id="2f84f-258">objectFalse</span></span> | <span data-ttu-id="2f84f-259">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-259">Bool</span></span> | <span data-ttu-id="2f84f-260">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2f84f-260">False</span></span> |
| <span data-ttu-id="2f84f-261">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="2f84f-261">arrayTrue</span></span> | <span data-ttu-id="2f84f-262">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-262">Bool</span></span> | <span data-ttu-id="2f84f-263">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-263">True</span></span> |
| <span data-ttu-id="2f84f-264">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="2f84f-264">arrayFalse</span></span> | <span data-ttu-id="2f84f-265">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-265">Bool</span></span> | <span data-ttu-id="2f84f-266">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2f84f-266">False</span></span> |

<a id="createarray" />

## <a name="createarray"></a><span data-ttu-id="2f84f-267">createarray</span><span class="sxs-lookup"><span data-stu-id="2f84f-267">createarray</span></span>
`createArray (arg1, arg2, arg3, ...)`

<span data-ttu-id="2f84f-268">Létrehoz egy tömb hello paraméterek.</span><span class="sxs-lookup"><span data-stu-id="2f84f-268">Creates an array from hello parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-269">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-269">Parameters</span></span>

| <span data-ttu-id="2f84f-270">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-270">Parameter</span></span> | <span data-ttu-id="2f84f-271">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-271">Required</span></span> | <span data-ttu-id="2f84f-272">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-272">Type</span></span> | <span data-ttu-id="2f84f-273">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-273">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-274">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-274">arg1</span></span> |<span data-ttu-id="2f84f-275">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-275">Yes</span></span> |<span data-ttu-id="2f84f-276">Karakterlánc, egész szám, a tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-276">String, Integer, Array, or Object</span></span> |<span data-ttu-id="2f84f-277">hello hello tömb első értékét.</span><span class="sxs-lookup"><span data-stu-id="2f84f-277">hello first value in hello array.</span></span> |
| <span data-ttu-id="2f84f-278">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="2f84f-278">additional arguments</span></span> |<span data-ttu-id="2f84f-279">Nem</span><span class="sxs-lookup"><span data-stu-id="2f84f-279">No</span></span> |<span data-ttu-id="2f84f-280">Karakterlánc, egész szám, a tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-280">String, Integer, Array, or Object</span></span> |<span data-ttu-id="2f84f-281">További értékeket hello tömbben.</span><span class="sxs-lookup"><span data-stu-id="2f84f-281">Additional values in hello array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-282">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-282">Return value</span></span>

<span data-ttu-id="2f84f-283">Egy tömb.</span><span class="sxs-lookup"><span data-stu-id="2f84f-283">An array.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-284">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-284">Example</span></span>

<span data-ttu-id="2f84f-285">a következő példa azt mutatja meg hogyan hello toouse createArray különböző típusú:</span><span class="sxs-lookup"><span data-stu-id="2f84f-285">hello following example shows how toouse createArray with different types:</span></span>

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

<span data-ttu-id="2f84f-286">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-286">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-287">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-287">Name</span></span> | <span data-ttu-id="2f84f-288">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-288">Type</span></span> | <span data-ttu-id="2f84f-289">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-289">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-290">stringArray</span><span class="sxs-lookup"><span data-stu-id="2f84f-290">stringArray</span></span> | <span data-ttu-id="2f84f-291">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-291">Array</span></span> | <span data-ttu-id="2f84f-292">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="2f84f-292">["a", "b", "c"]</span></span> |
| <span data-ttu-id="2f84f-293">intArray</span><span class="sxs-lookup"><span data-stu-id="2f84f-293">intArray</span></span> | <span data-ttu-id="2f84f-294">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-294">Array</span></span> | <span data-ttu-id="2f84f-295">[1, 2, 3]</span><span class="sxs-lookup"><span data-stu-id="2f84f-295">[1, 2, 3]</span></span> |
| <span data-ttu-id="2f84f-296">objectArray</span><span class="sxs-lookup"><span data-stu-id="2f84f-296">objectArray</span></span> | <span data-ttu-id="2f84f-297">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-297">Array</span></span> | <span data-ttu-id="2f84f-298">[{"egy": "a", "2": "b", "három": "c"}]</span><span class="sxs-lookup"><span data-stu-id="2f84f-298">[{"one": "a", "two": "b", "three": "c"}]</span></span> |
| <span data-ttu-id="2f84f-299">arrayArray</span><span class="sxs-lookup"><span data-stu-id="2f84f-299">arrayArray</span></span> | <span data-ttu-id="2f84f-300">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-300">Array</span></span> | <span data-ttu-id="2f84f-301">[["egy", "két", "három"]]</span><span class="sxs-lookup"><span data-stu-id="2f84f-301">[["one", "two", "three"]]</span></span> |

<a id="empty" />

## <a name="empty"></a><span data-ttu-id="2f84f-302">üres</span><span class="sxs-lookup"><span data-stu-id="2f84f-302">empty</span></span>

`empty(itemToTest)`

<span data-ttu-id="2f84f-303">Meghatározza, hogy egy tömb, az objektum, vagy a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="2f84f-303">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-304">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-304">Parameters</span></span>

| <span data-ttu-id="2f84f-305">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-305">Parameter</span></span> | <span data-ttu-id="2f84f-306">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-306">Required</span></span> | <span data-ttu-id="2f84f-307">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-307">Type</span></span> | <span data-ttu-id="2f84f-308">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-308">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-309">itemToTest</span><span class="sxs-lookup"><span data-stu-id="2f84f-309">itemToTest</span></span> |<span data-ttu-id="2f84f-310">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-310">Yes</span></span> |<span data-ttu-id="2f84f-311">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-311">array, object, or string</span></span> |<span data-ttu-id="2f84f-312">hello érték toocheck, ha üres.</span><span class="sxs-lookup"><span data-stu-id="2f84f-312">hello value toocheck if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-313">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-313">Return value</span></span>

<span data-ttu-id="2f84f-314">Beolvasása **igaz** hello értéke üres, ha sikertelen, ha **hamis**.</span><span class="sxs-lookup"><span data-stu-id="2f84f-314">Returns **True** if hello value is empty; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-315">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-315">Example</span></span>

<span data-ttu-id="2f84f-316">a következő példa hello ellenőrzi, hogy egy tömb, az objektumot, és a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="2f84f-316">hello following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="2f84f-317">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-317">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-318">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-318">Name</span></span> | <span data-ttu-id="2f84f-319">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-319">Type</span></span> | <span data-ttu-id="2f84f-320">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-320">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-321">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="2f84f-321">arrayEmpty</span></span> | <span data-ttu-id="2f84f-322">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-322">Bool</span></span> | <span data-ttu-id="2f84f-323">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-323">True</span></span> |
| <span data-ttu-id="2f84f-324">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="2f84f-324">objectEmpty</span></span> | <span data-ttu-id="2f84f-325">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-325">Bool</span></span> | <span data-ttu-id="2f84f-326">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-326">True</span></span> |
| <span data-ttu-id="2f84f-327">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="2f84f-327">stringEmpty</span></span> | <span data-ttu-id="2f84f-328">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-328">Bool</span></span> | <span data-ttu-id="2f84f-329">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-329">True</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="2f84f-330">első</span><span class="sxs-lookup"><span data-stu-id="2f84f-330">first</span></span>
`first(arg1)`

<span data-ttu-id="2f84f-331">Beolvasása hello hello tömb első eleme, vagy hello karakterlánc első karaktere.</span><span class="sxs-lookup"><span data-stu-id="2f84f-331">Returns hello first element of hello array, or first character of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-332">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-332">Parameters</span></span>

| <span data-ttu-id="2f84f-333">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-333">Parameter</span></span> | <span data-ttu-id="2f84f-334">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-334">Required</span></span> | <span data-ttu-id="2f84f-335">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-335">Type</span></span> | <span data-ttu-id="2f84f-336">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-336">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-337">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-337">arg1</span></span> |<span data-ttu-id="2f84f-338">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-338">Yes</span></span> |<span data-ttu-id="2f84f-339">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-339">array or string</span></span> |<span data-ttu-id="2f84f-340">hello érték tooretrieve hello első elem vagy karakter.</span><span class="sxs-lookup"><span data-stu-id="2f84f-340">hello value tooretrieve hello first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-341">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-341">Return value</span></span>

<span data-ttu-id="2f84f-342">egy tömb első eleme hello (karakterlánc, int, tömb vagy objektum) típusú hello, vagy egy karakterlánc első karaktere hello.</span><span class="sxs-lookup"><span data-stu-id="2f84f-342">hello type (string, int, array, or object) of hello first element in an array, or hello first character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-343">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-343">Example</span></span>

<span data-ttu-id="2f84f-344">hello következő példa bemutatja, hogyan toouse hello tömb és karakterlánc az első függvényét.</span><span class="sxs-lookup"><span data-stu-id="2f84f-344">hello following example shows how toouse hello first function with an array and string.</span></span>

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

<span data-ttu-id="2f84f-345">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-345">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-346">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-346">Name</span></span> | <span data-ttu-id="2f84f-347">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-347">Type</span></span> | <span data-ttu-id="2f84f-348">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-348">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-349">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-349">arrayOutput</span></span> | <span data-ttu-id="2f84f-350">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-350">String</span></span> | <span data-ttu-id="2f84f-351">egy</span><span class="sxs-lookup"><span data-stu-id="2f84f-351">one</span></span> |
| <span data-ttu-id="2f84f-352">stringOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-352">stringOutput</span></span> | <span data-ttu-id="2f84f-353">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-353">String</span></span> | <span data-ttu-id="2f84f-354">O</span><span class="sxs-lookup"><span data-stu-id="2f84f-354">O</span></span> |

<a id="intersection" />

## <a name="intersection"></a><span data-ttu-id="2f84f-355">metszetének</span><span class="sxs-lookup"><span data-stu-id="2f84f-355">intersection</span></span>
`intersection(arg1, arg2, arg3, ...)`

<span data-ttu-id="2f84f-356">Hello paraméterek egy egyetlen tömb vagy objektum hello szokványos elemeket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-356">Returns a single array or object with hello common elements from hello parameters.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-357">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-357">Parameters</span></span>

| <span data-ttu-id="2f84f-358">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-358">Parameter</span></span> | <span data-ttu-id="2f84f-359">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-359">Required</span></span> | <span data-ttu-id="2f84f-360">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-360">Type</span></span> | <span data-ttu-id="2f84f-361">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-361">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-362">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-362">arg1</span></span> |<span data-ttu-id="2f84f-363">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-363">Yes</span></span> |<span data-ttu-id="2f84f-364">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-364">array or object</span></span> |<span data-ttu-id="2f84f-365">hello első érték toouse közös elemek kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="2f84f-365">hello first value toouse for finding common elements.</span></span> |
| <span data-ttu-id="2f84f-366">Arg2</span><span class="sxs-lookup"><span data-stu-id="2f84f-366">arg2</span></span> |<span data-ttu-id="2f84f-367">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-367">Yes</span></span> |<span data-ttu-id="2f84f-368">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-368">array or object</span></span> |<span data-ttu-id="2f84f-369">hello második érték toouse közös elemek kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="2f84f-369">hello second value toouse for finding common elements.</span></span> |
| <span data-ttu-id="2f84f-370">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="2f84f-370">additional arguments</span></span> |<span data-ttu-id="2f84f-371">Nem</span><span class="sxs-lookup"><span data-stu-id="2f84f-371">No</span></span> |<span data-ttu-id="2f84f-372">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-372">array or object</span></span> |<span data-ttu-id="2f84f-373">További értékeket toouse közös elemek kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="2f84f-373">Additional values toouse for finding common elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-374">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-374">Return value</span></span>

<span data-ttu-id="2f84f-375">Tömb vagy objektum hello szokványos elemeket.</span><span class="sxs-lookup"><span data-stu-id="2f84f-375">An array or object with hello common elements.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-376">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-376">Example</span></span>

<span data-ttu-id="2f84f-377">a következő példa azt mutatja meg, hogyan toouse metszetének rendelkező tömbállandó hello és objektumok:</span><span class="sxs-lookup"><span data-stu-id="2f84f-377">hello following example shows how toouse intersection with arrays and objects:</span></span>

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

<span data-ttu-id="2f84f-378">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-378">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-379">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-379">Name</span></span> | <span data-ttu-id="2f84f-380">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-380">Type</span></span> | <span data-ttu-id="2f84f-381">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-381">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-382">objectOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-382">objectOutput</span></span> | <span data-ttu-id="2f84f-383">Objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-383">Object</span></span> | <span data-ttu-id="2f84f-384">{"egy": "a", "három": "c"}</span><span class="sxs-lookup"><span data-stu-id="2f84f-384">{"one": "a", "three": "c"}</span></span> |
| <span data-ttu-id="2f84f-385">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-385">arrayOutput</span></span> | <span data-ttu-id="2f84f-386">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-386">Array</span></span> | <span data-ttu-id="2f84f-387">["két", "három"]</span><span class="sxs-lookup"><span data-stu-id="2f84f-387">["two", "three"]</span></span> |


## <a name="json"></a><span data-ttu-id="2f84f-388">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="2f84f-388">json</span></span>
`json(arg1)`

<span data-ttu-id="2f84f-389">A JSON-objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-389">Returns a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-390">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-390">Parameters</span></span>

| <span data-ttu-id="2f84f-391">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-391">Parameter</span></span> | <span data-ttu-id="2f84f-392">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-392">Required</span></span> | <span data-ttu-id="2f84f-393">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-393">Type</span></span> | <span data-ttu-id="2f84f-394">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-394">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-395">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-395">arg1</span></span> |<span data-ttu-id="2f84f-396">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-396">Yes</span></span> |<span data-ttu-id="2f84f-397">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-397">string</span></span> |<span data-ttu-id="2f84f-398">hello érték tooconvert tooJSON.</span><span class="sxs-lookup"><span data-stu-id="2f84f-398">hello value tooconvert tooJSON.</span></span> |


### <a name="return-value"></a><span data-ttu-id="2f84f-399">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-399">Return value</span></span>

<span data-ttu-id="2f84f-400">hello JSON-objektumból származó hello megadott karakterlánc vagy üres objektum, amikor **null** van megadva.</span><span class="sxs-lookup"><span data-stu-id="2f84f-400">hello JSON object from hello specified string, or an empty object when **null** is specified.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-401">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-401">Example</span></span>

<span data-ttu-id="2f84f-402">a következő példa azt mutatja meg, hogyan toouse metszetének rendelkező tömbállandó hello és objektumok:</span><span class="sxs-lookup"><span data-stu-id="2f84f-402">hello following example shows how toouse intersection with arrays and objects:</span></span>

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

<span data-ttu-id="2f84f-403">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-403">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-404">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-404">Name</span></span> | <span data-ttu-id="2f84f-405">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-405">Type</span></span> | <span data-ttu-id="2f84f-406">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-406">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-407">jsonOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-407">jsonOutput</span></span> | <span data-ttu-id="2f84f-408">Objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-408">Object</span></span> | <span data-ttu-id="2f84f-409">{"a": "b"}</span><span class="sxs-lookup"><span data-stu-id="2f84f-409">{"a": "b"}</span></span> |
| <span data-ttu-id="2f84f-410">nullOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-410">nullOutput</span></span> | <span data-ttu-id="2f84f-411">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-411">Boolean</span></span> | <span data-ttu-id="2f84f-412">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2f84f-412">True</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="2f84f-413">utolsó</span><span class="sxs-lookup"><span data-stu-id="2f84f-413">last</span></span>
`last (arg1)`

<span data-ttu-id="2f84f-414">Beolvasása hello hello tömb utolsó eleme, vagy hello karakterlánc utolsó karaktere.</span><span class="sxs-lookup"><span data-stu-id="2f84f-414">Returns hello last element of hello array, or last character of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-415">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-415">Parameters</span></span>

| <span data-ttu-id="2f84f-416">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-416">Parameter</span></span> | <span data-ttu-id="2f84f-417">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-417">Required</span></span> | <span data-ttu-id="2f84f-418">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-418">Type</span></span> | <span data-ttu-id="2f84f-419">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-420">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-420">arg1</span></span> |<span data-ttu-id="2f84f-421">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-421">Yes</span></span> |<span data-ttu-id="2f84f-422">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-422">array or string</span></span> |<span data-ttu-id="2f84f-423">hello érték tooretrieve hello utolsó eleme vagy karakter.</span><span class="sxs-lookup"><span data-stu-id="2f84f-423">hello value tooretrieve hello last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-424">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-424">Return value</span></span>

<span data-ttu-id="2f84f-425">hello típusa (karakterlánc, int, tömb vagy objektum) utolsó eleme hello tömb vagy karakterlánc hello utolsó karaktere.</span><span class="sxs-lookup"><span data-stu-id="2f84f-425">hello type (string, int, array, or object) of hello last element in an array, or hello last character of a string.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-426">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-426">Example</span></span>

<span data-ttu-id="2f84f-427">hello következő példa bemutatja, hogyan toouse hello utolsó függvény egy tömböt és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2f84f-427">hello following example shows how toouse hello last function with an array and string.</span></span>

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

<span data-ttu-id="2f84f-428">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-428">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-429">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-429">Name</span></span> | <span data-ttu-id="2f84f-430">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-430">Type</span></span> | <span data-ttu-id="2f84f-431">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-432">arrayOutput</span></span> | <span data-ttu-id="2f84f-433">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-433">String</span></span> | <span data-ttu-id="2f84f-434">három</span><span class="sxs-lookup"><span data-stu-id="2f84f-434">three</span></span> |
| <span data-ttu-id="2f84f-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-435">stringOutput</span></span> | <span data-ttu-id="2f84f-436">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-436">String</span></span> | <span data-ttu-id="2f84f-437">E</span><span class="sxs-lookup"><span data-stu-id="2f84f-437">e</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="2f84f-438">Hossza</span><span class="sxs-lookup"><span data-stu-id="2f84f-438">length</span></span>
`length(arg1)`

<span data-ttu-id="2f84f-439">A tömb, vagy egy karakterlánc karaktereinek hello számú elemet ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-439">Returns hello number of elements in an array, or characters in a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-440">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-440">Parameters</span></span>

| <span data-ttu-id="2f84f-441">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-441">Parameter</span></span> | <span data-ttu-id="2f84f-442">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-442">Required</span></span> | <span data-ttu-id="2f84f-443">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-443">Type</span></span> | <span data-ttu-id="2f84f-444">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-444">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-445">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-445">arg1</span></span> |<span data-ttu-id="2f84f-446">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-446">Yes</span></span> |<span data-ttu-id="2f84f-447">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-447">array or string</span></span> |<span data-ttu-id="2f84f-448">első hello elemek száma a tömb toouse hello, vagy hello karakterlánc toouse kapcsolódnak a hello karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="2f84f-448">hello array toouse for getting hello number of elements, or hello string toouse for getting hello number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-449">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-449">Return value</span></span>

<span data-ttu-id="2f84f-450">Egy egész szám.</span><span class="sxs-lookup"><span data-stu-id="2f84f-450">An int.</span></span> 

### <a name="example"></a><span data-ttu-id="2f84f-451">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-451">Example</span></span>

<span data-ttu-id="2f84f-452">a következő példa azt mutatja meg hogyan hello toouse hosszúságú tömb és karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="2f84f-452">hello following example shows how toouse length with an array and string:</span></span>

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

<span data-ttu-id="2f84f-453">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-453">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-454">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-454">Name</span></span> | <span data-ttu-id="2f84f-455">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-455">Type</span></span> | <span data-ttu-id="2f84f-456">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-456">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-457">arrayLength</span><span class="sxs-lookup"><span data-stu-id="2f84f-457">arrayLength</span></span> | <span data-ttu-id="2f84f-458">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-458">Int</span></span> | <span data-ttu-id="2f84f-459">3</span><span class="sxs-lookup"><span data-stu-id="2f84f-459">3</span></span> |
| <span data-ttu-id="2f84f-460">stringLength</span><span class="sxs-lookup"><span data-stu-id="2f84f-460">stringLength</span></span> | <span data-ttu-id="2f84f-461">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-461">Int</span></span> | <span data-ttu-id="2f84f-462">13</span><span class="sxs-lookup"><span data-stu-id="2f84f-462">13</span></span> |

<span data-ttu-id="2f84f-463">E funkció használata egy tömb-toospecify hello az ismétlések száma az erőforrások létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2f84f-463">You can use this function with an array toospecify hello number of iterations when creating resources.</span></span> <span data-ttu-id="2f84f-464">A következő példa hello, hello paraméter **siteNames** nevek toouse tooan tömbjének kellene tekintse meg a hello webhelyek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="2f84f-464">In hello following example, hello parameter **siteNames** would refer tooan array of names toouse when creating hello web sites.</span></span>

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

<span data-ttu-id="2f84f-465">Ez a függvény egy tömb használatával kapcsolatban további információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="2f84f-465">For more information about using this function with an array, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<a id="min" />

## <a name="min"></a><span data-ttu-id="2f84f-466">perc</span><span class="sxs-lookup"><span data-stu-id="2f84f-466">min</span></span>
`min(arg1)`

<span data-ttu-id="2f84f-467">Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját az minimális értékét.</span><span class="sxs-lookup"><span data-stu-id="2f84f-467">Returns hello minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-468">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-468">Parameters</span></span>

| <span data-ttu-id="2f84f-469">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-469">Parameter</span></span> | <span data-ttu-id="2f84f-470">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-470">Required</span></span> | <span data-ttu-id="2f84f-471">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-471">Type</span></span> | <span data-ttu-id="2f84f-472">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-472">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-473">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-473">arg1</span></span> |<span data-ttu-id="2f84f-474">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-474">Yes</span></span> |<span data-ttu-id="2f84f-475">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="2f84f-475">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="2f84f-476">hello gyűjtemény tooget hello minimális érték.</span><span class="sxs-lookup"><span data-stu-id="2f84f-476">hello collection tooget hello minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-477">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-477">Return value</span></span>

<span data-ttu-id="2f84f-478">Az int hello minimális értékét képviselő.</span><span class="sxs-lookup"><span data-stu-id="2f84f-478">An int representing hello minimum value.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-479">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-479">Example</span></span>

<span data-ttu-id="2f84f-480">a következő példa azt mutatja meg hogyan hello toouse min tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="2f84f-480">hello following example shows how toouse min with an array and a list of integers:</span></span>

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

<span data-ttu-id="2f84f-481">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-481">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-482">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-482">Name</span></span> | <span data-ttu-id="2f84f-483">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-483">Type</span></span> | <span data-ttu-id="2f84f-484">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-484">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-485">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-485">arrayOutput</span></span> | <span data-ttu-id="2f84f-486">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-486">Int</span></span> | <span data-ttu-id="2f84f-487">0</span><span class="sxs-lookup"><span data-stu-id="2f84f-487">0</span></span> |
| <span data-ttu-id="2f84f-488">intOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-488">intOutput</span></span> | <span data-ttu-id="2f84f-489">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-489">Int</span></span> | <span data-ttu-id="2f84f-490">0</span><span class="sxs-lookup"><span data-stu-id="2f84f-490">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="2f84f-491">maximális</span><span class="sxs-lookup"><span data-stu-id="2f84f-491">max</span></span>
`max(arg1)`

<span data-ttu-id="2f84f-492">Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját maximális értéket.</span><span class="sxs-lookup"><span data-stu-id="2f84f-492">Returns hello maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-493">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-493">Parameters</span></span>

| <span data-ttu-id="2f84f-494">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-494">Parameter</span></span> | <span data-ttu-id="2f84f-495">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-495">Required</span></span> | <span data-ttu-id="2f84f-496">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-496">Type</span></span> | <span data-ttu-id="2f84f-497">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-497">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-498">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-498">arg1</span></span> |<span data-ttu-id="2f84f-499">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-499">Yes</span></span> |<span data-ttu-id="2f84f-500">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="2f84f-500">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="2f84f-501">hello gyűjtemény tooget hello maximális értéket.</span><span class="sxs-lookup"><span data-stu-id="2f84f-501">hello collection tooget hello maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-502">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-502">Return value</span></span>

<span data-ttu-id="2f84f-503">Az int hello maximális értékét képviselő.</span><span class="sxs-lookup"><span data-stu-id="2f84f-503">An int representing hello maximum value.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-504">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-504">Example</span></span>

<span data-ttu-id="2f84f-505">a következő példa azt mutatja meg hogyan hello toouse maximális tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="2f84f-505">hello following example shows how toouse max with an array and a list of integers:</span></span>

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

<span data-ttu-id="2f84f-506">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-506">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-507">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-507">Name</span></span> | <span data-ttu-id="2f84f-508">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-508">Type</span></span> | <span data-ttu-id="2f84f-509">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-509">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-510">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-510">arrayOutput</span></span> | <span data-ttu-id="2f84f-511">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-511">Int</span></span> | <span data-ttu-id="2f84f-512">5</span><span class="sxs-lookup"><span data-stu-id="2f84f-512">5</span></span> |
| <span data-ttu-id="2f84f-513">intOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-513">intOutput</span></span> | <span data-ttu-id="2f84f-514">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-514">Int</span></span> | <span data-ttu-id="2f84f-515">5</span><span class="sxs-lookup"><span data-stu-id="2f84f-515">5</span></span> |

<a id="range" />

## <a name="range"></a><span data-ttu-id="2f84f-516">tartomány</span><span class="sxs-lookup"><span data-stu-id="2f84f-516">range</span></span>
`range(startingInteger, numberOfElements)`

<span data-ttu-id="2f84f-517">Egy számokból álló tömb egy egész kezdő- és néhány elemet tartalmazó jön létre.</span><span class="sxs-lookup"><span data-stu-id="2f84f-517">Creates an array of integers from a starting integer and containing a number of items.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-518">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-518">Parameters</span></span>

| <span data-ttu-id="2f84f-519">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-519">Parameter</span></span> | <span data-ttu-id="2f84f-520">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-520">Required</span></span> | <span data-ttu-id="2f84f-521">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-521">Type</span></span> | <span data-ttu-id="2f84f-522">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-522">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-523">startingInteger</span><span class="sxs-lookup"><span data-stu-id="2f84f-523">startingInteger</span></span> |<span data-ttu-id="2f84f-524">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-524">Yes</span></span> |<span data-ttu-id="2f84f-525">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-525">int</span></span> |<span data-ttu-id="2f84f-526">hello első egész hello tömbben.</span><span class="sxs-lookup"><span data-stu-id="2f84f-526">hello first integer in hello array.</span></span> |
| <span data-ttu-id="2f84f-527">numberofElements</span><span class="sxs-lookup"><span data-stu-id="2f84f-527">numberofElements</span></span> |<span data-ttu-id="2f84f-528">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-528">Yes</span></span> |<span data-ttu-id="2f84f-529">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-529">int</span></span> |<span data-ttu-id="2f84f-530">egész számok hello tömb hello száma.</span><span class="sxs-lookup"><span data-stu-id="2f84f-530">hello number of integers in hello array.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-531">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-531">Return value</span></span>

<span data-ttu-id="2f84f-532">Egy számokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="2f84f-532">An array of integers.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-533">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-533">Example</span></span>

<span data-ttu-id="2f84f-534">hello a következő példa bemutatja, hogyan toouse hello tartomány függvényben:</span><span class="sxs-lookup"><span data-stu-id="2f84f-534">hello following example shows how toouse hello range function:</span></span>

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

<span data-ttu-id="2f84f-535">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-535">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-536">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-536">Name</span></span> | <span data-ttu-id="2f84f-537">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-537">Type</span></span> | <span data-ttu-id="2f84f-538">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-538">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-539">rangeOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-539">rangeOutput</span></span> | <span data-ttu-id="2f84f-540">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-540">Array</span></span> | <span data-ttu-id="2f84f-541">[5, 6, 7]</span><span class="sxs-lookup"><span data-stu-id="2f84f-541">[5, 6, 7]</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="2f84f-542">Kihagyása</span><span class="sxs-lookup"><span data-stu-id="2f84f-542">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="2f84f-543">Az összes hello elem tömböt ad vissza, miután hello hello tömb a megadott szám, vagy minden hello karakterekkel karakterláncot ad vissza, miután hello hello karakterláncban megadott szám.</span><span class="sxs-lookup"><span data-stu-id="2f84f-543">Returns an array with all hello elements after hello specified number in hello array, or returns a string with all hello characters after hello specified number in hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-544">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-544">Parameters</span></span>

| <span data-ttu-id="2f84f-545">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-545">Parameter</span></span> | <span data-ttu-id="2f84f-546">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-546">Required</span></span> | <span data-ttu-id="2f84f-547">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-547">Type</span></span> | <span data-ttu-id="2f84f-548">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-548">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-549">originalValue</span><span class="sxs-lookup"><span data-stu-id="2f84f-549">originalValue</span></span> |<span data-ttu-id="2f84f-550">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-550">Yes</span></span> |<span data-ttu-id="2f84f-551">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-551">array or string</span></span> |<span data-ttu-id="2f84f-552">hello tömb vagy karakterlánc toouse átugrásához.</span><span class="sxs-lookup"><span data-stu-id="2f84f-552">hello array or string toouse for skipping.</span></span> |
| <span data-ttu-id="2f84f-553">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="2f84f-553">numberToSkip</span></span> |<span data-ttu-id="2f84f-554">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-554">Yes</span></span> |<span data-ttu-id="2f84f-555">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-555">int</span></span> |<span data-ttu-id="2f84f-556">elemek vagy karaktereket tooskip hello száma.</span><span class="sxs-lookup"><span data-stu-id="2f84f-556">hello number of elements or characters tooskip.</span></span> <span data-ttu-id="2f84f-557">Ha ez az érték 0 vagy kisebb, az összes elem hello, vagy karakter hello értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-557">If this value is 0 or less, all hello elements or characters in hello value are returned.</span></span> <span data-ttu-id="2f84f-558">Ha hello hello tömb vagy karakterlánc hossza nagyobb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-558">If it is larger than hello length of hello array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-559">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-559">Return value</span></span>

<span data-ttu-id="2f84f-560">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2f84f-560">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-561">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-561">Example</span></span>

<span data-ttu-id="2f84f-562">a következő példa átugrása hello hello hello tömb megadott számú elemet, és hello egy karakterláncban megadott számú karaktert.</span><span class="sxs-lookup"><span data-stu-id="2f84f-562">hello following example skips hello specified number of elements in hello array, and hello specified number of characters in a string.</span></span>

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

<span data-ttu-id="2f84f-563">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-563">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-564">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-564">Name</span></span> | <span data-ttu-id="2f84f-565">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-565">Type</span></span> | <span data-ttu-id="2f84f-566">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-566">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-567">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-567">arrayOutput</span></span> | <span data-ttu-id="2f84f-568">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-568">Array</span></span> | <span data-ttu-id="2f84f-569">["három"]</span><span class="sxs-lookup"><span data-stu-id="2f84f-569">["three"]</span></span> |
| <span data-ttu-id="2f84f-570">stringOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-570">stringOutput</span></span> | <span data-ttu-id="2f84f-571">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-571">String</span></span> | <span data-ttu-id="2f84f-572">két három</span><span class="sxs-lookup"><span data-stu-id="2f84f-572">two three</span></span> |

<a id="take" />

## <a name="take"></a><span data-ttu-id="2f84f-573">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="2f84f-573">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="2f84f-574">Hello elemből álló tömböt megadott elemek számát adja vissza hello start hello tömb, vagy hello egy karakterlánc megadott számú hello karakterlánc kezdetét hello karaktert tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2f84f-574">Returns an array with hello specified number of elements from hello start of hello array, or a string with hello specified number of characters from hello start of hello string.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-575">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-575">Parameters</span></span>

| <span data-ttu-id="2f84f-576">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-576">Parameter</span></span> | <span data-ttu-id="2f84f-577">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-577">Required</span></span> | <span data-ttu-id="2f84f-578">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-578">Type</span></span> | <span data-ttu-id="2f84f-579">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-579">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-580">originalValue</span><span class="sxs-lookup"><span data-stu-id="2f84f-580">originalValue</span></span> |<span data-ttu-id="2f84f-581">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-581">Yes</span></span> |<span data-ttu-id="2f84f-582">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-582">array or string</span></span> |<span data-ttu-id="2f84f-583">hello tömb vagy karakterlánc tootake hello elemeit.</span><span class="sxs-lookup"><span data-stu-id="2f84f-583">hello array or string tootake hello elements from.</span></span> |
| <span data-ttu-id="2f84f-584">numberToTake</span><span class="sxs-lookup"><span data-stu-id="2f84f-584">numberToTake</span></span> |<span data-ttu-id="2f84f-585">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-585">Yes</span></span> |<span data-ttu-id="2f84f-586">int</span><span class="sxs-lookup"><span data-stu-id="2f84f-586">int</span></span> |<span data-ttu-id="2f84f-587">elemek vagy karaktereket tootake hello száma.</span><span class="sxs-lookup"><span data-stu-id="2f84f-587">hello number of elements or characters tootake.</span></span> <span data-ttu-id="2f84f-588">Ha ez az érték 0 vagy kisebb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-588">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="2f84f-589">Ha nagyobb, mint a megadott tömb vagy karakterlánc hello hello hosszát, visszaadja a hello tömb vagy karakterlánc összes hello eleme.</span><span class="sxs-lookup"><span data-stu-id="2f84f-589">If it is larger than hello length of hello given array or string, all hello elements in hello array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-590">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-590">Return value</span></span>

<span data-ttu-id="2f84f-591">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2f84f-591">An array or string.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-592">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-592">Example</span></span>

<span data-ttu-id="2f84f-593">a következő példa vesz hello hello megadott hello tömb elemei, és egy karakterláncból karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="2f84f-593">hello following example takes hello specified number of elements from hello array, and characters from a string.</span></span>

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

<span data-ttu-id="2f84f-594">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-594">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-595">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-595">Name</span></span> | <span data-ttu-id="2f84f-596">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-596">Type</span></span> | <span data-ttu-id="2f84f-597">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-597">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-598">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-598">arrayOutput</span></span> | <span data-ttu-id="2f84f-599">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-599">Array</span></span> | <span data-ttu-id="2f84f-600">["egy", "két"]</span><span class="sxs-lookup"><span data-stu-id="2f84f-600">["one", "two"]</span></span> |
| <span data-ttu-id="2f84f-601">stringOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-601">stringOutput</span></span> | <span data-ttu-id="2f84f-602">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2f84f-602">String</span></span> | <span data-ttu-id="2f84f-603">a</span><span class="sxs-lookup"><span data-stu-id="2f84f-603">on</span></span> |

<a id="union" />

## <a name="union"></a><span data-ttu-id="2f84f-604">a UNION</span><span class="sxs-lookup"><span data-stu-id="2f84f-604">union</span></span>
`union(arg1, arg2, arg3, ...)`

<span data-ttu-id="2f84f-605">Egy egyetlen tömb vagy objektum az összes elem hello paraméterek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-605">Returns a single array or object with all elements from hello parameters.</span></span> <span data-ttu-id="2f84f-606">Ismétlődő vagy kulcsok vannak csak egyszer tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2f84f-606">Duplicate values or keys are only included once.</span></span>

### <a name="parameters"></a><span data-ttu-id="2f84f-607">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2f84f-607">Parameters</span></span>

| <span data-ttu-id="2f84f-608">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2f84f-608">Parameter</span></span> | <span data-ttu-id="2f84f-609">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2f84f-609">Required</span></span> | <span data-ttu-id="2f84f-610">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-610">Type</span></span> | <span data-ttu-id="2f84f-611">Leírás</span><span class="sxs-lookup"><span data-stu-id="2f84f-611">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2f84f-612">arg1</span><span class="sxs-lookup"><span data-stu-id="2f84f-612">arg1</span></span> |<span data-ttu-id="2f84f-613">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-613">Yes</span></span> |<span data-ttu-id="2f84f-614">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-614">array or object</span></span> |<span data-ttu-id="2f84f-615">hello első érték toouse elemek való csatlakozásra.</span><span class="sxs-lookup"><span data-stu-id="2f84f-615">hello first value toouse for joining elements.</span></span> |
| <span data-ttu-id="2f84f-616">Arg2</span><span class="sxs-lookup"><span data-stu-id="2f84f-616">arg2</span></span> |<span data-ttu-id="2f84f-617">Igen</span><span class="sxs-lookup"><span data-stu-id="2f84f-617">Yes</span></span> |<span data-ttu-id="2f84f-618">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-618">array or object</span></span> |<span data-ttu-id="2f84f-619">hello második érték toouse elemek való csatlakozásra.</span><span class="sxs-lookup"><span data-stu-id="2f84f-619">hello second value toouse for joining elements.</span></span> |
| <span data-ttu-id="2f84f-620">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="2f84f-620">additional arguments</span></span> |<span data-ttu-id="2f84f-621">Nem</span><span class="sxs-lookup"><span data-stu-id="2f84f-621">No</span></span> |<span data-ttu-id="2f84f-622">tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-622">array or object</span></span> |<span data-ttu-id="2f84f-623">További értékeket toouse elemek való csatlakozásra.</span><span class="sxs-lookup"><span data-stu-id="2f84f-623">Additional values toouse for joining elements.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2f84f-624">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-624">Return value</span></span>

<span data-ttu-id="2f84f-625">Tömb vagy objektum.</span><span class="sxs-lookup"><span data-stu-id="2f84f-625">An array or object.</span></span>

### <a name="example"></a><span data-ttu-id="2f84f-626">Példa</span><span class="sxs-lookup"><span data-stu-id="2f84f-626">Example</span></span>

<span data-ttu-id="2f84f-627">a következő példa azt mutatja meg, hogyan toouse Unió tömbállandó hello és objektumok:</span><span class="sxs-lookup"><span data-stu-id="2f84f-627">hello following example shows how toouse union with arrays and objects:</span></span>

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

<span data-ttu-id="2f84f-628">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2f84f-628">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2f84f-629">Név</span><span class="sxs-lookup"><span data-stu-id="2f84f-629">Name</span></span> | <span data-ttu-id="2f84f-630">Típus</span><span class="sxs-lookup"><span data-stu-id="2f84f-630">Type</span></span> | <span data-ttu-id="2f84f-631">Érték</span><span class="sxs-lookup"><span data-stu-id="2f84f-631">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2f84f-632">objectOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-632">objectOutput</span></span> | <span data-ttu-id="2f84f-633">Objektum</span><span class="sxs-lookup"><span data-stu-id="2f84f-633">Object</span></span> | <span data-ttu-id="2f84f-634">{"egy": "a", "2": "b", "három": "c", "négy": "d", "5": "e"}</span><span class="sxs-lookup"><span data-stu-id="2f84f-634">{"one": "a", "two": "b", "three": "c", "four": "d", "five": "e"}</span></span> |
| <span data-ttu-id="2f84f-635">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="2f84f-635">arrayOutput</span></span> | <span data-ttu-id="2f84f-636">Tömb</span><span class="sxs-lookup"><span data-stu-id="2f84f-636">Array</span></span> | <span data-ttu-id="2f84f-637">["egy", "két", "három", "négy"]</span><span class="sxs-lookup"><span data-stu-id="2f84f-637">["one", "two", "three", "four"]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2f84f-638">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f84f-638">Next steps</span></span>
* <span data-ttu-id="2f84f-639">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2f84f-639">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="2f84f-640">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2f84f-640">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="2f84f-641">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="2f84f-641">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="2f84f-642">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="2f84f-642">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


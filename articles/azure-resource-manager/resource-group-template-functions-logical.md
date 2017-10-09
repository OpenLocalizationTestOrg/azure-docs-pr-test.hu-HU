---
title: "aaaAzure Resource Manager sablonfüggvényei - logikai |} Microsoft Docs"
description: "Az Azure Resource Manager sablon toodetermine logikai értékek hello funkciók toouse ismerteti."
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
ms.date: 08/01/2017
ms.author: tomfitz
ms.openlocfilehash: aec6341fbde00b4eba3b4539ff9a9aec774333fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="2395c-103">Az Azure Resource Manager sablonokhoz logikai funkciók</span><span class="sxs-lookup"><span data-stu-id="2395c-103">Logical functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="2395c-104">Erőforrás-kezelő számos funkciókat nyújt a sablonokban összehasonlításához.</span><span class="sxs-lookup"><span data-stu-id="2395c-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="2395c-105">és</span><span class="sxs-lookup"><span data-stu-id="2395c-105">and</span></span>](#and)
* [<span data-ttu-id="2395c-106">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-106">bool</span></span>](#bool)
* [<span data-ttu-id="2395c-107">Ha</span><span class="sxs-lookup"><span data-stu-id="2395c-107">if</span></span>](#if)
* [<span data-ttu-id="2395c-108">nem</span><span class="sxs-lookup"><span data-stu-id="2395c-108">not</span></span>](#not)
* [<span data-ttu-id="2395c-109">vagy</span><span class="sxs-lookup"><span data-stu-id="2395c-109">or</span></span>](#or)

## <a name="and"></a><span data-ttu-id="2395c-110">és</span><span class="sxs-lookup"><span data-stu-id="2395c-110">and</span></span>
`and(arg1, arg2)`

<span data-ttu-id="2395c-111">Ellenőrzi, hogy mindkét paraméter érték igaz.</span><span class="sxs-lookup"><span data-stu-id="2395c-111">Checks whether both parameter values are true.</span></span>

### <a name="parameters"></a><span data-ttu-id="2395c-112">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2395c-112">Parameters</span></span>

| <span data-ttu-id="2395c-113">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2395c-113">Parameter</span></span> | <span data-ttu-id="2395c-114">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2395c-114">Required</span></span> | <span data-ttu-id="2395c-115">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-115">Type</span></span> | <span data-ttu-id="2395c-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="2395c-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2395c-117">arg1</span><span class="sxs-lookup"><span data-stu-id="2395c-117">arg1</span></span> |<span data-ttu-id="2395c-118">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-118">Yes</span></span> |<span data-ttu-id="2395c-119">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-119">boolean</span></span> |<span data-ttu-id="2395c-120">első érték toocheck e hello értéke true.</span><span class="sxs-lookup"><span data-stu-id="2395c-120">hello first value toocheck whether is true.</span></span> |
| <span data-ttu-id="2395c-121">Arg2</span><span class="sxs-lookup"><span data-stu-id="2395c-121">arg2</span></span> |<span data-ttu-id="2395c-122">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-122">Yes</span></span> |<span data-ttu-id="2395c-123">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-123">boolean</span></span> |<span data-ttu-id="2395c-124">második érték toocheck e hello értéke true.</span><span class="sxs-lookup"><span data-stu-id="2395c-124">hello second value toocheck whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2395c-125">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2395c-125">Return value</span></span>

<span data-ttu-id="2395c-126">Beolvasása **igaz** Ha mindkét érték igaz, ha sikertelen, **hamis**.</span><span class="sxs-lookup"><span data-stu-id="2395c-126">Returns **True** if both values are true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="2395c-127">Példák</span><span class="sxs-lookup"><span data-stu-id="2395c-127">Examples</span></span>

<span data-ttu-id="2395c-128">a következő példa azt mutatja meg hogyan hello toouse logikai funkciók.</span><span class="sxs-lookup"><span data-stu-id="2395c-128">hello following example shows how toouse logical functions.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

<span data-ttu-id="2395c-129">Példa megelőző hello hello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="2395c-129">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="2395c-130">Név</span><span class="sxs-lookup"><span data-stu-id="2395c-130">Name</span></span> | <span data-ttu-id="2395c-131">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-131">Type</span></span> | <span data-ttu-id="2395c-132">Érték</span><span class="sxs-lookup"><span data-stu-id="2395c-132">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2395c-133">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-133">andExampleOutput</span></span> | <span data-ttu-id="2395c-134">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-134">Bool</span></span> | <span data-ttu-id="2395c-135">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-135">False</span></span> |
| <span data-ttu-id="2395c-136">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-136">orExampleOutput</span></span> | <span data-ttu-id="2395c-137">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-137">Bool</span></span> | <span data-ttu-id="2395c-138">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2395c-138">True</span></span> |
| <span data-ttu-id="2395c-139">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-139">notExampleOutput</span></span> | <span data-ttu-id="2395c-140">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-140">Bool</span></span> | <span data-ttu-id="2395c-141">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-141">False</span></span> |


## <a name="bool"></a><span data-ttu-id="2395c-142">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-142">bool</span></span>
`bool(arg1)`

<span data-ttu-id="2395c-143">Konvertálja hello paraméter tooa logikai.</span><span class="sxs-lookup"><span data-stu-id="2395c-143">Converts hello parameter tooa boolean.</span></span>

### <a name="parameters"></a><span data-ttu-id="2395c-144">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2395c-144">Parameters</span></span>

| <span data-ttu-id="2395c-145">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2395c-145">Parameter</span></span> | <span data-ttu-id="2395c-146">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2395c-146">Required</span></span> | <span data-ttu-id="2395c-147">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-147">Type</span></span> | <span data-ttu-id="2395c-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="2395c-148">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2395c-149">arg1</span><span class="sxs-lookup"><span data-stu-id="2395c-149">arg1</span></span> |<span data-ttu-id="2395c-150">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-150">Yes</span></span> |<span data-ttu-id="2395c-151">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="2395c-151">string or int</span></span> |<span data-ttu-id="2395c-152">érték tooconvert tooa hello logikai.</span><span class="sxs-lookup"><span data-stu-id="2395c-152">hello value tooconvert tooa boolean.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2395c-153">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2395c-153">Return value</span></span>
<span data-ttu-id="2395c-154">Hello logikai értékként konvertálni az értéket.</span><span class="sxs-lookup"><span data-stu-id="2395c-154">A boolean of hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="2395c-155">Példák</span><span class="sxs-lookup"><span data-stu-id="2395c-155">Examples</span></span>

<span data-ttu-id="2395c-156">a következő példa azt mutatja meg hogyan hello toouse logikai, egész szám vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2395c-156">hello following example shows how toouse bool with a string or integer.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

<span data-ttu-id="2395c-157">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2395c-157">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="2395c-158">Név</span><span class="sxs-lookup"><span data-stu-id="2395c-158">Name</span></span> | <span data-ttu-id="2395c-159">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-159">Type</span></span> | <span data-ttu-id="2395c-160">Érték</span><span class="sxs-lookup"><span data-stu-id="2395c-160">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2395c-161">trueString</span><span class="sxs-lookup"><span data-stu-id="2395c-161">trueString</span></span> | <span data-ttu-id="2395c-162">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-162">Bool</span></span> | <span data-ttu-id="2395c-163">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2395c-163">True</span></span> |
| <span data-ttu-id="2395c-164">falseString</span><span class="sxs-lookup"><span data-stu-id="2395c-164">falseString</span></span> | <span data-ttu-id="2395c-165">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-165">Bool</span></span> | <span data-ttu-id="2395c-166">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-166">False</span></span> |
| <span data-ttu-id="2395c-167">trueInt</span><span class="sxs-lookup"><span data-stu-id="2395c-167">trueInt</span></span> | <span data-ttu-id="2395c-168">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-168">Bool</span></span> | <span data-ttu-id="2395c-169">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2395c-169">True</span></span> |
| <span data-ttu-id="2395c-170">falseInt</span><span class="sxs-lookup"><span data-stu-id="2395c-170">falseInt</span></span> | <span data-ttu-id="2395c-171">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-171">Bool</span></span> | <span data-ttu-id="2395c-172">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-172">False</span></span> |

## <a name="if"></a><span data-ttu-id="2395c-173">Ha</span><span class="sxs-lookup"><span data-stu-id="2395c-173">if</span></span>
`if(condition, trueValue, falseValue)`

<span data-ttu-id="2395c-174">E alapján értéket adja vissza egy feltétele igaz vagy hamis.</span><span class="sxs-lookup"><span data-stu-id="2395c-174">Returns a value based on whether a condition is true or false.</span></span>

### <a name="parameters"></a><span data-ttu-id="2395c-175">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2395c-175">Parameters</span></span>

| <span data-ttu-id="2395c-176">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2395c-176">Parameter</span></span> | <span data-ttu-id="2395c-177">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2395c-177">Required</span></span> | <span data-ttu-id="2395c-178">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-178">Type</span></span> | <span data-ttu-id="2395c-179">Leírás</span><span class="sxs-lookup"><span data-stu-id="2395c-179">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2395c-180">Az állapot</span><span class="sxs-lookup"><span data-stu-id="2395c-180">condition</span></span> |<span data-ttu-id="2395c-181">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-181">Yes</span></span> |<span data-ttu-id="2395c-182">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-182">boolean</span></span> |<span data-ttu-id="2395c-183">hello érték toocheck, hogy igaz.</span><span class="sxs-lookup"><span data-stu-id="2395c-183">hello value toocheck whether it is true.</span></span> |
| <span data-ttu-id="2395c-184">trueValue</span><span class="sxs-lookup"><span data-stu-id="2395c-184">trueValue</span></span> |<span data-ttu-id="2395c-185">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-185">Yes</span></span> | <span data-ttu-id="2395c-186">karakterlánc, int, objektum vagy tömb</span><span class="sxs-lookup"><span data-stu-id="2395c-186">string, int, object, or array</span></span> |<span data-ttu-id="2395c-187">hello érték tooreturn hello feltétel teljesülése esetén.</span><span class="sxs-lookup"><span data-stu-id="2395c-187">hello value tooreturn when hello condition is true.</span></span> |
| <span data-ttu-id="2395c-188">falseValue</span><span class="sxs-lookup"><span data-stu-id="2395c-188">falseValue</span></span> |<span data-ttu-id="2395c-189">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-189">Yes</span></span> | <span data-ttu-id="2395c-190">karakterlánc, int, objektum vagy tömb</span><span class="sxs-lookup"><span data-stu-id="2395c-190">string, int, object, or array</span></span> |<span data-ttu-id="2395c-191">hello érték tooreturn, amikor hello feltétel nem teljesül.</span><span class="sxs-lookup"><span data-stu-id="2395c-191">hello value tooreturn when hello condition is false.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2395c-192">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2395c-192">Return value</span></span>

<span data-ttu-id="2395c-193">A második paraméter adja vissza, ha az első paraméter **igaz**; ellenkező esetben adja vissza a harmadik paraméter.</span><span class="sxs-lookup"><span data-stu-id="2395c-193">Returns second parameter when first parameter is **True**; otherwise, returns third parameter.</span></span>

### <a name="remarks"></a><span data-ttu-id="2395c-194">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2395c-194">Remarks</span></span>

<span data-ttu-id="2395c-195">Használhatja a függvény tooconditionally set erőforrás-tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="2395c-195">You can use this function tooconditionally set a resource property.</span></span> <span data-ttu-id="2395c-196">hello alábbi példa nem egy teljes sablonunk, de azt mutatja, hogy hello feltételesen beállításához hello rendelkezésre állási csoport megfelelő bizonyos részei.</span><span class="sxs-lookup"><span data-stu-id="2395c-196">hello following example is not a full template, but it shows hello relevant portions for conditionally setting hello availability set.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "availabilitySet": {
            "type": "string",
            "allowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "variables": {
        ...
        "availabilitySetName": "availabilitySet1",
        "availabilitySet": {
            "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
        }
    },
    "resources": [
        {
            "condition": "[equals(parameters('availabilitySet'),'yes')]",
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            ...
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "properties": {
                "availabilitySet": "[if(equals(parameters('availabilitySet'),'yes'), variables('availabilitySet'), json('null'))]",
                ...
            }
        },
        ...
    ],
    ...
}
```

### <a name="examples"></a><span data-ttu-id="2395c-197">Példák</span><span class="sxs-lookup"><span data-stu-id="2395c-197">Examples</span></span>

<span data-ttu-id="2395c-198">a következő példa azt mutatja meg hogyan hello toouse hello `if` függvény.</span><span class="sxs-lookup"><span data-stu-id="2395c-198">hello following example shows how toouse hello `if` function.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        }
    }
}
```

<span data-ttu-id="2395c-199">Példa megelőző hello hello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="2395c-199">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="2395c-200">Név</span><span class="sxs-lookup"><span data-stu-id="2395c-200">Name</span></span> | <span data-ttu-id="2395c-201">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-201">Type</span></span> | <span data-ttu-id="2395c-202">Érték</span><span class="sxs-lookup"><span data-stu-id="2395c-202">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2395c-203">yesOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-203">yesOutput</span></span> | <span data-ttu-id="2395c-204">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2395c-204">String</span></span> | <span data-ttu-id="2395c-205">igen</span><span class="sxs-lookup"><span data-stu-id="2395c-205">yes</span></span> |
| <span data-ttu-id="2395c-206">noOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-206">noOutput</span></span> | <span data-ttu-id="2395c-207">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2395c-207">String</span></span> | <span data-ttu-id="2395c-208">nem</span><span class="sxs-lookup"><span data-stu-id="2395c-208">no</span></span> |


## <a name="not"></a><span data-ttu-id="2395c-209">nem</span><span class="sxs-lookup"><span data-stu-id="2395c-209">not</span></span>
`not(arg1)`

<span data-ttu-id="2395c-210">Logikai érték tooits alakít ellentétes érték.</span><span class="sxs-lookup"><span data-stu-id="2395c-210">Converts boolean value tooits opposite value.</span></span>

### <a name="parameters"></a><span data-ttu-id="2395c-211">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2395c-211">Parameters</span></span>

| <span data-ttu-id="2395c-212">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2395c-212">Parameter</span></span> | <span data-ttu-id="2395c-213">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2395c-213">Required</span></span> | <span data-ttu-id="2395c-214">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-214">Type</span></span> | <span data-ttu-id="2395c-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="2395c-215">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2395c-216">arg1</span><span class="sxs-lookup"><span data-stu-id="2395c-216">arg1</span></span> |<span data-ttu-id="2395c-217">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-217">Yes</span></span> |<span data-ttu-id="2395c-218">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-218">boolean</span></span> |<span data-ttu-id="2395c-219">hello érték tooconvert.</span><span class="sxs-lookup"><span data-stu-id="2395c-219">hello value tooconvert.</span></span> |


### <a name="return-value"></a><span data-ttu-id="2395c-220">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2395c-220">Return value</span></span>

<span data-ttu-id="2395c-221">Beolvasása **igaz** paraméter esetén **hamis**.</span><span class="sxs-lookup"><span data-stu-id="2395c-221">Returns **True** when parameter is **False**.</span></span> <span data-ttu-id="2395c-222">Beolvasása **hamis** paraméter esetén **igaz**.</span><span class="sxs-lookup"><span data-stu-id="2395c-222">Returns **False** when parameter is **True**.</span></span>

### <a name="examples"></a><span data-ttu-id="2395c-223">Példák</span><span class="sxs-lookup"><span data-stu-id="2395c-223">Examples</span></span>

<span data-ttu-id="2395c-224">a következő példa azt mutatja meg hogyan hello toouse logikai funkciók.</span><span class="sxs-lookup"><span data-stu-id="2395c-224">hello following example shows how toouse logical functions.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

<span data-ttu-id="2395c-225">Példa megelőző hello hello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="2395c-225">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="2395c-226">Név</span><span class="sxs-lookup"><span data-stu-id="2395c-226">Name</span></span> | <span data-ttu-id="2395c-227">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-227">Type</span></span> | <span data-ttu-id="2395c-228">Érték</span><span class="sxs-lookup"><span data-stu-id="2395c-228">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2395c-229">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-229">andExampleOutput</span></span> | <span data-ttu-id="2395c-230">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-230">Bool</span></span> | <span data-ttu-id="2395c-231">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-231">False</span></span> |
| <span data-ttu-id="2395c-232">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-232">orExampleOutput</span></span> | <span data-ttu-id="2395c-233">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-233">Bool</span></span> | <span data-ttu-id="2395c-234">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2395c-234">True</span></span> |
| <span data-ttu-id="2395c-235">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-235">notExampleOutput</span></span> | <span data-ttu-id="2395c-236">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-236">Bool</span></span> | <span data-ttu-id="2395c-237">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-237">False</span></span> |

<span data-ttu-id="2395c-238">hello alábbi példában **nem** rendelkező [egyenlő](resource-group-template-functions-comparison.md#equals).</span><span class="sxs-lookup"><span data-stu-id="2395c-238">hello following example uses **not** with [equals](resource-group-template-functions-comparison.md#equals).</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

<span data-ttu-id="2395c-239">Példa megelőző hello hello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="2395c-239">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="2395c-240">Név</span><span class="sxs-lookup"><span data-stu-id="2395c-240">Name</span></span> | <span data-ttu-id="2395c-241">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-241">Type</span></span> | <span data-ttu-id="2395c-242">Érték</span><span class="sxs-lookup"><span data-stu-id="2395c-242">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2395c-243">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="2395c-243">checkNotEquals</span></span> | <span data-ttu-id="2395c-244">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-244">Bool</span></span> | <span data-ttu-id="2395c-245">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2395c-245">True</span></span> |


## <a name="or"></a><span data-ttu-id="2395c-246">vagy</span><span class="sxs-lookup"><span data-stu-id="2395c-246">or</span></span>
`or(arg1, arg2)`

<span data-ttu-id="2395c-247">Ellenőrzi, hogy mindkét paraméter értéke igaz.</span><span class="sxs-lookup"><span data-stu-id="2395c-247">Checks whether either parameter value is true.</span></span>

### <a name="parameters"></a><span data-ttu-id="2395c-248">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="2395c-248">Parameters</span></span>

| <span data-ttu-id="2395c-249">Paraméter</span><span class="sxs-lookup"><span data-stu-id="2395c-249">Parameter</span></span> | <span data-ttu-id="2395c-250">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2395c-250">Required</span></span> | <span data-ttu-id="2395c-251">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-251">Type</span></span> | <span data-ttu-id="2395c-252">Leírás</span><span class="sxs-lookup"><span data-stu-id="2395c-252">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="2395c-253">arg1</span><span class="sxs-lookup"><span data-stu-id="2395c-253">arg1</span></span> |<span data-ttu-id="2395c-254">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-254">Yes</span></span> |<span data-ttu-id="2395c-255">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-255">boolean</span></span> |<span data-ttu-id="2395c-256">első érték toocheck e hello értéke true.</span><span class="sxs-lookup"><span data-stu-id="2395c-256">hello first value toocheck whether is true.</span></span> |
| <span data-ttu-id="2395c-257">Arg2</span><span class="sxs-lookup"><span data-stu-id="2395c-257">arg2</span></span> |<span data-ttu-id="2395c-258">Igen</span><span class="sxs-lookup"><span data-stu-id="2395c-258">Yes</span></span> |<span data-ttu-id="2395c-259">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-259">boolean</span></span> |<span data-ttu-id="2395c-260">második érték toocheck e hello értéke true.</span><span class="sxs-lookup"><span data-stu-id="2395c-260">hello second value toocheck whether is true.</span></span> |

### <a name="return-value"></a><span data-ttu-id="2395c-261">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="2395c-261">Return value</span></span>

<span data-ttu-id="2395c-262">Beolvasása **igaz** értéke igaz, ha sikertelen, ha **hamis**.</span><span class="sxs-lookup"><span data-stu-id="2395c-262">Returns **True** if either value is true; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="2395c-263">Példák</span><span class="sxs-lookup"><span data-stu-id="2395c-263">Examples</span></span>

<span data-ttu-id="2395c-264">a következő példa azt mutatja meg hogyan hello toouse logikai funkciók.</span><span class="sxs-lookup"><span data-stu-id="2395c-264">hello following example shows how toouse logical functions.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

<span data-ttu-id="2395c-265">Példa megelőző hello hello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="2395c-265">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="2395c-266">Név</span><span class="sxs-lookup"><span data-stu-id="2395c-266">Name</span></span> | <span data-ttu-id="2395c-267">Típus</span><span class="sxs-lookup"><span data-stu-id="2395c-267">Type</span></span> | <span data-ttu-id="2395c-268">Érték</span><span class="sxs-lookup"><span data-stu-id="2395c-268">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="2395c-269">andExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-269">andExampleOutput</span></span> | <span data-ttu-id="2395c-270">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-270">Bool</span></span> | <span data-ttu-id="2395c-271">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-271">False</span></span> |
| <span data-ttu-id="2395c-272">orExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-272">orExampleOutput</span></span> | <span data-ttu-id="2395c-273">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-273">Bool</span></span> | <span data-ttu-id="2395c-274">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="2395c-274">True</span></span> |
| <span data-ttu-id="2395c-275">notExampleOutput</span><span class="sxs-lookup"><span data-stu-id="2395c-275">notExampleOutput</span></span> | <span data-ttu-id="2395c-276">logikai érték</span><span class="sxs-lookup"><span data-stu-id="2395c-276">Bool</span></span> | <span data-ttu-id="2395c-277">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="2395c-277">False</span></span> |


## <a name="next-steps"></a><span data-ttu-id="2395c-278">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2395c-278">Next steps</span></span>
* <span data-ttu-id="2395c-279">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2395c-279">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="2395c-280">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2395c-280">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="2395c-281">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="2395c-281">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="2395c-282">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="2395c-282">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


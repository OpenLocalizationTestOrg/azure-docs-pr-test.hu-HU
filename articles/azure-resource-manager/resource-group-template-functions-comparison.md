---
title: "Az Azure Resource Manager sablonfüggvényei - összehasonlítása |} Microsoft Docs"
description: "Az értékek összehasonlítása az Azure Resource Manager sablon használatával funkcióit ismerteti."
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
ms.openlocfilehash: 521e5ed06c138bcd374913588f06a2e6c1e99963
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="70946-103">Az Azure Resource Manager sablonokhoz összehasonlítás funkciók</span><span class="sxs-lookup"><span data-stu-id="70946-103">Comparison functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="70946-104">Erőforrás-kezelő számos funkciókat nyújt a sablonokban összehasonlításához.</span><span class="sxs-lookup"><span data-stu-id="70946-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="70946-105">egyenlő</span><span class="sxs-lookup"><span data-stu-id="70946-105">equals</span></span>](#equals)
* [<span data-ttu-id="70946-106">nagyobb</span><span class="sxs-lookup"><span data-stu-id="70946-106">greater</span></span>](#greater)
* [<span data-ttu-id="70946-107">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="70946-107">greaterOrEquals</span></span>](#greaterorequals)
* [<span data-ttu-id="70946-108">kevesebb</span><span class="sxs-lookup"><span data-stu-id="70946-108">less</span></span>](#less)
* [<span data-ttu-id="70946-109">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="70946-109">lessOrEquals</span></span>](#lessorequals)

## <a name="equals"></a><span data-ttu-id="70946-110">egyenlő</span><span class="sxs-lookup"><span data-stu-id="70946-110">equals</span></span>
`equals(arg1, arg2)`

<span data-ttu-id="70946-111">Ellenőrzi, hogy a két érték egyenlő egymással.</span><span class="sxs-lookup"><span data-stu-id="70946-111">Checks whether two values equal each other.</span></span>

### <a name="parameters"></a><span data-ttu-id="70946-112">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="70946-112">Parameters</span></span>

| <span data-ttu-id="70946-113">Paraméter</span><span class="sxs-lookup"><span data-stu-id="70946-113">Parameter</span></span> | <span data-ttu-id="70946-114">Szükséges</span><span class="sxs-lookup"><span data-stu-id="70946-114">Required</span></span> | <span data-ttu-id="70946-115">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-115">Type</span></span> | <span data-ttu-id="70946-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="70946-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="70946-117">arg1</span><span class="sxs-lookup"><span data-stu-id="70946-117">arg1</span></span> |<span data-ttu-id="70946-118">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-118">Yes</span></span> |<span data-ttu-id="70946-119">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="70946-119">int, string, array, or object</span></span> |<span data-ttu-id="70946-120">Az első érték egyenlő kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="70946-120">The first value to check for equality.</span></span> |
| <span data-ttu-id="70946-121">Arg2</span><span class="sxs-lookup"><span data-stu-id="70946-121">arg2</span></span> |<span data-ttu-id="70946-122">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-122">Yes</span></span> |<span data-ttu-id="70946-123">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="70946-123">int, string, array, or object</span></span> |<span data-ttu-id="70946-124">A második érték egyenlő kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="70946-124">The second value to check for equality.</span></span> |

### <a name="return-value"></a><span data-ttu-id="70946-125">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="70946-125">Return value</span></span>

<span data-ttu-id="70946-126">Beolvasása **igaz** Ha két érték egyenlő; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="70946-126">Returns **True** if the values are equal; otherwise, **False**.</span></span>

### <a name="remarks"></a><span data-ttu-id="70946-127">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="70946-127">Remarks</span></span>

<span data-ttu-id="70946-128">A mező értéke függvény gyakran használják a a `condition` elem annak megállapítására, hogy egy erőforrás van telepítve.</span><span class="sxs-lookup"><span data-stu-id="70946-128">The equals function is often used with the `condition` element to test whether a resource is deployed.</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a><span data-ttu-id="70946-129">Példa</span><span class="sxs-lookup"><span data-stu-id="70946-129">Example</span></span>

<span data-ttu-id="70946-130">A példa sablon ellenőrzi a különböző típusú érték azonosságát.</span><span class="sxs-lookup"><span data-stu-id="70946-130">The example template checks different types of values for equality.</span></span> <span data-ttu-id="70946-131">Az alapértelmezett értékeket adhat vissza IGAZ.</span><span class="sxs-lookup"><span data-stu-id="70946-131">All the default values return True.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

<span data-ttu-id="70946-132">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="70946-132">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="70946-133">Név</span><span class="sxs-lookup"><span data-stu-id="70946-133">Name</span></span> | <span data-ttu-id="70946-134">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-134">Type</span></span> | <span data-ttu-id="70946-135">Érték</span><span class="sxs-lookup"><span data-stu-id="70946-135">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="70946-136">checkInts</span><span class="sxs-lookup"><span data-stu-id="70946-136">checkInts</span></span> | <span data-ttu-id="70946-137">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-137">Bool</span></span> | <span data-ttu-id="70946-138">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-138">True</span></span> |
| <span data-ttu-id="70946-139">checkStrings</span><span class="sxs-lookup"><span data-stu-id="70946-139">checkStrings</span></span> | <span data-ttu-id="70946-140">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-140">Bool</span></span> | <span data-ttu-id="70946-141">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-141">True</span></span> |
| <span data-ttu-id="70946-142">checkArrays</span><span class="sxs-lookup"><span data-stu-id="70946-142">checkArrays</span></span> | <span data-ttu-id="70946-143">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-143">Bool</span></span> | <span data-ttu-id="70946-144">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-144">True</span></span> |
| <span data-ttu-id="70946-145">checkObjects</span><span class="sxs-lookup"><span data-stu-id="70946-145">checkObjects</span></span> | <span data-ttu-id="70946-146">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-146">Bool</span></span> | <span data-ttu-id="70946-147">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-147">True</span></span> |


<span data-ttu-id="70946-148">Az alábbi példában [nem](resource-group-template-functions-logical.md#not) rendelkező **egyenlő**.</span><span class="sxs-lookup"><span data-stu-id="70946-148">The following example uses [not](resource-group-template-functions-logical.md#not) with **equals**.</span></span>

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

<span data-ttu-id="70946-149">Az előző példában a kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="70946-149">The output from the preceding example is:</span></span>

| <span data-ttu-id="70946-150">Név</span><span class="sxs-lookup"><span data-stu-id="70946-150">Name</span></span> | <span data-ttu-id="70946-151">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-151">Type</span></span> | <span data-ttu-id="70946-152">Érték</span><span class="sxs-lookup"><span data-stu-id="70946-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="70946-153">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="70946-153">checkNotEquals</span></span> | <span data-ttu-id="70946-154">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-154">Bool</span></span> | <span data-ttu-id="70946-155">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-155">True</span></span> |


## <a name="greater"></a><span data-ttu-id="70946-156">nagyobb</span><span class="sxs-lookup"><span data-stu-id="70946-156">greater</span></span>
`greater(arg1, arg2)`

<span data-ttu-id="70946-157">Ellenőrzi, hogy az első érték nagyobb, mint a második érték.</span><span class="sxs-lookup"><span data-stu-id="70946-157">Checks whether the first value is greater than the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="70946-158">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="70946-158">Parameters</span></span>

| <span data-ttu-id="70946-159">Paraméter</span><span class="sxs-lookup"><span data-stu-id="70946-159">Parameter</span></span> | <span data-ttu-id="70946-160">Szükséges</span><span class="sxs-lookup"><span data-stu-id="70946-160">Required</span></span> | <span data-ttu-id="70946-161">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-161">Type</span></span> | <span data-ttu-id="70946-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="70946-162">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="70946-163">arg1</span><span class="sxs-lookup"><span data-stu-id="70946-163">arg1</span></span> |<span data-ttu-id="70946-164">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-164">Yes</span></span> |<span data-ttu-id="70946-165">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-165">int or string</span></span> |<span data-ttu-id="70946-166">Az első érték nagyobb összehasonlítására.</span><span class="sxs-lookup"><span data-stu-id="70946-166">The first value for the greater comparison.</span></span> |
| <span data-ttu-id="70946-167">Arg2</span><span class="sxs-lookup"><span data-stu-id="70946-167">arg2</span></span> |<span data-ttu-id="70946-168">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-168">Yes</span></span> |<span data-ttu-id="70946-169">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-169">int or string</span></span> |<span data-ttu-id="70946-170">A második érték nagyobb összehasonlítására.</span><span class="sxs-lookup"><span data-stu-id="70946-170">The second value for the greater comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="70946-171">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="70946-171">Return value</span></span>

<span data-ttu-id="70946-172">Beolvasása **igaz** Ha az első érték nagyobb, mint a második érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="70946-172">Returns **True** if the first value is greater than the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="70946-173">Példa</span><span class="sxs-lookup"><span data-stu-id="70946-173">Example</span></span>

<span data-ttu-id="70946-174">A példa sablon ellenőrzi, hogy az egyik érték nagyobb, mint a többi.</span><span class="sxs-lookup"><span data-stu-id="70946-174">The example template checks whether the one value is greater than the other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="70946-175">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="70946-175">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="70946-176">Név</span><span class="sxs-lookup"><span data-stu-id="70946-176">Name</span></span> | <span data-ttu-id="70946-177">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-177">Type</span></span> | <span data-ttu-id="70946-178">Érték</span><span class="sxs-lookup"><span data-stu-id="70946-178">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="70946-179">checkInts</span><span class="sxs-lookup"><span data-stu-id="70946-179">checkInts</span></span> | <span data-ttu-id="70946-180">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-180">Bool</span></span> | <span data-ttu-id="70946-181">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="70946-181">False</span></span> |
| <span data-ttu-id="70946-182">checkStrings</span><span class="sxs-lookup"><span data-stu-id="70946-182">checkStrings</span></span> | <span data-ttu-id="70946-183">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-183">Bool</span></span> | <span data-ttu-id="70946-184">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-184">True</span></span> |


## <a name="greaterorequals"></a><span data-ttu-id="70946-185">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="70946-185">greaterOrEquals</span></span>
`greaterOrEquals(arg1, arg2)`

<span data-ttu-id="70946-186">Ellenőrzi, hogy az első érték kisebb, mint a második érték.</span><span class="sxs-lookup"><span data-stu-id="70946-186">Checks whether the first value is greater than or equal to the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="70946-187">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="70946-187">Parameters</span></span>

| <span data-ttu-id="70946-188">Paraméter</span><span class="sxs-lookup"><span data-stu-id="70946-188">Parameter</span></span> | <span data-ttu-id="70946-189">Szükséges</span><span class="sxs-lookup"><span data-stu-id="70946-189">Required</span></span> | <span data-ttu-id="70946-190">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-190">Type</span></span> | <span data-ttu-id="70946-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="70946-191">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="70946-192">arg1</span><span class="sxs-lookup"><span data-stu-id="70946-192">arg1</span></span> |<span data-ttu-id="70946-193">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-193">Yes</span></span> |<span data-ttu-id="70946-194">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-194">int or string</span></span> |<span data-ttu-id="70946-195">Az első érték kisebb, mint összehasonlítására.</span><span class="sxs-lookup"><span data-stu-id="70946-195">The first value for the greater or equal comparison.</span></span> |
| <span data-ttu-id="70946-196">Arg2</span><span class="sxs-lookup"><span data-stu-id="70946-196">arg2</span></span> |<span data-ttu-id="70946-197">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-197">Yes</span></span> |<span data-ttu-id="70946-198">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-198">int or string</span></span> |<span data-ttu-id="70946-199">A második érték kisebb, mint összehasonlítására.</span><span class="sxs-lookup"><span data-stu-id="70946-199">The second value for the greater or equal comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="70946-200">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="70946-200">Return value</span></span>

<span data-ttu-id="70946-201">Beolvasása **igaz** Ha az első érték nagyobb, mint vagy egyenlő a második érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="70946-201">Returns **True** if the first value is greater than or equal to the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="70946-202">Példa</span><span class="sxs-lookup"><span data-stu-id="70946-202">Example</span></span>

<span data-ttu-id="70946-203">A példa sablon ellenőrzi, hogy az egyik érték nagyobb vagy egyenlő a másikra.</span><span class="sxs-lookup"><span data-stu-id="70946-203">The example template checks whether the one value is greater than or equal to the other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="70946-204">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="70946-204">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="70946-205">Név</span><span class="sxs-lookup"><span data-stu-id="70946-205">Name</span></span> | <span data-ttu-id="70946-206">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-206">Type</span></span> | <span data-ttu-id="70946-207">Érték</span><span class="sxs-lookup"><span data-stu-id="70946-207">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="70946-208">checkInts</span><span class="sxs-lookup"><span data-stu-id="70946-208">checkInts</span></span> | <span data-ttu-id="70946-209">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-209">Bool</span></span> | <span data-ttu-id="70946-210">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="70946-210">False</span></span> |
| <span data-ttu-id="70946-211">checkStrings</span><span class="sxs-lookup"><span data-stu-id="70946-211">checkStrings</span></span> | <span data-ttu-id="70946-212">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-212">Bool</span></span> | <span data-ttu-id="70946-213">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-213">True</span></span> |



## <a name="less"></a><span data-ttu-id="70946-214">kevesebb</span><span class="sxs-lookup"><span data-stu-id="70946-214">less</span></span>
`less(arg1, arg2)`

<span data-ttu-id="70946-215">Ellenőrzi, hogy van-e az első érték kisebb, mint a második érték.</span><span class="sxs-lookup"><span data-stu-id="70946-215">Checks whether the first value is less than the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="70946-216">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="70946-216">Parameters</span></span>

| <span data-ttu-id="70946-217">Paraméter</span><span class="sxs-lookup"><span data-stu-id="70946-217">Parameter</span></span> | <span data-ttu-id="70946-218">Szükséges</span><span class="sxs-lookup"><span data-stu-id="70946-218">Required</span></span> | <span data-ttu-id="70946-219">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-219">Type</span></span> | <span data-ttu-id="70946-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="70946-220">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="70946-221">arg1</span><span class="sxs-lookup"><span data-stu-id="70946-221">arg1</span></span> |<span data-ttu-id="70946-222">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-222">Yes</span></span> |<span data-ttu-id="70946-223">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-223">int or string</span></span> |<span data-ttu-id="70946-224">Az első érték kisebb összehasonlítására.</span><span class="sxs-lookup"><span data-stu-id="70946-224">The first value for the less comparison.</span></span> |
| <span data-ttu-id="70946-225">Arg2</span><span class="sxs-lookup"><span data-stu-id="70946-225">arg2</span></span> |<span data-ttu-id="70946-226">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-226">Yes</span></span> |<span data-ttu-id="70946-227">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-227">int or string</span></span> |<span data-ttu-id="70946-228">A második érték kevesebb összehasonlítására.</span><span class="sxs-lookup"><span data-stu-id="70946-228">The second value for the less comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="70946-229">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="70946-229">Return value</span></span>

<span data-ttu-id="70946-230">Beolvasása **igaz** Ha az első érték kisebb, mint a második érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="70946-230">Returns **True** if the first value is less than the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="70946-231">Példa</span><span class="sxs-lookup"><span data-stu-id="70946-231">Example</span></span>

<span data-ttu-id="70946-232">A példa sablon ellenőrzi, hogy az egyik érték kisebb, mint a többi.</span><span class="sxs-lookup"><span data-stu-id="70946-232">The example template checks whether the one value is less than the other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="70946-233">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="70946-233">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="70946-234">Név</span><span class="sxs-lookup"><span data-stu-id="70946-234">Name</span></span> | <span data-ttu-id="70946-235">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-235">Type</span></span> | <span data-ttu-id="70946-236">Érték</span><span class="sxs-lookup"><span data-stu-id="70946-236">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="70946-237">checkInts</span><span class="sxs-lookup"><span data-stu-id="70946-237">checkInts</span></span> | <span data-ttu-id="70946-238">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-238">Bool</span></span> | <span data-ttu-id="70946-239">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-239">True</span></span> |
| <span data-ttu-id="70946-240">checkStrings</span><span class="sxs-lookup"><span data-stu-id="70946-240">checkStrings</span></span> | <span data-ttu-id="70946-241">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-241">Bool</span></span> | <span data-ttu-id="70946-242">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="70946-242">False</span></span> |


## <a name="lessorequals"></a><span data-ttu-id="70946-243">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="70946-243">lessOrEquals</span></span>
`lessOrEquals(arg1, arg2)`

<span data-ttu-id="70946-244">Ellenőrzi, hogy az első érték nagyobb, mint a második érték.</span><span class="sxs-lookup"><span data-stu-id="70946-244">Checks whether the first value is less than or equal to the second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="70946-245">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="70946-245">Parameters</span></span>

| <span data-ttu-id="70946-246">Paraméter</span><span class="sxs-lookup"><span data-stu-id="70946-246">Parameter</span></span> | <span data-ttu-id="70946-247">Szükséges</span><span class="sxs-lookup"><span data-stu-id="70946-247">Required</span></span> | <span data-ttu-id="70946-248">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-248">Type</span></span> | <span data-ttu-id="70946-249">Leírás</span><span class="sxs-lookup"><span data-stu-id="70946-249">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="70946-250">arg1</span><span class="sxs-lookup"><span data-stu-id="70946-250">arg1</span></span> |<span data-ttu-id="70946-251">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-251">Yes</span></span> |<span data-ttu-id="70946-252">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-252">int or string</span></span> |<span data-ttu-id="70946-253">Az első értékét a kevésbé vagy egyenlőségi összehasonlítás.</span><span class="sxs-lookup"><span data-stu-id="70946-253">The first value for the less or equals comparison.</span></span> |
| <span data-ttu-id="70946-254">Arg2</span><span class="sxs-lookup"><span data-stu-id="70946-254">arg2</span></span> |<span data-ttu-id="70946-255">Igen</span><span class="sxs-lookup"><span data-stu-id="70946-255">Yes</span></span> |<span data-ttu-id="70946-256">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="70946-256">int or string</span></span> |<span data-ttu-id="70946-257">A második érték, annál kisebb a vagy egyenlőségi összehasonlítást.</span><span class="sxs-lookup"><span data-stu-id="70946-257">The second value for the less or equals comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="70946-258">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="70946-258">Return value</span></span>

<span data-ttu-id="70946-259">Beolvasása **igaz** az első érték kisebb, mint vagy egyenlő a második érték, ha sikertelen, ha **hamis**.</span><span class="sxs-lookup"><span data-stu-id="70946-259">Returns **True** if the first value is less than or equal to the second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="70946-260">Példa</span><span class="sxs-lookup"><span data-stu-id="70946-260">Example</span></span>

<span data-ttu-id="70946-261">A példa sablon ellenőrzi, hogy egy érték kisebb vagy egyenlő, mint a másikra.</span><span class="sxs-lookup"><span data-stu-id="70946-261">The example template checks whether the one value is less than or equal to the other.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

<span data-ttu-id="70946-262">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="70946-262">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="70946-263">Név</span><span class="sxs-lookup"><span data-stu-id="70946-263">Name</span></span> | <span data-ttu-id="70946-264">Típus</span><span class="sxs-lookup"><span data-stu-id="70946-264">Type</span></span> | <span data-ttu-id="70946-265">Érték</span><span class="sxs-lookup"><span data-stu-id="70946-265">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="70946-266">checkInts</span><span class="sxs-lookup"><span data-stu-id="70946-266">checkInts</span></span> | <span data-ttu-id="70946-267">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-267">Bool</span></span> | <span data-ttu-id="70946-268">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="70946-268">True</span></span> |
| <span data-ttu-id="70946-269">checkStrings</span><span class="sxs-lookup"><span data-stu-id="70946-269">checkStrings</span></span> | <span data-ttu-id="70946-270">logikai érték</span><span class="sxs-lookup"><span data-stu-id="70946-270">Bool</span></span> | <span data-ttu-id="70946-271">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="70946-271">False</span></span> |



## <a name="next-steps"></a><span data-ttu-id="70946-272">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70946-272">Next steps</span></span>
* <span data-ttu-id="70946-273">A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="70946-273">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="70946-274">Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="70946-274">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="70946-275">Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="70946-275">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="70946-276">A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="70946-276">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


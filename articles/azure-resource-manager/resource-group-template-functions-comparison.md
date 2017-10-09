---
title: "aaaAzure Resource Manager sablonfüggvényei - összehasonlítása |} Microsoft Docs"
description: "Az Azure Resource Manager sablon toocompare értékek hello funkciók toouse ismerteti."
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
ms.openlocfilehash: ebcfc9ed6c93f8b540ec4c066e9457c621800b7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="e2a0b-103">Az Azure Resource Manager sablonokhoz összehasonlítás funkciók</span><span class="sxs-lookup"><span data-stu-id="e2a0b-103">Comparison functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="e2a0b-104">Erőforrás-kezelő számos funkciókat nyújt a sablonokban összehasonlításához.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-104">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="e2a0b-105">egyenlő</span><span class="sxs-lookup"><span data-stu-id="e2a0b-105">equals</span></span>](#equals)
* [<span data-ttu-id="e2a0b-106">nagyobb</span><span class="sxs-lookup"><span data-stu-id="e2a0b-106">greater</span></span>](#greater)
* [<span data-ttu-id="e2a0b-107">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="e2a0b-107">greaterOrEquals</span></span>](#greaterorequals)
* [<span data-ttu-id="e2a0b-108">kevesebb</span><span class="sxs-lookup"><span data-stu-id="e2a0b-108">less</span></span>](#less)
* [<span data-ttu-id="e2a0b-109">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="e2a0b-109">lessOrEquals</span></span>](#lessorequals)

## <a name="equals"></a><span data-ttu-id="e2a0b-110">egyenlő</span><span class="sxs-lookup"><span data-stu-id="e2a0b-110">equals</span></span>
`equals(arg1, arg2)`

<span data-ttu-id="e2a0b-111">Ellenőrzi, hogy a két érték egyenlő egymással.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-111">Checks whether two values equal each other.</span></span>

### <a name="parameters"></a><span data-ttu-id="e2a0b-112">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="e2a0b-112">Parameters</span></span>

| <span data-ttu-id="e2a0b-113">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e2a0b-113">Parameter</span></span> | <span data-ttu-id="e2a0b-114">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e2a0b-114">Required</span></span> | <span data-ttu-id="e2a0b-115">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-115">Type</span></span> | <span data-ttu-id="e2a0b-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="e2a0b-116">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e2a0b-117">arg1</span><span class="sxs-lookup"><span data-stu-id="e2a0b-117">arg1</span></span> |<span data-ttu-id="e2a0b-118">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-118">Yes</span></span> |<span data-ttu-id="e2a0b-119">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="e2a0b-119">int, string, array, or object</span></span> |<span data-ttu-id="e2a0b-120">hello első érték toocheck az egyezés keresésekor.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-120">hello first value toocheck for equality.</span></span> |
| <span data-ttu-id="e2a0b-121">Arg2</span><span class="sxs-lookup"><span data-stu-id="e2a0b-121">arg2</span></span> |<span data-ttu-id="e2a0b-122">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-122">Yes</span></span> |<span data-ttu-id="e2a0b-123">int, string, tömb vagy objektum</span><span class="sxs-lookup"><span data-stu-id="e2a0b-123">int, string, array, or object</span></span> |<span data-ttu-id="e2a0b-124">hello második érték toocheck az egyezés keresésekor.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-124">hello second value toocheck for equality.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e2a0b-125">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-125">Return value</span></span>

<span data-ttu-id="e2a0b-126">Beolvasása **igaz** Ha hello érték egyenlő; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-126">Returns **True** if hello values are equal; otherwise, **False**.</span></span>

### <a name="remarks"></a><span data-ttu-id="e2a0b-127">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="e2a0b-127">Remarks</span></span>

<span data-ttu-id="e2a0b-128">hello egyenlő funkció ugyancsak gyakran használják a hello `condition` elem tootest e erőforrás van telepítve.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-128">hello equals function is often used with hello `condition` element tootest whether a resource is deployed.</span></span>

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

### <a name="example"></a><span data-ttu-id="e2a0b-129">Példa</span><span class="sxs-lookup"><span data-stu-id="e2a0b-129">Example</span></span>

<span data-ttu-id="e2a0b-130">hello példa sablon ellenőrzi a különböző típusú érték azonosságát.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-130">hello example template checks different types of values for equality.</span></span> <span data-ttu-id="e2a0b-131">Minden hello alapértelmezett értéket adnak vissza IGAZ értéket.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-131">All hello default values return True.</span></span>

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

<span data-ttu-id="e2a0b-132">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="e2a0b-132">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="e2a0b-133">Név</span><span class="sxs-lookup"><span data-stu-id="e2a0b-133">Name</span></span> | <span data-ttu-id="e2a0b-134">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-134">Type</span></span> | <span data-ttu-id="e2a0b-135">Érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-135">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e2a0b-136">checkInts</span><span class="sxs-lookup"><span data-stu-id="e2a0b-136">checkInts</span></span> | <span data-ttu-id="e2a0b-137">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-137">Bool</span></span> | <span data-ttu-id="e2a0b-138">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-138">True</span></span> |
| <span data-ttu-id="e2a0b-139">checkStrings</span><span class="sxs-lookup"><span data-stu-id="e2a0b-139">checkStrings</span></span> | <span data-ttu-id="e2a0b-140">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-140">Bool</span></span> | <span data-ttu-id="e2a0b-141">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-141">True</span></span> |
| <span data-ttu-id="e2a0b-142">checkArrays</span><span class="sxs-lookup"><span data-stu-id="e2a0b-142">checkArrays</span></span> | <span data-ttu-id="e2a0b-143">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-143">Bool</span></span> | <span data-ttu-id="e2a0b-144">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-144">True</span></span> |
| <span data-ttu-id="e2a0b-145">checkObjects</span><span class="sxs-lookup"><span data-stu-id="e2a0b-145">checkObjects</span></span> | <span data-ttu-id="e2a0b-146">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-146">Bool</span></span> | <span data-ttu-id="e2a0b-147">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-147">True</span></span> |


<span data-ttu-id="e2a0b-148">hello alábbi példában [nem](resource-group-template-functions-logical.md#not) rendelkező **egyenlő**.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-148">hello following example uses [not](resource-group-template-functions-logical.md#not) with **equals**.</span></span>

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

<span data-ttu-id="e2a0b-149">Példa megelőző hello hello kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="e2a0b-149">hello output from hello preceding example is:</span></span>

| <span data-ttu-id="e2a0b-150">Név</span><span class="sxs-lookup"><span data-stu-id="e2a0b-150">Name</span></span> | <span data-ttu-id="e2a0b-151">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-151">Type</span></span> | <span data-ttu-id="e2a0b-152">Érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e2a0b-153">checkNotEquals</span><span class="sxs-lookup"><span data-stu-id="e2a0b-153">checkNotEquals</span></span> | <span data-ttu-id="e2a0b-154">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-154">Bool</span></span> | <span data-ttu-id="e2a0b-155">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-155">True</span></span> |


## <a name="greater"></a><span data-ttu-id="e2a0b-156">nagyobb</span><span class="sxs-lookup"><span data-stu-id="e2a0b-156">greater</span></span>
`greater(arg1, arg2)`

<span data-ttu-id="e2a0b-157">Ellenőrzi, hogy hello első érték nagyobb, mint hello második érték.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-157">Checks whether hello first value is greater than hello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="e2a0b-158">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="e2a0b-158">Parameters</span></span>

| <span data-ttu-id="e2a0b-159">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e2a0b-159">Parameter</span></span> | <span data-ttu-id="e2a0b-160">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e2a0b-160">Required</span></span> | <span data-ttu-id="e2a0b-161">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-161">Type</span></span> | <span data-ttu-id="e2a0b-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="e2a0b-162">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e2a0b-163">arg1</span><span class="sxs-lookup"><span data-stu-id="e2a0b-163">arg1</span></span> |<span data-ttu-id="e2a0b-164">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-164">Yes</span></span> |<span data-ttu-id="e2a0b-165">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-165">int or string</span></span> |<span data-ttu-id="e2a0b-166">hello hello nagyobb összehasonlítás első érték.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-166">hello first value for hello greater comparison.</span></span> |
| <span data-ttu-id="e2a0b-167">Arg2</span><span class="sxs-lookup"><span data-stu-id="e2a0b-167">arg2</span></span> |<span data-ttu-id="e2a0b-168">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-168">Yes</span></span> |<span data-ttu-id="e2a0b-169">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-169">int or string</span></span> |<span data-ttu-id="e2a0b-170">hello hello nagyobb összehasonlítási második értéket.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-170">hello second value for hello greater comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e2a0b-171">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-171">Return value</span></span>

<span data-ttu-id="e2a0b-172">Beolvasása **igaz** Ha hello első érték nagyobb, mint a második érték hello; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-172">Returns **True** if hello first value is greater than hello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e2a0b-173">Példa</span><span class="sxs-lookup"><span data-stu-id="e2a0b-173">Example</span></span>

<span data-ttu-id="e2a0b-174">hello példa sablon ellenőrzi, hogy hello egyik érték nagyobb, mint más hello.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-174">hello example template checks whether hello one value is greater than hello other.</span></span>

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

<span data-ttu-id="e2a0b-175">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="e2a0b-175">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="e2a0b-176">Név</span><span class="sxs-lookup"><span data-stu-id="e2a0b-176">Name</span></span> | <span data-ttu-id="e2a0b-177">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-177">Type</span></span> | <span data-ttu-id="e2a0b-178">Érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-178">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e2a0b-179">checkInts</span><span class="sxs-lookup"><span data-stu-id="e2a0b-179">checkInts</span></span> | <span data-ttu-id="e2a0b-180">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-180">Bool</span></span> | <span data-ttu-id="e2a0b-181">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-181">False</span></span> |
| <span data-ttu-id="e2a0b-182">checkStrings</span><span class="sxs-lookup"><span data-stu-id="e2a0b-182">checkStrings</span></span> | <span data-ttu-id="e2a0b-183">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-183">Bool</span></span> | <span data-ttu-id="e2a0b-184">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-184">True</span></span> |


## <a name="greaterorequals"></a><span data-ttu-id="e2a0b-185">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="e2a0b-185">greaterOrEquals</span></span>
`greaterOrEquals(arg1, arg2)`

<span data-ttu-id="e2a0b-186">Ellenőrzi, hogy hello első érték második érték nagyobb vagy egyenlő toohello.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-186">Checks whether hello first value is greater than or equal toohello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="e2a0b-187">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="e2a0b-187">Parameters</span></span>

| <span data-ttu-id="e2a0b-188">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e2a0b-188">Parameter</span></span> | <span data-ttu-id="e2a0b-189">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e2a0b-189">Required</span></span> | <span data-ttu-id="e2a0b-190">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-190">Type</span></span> | <span data-ttu-id="e2a0b-191">Leírás</span><span class="sxs-lookup"><span data-stu-id="e2a0b-191">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e2a0b-192">arg1</span><span class="sxs-lookup"><span data-stu-id="e2a0b-192">arg1</span></span> |<span data-ttu-id="e2a0b-193">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-193">Yes</span></span> |<span data-ttu-id="e2a0b-194">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-194">int or string</span></span> |<span data-ttu-id="e2a0b-195">hello hello nagyobb vagy egyenlő összehasonlítás első érték.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-195">hello first value for hello greater or equal comparison.</span></span> |
| <span data-ttu-id="e2a0b-196">Arg2</span><span class="sxs-lookup"><span data-stu-id="e2a0b-196">arg2</span></span> |<span data-ttu-id="e2a0b-197">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-197">Yes</span></span> |<span data-ttu-id="e2a0b-198">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-198">int or string</span></span> |<span data-ttu-id="e2a0b-199">hello hello nagyobb vagy egyenlő összehasonlítási második értéket.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-199">hello second value for hello greater or equal comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e2a0b-200">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-200">Return value</span></span>

<span data-ttu-id="e2a0b-201">Beolvasása **igaz** Ha hello első érték második érték nagyobb vagy egyenlő toohello; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-201">Returns **True** if hello first value is greater than or equal toohello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e2a0b-202">Példa</span><span class="sxs-lookup"><span data-stu-id="e2a0b-202">Example</span></span>

<span data-ttu-id="e2a0b-203">hello példa sablon ellenőrzi, hogy hello egyik érték nagyobb, mint vagy egyenlő toohello más.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-203">hello example template checks whether hello one value is greater than or equal toohello other.</span></span>

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

<span data-ttu-id="e2a0b-204">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="e2a0b-204">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="e2a0b-205">Név</span><span class="sxs-lookup"><span data-stu-id="e2a0b-205">Name</span></span> | <span data-ttu-id="e2a0b-206">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-206">Type</span></span> | <span data-ttu-id="e2a0b-207">Érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-207">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e2a0b-208">checkInts</span><span class="sxs-lookup"><span data-stu-id="e2a0b-208">checkInts</span></span> | <span data-ttu-id="e2a0b-209">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-209">Bool</span></span> | <span data-ttu-id="e2a0b-210">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-210">False</span></span> |
| <span data-ttu-id="e2a0b-211">checkStrings</span><span class="sxs-lookup"><span data-stu-id="e2a0b-211">checkStrings</span></span> | <span data-ttu-id="e2a0b-212">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-212">Bool</span></span> | <span data-ttu-id="e2a0b-213">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-213">True</span></span> |



## <a name="less"></a><span data-ttu-id="e2a0b-214">kevesebb</span><span class="sxs-lookup"><span data-stu-id="e2a0b-214">less</span></span>
`less(arg1, arg2)`

<span data-ttu-id="e2a0b-215">Ellenőrzi, hogy hello első érték kisebb, mint hello a második érték.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-215">Checks whether hello first value is less than hello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="e2a0b-216">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="e2a0b-216">Parameters</span></span>

| <span data-ttu-id="e2a0b-217">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e2a0b-217">Parameter</span></span> | <span data-ttu-id="e2a0b-218">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e2a0b-218">Required</span></span> | <span data-ttu-id="e2a0b-219">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-219">Type</span></span> | <span data-ttu-id="e2a0b-220">Leírás</span><span class="sxs-lookup"><span data-stu-id="e2a0b-220">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e2a0b-221">arg1</span><span class="sxs-lookup"><span data-stu-id="e2a0b-221">arg1</span></span> |<span data-ttu-id="e2a0b-222">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-222">Yes</span></span> |<span data-ttu-id="e2a0b-223">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-223">int or string</span></span> |<span data-ttu-id="e2a0b-224">hello hello kevésbé összehasonlítás első érték.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-224">hello first value for hello less comparison.</span></span> |
| <span data-ttu-id="e2a0b-225">Arg2</span><span class="sxs-lookup"><span data-stu-id="e2a0b-225">arg2</span></span> |<span data-ttu-id="e2a0b-226">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-226">Yes</span></span> |<span data-ttu-id="e2a0b-227">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-227">int or string</span></span> |<span data-ttu-id="e2a0b-228">hello kevésbé összehasonlítás hello második érték.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-228">hello second value for hello less comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e2a0b-229">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-229">Return value</span></span>

<span data-ttu-id="e2a0b-230">Beolvasása **igaz** Ha hello első érték kisebb, mint hello második érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-230">Returns **True** if hello first value is less than hello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e2a0b-231">Példa</span><span class="sxs-lookup"><span data-stu-id="e2a0b-231">Example</span></span>

<span data-ttu-id="e2a0b-232">hello példa sablon ellenőrzi, hogy hello egyik érték kisebb, mint más hello.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-232">hello example template checks whether hello one value is less than hello other.</span></span>

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

<span data-ttu-id="e2a0b-233">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="e2a0b-233">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="e2a0b-234">Név</span><span class="sxs-lookup"><span data-stu-id="e2a0b-234">Name</span></span> | <span data-ttu-id="e2a0b-235">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-235">Type</span></span> | <span data-ttu-id="e2a0b-236">Érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-236">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e2a0b-237">checkInts</span><span class="sxs-lookup"><span data-stu-id="e2a0b-237">checkInts</span></span> | <span data-ttu-id="e2a0b-238">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-238">Bool</span></span> | <span data-ttu-id="e2a0b-239">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-239">True</span></span> |
| <span data-ttu-id="e2a0b-240">checkStrings</span><span class="sxs-lookup"><span data-stu-id="e2a0b-240">checkStrings</span></span> | <span data-ttu-id="e2a0b-241">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-241">Bool</span></span> | <span data-ttu-id="e2a0b-242">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-242">False</span></span> |


## <a name="lessorequals"></a><span data-ttu-id="e2a0b-243">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="e2a0b-243">lessOrEquals</span></span>
`lessOrEquals(arg1, arg2)`

<span data-ttu-id="e2a0b-244">Ellenőrzi, hogy hello első érték kisebb vagy egyenlő, mint a második érték toohello.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-244">Checks whether hello first value is less than or equal toohello second value.</span></span>

### <a name="parameters"></a><span data-ttu-id="e2a0b-245">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="e2a0b-245">Parameters</span></span>

| <span data-ttu-id="e2a0b-246">Paraméter</span><span class="sxs-lookup"><span data-stu-id="e2a0b-246">Parameter</span></span> | <span data-ttu-id="e2a0b-247">Szükséges</span><span class="sxs-lookup"><span data-stu-id="e2a0b-247">Required</span></span> | <span data-ttu-id="e2a0b-248">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-248">Type</span></span> | <span data-ttu-id="e2a0b-249">Leírás</span><span class="sxs-lookup"><span data-stu-id="e2a0b-249">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="e2a0b-250">arg1</span><span class="sxs-lookup"><span data-stu-id="e2a0b-250">arg1</span></span> |<span data-ttu-id="e2a0b-251">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-251">Yes</span></span> |<span data-ttu-id="e2a0b-252">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-252">int or string</span></span> |<span data-ttu-id="e2a0b-253">hello első értéke hello kisebb vagy egyenlő összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-253">hello first value for hello less or equals comparison.</span></span> |
| <span data-ttu-id="e2a0b-254">Arg2</span><span class="sxs-lookup"><span data-stu-id="e2a0b-254">arg2</span></span> |<span data-ttu-id="e2a0b-255">Igen</span><span class="sxs-lookup"><span data-stu-id="e2a0b-255">Yes</span></span> |<span data-ttu-id="e2a0b-256">egész szám vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e2a0b-256">int or string</span></span> |<span data-ttu-id="e2a0b-257">második értéke hello hello kisebb vagy egyenlő összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-257">hello second value for hello less or equals comparison.</span></span> |

### <a name="return-value"></a><span data-ttu-id="e2a0b-258">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-258">Return value</span></span>

<span data-ttu-id="e2a0b-259">Beolvasása **igaz** hello első értéke kisebb vagy egyenlő, mint ha toohello második érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-259">Returns **True** if hello first value is less than or equal toohello second value; otherwise, **False**.</span></span>

### <a name="example"></a><span data-ttu-id="e2a0b-260">Példa</span><span class="sxs-lookup"><span data-stu-id="e2a0b-260">Example</span></span>

<span data-ttu-id="e2a0b-261">hello példa sablon ellenőrzi, hogy egy érték hello kisebb vagy egyenlő, mint más toohello.</span><span class="sxs-lookup"><span data-stu-id="e2a0b-261">hello example template checks whether hello one value is less than or equal toohello other.</span></span>

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

<span data-ttu-id="e2a0b-262">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="e2a0b-262">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="e2a0b-263">Név</span><span class="sxs-lookup"><span data-stu-id="e2a0b-263">Name</span></span> | <span data-ttu-id="e2a0b-264">Típus</span><span class="sxs-lookup"><span data-stu-id="e2a0b-264">Type</span></span> | <span data-ttu-id="e2a0b-265">Érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-265">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="e2a0b-266">checkInts</span><span class="sxs-lookup"><span data-stu-id="e2a0b-266">checkInts</span></span> | <span data-ttu-id="e2a0b-267">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-267">Bool</span></span> | <span data-ttu-id="e2a0b-268">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-268">True</span></span> |
| <span data-ttu-id="e2a0b-269">checkStrings</span><span class="sxs-lookup"><span data-stu-id="e2a0b-269">checkStrings</span></span> | <span data-ttu-id="e2a0b-270">logikai érték</span><span class="sxs-lookup"><span data-stu-id="e2a0b-270">Bool</span></span> | <span data-ttu-id="e2a0b-271">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="e2a0b-271">False</span></span> |



## <a name="next-steps"></a><span data-ttu-id="e2a0b-272">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2a0b-272">Next steps</span></span>
* <span data-ttu-id="e2a0b-273">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e2a0b-273">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="e2a0b-274">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="e2a0b-274">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="e2a0b-275">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e2a0b-275">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="e2a0b-276">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="e2a0b-276">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


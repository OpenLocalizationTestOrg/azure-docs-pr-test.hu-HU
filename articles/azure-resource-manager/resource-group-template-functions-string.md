---
title: "Az Azure Resource Manager sablonfüggvényei - karakterlánc |} Microsoft Docs"
description: "Az Azure Resource Manager-sablonok segítségével karakterláncok használata funkcióit ismerteti."
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
ms.openlocfilehash: 3e5c9ca546629f782a3d722b49f5fbaf5147e823
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="string-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="bbd36-103">Az Azure Resource Manager sablonokhoz karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-103">String functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="bbd36-104">Erőforrás-kezelő a következő funkciókat nyújt karakterláncok használata.</span><span class="sxs-lookup"><span data-stu-id="bbd36-104">Resource Manager provides the following functions for working with strings:</span></span>

* [<span data-ttu-id="bbd36-105">a Base64</span><span class="sxs-lookup"><span data-stu-id="bbd36-105">base64</span></span>](#base64)
* [<span data-ttu-id="bbd36-106">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="bbd36-106">base64ToJson</span></span>](#base64tojson)
* [<span data-ttu-id="bbd36-107">base64ToString</span><span class="sxs-lookup"><span data-stu-id="bbd36-107">base64ToString</span></span>](#base64tostring)
* [<span data-ttu-id="bbd36-108">Concat</span><span class="sxs-lookup"><span data-stu-id="bbd36-108">concat</span></span>](#concat)
* [<span data-ttu-id="bbd36-109">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="bbd36-109">contains</span></span>](#contains)
* [<span data-ttu-id="bbd36-110">dataUri</span><span class="sxs-lookup"><span data-stu-id="bbd36-110">dataUri</span></span>](#datauri)
* [<span data-ttu-id="bbd36-111">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="bbd36-111">dataUriToString</span></span>](#datauritostring)
* [<span data-ttu-id="bbd36-112">üres</span><span class="sxs-lookup"><span data-stu-id="bbd36-112">empty</span></span>](#empty)
* [<span data-ttu-id="bbd36-113">megadott módon végződő</span><span class="sxs-lookup"><span data-stu-id="bbd36-113">endsWith</span></span>](#endswith)
* [<span data-ttu-id="bbd36-114">első</span><span class="sxs-lookup"><span data-stu-id="bbd36-114">first</span></span>](#first)
* [<span data-ttu-id="bbd36-115">indexOf</span><span class="sxs-lookup"><span data-stu-id="bbd36-115">indexOf</span></span>](#indexof)
* [<span data-ttu-id="bbd36-116">utolsó</span><span class="sxs-lookup"><span data-stu-id="bbd36-116">last</span></span>](#last)
* [<span data-ttu-id="bbd36-117">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="bbd36-117">lastIndexOf</span></span>](#lastindexof)
* [<span data-ttu-id="bbd36-118">hossza</span><span class="sxs-lookup"><span data-stu-id="bbd36-118">length</span></span>](#length)
* [<span data-ttu-id="bbd36-119">padLeft</span><span class="sxs-lookup"><span data-stu-id="bbd36-119">padLeft</span></span>](#padleft)
* [<span data-ttu-id="bbd36-120">cserélje le</span><span class="sxs-lookup"><span data-stu-id="bbd36-120">replace</span></span>](#replace)
* [<span data-ttu-id="bbd36-121">hagyja ki</span><span class="sxs-lookup"><span data-stu-id="bbd36-121">skip</span></span>](#skip)
* [<span data-ttu-id="bbd36-122">felosztás</span><span class="sxs-lookup"><span data-stu-id="bbd36-122">split</span></span>](#split)
* [<span data-ttu-id="bbd36-123">startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="bbd36-123">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="bbd36-124">karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-124">string</span></span>](#string)
* [<span data-ttu-id="bbd36-125">Substring</span><span class="sxs-lookup"><span data-stu-id="bbd36-125">substring</span></span>](#substring)
* [<span data-ttu-id="bbd36-126">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="bbd36-126">take</span></span>](#take)
* [<span data-ttu-id="bbd36-127">toLower</span><span class="sxs-lookup"><span data-stu-id="bbd36-127">toLower</span></span>](#tolower)
* [<span data-ttu-id="bbd36-128">toUpper</span><span class="sxs-lookup"><span data-stu-id="bbd36-128">toUpper</span></span>](#toupper)
* [<span data-ttu-id="bbd36-129">Trim</span><span class="sxs-lookup"><span data-stu-id="bbd36-129">trim</span></span>](#trim)
* [<span data-ttu-id="bbd36-130">uniqueString</span><span class="sxs-lookup"><span data-stu-id="bbd36-130">uniqueString</span></span>](#uniquestring)
* [<span data-ttu-id="bbd36-131">URI</span><span class="sxs-lookup"><span data-stu-id="bbd36-131">uri</span></span>](#uri)
* [<span data-ttu-id="bbd36-132">uriComponent</span><span class="sxs-lookup"><span data-stu-id="bbd36-132">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="bbd36-133">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="bbd36-133">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a><span data-ttu-id="bbd36-134">a Base64</span><span class="sxs-lookup"><span data-stu-id="bbd36-134">base64</span></span>
`base64(inputString)`

<span data-ttu-id="bbd36-135">A bemeneti karakterlánc a base64 alakot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-135">Returns the base64 representation of the input string.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-136">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-136">Parameters</span></span>

| <span data-ttu-id="bbd36-137">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-137">Parameter</span></span> | <span data-ttu-id="bbd36-138">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-138">Required</span></span> | <span data-ttu-id="bbd36-139">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-139">Type</span></span> | <span data-ttu-id="bbd36-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-140">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-141">inputString</span><span class="sxs-lookup"><span data-stu-id="bbd36-141">inputString</span></span> |<span data-ttu-id="bbd36-142">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-142">Yes</span></span> |<span data-ttu-id="bbd36-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-143">string</span></span> |<span data-ttu-id="bbd36-144">A visszatérési érték, mint a Base64 kódolású megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="bbd36-144">The value to return as a base64 representation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-145">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-145">Return value</span></span>

<span data-ttu-id="bbd36-146">A base64 tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-146">A string containing the base64 representation.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-147">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-147">Examples</span></span>

<span data-ttu-id="bbd36-148">A következő példa bemutatja, hogyan a Base64 kódolású függvény használatát.</span><span class="sxs-lookup"><span data-stu-id="bbd36-148">The following example shows how to use the base64 function.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-149">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-149">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-150">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-150">Name</span></span> | <span data-ttu-id="bbd36-151">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-151">Type</span></span> | <span data-ttu-id="bbd36-152">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-153">base64Output</span><span class="sxs-lookup"><span data-stu-id="bbd36-153">base64Output</span></span> | <span data-ttu-id="bbd36-154">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-154">String</span></span> | <span data-ttu-id="bbd36-155">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="bbd36-155">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="bbd36-156">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-156">toStringOutput</span></span> | <span data-ttu-id="bbd36-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-157">String</span></span> | <span data-ttu-id="bbd36-158">egy két há'</span><span class="sxs-lookup"><span data-stu-id="bbd36-158">one, two, three</span></span> |
| <span data-ttu-id="bbd36-159">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-159">toJsonOutput</span></span> | <span data-ttu-id="bbd36-160">Objektum</span><span class="sxs-lookup"><span data-stu-id="bbd36-160">Object</span></span> | <span data-ttu-id="bbd36-161">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="bbd36-161">{"one": "a", "two": "b"}</span></span> |

<a id="base64tojson" />

## <a name="base64tojson"></a><span data-ttu-id="bbd36-162">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="bbd36-162">base64ToJson</span></span>
`base64tojson`

<span data-ttu-id="bbd36-163">A Base64 kódolású megjelenítése konvertál egy JSON-objektum.</span><span class="sxs-lookup"><span data-stu-id="bbd36-163">Converts a base64 representation to a JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-164">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-164">Parameters</span></span>

| <span data-ttu-id="bbd36-165">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-165">Parameter</span></span> | <span data-ttu-id="bbd36-166">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-166">Required</span></span> | <span data-ttu-id="bbd36-167">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-167">Type</span></span> | <span data-ttu-id="bbd36-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-168">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-169">base64Value</span><span class="sxs-lookup"><span data-stu-id="bbd36-169">base64Value</span></span> |<span data-ttu-id="bbd36-170">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-170">Yes</span></span> |<span data-ttu-id="bbd36-171">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-171">string</span></span> |<span data-ttu-id="bbd36-172">A base64 ábrázolását, egy JSON-objektum konvertálása.</span><span class="sxs-lookup"><span data-stu-id="bbd36-172">The base64 representation to convert to a JSON object.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-173">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-173">Return value</span></span>

<span data-ttu-id="bbd36-174">A JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="bbd36-174">A JSON object.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-175">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-175">Examples</span></span>

<span data-ttu-id="bbd36-176">Az alábbi példában a base64ToJson függvény konvertálni az base64 értéket:</span><span class="sxs-lookup"><span data-stu-id="bbd36-176">The following example uses the base64ToJson function to convert a base64 value:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-177">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-177">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-178">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-178">Name</span></span> | <span data-ttu-id="bbd36-179">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-179">Type</span></span> | <span data-ttu-id="bbd36-180">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-180">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-181">base64Output</span><span class="sxs-lookup"><span data-stu-id="bbd36-181">base64Output</span></span> | <span data-ttu-id="bbd36-182">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-182">String</span></span> | <span data-ttu-id="bbd36-183">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="bbd36-183">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="bbd36-184">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-184">toStringOutput</span></span> | <span data-ttu-id="bbd36-185">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-185">String</span></span> | <span data-ttu-id="bbd36-186">egy két há'</span><span class="sxs-lookup"><span data-stu-id="bbd36-186">one, two, three</span></span> |
| <span data-ttu-id="bbd36-187">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-187">toJsonOutput</span></span> | <span data-ttu-id="bbd36-188">Objektum</span><span class="sxs-lookup"><span data-stu-id="bbd36-188">Object</span></span> | <span data-ttu-id="bbd36-189">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="bbd36-189">{"one": "a", "two": "b"}</span></span> |

<a id="base64tostring" />

## <a name="base64tostring"></a><span data-ttu-id="bbd36-190">base64ToString</span><span class="sxs-lookup"><span data-stu-id="bbd36-190">base64ToString</span></span>
`base64ToString(base64Value)`

<span data-ttu-id="bbd36-191">A Base64 kódolású megjelenítése karakterlánccá alakítja át.</span><span class="sxs-lookup"><span data-stu-id="bbd36-191">Converts a base64 representation to a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-192">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-192">Parameters</span></span>

| <span data-ttu-id="bbd36-193">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-193">Parameter</span></span> | <span data-ttu-id="bbd36-194">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-194">Required</span></span> | <span data-ttu-id="bbd36-195">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-195">Type</span></span> | <span data-ttu-id="bbd36-196">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-196">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-197">base64Value</span><span class="sxs-lookup"><span data-stu-id="bbd36-197">base64Value</span></span> |<span data-ttu-id="bbd36-198">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-198">Yes</span></span> |<span data-ttu-id="bbd36-199">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-199">string</span></span> |<span data-ttu-id="bbd36-200">A base64 ábrázolását karakterlánccá konvertálni.</span><span class="sxs-lookup"><span data-stu-id="bbd36-200">The base64 representation to convert to a string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-201">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-201">Return value</span></span>

<span data-ttu-id="bbd36-202">A konvertált base64 érték karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-202">A string of the converted base64 value.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-203">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-203">Examples</span></span>

<span data-ttu-id="bbd36-204">Az alábbi példában a base64ToString függvény konvertálni az base64 értéket:</span><span class="sxs-lookup"><span data-stu-id="bbd36-204">The following example uses the base64ToString function to convert a base64 value:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-205">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-205">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-206">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-206">Name</span></span> | <span data-ttu-id="bbd36-207">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-207">Type</span></span> | <span data-ttu-id="bbd36-208">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-208">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-209">base64Output</span><span class="sxs-lookup"><span data-stu-id="bbd36-209">base64Output</span></span> | <span data-ttu-id="bbd36-210">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-210">String</span></span> | <span data-ttu-id="bbd36-211">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="bbd36-211">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="bbd36-212">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-212">toStringOutput</span></span> | <span data-ttu-id="bbd36-213">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-213">String</span></span> | <span data-ttu-id="bbd36-214">egy két há'</span><span class="sxs-lookup"><span data-stu-id="bbd36-214">one, two, three</span></span> |
| <span data-ttu-id="bbd36-215">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-215">toJsonOutput</span></span> | <span data-ttu-id="bbd36-216">Objektum</span><span class="sxs-lookup"><span data-stu-id="bbd36-216">Object</span></span> | <span data-ttu-id="bbd36-217">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="bbd36-217">{"one": "a", "two": "b"}</span></span> |



<a id="concat" />

## <a name="concat"></a><span data-ttu-id="bbd36-218">Concat</span><span class="sxs-lookup"><span data-stu-id="bbd36-218">concat</span></span>
`concat (arg1, arg2, arg3, ...)`

<span data-ttu-id="bbd36-219">Több karakterlánc-értékek egyesíti, és a összefűzött karakterláncot ad vissza, vagy több tömbök egyesíti, és a összefűzött tömböt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-219">Combines multiple string values and returns the concatenated string, or combines multiple arrays and returns the concatenated array.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-220">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-220">Parameters</span></span>

| <span data-ttu-id="bbd36-221">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-221">Parameter</span></span> | <span data-ttu-id="bbd36-222">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-222">Required</span></span> | <span data-ttu-id="bbd36-223">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-223">Type</span></span> | <span data-ttu-id="bbd36-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-224">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-225">arg1</span><span class="sxs-lookup"><span data-stu-id="bbd36-225">arg1</span></span> |<span data-ttu-id="bbd36-226">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-226">Yes</span></span> |<span data-ttu-id="bbd36-227">karakterlánc vagy tömb</span><span class="sxs-lookup"><span data-stu-id="bbd36-227">string or array</span></span> |<span data-ttu-id="bbd36-228">A kapott első értéke.</span><span class="sxs-lookup"><span data-stu-id="bbd36-228">The first value for concatenation.</span></span> |
| <span data-ttu-id="bbd36-229">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="bbd36-229">additional arguments</span></span> |<span data-ttu-id="bbd36-230">Nem</span><span class="sxs-lookup"><span data-stu-id="bbd36-230">No</span></span> |<span data-ttu-id="bbd36-231">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-231">string</span></span> |<span data-ttu-id="bbd36-232">További értéket kapott a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="bbd36-232">Additional values in sequential order for concatenation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-233">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-233">Return value</span></span>
<span data-ttu-id="bbd36-234">A karakterlánc vagy tömb összefűzött.</span><span class="sxs-lookup"><span data-stu-id="bbd36-234">A string or array of concatenated values.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-235">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-235">Examples</span></span>

<span data-ttu-id="bbd36-236">A következő példa bemutatja, hogyan kombinálhatja a két karakterlánc-értékeket, és olyan összefűzött karakterláncot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-236">The following example shows how to combine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="bbd36-237">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-237">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-238">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-238">Name</span></span> | <span data-ttu-id="bbd36-239">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-239">Type</span></span> | <span data-ttu-id="bbd36-240">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-240">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-241">concatOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-241">concatOutput</span></span> | <span data-ttu-id="bbd36-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-242">String</span></span> | <span data-ttu-id="bbd36-243">előtag-5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="bbd36-243">prefix-5yj4yjf5mbg72</span></span> |

<span data-ttu-id="bbd36-244">A következő példa bemutatja, hogyan kombinálhatók két tömb.</span><span class="sxs-lookup"><span data-stu-id="bbd36-244">The following example shows how to combine two arrays.</span></span>

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

<span data-ttu-id="bbd36-245">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-245">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-246">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-246">Name</span></span> | <span data-ttu-id="bbd36-247">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-247">Type</span></span> | <span data-ttu-id="bbd36-248">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-249">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="bbd36-249">return</span></span> | <span data-ttu-id="bbd36-250">A tömb</span><span class="sxs-lookup"><span data-stu-id="bbd36-250">Array</span></span> | <span data-ttu-id="bbd36-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="bbd36-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="bbd36-252">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="bbd36-252">contains</span></span>
`contains (container, itemToFind)`

<span data-ttu-id="bbd36-253">Ellenőrzi, hogy egy tömb értéket tartalmaz, objektum kulcsot tartalmaz, vagy egy karakterláncot egy substring tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-253">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-254">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-254">Parameters</span></span>

| <span data-ttu-id="bbd36-255">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-255">Parameter</span></span> | <span data-ttu-id="bbd36-256">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-256">Required</span></span> | <span data-ttu-id="bbd36-257">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-257">Type</span></span> | <span data-ttu-id="bbd36-258">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-258">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-259">Tároló</span><span class="sxs-lookup"><span data-stu-id="bbd36-259">container</span></span> |<span data-ttu-id="bbd36-260">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-260">Yes</span></span> |<span data-ttu-id="bbd36-261">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-261">array, object, or string</span></span> |<span data-ttu-id="bbd36-262">Az érték, amely tartalmazza a keresendő érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-262">The value that contains the value to find.</span></span> |
| <span data-ttu-id="bbd36-263">itemToFind</span><span class="sxs-lookup"><span data-stu-id="bbd36-263">itemToFind</span></span> |<span data-ttu-id="bbd36-264">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-264">Yes</span></span> |<span data-ttu-id="bbd36-265">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="bbd36-265">string or int</span></span> |<span data-ttu-id="bbd36-266">Az érték kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="bbd36-266">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-267">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-267">Return value</span></span>

<span data-ttu-id="bbd36-268">**Igaz** Ha az elem található; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="bbd36-268">**True** if the item is found; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-269">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-269">Examples</span></span>

<span data-ttu-id="bbd36-270">A következő példa bemutatja, hogyan használható különböző típusú tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="bbd36-270">The following example shows how to use contains with different types:</span></span>

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

<span data-ttu-id="bbd36-271">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-271">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-272">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-272">Name</span></span> | <span data-ttu-id="bbd36-273">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-273">Type</span></span> | <span data-ttu-id="bbd36-274">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-274">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-275">stringTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-275">stringTrue</span></span> | <span data-ttu-id="bbd36-276">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-276">Bool</span></span> | <span data-ttu-id="bbd36-277">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-277">True</span></span> |
| <span data-ttu-id="bbd36-278">stringFalse</span><span class="sxs-lookup"><span data-stu-id="bbd36-278">stringFalse</span></span> | <span data-ttu-id="bbd36-279">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-279">Bool</span></span> | <span data-ttu-id="bbd36-280">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="bbd36-280">False</span></span> |
| <span data-ttu-id="bbd36-281">objectTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-281">objectTrue</span></span> | <span data-ttu-id="bbd36-282">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-282">Bool</span></span> | <span data-ttu-id="bbd36-283">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-283">True</span></span> |
| <span data-ttu-id="bbd36-284">objectFalse</span><span class="sxs-lookup"><span data-stu-id="bbd36-284">objectFalse</span></span> | <span data-ttu-id="bbd36-285">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-285">Bool</span></span> | <span data-ttu-id="bbd36-286">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="bbd36-286">False</span></span> |
| <span data-ttu-id="bbd36-287">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-287">arrayTrue</span></span> | <span data-ttu-id="bbd36-288">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-288">Bool</span></span> | <span data-ttu-id="bbd36-289">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-289">True</span></span> |
| <span data-ttu-id="bbd36-290">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="bbd36-290">arrayFalse</span></span> | <span data-ttu-id="bbd36-291">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-291">Bool</span></span> | <span data-ttu-id="bbd36-292">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="bbd36-292">False</span></span> |

<a id="datauri" />

## <a name="datauri"></a><span data-ttu-id="bbd36-293">dataUri</span><span class="sxs-lookup"><span data-stu-id="bbd36-293">dataUri</span></span>
`dataUri(stringToConvert)`

<span data-ttu-id="bbd36-294">Egy adat-URI azonosító alakít egy értéket.</span><span class="sxs-lookup"><span data-stu-id="bbd36-294">Converts a value to a data URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-295">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-295">Parameters</span></span>

| <span data-ttu-id="bbd36-296">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-296">Parameter</span></span> | <span data-ttu-id="bbd36-297">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-297">Required</span></span> | <span data-ttu-id="bbd36-298">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-298">Type</span></span> | <span data-ttu-id="bbd36-299">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-299">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-300">stringToConvert</span><span class="sxs-lookup"><span data-stu-id="bbd36-300">stringToConvert</span></span> |<span data-ttu-id="bbd36-301">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-301">Yes</span></span> |<span data-ttu-id="bbd36-302">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-302">string</span></span> |<span data-ttu-id="bbd36-303">Az érték átalakítása egy adat-URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="bbd36-303">The value to convert to a data URI.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-304">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-304">Return value</span></span>

<span data-ttu-id="bbd36-305">Egy karakterláncot formázni egy adat-URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="bbd36-305">A string formatted as a data URI.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-306">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-306">Examples</span></span>

<span data-ttu-id="bbd36-307">A következő példa egy adat-URI azonosító alakít egy értéket, és egy adat-URI azonosító alakít át karakterlánccá:</span><span class="sxs-lookup"><span data-stu-id="bbd36-307">The following example converts a value to a data URI, and converts a data URI to a string:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-308">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-308">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-309">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-309">Name</span></span> | <span data-ttu-id="bbd36-310">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-310">Type</span></span> | <span data-ttu-id="bbd36-311">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-311">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-312">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-312">dataUriOutput</span></span> | <span data-ttu-id="bbd36-313">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-313">String</span></span> | <span data-ttu-id="bbd36-314">adatok: text / egyszerű; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="bbd36-314">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="bbd36-315">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-315">toStringOutput</span></span> | <span data-ttu-id="bbd36-316">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-316">String</span></span> | <span data-ttu-id="bbd36-317">helló világ!</span><span class="sxs-lookup"><span data-stu-id="bbd36-317">Hello, World!</span></span> |

<a id="datauritostring" />

## <a name="datauritostring"></a><span data-ttu-id="bbd36-318">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="bbd36-318">dataUriToString</span></span>
`dataUriToString(dataUriToConvert)`

<span data-ttu-id="bbd36-319">Egy adat-URI azonosító formátuma értékét karakterlánccá.</span><span class="sxs-lookup"><span data-stu-id="bbd36-319">Converts a data URI formatted value to a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-320">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-320">Parameters</span></span>

| <span data-ttu-id="bbd36-321">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-321">Parameter</span></span> | <span data-ttu-id="bbd36-322">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-322">Required</span></span> | <span data-ttu-id="bbd36-323">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-323">Type</span></span> | <span data-ttu-id="bbd36-324">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-324">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-325">dataUriToConvert</span><span class="sxs-lookup"><span data-stu-id="bbd36-325">dataUriToConvert</span></span> |<span data-ttu-id="bbd36-326">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-326">Yes</span></span> |<span data-ttu-id="bbd36-327">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-327">string</span></span> |<span data-ttu-id="bbd36-328">Az adatok URI értéket átalakítani.</span><span class="sxs-lookup"><span data-stu-id="bbd36-328">The data URI value to convert.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-329">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-329">Return value</span></span>

<span data-ttu-id="bbd36-330">A konvertált értéket tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-330">A string containing the converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-331">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-331">Examples</span></span>

<span data-ttu-id="bbd36-332">A következő példa egy adat-URI azonosító alakít egy értéket, és egy adat-URI azonosító alakít át karakterlánccá:</span><span class="sxs-lookup"><span data-stu-id="bbd36-332">The following example converts a value to a data URI, and converts a data URI to a string:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-333">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-333">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-334">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-334">Name</span></span> | <span data-ttu-id="bbd36-335">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-335">Type</span></span> | <span data-ttu-id="bbd36-336">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-336">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-337">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-337">dataUriOutput</span></span> | <span data-ttu-id="bbd36-338">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-338">String</span></span> | <span data-ttu-id="bbd36-339">adatok: text / egyszerű; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="bbd36-339">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="bbd36-340">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-340">toStringOutput</span></span> | <span data-ttu-id="bbd36-341">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-341">String</span></span> | <span data-ttu-id="bbd36-342">helló világ!</span><span class="sxs-lookup"><span data-stu-id="bbd36-342">Hello, World!</span></span> |

<a id="empty" /> 

## <a name="empty"></a><span data-ttu-id="bbd36-343">üres</span><span class="sxs-lookup"><span data-stu-id="bbd36-343">empty</span></span>
`empty(itemToTest)`

<span data-ttu-id="bbd36-344">Meghatározza, hogy egy tömb, az objektum, vagy a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="bbd36-344">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-345">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-345">Parameters</span></span>

| <span data-ttu-id="bbd36-346">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-346">Parameter</span></span> | <span data-ttu-id="bbd36-347">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-347">Required</span></span> | <span data-ttu-id="bbd36-348">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-348">Type</span></span> | <span data-ttu-id="bbd36-349">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-349">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-350">itemToTest</span><span class="sxs-lookup"><span data-stu-id="bbd36-350">itemToTest</span></span> |<span data-ttu-id="bbd36-351">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-351">Yes</span></span> |<span data-ttu-id="bbd36-352">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-352">array, object, or string</span></span> |<span data-ttu-id="bbd36-353">Ellenőrizze a esetén üres érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-353">The value to check if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-354">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-354">Return value</span></span>

<span data-ttu-id="bbd36-355">Beolvasása **igaz** értéke üres, ha sikertelen, ha **hamis**.</span><span class="sxs-lookup"><span data-stu-id="bbd36-355">Returns **True** if the value is empty; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-356">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-356">Examples</span></span>

<span data-ttu-id="bbd36-357">A következő példa ellenőrzi, hogy egy tömb, az objektumot, és a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="bbd36-357">The following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="bbd36-358">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-358">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-359">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-359">Name</span></span> | <span data-ttu-id="bbd36-360">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-360">Type</span></span> | <span data-ttu-id="bbd36-361">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-361">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-362">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="bbd36-362">arrayEmpty</span></span> | <span data-ttu-id="bbd36-363">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-363">Bool</span></span> | <span data-ttu-id="bbd36-364">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-364">True</span></span> |
| <span data-ttu-id="bbd36-365">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="bbd36-365">objectEmpty</span></span> | <span data-ttu-id="bbd36-366">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-366">Bool</span></span> | <span data-ttu-id="bbd36-367">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-367">True</span></span> |
| <span data-ttu-id="bbd36-368">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="bbd36-368">stringEmpty</span></span> | <span data-ttu-id="bbd36-369">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-369">Bool</span></span> | <span data-ttu-id="bbd36-370">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-370">True</span></span> |

<a id="endswith" />

## <a name="endswith"></a><span data-ttu-id="bbd36-371">megadott módon végződő</span><span class="sxs-lookup"><span data-stu-id="bbd36-371">endsWith</span></span>
`endsWith(stringToSearch, stringToFind)`

<span data-ttu-id="bbd36-372">Meghatározza, hogy egy karakterláncot végződik-e értéket.</span><span class="sxs-lookup"><span data-stu-id="bbd36-372">Determines whether a string ends with a value.</span></span> <span data-ttu-id="bbd36-373">Eredményű összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="bbd36-373">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-374">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-374">Parameters</span></span>

| <span data-ttu-id="bbd36-375">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-375">Parameter</span></span> | <span data-ttu-id="bbd36-376">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-376">Required</span></span> | <span data-ttu-id="bbd36-377">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-377">Type</span></span> | <span data-ttu-id="bbd36-378">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-378">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-379">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="bbd36-379">stringToSearch</span></span> |<span data-ttu-id="bbd36-380">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-380">Yes</span></span> |<span data-ttu-id="bbd36-381">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-381">string</span></span> |<span data-ttu-id="bbd36-382">Az érték, amely tartalmazza az elem található.</span><span class="sxs-lookup"><span data-stu-id="bbd36-382">The value that contains the item to find.</span></span> |
| <span data-ttu-id="bbd36-383">stringToFind</span><span class="sxs-lookup"><span data-stu-id="bbd36-383">stringToFind</span></span> |<span data-ttu-id="bbd36-384">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-384">Yes</span></span> |<span data-ttu-id="bbd36-385">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-385">string</span></span> |<span data-ttu-id="bbd36-386">Az érték kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="bbd36-386">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-387">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-387">Return value</span></span>

<span data-ttu-id="bbd36-388">**Igaz** Ha az utolsó karaktereit a karakterlánc megfelel a érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="bbd36-388">**True** if the last character or characters of the string match the value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-389">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-389">Examples</span></span>

<span data-ttu-id="bbd36-390">A következő példa bemutatja, hogyan használja a megadott módon kezdődő és megadott módon végződő függvények:</span><span class="sxs-lookup"><span data-stu-id="bbd36-390">The following example shows how to use the startsWith and endsWith functions:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

<span data-ttu-id="bbd36-391">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-391">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-392">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-392">Name</span></span> | <span data-ttu-id="bbd36-393">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-393">Type</span></span> | <span data-ttu-id="bbd36-394">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-394">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-395">startsTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-395">startsTrue</span></span> | <span data-ttu-id="bbd36-396">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-396">Bool</span></span> | <span data-ttu-id="bbd36-397">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-397">True</span></span> |
| <span data-ttu-id="bbd36-398">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-398">startsCapTrue</span></span> | <span data-ttu-id="bbd36-399">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-399">Bool</span></span> | <span data-ttu-id="bbd36-400">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-400">True</span></span> |
| <span data-ttu-id="bbd36-401">startsFalse</span><span class="sxs-lookup"><span data-stu-id="bbd36-401">startsFalse</span></span> | <span data-ttu-id="bbd36-402">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-402">Bool</span></span> | <span data-ttu-id="bbd36-403">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="bbd36-403">False</span></span> |
| <span data-ttu-id="bbd36-404">endsTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-404">endsTrue</span></span> | <span data-ttu-id="bbd36-405">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-405">Bool</span></span> | <span data-ttu-id="bbd36-406">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-406">True</span></span> |
| <span data-ttu-id="bbd36-407">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-407">endsCapTrue</span></span> | <span data-ttu-id="bbd36-408">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-408">Bool</span></span> | <span data-ttu-id="bbd36-409">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-409">True</span></span> |
| <span data-ttu-id="bbd36-410">endsFalse</span><span class="sxs-lookup"><span data-stu-id="bbd36-410">endsFalse</span></span> | <span data-ttu-id="bbd36-411">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-411">Bool</span></span> | <span data-ttu-id="bbd36-412">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="bbd36-412">False</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="bbd36-413">első</span><span class="sxs-lookup"><span data-stu-id="bbd36-413">first</span></span>
`first(arg1)`

<span data-ttu-id="bbd36-414">A karakterlánc, vagy a tömb első eleme első karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-414">Returns the first character of the string, or first element of the array.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-415">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-415">Parameters</span></span>

| <span data-ttu-id="bbd36-416">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-416">Parameter</span></span> | <span data-ttu-id="bbd36-417">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-417">Required</span></span> | <span data-ttu-id="bbd36-418">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-418">Type</span></span> | <span data-ttu-id="bbd36-419">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-420">arg1</span><span class="sxs-lookup"><span data-stu-id="bbd36-420">arg1</span></span> |<span data-ttu-id="bbd36-421">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-421">Yes</span></span> |<span data-ttu-id="bbd36-422">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-422">array or string</span></span> |<span data-ttu-id="bbd36-423">Az érték első karakter vagy elem lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="bbd36-423">The value to retrieve the first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-424">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-424">Return value</span></span>

<span data-ttu-id="bbd36-425">Egy karakterlánc az első karakter, vagy a tömb első elem típusa (karakterlánc, int, tömb vagy objektum).</span><span class="sxs-lookup"><span data-stu-id="bbd36-425">A string of the first character, or the type (string, int, array, or object) of the first element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-426">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-426">Examples</span></span>

<span data-ttu-id="bbd36-427">A következő példa bemutatja, hogyan használható az első függvényét egy tömb és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-427">The following example shows how to use the first function with an array and string.</span></span>

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

<span data-ttu-id="bbd36-428">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-428">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-429">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-429">Name</span></span> | <span data-ttu-id="bbd36-430">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-430">Type</span></span> | <span data-ttu-id="bbd36-431">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-432">arrayOutput</span></span> | <span data-ttu-id="bbd36-433">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-433">String</span></span> | <span data-ttu-id="bbd36-434">egy</span><span class="sxs-lookup"><span data-stu-id="bbd36-434">one</span></span> |
| <span data-ttu-id="bbd36-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-435">stringOutput</span></span> | <span data-ttu-id="bbd36-436">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-436">String</span></span> | <span data-ttu-id="bbd36-437">O</span><span class="sxs-lookup"><span data-stu-id="bbd36-437">O</span></span> |

<a id="indexof" />

## <a name="indexof"></a><span data-ttu-id="bbd36-438">indexOf</span><span class="sxs-lookup"><span data-stu-id="bbd36-438">indexOf</span></span>
`indexOf(stringToSearch, stringToFind)`

<span data-ttu-id="bbd36-439">Egy értékének a karakterláncon belüli első helyét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-439">Returns the first position of a value within a string.</span></span> <span data-ttu-id="bbd36-440">Eredményű összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="bbd36-440">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-441">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-441">Parameters</span></span>

| <span data-ttu-id="bbd36-442">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-442">Parameter</span></span> | <span data-ttu-id="bbd36-443">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-443">Required</span></span> | <span data-ttu-id="bbd36-444">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-444">Type</span></span> | <span data-ttu-id="bbd36-445">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-445">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-446">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="bbd36-446">stringToSearch</span></span> |<span data-ttu-id="bbd36-447">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-447">Yes</span></span> |<span data-ttu-id="bbd36-448">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-448">string</span></span> |<span data-ttu-id="bbd36-449">Az érték, amely tartalmazza az elem található.</span><span class="sxs-lookup"><span data-stu-id="bbd36-449">The value that contains the item to find.</span></span> |
| <span data-ttu-id="bbd36-450">stringToFind</span><span class="sxs-lookup"><span data-stu-id="bbd36-450">stringToFind</span></span> |<span data-ttu-id="bbd36-451">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-451">Yes</span></span> |<span data-ttu-id="bbd36-452">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-452">string</span></span> |<span data-ttu-id="bbd36-453">Az érték kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="bbd36-453">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-454">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-454">Return value</span></span>

<span data-ttu-id="bbd36-455">Az elem található a pozíció jelölő egész.</span><span class="sxs-lookup"><span data-stu-id="bbd36-455">An integer that represents the position of the item to find.</span></span> <span data-ttu-id="bbd36-456">Az érték nulla alapú.</span><span class="sxs-lookup"><span data-stu-id="bbd36-456">The value is zero-based.</span></span> <span data-ttu-id="bbd36-457">Ha az elem nem található,-1 értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-457">If the item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-458">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-458">Examples</span></span>

<span data-ttu-id="bbd36-459">A következő példa bemutatja, hogyan használja a indexOf és lastIndexOf függvények:</span><span class="sxs-lookup"><span data-stu-id="bbd36-459">The following example shows how to use the indexOf and lastIndexOf functions:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

<span data-ttu-id="bbd36-460">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-460">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-461">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-461">Name</span></span> | <span data-ttu-id="bbd36-462">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-462">Type</span></span> | <span data-ttu-id="bbd36-463">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-463">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-464">firstT</span><span class="sxs-lookup"><span data-stu-id="bbd36-464">firstT</span></span> | <span data-ttu-id="bbd36-465">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-465">Int</span></span> | <span data-ttu-id="bbd36-466">0</span><span class="sxs-lookup"><span data-stu-id="bbd36-466">0</span></span> |
| <span data-ttu-id="bbd36-467">lastT</span><span class="sxs-lookup"><span data-stu-id="bbd36-467">lastT</span></span> | <span data-ttu-id="bbd36-468">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-468">Int</span></span> | <span data-ttu-id="bbd36-469">3</span><span class="sxs-lookup"><span data-stu-id="bbd36-469">3</span></span> |
| <span data-ttu-id="bbd36-470">firstString</span><span class="sxs-lookup"><span data-stu-id="bbd36-470">firstString</span></span> | <span data-ttu-id="bbd36-471">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-471">Int</span></span> | <span data-ttu-id="bbd36-472">2</span><span class="sxs-lookup"><span data-stu-id="bbd36-472">2</span></span> |
| <span data-ttu-id="bbd36-473">lastString</span><span class="sxs-lookup"><span data-stu-id="bbd36-473">lastString</span></span> | <span data-ttu-id="bbd36-474">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-474">Int</span></span> | <span data-ttu-id="bbd36-475">0</span><span class="sxs-lookup"><span data-stu-id="bbd36-475">0</span></span> |
| <span data-ttu-id="bbd36-476">notFound</span><span class="sxs-lookup"><span data-stu-id="bbd36-476">notFound</span></span> | <span data-ttu-id="bbd36-477">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-477">Int</span></span> | <span data-ttu-id="bbd36-478">-1</span><span class="sxs-lookup"><span data-stu-id="bbd36-478">-1</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="bbd36-479">utolsó</span><span class="sxs-lookup"><span data-stu-id="bbd36-479">last</span></span>
`last (arg1)`

<span data-ttu-id="bbd36-480">Az utolsó karakter a karakterlánc vagy tömb utolsó eleme adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-480">Returns last character of the string, or the last element of the array.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-481">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-481">Parameters</span></span>

| <span data-ttu-id="bbd36-482">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-482">Parameter</span></span> | <span data-ttu-id="bbd36-483">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-483">Required</span></span> | <span data-ttu-id="bbd36-484">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-484">Type</span></span> | <span data-ttu-id="bbd36-485">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-485">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-486">arg1</span><span class="sxs-lookup"><span data-stu-id="bbd36-486">arg1</span></span> |<span data-ttu-id="bbd36-487">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-487">Yes</span></span> |<span data-ttu-id="bbd36-488">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-488">array or string</span></span> |<span data-ttu-id="bbd36-489">Az érték utolsó karakter vagy elem lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="bbd36-489">The value to retrieve the last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-490">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-490">Return value</span></span>

<span data-ttu-id="bbd36-491">Az utolsó karakter, vagy a tömb utolsó eleme típusa (karakterlánc, int, tömb vagy objektum) formátumú karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-491">A string of the last character, or the type (string, int, array, or object) of the last element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-492">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-492">Examples</span></span>

<span data-ttu-id="bbd36-493">A következő példa bemutatja, hogyan használható az utolsó függvény egy tömb és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-493">The following example shows how to use the last function with an array and string.</span></span>

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

<span data-ttu-id="bbd36-494">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-494">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-495">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-495">Name</span></span> | <span data-ttu-id="bbd36-496">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-496">Type</span></span> | <span data-ttu-id="bbd36-497">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-497">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-498">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-498">arrayOutput</span></span> | <span data-ttu-id="bbd36-499">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-499">String</span></span> | <span data-ttu-id="bbd36-500">három</span><span class="sxs-lookup"><span data-stu-id="bbd36-500">three</span></span> |
| <span data-ttu-id="bbd36-501">stringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-501">stringOutput</span></span> | <span data-ttu-id="bbd36-502">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-502">String</span></span> | <span data-ttu-id="bbd36-503">E</span><span class="sxs-lookup"><span data-stu-id="bbd36-503">e</span></span> |

<a id="lastindexof" />

## <a name="lastindexof"></a><span data-ttu-id="bbd36-504">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="bbd36-504">lastIndexOf</span></span>
`lastIndexOf(stringToSearch, stringToFind)`

<span data-ttu-id="bbd36-505">Egy értékének a karakterláncon belüli utolsó helyét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-505">Returns the last position of a value within a string.</span></span> <span data-ttu-id="bbd36-506">Eredményű összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="bbd36-506">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-507">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-507">Parameters</span></span>

| <span data-ttu-id="bbd36-508">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-508">Parameter</span></span> | <span data-ttu-id="bbd36-509">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-509">Required</span></span> | <span data-ttu-id="bbd36-510">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-510">Type</span></span> | <span data-ttu-id="bbd36-511">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-511">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-512">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="bbd36-512">stringToSearch</span></span> |<span data-ttu-id="bbd36-513">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-513">Yes</span></span> |<span data-ttu-id="bbd36-514">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-514">string</span></span> |<span data-ttu-id="bbd36-515">Az érték, amely tartalmazza az elem található.</span><span class="sxs-lookup"><span data-stu-id="bbd36-515">The value that contains the item to find.</span></span> |
| <span data-ttu-id="bbd36-516">stringToFind</span><span class="sxs-lookup"><span data-stu-id="bbd36-516">stringToFind</span></span> |<span data-ttu-id="bbd36-517">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-517">Yes</span></span> |<span data-ttu-id="bbd36-518">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-518">string</span></span> |<span data-ttu-id="bbd36-519">Az érték kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="bbd36-519">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-520">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-520">Return value</span></span>

<span data-ttu-id="bbd36-521">Az elem található utolsó pozícióját jelölő egész.</span><span class="sxs-lookup"><span data-stu-id="bbd36-521">An integer that represents the last position of the item to find.</span></span> <span data-ttu-id="bbd36-522">Az érték nulla alapú.</span><span class="sxs-lookup"><span data-stu-id="bbd36-522">The value is zero-based.</span></span> <span data-ttu-id="bbd36-523">Ha az elem nem található,-1 értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-523">If the item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-524">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-524">Examples</span></span>

<span data-ttu-id="bbd36-525">A következő példa bemutatja, hogyan használja a indexOf és lastIndexOf függvények:</span><span class="sxs-lookup"><span data-stu-id="bbd36-525">The following example shows how to use the indexOf and lastIndexOf functions:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

<span data-ttu-id="bbd36-526">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-526">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-527">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-527">Name</span></span> | <span data-ttu-id="bbd36-528">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-528">Type</span></span> | <span data-ttu-id="bbd36-529">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-529">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-530">firstT</span><span class="sxs-lookup"><span data-stu-id="bbd36-530">firstT</span></span> | <span data-ttu-id="bbd36-531">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-531">Int</span></span> | <span data-ttu-id="bbd36-532">0</span><span class="sxs-lookup"><span data-stu-id="bbd36-532">0</span></span> |
| <span data-ttu-id="bbd36-533">lastT</span><span class="sxs-lookup"><span data-stu-id="bbd36-533">lastT</span></span> | <span data-ttu-id="bbd36-534">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-534">Int</span></span> | <span data-ttu-id="bbd36-535">3</span><span class="sxs-lookup"><span data-stu-id="bbd36-535">3</span></span> |
| <span data-ttu-id="bbd36-536">firstString</span><span class="sxs-lookup"><span data-stu-id="bbd36-536">firstString</span></span> | <span data-ttu-id="bbd36-537">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-537">Int</span></span> | <span data-ttu-id="bbd36-538">2</span><span class="sxs-lookup"><span data-stu-id="bbd36-538">2</span></span> |
| <span data-ttu-id="bbd36-539">lastString</span><span class="sxs-lookup"><span data-stu-id="bbd36-539">lastString</span></span> | <span data-ttu-id="bbd36-540">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-540">Int</span></span> | <span data-ttu-id="bbd36-541">0</span><span class="sxs-lookup"><span data-stu-id="bbd36-541">0</span></span> |
| <span data-ttu-id="bbd36-542">notFound</span><span class="sxs-lookup"><span data-stu-id="bbd36-542">notFound</span></span> | <span data-ttu-id="bbd36-543">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-543">Int</span></span> | <span data-ttu-id="bbd36-544">-1</span><span class="sxs-lookup"><span data-stu-id="bbd36-544">-1</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="bbd36-545">Hossza</span><span class="sxs-lookup"><span data-stu-id="bbd36-545">length</span></span>
`length(string)`

<span data-ttu-id="bbd36-546">A karakterlánc vagy tömb elemeinek a számú karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-546">Returns the number of characters in a string, or elements in an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-547">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-547">Parameters</span></span>

| <span data-ttu-id="bbd36-548">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-548">Parameter</span></span> | <span data-ttu-id="bbd36-549">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-549">Required</span></span> | <span data-ttu-id="bbd36-550">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-550">Type</span></span> | <span data-ttu-id="bbd36-551">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-551">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-552">arg1</span><span class="sxs-lookup"><span data-stu-id="bbd36-552">arg1</span></span> |<span data-ttu-id="bbd36-553">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-553">Yes</span></span> |<span data-ttu-id="bbd36-554">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-554">array or string</span></span> |<span data-ttu-id="bbd36-555">A tömb használni az első elemek, vagy a karakterlánc első karakterek használata.</span><span class="sxs-lookup"><span data-stu-id="bbd36-555">The array to use for getting the number of elements, or the string to use for getting the number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-556">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-556">Return value</span></span>

<span data-ttu-id="bbd36-557">Egy egész szám.</span><span class="sxs-lookup"><span data-stu-id="bbd36-557">An int.</span></span> 

### <a name="examples"></a><span data-ttu-id="bbd36-558">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-558">Examples</span></span>

<span data-ttu-id="bbd36-559">A következő példa bemutatja, hogyan egy tömb és a karakterlánc hossza használhat:</span><span class="sxs-lookup"><span data-stu-id="bbd36-559">The following example shows how to use length with an array and string:</span></span>

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

<span data-ttu-id="bbd36-560">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-560">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-561">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-561">Name</span></span> | <span data-ttu-id="bbd36-562">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-562">Type</span></span> | <span data-ttu-id="bbd36-563">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-563">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-564">arrayLength</span><span class="sxs-lookup"><span data-stu-id="bbd36-564">arrayLength</span></span> | <span data-ttu-id="bbd36-565">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-565">Int</span></span> | <span data-ttu-id="bbd36-566">3</span><span class="sxs-lookup"><span data-stu-id="bbd36-566">3</span></span> |
| <span data-ttu-id="bbd36-567">stringLength</span><span class="sxs-lookup"><span data-stu-id="bbd36-567">stringLength</span></span> | <span data-ttu-id="bbd36-568">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-568">Int</span></span> | <span data-ttu-id="bbd36-569">13</span><span class="sxs-lookup"><span data-stu-id="bbd36-569">13</span></span> |

<a id="padleft" />

## <a name="padleft"></a><span data-ttu-id="bbd36-570">PadLeft</span><span class="sxs-lookup"><span data-stu-id="bbd36-570">padLeft</span></span>
`padLeft(valueToPad, totalLength, paddingCharacter)`

<span data-ttu-id="bbd36-571">Karakterek hozzáadásával a bal oldali az összes megadott hosszúságú eléréséig jobbra igazított karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-571">Returns a right-aligned string by adding characters to the left until reaching the total specified length.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-572">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-572">Parameters</span></span>

| <span data-ttu-id="bbd36-573">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-573">Parameter</span></span> | <span data-ttu-id="bbd36-574">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-574">Required</span></span> | <span data-ttu-id="bbd36-575">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-575">Type</span></span> | <span data-ttu-id="bbd36-576">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-576">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-577">valueToPad</span><span class="sxs-lookup"><span data-stu-id="bbd36-577">valueToPad</span></span> |<span data-ttu-id="bbd36-578">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-578">Yes</span></span> |<span data-ttu-id="bbd36-579">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="bbd36-579">string or int</span></span> |<span data-ttu-id="bbd36-580">A jobbra igazított érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-580">The value to right-align.</span></span> |
| <span data-ttu-id="bbd36-581">totalLength</span><span class="sxs-lookup"><span data-stu-id="bbd36-581">totalLength</span></span> |<span data-ttu-id="bbd36-582">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-582">Yes</span></span> |<span data-ttu-id="bbd36-583">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-583">int</span></span> |<span data-ttu-id="bbd36-584">A visszaadott karakterláncban szereplő karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="bbd36-584">The total number of characters in the returned string.</span></span> |
| <span data-ttu-id="bbd36-585">paddingCharacter</span><span class="sxs-lookup"><span data-stu-id="bbd36-585">paddingCharacter</span></span> |<span data-ttu-id="bbd36-586">Nem</span><span class="sxs-lookup"><span data-stu-id="bbd36-586">No</span></span> |<span data-ttu-id="bbd36-587">egyetlen karakter</span><span class="sxs-lookup"><span data-stu-id="bbd36-587">single character</span></span> |<span data-ttu-id="bbd36-588">Bal-kitöltési, amíg a teljes hossza nem csökken használandó karakter.</span><span class="sxs-lookup"><span data-stu-id="bbd36-588">The character to use for left-padding until the total length is reached.</span></span> <span data-ttu-id="bbd36-589">Az alapértelmezett érték: szóközzel.</span><span class="sxs-lookup"><span data-stu-id="bbd36-589">The default value is a space.</span></span> |

<span data-ttu-id="bbd36-590">Ha az eredeti karakterláncot elválasztásához karakterek áll, akkor egyetlen karakter kerülnek.</span><span class="sxs-lookup"><span data-stu-id="bbd36-590">If the original string is longer than the number of characters to pad, no characters are added.</span></span>

### <a name="return-value"></a><span data-ttu-id="bbd36-591">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-591">Return value</span></span>

<span data-ttu-id="bbd36-592">A karakterlánc a legalább a megadott karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="bbd36-592">A string with at least the number of specified characters.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-593">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-593">Examples</span></span>

<span data-ttu-id="bbd36-594">A következő példa bemutatja, hogyan elválasztásához a felhasználó által megadott paraméter értéke nulla karakter hozzáadásával, amíg eléri a karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="bbd36-594">The following example shows how to pad the user-provided parameter value by adding the zero character until it reaches the total number of characters.</span></span> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

<span data-ttu-id="bbd36-595">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-595">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-596">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-596">Name</span></span> | <span data-ttu-id="bbd36-597">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-597">Type</span></span> | <span data-ttu-id="bbd36-598">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-598">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-599">stringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-599">stringOutput</span></span> | <span data-ttu-id="bbd36-600">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-600">String</span></span> | <span data-ttu-id="bbd36-601">0000000123</span><span class="sxs-lookup"><span data-stu-id="bbd36-601">0000000123</span></span> |

<a id="replace" />

## <a name="replace"></a><span data-ttu-id="bbd36-602">cserélje le</span><span class="sxs-lookup"><span data-stu-id="bbd36-602">replace</span></span>
`replace(originalString, oldString, newString)`

<span data-ttu-id="bbd36-603">Egy másik karakterlánc szerepét egy karakterlánc összes előfordulásának új karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-603">Returns a new string with all instances of one string replaced by another string.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-604">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-604">Parameters</span></span>

| <span data-ttu-id="bbd36-605">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-605">Parameter</span></span> | <span data-ttu-id="bbd36-606">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-606">Required</span></span> | <span data-ttu-id="bbd36-607">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-607">Type</span></span> | <span data-ttu-id="bbd36-608">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-608">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-609">originalString</span><span class="sxs-lookup"><span data-stu-id="bbd36-609">originalString</span></span> |<span data-ttu-id="bbd36-610">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-610">Yes</span></span> |<span data-ttu-id="bbd36-611">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-611">string</span></span> |<span data-ttu-id="bbd36-612">Az érték, amelynek egy karakterlánc másik karakterlánc szerepét összes példányát.</span><span class="sxs-lookup"><span data-stu-id="bbd36-612">The value that has all instances of one string replaced by another string.</span></span> |
| <span data-ttu-id="bbd36-613">oldString</span><span class="sxs-lookup"><span data-stu-id="bbd36-613">oldString</span></span> |<span data-ttu-id="bbd36-614">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-614">Yes</span></span> |<span data-ttu-id="bbd36-615">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-615">string</span></span> |<span data-ttu-id="bbd36-616">Az eredeti karakterláncot eltávolítja a karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bbd36-616">The string to be removed from the original string.</span></span> |
| <span data-ttu-id="bbd36-617">newString</span><span class="sxs-lookup"><span data-stu-id="bbd36-617">newString</span></span> |<span data-ttu-id="bbd36-618">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-618">Yes</span></span> |<span data-ttu-id="bbd36-619">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-619">string</span></span> |<span data-ttu-id="bbd36-620">A karakterlánc helyett, az eltávolított karakterlánc hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="bbd36-620">The string to add in place of the removed string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-621">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-621">Return value</span></span>

<span data-ttu-id="bbd36-622">A kicserélt karaktert tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-622">A string with the replaced characters.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-623">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-623">Examples</span></span>

<span data-ttu-id="bbd36-624">A következő példa bemutatja, távolítsa el az összes kötőjelek a felhasználó által megadott karakterláncot, és a karakterlánc egy részét lecseréli egy másik karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bbd36-624">The following example shows how to remove all dashes from the user-provided string, and how to replace part of the string with another string.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secodeOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

<span data-ttu-id="bbd36-625">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-625">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-626">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-626">Name</span></span> | <span data-ttu-id="bbd36-627">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-627">Type</span></span> | <span data-ttu-id="bbd36-628">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-628">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-629">firstOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-629">firstOutput</span></span> | <span data-ttu-id="bbd36-630">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-630">String</span></span> | <span data-ttu-id="bbd36-631">1231231234</span><span class="sxs-lookup"><span data-stu-id="bbd36-631">1231231234</span></span> |
| <span data-ttu-id="bbd36-632">secodeOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-632">secodeOutput</span></span> | <span data-ttu-id="bbd36-633">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-633">String</span></span> | <span data-ttu-id="bbd36-634">123-123-xxxx</span><span class="sxs-lookup"><span data-stu-id="bbd36-634">123-123-xxxx</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="bbd36-635">Kihagyása</span><span class="sxs-lookup"><span data-stu-id="bbd36-635">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="bbd36-636">Egy karakterláncot ad vissza az összes karakter után a megadott számú karaktert, vagy az összes elem tömb után a megadott számú elemet.</span><span class="sxs-lookup"><span data-stu-id="bbd36-636">Returns a string with all the characters after the specified number of characters, or an array with all the elements after the specified number of elements.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-637">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-637">Parameters</span></span>

| <span data-ttu-id="bbd36-638">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-638">Parameter</span></span> | <span data-ttu-id="bbd36-639">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-639">Required</span></span> | <span data-ttu-id="bbd36-640">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-640">Type</span></span> | <span data-ttu-id="bbd36-641">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-641">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-642">originalValue</span><span class="sxs-lookup"><span data-stu-id="bbd36-642">originalValue</span></span> |<span data-ttu-id="bbd36-643">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-643">Yes</span></span> |<span data-ttu-id="bbd36-644">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-644">array or string</span></span> |<span data-ttu-id="bbd36-645">A tömb vagy karakterlánc kihagyása használandó.</span><span class="sxs-lookup"><span data-stu-id="bbd36-645">The array or string to use for skipping.</span></span> |
| <span data-ttu-id="bbd36-646">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="bbd36-646">numberToSkip</span></span> |<span data-ttu-id="bbd36-647">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-647">Yes</span></span> |<span data-ttu-id="bbd36-648">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-648">int</span></span> |<span data-ttu-id="bbd36-649">Elemek vagy kihagyását karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="bbd36-649">The number of elements or characters to skip.</span></span> <span data-ttu-id="bbd36-650">Ha ez az érték 0 vagy kisebb, a elemek vagy az karaktere adott vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-650">If this value is 0 or less, all the elements or characters in the value are returned.</span></span> <span data-ttu-id="bbd36-651">Ha a tömb vagy karakterlánc hossza nagyobb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-651">If it is larger than the length of the array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-652">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-652">Return value</span></span>

<span data-ttu-id="bbd36-653">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-653">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-654">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-654">Examples</span></span>

<span data-ttu-id="bbd36-655">Az alábbi példa kihagyja a megadott számú elemet a tömbben, és a megadott számú karaktert egy karakterláncon belül.</span><span class="sxs-lookup"><span data-stu-id="bbd36-655">The following example skips the specified number of elements in the array, and the specified number of characters in a string.</span></span>

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

<span data-ttu-id="bbd36-656">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-656">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-657">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-657">Name</span></span> | <span data-ttu-id="bbd36-658">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-658">Type</span></span> | <span data-ttu-id="bbd36-659">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-659">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-660">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-660">arrayOutput</span></span> | <span data-ttu-id="bbd36-661">A tömb</span><span class="sxs-lookup"><span data-stu-id="bbd36-661">Array</span></span> | <span data-ttu-id="bbd36-662">["három"]</span><span class="sxs-lookup"><span data-stu-id="bbd36-662">["three"]</span></span> |
| <span data-ttu-id="bbd36-663">stringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-663">stringOutput</span></span> | <span data-ttu-id="bbd36-664">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-664">String</span></span> | <span data-ttu-id="bbd36-665">két három</span><span class="sxs-lookup"><span data-stu-id="bbd36-665">two three</span></span> |

<a id="split" />

## <a name="split"></a><span data-ttu-id="bbd36-666">felosztás</span><span class="sxs-lookup"><span data-stu-id="bbd36-666">split</span></span>
`split(inputString, delimiter)`

<span data-ttu-id="bbd36-667">Tartalmazza a karakterláncrészletet tartalmazza a bemeneti karakterlánc, amely a megadott elválasztók határolja karakterláncok tömbjét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-667">Returns an array of strings that contains the substrings of the input string that are delimited by the specified delimiters.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-668">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-668">Parameters</span></span>

| <span data-ttu-id="bbd36-669">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-669">Parameter</span></span> | <span data-ttu-id="bbd36-670">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-670">Required</span></span> | <span data-ttu-id="bbd36-671">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-671">Type</span></span> | <span data-ttu-id="bbd36-672">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-672">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-673">inputString</span><span class="sxs-lookup"><span data-stu-id="bbd36-673">inputString</span></span> |<span data-ttu-id="bbd36-674">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-674">Yes</span></span> |<span data-ttu-id="bbd36-675">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-675">string</span></span> |<span data-ttu-id="bbd36-676">Ossza fel a karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bbd36-676">The string to split.</span></span> |
| <span data-ttu-id="bbd36-677">Elválasztó</span><span class="sxs-lookup"><span data-stu-id="bbd36-677">delimiter</span></span> |<span data-ttu-id="bbd36-678">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-678">Yes</span></span> |<span data-ttu-id="bbd36-679">karakterláncot vagy karakterláncok</span><span class="sxs-lookup"><span data-stu-id="bbd36-679">string or array of strings</span></span> |<span data-ttu-id="bbd36-680">A karakterlánc a felosztás használandó elválasztó.</span><span class="sxs-lookup"><span data-stu-id="bbd36-680">The delimiter to use for splitting the string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-681">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-681">Return value</span></span>

<span data-ttu-id="bbd36-682">Karakterláncokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="bbd36-682">An array of strings.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-683">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-683">Examples</span></span>

<span data-ttu-id="bbd36-684">Az alábbi példa felosztja a bemeneti karakterlánc egy vesszővel válassza el egymástól, és vesszővel vagy pontosvesszővel kell elválasztani őket.</span><span class="sxs-lookup"><span data-stu-id="bbd36-684">The following example splits the input string with a comma, and with either a comma or a semi-colon.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-685">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-685">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-686">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-686">Name</span></span> | <span data-ttu-id="bbd36-687">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-687">Type</span></span> | <span data-ttu-id="bbd36-688">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-688">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-689">firstOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-689">firstOutput</span></span> | <span data-ttu-id="bbd36-690">A tömb</span><span class="sxs-lookup"><span data-stu-id="bbd36-690">Array</span></span> | <span data-ttu-id="bbd36-691">["egy", "két", "három"]</span><span class="sxs-lookup"><span data-stu-id="bbd36-691">["one", "two", "three"]</span></span> |
| <span data-ttu-id="bbd36-692">secondOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-692">secondOutput</span></span> | <span data-ttu-id="bbd36-693">A tömb</span><span class="sxs-lookup"><span data-stu-id="bbd36-693">Array</span></span> | <span data-ttu-id="bbd36-694">["egy", "két", "három"]</span><span class="sxs-lookup"><span data-stu-id="bbd36-694">["one", "two", "three"]</span></span> |

<a id="startswith" />

## <a name="startswith"></a><span data-ttu-id="bbd36-695">startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="bbd36-695">startsWith</span></span>
`startsWith(stringToSearch, stringToFind)`

<span data-ttu-id="bbd36-696">Meghatározza, hogy egy karakterlánc értékkel kezdődik-e.</span><span class="sxs-lookup"><span data-stu-id="bbd36-696">Determines whether a string starts with a value.</span></span> <span data-ttu-id="bbd36-697">Eredményű összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="bbd36-697">The comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-698">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-698">Parameters</span></span>

| <span data-ttu-id="bbd36-699">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-699">Parameter</span></span> | <span data-ttu-id="bbd36-700">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-700">Required</span></span> | <span data-ttu-id="bbd36-701">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-701">Type</span></span> | <span data-ttu-id="bbd36-702">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-702">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-703">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="bbd36-703">stringToSearch</span></span> |<span data-ttu-id="bbd36-704">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-704">Yes</span></span> |<span data-ttu-id="bbd36-705">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-705">string</span></span> |<span data-ttu-id="bbd36-706">Az érték, amely tartalmazza az elem található.</span><span class="sxs-lookup"><span data-stu-id="bbd36-706">The value that contains the item to find.</span></span> |
| <span data-ttu-id="bbd36-707">stringToFind</span><span class="sxs-lookup"><span data-stu-id="bbd36-707">stringToFind</span></span> |<span data-ttu-id="bbd36-708">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-708">Yes</span></span> |<span data-ttu-id="bbd36-709">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-709">string</span></span> |<span data-ttu-id="bbd36-710">Az érték kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="bbd36-710">The value to find.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-711">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-711">Return value</span></span>

<span data-ttu-id="bbd36-712">**Igaz** Ha az első karaktereit a karakterlánc megfelel a érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="bbd36-712">**True** if the first character or characters of the string match the value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-713">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-713">Examples</span></span>

<span data-ttu-id="bbd36-714">A következő példa bemutatja, hogyan használja a megadott módon kezdődő és megadott módon végződő függvények:</span><span class="sxs-lookup"><span data-stu-id="bbd36-714">The following example shows how to use the startsWith and endsWith functions:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

<span data-ttu-id="bbd36-715">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-715">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-716">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-716">Name</span></span> | <span data-ttu-id="bbd36-717">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-717">Type</span></span> | <span data-ttu-id="bbd36-718">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-718">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-719">startsTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-719">startsTrue</span></span> | <span data-ttu-id="bbd36-720">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-720">Bool</span></span> | <span data-ttu-id="bbd36-721">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-721">True</span></span> |
| <span data-ttu-id="bbd36-722">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-722">startsCapTrue</span></span> | <span data-ttu-id="bbd36-723">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-723">Bool</span></span> | <span data-ttu-id="bbd36-724">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-724">True</span></span> |
| <span data-ttu-id="bbd36-725">startsFalse</span><span class="sxs-lookup"><span data-stu-id="bbd36-725">startsFalse</span></span> | <span data-ttu-id="bbd36-726">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-726">Bool</span></span> | <span data-ttu-id="bbd36-727">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="bbd36-727">False</span></span> |
| <span data-ttu-id="bbd36-728">endsTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-728">endsTrue</span></span> | <span data-ttu-id="bbd36-729">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-729">Bool</span></span> | <span data-ttu-id="bbd36-730">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-730">True</span></span> |
| <span data-ttu-id="bbd36-731">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="bbd36-731">endsCapTrue</span></span> | <span data-ttu-id="bbd36-732">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-732">Bool</span></span> | <span data-ttu-id="bbd36-733">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="bbd36-733">True</span></span> |
| <span data-ttu-id="bbd36-734">endsFalse</span><span class="sxs-lookup"><span data-stu-id="bbd36-734">endsFalse</span></span> | <span data-ttu-id="bbd36-735">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-735">Bool</span></span> | <span data-ttu-id="bbd36-736">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="bbd36-736">False</span></span> |

<a id="string" />

## <a name="string"></a><span data-ttu-id="bbd36-737">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-737">string</span></span>
`string(valueToConvert)`

<span data-ttu-id="bbd36-738">A megadott érték konvertálása egy karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bbd36-738">Converts the specified value to a string.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-739">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-739">Parameters</span></span>

| <span data-ttu-id="bbd36-740">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-740">Parameter</span></span> | <span data-ttu-id="bbd36-741">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-741">Required</span></span> | <span data-ttu-id="bbd36-742">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-742">Type</span></span> | <span data-ttu-id="bbd36-743">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-743">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-744">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="bbd36-744">valueToConvert</span></span> |<span data-ttu-id="bbd36-745">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-745">Yes</span></span> | <span data-ttu-id="bbd36-746">Bármelyik</span><span class="sxs-lookup"><span data-stu-id="bbd36-746">Any</span></span> |<span data-ttu-id="bbd36-747">Az érték karakterlánccá konvertálni.</span><span class="sxs-lookup"><span data-stu-id="bbd36-747">The value to convert to string.</span></span> <span data-ttu-id="bbd36-748">Bármely típusú érték lehet konvertálni, többek között az objektumok és tömböket.</span><span class="sxs-lookup"><span data-stu-id="bbd36-748">Any type of value can be converted, including objects and arrays.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-749">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-749">Return value</span></span>

<span data-ttu-id="bbd36-750">Az átalakított érték karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-750">A string of the converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-751">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-751">Examples</span></span>

<span data-ttu-id="bbd36-752">A következő példa bemutatja, hogyan különböző típusú értékeket átalakítani karakterláncok:</span><span class="sxs-lookup"><span data-stu-id="bbd36-752">The following example shows how to convert different types of values to strings:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-753">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-753">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-754">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-754">Name</span></span> | <span data-ttu-id="bbd36-755">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-755">Type</span></span> | <span data-ttu-id="bbd36-756">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-756">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-757">objectOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-757">objectOutput</span></span> | <span data-ttu-id="bbd36-758">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-758">String</span></span> | <span data-ttu-id="bbd36-759">{"valueA": 10, "valueB": "Példaszöveg"}</span><span class="sxs-lookup"><span data-stu-id="bbd36-759">{"valueA":10,"valueB":"Example Text"}</span></span> |
| <span data-ttu-id="bbd36-760">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-760">arrayOutput</span></span> | <span data-ttu-id="bbd36-761">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-761">String</span></span> | <span data-ttu-id="bbd36-762">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="bbd36-762">["a","b","c"]</span></span> |
| <span data-ttu-id="bbd36-763">intOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-763">intOutput</span></span> | <span data-ttu-id="bbd36-764">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-764">String</span></span> | <span data-ttu-id="bbd36-765">5</span><span class="sxs-lookup"><span data-stu-id="bbd36-765">5</span></span> |

<a id="substring" />

## <a name="substring"></a><span data-ttu-id="bbd36-766">Substring</span><span class="sxs-lookup"><span data-stu-id="bbd36-766">substring</span></span>
`substring(stringToParse, startIndex, length)`

<span data-ttu-id="bbd36-767">Egy részét, amely a megadott Karakterpozíció kezdődik, és tartalmazza a megadott számú karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-767">Returns a substring that starts at the specified character position and contains the specified number of characters.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-768">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-768">Parameters</span></span>

| <span data-ttu-id="bbd36-769">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-769">Parameter</span></span> | <span data-ttu-id="bbd36-770">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-770">Required</span></span> | <span data-ttu-id="bbd36-771">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-771">Type</span></span> | <span data-ttu-id="bbd36-772">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-772">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-773">stringToParse</span><span class="sxs-lookup"><span data-stu-id="bbd36-773">stringToParse</span></span> |<span data-ttu-id="bbd36-774">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-774">Yes</span></span> |<span data-ttu-id="bbd36-775">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-775">string</span></span> |<span data-ttu-id="bbd36-776">Az eredeti karakterláncot, amely a karakterláncrészletet ki kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="bbd36-776">The original string from which the substring is extracted.</span></span> |
| <span data-ttu-id="bbd36-777">startIndex</span><span class="sxs-lookup"><span data-stu-id="bbd36-777">startIndex</span></span> |<span data-ttu-id="bbd36-778">Nem</span><span class="sxs-lookup"><span data-stu-id="bbd36-778">No</span></span> |<span data-ttu-id="bbd36-779">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-779">int</span></span> |<span data-ttu-id="bbd36-780">A nulla alapú karakter kezdőpozíciója a karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="bbd36-780">The zero-based starting character position for the substring.</span></span> |
| <span data-ttu-id="bbd36-781">Hossza</span><span class="sxs-lookup"><span data-stu-id="bbd36-781">length</span></span> |<span data-ttu-id="bbd36-782">Nem</span><span class="sxs-lookup"><span data-stu-id="bbd36-782">No</span></span> |<span data-ttu-id="bbd36-783">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-783">int</span></span> |<span data-ttu-id="bbd36-784">A substring karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="bbd36-784">The number of characters for the substring.</span></span> <span data-ttu-id="bbd36-785">A karakterláncon belüli helyet kell képviselnie.</span><span class="sxs-lookup"><span data-stu-id="bbd36-785">Must refer to a location within the string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-786">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-786">Return value</span></span>

<span data-ttu-id="bbd36-787">A karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="bbd36-787">The substring.</span></span>

### <a name="remarks"></a><span data-ttu-id="bbd36-788">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bbd36-788">Remarks</span></span>

<span data-ttu-id="bbd36-789">A parancs nem működik, amikor a substring túlnyúlik a karakterlánc végén.</span><span class="sxs-lookup"><span data-stu-id="bbd36-789">The function fails when the substring extends beyond the end of the string.</span></span> <span data-ttu-id="bbd36-790">Hibával meghiúsul a következő példa "az index és length paraméterek a karakterláncon belüli helyet kell képviselnie.</span><span class="sxs-lookup"><span data-stu-id="bbd36-790">The following example fails with the error "The index and length parameters must refer to a location within the string.</span></span> <span data-ttu-id="bbd36-791">Az index paraméter: "0", a length paraméter: "11", a karakterlánc-paraméter hossza: "10". ".</span><span class="sxs-lookup"><span data-stu-id="bbd36-791">The index parameter: '0', the length parameter: '11', the length of the string parameter: '10'.".</span></span>

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a><span data-ttu-id="bbd36-792">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-792">Examples</span></span>

<span data-ttu-id="bbd36-793">A következő példa egy substring kiolvassa a paramétert.</span><span class="sxs-lookup"><span data-stu-id="bbd36-793">The following example extracts a substring from a parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

<span data-ttu-id="bbd36-794">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-794">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-795">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-795">Name</span></span> | <span data-ttu-id="bbd36-796">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-796">Type</span></span> | <span data-ttu-id="bbd36-797">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-797">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-798">substringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-798">substringOutput</span></span> | <span data-ttu-id="bbd36-799">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-799">String</span></span> | <span data-ttu-id="bbd36-800">két</span><span class="sxs-lookup"><span data-stu-id="bbd36-800">two</span></span> |


<a id="take" />

## <a name="take"></a><span data-ttu-id="bbd36-801">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="bbd36-801">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="bbd36-802">A karakterlánc vagy tömb a megadott számú elemet a tömb kezdetétől a kezdetétől a megadott számú karaktert karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-802">Returns a string with the specified number of characters from the start of the string, or an array with the specified number of elements from the start of the array.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-803">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-803">Parameters</span></span>

| <span data-ttu-id="bbd36-804">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-804">Parameter</span></span> | <span data-ttu-id="bbd36-805">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-805">Required</span></span> | <span data-ttu-id="bbd36-806">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-806">Type</span></span> | <span data-ttu-id="bbd36-807">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-807">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-808">originalValue</span><span class="sxs-lookup"><span data-stu-id="bbd36-808">originalValue</span></span> |<span data-ttu-id="bbd36-809">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-809">Yes</span></span> |<span data-ttu-id="bbd36-810">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-810">array or string</span></span> |<span data-ttu-id="bbd36-811">A tömb vagy karakterlánc az elemek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="bbd36-811">The array or string to take the elements from.</span></span> |
| <span data-ttu-id="bbd36-812">numberToTake</span><span class="sxs-lookup"><span data-stu-id="bbd36-812">numberToTake</span></span> |<span data-ttu-id="bbd36-813">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-813">Yes</span></span> |<span data-ttu-id="bbd36-814">int</span><span class="sxs-lookup"><span data-stu-id="bbd36-814">int</span></span> |<span data-ttu-id="bbd36-815">Elemek és érvénybe karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="bbd36-815">The number of elements or characters to take.</span></span> <span data-ttu-id="bbd36-816">Ha ez az érték 0 vagy kisebb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bbd36-816">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="bbd36-817">Ha a megadott tömb vagy karakterlánc hossza nagyobb, a tömb vagy karakterlánc összes elemet visszaadja a.</span><span class="sxs-lookup"><span data-stu-id="bbd36-817">If it is larger than the length of the given array or string, all the elements in the array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-818">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-818">Return value</span></span>

<span data-ttu-id="bbd36-819">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-819">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-820">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-820">Examples</span></span>

<span data-ttu-id="bbd36-821">A következő példa a tömb elemei, és a karakterek egy karakterlánc megadott számú vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="bbd36-821">The following example takes the specified number of elements from the array, and characters from a string.</span></span>

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

<span data-ttu-id="bbd36-822">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-822">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-823">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-823">Name</span></span> | <span data-ttu-id="bbd36-824">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-824">Type</span></span> | <span data-ttu-id="bbd36-825">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-825">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-826">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-826">arrayOutput</span></span> | <span data-ttu-id="bbd36-827">A tömb</span><span class="sxs-lookup"><span data-stu-id="bbd36-827">Array</span></span> | <span data-ttu-id="bbd36-828">["egy", "két"]</span><span class="sxs-lookup"><span data-stu-id="bbd36-828">["one", "two"]</span></span> |
| <span data-ttu-id="bbd36-829">stringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-829">stringOutput</span></span> | <span data-ttu-id="bbd36-830">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-830">String</span></span> | <span data-ttu-id="bbd36-831">a</span><span class="sxs-lookup"><span data-stu-id="bbd36-831">on</span></span> |

<a id="tolower" />

## <a name="tolower"></a><span data-ttu-id="bbd36-832">toLower</span><span class="sxs-lookup"><span data-stu-id="bbd36-832">toLower</span></span>
`toLower(stringToChange)`

<span data-ttu-id="bbd36-833">Kisbetűsre alakítja át a megadott karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-833">Converts the specified string to lower case.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-834">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-834">Parameters</span></span>

| <span data-ttu-id="bbd36-835">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-835">Parameter</span></span> | <span data-ttu-id="bbd36-836">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-836">Required</span></span> | <span data-ttu-id="bbd36-837">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-837">Type</span></span> | <span data-ttu-id="bbd36-838">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-838">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-839">stringToChange</span><span class="sxs-lookup"><span data-stu-id="bbd36-839">stringToChange</span></span> |<span data-ttu-id="bbd36-840">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-840">Yes</span></span> |<span data-ttu-id="bbd36-841">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-841">string</span></span> |<span data-ttu-id="bbd36-842">Az érték kisbetűsre konvertálja.</span><span class="sxs-lookup"><span data-stu-id="bbd36-842">The value to convert to lower case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-843">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-843">Return value</span></span>

<span data-ttu-id="bbd36-844">A karakterlánc kisbetűsre alakítja.</span><span class="sxs-lookup"><span data-stu-id="bbd36-844">The string converted to lower case.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-845">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-845">Examples</span></span>

<span data-ttu-id="bbd36-846">Az alábbi példa egy paraméterérték konvertál, kisbetű és nagybetű.</span><span class="sxs-lookup"><span data-stu-id="bbd36-846">The following example converts a parameter value to lower case and to upper case.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-847">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-847">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-848">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-848">Name</span></span> | <span data-ttu-id="bbd36-849">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-849">Type</span></span> | <span data-ttu-id="bbd36-850">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-850">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-851">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-851">toLowerOutput</span></span> | <span data-ttu-id="bbd36-852">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-852">String</span></span> | <span data-ttu-id="bbd36-853">egy két há'</span><span class="sxs-lookup"><span data-stu-id="bbd36-853">one two three</span></span> |
| <span data-ttu-id="bbd36-854">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-854">toUpperOutput</span></span> | <span data-ttu-id="bbd36-855">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-855">String</span></span> | <span data-ttu-id="bbd36-856">EGY KÉT HÁ'</span><span class="sxs-lookup"><span data-stu-id="bbd36-856">ONE TWO THREE</span></span> |

<a id="toupper" />

## <a name="toupper"></a><span data-ttu-id="bbd36-857">toUpper</span><span class="sxs-lookup"><span data-stu-id="bbd36-857">toUpper</span></span>
`toUpper(stringToChange)`

<span data-ttu-id="bbd36-858">A megadott karakterlánc nagybetűvel alakítja.</span><span class="sxs-lookup"><span data-stu-id="bbd36-858">Converts the specified string to upper case.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-859">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-859">Parameters</span></span>

| <span data-ttu-id="bbd36-860">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-860">Parameter</span></span> | <span data-ttu-id="bbd36-861">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-861">Required</span></span> | <span data-ttu-id="bbd36-862">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-862">Type</span></span> | <span data-ttu-id="bbd36-863">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-863">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-864">stringToChange</span><span class="sxs-lookup"><span data-stu-id="bbd36-864">stringToChange</span></span> |<span data-ttu-id="bbd36-865">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-865">Yes</span></span> |<span data-ttu-id="bbd36-866">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-866">string</span></span> |<span data-ttu-id="bbd36-867">A nagybetűvel átalakítása érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-867">The value to convert to upper case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-868">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-868">Return value</span></span>

<span data-ttu-id="bbd36-869">A karakterlánc nagybetűsre alakítja.</span><span class="sxs-lookup"><span data-stu-id="bbd36-869">The string converted to upper case.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-870">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-870">Examples</span></span>

<span data-ttu-id="bbd36-871">Az alábbi példa egy paraméterérték konvertál, kisbetű és nagybetű.</span><span class="sxs-lookup"><span data-stu-id="bbd36-871">The following example converts a parameter value to lower case and to upper case.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-872">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-872">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-873">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-873">Name</span></span> | <span data-ttu-id="bbd36-874">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-874">Type</span></span> | <span data-ttu-id="bbd36-875">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-875">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-876">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-876">toLowerOutput</span></span> | <span data-ttu-id="bbd36-877">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-877">String</span></span> | <span data-ttu-id="bbd36-878">egy két há'</span><span class="sxs-lookup"><span data-stu-id="bbd36-878">one two three</span></span> |
| <span data-ttu-id="bbd36-879">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-879">toUpperOutput</span></span> | <span data-ttu-id="bbd36-880">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-880">String</span></span> | <span data-ttu-id="bbd36-881">EGY KÉT HÁ'</span><span class="sxs-lookup"><span data-stu-id="bbd36-881">ONE TWO THREE</span></span> |

<a id="trim" />

## <a name="trim"></a><span data-ttu-id="bbd36-882">Trim</span><span class="sxs-lookup"><span data-stu-id="bbd36-882">trim</span></span>
`trim (stringToTrim)`

<span data-ttu-id="bbd36-883">Eltávolítja az összes kezdő és záró üres karaktereket a megadott karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="bbd36-883">Removes all leading and trailing white-space characters from the specified string.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-884">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-884">Parameters</span></span>

| <span data-ttu-id="bbd36-885">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-885">Parameter</span></span> | <span data-ttu-id="bbd36-886">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-886">Required</span></span> | <span data-ttu-id="bbd36-887">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-887">Type</span></span> | <span data-ttu-id="bbd36-888">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-888">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-889">stringToTrim</span><span class="sxs-lookup"><span data-stu-id="bbd36-889">stringToTrim</span></span> |<span data-ttu-id="bbd36-890">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-890">Yes</span></span> |<span data-ttu-id="bbd36-891">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-891">string</span></span> |<span data-ttu-id="bbd36-892">A vágásokról érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-892">The value to trim.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-893">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-893">Return value</span></span>

<span data-ttu-id="bbd36-894">A karakterlánc nélkül kezdő és záró üres karaktereket.</span><span class="sxs-lookup"><span data-stu-id="bbd36-894">The string without leading and trailing white-space characters.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-895">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-895">Examples</span></span>

<span data-ttu-id="bbd36-896">Az alábbi példa levágja a üres karaktereket és a paraméter.</span><span class="sxs-lookup"><span data-stu-id="bbd36-896">The following example trims the white-space characters from the parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-897">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-897">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-898">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-898">Name</span></span> | <span data-ttu-id="bbd36-899">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-899">Type</span></span> | <span data-ttu-id="bbd36-900">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-900">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-901">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="bbd36-901">return</span></span> | <span data-ttu-id="bbd36-902">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-902">String</span></span> | <span data-ttu-id="bbd36-903">egy két há'</span><span class="sxs-lookup"><span data-stu-id="bbd36-903">one two three</span></span> |

<a id="uniquestring" />

## <a name="uniquestring"></a><span data-ttu-id="bbd36-904">uniqueString</span><span class="sxs-lookup"><span data-stu-id="bbd36-904">uniqueString</span></span>
`uniqueString (baseString, ...)`

<span data-ttu-id="bbd36-905">A paraméterként megadott értékek alapján determinisztikus kivonat karakterláncot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bbd36-905">Creates a deterministic hash string based on the values provided as parameters.</span></span> 

### <a name="parameters"></a><span data-ttu-id="bbd36-906">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-906">Parameters</span></span>

| <span data-ttu-id="bbd36-907">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-907">Parameter</span></span> | <span data-ttu-id="bbd36-908">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-908">Required</span></span> | <span data-ttu-id="bbd36-909">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-909">Type</span></span> | <span data-ttu-id="bbd36-910">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-910">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-911">baseString</span><span class="sxs-lookup"><span data-stu-id="bbd36-911">baseString</span></span> |<span data-ttu-id="bbd36-912">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-912">Yes</span></span> |<span data-ttu-id="bbd36-913">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-913">string</span></span> |<span data-ttu-id="bbd36-914">Az értéknek a kivonatoló függvényt egy egyedi karakterlánc létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="bbd36-914">The value used in the hash function to create a unique string.</span></span> |
| <span data-ttu-id="bbd36-915">szükség szerint további paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-915">additional parameters as needed</span></span> |<span data-ttu-id="bbd36-916">Nem</span><span class="sxs-lookup"><span data-stu-id="bbd36-916">No</span></span> |<span data-ttu-id="bbd36-917">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-917">string</span></span> |<span data-ttu-id="bbd36-918">Az érték, amely meghatározza a egyediségi szintű létrehozásához szükséges annyi karakterláncok adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="bbd36-918">You can add as many strings as needed to create the value that specifies the level of uniqueness.</span></span> |

### <a name="remarks"></a><span data-ttu-id="bbd36-919">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bbd36-919">Remarks</span></span>

<span data-ttu-id="bbd36-920">Ez a funkció akkor hasznos, ha kell létrehoznia egy erőforrást egy egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="bbd36-920">This function is helpful when you need to create a unique name for a resource.</span></span> <span data-ttu-id="bbd36-921">Az eredmény egyediségének hatókörét korlátozó paraméter értékeket ad meg.</span><span class="sxs-lookup"><span data-stu-id="bbd36-921">You provide parameter values that limit the scope of uniqueness for the result.</span></span> <span data-ttu-id="bbd36-922">Megadhatja, hogy egyedi előfizetés, a csoport vagy a központi telepítési le-e a neve.</span><span class="sxs-lookup"><span data-stu-id="bbd36-922">You can specify whether the name is unique down to subscription, resource group, or deployment.</span></span> 

<span data-ttu-id="bbd36-923">A visszaadott érték nem véletlenszerű karakterlánc, hanem inkább a kivonatoló függvényt eredményét.</span><span class="sxs-lookup"><span data-stu-id="bbd36-923">The returned value is not a random string, but rather the result of a hash function.</span></span> <span data-ttu-id="bbd36-924">A visszaadott érték 13 karakterig.</span><span class="sxs-lookup"><span data-stu-id="bbd36-924">The returned value is 13 characters long.</span></span> <span data-ttu-id="bbd36-925">Nincs globálisan egyedi.</span><span class="sxs-lookup"><span data-stu-id="bbd36-925">It is not globally unique.</span></span> <span data-ttu-id="bbd36-926">Érdemes lehet az érték kombinálhatja az elnevezési, kifejező nevet létrehozni a előtaggal kezdődik.</span><span class="sxs-lookup"><span data-stu-id="bbd36-926">You may want to combine the value with a prefix from your naming convention to create a name that is meaningful.</span></span> <span data-ttu-id="bbd36-927">A következő példa bemutatja a visszaadott érték formátuma.</span><span class="sxs-lookup"><span data-stu-id="bbd36-927">The following example shows the format of the returned value.</span></span> <span data-ttu-id="bbd36-928">A tényleges érték függ a megadott paraméterek.</span><span class="sxs-lookup"><span data-stu-id="bbd36-928">The actual value varies by the provided parameters.</span></span>

    tcvhiyu5h2o5o

<span data-ttu-id="bbd36-929">A következő példák szemléltetik a uniqueString segítségével hozzon létre egy egyedi értéket a gyakran használt szintek.</span><span class="sxs-lookup"><span data-stu-id="bbd36-929">The following examples show how to use uniqueString to create a unique value for commonly used levels.</span></span>

<span data-ttu-id="bbd36-930">Egyedi előfizetés hatóköre</span><span class="sxs-lookup"><span data-stu-id="bbd36-930">Unique scoped to subscription</span></span>

```json
"[uniqueString(subscription().subscriptionId)]"
```

<span data-ttu-id="bbd36-931">Egyedi erőforráscsoport hatóköre</span><span class="sxs-lookup"><span data-stu-id="bbd36-931">Unique scoped to resource group</span></span>

```json
"[uniqueString(resourceGroup().id)]"
```

<span data-ttu-id="bbd36-932">Egyedi telepítési erőforrás csoport hatóköre</span><span class="sxs-lookup"><span data-stu-id="bbd36-932">Unique scoped to deployment for a resource group</span></span>

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

<span data-ttu-id="bbd36-933">A következő példa bemutatja, hogyan hozzon létre egy tárfiókot, az erőforráscsoport alapján egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="bbd36-933">The following example shows how to create a unique name for a storage account based on your resource group.</span></span> <span data-ttu-id="bbd36-934">Az erőforráscsoport belül a név nincs egyedi, ha a megszokott módon.</span><span class="sxs-lookup"><span data-stu-id="bbd36-934">Inside the resource group, the name is not unique if constructed the same way.</span></span>

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a><span data-ttu-id="bbd36-935">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-935">Return value</span></span>

<span data-ttu-id="bbd36-936">13 karaktereket tartalmazó karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bbd36-936">A string containing 13 characters.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-937">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-937">Examples</span></span>

<span data-ttu-id="bbd36-938">A következő példa eredménye uniquestring adja vissza:</span><span class="sxs-lookup"><span data-stu-id="bbd36-938">The following example returns results from uniquestring:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

<a id="uri" />

## <a name="uri"></a><span data-ttu-id="bbd36-939">URI</span><span class="sxs-lookup"><span data-stu-id="bbd36-939">uri</span></span>
`uri (baseUri, relativeUri)`

<span data-ttu-id="bbd36-940">A baseUri és a relativeUri karakterlánc kombinálásával hoz létre egy abszolút URI.</span><span class="sxs-lookup"><span data-stu-id="bbd36-940">Creates an absolute URI by combining the baseUri and the relativeUri string.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-941">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-941">Parameters</span></span>

| <span data-ttu-id="bbd36-942">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-942">Parameter</span></span> | <span data-ttu-id="bbd36-943">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-943">Required</span></span> | <span data-ttu-id="bbd36-944">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-944">Type</span></span> | <span data-ttu-id="bbd36-945">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-945">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-946">a baseUri</span><span class="sxs-lookup"><span data-stu-id="bbd36-946">baseUri</span></span> |<span data-ttu-id="bbd36-947">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-947">Yes</span></span> |<span data-ttu-id="bbd36-948">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-948">string</span></span> |<span data-ttu-id="bbd36-949">Az alap URI-azonosító karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bbd36-949">The base uri string.</span></span> |
| <span data-ttu-id="bbd36-950">relativeUri</span><span class="sxs-lookup"><span data-stu-id="bbd36-950">relativeUri</span></span> |<span data-ttu-id="bbd36-951">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-951">Yes</span></span> |<span data-ttu-id="bbd36-952">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-952">string</span></span> |<span data-ttu-id="bbd36-953">A relatív uri karakterlánc hozzáadása az alap URI-azonosító karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bbd36-953">The relative uri string to add to the base uri string.</span></span> |

<span data-ttu-id="bbd36-954">Az érték a **baseUri** a paraméter egy adott fájlt tartalmazhat, de csak az alap elérési utat használja az URI konstrukciója során.</span><span class="sxs-lookup"><span data-stu-id="bbd36-954">The value for the **baseUri** parameter can include a specific file, but only the base path is used when constructing the URI.</span></span> <span data-ttu-id="bbd36-955">Például, hogy `http://contoso.com/resources/azuredeploy.json` alap URI-azonosítója a baseUri paraméter kiemelve `http://contoso.com/resources/`.</span><span class="sxs-lookup"><span data-stu-id="bbd36-955">For example, passing `http://contoso.com/resources/azuredeploy.json` as the baseUri parameter results in a base URI of `http://contoso.com/resources/`.</span></span>

### <a name="return-value"></a><span data-ttu-id="bbd36-956">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-956">Return value</span></span>

<span data-ttu-id="bbd36-957">Egy karakterlánc, amely az alapszintű és a relatív értékek abszolút URI.</span><span class="sxs-lookup"><span data-stu-id="bbd36-957">A string representing the absolute URI for the base and relative values.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-958">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-958">Examples</span></span>

<span data-ttu-id="bbd36-959">A következő példa bemutatja, hogyan egy hivatkozást a beágyazott sablonok alapján a szülő-sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="bbd36-959">The following example shows how to construct a link to a nested template based on the value of the parent template.</span></span>

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

<span data-ttu-id="bbd36-960">A következő példa bemutatja, hogyan uri, uriComponent és uriComponentToString használatára:</span><span class="sxs-lookup"><span data-stu-id="bbd36-960">The following example shows how to use uri, uriComponent, and uriComponentToString:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-961">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-961">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-962">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-962">Name</span></span> | <span data-ttu-id="bbd36-963">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-963">Type</span></span> | <span data-ttu-id="bbd36-964">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-964">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-965">uriOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-965">uriOutput</span></span> | <span data-ttu-id="bbd36-966">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-966">String</span></span> | <span data-ttu-id="bbd36-967">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-967">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="bbd36-968">componentOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-968">componentOutput</span></span> | <span data-ttu-id="bbd36-969">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-969">String</span></span> | <span data-ttu-id="bbd36-970">HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-970">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="bbd36-971">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-971">toStringOutput</span></span> | <span data-ttu-id="bbd36-972">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-972">String</span></span> | <span data-ttu-id="bbd36-973">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-973">http://contoso.com/resources/nested/azuredeploy.json</span></span> |

<a id="uricomponent" />

## <a name="uricomponent"></a><span data-ttu-id="bbd36-974">uriComponent</span><span class="sxs-lookup"><span data-stu-id="bbd36-974">uriComponent</span></span>
`uricomponent(stringToEncode)`

<span data-ttu-id="bbd36-975">Kódolja URI.</span><span class="sxs-lookup"><span data-stu-id="bbd36-975">Encodes a URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-976">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-976">Parameters</span></span>

| <span data-ttu-id="bbd36-977">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-977">Parameter</span></span> | <span data-ttu-id="bbd36-978">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-978">Required</span></span> | <span data-ttu-id="bbd36-979">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-979">Type</span></span> | <span data-ttu-id="bbd36-980">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-980">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-981">stringToEncode</span><span class="sxs-lookup"><span data-stu-id="bbd36-981">stringToEncode</span></span> |<span data-ttu-id="bbd36-982">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-982">Yes</span></span> |<span data-ttu-id="bbd36-983">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-983">string</span></span> |<span data-ttu-id="bbd36-984">Az érték kódolására.</span><span class="sxs-lookup"><span data-stu-id="bbd36-984">The value to encode.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-985">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-985">Return value</span></span>

<span data-ttu-id="bbd36-986">A URI karakterlánc kódolt érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-986">A string of the URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-987">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-987">Examples</span></span>

<span data-ttu-id="bbd36-988">A következő példa bemutatja, hogyan uri, uriComponent és uriComponentToString használatára:</span><span class="sxs-lookup"><span data-stu-id="bbd36-988">The following example shows how to use uri, uriComponent, and uriComponentToString:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-989">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-989">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-990">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-990">Name</span></span> | <span data-ttu-id="bbd36-991">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-991">Type</span></span> | <span data-ttu-id="bbd36-992">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-992">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-993">uriOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-993">uriOutput</span></span> | <span data-ttu-id="bbd36-994">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-994">String</span></span> | <span data-ttu-id="bbd36-995">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-995">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="bbd36-996">componentOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-996">componentOutput</span></span> | <span data-ttu-id="bbd36-997">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-997">String</span></span> | <span data-ttu-id="bbd36-998">HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-998">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="bbd36-999">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-999">toStringOutput</span></span> | <span data-ttu-id="bbd36-1000">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-1000">String</span></span> | <span data-ttu-id="bbd36-1001">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-1001">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a><span data-ttu-id="bbd36-1002">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="bbd36-1002">uriComponentToString</span></span>
`uriComponentToString(uriEncodedString)`

<span data-ttu-id="bbd36-1003">Adja vissza a URI karakterlánc kódolású érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-1003">Returns a string of a URI encoded value.</span></span>

### <a name="parameters"></a><span data-ttu-id="bbd36-1004">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="bbd36-1004">Parameters</span></span>

| <span data-ttu-id="bbd36-1005">Paraméter</span><span class="sxs-lookup"><span data-stu-id="bbd36-1005">Parameter</span></span> | <span data-ttu-id="bbd36-1006">Szükséges</span><span class="sxs-lookup"><span data-stu-id="bbd36-1006">Required</span></span> | <span data-ttu-id="bbd36-1007">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-1007">Type</span></span> | <span data-ttu-id="bbd36-1008">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbd36-1008">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="bbd36-1009">uriEncodedString</span><span class="sxs-lookup"><span data-stu-id="bbd36-1009">uriEncodedString</span></span> |<span data-ttu-id="bbd36-1010">Igen</span><span class="sxs-lookup"><span data-stu-id="bbd36-1010">Yes</span></span> |<span data-ttu-id="bbd36-1011">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-1011">string</span></span> |<span data-ttu-id="bbd36-1012">Az URI kódolású értékét karakterlánccá konvertálni.</span><span class="sxs-lookup"><span data-stu-id="bbd36-1012">The URI encoded value to convert to a string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="bbd36-1013">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-1013">Return value</span></span>

<span data-ttu-id="bbd36-1014">A dekódolt karakterlánc URI-kódolt érték.</span><span class="sxs-lookup"><span data-stu-id="bbd36-1014">A decoded string of URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="bbd36-1015">Példák</span><span class="sxs-lookup"><span data-stu-id="bbd36-1015">Examples</span></span>

<span data-ttu-id="bbd36-1016">A következő példa bemutatja, hogyan uri, uriComponent és uriComponentToString használatára:</span><span class="sxs-lookup"><span data-stu-id="bbd36-1016">The following example shows how to use uri, uriComponent, and uriComponentToString:</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

<span data-ttu-id="bbd36-1017">Az alapértelmezett értékeit az előző példából kimenete:</span><span class="sxs-lookup"><span data-stu-id="bbd36-1017">The output from the preceding example with the default values is:</span></span>

| <span data-ttu-id="bbd36-1018">Név</span><span class="sxs-lookup"><span data-stu-id="bbd36-1018">Name</span></span> | <span data-ttu-id="bbd36-1019">Típus</span><span class="sxs-lookup"><span data-stu-id="bbd36-1019">Type</span></span> | <span data-ttu-id="bbd36-1020">Érték</span><span class="sxs-lookup"><span data-stu-id="bbd36-1020">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="bbd36-1021">uriOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-1021">uriOutput</span></span> | <span data-ttu-id="bbd36-1022">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-1022">String</span></span> | <span data-ttu-id="bbd36-1023">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-1023">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="bbd36-1024">componentOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-1024">componentOutput</span></span> | <span data-ttu-id="bbd36-1025">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-1025">String</span></span> | <span data-ttu-id="bbd36-1026">HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-1026">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="bbd36-1027">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="bbd36-1027">toStringOutput</span></span> | <span data-ttu-id="bbd36-1028">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bbd36-1028">String</span></span> | <span data-ttu-id="bbd36-1029">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="bbd36-1029">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


## <a name="next-steps"></a><span data-ttu-id="bbd36-1030">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bbd36-1030">Next steps</span></span>
* <span data-ttu-id="bbd36-1031">A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="bbd36-1031">For a description of the sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="bbd36-1032">Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="bbd36-1032">To merge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="bbd36-1033">Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="bbd36-1033">To iterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="bbd36-1034">A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="bbd36-1034">To see how to deploy the template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


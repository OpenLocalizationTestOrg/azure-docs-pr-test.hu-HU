---
title: "aaaAzure Resource Manager sablonfüggvényei - karakterlánc |} Microsoft Docs"
description: "A karakterláncok az Azure Resource Manager sablon toowork hello funkciók toouse ismerteti."
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
ms.openlocfilehash: 27f7f6a52cbe4e9915718184433e92ca92999346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="string-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="a0766-103">Az Azure Resource Manager sablonokhoz karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-103">String functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="a0766-104">A Resource Manager biztosít a következő funkciók karakterláncok való munkához hello:</span><span class="sxs-lookup"><span data-stu-id="a0766-104">Resource Manager provides hello following functions for working with strings:</span></span>

* [<span data-ttu-id="a0766-105">a Base64</span><span class="sxs-lookup"><span data-stu-id="a0766-105">base64</span></span>](#base64)
* [<span data-ttu-id="a0766-106">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="a0766-106">base64ToJson</span></span>](#base64tojson)
* [<span data-ttu-id="a0766-107">base64ToString</span><span class="sxs-lookup"><span data-stu-id="a0766-107">base64ToString</span></span>](#base64tostring)
* [<span data-ttu-id="a0766-108">Concat</span><span class="sxs-lookup"><span data-stu-id="a0766-108">concat</span></span>](#concat)
* [<span data-ttu-id="a0766-109">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="a0766-109">contains</span></span>](#contains)
* [<span data-ttu-id="a0766-110">dataUri</span><span class="sxs-lookup"><span data-stu-id="a0766-110">dataUri</span></span>](#datauri)
* [<span data-ttu-id="a0766-111">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="a0766-111">dataUriToString</span></span>](#datauritostring)
* [<span data-ttu-id="a0766-112">üres</span><span class="sxs-lookup"><span data-stu-id="a0766-112">empty</span></span>](#empty)
* [<span data-ttu-id="a0766-113">megadott módon végződő</span><span class="sxs-lookup"><span data-stu-id="a0766-113">endsWith</span></span>](#endswith)
* [<span data-ttu-id="a0766-114">első</span><span class="sxs-lookup"><span data-stu-id="a0766-114">first</span></span>](#first)
* [<span data-ttu-id="a0766-115">indexOf</span><span class="sxs-lookup"><span data-stu-id="a0766-115">indexOf</span></span>](#indexof)
* [<span data-ttu-id="a0766-116">utolsó</span><span class="sxs-lookup"><span data-stu-id="a0766-116">last</span></span>](#last)
* [<span data-ttu-id="a0766-117">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="a0766-117">lastIndexOf</span></span>](#lastindexof)
* [<span data-ttu-id="a0766-118">hossza</span><span class="sxs-lookup"><span data-stu-id="a0766-118">length</span></span>](#length)
* [<span data-ttu-id="a0766-119">padLeft</span><span class="sxs-lookup"><span data-stu-id="a0766-119">padLeft</span></span>](#padleft)
* [<span data-ttu-id="a0766-120">cserélje le</span><span class="sxs-lookup"><span data-stu-id="a0766-120">replace</span></span>](#replace)
* [<span data-ttu-id="a0766-121">hagyja ki</span><span class="sxs-lookup"><span data-stu-id="a0766-121">skip</span></span>](#skip)
* [<span data-ttu-id="a0766-122">felosztás</span><span class="sxs-lookup"><span data-stu-id="a0766-122">split</span></span>](#split)
* [<span data-ttu-id="a0766-123">startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="a0766-123">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="a0766-124">karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-124">string</span></span>](#string)
* [<span data-ttu-id="a0766-125">Substring</span><span class="sxs-lookup"><span data-stu-id="a0766-125">substring</span></span>](#substring)
* [<span data-ttu-id="a0766-126">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="a0766-126">take</span></span>](#take)
* [<span data-ttu-id="a0766-127">toLower</span><span class="sxs-lookup"><span data-stu-id="a0766-127">toLower</span></span>](#tolower)
* [<span data-ttu-id="a0766-128">toUpper</span><span class="sxs-lookup"><span data-stu-id="a0766-128">toUpper</span></span>](#toupper)
* [<span data-ttu-id="a0766-129">Trim</span><span class="sxs-lookup"><span data-stu-id="a0766-129">trim</span></span>](#trim)
* [<span data-ttu-id="a0766-130">uniqueString</span><span class="sxs-lookup"><span data-stu-id="a0766-130">uniqueString</span></span>](#uniquestring)
* [<span data-ttu-id="a0766-131">URI</span><span class="sxs-lookup"><span data-stu-id="a0766-131">uri</span></span>](#uri)
* [<span data-ttu-id="a0766-132">uriComponent</span><span class="sxs-lookup"><span data-stu-id="a0766-132">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="a0766-133">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="a0766-133">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a><span data-ttu-id="a0766-134">a Base64</span><span class="sxs-lookup"><span data-stu-id="a0766-134">base64</span></span>
`base64(inputString)`

<span data-ttu-id="a0766-135">Értéket ad vissza a bemeneti karakterlánc hello base64 ábrázolását hello.</span><span class="sxs-lookup"><span data-stu-id="a0766-135">Returns hello base64 representation of hello input string.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-136">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-136">Parameters</span></span>

| <span data-ttu-id="a0766-137">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-137">Parameter</span></span> | <span data-ttu-id="a0766-138">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-138">Required</span></span> | <span data-ttu-id="a0766-139">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-139">Type</span></span> | <span data-ttu-id="a0766-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-140">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-141">inputString</span><span class="sxs-lookup"><span data-stu-id="a0766-141">inputString</span></span> |<span data-ttu-id="a0766-142">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-142">Yes</span></span> |<span data-ttu-id="a0766-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-143">string</span></span> |<span data-ttu-id="a0766-144">hello érték tooreturn, mint a Base64 kódolású megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a0766-144">hello value tooreturn as a base64 representation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-145">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-145">Return value</span></span>

<span data-ttu-id="a0766-146">Hello base64 tartalmazó karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-146">A string containing hello base64 representation.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-147">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-147">Examples</span></span>

<span data-ttu-id="a0766-148">hello a következő példa bemutatja, hogyan toouse hello base64 függvény.</span><span class="sxs-lookup"><span data-stu-id="a0766-148">hello following example shows how toouse hello base64 function.</span></span>

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

<span data-ttu-id="a0766-149">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-149">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-150">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-150">Name</span></span> | <span data-ttu-id="a0766-151">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-151">Type</span></span> | <span data-ttu-id="a0766-152">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-152">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-153">base64Output</span><span class="sxs-lookup"><span data-stu-id="a0766-153">base64Output</span></span> | <span data-ttu-id="a0766-154">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-154">String</span></span> | <span data-ttu-id="a0766-155">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="a0766-155">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="a0766-156">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-156">toStringOutput</span></span> | <span data-ttu-id="a0766-157">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-157">String</span></span> | <span data-ttu-id="a0766-158">egy két há'</span><span class="sxs-lookup"><span data-stu-id="a0766-158">one, two, three</span></span> |
| <span data-ttu-id="a0766-159">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-159">toJsonOutput</span></span> | <span data-ttu-id="a0766-160">Objektum</span><span class="sxs-lookup"><span data-stu-id="a0766-160">Object</span></span> | <span data-ttu-id="a0766-161">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="a0766-161">{"one": "a", "two": "b"}</span></span> |

<a id="base64tojson" />

## <a name="base64tojson"></a><span data-ttu-id="a0766-162">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="a0766-162">base64ToJson</span></span>
`base64tojson`

<span data-ttu-id="a0766-163">A base64 ábrázolását tooa JSON-objektum alakítja.</span><span class="sxs-lookup"><span data-stu-id="a0766-163">Converts a base64 representation tooa JSON object.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-164">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-164">Parameters</span></span>

| <span data-ttu-id="a0766-165">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-165">Parameter</span></span> | <span data-ttu-id="a0766-166">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-166">Required</span></span> | <span data-ttu-id="a0766-167">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-167">Type</span></span> | <span data-ttu-id="a0766-168">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-168">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-169">base64Value</span><span class="sxs-lookup"><span data-stu-id="a0766-169">base64Value</span></span> |<span data-ttu-id="a0766-170">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-170">Yes</span></span> |<span data-ttu-id="a0766-171">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-171">string</span></span> |<span data-ttu-id="a0766-172">hello base64 ábrázolását tooconvert tooa JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="a0766-172">hello base64 representation tooconvert tooa JSON object.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-173">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-173">Return value</span></span>

<span data-ttu-id="a0766-174">A JSON-objektumból.</span><span class="sxs-lookup"><span data-stu-id="a0766-174">A JSON object.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-175">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-175">Examples</span></span>

<span data-ttu-id="a0766-176">hello következő példában hello base64ToJson függvény tooconvert base64 értéket:</span><span class="sxs-lookup"><span data-stu-id="a0766-176">hello following example uses hello base64ToJson function tooconvert a base64 value:</span></span>

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

<span data-ttu-id="a0766-177">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-177">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-178">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-178">Name</span></span> | <span data-ttu-id="a0766-179">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-179">Type</span></span> | <span data-ttu-id="a0766-180">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-180">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-181">base64Output</span><span class="sxs-lookup"><span data-stu-id="a0766-181">base64Output</span></span> | <span data-ttu-id="a0766-182">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-182">String</span></span> | <span data-ttu-id="a0766-183">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="a0766-183">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="a0766-184">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-184">toStringOutput</span></span> | <span data-ttu-id="a0766-185">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-185">String</span></span> | <span data-ttu-id="a0766-186">egy két há'</span><span class="sxs-lookup"><span data-stu-id="a0766-186">one, two, three</span></span> |
| <span data-ttu-id="a0766-187">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-187">toJsonOutput</span></span> | <span data-ttu-id="a0766-188">Objektum</span><span class="sxs-lookup"><span data-stu-id="a0766-188">Object</span></span> | <span data-ttu-id="a0766-189">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="a0766-189">{"one": "a", "two": "b"}</span></span> |

<a id="base64tostring" />

## <a name="base64tostring"></a><span data-ttu-id="a0766-190">base64ToString</span><span class="sxs-lookup"><span data-stu-id="a0766-190">base64ToString</span></span>
`base64ToString(base64Value)`

<span data-ttu-id="a0766-191">Számmá alakít egy base64 ábrázolását tooa karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a0766-191">Converts a base64 representation tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-192">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-192">Parameters</span></span>

| <span data-ttu-id="a0766-193">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-193">Parameter</span></span> | <span data-ttu-id="a0766-194">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-194">Required</span></span> | <span data-ttu-id="a0766-195">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-195">Type</span></span> | <span data-ttu-id="a0766-196">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-196">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-197">base64Value</span><span class="sxs-lookup"><span data-stu-id="a0766-197">base64Value</span></span> |<span data-ttu-id="a0766-198">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-198">Yes</span></span> |<span data-ttu-id="a0766-199">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-199">string</span></span> |<span data-ttu-id="a0766-200">hello base64 ábrázolását tooconvert tooa karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-200">hello base64 representation tooconvert tooa string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-201">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-201">Return value</span></span>

<span data-ttu-id="a0766-202">Hello karakterlánc base64 érték alakítja.</span><span class="sxs-lookup"><span data-stu-id="a0766-202">A string of hello converted base64 value.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-203">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-203">Examples</span></span>

<span data-ttu-id="a0766-204">hello következő példában hello base64ToString függvény tooconvert base64 értéket:</span><span class="sxs-lookup"><span data-stu-id="a0766-204">hello following example uses hello base64ToString function tooconvert a base64 value:</span></span>

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

<span data-ttu-id="a0766-205">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-205">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-206">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-206">Name</span></span> | <span data-ttu-id="a0766-207">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-207">Type</span></span> | <span data-ttu-id="a0766-208">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-208">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-209">base64Output</span><span class="sxs-lookup"><span data-stu-id="a0766-209">base64Output</span></span> | <span data-ttu-id="a0766-210">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-210">String</span></span> | <span data-ttu-id="a0766-211">b25lLCB0d28sIHRocmVl</span><span class="sxs-lookup"><span data-stu-id="a0766-211">b25lLCB0d28sIHRocmVl</span></span> |
| <span data-ttu-id="a0766-212">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-212">toStringOutput</span></span> | <span data-ttu-id="a0766-213">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-213">String</span></span> | <span data-ttu-id="a0766-214">egy két há'</span><span class="sxs-lookup"><span data-stu-id="a0766-214">one, two, three</span></span> |
| <span data-ttu-id="a0766-215">toJsonOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-215">toJsonOutput</span></span> | <span data-ttu-id="a0766-216">Objektum</span><span class="sxs-lookup"><span data-stu-id="a0766-216">Object</span></span> | <span data-ttu-id="a0766-217">{"egy": "a", "2": "b"}</span><span class="sxs-lookup"><span data-stu-id="a0766-217">{"one": "a", "two": "b"}</span></span> |



<a id="concat" />

## <a name="concat"></a><span data-ttu-id="a0766-218">Concat</span><span class="sxs-lookup"><span data-stu-id="a0766-218">concat</span></span>
`concat (arg1, arg2, arg3, ...)`

<span data-ttu-id="a0766-219">Több karakterlánc-értékek egyesíti, és összefűzendő hello karakterláncot ad vissza, vagy több tömbök egyesíti, és összefűzendő hello tömböt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-219">Combines multiple string values and returns hello concatenated string, or combines multiple arrays and returns hello concatenated array.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-220">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-220">Parameters</span></span>

| <span data-ttu-id="a0766-221">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-221">Parameter</span></span> | <span data-ttu-id="a0766-222">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-222">Required</span></span> | <span data-ttu-id="a0766-223">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-223">Type</span></span> | <span data-ttu-id="a0766-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-224">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-225">arg1</span><span class="sxs-lookup"><span data-stu-id="a0766-225">arg1</span></span> |<span data-ttu-id="a0766-226">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-226">Yes</span></span> |<span data-ttu-id="a0766-227">karakterlánc vagy tömb</span><span class="sxs-lookup"><span data-stu-id="a0766-227">string or array</span></span> |<span data-ttu-id="a0766-228">hello első érték a kapott.</span><span class="sxs-lookup"><span data-stu-id="a0766-228">hello first value for concatenation.</span></span> |
| <span data-ttu-id="a0766-229">További argumentumok</span><span class="sxs-lookup"><span data-stu-id="a0766-229">additional arguments</span></span> |<span data-ttu-id="a0766-230">Nem</span><span class="sxs-lookup"><span data-stu-id="a0766-230">No</span></span> |<span data-ttu-id="a0766-231">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-231">string</span></span> |<span data-ttu-id="a0766-232">További értéket kapott a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="a0766-232">Additional values in sequential order for concatenation.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-233">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-233">Return value</span></span>
<span data-ttu-id="a0766-234">A karakterlánc vagy tömb összefűzött.</span><span class="sxs-lookup"><span data-stu-id="a0766-234">A string or array of concatenated values.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-235">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-235">Examples</span></span>

<span data-ttu-id="a0766-236">hello a következő példa bemutatja, hogyan toocombine két karakterlánc-értékeket, és térjen vissza olyan összefűzött karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a0766-236">hello following example shows how toocombine two string values and return a concatenated string.</span></span>

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

<span data-ttu-id="a0766-237">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-237">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-238">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-238">Name</span></span> | <span data-ttu-id="a0766-239">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-239">Type</span></span> | <span data-ttu-id="a0766-240">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-240">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-241">concatOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-241">concatOutput</span></span> | <span data-ttu-id="a0766-242">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-242">String</span></span> | <span data-ttu-id="a0766-243">előtag-5yj4yjf5mbg72</span><span class="sxs-lookup"><span data-stu-id="a0766-243">prefix-5yj4yjf5mbg72</span></span> |

<span data-ttu-id="a0766-244">hello a következő példa bemutatja, hogyan két toocombine tömbállandó.</span><span class="sxs-lookup"><span data-stu-id="a0766-244">hello following example shows how toocombine two arrays.</span></span>

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

<span data-ttu-id="a0766-245">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-245">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-246">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-246">Name</span></span> | <span data-ttu-id="a0766-247">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-247">Type</span></span> | <span data-ttu-id="a0766-248">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-248">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-249">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="a0766-249">return</span></span> | <span data-ttu-id="a0766-250">Tömb</span><span class="sxs-lookup"><span data-stu-id="a0766-250">Array</span></span> | <span data-ttu-id="a0766-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span><span class="sxs-lookup"><span data-stu-id="a0766-251">["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"]</span></span> |

<a id="contains" />

## <a name="contains"></a><span data-ttu-id="a0766-252">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="a0766-252">contains</span></span>
`contains (container, itemToFind)`

<span data-ttu-id="a0766-253">Ellenőrzi, hogy egy tömb értéket tartalmaz, objektum kulcsot tartalmaz, vagy egy karakterláncot egy substring tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a0766-253">Checks whether an array contains a value, an object contains a key, or a string contains a substring.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-254">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-254">Parameters</span></span>

| <span data-ttu-id="a0766-255">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-255">Parameter</span></span> | <span data-ttu-id="a0766-256">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-256">Required</span></span> | <span data-ttu-id="a0766-257">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-257">Type</span></span> | <span data-ttu-id="a0766-258">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-258">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-259">Tároló</span><span class="sxs-lookup"><span data-stu-id="a0766-259">container</span></span> |<span data-ttu-id="a0766-260">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-260">Yes</span></span> |<span data-ttu-id="a0766-261">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-261">array, object, or string</span></span> |<span data-ttu-id="a0766-262">hello érték, amely hello érték toofind tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a0766-262">hello value that contains hello value toofind.</span></span> |
| <span data-ttu-id="a0766-263">itemToFind</span><span class="sxs-lookup"><span data-stu-id="a0766-263">itemToFind</span></span> |<span data-ttu-id="a0766-264">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-264">Yes</span></span> |<span data-ttu-id="a0766-265">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="a0766-265">string or int</span></span> |<span data-ttu-id="a0766-266">hello érték toofind.</span><span class="sxs-lookup"><span data-stu-id="a0766-266">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-267">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-267">Return value</span></span>

<span data-ttu-id="a0766-268">**Igaz** Ha hello elem található; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="a0766-268">**True** if hello item is found; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-269">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-269">Examples</span></span>

<span data-ttu-id="a0766-270">hello következő példa bemutatja, hogyan toouse különböző típusú tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a0766-270">hello following example shows how toouse contains with different types:</span></span>

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

<span data-ttu-id="a0766-271">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-271">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-272">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-272">Name</span></span> | <span data-ttu-id="a0766-273">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-273">Type</span></span> | <span data-ttu-id="a0766-274">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-274">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-275">stringTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-275">stringTrue</span></span> | <span data-ttu-id="a0766-276">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-276">Bool</span></span> | <span data-ttu-id="a0766-277">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-277">True</span></span> |
| <span data-ttu-id="a0766-278">stringFalse</span><span class="sxs-lookup"><span data-stu-id="a0766-278">stringFalse</span></span> | <span data-ttu-id="a0766-279">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-279">Bool</span></span> | <span data-ttu-id="a0766-280">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="a0766-280">False</span></span> |
| <span data-ttu-id="a0766-281">objectTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-281">objectTrue</span></span> | <span data-ttu-id="a0766-282">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-282">Bool</span></span> | <span data-ttu-id="a0766-283">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-283">True</span></span> |
| <span data-ttu-id="a0766-284">objectFalse</span><span class="sxs-lookup"><span data-stu-id="a0766-284">objectFalse</span></span> | <span data-ttu-id="a0766-285">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-285">Bool</span></span> | <span data-ttu-id="a0766-286">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="a0766-286">False</span></span> |
| <span data-ttu-id="a0766-287">arrayTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-287">arrayTrue</span></span> | <span data-ttu-id="a0766-288">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-288">Bool</span></span> | <span data-ttu-id="a0766-289">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-289">True</span></span> |
| <span data-ttu-id="a0766-290">arrayFalse</span><span class="sxs-lookup"><span data-stu-id="a0766-290">arrayFalse</span></span> | <span data-ttu-id="a0766-291">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-291">Bool</span></span> | <span data-ttu-id="a0766-292">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="a0766-292">False</span></span> |

<a id="datauri" />

## <a name="datauri"></a><span data-ttu-id="a0766-293">dataUri</span><span class="sxs-lookup"><span data-stu-id="a0766-293">dataUri</span></span>
`dataUri(stringToConvert)`

<span data-ttu-id="a0766-294">Alakít egy értéket tooa adat-URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="a0766-294">Converts a value tooa data URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-295">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-295">Parameters</span></span>

| <span data-ttu-id="a0766-296">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-296">Parameter</span></span> | <span data-ttu-id="a0766-297">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-297">Required</span></span> | <span data-ttu-id="a0766-298">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-298">Type</span></span> | <span data-ttu-id="a0766-299">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-299">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-300">stringToConvert</span><span class="sxs-lookup"><span data-stu-id="a0766-300">stringToConvert</span></span> |<span data-ttu-id="a0766-301">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-301">Yes</span></span> |<span data-ttu-id="a0766-302">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-302">string</span></span> |<span data-ttu-id="a0766-303">hello tooconvert tooa érték URI.</span><span class="sxs-lookup"><span data-stu-id="a0766-303">hello value tooconvert tooa data URI.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-304">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-304">Return value</span></span>

<span data-ttu-id="a0766-305">Egy karakterláncot formázni egy adat-URI azonosító.</span><span class="sxs-lookup"><span data-stu-id="a0766-305">A string formatted as a data URI.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-306">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-306">Examples</span></span>

<span data-ttu-id="a0766-307">a következő példa hello alakít egy értéket tooa adat-URI azonosító, és konvertálja az egy URI tooa karakterlánc adatokat:</span><span class="sxs-lookup"><span data-stu-id="a0766-307">hello following example converts a value tooa data URI, and converts a data URI tooa string:</span></span>

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

<span data-ttu-id="a0766-308">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-308">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-309">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-309">Name</span></span> | <span data-ttu-id="a0766-310">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-310">Type</span></span> | <span data-ttu-id="a0766-311">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-311">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-312">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-312">dataUriOutput</span></span> | <span data-ttu-id="a0766-313">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-313">String</span></span> | <span data-ttu-id="a0766-314">adatok: text / egyszerű; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="a0766-314">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="a0766-315">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-315">toStringOutput</span></span> | <span data-ttu-id="a0766-316">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-316">String</span></span> | <span data-ttu-id="a0766-317">helló világ!</span><span class="sxs-lookup"><span data-stu-id="a0766-317">Hello, World!</span></span> |

<a id="datauritostring" />

## <a name="datauritostring"></a><span data-ttu-id="a0766-318">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="a0766-318">dataUriToString</span></span>
`dataUriToString(dataUriToConvert)`

<span data-ttu-id="a0766-319">Egy adat-URI azonosító érték tooa karakterlánc formátuma.</span><span class="sxs-lookup"><span data-stu-id="a0766-319">Converts a data URI formatted value tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-320">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-320">Parameters</span></span>

| <span data-ttu-id="a0766-321">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-321">Parameter</span></span> | <span data-ttu-id="a0766-322">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-322">Required</span></span> | <span data-ttu-id="a0766-323">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-323">Type</span></span> | <span data-ttu-id="a0766-324">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-324">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-325">dataUriToConvert</span><span class="sxs-lookup"><span data-stu-id="a0766-325">dataUriToConvert</span></span> |<span data-ttu-id="a0766-326">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-326">Yes</span></span> |<span data-ttu-id="a0766-327">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-327">string</span></span> |<span data-ttu-id="a0766-328">hello adatok URI érték tooconvert.</span><span class="sxs-lookup"><span data-stu-id="a0766-328">hello data URI value tooconvert.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-329">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-329">Return value</span></span>

<span data-ttu-id="a0766-330">Hello tartalmazó karakterlánc érték alakítja.</span><span class="sxs-lookup"><span data-stu-id="a0766-330">A string containing hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-331">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-331">Examples</span></span>

<span data-ttu-id="a0766-332">a következő példa hello alakít egy értéket tooa adat-URI azonosító, és konvertálja az egy URI tooa karakterlánc adatokat:</span><span class="sxs-lookup"><span data-stu-id="a0766-332">hello following example converts a value tooa data URI, and converts a data URI tooa string:</span></span>

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

<span data-ttu-id="a0766-333">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-333">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-334">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-334">Name</span></span> | <span data-ttu-id="a0766-335">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-335">Type</span></span> | <span data-ttu-id="a0766-336">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-336">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-337">dataUriOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-337">dataUriOutput</span></span> | <span data-ttu-id="a0766-338">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-338">String</span></span> | <span data-ttu-id="a0766-339">adatok: text / egyszerű; charset = utf8; base64, SGVsbG8 =</span><span class="sxs-lookup"><span data-stu-id="a0766-339">data:text/plain;charset=utf8;base64,SGVsbG8=</span></span> |
| <span data-ttu-id="a0766-340">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-340">toStringOutput</span></span> | <span data-ttu-id="a0766-341">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-341">String</span></span> | <span data-ttu-id="a0766-342">helló világ!</span><span class="sxs-lookup"><span data-stu-id="a0766-342">Hello, World!</span></span> |

<a id="empty" /> 

## <a name="empty"></a><span data-ttu-id="a0766-343">üres</span><span class="sxs-lookup"><span data-stu-id="a0766-343">empty</span></span>
`empty(itemToTest)`

<span data-ttu-id="a0766-344">Meghatározza, hogy egy tömb, az objektum, vagy a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="a0766-344">Determines if an array, object, or string is empty.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-345">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-345">Parameters</span></span>

| <span data-ttu-id="a0766-346">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-346">Parameter</span></span> | <span data-ttu-id="a0766-347">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-347">Required</span></span> | <span data-ttu-id="a0766-348">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-348">Type</span></span> | <span data-ttu-id="a0766-349">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-349">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-350">itemToTest</span><span class="sxs-lookup"><span data-stu-id="a0766-350">itemToTest</span></span> |<span data-ttu-id="a0766-351">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-351">Yes</span></span> |<span data-ttu-id="a0766-352">a tömb, objektum vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-352">array, object, or string</span></span> |<span data-ttu-id="a0766-353">hello érték toocheck, ha üres.</span><span class="sxs-lookup"><span data-stu-id="a0766-353">hello value toocheck if it is empty.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-354">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-354">Return value</span></span>

<span data-ttu-id="a0766-355">Beolvasása **igaz** hello értéke üres, ha sikertelen, ha **hamis**.</span><span class="sxs-lookup"><span data-stu-id="a0766-355">Returns **True** if hello value is empty; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-356">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-356">Examples</span></span>

<span data-ttu-id="a0766-357">a következő példa hello ellenőrzi, hogy egy tömb, az objektumot, és a karakterlánc üres.</span><span class="sxs-lookup"><span data-stu-id="a0766-357">hello following example checks whether an array, object, and string are empty.</span></span>

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

<span data-ttu-id="a0766-358">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-358">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-359">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-359">Name</span></span> | <span data-ttu-id="a0766-360">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-360">Type</span></span> | <span data-ttu-id="a0766-361">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-361">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-362">arrayEmpty</span><span class="sxs-lookup"><span data-stu-id="a0766-362">arrayEmpty</span></span> | <span data-ttu-id="a0766-363">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-363">Bool</span></span> | <span data-ttu-id="a0766-364">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-364">True</span></span> |
| <span data-ttu-id="a0766-365">objectEmpty</span><span class="sxs-lookup"><span data-stu-id="a0766-365">objectEmpty</span></span> | <span data-ttu-id="a0766-366">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-366">Bool</span></span> | <span data-ttu-id="a0766-367">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-367">True</span></span> |
| <span data-ttu-id="a0766-368">stringEmpty</span><span class="sxs-lookup"><span data-stu-id="a0766-368">stringEmpty</span></span> | <span data-ttu-id="a0766-369">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-369">Bool</span></span> | <span data-ttu-id="a0766-370">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-370">True</span></span> |

<a id="endswith" />

## <a name="endswith"></a><span data-ttu-id="a0766-371">megadott módon végződő</span><span class="sxs-lookup"><span data-stu-id="a0766-371">endsWith</span></span>
`endsWith(stringToSearch, stringToFind)`

<span data-ttu-id="a0766-372">Meghatározza, hogy egy karakterláncot végződik-e értéket.</span><span class="sxs-lookup"><span data-stu-id="a0766-372">Determines whether a string ends with a value.</span></span> <span data-ttu-id="a0766-373">hello összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="a0766-373">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-374">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-374">Parameters</span></span>

| <span data-ttu-id="a0766-375">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-375">Parameter</span></span> | <span data-ttu-id="a0766-376">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-376">Required</span></span> | <span data-ttu-id="a0766-377">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-377">Type</span></span> | <span data-ttu-id="a0766-378">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-378">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-379">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="a0766-379">stringToSearch</span></span> |<span data-ttu-id="a0766-380">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-380">Yes</span></span> |<span data-ttu-id="a0766-381">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-381">string</span></span> |<span data-ttu-id="a0766-382">hello érték, amely hello elem toofind tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a0766-382">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="a0766-383">stringToFind</span><span class="sxs-lookup"><span data-stu-id="a0766-383">stringToFind</span></span> |<span data-ttu-id="a0766-384">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-384">Yes</span></span> |<span data-ttu-id="a0766-385">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-385">string</span></span> |<span data-ttu-id="a0766-386">hello érték toofind.</span><span class="sxs-lookup"><span data-stu-id="a0766-386">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-387">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-387">Return value</span></span>

<span data-ttu-id="a0766-388">**Igaz** Ha hello utolsó karaktert vagy karaktereket hello karakterlánc megfelel a hello érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="a0766-388">**True** if hello last character or characters of hello string match hello value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-389">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-389">Examples</span></span>

<span data-ttu-id="a0766-390">hello a következő példa bemutatja, hogyan toouse hello megadott módon kezdődő és megadott módon végződő funkciók:</span><span class="sxs-lookup"><span data-stu-id="a0766-390">hello following example shows how toouse hello startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="a0766-391">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-391">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-392">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-392">Name</span></span> | <span data-ttu-id="a0766-393">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-393">Type</span></span> | <span data-ttu-id="a0766-394">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-394">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-395">startsTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-395">startsTrue</span></span> | <span data-ttu-id="a0766-396">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-396">Bool</span></span> | <span data-ttu-id="a0766-397">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-397">True</span></span> |
| <span data-ttu-id="a0766-398">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-398">startsCapTrue</span></span> | <span data-ttu-id="a0766-399">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-399">Bool</span></span> | <span data-ttu-id="a0766-400">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-400">True</span></span> |
| <span data-ttu-id="a0766-401">startsFalse</span><span class="sxs-lookup"><span data-stu-id="a0766-401">startsFalse</span></span> | <span data-ttu-id="a0766-402">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-402">Bool</span></span> | <span data-ttu-id="a0766-403">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="a0766-403">False</span></span> |
| <span data-ttu-id="a0766-404">endsTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-404">endsTrue</span></span> | <span data-ttu-id="a0766-405">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-405">Bool</span></span> | <span data-ttu-id="a0766-406">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-406">True</span></span> |
| <span data-ttu-id="a0766-407">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-407">endsCapTrue</span></span> | <span data-ttu-id="a0766-408">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-408">Bool</span></span> | <span data-ttu-id="a0766-409">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-409">True</span></span> |
| <span data-ttu-id="a0766-410">endsFalse</span><span class="sxs-lookup"><span data-stu-id="a0766-410">endsFalse</span></span> | <span data-ttu-id="a0766-411">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-411">Bool</span></span> | <span data-ttu-id="a0766-412">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="a0766-412">False</span></span> |

<a id="first" />

## <a name="first"></a><span data-ttu-id="a0766-413">első</span><span class="sxs-lookup"><span data-stu-id="a0766-413">first</span></span>
`first(arg1)`

<span data-ttu-id="a0766-414">Beolvasása hello hello karakterlánc első karaktere vagy hello tömb első eleme.</span><span class="sxs-lookup"><span data-stu-id="a0766-414">Returns hello first character of hello string, or first element of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-415">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-415">Parameters</span></span>

| <span data-ttu-id="a0766-416">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-416">Parameter</span></span> | <span data-ttu-id="a0766-417">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-417">Required</span></span> | <span data-ttu-id="a0766-418">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-418">Type</span></span> | <span data-ttu-id="a0766-419">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-419">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-420">arg1</span><span class="sxs-lookup"><span data-stu-id="a0766-420">arg1</span></span> |<span data-ttu-id="a0766-421">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-421">Yes</span></span> |<span data-ttu-id="a0766-422">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-422">array or string</span></span> |<span data-ttu-id="a0766-423">hello érték tooretrieve hello első elem vagy karakter.</span><span class="sxs-lookup"><span data-stu-id="a0766-423">hello value tooretrieve hello first element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-424">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-424">Return value</span></span>

<span data-ttu-id="a0766-425">Egy karakterlánc első karaktere hello vagy hello típusú (karakterlánc, int, tömb vagy objektum) hello egy tömb első eleme.</span><span class="sxs-lookup"><span data-stu-id="a0766-425">A string of hello first character, or hello type (string, int, array, or object) of hello first element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-426">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-426">Examples</span></span>

<span data-ttu-id="a0766-427">hello következő példa bemutatja, hogyan toouse hello tömb és karakterlánc az első függvényét.</span><span class="sxs-lookup"><span data-stu-id="a0766-427">hello following example shows how toouse hello first function with an array and string.</span></span>

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

<span data-ttu-id="a0766-428">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-428">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-429">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-429">Name</span></span> | <span data-ttu-id="a0766-430">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-430">Type</span></span> | <span data-ttu-id="a0766-431">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-431">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-432">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-432">arrayOutput</span></span> | <span data-ttu-id="a0766-433">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-433">String</span></span> | <span data-ttu-id="a0766-434">egy</span><span class="sxs-lookup"><span data-stu-id="a0766-434">one</span></span> |
| <span data-ttu-id="a0766-435">stringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-435">stringOutput</span></span> | <span data-ttu-id="a0766-436">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-436">String</span></span> | <span data-ttu-id="a0766-437">O</span><span class="sxs-lookup"><span data-stu-id="a0766-437">O</span></span> |

<a id="indexof" />

## <a name="indexof"></a><span data-ttu-id="a0766-438">indexOf</span><span class="sxs-lookup"><span data-stu-id="a0766-438">indexOf</span></span>
`indexOf(stringToSearch, stringToFind)`

<span data-ttu-id="a0766-439">Értéket ad vissza egy érték, egy karakterláncon belüli első helyének hello.</span><span class="sxs-lookup"><span data-stu-id="a0766-439">Returns hello first position of a value within a string.</span></span> <span data-ttu-id="a0766-440">hello összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="a0766-440">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-441">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-441">Parameters</span></span>

| <span data-ttu-id="a0766-442">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-442">Parameter</span></span> | <span data-ttu-id="a0766-443">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-443">Required</span></span> | <span data-ttu-id="a0766-444">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-444">Type</span></span> | <span data-ttu-id="a0766-445">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-445">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-446">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="a0766-446">stringToSearch</span></span> |<span data-ttu-id="a0766-447">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-447">Yes</span></span> |<span data-ttu-id="a0766-448">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-448">string</span></span> |<span data-ttu-id="a0766-449">hello érték, amely hello elem toofind tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a0766-449">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="a0766-450">stringToFind</span><span class="sxs-lookup"><span data-stu-id="a0766-450">stringToFind</span></span> |<span data-ttu-id="a0766-451">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-451">Yes</span></span> |<span data-ttu-id="a0766-452">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-452">string</span></span> |<span data-ttu-id="a0766-453">hello érték toofind.</span><span class="sxs-lookup"><span data-stu-id="a0766-453">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-454">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-454">Return value</span></span>

<span data-ttu-id="a0766-455">Egész szám, amely hello elem toofind hello pozícióját jelöli.</span><span class="sxs-lookup"><span data-stu-id="a0766-455">An integer that represents hello position of hello item toofind.</span></span> <span data-ttu-id="a0766-456">hello értéke nulla.</span><span class="sxs-lookup"><span data-stu-id="a0766-456">hello value is zero-based.</span></span> <span data-ttu-id="a0766-457">Ha hello elem nem található,-1 értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-457">If hello item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-458">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-458">Examples</span></span>

<span data-ttu-id="a0766-459">hello a következő példa bemutatja, hogyan toouse hello indexOf és lastIndexOf funkciók:</span><span class="sxs-lookup"><span data-stu-id="a0766-459">hello following example shows how toouse hello indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="a0766-460">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-460">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-461">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-461">Name</span></span> | <span data-ttu-id="a0766-462">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-462">Type</span></span> | <span data-ttu-id="a0766-463">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-463">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-464">firstT</span><span class="sxs-lookup"><span data-stu-id="a0766-464">firstT</span></span> | <span data-ttu-id="a0766-465">int</span><span class="sxs-lookup"><span data-stu-id="a0766-465">Int</span></span> | <span data-ttu-id="a0766-466">0</span><span class="sxs-lookup"><span data-stu-id="a0766-466">0</span></span> |
| <span data-ttu-id="a0766-467">lastT</span><span class="sxs-lookup"><span data-stu-id="a0766-467">lastT</span></span> | <span data-ttu-id="a0766-468">int</span><span class="sxs-lookup"><span data-stu-id="a0766-468">Int</span></span> | <span data-ttu-id="a0766-469">3</span><span class="sxs-lookup"><span data-stu-id="a0766-469">3</span></span> |
| <span data-ttu-id="a0766-470">firstString</span><span class="sxs-lookup"><span data-stu-id="a0766-470">firstString</span></span> | <span data-ttu-id="a0766-471">int</span><span class="sxs-lookup"><span data-stu-id="a0766-471">Int</span></span> | <span data-ttu-id="a0766-472">2</span><span class="sxs-lookup"><span data-stu-id="a0766-472">2</span></span> |
| <span data-ttu-id="a0766-473">lastString</span><span class="sxs-lookup"><span data-stu-id="a0766-473">lastString</span></span> | <span data-ttu-id="a0766-474">int</span><span class="sxs-lookup"><span data-stu-id="a0766-474">Int</span></span> | <span data-ttu-id="a0766-475">0</span><span class="sxs-lookup"><span data-stu-id="a0766-475">0</span></span> |
| <span data-ttu-id="a0766-476">notFound</span><span class="sxs-lookup"><span data-stu-id="a0766-476">notFound</span></span> | <span data-ttu-id="a0766-477">int</span><span class="sxs-lookup"><span data-stu-id="a0766-477">Int</span></span> | <span data-ttu-id="a0766-478">-1</span><span class="sxs-lookup"><span data-stu-id="a0766-478">-1</span></span> |

<a id="last" />

## <a name="last"></a><span data-ttu-id="a0766-479">utolsó</span><span class="sxs-lookup"><span data-stu-id="a0766-479">last</span></span>
`last (arg1)`

<span data-ttu-id="a0766-480">Az utolsó karakter hello karakterlánc vagy hello hello tömb utolsó eleme adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-480">Returns last character of hello string, or hello last element of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-481">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-481">Parameters</span></span>

| <span data-ttu-id="a0766-482">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-482">Parameter</span></span> | <span data-ttu-id="a0766-483">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-483">Required</span></span> | <span data-ttu-id="a0766-484">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-484">Type</span></span> | <span data-ttu-id="a0766-485">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-485">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-486">arg1</span><span class="sxs-lookup"><span data-stu-id="a0766-486">arg1</span></span> |<span data-ttu-id="a0766-487">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-487">Yes</span></span> |<span data-ttu-id="a0766-488">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-488">array or string</span></span> |<span data-ttu-id="a0766-489">hello érték tooretrieve hello utolsó eleme vagy karakter.</span><span class="sxs-lookup"><span data-stu-id="a0766-489">hello value tooretrieve hello last element or character.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-490">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-490">Return value</span></span>

<span data-ttu-id="a0766-491">Egy karakterlánc hello utolsó karakter vagy hello típusú (karakterlánc, int, tömb vagy objektum) utolsó eleme hello tömbben.</span><span class="sxs-lookup"><span data-stu-id="a0766-491">A string of hello last character, or hello type (string, int, array, or object) of hello last element in an array.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-492">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-492">Examples</span></span>

<span data-ttu-id="a0766-493">hello következő példa bemutatja, hogyan toouse hello utolsó függvény egy tömböt és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-493">hello following example shows how toouse hello last function with an array and string.</span></span>

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

<span data-ttu-id="a0766-494">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-494">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-495">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-495">Name</span></span> | <span data-ttu-id="a0766-496">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-496">Type</span></span> | <span data-ttu-id="a0766-497">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-497">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-498">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-498">arrayOutput</span></span> | <span data-ttu-id="a0766-499">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-499">String</span></span> | <span data-ttu-id="a0766-500">három</span><span class="sxs-lookup"><span data-stu-id="a0766-500">three</span></span> |
| <span data-ttu-id="a0766-501">stringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-501">stringOutput</span></span> | <span data-ttu-id="a0766-502">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-502">String</span></span> | <span data-ttu-id="a0766-503">E</span><span class="sxs-lookup"><span data-stu-id="a0766-503">e</span></span> |

<a id="lastindexof" />

## <a name="lastindexof"></a><span data-ttu-id="a0766-504">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="a0766-504">lastIndexOf</span></span>
`lastIndexOf(stringToSearch, stringToFind)`

<span data-ttu-id="a0766-505">Értéket ad vissza egy érték, egy karakterláncon belüli utolsó helyének hello.</span><span class="sxs-lookup"><span data-stu-id="a0766-505">Returns hello last position of a value within a string.</span></span> <span data-ttu-id="a0766-506">hello összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="a0766-506">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-507">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-507">Parameters</span></span>

| <span data-ttu-id="a0766-508">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-508">Parameter</span></span> | <span data-ttu-id="a0766-509">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-509">Required</span></span> | <span data-ttu-id="a0766-510">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-510">Type</span></span> | <span data-ttu-id="a0766-511">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-511">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-512">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="a0766-512">stringToSearch</span></span> |<span data-ttu-id="a0766-513">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-513">Yes</span></span> |<span data-ttu-id="a0766-514">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-514">string</span></span> |<span data-ttu-id="a0766-515">hello érték, amely hello elem toofind tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a0766-515">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="a0766-516">stringToFind</span><span class="sxs-lookup"><span data-stu-id="a0766-516">stringToFind</span></span> |<span data-ttu-id="a0766-517">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-517">Yes</span></span> |<span data-ttu-id="a0766-518">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-518">string</span></span> |<span data-ttu-id="a0766-519">hello érték toofind.</span><span class="sxs-lookup"><span data-stu-id="a0766-519">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-520">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-520">Return value</span></span>

<span data-ttu-id="a0766-521">Egész szám, amely hello elem toofind hello utolsó pozícióját jelöli.</span><span class="sxs-lookup"><span data-stu-id="a0766-521">An integer that represents hello last position of hello item toofind.</span></span> <span data-ttu-id="a0766-522">hello értéke nulla.</span><span class="sxs-lookup"><span data-stu-id="a0766-522">hello value is zero-based.</span></span> <span data-ttu-id="a0766-523">Ha hello elem nem található,-1 értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-523">If hello item is not found, -1 is returned.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-524">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-524">Examples</span></span>

<span data-ttu-id="a0766-525">hello a következő példa bemutatja, hogyan toouse hello indexOf és lastIndexOf funkciók:</span><span class="sxs-lookup"><span data-stu-id="a0766-525">hello following example shows how toouse hello indexOf and lastIndexOf functions:</span></span>

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

<span data-ttu-id="a0766-526">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-526">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-527">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-527">Name</span></span> | <span data-ttu-id="a0766-528">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-528">Type</span></span> | <span data-ttu-id="a0766-529">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-529">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-530">firstT</span><span class="sxs-lookup"><span data-stu-id="a0766-530">firstT</span></span> | <span data-ttu-id="a0766-531">int</span><span class="sxs-lookup"><span data-stu-id="a0766-531">Int</span></span> | <span data-ttu-id="a0766-532">0</span><span class="sxs-lookup"><span data-stu-id="a0766-532">0</span></span> |
| <span data-ttu-id="a0766-533">lastT</span><span class="sxs-lookup"><span data-stu-id="a0766-533">lastT</span></span> | <span data-ttu-id="a0766-534">int</span><span class="sxs-lookup"><span data-stu-id="a0766-534">Int</span></span> | <span data-ttu-id="a0766-535">3</span><span class="sxs-lookup"><span data-stu-id="a0766-535">3</span></span> |
| <span data-ttu-id="a0766-536">firstString</span><span class="sxs-lookup"><span data-stu-id="a0766-536">firstString</span></span> | <span data-ttu-id="a0766-537">int</span><span class="sxs-lookup"><span data-stu-id="a0766-537">Int</span></span> | <span data-ttu-id="a0766-538">2</span><span class="sxs-lookup"><span data-stu-id="a0766-538">2</span></span> |
| <span data-ttu-id="a0766-539">lastString</span><span class="sxs-lookup"><span data-stu-id="a0766-539">lastString</span></span> | <span data-ttu-id="a0766-540">int</span><span class="sxs-lookup"><span data-stu-id="a0766-540">Int</span></span> | <span data-ttu-id="a0766-541">0</span><span class="sxs-lookup"><span data-stu-id="a0766-541">0</span></span> |
| <span data-ttu-id="a0766-542">notFound</span><span class="sxs-lookup"><span data-stu-id="a0766-542">notFound</span></span> | <span data-ttu-id="a0766-543">int</span><span class="sxs-lookup"><span data-stu-id="a0766-543">Int</span></span> | <span data-ttu-id="a0766-544">-1</span><span class="sxs-lookup"><span data-stu-id="a0766-544">-1</span></span> |

<a id="length" />

## <a name="length"></a><span data-ttu-id="a0766-545">Hossza</span><span class="sxs-lookup"><span data-stu-id="a0766-545">length</span></span>
`length(string)`

<span data-ttu-id="a0766-546">A karakterlánc vagy tömb elemeinek hello számú karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-546">Returns hello number of characters in a string, or elements in an array.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-547">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-547">Parameters</span></span>

| <span data-ttu-id="a0766-548">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-548">Parameter</span></span> | <span data-ttu-id="a0766-549">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-549">Required</span></span> | <span data-ttu-id="a0766-550">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-550">Type</span></span> | <span data-ttu-id="a0766-551">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-551">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-552">arg1</span><span class="sxs-lookup"><span data-stu-id="a0766-552">arg1</span></span> |<span data-ttu-id="a0766-553">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-553">Yes</span></span> |<span data-ttu-id="a0766-554">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-554">array or string</span></span> |<span data-ttu-id="a0766-555">első hello elemek száma a tömb toouse hello, vagy hello karakterlánc toouse kapcsolódnak a hello karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="a0766-555">hello array toouse for getting hello number of elements, or hello string toouse for getting hello number of characters.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-556">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-556">Return value</span></span>

<span data-ttu-id="a0766-557">Egy egész szám.</span><span class="sxs-lookup"><span data-stu-id="a0766-557">An int.</span></span> 

### <a name="examples"></a><span data-ttu-id="a0766-558">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-558">Examples</span></span>

<span data-ttu-id="a0766-559">a következő példa azt mutatja meg hogyan hello toouse hosszúságú tömb és karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="a0766-559">hello following example shows how toouse length with an array and string:</span></span>

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

<span data-ttu-id="a0766-560">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-560">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-561">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-561">Name</span></span> | <span data-ttu-id="a0766-562">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-562">Type</span></span> | <span data-ttu-id="a0766-563">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-563">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-564">arrayLength</span><span class="sxs-lookup"><span data-stu-id="a0766-564">arrayLength</span></span> | <span data-ttu-id="a0766-565">int</span><span class="sxs-lookup"><span data-stu-id="a0766-565">Int</span></span> | <span data-ttu-id="a0766-566">3</span><span class="sxs-lookup"><span data-stu-id="a0766-566">3</span></span> |
| <span data-ttu-id="a0766-567">stringLength</span><span class="sxs-lookup"><span data-stu-id="a0766-567">stringLength</span></span> | <span data-ttu-id="a0766-568">int</span><span class="sxs-lookup"><span data-stu-id="a0766-568">Int</span></span> | <span data-ttu-id="a0766-569">13</span><span class="sxs-lookup"><span data-stu-id="a0766-569">13</span></span> |

<a id="padleft" />

## <a name="padleft"></a><span data-ttu-id="a0766-570">PadLeft</span><span class="sxs-lookup"><span data-stu-id="a0766-570">padLeft</span></span>
`padLeft(valueToPad, totalLength, paddingCharacter)`

<span data-ttu-id="a0766-571">Hello teljes megadott hosszúságú eléréséig karakterek toohello balra hozzáadásával egy jobbra igazított karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-571">Returns a right-aligned string by adding characters toohello left until reaching hello total specified length.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-572">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-572">Parameters</span></span>

| <span data-ttu-id="a0766-573">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-573">Parameter</span></span> | <span data-ttu-id="a0766-574">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-574">Required</span></span> | <span data-ttu-id="a0766-575">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-575">Type</span></span> | <span data-ttu-id="a0766-576">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-576">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-577">valueToPad</span><span class="sxs-lookup"><span data-stu-id="a0766-577">valueToPad</span></span> |<span data-ttu-id="a0766-578">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-578">Yes</span></span> |<span data-ttu-id="a0766-579">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="a0766-579">string or int</span></span> |<span data-ttu-id="a0766-580">hello érték tooright-igazítása.</span><span class="sxs-lookup"><span data-stu-id="a0766-580">hello value tooright-align.</span></span> |
| <span data-ttu-id="a0766-581">totalLength</span><span class="sxs-lookup"><span data-stu-id="a0766-581">totalLength</span></span> |<span data-ttu-id="a0766-582">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-582">Yes</span></span> |<span data-ttu-id="a0766-583">int</span><span class="sxs-lookup"><span data-stu-id="a0766-583">int</span></span> |<span data-ttu-id="a0766-584">hello lévő karakterek összesített száma hello karakterláncot adott vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-584">hello total number of characters in hello returned string.</span></span> |
| <span data-ttu-id="a0766-585">paddingCharacter</span><span class="sxs-lookup"><span data-stu-id="a0766-585">paddingCharacter</span></span> |<span data-ttu-id="a0766-586">Nem</span><span class="sxs-lookup"><span data-stu-id="a0766-586">No</span></span> |<span data-ttu-id="a0766-587">egyetlen karakter</span><span class="sxs-lookup"><span data-stu-id="a0766-587">single character</span></span> |<span data-ttu-id="a0766-588">hello karakter toouse balra-nullákból amíg hello teljes hossza nem csökken.</span><span class="sxs-lookup"><span data-stu-id="a0766-588">hello character toouse for left-padding until hello total length is reached.</span></span> <span data-ttu-id="a0766-589">hello alapértelmezett érték adható meg.</span><span class="sxs-lookup"><span data-stu-id="a0766-589">hello default value is a space.</span></span> |

<span data-ttu-id="a0766-590">Ha hello eredeti karakterlánc hosszabb, mint karakterek toopad hello száma, egyetlen karakter kerülnek.</span><span class="sxs-lookup"><span data-stu-id="a0766-590">If hello original string is longer than hello number of characters toopad, no characters are added.</span></span>

### <a name="return-value"></a><span data-ttu-id="a0766-591">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-591">Return value</span></span>

<span data-ttu-id="a0766-592">A karakterláncnak legalább hello megadott karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="a0766-592">A string with at least hello number of specified characters.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-593">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-593">Examples</span></span>

<span data-ttu-id="a0766-594">hello a következő példa bemutatja, hogyan toopad hello felhasználó által megadott paraméterérték hello hozzáadásával nulla karakter eléréséig hello karakterek összesített száma.</span><span class="sxs-lookup"><span data-stu-id="a0766-594">hello following example shows how toopad hello user-provided parameter value by adding hello zero character until it reaches hello total number of characters.</span></span> 

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

<span data-ttu-id="a0766-595">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-595">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-596">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-596">Name</span></span> | <span data-ttu-id="a0766-597">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-597">Type</span></span> | <span data-ttu-id="a0766-598">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-598">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-599">stringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-599">stringOutput</span></span> | <span data-ttu-id="a0766-600">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-600">String</span></span> | <span data-ttu-id="a0766-601">0000000123</span><span class="sxs-lookup"><span data-stu-id="a0766-601">0000000123</span></span> |

<a id="replace" />

## <a name="replace"></a><span data-ttu-id="a0766-602">cserélje le</span><span class="sxs-lookup"><span data-stu-id="a0766-602">replace</span></span>
`replace(originalString, oldString, newString)`

<span data-ttu-id="a0766-603">Egy másik karakterlánc szerepét egy karakterlánc összes előfordulásának új karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-603">Returns a new string with all instances of one string replaced by another string.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-604">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-604">Parameters</span></span>

| <span data-ttu-id="a0766-605">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-605">Parameter</span></span> | <span data-ttu-id="a0766-606">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-606">Required</span></span> | <span data-ttu-id="a0766-607">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-607">Type</span></span> | <span data-ttu-id="a0766-608">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-608">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-609">originalString</span><span class="sxs-lookup"><span data-stu-id="a0766-609">originalString</span></span> |<span data-ttu-id="a0766-610">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-610">Yes</span></span> |<span data-ttu-id="a0766-611">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-611">string</span></span> |<span data-ttu-id="a0766-612">hello érték, amely egy karakterlánc másik karakterlánc szerepét minden példánya van.</span><span class="sxs-lookup"><span data-stu-id="a0766-612">hello value that has all instances of one string replaced by another string.</span></span> |
| <span data-ttu-id="a0766-613">oldString</span><span class="sxs-lookup"><span data-stu-id="a0766-613">oldString</span></span> |<span data-ttu-id="a0766-614">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-614">Yes</span></span> |<span data-ttu-id="a0766-615">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-615">string</span></span> |<span data-ttu-id="a0766-616">hello karakterlánc toobe távolítva eredeti hello karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="a0766-616">hello string toobe removed from hello original string.</span></span> |
| <span data-ttu-id="a0766-617">newString</span><span class="sxs-lookup"><span data-stu-id="a0766-617">newString</span></span> |<span data-ttu-id="a0766-618">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-618">Yes</span></span> |<span data-ttu-id="a0766-619">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-619">string</span></span> |<span data-ttu-id="a0766-620">hello karakterlánc tooadd helyett hello karakterlánc eltávolítva.</span><span class="sxs-lookup"><span data-stu-id="a0766-620">hello string tooadd in place of hello removed string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-621">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-621">Return value</span></span>

<span data-ttu-id="a0766-622">Hello karakterláncnak karakterek helyett.</span><span class="sxs-lookup"><span data-stu-id="a0766-622">A string with hello replaced characters.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-623">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-623">Examples</span></span>

<span data-ttu-id="a0766-624">hello a következő példa bemutatja, hogyan összes tooremove kötőjelekből hello felhasználó által megadott karakterláncból, és hogyan hello tooreplace részét karakterláncra.</span><span class="sxs-lookup"><span data-stu-id="a0766-624">hello following example shows how tooremove all dashes from hello user-provided string, and how tooreplace part of hello string with another string.</span></span>

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

<span data-ttu-id="a0766-625">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-625">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-626">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-626">Name</span></span> | <span data-ttu-id="a0766-627">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-627">Type</span></span> | <span data-ttu-id="a0766-628">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-628">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-629">firstOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-629">firstOutput</span></span> | <span data-ttu-id="a0766-630">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-630">String</span></span> | <span data-ttu-id="a0766-631">1231231234</span><span class="sxs-lookup"><span data-stu-id="a0766-631">1231231234</span></span> |
| <span data-ttu-id="a0766-632">secodeOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-632">secodeOutput</span></span> | <span data-ttu-id="a0766-633">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-633">String</span></span> | <span data-ttu-id="a0766-634">123-123-xxxx</span><span class="sxs-lookup"><span data-stu-id="a0766-634">123-123-xxxx</span></span> |

<a id="skip" />

## <a name="skip"></a><span data-ttu-id="a0766-635">Kihagyása</span><span class="sxs-lookup"><span data-stu-id="a0766-635">skip</span></span>
`skip(originalValue, numberToSkip)`

<span data-ttu-id="a0766-636">Minden hello karaktereket tartalmazó karakterláncot ad vissza, miután hello összes hello elem a megadott számú karakter vagy egy tömb, után hello megadott számú elemet.</span><span class="sxs-lookup"><span data-stu-id="a0766-636">Returns a string with all hello characters after hello specified number of characters, or an array with all hello elements after hello specified number of elements.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-637">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-637">Parameters</span></span>

| <span data-ttu-id="a0766-638">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-638">Parameter</span></span> | <span data-ttu-id="a0766-639">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-639">Required</span></span> | <span data-ttu-id="a0766-640">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-640">Type</span></span> | <span data-ttu-id="a0766-641">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-641">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-642">originalValue</span><span class="sxs-lookup"><span data-stu-id="a0766-642">originalValue</span></span> |<span data-ttu-id="a0766-643">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-643">Yes</span></span> |<span data-ttu-id="a0766-644">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-644">array or string</span></span> |<span data-ttu-id="a0766-645">hello tömb vagy karakterlánc toouse átugrásához.</span><span class="sxs-lookup"><span data-stu-id="a0766-645">hello array or string toouse for skipping.</span></span> |
| <span data-ttu-id="a0766-646">numberToSkip</span><span class="sxs-lookup"><span data-stu-id="a0766-646">numberToSkip</span></span> |<span data-ttu-id="a0766-647">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-647">Yes</span></span> |<span data-ttu-id="a0766-648">int</span><span class="sxs-lookup"><span data-stu-id="a0766-648">int</span></span> |<span data-ttu-id="a0766-649">elemek vagy karaktereket tooskip hello száma.</span><span class="sxs-lookup"><span data-stu-id="a0766-649">hello number of elements or characters tooskip.</span></span> <span data-ttu-id="a0766-650">Ha ez az érték 0 vagy kisebb, az összes elem hello, vagy karakter hello értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-650">If this value is 0 or less, all hello elements or characters in hello value are returned.</span></span> <span data-ttu-id="a0766-651">Ha hello hello tömb vagy karakterlánc hossza nagyobb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-651">If it is larger than hello length of hello array or string, an empty array or string is returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-652">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-652">Return value</span></span>

<span data-ttu-id="a0766-653">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-653">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-654">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-654">Examples</span></span>

<span data-ttu-id="a0766-655">a következő példa átugrása hello hello hello tömb megadott számú elemet, és hello egy karakterláncban megadott számú karaktert.</span><span class="sxs-lookup"><span data-stu-id="a0766-655">hello following example skips hello specified number of elements in hello array, and hello specified number of characters in a string.</span></span>

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

<span data-ttu-id="a0766-656">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-656">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-657">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-657">Name</span></span> | <span data-ttu-id="a0766-658">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-658">Type</span></span> | <span data-ttu-id="a0766-659">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-659">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-660">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-660">arrayOutput</span></span> | <span data-ttu-id="a0766-661">Tömb</span><span class="sxs-lookup"><span data-stu-id="a0766-661">Array</span></span> | <span data-ttu-id="a0766-662">["három"]</span><span class="sxs-lookup"><span data-stu-id="a0766-662">["three"]</span></span> |
| <span data-ttu-id="a0766-663">stringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-663">stringOutput</span></span> | <span data-ttu-id="a0766-664">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-664">String</span></span> | <span data-ttu-id="a0766-665">két három</span><span class="sxs-lookup"><span data-stu-id="a0766-665">two three</span></span> |

<a id="split" />

## <a name="split"></a><span data-ttu-id="a0766-666">felosztás</span><span class="sxs-lookup"><span data-stu-id="a0766-666">split</span></span>
`split(inputString, delimiter)`

<span data-ttu-id="a0766-667">Értéket ad vissza, amely tartalmazza a hello hello karakterláncrészletek karakterláncokból álló tömb bemeneti karakterlánc, amely hello határolja megadott elválasztó karaktert.</span><span class="sxs-lookup"><span data-stu-id="a0766-667">Returns an array of strings that contains hello substrings of hello input string that are delimited by hello specified delimiters.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-668">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-668">Parameters</span></span>

| <span data-ttu-id="a0766-669">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-669">Parameter</span></span> | <span data-ttu-id="a0766-670">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-670">Required</span></span> | <span data-ttu-id="a0766-671">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-671">Type</span></span> | <span data-ttu-id="a0766-672">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-672">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-673">inputString</span><span class="sxs-lookup"><span data-stu-id="a0766-673">inputString</span></span> |<span data-ttu-id="a0766-674">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-674">Yes</span></span> |<span data-ttu-id="a0766-675">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-675">string</span></span> |<span data-ttu-id="a0766-676">hello karakterlánc toosplit.</span><span class="sxs-lookup"><span data-stu-id="a0766-676">hello string toosplit.</span></span> |
| <span data-ttu-id="a0766-677">Elválasztó</span><span class="sxs-lookup"><span data-stu-id="a0766-677">delimiter</span></span> |<span data-ttu-id="a0766-678">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-678">Yes</span></span> |<span data-ttu-id="a0766-679">karakterláncot vagy karakterláncok</span><span class="sxs-lookup"><span data-stu-id="a0766-679">string or array of strings</span></span> |<span data-ttu-id="a0766-680">hello elválasztó toouse vágását meghatározó hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-680">hello delimiter toouse for splitting hello string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-681">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-681">Return value</span></span>

<span data-ttu-id="a0766-682">Karakterláncokból álló tömb.</span><span class="sxs-lookup"><span data-stu-id="a0766-682">An array of strings.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-683">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-683">Examples</span></span>

<span data-ttu-id="a0766-684">hello alábbi példa felosztja a bemeneti karakterlánc hello vesszővel válassza el, és vesszővel vagy pontosvesszővel kell elválasztani őket.</span><span class="sxs-lookup"><span data-stu-id="a0766-684">hello following example splits hello input string with a comma, and with either a comma or a semi-colon.</span></span>

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

<span data-ttu-id="a0766-685">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-685">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-686">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-686">Name</span></span> | <span data-ttu-id="a0766-687">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-687">Type</span></span> | <span data-ttu-id="a0766-688">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-688">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-689">firstOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-689">firstOutput</span></span> | <span data-ttu-id="a0766-690">Tömb</span><span class="sxs-lookup"><span data-stu-id="a0766-690">Array</span></span> | <span data-ttu-id="a0766-691">["egy", "két", "három"]</span><span class="sxs-lookup"><span data-stu-id="a0766-691">["one", "two", "three"]</span></span> |
| <span data-ttu-id="a0766-692">secondOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-692">secondOutput</span></span> | <span data-ttu-id="a0766-693">Tömb</span><span class="sxs-lookup"><span data-stu-id="a0766-693">Array</span></span> | <span data-ttu-id="a0766-694">["egy", "két", "három"]</span><span class="sxs-lookup"><span data-stu-id="a0766-694">["one", "two", "three"]</span></span> |

<a id="startswith" />

## <a name="startswith"></a><span data-ttu-id="a0766-695">startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="a0766-695">startsWith</span></span>
`startsWith(stringToSearch, stringToFind)`

<span data-ttu-id="a0766-696">Meghatározza, hogy egy karakterlánc értékkel kezdődik-e.</span><span class="sxs-lookup"><span data-stu-id="a0766-696">Determines whether a string starts with a value.</span></span> <span data-ttu-id="a0766-697">hello összehasonlítás esetén azonban nem.</span><span class="sxs-lookup"><span data-stu-id="a0766-697">hello comparison is case-insensitive.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-698">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-698">Parameters</span></span>

| <span data-ttu-id="a0766-699">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-699">Parameter</span></span> | <span data-ttu-id="a0766-700">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-700">Required</span></span> | <span data-ttu-id="a0766-701">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-701">Type</span></span> | <span data-ttu-id="a0766-702">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-702">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-703">stringToSearch</span><span class="sxs-lookup"><span data-stu-id="a0766-703">stringToSearch</span></span> |<span data-ttu-id="a0766-704">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-704">Yes</span></span> |<span data-ttu-id="a0766-705">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-705">string</span></span> |<span data-ttu-id="a0766-706">hello érték, amely hello elem toofind tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a0766-706">hello value that contains hello item toofind.</span></span> |
| <span data-ttu-id="a0766-707">stringToFind</span><span class="sxs-lookup"><span data-stu-id="a0766-707">stringToFind</span></span> |<span data-ttu-id="a0766-708">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-708">Yes</span></span> |<span data-ttu-id="a0766-709">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-709">string</span></span> |<span data-ttu-id="a0766-710">hello érték toofind.</span><span class="sxs-lookup"><span data-stu-id="a0766-710">hello value toofind.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-711">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-711">Return value</span></span>

<span data-ttu-id="a0766-712">**Igaz** Ha hello első karaktert vagy karaktereket hello karakterlánc megfelel a hello érték; ellenkező esetben **hamis**.</span><span class="sxs-lookup"><span data-stu-id="a0766-712">**True** if hello first character or characters of hello string match hello value; otherwise, **False**.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-713">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-713">Examples</span></span>

<span data-ttu-id="a0766-714">hello a következő példa bemutatja, hogyan toouse hello megadott módon kezdődő és megadott módon végződő funkciók:</span><span class="sxs-lookup"><span data-stu-id="a0766-714">hello following example shows how toouse hello startsWith and endsWith functions:</span></span>

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

<span data-ttu-id="a0766-715">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-715">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-716">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-716">Name</span></span> | <span data-ttu-id="a0766-717">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-717">Type</span></span> | <span data-ttu-id="a0766-718">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-718">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-719">startsTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-719">startsTrue</span></span> | <span data-ttu-id="a0766-720">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-720">Bool</span></span> | <span data-ttu-id="a0766-721">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-721">True</span></span> |
| <span data-ttu-id="a0766-722">startsCapTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-722">startsCapTrue</span></span> | <span data-ttu-id="a0766-723">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-723">Bool</span></span> | <span data-ttu-id="a0766-724">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-724">True</span></span> |
| <span data-ttu-id="a0766-725">startsFalse</span><span class="sxs-lookup"><span data-stu-id="a0766-725">startsFalse</span></span> | <span data-ttu-id="a0766-726">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-726">Bool</span></span> | <span data-ttu-id="a0766-727">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="a0766-727">False</span></span> |
| <span data-ttu-id="a0766-728">endsTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-728">endsTrue</span></span> | <span data-ttu-id="a0766-729">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-729">Bool</span></span> | <span data-ttu-id="a0766-730">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-730">True</span></span> |
| <span data-ttu-id="a0766-731">endsCapTrue</span><span class="sxs-lookup"><span data-stu-id="a0766-731">endsCapTrue</span></span> | <span data-ttu-id="a0766-732">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-732">Bool</span></span> | <span data-ttu-id="a0766-733">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="a0766-733">True</span></span> |
| <span data-ttu-id="a0766-734">endsFalse</span><span class="sxs-lookup"><span data-stu-id="a0766-734">endsFalse</span></span> | <span data-ttu-id="a0766-735">logikai érték</span><span class="sxs-lookup"><span data-stu-id="a0766-735">Bool</span></span> | <span data-ttu-id="a0766-736">False (Hamis)</span><span class="sxs-lookup"><span data-stu-id="a0766-736">False</span></span> |

<a id="string" />

## <a name="string"></a><span data-ttu-id="a0766-737">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-737">string</span></span>
`string(valueToConvert)`

<span data-ttu-id="a0766-738">Konvertálja hello megadott érték tooa karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-738">Converts hello specified value tooa string.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-739">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-739">Parameters</span></span>

| <span data-ttu-id="a0766-740">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-740">Parameter</span></span> | <span data-ttu-id="a0766-741">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-741">Required</span></span> | <span data-ttu-id="a0766-742">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-742">Type</span></span> | <span data-ttu-id="a0766-743">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-743">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-744">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="a0766-744">valueToConvert</span></span> |<span data-ttu-id="a0766-745">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-745">Yes</span></span> | <span data-ttu-id="a0766-746">Bármelyik</span><span class="sxs-lookup"><span data-stu-id="a0766-746">Any</span></span> |<span data-ttu-id="a0766-747">hello érték tooconvert toostring.</span><span class="sxs-lookup"><span data-stu-id="a0766-747">hello value tooconvert toostring.</span></span> <span data-ttu-id="a0766-748">Bármely típusú érték lehet konvertálni, többek között az objektumok és tömböket.</span><span class="sxs-lookup"><span data-stu-id="a0766-748">Any type of value can be converted, including objects and arrays.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-749">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-749">Return value</span></span>

<span data-ttu-id="a0766-750">Hello karakterlánc konvertálni az értéket.</span><span class="sxs-lookup"><span data-stu-id="a0766-750">A string of hello converted value.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-751">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-751">Examples</span></span>

<span data-ttu-id="a0766-752">hello a következő példa bemutatja, hogyan tooconvert különböző típusú értékek toostrings:</span><span class="sxs-lookup"><span data-stu-id="a0766-752">hello following example shows how tooconvert different types of values toostrings:</span></span>

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

<span data-ttu-id="a0766-753">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-753">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-754">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-754">Name</span></span> | <span data-ttu-id="a0766-755">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-755">Type</span></span> | <span data-ttu-id="a0766-756">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-756">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-757">objectOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-757">objectOutput</span></span> | <span data-ttu-id="a0766-758">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-758">String</span></span> | <span data-ttu-id="a0766-759">{"valueA": 10, "valueB": "Példaszöveg"}</span><span class="sxs-lookup"><span data-stu-id="a0766-759">{"valueA":10,"valueB":"Example Text"}</span></span> |
| <span data-ttu-id="a0766-760">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-760">arrayOutput</span></span> | <span data-ttu-id="a0766-761">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-761">String</span></span> | <span data-ttu-id="a0766-762">["a", "b", "c"]</span><span class="sxs-lookup"><span data-stu-id="a0766-762">["a","b","c"]</span></span> |
| <span data-ttu-id="a0766-763">intOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-763">intOutput</span></span> | <span data-ttu-id="a0766-764">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-764">String</span></span> | <span data-ttu-id="a0766-765">5</span><span class="sxs-lookup"><span data-stu-id="a0766-765">5</span></span> |

<a id="substring" />

## <a name="substring"></a><span data-ttu-id="a0766-766">Substring</span><span class="sxs-lookup"><span data-stu-id="a0766-766">substring</span></span>
`substring(stringToParse, startIndex, length)`

<span data-ttu-id="a0766-767">Visszaadja, hogy a megadott hello kezdődik karakter pozíciója, és tartalmazza a hello karakterláncrész megadott számú karaktert.</span><span class="sxs-lookup"><span data-stu-id="a0766-767">Returns a substring that starts at hello specified character position and contains hello specified number of characters.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-768">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-768">Parameters</span></span>

| <span data-ttu-id="a0766-769">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-769">Parameter</span></span> | <span data-ttu-id="a0766-770">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-770">Required</span></span> | <span data-ttu-id="a0766-771">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-771">Type</span></span> | <span data-ttu-id="a0766-772">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-772">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-773">stringToParse</span><span class="sxs-lookup"><span data-stu-id="a0766-773">stringToParse</span></span> |<span data-ttu-id="a0766-774">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-774">Yes</span></span> |<span data-ttu-id="a0766-775">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-775">string</span></span> |<span data-ttu-id="a0766-776">hello eredeti karakterlánc, mely hello karakterláncrészletet ki kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="a0766-776">hello original string from which hello substring is extracted.</span></span> |
| <span data-ttu-id="a0766-777">startIndex</span><span class="sxs-lookup"><span data-stu-id="a0766-777">startIndex</span></span> |<span data-ttu-id="a0766-778">Nem</span><span class="sxs-lookup"><span data-stu-id="a0766-778">No</span></span> |<span data-ttu-id="a0766-779">int</span><span class="sxs-lookup"><span data-stu-id="a0766-779">int</span></span> |<span data-ttu-id="a0766-780">hello nulla alapú karakter kezdőpozíciója hello karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="a0766-780">hello zero-based starting character position for hello substring.</span></span> |
| <span data-ttu-id="a0766-781">Hossza</span><span class="sxs-lookup"><span data-stu-id="a0766-781">length</span></span> |<span data-ttu-id="a0766-782">Nem</span><span class="sxs-lookup"><span data-stu-id="a0766-782">No</span></span> |<span data-ttu-id="a0766-783">int</span><span class="sxs-lookup"><span data-stu-id="a0766-783">int</span></span> |<span data-ttu-id="a0766-784">hello karakterszámot hello karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="a0766-784">hello number of characters for hello substring.</span></span> <span data-ttu-id="a0766-785">Hello karakterláncon belüli tooa helyet kell képviselnie.</span><span class="sxs-lookup"><span data-stu-id="a0766-785">Must refer tooa location within hello string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-786">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-786">Return value</span></span>

<span data-ttu-id="a0766-787">hello karakterláncrészletet.</span><span class="sxs-lookup"><span data-stu-id="a0766-787">hello substring.</span></span>

### <a name="remarks"></a><span data-ttu-id="a0766-788">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a0766-788">Remarks</span></span>

<span data-ttu-id="a0766-789">hello függvény sikertelen lesz, amikor hello substring túlnyúlik hello karakterlánc hello végét.</span><span class="sxs-lookup"><span data-stu-id="a0766-789">hello function fails when hello substring extends beyond hello end of hello string.</span></span> <span data-ttu-id="a0766-790">a következő példa hello meghiúsul, és hello hiba "hello indexét és hosszát paraméterek hivatkozhatnak hello karakterlánc tooa helyét.</span><span class="sxs-lookup"><span data-stu-id="a0766-790">hello following example fails with hello error "hello index and length parameters must refer tooa location within hello string.</span></span> <span data-ttu-id="a0766-791">hello Indexparaméter: "0" hello hosszparaméter: "11" hello hello karakterlánc-paraméter hossza: "10". ".</span><span class="sxs-lookup"><span data-stu-id="a0766-791">hello index parameter: '0', hello length parameter: '11', hello length of hello string parameter: '10'.".</span></span>

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a><span data-ttu-id="a0766-792">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-792">Examples</span></span>

<span data-ttu-id="a0766-793">a következő példa hello karakterláncrész kiolvassa a paramétert.</span><span class="sxs-lookup"><span data-stu-id="a0766-793">hello following example extracts a substring from a parameter.</span></span>

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

<span data-ttu-id="a0766-794">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-794">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-795">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-795">Name</span></span> | <span data-ttu-id="a0766-796">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-796">Type</span></span> | <span data-ttu-id="a0766-797">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-797">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-798">substringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-798">substringOutput</span></span> | <span data-ttu-id="a0766-799">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-799">String</span></span> | <span data-ttu-id="a0766-800">két</span><span class="sxs-lookup"><span data-stu-id="a0766-800">two</span></span> |


<a id="take" />

## <a name="take"></a><span data-ttu-id="a0766-801">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="a0766-801">take</span></span>
`take(originalValue, numberToTake)`

<span data-ttu-id="a0766-802">Hello egy karakterlánc megadott számú karaktert hello kezdetét adja vissza hello karakterlánc, vagy egy tömb hello megadott hello indítás hello tömb elemeinek száma.</span><span class="sxs-lookup"><span data-stu-id="a0766-802">Returns a string with hello specified number of characters from hello start of hello string, or an array with hello specified number of elements from hello start of hello array.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-803">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-803">Parameters</span></span>

| <span data-ttu-id="a0766-804">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-804">Parameter</span></span> | <span data-ttu-id="a0766-805">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-805">Required</span></span> | <span data-ttu-id="a0766-806">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-806">Type</span></span> | <span data-ttu-id="a0766-807">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-807">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-808">originalValue</span><span class="sxs-lookup"><span data-stu-id="a0766-808">originalValue</span></span> |<span data-ttu-id="a0766-809">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-809">Yes</span></span> |<span data-ttu-id="a0766-810">a tömb vagy karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-810">array or string</span></span> |<span data-ttu-id="a0766-811">hello tömb vagy karakterlánc tootake hello elemeit.</span><span class="sxs-lookup"><span data-stu-id="a0766-811">hello array or string tootake hello elements from.</span></span> |
| <span data-ttu-id="a0766-812">numberToTake</span><span class="sxs-lookup"><span data-stu-id="a0766-812">numberToTake</span></span> |<span data-ttu-id="a0766-813">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-813">Yes</span></span> |<span data-ttu-id="a0766-814">int</span><span class="sxs-lookup"><span data-stu-id="a0766-814">int</span></span> |<span data-ttu-id="a0766-815">elemek vagy karaktereket tootake hello száma.</span><span class="sxs-lookup"><span data-stu-id="a0766-815">hello number of elements or characters tootake.</span></span> <span data-ttu-id="a0766-816">Ha ez az érték 0 vagy kisebb, üres tömb vagy karakterlánc adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-816">If this value is 0 or less, an empty array or string is returned.</span></span> <span data-ttu-id="a0766-817">Ha nagyobb, mint a megadott tömb vagy karakterlánc hello hello hosszát, visszaadja a hello tömb vagy karakterlánc összes hello eleme.</span><span class="sxs-lookup"><span data-stu-id="a0766-817">If it is larger than hello length of hello given array or string, all hello elements in hello array or string are returned.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-818">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-818">Return value</span></span>

<span data-ttu-id="a0766-819">Tömb vagy karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-819">An array or string.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-820">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-820">Examples</span></span>

<span data-ttu-id="a0766-821">a következő példa vesz hello hello megadott hello tömb elemei, és egy karakterláncból karakterek száma.</span><span class="sxs-lookup"><span data-stu-id="a0766-821">hello following example takes hello specified number of elements from hello array, and characters from a string.</span></span>

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

<span data-ttu-id="a0766-822">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-822">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-823">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-823">Name</span></span> | <span data-ttu-id="a0766-824">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-824">Type</span></span> | <span data-ttu-id="a0766-825">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-825">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-826">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-826">arrayOutput</span></span> | <span data-ttu-id="a0766-827">Tömb</span><span class="sxs-lookup"><span data-stu-id="a0766-827">Array</span></span> | <span data-ttu-id="a0766-828">["egy", "két"]</span><span class="sxs-lookup"><span data-stu-id="a0766-828">["one", "two"]</span></span> |
| <span data-ttu-id="a0766-829">stringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-829">stringOutput</span></span> | <span data-ttu-id="a0766-830">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-830">String</span></span> | <span data-ttu-id="a0766-831">a</span><span class="sxs-lookup"><span data-stu-id="a0766-831">on</span></span> |

<a id="tolower" />

## <a name="tolower"></a><span data-ttu-id="a0766-832">toLower</span><span class="sxs-lookup"><span data-stu-id="a0766-832">toLower</span></span>
`toLower(stringToChange)`

<span data-ttu-id="a0766-833">A megadott karakterlánc toolower eset konvertálja hello.</span><span class="sxs-lookup"><span data-stu-id="a0766-833">Converts hello specified string toolower case.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-834">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-834">Parameters</span></span>

| <span data-ttu-id="a0766-835">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-835">Parameter</span></span> | <span data-ttu-id="a0766-836">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-836">Required</span></span> | <span data-ttu-id="a0766-837">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-837">Type</span></span> | <span data-ttu-id="a0766-838">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-838">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-839">stringToChange</span><span class="sxs-lookup"><span data-stu-id="a0766-839">stringToChange</span></span> |<span data-ttu-id="a0766-840">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-840">Yes</span></span> |<span data-ttu-id="a0766-841">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-841">string</span></span> |<span data-ttu-id="a0766-842">hello érték tooconvert toolower eset.</span><span class="sxs-lookup"><span data-stu-id="a0766-842">hello value tooconvert toolower case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-843">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-843">Return value</span></span>

<span data-ttu-id="a0766-844">a konvertált toolower eset hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-844">hello string converted toolower case.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-845">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-845">Examples</span></span>

<span data-ttu-id="a0766-846">a következő példa hello alakít egy paraméter értéke toolower eset és tooupper eset.</span><span class="sxs-lookup"><span data-stu-id="a0766-846">hello following example converts a parameter value toolower case and tooupper case.</span></span>

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

<span data-ttu-id="a0766-847">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-847">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-848">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-848">Name</span></span> | <span data-ttu-id="a0766-849">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-849">Type</span></span> | <span data-ttu-id="a0766-850">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-850">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-851">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-851">toLowerOutput</span></span> | <span data-ttu-id="a0766-852">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-852">String</span></span> | <span data-ttu-id="a0766-853">egy két há'</span><span class="sxs-lookup"><span data-stu-id="a0766-853">one two three</span></span> |
| <span data-ttu-id="a0766-854">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-854">toUpperOutput</span></span> | <span data-ttu-id="a0766-855">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-855">String</span></span> | <span data-ttu-id="a0766-856">EGY KÉT HÁ'</span><span class="sxs-lookup"><span data-stu-id="a0766-856">ONE TWO THREE</span></span> |

<a id="toupper" />

## <a name="toupper"></a><span data-ttu-id="a0766-857">toUpper</span><span class="sxs-lookup"><span data-stu-id="a0766-857">toUpper</span></span>
`toUpper(stringToChange)`

<span data-ttu-id="a0766-858">A megadott karakterlánc tooupper eset konvertálja hello.</span><span class="sxs-lookup"><span data-stu-id="a0766-858">Converts hello specified string tooupper case.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-859">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-859">Parameters</span></span>

| <span data-ttu-id="a0766-860">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-860">Parameter</span></span> | <span data-ttu-id="a0766-861">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-861">Required</span></span> | <span data-ttu-id="a0766-862">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-862">Type</span></span> | <span data-ttu-id="a0766-863">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-863">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-864">stringToChange</span><span class="sxs-lookup"><span data-stu-id="a0766-864">stringToChange</span></span> |<span data-ttu-id="a0766-865">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-865">Yes</span></span> |<span data-ttu-id="a0766-866">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-866">string</span></span> |<span data-ttu-id="a0766-867">hello érték tooconvert tooupper eset.</span><span class="sxs-lookup"><span data-stu-id="a0766-867">hello value tooconvert tooupper case.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-868">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-868">Return value</span></span>

<span data-ttu-id="a0766-869">a konvertált tooupper eset hello karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-869">hello string converted tooupper case.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-870">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-870">Examples</span></span>

<span data-ttu-id="a0766-871">a következő példa hello alakít egy paraméter értéke toolower eset és tooupper eset.</span><span class="sxs-lookup"><span data-stu-id="a0766-871">hello following example converts a parameter value toolower case and tooupper case.</span></span>

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

<span data-ttu-id="a0766-872">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-872">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-873">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-873">Name</span></span> | <span data-ttu-id="a0766-874">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-874">Type</span></span> | <span data-ttu-id="a0766-875">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-875">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-876">toLowerOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-876">toLowerOutput</span></span> | <span data-ttu-id="a0766-877">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-877">String</span></span> | <span data-ttu-id="a0766-878">egy két há'</span><span class="sxs-lookup"><span data-stu-id="a0766-878">one two three</span></span> |
| <span data-ttu-id="a0766-879">toUpperOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-879">toUpperOutput</span></span> | <span data-ttu-id="a0766-880">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-880">String</span></span> | <span data-ttu-id="a0766-881">EGY KÉT HÁ'</span><span class="sxs-lookup"><span data-stu-id="a0766-881">ONE TWO THREE</span></span> |

<a id="trim" />

## <a name="trim"></a><span data-ttu-id="a0766-882">Trim</span><span class="sxs-lookup"><span data-stu-id="a0766-882">trim</span></span>
`trim (stringToTrim)`

<span data-ttu-id="a0766-883">Eltávolítja az összes kezdő és záró üres karaktereket hello a megadott karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-883">Removes all leading and trailing white-space characters from hello specified string.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-884">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-884">Parameters</span></span>

| <span data-ttu-id="a0766-885">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-885">Parameter</span></span> | <span data-ttu-id="a0766-886">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-886">Required</span></span> | <span data-ttu-id="a0766-887">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-887">Type</span></span> | <span data-ttu-id="a0766-888">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-888">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-889">stringToTrim</span><span class="sxs-lookup"><span data-stu-id="a0766-889">stringToTrim</span></span> |<span data-ttu-id="a0766-890">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-890">Yes</span></span> |<span data-ttu-id="a0766-891">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-891">string</span></span> |<span data-ttu-id="a0766-892">hello érték tootrim.</span><span class="sxs-lookup"><span data-stu-id="a0766-892">hello value tootrim.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-893">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-893">Return value</span></span>

<span data-ttu-id="a0766-894">kezdő és záró szóköz karakterek nélküli karakterláncot hello.</span><span class="sxs-lookup"><span data-stu-id="a0766-894">hello string without leading and trailing white-space characters.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-895">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-895">Examples</span></span>

<span data-ttu-id="a0766-896">hello alábbi példa levágja hello üres karaktereket hello paraméter.</span><span class="sxs-lookup"><span data-stu-id="a0766-896">hello following example trims hello white-space characters from hello parameter.</span></span>

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

<span data-ttu-id="a0766-897">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-897">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-898">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-898">Name</span></span> | <span data-ttu-id="a0766-899">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-899">Type</span></span> | <span data-ttu-id="a0766-900">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-900">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-901">térjen vissza</span><span class="sxs-lookup"><span data-stu-id="a0766-901">return</span></span> | <span data-ttu-id="a0766-902">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-902">String</span></span> | <span data-ttu-id="a0766-903">egy két há'</span><span class="sxs-lookup"><span data-stu-id="a0766-903">one two three</span></span> |

<a id="uniquestring" />

## <a name="uniquestring"></a><span data-ttu-id="a0766-904">uniqueString</span><span class="sxs-lookup"><span data-stu-id="a0766-904">uniqueString</span></span>
`uniqueString (baseString, ...)`

<span data-ttu-id="a0766-905">A paraméterként megadott hello értékek alapján determinisztikus kivonat karakterláncot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a0766-905">Creates a deterministic hash string based on hello values provided as parameters.</span></span> 

### <a name="parameters"></a><span data-ttu-id="a0766-906">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-906">Parameters</span></span>

| <span data-ttu-id="a0766-907">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-907">Parameter</span></span> | <span data-ttu-id="a0766-908">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-908">Required</span></span> | <span data-ttu-id="a0766-909">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-909">Type</span></span> | <span data-ttu-id="a0766-910">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-910">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-911">baseString</span><span class="sxs-lookup"><span data-stu-id="a0766-911">baseString</span></span> |<span data-ttu-id="a0766-912">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-912">Yes</span></span> |<span data-ttu-id="a0766-913">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-913">string</span></span> |<span data-ttu-id="a0766-914">hello értéket használja hello kivonatoló függvényt toocreate egy egyedi karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-914">hello value used in hello hash function toocreate a unique string.</span></span> |
| <span data-ttu-id="a0766-915">szükség szerint további paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-915">additional parameters as needed</span></span> |<span data-ttu-id="a0766-916">Nem</span><span class="sxs-lookup"><span data-stu-id="a0766-916">No</span></span> |<span data-ttu-id="a0766-917">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-917">string</span></span> |<span data-ttu-id="a0766-918">Tetszőleges számú karakterláncok értékként szükséges toocreate hello egyediségi szintű hello megadó adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="a0766-918">You can add as many strings as needed toocreate hello value that specifies hello level of uniqueness.</span></span> |

### <a name="remarks"></a><span data-ttu-id="a0766-919">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a0766-919">Remarks</span></span>

<span data-ttu-id="a0766-920">Ez a funkció akkor hasznos, ha egy erőforrást egy egyedi nevet toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a0766-920">This function is helpful when you need toocreate a unique name for a resource.</span></span> <span data-ttu-id="a0766-921">Hello eredmény hello egyediségének hatókörét korlátozó paraméter értékeket ad meg.</span><span class="sxs-lookup"><span data-stu-id="a0766-921">You provide parameter values that limit hello scope of uniqueness for hello result.</span></span> <span data-ttu-id="a0766-922">Megadható, hogy hello név egyedi toosubscription, erőforráscsoport és központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="a0766-922">You can specify whether hello name is unique down toosubscription, resource group, or deployment.</span></span> 

<span data-ttu-id="a0766-923">hello érték nincs véletlenszerű karakterlánc, de hello ahelyett, hogy a kivonatoló függvényt eredményét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-923">hello returned value is not a random string, but rather hello result of a hash function.</span></span> <span data-ttu-id="a0766-924">hello visszaadott érték 13 karakterig.</span><span class="sxs-lookup"><span data-stu-id="a0766-924">hello returned value is 13 characters long.</span></span> <span data-ttu-id="a0766-925">Nincs globálisan egyedi.</span><span class="sxs-lookup"><span data-stu-id="a0766-925">It is not globally unique.</span></span> <span data-ttu-id="a0766-926">Az elnevezési egyezmény toocreate, kifejező nevet a érdemes lehet toocombine hello érték előtaggal kezdődik.</span><span class="sxs-lookup"><span data-stu-id="a0766-926">You may want toocombine hello value with a prefix from your naming convention toocreate a name that is meaningful.</span></span> <span data-ttu-id="a0766-927">hello következő példa bemutatja hello formátuma hello értéket adott vissza.</span><span class="sxs-lookup"><span data-stu-id="a0766-927">hello following example shows hello format of hello returned value.</span></span> <span data-ttu-id="a0766-928">a megadott paraméterek hello függ a hello tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="a0766-928">hello actual value varies by hello provided parameters.</span></span>

    tcvhiyu5h2o5o

<span data-ttu-id="a0766-929">hello a következő példák bemutatják, hogyan toouse uniqueString toocreate egy egyedi érték a gyakran használt szintek.</span><span class="sxs-lookup"><span data-stu-id="a0766-929">hello following examples show how toouse uniqueString toocreate a unique value for commonly used levels.</span></span>

<span data-ttu-id="a0766-930">Egyedi hatókörön belüli toosubscription</span><span class="sxs-lookup"><span data-stu-id="a0766-930">Unique scoped toosubscription</span></span>

```json
"[uniqueString(subscription().subscriptionId)]"
```

<span data-ttu-id="a0766-931">Egyedi hatókörön belüli tooresource csoport</span><span class="sxs-lookup"><span data-stu-id="a0766-931">Unique scoped tooresource group</span></span>

```json
"[uniqueString(resourceGroup().id)]"
```

<span data-ttu-id="a0766-932">Egyedi hatókörön belüli toodeployment erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="a0766-932">Unique scoped toodeployment for a resource group</span></span>

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

<span data-ttu-id="a0766-933">hello a következő példa bemutatja, hogyan toocreate a storage-fiók egy egyedi nevet az erőforrás-csoport alapján.</span><span class="sxs-lookup"><span data-stu-id="a0766-933">hello following example shows how toocreate a unique name for a storage account based on your resource group.</span></span> <span data-ttu-id="a0766-934">Hello erőforráscsoport, belül hello nincs egyedi, ha hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="a0766-934">Inside hello resource group, hello name is not unique if constructed hello same way.</span></span>

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a><span data-ttu-id="a0766-935">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-935">Return value</span></span>

<span data-ttu-id="a0766-936">13 karaktereket tartalmazó karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a0766-936">A string containing 13 characters.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-937">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-937">Examples</span></span>

<span data-ttu-id="a0766-938">a következő példa hello eredmények uniquestring adja vissza:</span><span class="sxs-lookup"><span data-stu-id="a0766-938">hello following example returns results from uniquestring:</span></span>

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

## <a name="uri"></a><span data-ttu-id="a0766-939">URI</span><span class="sxs-lookup"><span data-stu-id="a0766-939">uri</span></span>
`uri (baseUri, relativeUri)`

<span data-ttu-id="a0766-940">Létrehoz egy abszolút URI hello baseUri és hello relativeUri karakterlánc kombinálásával.</span><span class="sxs-lookup"><span data-stu-id="a0766-940">Creates an absolute URI by combining hello baseUri and hello relativeUri string.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-941">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-941">Parameters</span></span>

| <span data-ttu-id="a0766-942">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-942">Parameter</span></span> | <span data-ttu-id="a0766-943">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-943">Required</span></span> | <span data-ttu-id="a0766-944">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-944">Type</span></span> | <span data-ttu-id="a0766-945">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-945">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-946">a baseUri</span><span class="sxs-lookup"><span data-stu-id="a0766-946">baseUri</span></span> |<span data-ttu-id="a0766-947">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-947">Yes</span></span> |<span data-ttu-id="a0766-948">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-948">string</span></span> |<span data-ttu-id="a0766-949">hello alap URI-azonosító karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a0766-949">hello base uri string.</span></span> |
| <span data-ttu-id="a0766-950">relativeUri</span><span class="sxs-lookup"><span data-stu-id="a0766-950">relativeUri</span></span> |<span data-ttu-id="a0766-951">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-951">Yes</span></span> |<span data-ttu-id="a0766-952">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-952">string</span></span> |<span data-ttu-id="a0766-953">hello relatív uri karakterlánc tooadd toohello alap URI-azonosító karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="a0766-953">hello relative uri string tooadd toohello base uri string.</span></span> |

<span data-ttu-id="a0766-954">hello értéke hello **baseUri** a paraméter egy adott fájlt tartalmazhat, de csak hello alap elérési utat használja hello URI konstrukciója során.</span><span class="sxs-lookup"><span data-stu-id="a0766-954">hello value for hello **baseUri** parameter can include a specific file, but only hello base path is used when constructing hello URI.</span></span> <span data-ttu-id="a0766-955">Például, hogy `http://contoso.com/resources/azuredeploy.json` hello baseUri paraméter alap URI-azonosítója a találatok között `http://contoso.com/resources/`.</span><span class="sxs-lookup"><span data-stu-id="a0766-955">For example, passing `http://contoso.com/resources/azuredeploy.json` as hello baseUri parameter results in a base URI of `http://contoso.com/resources/`.</span></span>

### <a name="return-value"></a><span data-ttu-id="a0766-956">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-956">Return value</span></span>

<span data-ttu-id="a0766-957">Abszolút URI-JÁNAK hello kiinduló és a relatív értéket jelölő karakterláncot hello.</span><span class="sxs-lookup"><span data-stu-id="a0766-957">A string representing hello absolute URI for hello base and relative values.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-958">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-958">Examples</span></span>

<span data-ttu-id="a0766-959">hello a következő példa bemutatja, hogyan tooconstruct hivatkozás tooa beágyazott sablon hello érték alapján hello szülő sablon.</span><span class="sxs-lookup"><span data-stu-id="a0766-959">hello following example shows how tooconstruct a link tooa nested template based on hello value of hello parent template.</span></span>

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

<span data-ttu-id="a0766-960">a következő példa azt mutatja meg hogyan hello toouse uri, uriComponent, és uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="a0766-960">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="a0766-961">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-961">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-962">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-962">Name</span></span> | <span data-ttu-id="a0766-963">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-963">Type</span></span> | <span data-ttu-id="a0766-964">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-964">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-965">uriOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-965">uriOutput</span></span> | <span data-ttu-id="a0766-966">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-966">String</span></span> | <span data-ttu-id="a0766-967">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-967">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="a0766-968">componentOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-968">componentOutput</span></span> | <span data-ttu-id="a0766-969">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-969">String</span></span> | <span data-ttu-id="a0766-970">HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-970">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="a0766-971">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-971">toStringOutput</span></span> | <span data-ttu-id="a0766-972">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-972">String</span></span> | <span data-ttu-id="a0766-973">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-973">http://contoso.com/resources/nested/azuredeploy.json</span></span> |

<a id="uricomponent" />

## <a name="uricomponent"></a><span data-ttu-id="a0766-974">uriComponent</span><span class="sxs-lookup"><span data-stu-id="a0766-974">uriComponent</span></span>
`uricomponent(stringToEncode)`

<span data-ttu-id="a0766-975">Kódolja URI.</span><span class="sxs-lookup"><span data-stu-id="a0766-975">Encodes a URI.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-976">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-976">Parameters</span></span>

| <span data-ttu-id="a0766-977">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-977">Parameter</span></span> | <span data-ttu-id="a0766-978">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-978">Required</span></span> | <span data-ttu-id="a0766-979">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-979">Type</span></span> | <span data-ttu-id="a0766-980">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-980">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-981">stringToEncode</span><span class="sxs-lookup"><span data-stu-id="a0766-981">stringToEncode</span></span> |<span data-ttu-id="a0766-982">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-982">Yes</span></span> |<span data-ttu-id="a0766-983">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-983">string</span></span> |<span data-ttu-id="a0766-984">hello érték tooencode.</span><span class="sxs-lookup"><span data-stu-id="a0766-984">hello value tooencode.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-985">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-985">Return value</span></span>

<span data-ttu-id="a0766-986">Hello URI karakterlánc kódolt érték.</span><span class="sxs-lookup"><span data-stu-id="a0766-986">A string of hello URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-987">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-987">Examples</span></span>

<span data-ttu-id="a0766-988">a következő példa azt mutatja meg hogyan hello toouse uri, uriComponent, és uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="a0766-988">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="a0766-989">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-989">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-990">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-990">Name</span></span> | <span data-ttu-id="a0766-991">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-991">Type</span></span> | <span data-ttu-id="a0766-992">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-992">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-993">uriOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-993">uriOutput</span></span> | <span data-ttu-id="a0766-994">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-994">String</span></span> | <span data-ttu-id="a0766-995">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-995">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="a0766-996">componentOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-996">componentOutput</span></span> | <span data-ttu-id="a0766-997">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-997">String</span></span> | <span data-ttu-id="a0766-998">HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-998">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="a0766-999">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-999">toStringOutput</span></span> | <span data-ttu-id="a0766-1000">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-1000">String</span></span> | <span data-ttu-id="a0766-1001">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-1001">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a><span data-ttu-id="a0766-1002">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="a0766-1002">uriComponentToString</span></span>
`uriComponentToString(uriEncodedString)`

<span data-ttu-id="a0766-1003">Adja vissza a URI karakterlánc kódolású érték.</span><span class="sxs-lookup"><span data-stu-id="a0766-1003">Returns a string of a URI encoded value.</span></span>

### <a name="parameters"></a><span data-ttu-id="a0766-1004">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a0766-1004">Parameters</span></span>

| <span data-ttu-id="a0766-1005">Paraméter</span><span class="sxs-lookup"><span data-stu-id="a0766-1005">Parameter</span></span> | <span data-ttu-id="a0766-1006">Szükséges</span><span class="sxs-lookup"><span data-stu-id="a0766-1006">Required</span></span> | <span data-ttu-id="a0766-1007">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-1007">Type</span></span> | <span data-ttu-id="a0766-1008">Leírás</span><span class="sxs-lookup"><span data-stu-id="a0766-1008">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="a0766-1009">uriEncodedString</span><span class="sxs-lookup"><span data-stu-id="a0766-1009">uriEncodedString</span></span> |<span data-ttu-id="a0766-1010">Igen</span><span class="sxs-lookup"><span data-stu-id="a0766-1010">Yes</span></span> |<span data-ttu-id="a0766-1011">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-1011">string</span></span> |<span data-ttu-id="a0766-1012">hello URI-kódolású érték tooconvert tooa karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a0766-1012">hello URI encoded value tooconvert tooa string.</span></span> |

### <a name="return-value"></a><span data-ttu-id="a0766-1013">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="a0766-1013">Return value</span></span>

<span data-ttu-id="a0766-1014">A dekódolt karakterlánc URI-kódolt érték.</span><span class="sxs-lookup"><span data-stu-id="a0766-1014">A decoded string of URI encoded value.</span></span>

### <a name="examples"></a><span data-ttu-id="a0766-1015">Példák</span><span class="sxs-lookup"><span data-stu-id="a0766-1015">Examples</span></span>

<span data-ttu-id="a0766-1016">a következő példa azt mutatja meg hogyan hello toouse uri, uriComponent, és uriComponentToString:</span><span class="sxs-lookup"><span data-stu-id="a0766-1016">hello following example shows how toouse uri, uriComponent, and uriComponentToString:</span></span>

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

<span data-ttu-id="a0766-1017">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a0766-1017">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="a0766-1018">Név</span><span class="sxs-lookup"><span data-stu-id="a0766-1018">Name</span></span> | <span data-ttu-id="a0766-1019">Típus</span><span class="sxs-lookup"><span data-stu-id="a0766-1019">Type</span></span> | <span data-ttu-id="a0766-1020">Érték</span><span class="sxs-lookup"><span data-stu-id="a0766-1020">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="a0766-1021">uriOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-1021">uriOutput</span></span> | <span data-ttu-id="a0766-1022">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-1022">String</span></span> | <span data-ttu-id="a0766-1023">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-1023">http://contoso.com/resources/nested/azuredeploy.json</span></span> |
| <span data-ttu-id="a0766-1024">componentOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-1024">componentOutput</span></span> | <span data-ttu-id="a0766-1025">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-1025">String</span></span> | <span data-ttu-id="a0766-1026">HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-1026">http%3A%2F%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.json</span></span> |
| <span data-ttu-id="a0766-1027">toStringOutput</span><span class="sxs-lookup"><span data-stu-id="a0766-1027">toStringOutput</span></span> | <span data-ttu-id="a0766-1028">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="a0766-1028">String</span></span> | <span data-ttu-id="a0766-1029">http://contoso.com/resources/nested/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a0766-1029">http://contoso.com/resources/nested/azuredeploy.json</span></span> |


## <a name="next-steps"></a><span data-ttu-id="a0766-1030">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a0766-1030">Next steps</span></span>
* <span data-ttu-id="a0766-1031">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a0766-1031">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="a0766-1032">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a0766-1032">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="a0766-1033">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="a0766-1033">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="a0766-1034">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="a0766-1034">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


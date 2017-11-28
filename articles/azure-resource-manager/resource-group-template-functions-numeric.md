---
title: "aaaAzure Resource Manager sablonfüggvényei - numerikus |} Microsoft Docs"
description: "Ismerteti az Azure Resource Manager sablon toowork a hello funkciók toouse számokkal."
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
ms.openlocfilehash: 855d5b354d094b9815edc160e3d72efbfd36ba77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a><span data-ttu-id="1fcc5-103">Az Azure Resource Manager sablonokhoz numerikus funkciók</span><span class="sxs-lookup"><span data-stu-id="1fcc5-103">Numeric functions for Azure Resource Manager templates</span></span>

<span data-ttu-id="1fcc5-104">A Resource Manager biztosít a következő funkciók egész számok való munkához hello:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-104">Resource Manager provides hello following functions for working with integers:</span></span>

* [<span data-ttu-id="1fcc5-105">hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1fcc5-105">add</span></span>](#add)
* [<span data-ttu-id="1fcc5-106">copyIndex</span><span class="sxs-lookup"><span data-stu-id="1fcc5-106">copyIndex</span></span>](#copyindex)
* [<span data-ttu-id="1fcc5-107">DIV</span><span class="sxs-lookup"><span data-stu-id="1fcc5-107">div</span></span>](#div)
* [<span data-ttu-id="1fcc5-108">lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="1fcc5-108">float</span></span>](#float)
* [<span data-ttu-id="1fcc5-109">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-109">int</span></span>](#int)
* [<span data-ttu-id="1fcc5-110">perc</span><span class="sxs-lookup"><span data-stu-id="1fcc5-110">min</span></span>](#min)
* [<span data-ttu-id="1fcc5-111">maximális</span><span class="sxs-lookup"><span data-stu-id="1fcc5-111">max</span></span>](#max)
* [<span data-ttu-id="1fcc5-112">MOD</span><span class="sxs-lookup"><span data-stu-id="1fcc5-112">mod</span></span>](#mod)
* [<span data-ttu-id="1fcc5-113">MUL számú</span><span class="sxs-lookup"><span data-stu-id="1fcc5-113">mul</span></span>](#mul)
* [<span data-ttu-id="1fcc5-114">Sub</span><span class="sxs-lookup"><span data-stu-id="1fcc5-114">sub</span></span>](#sub)

<a id="add" />

## <a name="add"></a><span data-ttu-id="1fcc5-115">Hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1fcc5-115">add</span></span>
`add(operand1, operand2)`

<span data-ttu-id="1fcc5-116">Beolvasása hello hello két megadott egész számok összege.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-116">Returns hello sum of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-117">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-117">Parameters</span></span>

| <span data-ttu-id="1fcc5-118">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-118">Parameter</span></span> | <span data-ttu-id="1fcc5-119">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-119">Required</span></span> | <span data-ttu-id="1fcc5-120">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-120">Type</span></span> | <span data-ttu-id="1fcc5-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-121">Description</span></span> |
|:--- |:--- |:--- |:--- | 
|<span data-ttu-id="1fcc5-122">operand1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-122">operand1</span></span> |<span data-ttu-id="1fcc5-123">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-123">Yes</span></span> |<span data-ttu-id="1fcc5-124">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-124">int</span></span> |<span data-ttu-id="1fcc5-125">Első számú tooadd.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-125">First number tooadd.</span></span> |
|<span data-ttu-id="1fcc5-126">operand2</span><span class="sxs-lookup"><span data-stu-id="1fcc5-126">operand2</span></span> |<span data-ttu-id="1fcc5-127">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-127">Yes</span></span> |<span data-ttu-id="1fcc5-128">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-128">int</span></span> |<span data-ttu-id="1fcc5-129">Második szám tooadd.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-129">Second number tooadd.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-130">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-130">Return value</span></span>

<span data-ttu-id="1fcc5-131">Egész szám, amely hello összege hello paramétereket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-131">An integer that contains hello sum of hello parameters.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-132">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-132">Example</span></span>

<span data-ttu-id="1fcc5-133">a következő példa hello két paramétereket ad.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-133">hello following example adds two parameters.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer tooadd"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer tooadd"
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

<span data-ttu-id="1fcc5-134">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-134">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-135">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-135">Name</span></span> | <span data-ttu-id="1fcc5-136">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-136">Type</span></span> | <span data-ttu-id="1fcc5-137">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-137">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-138">addResult</span><span class="sxs-lookup"><span data-stu-id="1fcc5-138">addResult</span></span> | <span data-ttu-id="1fcc5-139">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-139">Int</span></span> | <span data-ttu-id="1fcc5-140">8</span><span class="sxs-lookup"><span data-stu-id="1fcc5-140">8</span></span> |

<a id="copyindex" />

## <a name="copyindex"></a><span data-ttu-id="1fcc5-141">copyIndex</span><span class="sxs-lookup"><span data-stu-id="1fcc5-141">copyIndex</span></span>
`copyIndex(loopName, offset)`

<span data-ttu-id="1fcc5-142">Értéket ad vissza egy iteráció hurok index hello.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-142">Returns hello index of an iteration loop.</span></span> 

### <a name="parameters"></a><span data-ttu-id="1fcc5-143">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-143">Parameters</span></span>

| <span data-ttu-id="1fcc5-144">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-144">Parameter</span></span> | <span data-ttu-id="1fcc5-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-145">Required</span></span> | <span data-ttu-id="1fcc5-146">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-146">Type</span></span> | <span data-ttu-id="1fcc5-147">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-147">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-148">loopName</span><span class="sxs-lookup"><span data-stu-id="1fcc5-148">loopName</span></span> | <span data-ttu-id="1fcc5-149">Nem</span><span class="sxs-lookup"><span data-stu-id="1fcc5-149">No</span></span> | <span data-ttu-id="1fcc5-150">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fcc5-150">string</span></span> | <span data-ttu-id="1fcc5-151">hello neve hello hurok hello iterációs beolvasásakor.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-151">hello name of hello loop for getting hello iteration.</span></span> |
| <span data-ttu-id="1fcc5-152">Az offset</span><span class="sxs-lookup"><span data-stu-id="1fcc5-152">offset</span></span> |<span data-ttu-id="1fcc5-153">Nem</span><span class="sxs-lookup"><span data-stu-id="1fcc5-153">No</span></span> |<span data-ttu-id="1fcc5-154">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-154">int</span></span> |<span data-ttu-id="1fcc5-155">hello tooadd toohello nulla alapú iterációs számértéket.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-155">hello number tooadd toohello zero-based iteration value.</span></span> |

### <a name="remarks"></a><span data-ttu-id="1fcc5-156">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1fcc5-156">Remarks</span></span>

<span data-ttu-id="1fcc5-157">Ez a funkció mindig használatos a **másolási** objektum.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-157">This function is always used with a **copy** object.</span></span> <span data-ttu-id="1fcc5-158">Ha nincs érték megadva, a **eltolás**, hello aktuális iterációs értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-158">If no value is provided for **offset**, hello current iteration value is returned.</span></span> <span data-ttu-id="1fcc5-159">hello ismétlési érték nulla kezdődik.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-159">hello iteration value starts at zero.</span></span>

<span data-ttu-id="1fcc5-160">Hello **loopName** tulajdonság lehetővé teszi toospecify e copyIndex tooa erőforrás iterációs vagy tulajdonság iterációs hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-160">hello **loopName** property enables you toospecify whether copyIndex is referring tooa resource iteration or property iteration.</span></span> <span data-ttu-id="1fcc5-161">Ha nincs érték megadva, a **loopName**, hello aktuális erőforrás típusa iterációs szolgál.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-161">If no value is provided for **loopName**, hello current resource type iteration is used.</span></span> <span data-ttu-id="1fcc5-162">Adjon meg egy értéket a **loopName** amikor léptetés tulajdonság alapján.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-162">Provide a value for **loopName** when iterating on a property.</span></span> 
 
<span data-ttu-id="1fcc5-163">Teljes leírását az használatának **copyIndex**, lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="1fcc5-163">For a complete description of how you use **copyIndex**, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-164">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-164">Example</span></span>

<span data-ttu-id="1fcc5-165">hello következő példa bemutatja a másolási hurok és hello indexértéket hello neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-165">hello following example shows a copy loop and hello index value included in hello name.</span></span> 

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

### <a name="return-value"></a><span data-ttu-id="1fcc5-166">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-166">Return value</span></span>

<span data-ttu-id="1fcc5-167">Az aktuális index hello hello iterációs jelző egész számot.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-167">An integer representing hello current index of hello iteration.</span></span>

<a id="div" />

## <a name="div"></a><span data-ttu-id="1fcc5-168">DIV</span><span class="sxs-lookup"><span data-stu-id="1fcc5-168">div</span></span>
`div(operand1, operand2)`

<span data-ttu-id="1fcc5-169">Adja vissza egész osztás hello két megadott egész számok hello.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-169">Returns hello integer division of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-170">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-170">Parameters</span></span>

| <span data-ttu-id="1fcc5-171">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-171">Parameter</span></span> | <span data-ttu-id="1fcc5-172">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-172">Required</span></span> | <span data-ttu-id="1fcc5-173">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-173">Type</span></span> | <span data-ttu-id="1fcc5-174">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-174">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-175">operand1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-175">operand1</span></span> |<span data-ttu-id="1fcc5-176">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-176">Yes</span></span> |<span data-ttu-id="1fcc5-177">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-177">int</span></span> |<span data-ttu-id="1fcc5-178">felosztják hello számát.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-178">hello number being divided.</span></span> |
| <span data-ttu-id="1fcc5-179">operand2</span><span class="sxs-lookup"><span data-stu-id="1fcc5-179">operand2</span></span> |<span data-ttu-id="1fcc5-180">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-180">Yes</span></span> |<span data-ttu-id="1fcc5-181">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-181">int</span></span> |<span data-ttu-id="1fcc5-182">hello szám használt toodivide.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-182">hello number that is used toodivide.</span></span> <span data-ttu-id="1fcc5-183">Nem lehet 0.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-183">Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-184">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-184">Return value</span></span>

<span data-ttu-id="1fcc5-185">Egy egész számot jelölő hello osztás.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-185">An integer representing hello division.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-186">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-186">Example</span></span>

<span data-ttu-id="1fcc5-187">a következő példa hello felosztja egy másik paraméterrel egy paramétert.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-187">hello following example divides one parameter by another parameter.</span></span>

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
                "description": "Integer used toodivide"
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

<span data-ttu-id="1fcc5-188">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-188">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-189">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-189">Name</span></span> | <span data-ttu-id="1fcc5-190">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-190">Type</span></span> | <span data-ttu-id="1fcc5-191">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-191">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-192">divResult</span><span class="sxs-lookup"><span data-stu-id="1fcc5-192">divResult</span></span> | <span data-ttu-id="1fcc5-193">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-193">Int</span></span> | <span data-ttu-id="1fcc5-194">2</span><span class="sxs-lookup"><span data-stu-id="1fcc5-194">2</span></span> |

<a id="float" />

## <a name="float"></a><span data-ttu-id="1fcc5-195">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="1fcc5-195">float</span></span>
`float(arg1)`

<span data-ttu-id="1fcc5-196">Lebegőpontos szám hello érték tooa alakítja.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-196">Converts hello value tooa floating point number.</span></span> <span data-ttu-id="1fcc5-197">Ez a függvény csak ha egyéni paraméterek átadása tooan alkalmazás, például a logikai alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-197">You only use this function when passing custom parameters tooan application, such as a Logic App.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-198">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-198">Parameters</span></span>

| <span data-ttu-id="1fcc5-199">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-199">Parameter</span></span> | <span data-ttu-id="1fcc5-200">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-200">Required</span></span> | <span data-ttu-id="1fcc5-201">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-201">Type</span></span> | <span data-ttu-id="1fcc5-202">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-202">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-203">arg1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-203">arg1</span></span> |<span data-ttu-id="1fcc5-204">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-204">Yes</span></span> |<span data-ttu-id="1fcc5-205">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-205">string or int</span></span> |<span data-ttu-id="1fcc5-206">hello érték tooconvert tooa lebegőpontos szám.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-206">hello value tooconvert tooa floating point number.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-207">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-207">Return value</span></span>
<span data-ttu-id="1fcc5-208">Lebegőpontos szám.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-208">A floating point number.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-209">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-209">Example</span></span>

<span data-ttu-id="1fcc5-210">hello a következő példa bemutatja, hogyan toouse lebegőpontos toopass paraméterek tooa logikai alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-210">hello following example shows how toouse float toopass parameters tooa Logic App:</span></span>

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

## <a name="int"></a><span data-ttu-id="1fcc5-211">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-211">int</span></span>
`int(valueToConvert)`

<span data-ttu-id="1fcc5-212">Hello megadott érték tooan egész számra konvertál.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-212">Converts hello specified value tooan integer.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-213">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-213">Parameters</span></span>

| <span data-ttu-id="1fcc5-214">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-214">Parameter</span></span> | <span data-ttu-id="1fcc5-215">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-215">Required</span></span> | <span data-ttu-id="1fcc5-216">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-216">Type</span></span> | <span data-ttu-id="1fcc5-217">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-217">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-218">valueToConvert</span><span class="sxs-lookup"><span data-stu-id="1fcc5-218">valueToConvert</span></span> |<span data-ttu-id="1fcc5-219">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-219">Yes</span></span> |<span data-ttu-id="1fcc5-220">karakterlánc- vagy int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-220">string or int</span></span> |<span data-ttu-id="1fcc5-221">hello érték tooconvert tooan egész szám.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-221">hello value tooconvert tooan integer.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-222">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-222">Return value</span></span>

<span data-ttu-id="1fcc5-223">Hello konvertálni érték egész szám.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-223">An integer of hello converted value.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-224">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-224">Example</span></span>

<span data-ttu-id="1fcc5-225">hello alábbi példa konvertál hello felhasználó által megadott paraméter értéke toointeger.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-225">hello following example converts hello user-provided parameter value toointeger.</span></span>

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

<span data-ttu-id="1fcc5-226">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-226">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-227">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-227">Name</span></span> | <span data-ttu-id="1fcc5-228">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-228">Type</span></span> | <span data-ttu-id="1fcc5-229">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-229">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-230">intEredmeny</span><span class="sxs-lookup"><span data-stu-id="1fcc5-230">intResult</span></span> | <span data-ttu-id="1fcc5-231">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-231">Int</span></span> | <span data-ttu-id="1fcc5-232">4</span><span class="sxs-lookup"><span data-stu-id="1fcc5-232">4</span></span> |


<a id="min" />

## <a name="min"></a><span data-ttu-id="1fcc5-233">perc</span><span class="sxs-lookup"><span data-stu-id="1fcc5-233">min</span></span>
`min (arg1)`

<span data-ttu-id="1fcc5-234">Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját az minimális értékét.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-234">Returns hello minimum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-235">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-235">Parameters</span></span>

| <span data-ttu-id="1fcc5-236">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-236">Parameter</span></span> | <span data-ttu-id="1fcc5-237">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-237">Required</span></span> | <span data-ttu-id="1fcc5-238">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-238">Type</span></span> | <span data-ttu-id="1fcc5-239">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-239">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-240">arg1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-240">arg1</span></span> |<span data-ttu-id="1fcc5-241">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-241">Yes</span></span> |<span data-ttu-id="1fcc5-242">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="1fcc5-242">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="1fcc5-243">hello gyűjtemény tooget hello minimális érték.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-243">hello collection tooget hello minimum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-244">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-244">Return value</span></span>

<span data-ttu-id="1fcc5-245">A minimális érték hello gyűjteményből jelző egész számot.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-245">An integer representing minimum value from hello collection.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-246">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-246">Example</span></span>

<span data-ttu-id="1fcc5-247">a következő példa azt mutatja meg hogyan hello toouse min tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-247">hello following example shows how toouse min with an array and a list of integers:</span></span>

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

<span data-ttu-id="1fcc5-248">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-248">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-249">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-249">Name</span></span> | <span data-ttu-id="1fcc5-250">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-250">Type</span></span> | <span data-ttu-id="1fcc5-251">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-251">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-252">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="1fcc5-252">arrayOutput</span></span> | <span data-ttu-id="1fcc5-253">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-253">Int</span></span> | <span data-ttu-id="1fcc5-254">0</span><span class="sxs-lookup"><span data-stu-id="1fcc5-254">0</span></span> |
| <span data-ttu-id="1fcc5-255">intOutput</span><span class="sxs-lookup"><span data-stu-id="1fcc5-255">intOutput</span></span> | <span data-ttu-id="1fcc5-256">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-256">Int</span></span> | <span data-ttu-id="1fcc5-257">0</span><span class="sxs-lookup"><span data-stu-id="1fcc5-257">0</span></span> |

<a id="max" />

## <a name="max"></a><span data-ttu-id="1fcc5-258">maximális</span><span class="sxs-lookup"><span data-stu-id="1fcc5-258">max</span></span>
`max (arg1)`

<span data-ttu-id="1fcc5-259">Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját maximális értéket.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-259">Returns hello maximum value from an array of integers or a comma-separated list of integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-260">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-260">Parameters</span></span>

| <span data-ttu-id="1fcc5-261">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-261">Parameter</span></span> | <span data-ttu-id="1fcc5-262">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-262">Required</span></span> | <span data-ttu-id="1fcc5-263">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-263">Type</span></span> | <span data-ttu-id="1fcc5-264">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-264">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-265">arg1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-265">arg1</span></span> |<span data-ttu-id="1fcc5-266">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-266">Yes</span></span> |<span data-ttu-id="1fcc5-267">a tömb egész szám vagy egészek vesszővel elválasztott felsorolása</span><span class="sxs-lookup"><span data-stu-id="1fcc5-267">array of integers, or comma-separated list of integers</span></span> |<span data-ttu-id="1fcc5-268">hello gyűjtemény tooget hello maximális értéket.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-268">hello collection tooget hello maximum value.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-269">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-269">Return value</span></span>

<span data-ttu-id="1fcc5-270">Jelző egész számot hello maximális érték hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-270">An integer representing hello maximum value from hello collection.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-271">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-271">Example</span></span>

<span data-ttu-id="1fcc5-272">a következő példa azt mutatja meg hogyan hello toouse maximális tömb és az egész számok listáját:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-272">hello following example shows how toouse max with an array and a list of integers:</span></span>

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

<span data-ttu-id="1fcc5-273">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-273">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-274">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-274">Name</span></span> | <span data-ttu-id="1fcc5-275">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-275">Type</span></span> | <span data-ttu-id="1fcc5-276">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-276">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-277">arrayOutput</span><span class="sxs-lookup"><span data-stu-id="1fcc5-277">arrayOutput</span></span> | <span data-ttu-id="1fcc5-278">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-278">Int</span></span> | <span data-ttu-id="1fcc5-279">5</span><span class="sxs-lookup"><span data-stu-id="1fcc5-279">5</span></span> |
| <span data-ttu-id="1fcc5-280">intOutput</span><span class="sxs-lookup"><span data-stu-id="1fcc5-280">intOutput</span></span> | <span data-ttu-id="1fcc5-281">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-281">Int</span></span> | <span data-ttu-id="1fcc5-282">5</span><span class="sxs-lookup"><span data-stu-id="1fcc5-282">5</span></span> |

<a id="mod" />

## <a name="mod"></a><span data-ttu-id="1fcc5-283">MOD</span><span class="sxs-lookup"><span data-stu-id="1fcc5-283">mod</span></span>
`mod(operand1, operand2)`

<span data-ttu-id="1fcc5-284">Hello egész osztály használatával hello két megadott egész számok hello maradékot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-284">Returns hello remainder of hello integer division using hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-285">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-285">Parameters</span></span>

| <span data-ttu-id="1fcc5-286">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-286">Parameter</span></span> | <span data-ttu-id="1fcc5-287">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-287">Required</span></span> | <span data-ttu-id="1fcc5-288">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-288">Type</span></span> | <span data-ttu-id="1fcc5-289">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-289">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-290">operand1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-290">operand1</span></span> |<span data-ttu-id="1fcc5-291">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-291">Yes</span></span> |<span data-ttu-id="1fcc5-292">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-292">int</span></span> |<span data-ttu-id="1fcc5-293">felosztják hello számát.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-293">hello number being divided.</span></span> |
| <span data-ttu-id="1fcc5-294">operand2</span><span class="sxs-lookup"><span data-stu-id="1fcc5-294">operand2</span></span> |<span data-ttu-id="1fcc5-295">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-295">Yes</span></span> |<span data-ttu-id="1fcc5-296">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-296">int</span></span> |<span data-ttu-id="1fcc5-297">használt toodivide hello szám nem lehet 0.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-297">hello number that is used toodivide, Cannot be 0.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-298">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-298">Return value</span></span>
<span data-ttu-id="1fcc5-299">Egy egész számot jelölő hello maradékot.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-299">An integer representing hello remainder.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-300">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-300">Example</span></span>

<span data-ttu-id="1fcc5-301">hello alábbi példa maradékot adja vissza hello felosztása egy paraméter egy másik paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-301">hello following example returns hello remainder of dividing one parameter by another parameter.</span></span>

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
                "description": "Integer used toodivide"
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

<span data-ttu-id="1fcc5-302">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-302">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-303">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-303">Name</span></span> | <span data-ttu-id="1fcc5-304">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-304">Type</span></span> | <span data-ttu-id="1fcc5-305">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-305">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-306">modResult</span><span class="sxs-lookup"><span data-stu-id="1fcc5-306">modResult</span></span> | <span data-ttu-id="1fcc5-307">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-307">Int</span></span> | <span data-ttu-id="1fcc5-308">1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-308">1</span></span> |

<a id="mul" />

## <a name="mul"></a><span data-ttu-id="1fcc5-309">MUL számú</span><span class="sxs-lookup"><span data-stu-id="1fcc5-309">mul</span></span>
`mul(operand1, operand2)`

<span data-ttu-id="1fcc5-310">Értéket ad vissza a szorzás hello két megadott egész számok hello.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-310">Returns hello multiplication of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-311">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-311">Parameters</span></span>

| <span data-ttu-id="1fcc5-312">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-312">Parameter</span></span> | <span data-ttu-id="1fcc5-313">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-313">Required</span></span> | <span data-ttu-id="1fcc5-314">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-314">Type</span></span> | <span data-ttu-id="1fcc5-315">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-315">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-316">operand1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-316">operand1</span></span> |<span data-ttu-id="1fcc5-317">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-317">Yes</span></span> |<span data-ttu-id="1fcc5-318">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-318">int</span></span> |<span data-ttu-id="1fcc5-319">Első számú toomultiply.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-319">First number toomultiply.</span></span> |
| <span data-ttu-id="1fcc5-320">operand2</span><span class="sxs-lookup"><span data-stu-id="1fcc5-320">operand2</span></span> |<span data-ttu-id="1fcc5-321">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-321">Yes</span></span> |<span data-ttu-id="1fcc5-322">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-322">int</span></span> |<span data-ttu-id="1fcc5-323">Második szám toomultiply.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-323">Second number toomultiply.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-324">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-324">Return value</span></span>

<span data-ttu-id="1fcc5-325">Egy egész számot jelölő hello szorzást végezhet.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-325">An integer representing hello multiplication.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-326">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-326">Example</span></span>

<span data-ttu-id="1fcc5-327">a következő példa hello szorozza meg egy másik paraméterrel egy paramétert.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-327">hello following example multiplies one parameter by another parameter.</span></span>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer toomultiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer toomultiply"
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

<span data-ttu-id="1fcc5-328">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-328">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-329">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-329">Name</span></span> | <span data-ttu-id="1fcc5-330">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-330">Type</span></span> | <span data-ttu-id="1fcc5-331">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-331">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-332">mulResult</span><span class="sxs-lookup"><span data-stu-id="1fcc5-332">mulResult</span></span> | <span data-ttu-id="1fcc5-333">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-333">Int</span></span> | <span data-ttu-id="1fcc5-334">15</span><span class="sxs-lookup"><span data-stu-id="1fcc5-334">15</span></span> |

<a id="sub" />

## <a name="sub"></a><span data-ttu-id="1fcc5-335">Sub</span><span class="sxs-lookup"><span data-stu-id="1fcc5-335">sub</span></span>
`sub(operand1, operand2)`

<span data-ttu-id="1fcc5-336">Értéket ad vissza a két megadott egészek hello kivonás hello.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-336">Returns hello subtraction of hello two provided integers.</span></span>

### <a name="parameters"></a><span data-ttu-id="1fcc5-337">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fcc5-337">Parameters</span></span>

| <span data-ttu-id="1fcc5-338">Paraméter</span><span class="sxs-lookup"><span data-stu-id="1fcc5-338">Parameter</span></span> | <span data-ttu-id="1fcc5-339">Szükséges</span><span class="sxs-lookup"><span data-stu-id="1fcc5-339">Required</span></span> | <span data-ttu-id="1fcc5-340">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-340">Type</span></span> | <span data-ttu-id="1fcc5-341">Leírás</span><span class="sxs-lookup"><span data-stu-id="1fcc5-341">Description</span></span> |
|:--- |:--- |:--- |:--- |
| <span data-ttu-id="1fcc5-342">operand1</span><span class="sxs-lookup"><span data-stu-id="1fcc5-342">operand1</span></span> |<span data-ttu-id="1fcc5-343">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-343">Yes</span></span> |<span data-ttu-id="1fcc5-344">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-344">int</span></span> |<span data-ttu-id="1fcc5-345">a program levonja az hello száma.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-345">hello number that is subtracted from.</span></span> |
| <span data-ttu-id="1fcc5-346">operand2</span><span class="sxs-lookup"><span data-stu-id="1fcc5-346">operand2</span></span> |<span data-ttu-id="1fcc5-347">Igen</span><span class="sxs-lookup"><span data-stu-id="1fcc5-347">Yes</span></span> |<span data-ttu-id="1fcc5-348">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-348">int</span></span> |<span data-ttu-id="1fcc5-349">a program levonja hello száma.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-349">hello number that is subtracted.</span></span> |

### <a name="return-value"></a><span data-ttu-id="1fcc5-350">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-350">Return value</span></span>
<span data-ttu-id="1fcc5-351">Egy egész számot jelölő hello kivonás.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-351">An integer representing hello subtraction.</span></span>

### <a name="example"></a><span data-ttu-id="1fcc5-352">Példa</span><span class="sxs-lookup"><span data-stu-id="1fcc5-352">Example</span></span>

<span data-ttu-id="1fcc5-353">a következő példa hello kivonja másik paraméter egy paramétert.</span><span class="sxs-lookup"><span data-stu-id="1fcc5-353">hello following example subtracts one parameter from another parameter.</span></span>

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
                "description": "Integer toosubtract"
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

<span data-ttu-id="1fcc5-354">hello kimenetét hello előző példa hello alapértelmezett értékekkel:</span><span class="sxs-lookup"><span data-stu-id="1fcc5-354">hello output from hello preceding example with hello default values is:</span></span>

| <span data-ttu-id="1fcc5-355">Név</span><span class="sxs-lookup"><span data-stu-id="1fcc5-355">Name</span></span> | <span data-ttu-id="1fcc5-356">Típus</span><span class="sxs-lookup"><span data-stu-id="1fcc5-356">Type</span></span> | <span data-ttu-id="1fcc5-357">Érték</span><span class="sxs-lookup"><span data-stu-id="1fcc5-357">Value</span></span> |
| ---- | ---- | ----- |
| <span data-ttu-id="1fcc5-358">subResult</span><span class="sxs-lookup"><span data-stu-id="1fcc5-358">subResult</span></span> | <span data-ttu-id="1fcc5-359">int</span><span class="sxs-lookup"><span data-stu-id="1fcc5-359">Int</span></span> | <span data-ttu-id="1fcc5-360">4</span><span class="sxs-lookup"><span data-stu-id="1fcc5-360">4</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1fcc5-361">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fcc5-361">Next steps</span></span>
* <span data-ttu-id="1fcc5-362">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1fcc5-362">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1fcc5-363">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1fcc5-363">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="1fcc5-364">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="1fcc5-364">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>
* <span data-ttu-id="1fcc5-365">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1fcc5-365">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>


---
title: "Az Azure által felügyelt alkalmazás, hozzon létre felhasználói felület definition funkciók |} Microsoft Docs"
description: "Az Azure által felügyelt alkalmazások felhasználói felületi definíciók konstrukciója során használandó funkcióit ismerteti"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: 62ee10eb8e6f33cc4d828cf01b405c846bef8aa4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="createuidefinition-functions"></a><span data-ttu-id="1fbcd-103">CreateUiDefinition funkciók</span><span class="sxs-lookup"><span data-stu-id="1fbcd-103">CreateUiDefinition functions</span></span>
<span data-ttu-id="1fbcd-104">Ez a szakasz az összes támogatott funkcióit egy CreateUiDefinition aláírásai.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-104">This section contains the signatures for all supported functions of a CreateUiDefinition.</span></span>

<span data-ttu-id="1fbcd-105">A funkció használatához helyezze a szögletes zárójelek deklaráció.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-105">To use a function, surround the declaration with square brackets.</span></span> <span data-ttu-id="1fbcd-106">Példa:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-106">For example:</span></span>

```json
"[function()]"
```

<span data-ttu-id="1fbcd-107">Karakterláncok és egyéb funkciók függvény paramétereinek lehet hivatkozni, de karakterláncok kell körül, szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-107">Strings and other functions can be referenced as parameters for a function, but strings must be surrounded in single quotes.</span></span> <span data-ttu-id="1fbcd-108">Példa:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-108">For example:</span></span>

```json
"[fn1(fn2(), 'foobar')]"
```

<span data-ttu-id="1fbcd-109">Ahol lehetséges, melyeket referenciaként használhat, a kimenet egy függvény tulajdonságok a pont operátor használatával.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-109">Where applicable, you can reference properties of the output of a function by using the dot operator.</span></span> <span data-ttu-id="1fbcd-110">Példa:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-110">For example:</span></span>

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a><span data-ttu-id="1fbcd-111">Hivatkozási funkciók</span><span class="sxs-lookup"><span data-stu-id="1fbcd-111">Referencing functions</span></span>
<span data-ttu-id="1fbcd-112">Ezek a funkciók való hivatkozáshoz kimenetek a Tulajdonságok vagy egy CreateUiDefinition kontextusában használható.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-112">These functions can be used to reference outputs from the properties or context of a CreateUiDefinition.</span></span>

### <a name="basics"></a><span data-ttu-id="1fbcd-113">alapvető tudnivalók</span><span class="sxs-lookup"><span data-stu-id="1fbcd-113">basics</span></span>
<span data-ttu-id="1fbcd-114">Az alapok lépésben megadott elem kimeneti értékeit adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-114">Returns the output values of an element that is defined in the Basics step.</span></span>

<span data-ttu-id="1fbcd-115">A következő példa a nevű elem a kimenetet visszaadja `foo` az alapvető lépésben:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-115">The following example returns the output of the element named `foo` in the Basics step:</span></span>

```json
"[basics('foo')]"
```

### <a name="steps"></a><span data-ttu-id="1fbcd-116">Lépések</span><span class="sxs-lookup"><span data-stu-id="1fbcd-116">steps</span></span>
<span data-ttu-id="1fbcd-117">Egy elem az adott lépéssel meghatározott kimeneti értékeit adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-117">Returns the output values of an element that is defined in the specified step.</span></span> <span data-ttu-id="1fbcd-118">A kimeneti értékek közül az alapvető lépésben, amelyet `basics()` helyette.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-118">To get the output values of elements in the Basics step, use `basics()` instead.</span></span>

<span data-ttu-id="1fbcd-119">A következő példa a nevű elem a kimenetet visszaadja `bar` nevű lépésben `foo`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-119">The following example returns the output of the element named `bar` in the step named `foo`:</span></span>

```json
"[steps('foo').bar]"
```

### <a name="location"></a><span data-ttu-id="1fbcd-120">location</span><span class="sxs-lookup"><span data-stu-id="1fbcd-120">location</span></span>
<span data-ttu-id="1fbcd-121">Az alapok lépés vagy a jelenlegi környezet kijelölt helyét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-121">Returns the location selected in the Basics step or the current context.</span></span>

<span data-ttu-id="1fbcd-122">Az alábbi példa visszaadhatja `"westus"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-122">The following example could return `"westus"`:</span></span>

```json
"[location()]"
```

## <a name="string-functions"></a><span data-ttu-id="1fbcd-123">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-123">String functions</span></span>
<span data-ttu-id="1fbcd-124">Ezek a függvények csak JSON karakterláncok használható.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-124">These functions can only be used with JSON strings.</span></span>

### <a name="concat"></a><span data-ttu-id="1fbcd-125">Concat</span><span class="sxs-lookup"><span data-stu-id="1fbcd-125">concat</span></span>
<span data-ttu-id="1fbcd-126">Egy vagy több karakterlánc fűzi össze.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-126">Concatenates one or more strings.</span></span>

<span data-ttu-id="1fbcd-127">Például ha a kimeneti értéke `element1` Ha `"bar"`, akkor ebben a példában a karakterláncot ad vissza, `"foobar!"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-127">For example, if the output value of `element1` if `"bar"`, then this example returns the string `"foobar!"`:</span></span>

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a><span data-ttu-id="1fbcd-128">Substring</span><span class="sxs-lookup"><span data-stu-id="1fbcd-128">substring</span></span>
<span data-ttu-id="1fbcd-129">A megadott karakterlánc részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-129">Returns the substring of the specified string.</span></span> <span data-ttu-id="1fbcd-130">A substring megadott indexű elindul, és a megadott időtartam.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-130">The substring starts at the specified index and has the specified length.</span></span>

<span data-ttu-id="1fbcd-131">A következő példa `"ftw"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-131">The following example returns `"ftw"`:</span></span>

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a><span data-ttu-id="1fbcd-132">cserélje le</span><span class="sxs-lookup"><span data-stu-id="1fbcd-132">replace</span></span>
<span data-ttu-id="1fbcd-133">Egy karakterláncot, amelyben a jelenlegi karakterláncban megadott karakterlánc összes előfordulásának helyére egy másik karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-133">Returns a string in which all occurrences of the specified string in the current string are replaced with another string.</span></span>

<span data-ttu-id="1fbcd-134">A következő példa `"Everything is awesome!"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-134">The following example returns `"Everything is awesome!"`:</span></span>

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a><span data-ttu-id="1fbcd-135">GUID</span><span class="sxs-lookup"><span data-stu-id="1fbcd-135">guid</span></span>
<span data-ttu-id="1fbcd-136">Karakterláncot hoz létre globálisan egyedi (GUID).</span><span class="sxs-lookup"><span data-stu-id="1fbcd-136">Generates a globally unique string (GUID).</span></span>

<span data-ttu-id="1fbcd-137">Az alábbi példa visszaadhatja `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-137">The following example could return `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span></span>

```json
"[guid()]"
```

### <a name="tolower"></a><span data-ttu-id="1fbcd-138">toLower</span><span class="sxs-lookup"><span data-stu-id="1fbcd-138">toLower</span></span>
<span data-ttu-id="1fbcd-139">Egy kisbetűssé konvertált karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-139">Returns a string converted to lowercase.</span></span>

<span data-ttu-id="1fbcd-140">A következő példa `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-140">The following example returns `"foobar"`:</span></span>

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a><span data-ttu-id="1fbcd-141">toUpper</span><span class="sxs-lookup"><span data-stu-id="1fbcd-141">toUpper</span></span>
<span data-ttu-id="1fbcd-142">Nagybetűssé konvertált karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-142">Returns a string converted to uppercase.</span></span>

<span data-ttu-id="1fbcd-143">A következő példa `"FOOBAR"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-143">The following example returns `"FOOBAR"`:</span></span>

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a><span data-ttu-id="1fbcd-144">Gyűjtemény-funkciók</span><span class="sxs-lookup"><span data-stu-id="1fbcd-144">Collection functions</span></span>
<span data-ttu-id="1fbcd-145">Ezek a funkciók gyűjtemények, például a JSON-karakterláncok, tömbök és objektumok használható.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-145">These functions can be used with collections, like JSON strings, arrays and objects.</span></span>

### <a name="contains"></a><span data-ttu-id="1fbcd-146">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="1fbcd-146">contains</span></span>
<span data-ttu-id="1fbcd-147">Beolvasása `true` Ha egy karakterláncot tartalmazza a megadott, egy tömb a megadott értéket tartalmaz, vagy az objektum tartalmazza a megadott kulcs.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-147">Returns `true` if a string contains the specified substring, an array contains the specified value, or an object contains the specified key.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="1fbcd-148">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-148">Example 1: string</span></span>
<span data-ttu-id="1fbcd-149">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-149">The following example returns `true`:</span></span>

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="1fbcd-150">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-150">Example 2: array</span></span>
<span data-ttu-id="1fbcd-151">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-151">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="1fbcd-152">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-152">The following example returns `false`:</span></span>

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="1fbcd-153">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="1fbcd-153">Example 3: object</span></span>
<span data-ttu-id="1fbcd-154">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-154">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="1fbcd-155">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-155">The following example returns `true`:</span></span>

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a><span data-ttu-id="1fbcd-156">Hossza</span><span class="sxs-lookup"><span data-stu-id="1fbcd-156">length</span></span>
<span data-ttu-id="1fbcd-157">Egy karakterlánc, tömbben értékek száma vagy az objektum kulcsainak száma a karakterek számát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-157">Returns the number of characters in a string, the number of values in an array, or the number of keys in an object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="1fbcd-158">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-158">Example 1: string</span></span>
<span data-ttu-id="1fbcd-159">A következő példa `6`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-159">The following example returns `6`:</span></span>

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="1fbcd-160">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-160">Example 2: array</span></span>
<span data-ttu-id="1fbcd-161">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-161">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="1fbcd-162">A következő példa `3`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-162">The following example returns `3`:</span></span>

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="1fbcd-163">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="1fbcd-163">Example 3: object</span></span>
<span data-ttu-id="1fbcd-164">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-164">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="1fbcd-165">A következő példa `2`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-165">The following example returns `2`:</span></span>

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a><span data-ttu-id="1fbcd-166">üres</span><span class="sxs-lookup"><span data-stu-id="1fbcd-166">empty</span></span>
<span data-ttu-id="1fbcd-167">Beolvasása `true` esetén a karakterláncot, a tömb vagy objektum null értékű vagy üres.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-167">Returns `true` if the string, array, or object is null or empty.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="1fbcd-168">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-168">Example 1: string</span></span>
<span data-ttu-id="1fbcd-169">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-169">The following example returns `true`:</span></span>

```json
"[empty('')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="1fbcd-170">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-170">Example 2: array</span></span>
<span data-ttu-id="1fbcd-171">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-171">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="1fbcd-172">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-172">The following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="1fbcd-173">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="1fbcd-173">Example 3: object</span></span>
<span data-ttu-id="1fbcd-174">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-174">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="1fbcd-175">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-175">The following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a><span data-ttu-id="1fbcd-176">4. példa: null, és nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="1fbcd-176">Example 4: null and undefined</span></span>
<span data-ttu-id="1fbcd-177">Tegyük fel `element1` van `null` vagy nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-177">Assume `element1` is `null` or undefined.</span></span> <span data-ttu-id="1fbcd-178">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-178">The following example returns `true`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a><span data-ttu-id="1fbcd-179">első</span><span class="sxs-lookup"><span data-stu-id="1fbcd-179">first</span></span>
<span data-ttu-id="1fbcd-180">Visszaadja a megadott karakterlánc; az első karakter első érték a megadott tömb; vagy az első kulcsot és értéket a megadott objektum.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-180">Returns the first character of the specified string; first value of the specified array; or the first key and value of the specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="1fbcd-181">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-181">Example 1: string</span></span>
<span data-ttu-id="1fbcd-182">A következő példa `"f"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-182">The following example returns `"f"`:</span></span>

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="1fbcd-183">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-183">Example 2: array</span></span>
<span data-ttu-id="1fbcd-184">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-184">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="1fbcd-185">A következő példa `1`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-185">The following example returns `1`:</span></span>

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="1fbcd-186">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="1fbcd-186">Example 3: object</span></span>
<span data-ttu-id="1fbcd-187">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-187">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="1fbcd-188">A következő példa `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-188">The following example returns `{"key1": "foobar"}`:</span></span>

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a><span data-ttu-id="1fbcd-189">utolsó</span><span class="sxs-lookup"><span data-stu-id="1fbcd-189">last</span></span>
<span data-ttu-id="1fbcd-190">A megadott karakterlánc, a legutóbbi értéket a megadott tömb, vagy a legutóbbi kulcs és az értéke a megadott objektum utolsó karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-190">Returns the last character of the specified string, the last value of the specified array, or the last key and value of the specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="1fbcd-191">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-191">Example 1: string</span></span>
<span data-ttu-id="1fbcd-192">A következő példa `"r"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-192">The following example returns `"r"`:</span></span>

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="1fbcd-193">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-193">Example 2: array</span></span>
<span data-ttu-id="1fbcd-194">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-194">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="1fbcd-195">A következő példa `2`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-195">The following example returns `2`:</span></span>

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="1fbcd-196">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="1fbcd-196">Example 3: object</span></span>
<span data-ttu-id="1fbcd-197">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-197">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="1fbcd-198">A következő példa `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-198">The following example returns `{"key2": "raboof"}`:</span></span>

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a><span data-ttu-id="1fbcd-199">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="1fbcd-199">take</span></span>
<span data-ttu-id="1fbcd-200">Összefüggő karakterből karakterlánc adott számú, megadott számú folytonos értékek elejéről. a tömb vagy összefüggő kulcsok és értékek elejéről. az objektum a megadott számú adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-200">Returns a specified number of contiguous characters from the start of the string, a specified number of contiguous values from the start of the array, or a specified number of contiguous keys and values from the start of the object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="1fbcd-201">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-201">Example 1: string</span></span>
<span data-ttu-id="1fbcd-202">A következő példa `"foo"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-202">The following example returns `"foo"`:</span></span>

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="1fbcd-203">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-203">Example 2: array</span></span>
<span data-ttu-id="1fbcd-204">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-204">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="1fbcd-205">A következő példa `[1, 2]`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-205">The following example returns `[1, 2]`:</span></span>

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="1fbcd-206">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="1fbcd-206">Example 3: object</span></span>
<span data-ttu-id="1fbcd-207">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-207">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="1fbcd-208">A következő példa `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-208">The following example returns `{"key1": "foobar"}`:</span></span>

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a><span data-ttu-id="1fbcd-209">Kihagyása</span><span class="sxs-lookup"><span data-stu-id="1fbcd-209">skip</span></span>
<span data-ttu-id="1fbcd-210">A megadott számú elemet egy gyűjtemény megkerüli, és a fennmaradó elemeket adja majd vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-210">Bypasses a specified number of elements in a collection, and then returns the remaining elements.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="1fbcd-211">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-211">Example 1: string</span></span>
<span data-ttu-id="1fbcd-212">A következő példa `"bar"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-212">The following example returns `"bar"`:</span></span>

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="1fbcd-213">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-213">Example 2: array</span></span>
<span data-ttu-id="1fbcd-214">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-214">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="1fbcd-215">A következő példa `[3]`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-215">The following example returns `[3]`:</span></span>

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="1fbcd-216">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="1fbcd-216">Example 3: object</span></span>
<span data-ttu-id="1fbcd-217">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-217">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="1fbcd-218">A következő példa `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-218">The following example returns `{"key2": "raboof"}`:</span></span>

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a><span data-ttu-id="1fbcd-219">Logikai funkciók</span><span class="sxs-lookup"><span data-stu-id="1fbcd-219">Logical functions</span></span>
<span data-ttu-id="1fbcd-220">Ezek a funkciók conditionals használható.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-220">These functions can be used in conditionals.</span></span> <span data-ttu-id="1fbcd-221">Egyes funkciókat esetleg nem támogatja az összes JSON-adattípus.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-221">Some functions may not support all JSON data types.</span></span>

### <a name="equals"></a><span data-ttu-id="1fbcd-222">egyenlő</span><span class="sxs-lookup"><span data-stu-id="1fbcd-222">equals</span></span>
<span data-ttu-id="1fbcd-223">Beolvasása `true` Ha mindkét paraméter azonos típusa és értéke.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-223">Returns `true` if both parameters have the same type and value.</span></span> <span data-ttu-id="1fbcd-224">Ez a függvény minden JSON adattípusokat támogat.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-224">This function supports all JSON data types.</span></span>

<span data-ttu-id="1fbcd-225">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-225">The following example returns `true`:</span></span>

```json
"[equals(0, 0)]"
```

<span data-ttu-id="1fbcd-226">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-226">The following example returns `true`:</span></span>

```json
"[equals('foo', 'foo')]"
```

<span data-ttu-id="1fbcd-227">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-227">The following example returns `false`:</span></span>

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a><span data-ttu-id="1fbcd-228">kevesebb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-228">less</span></span>
<span data-ttu-id="1fbcd-229">Beolvasása `true` esetén az első paraméter szigorúan kevesebb, mint a második paraméter.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-229">Returns `true` if the first parameter is strictly less than the second parameter.</span></span> <span data-ttu-id="1fbcd-230">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-230">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="1fbcd-231">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-231">The following example returns `true`:</span></span>

```json
"[less(1, 2)]"
```

<span data-ttu-id="1fbcd-232">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-232">The following example returns `false`:</span></span>

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a><span data-ttu-id="1fbcd-233">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="1fbcd-233">lessOrEquals</span></span>
<span data-ttu-id="1fbcd-234">Beolvasása `true` kisebb vagy egyenlő, mint a második paraméter az első paraméter esetén.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-234">Returns `true` if the first parameter is less than or equal to the second parameter.</span></span> <span data-ttu-id="1fbcd-235">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-235">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="1fbcd-236">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-236">The following example returns `true`:</span></span>

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a><span data-ttu-id="1fbcd-237">nagyobb</span><span class="sxs-lookup"><span data-stu-id="1fbcd-237">greater</span></span>
<span data-ttu-id="1fbcd-238">Beolvasása `true` szigorúan nagyobb, mint a második paraméter az első paraméter esetén.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-238">Returns `true` if the first parameter is strictly greater than the second parameter.</span></span> <span data-ttu-id="1fbcd-239">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-239">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="1fbcd-240">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-240">The following example returns `false`:</span></span>

```json
"[greater(1, 2)]"
```

<span data-ttu-id="1fbcd-241">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-241">The following example returns `true`:</span></span>

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a><span data-ttu-id="1fbcd-242">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="1fbcd-242">greaterOrEquals</span></span>
<span data-ttu-id="1fbcd-243">Beolvasása `true` az első paraméter nagyobb vagy egyenlő a második paraméter esetén.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-243">Returns `true` if the first parameter is greater than or equal to the second parameter.</span></span> <span data-ttu-id="1fbcd-244">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-244">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="1fbcd-245">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-245">The following example returns `true`:</span></span>

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a><span data-ttu-id="1fbcd-246">és</span><span class="sxs-lookup"><span data-stu-id="1fbcd-246">and</span></span>
<span data-ttu-id="1fbcd-247">Beolvasása `true` Ha a paraméterek értékelhetők `true`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-247">Returns `true` if all the parameters evaluate to `true`.</span></span> <span data-ttu-id="1fbcd-248">Ez a függvény csak logikai típusú két vagy több paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-248">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="1fbcd-249">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-249">The following example returns `true`:</span></span>

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="1fbcd-250">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-250">The following example returns `false`:</span></span>

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a><span data-ttu-id="1fbcd-251">vagy</span><span class="sxs-lookup"><span data-stu-id="1fbcd-251">or</span></span>
<span data-ttu-id="1fbcd-252">Beolvasása `true` Ha a paraméterek közül legalább egy értékelődik ki `true`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-252">Returns `true` if at least one of the parameters evaluates to `true`.</span></span> <span data-ttu-id="1fbcd-253">Ez a függvény csak logikai típusú két vagy több paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-253">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="1fbcd-254">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-254">The following example returns `true`:</span></span>

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="1fbcd-255">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-255">The following example returns `true`:</span></span>

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a><span data-ttu-id="1fbcd-256">nem</span><span class="sxs-lookup"><span data-stu-id="1fbcd-256">not</span></span>
<span data-ttu-id="1fbcd-257">Beolvasása `true` Ha a paraméter értéke `false`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-257">Returns `true` if the parameter evaluates to `false`.</span></span> <span data-ttu-id="1fbcd-258">Ez a függvény csak logikai típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-258">This function supports parameters only of type Boolean.</span></span>

<span data-ttu-id="1fbcd-259">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-259">The following example returns `true`:</span></span>

```json
"[not(false)]"
```

<span data-ttu-id="1fbcd-260">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-260">The following example returns `false`:</span></span>

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a><span data-ttu-id="1fbcd-261">Egyesítés</span><span class="sxs-lookup"><span data-stu-id="1fbcd-261">coalesce</span></span>
<span data-ttu-id="1fbcd-262">Az első nem üres paraméter értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-262">Returns the value of the first non-null parameter.</span></span> <span data-ttu-id="1fbcd-263">Ez a függvény minden JSON adattípusokat támogat.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-263">This function supports all JSON data types.</span></span>

<span data-ttu-id="1fbcd-264">Tegyük fel `element1` és `element2` nincs definiálva.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-264">Assume `element1` and `element2` are undefined.</span></span> <span data-ttu-id="1fbcd-265">A következő példa `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-265">The following example returns `"foobar"`:</span></span>

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a><span data-ttu-id="1fbcd-266">Átalakítás funkciók</span><span class="sxs-lookup"><span data-stu-id="1fbcd-266">Conversion functions</span></span>
<span data-ttu-id="1fbcd-267">Ezek a funkciók alakítható át JSON-adattípusok és kódolások használható.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-267">These functions can be used to convert values between JSON data types and encodings.</span></span>

### <a name="int"></a><span data-ttu-id="1fbcd-268">int</span><span class="sxs-lookup"><span data-stu-id="1fbcd-268">int</span></span>
<span data-ttu-id="1fbcd-269">A paraméter konvertál egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-269">Converts the parameter to an integer.</span></span> <span data-ttu-id="1fbcd-270">Ez a funkció támogatja a paraméterek száma és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-270">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="1fbcd-271">A következő példa `1`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-271">The following example returns `1`:</span></span>

```json
"[int('1')]"
```

<span data-ttu-id="1fbcd-272">A következő példa `2`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-272">The following example returns `2`:</span></span>

```json
"[int(2.9)]"
```

### <a name="float"></a><span data-ttu-id="1fbcd-273">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="1fbcd-273">float</span></span>
<span data-ttu-id="1fbcd-274">A paraméter egy lebegőpontos alakítja át.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-274">Converts the parameter to a floating-point.</span></span> <span data-ttu-id="1fbcd-275">Ez a funkció támogatja a paraméterek száma és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-275">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="1fbcd-276">A következő példa `1.0`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-276">The following example returns `1.0`:</span></span>

```json
"[float('1.0')]"
```

<span data-ttu-id="1fbcd-277">A következő példa `2.9`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-277">The following example returns `2.9`:</span></span>

```json
"[float(2.9)]"
```

### <a name="string"></a><span data-ttu-id="1fbcd-278">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-278">string</span></span>
<span data-ttu-id="1fbcd-279">Konvertálja a paraméter karakterlánccá.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-279">Converts the parameter to a string.</span></span> <span data-ttu-id="1fbcd-280">Ez a függvény minden JSON adattípusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-280">This function supports parameters of all JSON data types.</span></span>

<span data-ttu-id="1fbcd-281">A következő példa `"1"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-281">The following example returns `"1"`:</span></span>

```json
"[string(1)]"
```

<span data-ttu-id="1fbcd-282">A következő példa `"2.9"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-282">The following example returns `"2.9"`:</span></span>

```json
"[string(2.9)]"
```

<span data-ttu-id="1fbcd-283">A következő példa `"[1,2,3]"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-283">The following example returns `"[1,2,3]"`:</span></span>

```json
"[string([1,2,3])]"
```

<span data-ttu-id="1fbcd-284">A következő példa `"{"foo":"bar"}"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-284">The following example returns `"{"foo":"bar"}"`:</span></span>

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a><span data-ttu-id="1fbcd-285">logikai érték</span><span class="sxs-lookup"><span data-stu-id="1fbcd-285">bool</span></span>
<span data-ttu-id="1fbcd-286">A paraméter konvertálása logikai érték.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-286">Converts the parameter to a Boolean.</span></span> <span data-ttu-id="1fbcd-287">Ez a funkció támogatja a paraméterek száma, a karakterlánc és a logikai.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-287">This function supports parameters of type number, string, and Boolean.</span></span> <span data-ttu-id="1fbcd-288">A JavaScript logikai hasonló, bármelyik értéke kivéve `0` vagy `'false'` adja vissza `true`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-288">Similar to Booleans in JavaScript, any value except `0` or `'false'` returns `true`.</span></span>

<span data-ttu-id="1fbcd-289">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-289">The following example returns `true`:</span></span>

```json
"[bool(1)]"
```

<span data-ttu-id="1fbcd-290">A következő példa `false`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-290">The following example returns `false`:</span></span>

```json
"[bool(0)]"
```

<span data-ttu-id="1fbcd-291">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-291">The following example returns `true`:</span></span>

```json
"[bool(true)]"
```

<span data-ttu-id="1fbcd-292">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-292">The following example returns `true`:</span></span>

```json
"[bool('true')]"
```

### <a name="parse"></a><span data-ttu-id="1fbcd-293">elemzése</span><span class="sxs-lookup"><span data-stu-id="1fbcd-293">parse</span></span>
<span data-ttu-id="1fbcd-294">Konvertálja a paraméter natív típusa.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-294">Converts the parameter to a native type.</span></span> <span data-ttu-id="1fbcd-295">Más szóval ez a függvény megkülönbözteti a inverzét `string()`.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-295">In other words, this function is the inverse of `string()`.</span></span> <span data-ttu-id="1fbcd-296">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-296">This function supports parameters only of type string.</span></span>

<span data-ttu-id="1fbcd-297">A következő példa `1`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-297">The following example returns `1`:</span></span>

```json
"[parse('1')]"
```

<span data-ttu-id="1fbcd-298">A következő példa `true`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-298">The following example returns `true`:</span></span>

```json
"[parse('true')]"
```

<span data-ttu-id="1fbcd-299">A következő példa `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-299">The following example returns `[1,2,3]`:</span></span>

```json
"[parse('[1,2,3]')]"
```

<span data-ttu-id="1fbcd-300">A következő példa `{"foo":"bar"}`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-300">The following example returns `{"foo":"bar"}`:</span></span>

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a><span data-ttu-id="1fbcd-301">encodeBase64</span><span class="sxs-lookup"><span data-stu-id="1fbcd-301">encodeBase64</span></span>
<span data-ttu-id="1fbcd-302">A paraméter base-64 kódolású karakterlánc kódolja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-302">Encodes the parameter to a base-64 encoded string.</span></span> <span data-ttu-id="1fbcd-303">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-303">This function supports parameters only of type string.</span></span>

<span data-ttu-id="1fbcd-304">A következő példa `"Zm9vYmFy"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-304">The following example returns `"Zm9vYmFy"`:</span></span>

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a><span data-ttu-id="1fbcd-305">decodeBase64</span><span class="sxs-lookup"><span data-stu-id="1fbcd-305">decodeBase64</span></span>
<span data-ttu-id="1fbcd-306">A paraméter egy base-64 kódolású karakterlánc dekódol.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-306">Decodes the parameter from a base-64 encoded string.</span></span> <span data-ttu-id="1fbcd-307">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-307">This function supports parameters only of type string.</span></span>

<span data-ttu-id="1fbcd-308">A következő példa `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-308">The following example returns `"foobar"`:</span></span>

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a><span data-ttu-id="1fbcd-309">encodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="1fbcd-309">encodeUriComponent</span></span>
<span data-ttu-id="1fbcd-310">A paraméter egy URL-kódolású karakterlánc kódolja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-310">Encodes the parameter to a URL encoded string.</span></span> <span data-ttu-id="1fbcd-311">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-311">This function supports parameters only of type string.</span></span>

<span data-ttu-id="1fbcd-312">A következő példa `"https%3A%2F%2Fportal.azure.com%2F"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-312">The following example returns `"https%3A%2F%2Fportal.azure.com%2F"`:</span></span>

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a><span data-ttu-id="1fbcd-313">decodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="1fbcd-313">decodeUriComponent</span></span>
<span data-ttu-id="1fbcd-314">A paraméter egy URL-kódolású karakterlánc dekódol.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-314">Decodes the parameter from a URL encoded string.</span></span> <span data-ttu-id="1fbcd-315">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-315">This function supports parameters only of type string.</span></span>

<span data-ttu-id="1fbcd-316">A következő példa `"https://portal.azure.com/"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-316">The following example returns `"https://portal.azure.com/"`:</span></span>

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a><span data-ttu-id="1fbcd-317">Matematikai függvények</span><span class="sxs-lookup"><span data-stu-id="1fbcd-317">Math functions</span></span>
### <a name="add"></a><span data-ttu-id="1fbcd-318">Hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1fbcd-318">add</span></span>
<span data-ttu-id="1fbcd-319">Két számot ad, és visszaadja az eredményt.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-319">Adds two numbers, and returns the result.</span></span>

<span data-ttu-id="1fbcd-320">A következő példa `3`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-320">The following example returns `3`:</span></span>

```json
"[add(1, 2)]"
```

### <a name="sub"></a><span data-ttu-id="1fbcd-321">Sub</span><span class="sxs-lookup"><span data-stu-id="1fbcd-321">sub</span></span>
<span data-ttu-id="1fbcd-322">A második szám az első szám levonja, és az eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-322">Subtracts the second number from the first number, and returns the result.</span></span>

<span data-ttu-id="1fbcd-323">A következő példa `1`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-323">The following example returns `1`:</span></span>

```json
"[sub(3, 2)]"
```

### <a name="mul"></a><span data-ttu-id="1fbcd-324">MUL számú</span><span class="sxs-lookup"><span data-stu-id="1fbcd-324">mul</span></span>
<span data-ttu-id="1fbcd-325">Két szám szorozza meg, és visszaadja az eredményt.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-325">Multiplies two numbers, and returns the result.</span></span>

<span data-ttu-id="1fbcd-326">A következő példa `6`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-326">The following example returns `6`:</span></span>

```json
"[mul(2, 3)]"
```

### <a name="div"></a><span data-ttu-id="1fbcd-327">DIV</span><span class="sxs-lookup"><span data-stu-id="1fbcd-327">div</span></span>
<span data-ttu-id="1fbcd-328">A második szám szerint az első szám osztja, és visszaadja az eredményt.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-328">Divides the first number by the second number, and returns the result.</span></span> <span data-ttu-id="1fbcd-329">Eredménye mindig egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-329">The result is always an integer.</span></span>

<span data-ttu-id="1fbcd-330">A következő példa `2`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-330">The following example returns `2`:</span></span>

```json
"[div(6, 3)]"
```

### <a name="mod"></a><span data-ttu-id="1fbcd-331">MOD</span><span class="sxs-lookup"><span data-stu-id="1fbcd-331">mod</span></span>
<span data-ttu-id="1fbcd-332">A második szám szerint az első szám osztja, és a maradékot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-332">Divides the first number by the second number, and returns the remainder.</span></span>

<span data-ttu-id="1fbcd-333">A következő példa `0`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-333">The following example returns `0`:</span></span>

```json
"[mod(6, 3)]"
```

<span data-ttu-id="1fbcd-334">A következő példa `2`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-334">The following example returns `2`:</span></span>

```json
"[mod(6, 4)]"
```

### <a name="min"></a><span data-ttu-id="1fbcd-335">perc</span><span class="sxs-lookup"><span data-stu-id="1fbcd-335">min</span></span>
<span data-ttu-id="1fbcd-336">A kis a két számot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-336">Returns the small of the two numbers.</span></span>

<span data-ttu-id="1fbcd-337">A következő példa `1`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-337">The following example returns `1`:</span></span>

```json
"[min(1, 2)]"
```

### <a name="max"></a><span data-ttu-id="1fbcd-338">maximális</span><span class="sxs-lookup"><span data-stu-id="1fbcd-338">max</span></span>
<span data-ttu-id="1fbcd-339">A két szám nagyobb értéket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-339">Returns the larger of the two numbers.</span></span>

<span data-ttu-id="1fbcd-340">A következő példa `2`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-340">The following example returns `2`:</span></span>

```json
"[max(1, 2)]"
```

### <a name="range"></a><span data-ttu-id="1fbcd-341">tartomány</span><span class="sxs-lookup"><span data-stu-id="1fbcd-341">range</span></span>
<span data-ttu-id="1fbcd-342">A megadott tartományban egész számok sorozata állít elő.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-342">Generates a sequence of integral numbers within the specified range.</span></span>

<span data-ttu-id="1fbcd-343">A következő példa `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-343">The following example returns `[1,2,3]`:</span></span>

```json
"[range(1, 3)]"
```

### <a name="rand"></a><span data-ttu-id="1fbcd-344">VÉL</span><span class="sxs-lookup"><span data-stu-id="1fbcd-344">rand</span></span>
<span data-ttu-id="1fbcd-345">A megadott tartományban szerves véletlenszerű számot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-345">Returns a random integral number within the specified range.</span></span> <span data-ttu-id="1fbcd-346">Ez a funkció nem hoz létre olyan titkosítással biztonságos véletlenszerű számból.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-346">This function does not generate cryptographically secure random numbers.</span></span>

<span data-ttu-id="1fbcd-347">Az alábbi példa visszaadhatja `42`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-347">The following example could return `42`:</span></span>

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a><span data-ttu-id="1fbcd-348">Emelet</span><span class="sxs-lookup"><span data-stu-id="1fbcd-348">floor</span></span>
<span data-ttu-id="1fbcd-349">A legnagyobb egész számot ad vissza kisebb vagy egyenlő, mint a megadott szám.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-349">Returns the largest integer less than or equal to the specified number.</span></span>

<span data-ttu-id="1fbcd-350">A következő példa `3`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-350">The following example returns `3`:</span></span>

```json
"[floor(3.14)]"
```

### <a name="ceil"></a><span data-ttu-id="1fbcd-351">ceil</span><span class="sxs-lookup"><span data-stu-id="1fbcd-351">ceil</span></span>
<span data-ttu-id="1fbcd-352">A legnagyobb egész számot ad vissza, nagyobb vagy egyenlő a megadott számára korlátozva.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-352">Returns the largest integer greater than or equal to the specified number.</span></span>

<span data-ttu-id="1fbcd-353">A következő példa `4`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-353">The following example returns `4`:</span></span>

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a><span data-ttu-id="1fbcd-354">Dátum-funkciók</span><span class="sxs-lookup"><span data-stu-id="1fbcd-354">Date functions</span></span>
### <a name="utcnow"></a><span data-ttu-id="1fbcd-355">utcNow</span><span class="sxs-lookup"><span data-stu-id="1fbcd-355">utcNow</span></span>
<span data-ttu-id="1fbcd-356">A helyi számítógépen a jelenlegi dátum és idő ISO 8601 formátumú karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-356">Returns a string in ISO 8601 format of the current date and time on the local computer.</span></span>

<span data-ttu-id="1fbcd-357">Az alábbi példa visszaadhatja `"1990-12-31T23:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-357">The following example could return `"1990-12-31T23:59:59.000Z"`:</span></span>

```json
"[utcNow()]"
```

### <a name="addseconds"></a><span data-ttu-id="1fbcd-358">masodpercekHozzaadasa</span><span class="sxs-lookup"><span data-stu-id="1fbcd-358">addSeconds</span></span>
<span data-ttu-id="1fbcd-359">Hozzáadja a megadott időbélyeg egész számú másodperc.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-359">Adds an integral number of seconds to the specified timestamp.</span></span>

<span data-ttu-id="1fbcd-360">A következő példa `"1991-01-01T00:00:00.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-360">The following example returns `"1991-01-01T00:00:00.000Z"`:</span></span>

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a><span data-ttu-id="1fbcd-361">addMinutes</span><span class="sxs-lookup"><span data-stu-id="1fbcd-361">addMinutes</span></span>
<span data-ttu-id="1fbcd-362">Hozzáadja a megadott időbélyeg egész számú percben.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-362">Adds an integral number of minutes to the specified timestamp.</span></span>

<span data-ttu-id="1fbcd-363">A következő példa `"1991-01-01T00:00:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-363">The following example returns `"1991-01-01T00:00:59.000Z"`:</span></span>

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a><span data-ttu-id="1fbcd-364">addHours</span><span class="sxs-lookup"><span data-stu-id="1fbcd-364">addHours</span></span>
<span data-ttu-id="1fbcd-365">A megadott időbélyeg ad hozzá az órát egész szám.</span><span class="sxs-lookup"><span data-stu-id="1fbcd-365">Adds an integral number of hours to the specified timestamp.</span></span>

<span data-ttu-id="1fbcd-366">A következő példa `"1991-01-01T00:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="1fbcd-366">The following example returns `"1991-01-01T00:59:59.000Z"`:</span></span>

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a><span data-ttu-id="1fbcd-367">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fbcd-367">Next steps</span></span>
* <span data-ttu-id="1fbcd-368">Lásd a Bevezetés az Azure Resource Manager [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1fbcd-368">For an introduction to Azure Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>


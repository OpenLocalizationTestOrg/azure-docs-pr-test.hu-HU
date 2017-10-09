---
title: "aaaAzure kezelt alkalmazás, hozzon létre felhasználói felület definition funkciók |} Microsoft Docs"
description: "Hello funkciók toouse ismerteti az Azure által felügyelt alkalmazások felhasználói felületi meghatározások létrehozása"
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
ms.openlocfilehash: 7a311a25404ccaec8c19c3ed8cd7038f6887c013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="createuidefinition-functions"></a><span data-ttu-id="b81b7-103">CreateUiDefinition funkciók</span><span class="sxs-lookup"><span data-stu-id="b81b7-103">CreateUiDefinition functions</span></span>
<span data-ttu-id="b81b7-104">Ez a szakasz egy CreateUiDefinition támogatott feladatai hello aláírások.</span><span class="sxs-lookup"><span data-stu-id="b81b7-104">This section contains hello signatures for all supported functions of a CreateUiDefinition.</span></span>

<span data-ttu-id="b81b7-105">toouse egy funkciót, a szögletes zárójelek között legyen hello nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="b81b7-105">toouse a function, surround hello declaration with square brackets.</span></span> <span data-ttu-id="b81b7-106">Példa:</span><span class="sxs-lookup"><span data-stu-id="b81b7-106">For example:</span></span>

```json
"[function()]"
```

<span data-ttu-id="b81b7-107">Karakterláncok és egyéb funkciók függvény paramétereinek lehet hivatkozni, de karakterláncok kell körül, szimpla idézőjelben.</span><span class="sxs-lookup"><span data-stu-id="b81b7-107">Strings and other functions can be referenced as parameters for a function, but strings must be surrounded in single quotes.</span></span> <span data-ttu-id="b81b7-108">Példa:</span><span class="sxs-lookup"><span data-stu-id="b81b7-108">For example:</span></span>

```json
"[fn1(fn2(), 'foobar')]"
```

<span data-ttu-id="b81b7-109">Adott esetben egy függvény hello kimeneti tulajdonságainak hello pont operátor használatával is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b81b7-109">Where applicable, you can reference properties of hello output of a function by using hello dot operator.</span></span> <span data-ttu-id="b81b7-110">Példa:</span><span class="sxs-lookup"><span data-stu-id="b81b7-110">For example:</span></span>

```json
"[func().prop1]"
```

## <a name="referencing-functions"></a><span data-ttu-id="b81b7-111">Hivatkozási funkciók</span><span class="sxs-lookup"><span data-stu-id="b81b7-111">Referencing functions</span></span>
<span data-ttu-id="b81b7-112">Ezek a funkciók hello tulajdonságok vagy egy CreateUiDefinition környezetében használt tooreference kimeneteinek lehet.</span><span class="sxs-lookup"><span data-stu-id="b81b7-112">These functions can be used tooreference outputs from hello properties or context of a CreateUiDefinition.</span></span>

### <a name="basics"></a><span data-ttu-id="b81b7-113">alapvető tudnivalók</span><span class="sxs-lookup"><span data-stu-id="b81b7-113">basics</span></span>
<span data-ttu-id="b81b7-114">Hello kimeneti értékeit hello alapjai lépésben megadott elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-114">Returns hello output values of an element that is defined in hello Basics step.</span></span>

<span data-ttu-id="b81b7-115">hello alábbi példa hello-kimenet visszaadása nevű hello elem `foo` hello alapjai lépésben:</span><span class="sxs-lookup"><span data-stu-id="b81b7-115">hello following example returns hello output of hello element named `foo` in hello Basics step:</span></span>

```json
"[basics('foo')]"
```

### <a name="steps"></a><span data-ttu-id="b81b7-116">lépések</span><span class="sxs-lookup"><span data-stu-id="b81b7-116">steps</span></span>
<span data-ttu-id="b81b7-117">Hello kimeneti értékeit hello adott lépéssel definiált elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-117">Returns hello output values of an element that is defined in hello specified step.</span></span> <span data-ttu-id="b81b7-118">tooget hello kimeneti értékek hello alapjai lépésben elemek használata `basics()` helyette.</span><span class="sxs-lookup"><span data-stu-id="b81b7-118">tooget hello output values of elements in hello Basics step, use `basics()` instead.</span></span>

<span data-ttu-id="b81b7-119">hello alábbi példa hello-kimenet visszaadása nevű hello elem `bar` nevű hello lépésben `foo`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-119">hello following example returns hello output of hello element named `bar` in hello step named `foo`:</span></span>

```json
"[steps('foo').bar]"
```

### <a name="location"></a><span data-ttu-id="b81b7-120">location</span><span class="sxs-lookup"><span data-stu-id="b81b7-120">location</span></span>
<span data-ttu-id="b81b7-121">Hello alapjai lépés vagy hello kontextusban kiválasztott hello hely adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-121">Returns hello location selected in hello Basics step or hello current context.</span></span>

<span data-ttu-id="b81b7-122">hello alábbi példa visszaadhatja `"westus"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-122">hello following example could return `"westus"`:</span></span>

```json
"[location()]"
```

## <a name="string-functions"></a><span data-ttu-id="b81b7-123">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-123">String functions</span></span>
<span data-ttu-id="b81b7-124">Ezek a függvények csak JSON karakterláncok használható.</span><span class="sxs-lookup"><span data-stu-id="b81b7-124">These functions can only be used with JSON strings.</span></span>

### <a name="concat"></a><span data-ttu-id="b81b7-125">Concat</span><span class="sxs-lookup"><span data-stu-id="b81b7-125">concat</span></span>
<span data-ttu-id="b81b7-126">Egy vagy több karakterlánc fűzi össze.</span><span class="sxs-lookup"><span data-stu-id="b81b7-126">Concatenates one or more strings.</span></span>

<span data-ttu-id="b81b7-127">Például, ha hello kimeneti értéke `element1` Ha `"bar"`, akkor ez a példa hello karakterláncot ad vissza, `"foobar!"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-127">For example, if hello output value of `element1` if `"bar"`, then this example returns hello string `"foobar!"`:</span></span>

```json
"[concat('foo', steps('step1').element1), '!']"
```

### <a name="substring"></a><span data-ttu-id="b81b7-128">Substring</span><span class="sxs-lookup"><span data-stu-id="b81b7-128">substring</span></span>
<span data-ttu-id="b81b7-129">A megadott karakterlánc hello hello részét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-129">Returns hello substring of hello specified string.</span></span> <span data-ttu-id="b81b7-130">hello substring hello megadott indexnél elindul, és rendelkezik a hello megadott hossza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-130">hello substring starts at hello specified index and has hello specified length.</span></span>

<span data-ttu-id="b81b7-131">hello következő példa eredménye `"ftw"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-131">hello following example returns `"ftw"`:</span></span>

```json
"[substring('azure-ftw!!!1one', 6, 3)]"
```

### <a name="replace"></a><span data-ttu-id="b81b7-132">cserélje le</span><span class="sxs-lookup"><span data-stu-id="b81b7-132">replace</span></span>
<span data-ttu-id="b81b7-133">Egy karakterlánc, mely hello összes előfordulását a hello aktuális karakterláncban megadott karakterláncot ad vissza egy másik karakterlánc helyére kerülnek.</span><span class="sxs-lookup"><span data-stu-id="b81b7-133">Returns a string in which all occurrences of hello specified string in hello current string are replaced with another string.</span></span>

<span data-ttu-id="b81b7-134">hello következő példa eredménye `"Everything is awesome!"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-134">hello following example returns `"Everything is awesome!"`:</span></span>

```json
"[replace('Everything is terrible!', 'terrible', 'awesome')]"
```

### <a name="guid"></a><span data-ttu-id="b81b7-135">GUID</span><span class="sxs-lookup"><span data-stu-id="b81b7-135">guid</span></span>
<span data-ttu-id="b81b7-136">Karakterláncot hoz létre globálisan egyedi (GUID).</span><span class="sxs-lookup"><span data-stu-id="b81b7-136">Generates a globally unique string (GUID).</span></span>

<span data-ttu-id="b81b7-137">hello alábbi példa visszaadhatja `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-137">hello following example could return `"c7bc8bdc-7252-4a82-ba53-7c468679a511"`:</span></span>

```json
"[guid()]"
```

### <a name="tolower"></a><span data-ttu-id="b81b7-138">toLower</span><span class="sxs-lookup"><span data-stu-id="b81b7-138">toLower</span></span>
<span data-ttu-id="b81b7-139">A konvertált karakterláncot toolowercase adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-139">Returns a string converted toolowercase.</span></span>

<span data-ttu-id="b81b7-140">hello következő példa eredménye `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-140">hello following example returns `"foobar"`:</span></span>

```json
"[toLower('FOOBAR')]"
```

### <a name="toupper"></a><span data-ttu-id="b81b7-141">toUpper</span><span class="sxs-lookup"><span data-stu-id="b81b7-141">toUpper</span></span>
<span data-ttu-id="b81b7-142">A konvertált karakterláncot toouppercase adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-142">Returns a string converted toouppercase.</span></span>

<span data-ttu-id="b81b7-143">hello következő példa eredménye `"FOOBAR"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-143">hello following example returns `"FOOBAR"`:</span></span>

```json
"[toUpper('foobar')]"
```

## <a name="collection-functions"></a><span data-ttu-id="b81b7-144">Gyűjtemény-funkciók</span><span class="sxs-lookup"><span data-stu-id="b81b7-144">Collection functions</span></span>
<span data-ttu-id="b81b7-145">Ezek a funkciók gyűjtemények, például a JSON-karakterláncok, tömbök és objektumok használható.</span><span class="sxs-lookup"><span data-stu-id="b81b7-145">These functions can be used with collections, like JSON strings, arrays and objects.</span></span>

### <a name="contains"></a><span data-ttu-id="b81b7-146">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="b81b7-146">contains</span></span>
<span data-ttu-id="b81b7-147">Beolvasása `true` Ha egy karakterláncot tartalmaz hello megadott karakterláncrészlet, tömböt tartalmaz hello megadott értéket, vagy az objektum tartalmazza a megadott kulcs hello.</span><span class="sxs-lookup"><span data-stu-id="b81b7-147">Returns `true` if a string contains hello specified substring, an array contains hello specified value, or an object contains hello specified key.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="b81b7-148">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-148">Example 1: string</span></span>
<span data-ttu-id="b81b7-149">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-149">hello following example returns `true`:</span></span>

```json
"[contains('foobar', 'foo')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="b81b7-150">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="b81b7-150">Example 2: array</span></span>
<span data-ttu-id="b81b7-151">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-151">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="b81b7-152">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-152">hello following example returns `false`:</span></span>

```json
"[contains(steps('foo').element1, 4)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="b81b7-153">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="b81b7-153">Example 3: object</span></span>
<span data-ttu-id="b81b7-154">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b81b7-154">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="b81b7-155">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-155">hello following example returns `true`:</span></span>

```json
"[contains(steps('foo').element1, 'key1')]"
```

### <a name="length"></a><span data-ttu-id="b81b7-156">Hossza</span><span class="sxs-lookup"><span data-stu-id="b81b7-156">length</span></span>
<span data-ttu-id="b81b7-157">Egy karakterlánc, hello száma egy tömbben szereplő értékeket, vagy az objektum kulcsainak száma hello hello számú karaktert adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-157">Returns hello number of characters in a string, hello number of values in an array, or hello number of keys in an object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="b81b7-158">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-158">Example 1: string</span></span>
<span data-ttu-id="b81b7-159">hello következő példa eredménye `6`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-159">hello following example returns `6`:</span></span>

```json
"[length('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="b81b7-160">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="b81b7-160">Example 2: array</span></span>
<span data-ttu-id="b81b7-161">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-161">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="b81b7-162">hello következő példa eredménye `3`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-162">hello following example returns `3`:</span></span>

```json
"[length(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="b81b7-163">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="b81b7-163">Example 3: object</span></span>
<span data-ttu-id="b81b7-164">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b81b7-164">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="b81b7-165">hello következő példa eredménye `2`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-165">hello following example returns `2`:</span></span>

```json
"[length(steps('foo').element1)]"
```

### <a name="empty"></a><span data-ttu-id="b81b7-166">üres</span><span class="sxs-lookup"><span data-stu-id="b81b7-166">empty</span></span>
<span data-ttu-id="b81b7-167">Beolvasása `true` Ha hello karakterlánc, a tömb vagy objektum értéke null vagy üres.</span><span class="sxs-lookup"><span data-stu-id="b81b7-167">Returns `true` if hello string, array, or object is null or empty.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="b81b7-168">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-168">Example 1: string</span></span>
<span data-ttu-id="b81b7-169">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-169">hello following example returns `true`:</span></span>

```json
"[empty('')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="b81b7-170">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="b81b7-170">Example 2: array</span></span>
<span data-ttu-id="b81b7-171">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-171">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="b81b7-172">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-172">hello following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="b81b7-173">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="b81b7-173">Example 3: object</span></span>
<span data-ttu-id="b81b7-174">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b81b7-174">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="b81b7-175">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-175">hello following example returns `false`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

#### <a name="example-4-null-and-undefined"></a><span data-ttu-id="b81b7-176">4. példa: null, és nincs definiálva</span><span class="sxs-lookup"><span data-stu-id="b81b7-176">Example 4: null and undefined</span></span>
<span data-ttu-id="b81b7-177">Tegyük fel `element1` van `null` vagy nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="b81b7-177">Assume `element1` is `null` or undefined.</span></span> <span data-ttu-id="b81b7-178">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-178">hello following example returns `true`:</span></span>

```json
"[empty(steps('foo').element1)]"
```

### <a name="first"></a><span data-ttu-id="b81b7-179">első</span><span class="sxs-lookup"><span data-stu-id="b81b7-179">first</span></span>
<span data-ttu-id="b81b7-180">A megadott értéket ad vissza hello első karakterének hello karakterlánc; első érték a megadott tömb hello; vagy hello első kulcs és hello megadott objektum értéket.</span><span class="sxs-lookup"><span data-stu-id="b81b7-180">Returns hello first character of hello specified string; first value of hello specified array; or hello first key and value of hello specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="b81b7-181">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-181">Example 1: string</span></span>
<span data-ttu-id="b81b7-182">hello következő példa eredménye `"f"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-182">hello following example returns `"f"`:</span></span>

```json
"[first('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="b81b7-183">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="b81b7-183">Example 2: array</span></span>
<span data-ttu-id="b81b7-184">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-184">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="b81b7-185">hello következő példa eredménye `1`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-185">hello following example returns `1`:</span></span>

```json
"[first(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="b81b7-186">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="b81b7-186">Example 3: object</span></span>
<span data-ttu-id="b81b7-187">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b81b7-187">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="b81b7-188">hello következő példa eredménye `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-188">hello following example returns `{"key1": "foobar"}`:</span></span>

```json
"[first(steps('foo').element1)]"
```

### <a name="last"></a><span data-ttu-id="b81b7-189">utolsó</span><span class="sxs-lookup"><span data-stu-id="b81b7-189">last</span></span>
<span data-ttu-id="b81b7-190">Beolvasása hello utolsó karakterének hello megadott karakterlánc, hello utolsó értéke a megadott tömb hello, vagy utolsó kulcs és a megadott objektum hello értékének hello.</span><span class="sxs-lookup"><span data-stu-id="b81b7-190">Returns hello last character of hello specified string, hello last value of hello specified array, or hello last key and value of hello specified object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="b81b7-191">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-191">Example 1: string</span></span>
<span data-ttu-id="b81b7-192">hello következő példa eredménye `"r"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-192">hello following example returns `"r"`:</span></span>

```json
"[last('foobar')]"
```

#### <a name="example-2-array"></a><span data-ttu-id="b81b7-193">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="b81b7-193">Example 2: array</span></span>
<span data-ttu-id="b81b7-194">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-194">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="b81b7-195">hello következő példa eredménye `2`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-195">hello following example returns `2`:</span></span>

```json
"[last(steps('foo').element1)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="b81b7-196">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="b81b7-196">Example 3: object</span></span>
<span data-ttu-id="b81b7-197">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b81b7-197">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="b81b7-198">hello következő példa eredménye `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-198">hello following example returns `{"key2": "raboof"}`:</span></span>

```json
"[last(steps('foo').element1)]"
```

### <a name="take"></a><span data-ttu-id="b81b7-199">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="b81b7-199">take</span></span>
<span data-ttu-id="b81b7-200">A megadott számú hello indítás hello karakterlánc összefüggő karaktert, folytonos értékek hello indítás hello tömb a megadott számú vagy összefüggő kulcsok és értékek hello indítás hello objektum a megadott számú adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-200">Returns a specified number of contiguous characters from hello start of hello string, a specified number of contiguous values from hello start of hello array, or a specified number of contiguous keys and values from hello start of hello object.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="b81b7-201">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-201">Example 1: string</span></span>
<span data-ttu-id="b81b7-202">hello következő példa eredménye `"foo"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-202">hello following example returns `"foo"`:</span></span>

```json
"[take('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="b81b7-203">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="b81b7-203">Example 2: array</span></span>
<span data-ttu-id="b81b7-204">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-204">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="b81b7-205">hello következő példa eredménye `[1, 2]`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-205">hello following example returns `[1, 2]`:</span></span>

```json
"[take(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="b81b7-206">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="b81b7-206">Example 3: object</span></span>
<span data-ttu-id="b81b7-207">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b81b7-207">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```

<span data-ttu-id="b81b7-208">hello következő példa eredménye `{"key1": "foobar"}`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-208">hello following example returns `{"key1": "foobar"}`:</span></span>

```json
"[take(steps('foo').element1, 1)]"
```

### <a name="skip"></a><span data-ttu-id="b81b7-209">Kihagyása</span><span class="sxs-lookup"><span data-stu-id="b81b7-209">skip</span></span>
<span data-ttu-id="b81b7-210">A megadott számú elemet egy gyűjtemény megkerüli, és elemek fennmaradó hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-210">Bypasses a specified number of elements in a collection, and then returns hello remaining elements.</span></span>

#### <a name="example-1-string"></a><span data-ttu-id="b81b7-211">1. példa: karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-211">Example 1: string</span></span>
<span data-ttu-id="b81b7-212">hello következő példa eredménye `"bar"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-212">hello following example returns `"bar"`:</span></span>

```json
"[skip('foobar', 3)]"
```

#### <a name="example-2-array"></a><span data-ttu-id="b81b7-213">2. példa: tömb</span><span class="sxs-lookup"><span data-stu-id="b81b7-213">Example 2: array</span></span>
<span data-ttu-id="b81b7-214">Tegyük fel `element1` adja vissza `[1, 2, 3]`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-214">Assume `element1` returns `[1, 2, 3]`.</span></span> <span data-ttu-id="b81b7-215">hello következő példa eredménye `[3]`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-215">hello following example returns `[3]`:</span></span>

```json
"[skip(steps('foo').element1, 2)]"
```

#### <a name="example-3-object"></a><span data-ttu-id="b81b7-216">3. példa: objektum</span><span class="sxs-lookup"><span data-stu-id="b81b7-216">Example 3: object</span></span>
<span data-ttu-id="b81b7-217">Tegyük fel `element1` adja vissza:</span><span class="sxs-lookup"><span data-stu-id="b81b7-217">Assume `element1` returns:</span></span>

```json
{
  "key1": "foobar",
  "key2": "raboof"
}
```
<span data-ttu-id="b81b7-218">hello következő példa eredménye `{"key2": "raboof"}`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-218">hello following example returns `{"key2": "raboof"}`:</span></span>

```json
"[skip(steps('foo').element1, 1)]"
```

## <a name="logical-functions"></a><span data-ttu-id="b81b7-219">Logikai funkciók</span><span class="sxs-lookup"><span data-stu-id="b81b7-219">Logical functions</span></span>
<span data-ttu-id="b81b7-220">Ezek a funkciók conditionals használható.</span><span class="sxs-lookup"><span data-stu-id="b81b7-220">These functions can be used in conditionals.</span></span> <span data-ttu-id="b81b7-221">Egyes funkciókat esetleg nem támogatja az összes JSON-adattípus.</span><span class="sxs-lookup"><span data-stu-id="b81b7-221">Some functions may not support all JSON data types.</span></span>

### <a name="equals"></a><span data-ttu-id="b81b7-222">egyenlő</span><span class="sxs-lookup"><span data-stu-id="b81b7-222">equals</span></span>
<span data-ttu-id="b81b7-223">Beolvasása `true` Ha mindkét paraméter hello azonos adja meg, és értéket.</span><span class="sxs-lookup"><span data-stu-id="b81b7-223">Returns `true` if both parameters have hello same type and value.</span></span> <span data-ttu-id="b81b7-224">Ez a függvény minden JSON adattípusokat támogat.</span><span class="sxs-lookup"><span data-stu-id="b81b7-224">This function supports all JSON data types.</span></span>

<span data-ttu-id="b81b7-225">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-225">hello following example returns `true`:</span></span>

```json
"[equals(0, 0)]"
```

<span data-ttu-id="b81b7-226">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-226">hello following example returns `true`:</span></span>

```json
"[equals('foo', 'foo')]"
```

<span data-ttu-id="b81b7-227">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-227">hello following example returns `false`:</span></span>

```json
"[equals('abc', ['a', 'b', 'c'])]"
```

### <a name="less"></a><span data-ttu-id="b81b7-228">kevesebb</span><span class="sxs-lookup"><span data-stu-id="b81b7-228">less</span></span>
<span data-ttu-id="b81b7-229">Beolvasása `true` Ha hello első paraméter szigorúan kisebb, mint a második paraméter hello.</span><span class="sxs-lookup"><span data-stu-id="b81b7-229">Returns `true` if hello first parameter is strictly less than hello second parameter.</span></span> <span data-ttu-id="b81b7-230">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-230">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="b81b7-231">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-231">hello following example returns `true`:</span></span>

```json
"[less(1, 2)]"
```

<span data-ttu-id="b81b7-232">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-232">hello following example returns `false`:</span></span>

```json
"[less('9', '10')]"
```

### <a name="lessorequals"></a><span data-ttu-id="b81b7-233">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="b81b7-233">lessOrEquals</span></span>
<span data-ttu-id="b81b7-234">Beolvasása `true` hello első paraméter értéke kisebb vagy egyenlő, mint ha toohello második paramétere.</span><span class="sxs-lookup"><span data-stu-id="b81b7-234">Returns `true` if hello first parameter is less than or equal toohello second parameter.</span></span> <span data-ttu-id="b81b7-235">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-235">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="b81b7-236">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-236">hello following example returns `true`:</span></span>

```json
"[lessOrEquals(2, 2)]"
```

### <a name="greater"></a><span data-ttu-id="b81b7-237">nagyobb</span><span class="sxs-lookup"><span data-stu-id="b81b7-237">greater</span></span>
<span data-ttu-id="b81b7-238">Beolvasása `true` szigorúan nagyobb, mint a második paraméter hello hello első paraméter esetén.</span><span class="sxs-lookup"><span data-stu-id="b81b7-238">Returns `true` if hello first parameter is strictly greater than hello second parameter.</span></span> <span data-ttu-id="b81b7-239">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-239">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="b81b7-240">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-240">hello following example returns `false`:</span></span>

```json
"[greater(1, 2)]"
```

<span data-ttu-id="b81b7-241">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-241">hello following example returns `true`:</span></span>

```json
"[greater('9', '10')]"
```

### <a name="greaterorequals"></a><span data-ttu-id="b81b7-242">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="b81b7-242">greaterOrEquals</span></span>
<span data-ttu-id="b81b7-243">Beolvasása `true` Ha hello első paraméter nagyobb vagy egyenlő toohello második paramétere.</span><span class="sxs-lookup"><span data-stu-id="b81b7-243">Returns `true` if hello first parameter is greater than or equal toohello second parameter.</span></span> <span data-ttu-id="b81b7-244">Ez a függvény csak a típus számát és a karakterlánc paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-244">This function supports parameters only of type number and string.</span></span>

<span data-ttu-id="b81b7-245">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-245">hello following example returns `true`:</span></span>

```json
"[greaterOrEquals(2, 2)]"
```

### <a name="and"></a><span data-ttu-id="b81b7-246">és</span><span class="sxs-lookup"><span data-stu-id="b81b7-246">and</span></span>
<span data-ttu-id="b81b7-247">Beolvasása `true` ha összes hello paraméter kiértékelése túl`true`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-247">Returns `true` if all hello parameters evaluate too`true`.</span></span> <span data-ttu-id="b81b7-248">Ez a függvény csak logikai típusú két vagy több paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-248">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="b81b7-249">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-249">hello following example returns `true`:</span></span>

```json
"[and(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="b81b7-250">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-250">hello following example returns `false`:</span></span>

```json
"[and(equals(0, 0), greater(1, 2))]"
```

### <a name="or"></a><span data-ttu-id="b81b7-251">vagy</span><span class="sxs-lookup"><span data-stu-id="b81b7-251">or</span></span>
<span data-ttu-id="b81b7-252">Beolvasása `true` Ha hello paraméterek közül legalább egy értéke túl`true`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-252">Returns `true` if at least one of hello parameters evaluates too`true`.</span></span> <span data-ttu-id="b81b7-253">Ez a függvény csak logikai típusú két vagy több paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-253">This function supports two or more parameters only of type Boolean.</span></span>

<span data-ttu-id="b81b7-254">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-254">hello following example returns `true`:</span></span>

```json
"[or(equals(0, 0), equals('foo', 'foo'), less(1, 2))]"
```

<span data-ttu-id="b81b7-255">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-255">hello following example returns `true`:</span></span>

```json
"[or(equals(0, 0), greater(1, 2))]"
```

### <a name="not"></a><span data-ttu-id="b81b7-256">nem</span><span class="sxs-lookup"><span data-stu-id="b81b7-256">not</span></span>
<span data-ttu-id="b81b7-257">Beolvasása `true` Ha hello paraméter értéke túl`false`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-257">Returns `true` if hello parameter evaluates too`false`.</span></span> <span data-ttu-id="b81b7-258">Ez a függvény csak logikai típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-258">This function supports parameters only of type Boolean.</span></span>

<span data-ttu-id="b81b7-259">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-259">hello following example returns `true`:</span></span>

```json
"[not(false)]"
```

<span data-ttu-id="b81b7-260">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-260">hello following example returns `false`:</span></span>

```json
"[not(equals(0, 0))]"
```

### <a name="coalesce"></a><span data-ttu-id="b81b7-261">Egyesítés</span><span class="sxs-lookup"><span data-stu-id="b81b7-261">coalesce</span></span>
<span data-ttu-id="b81b7-262">Beolvasása hello hello első nem üres paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="b81b7-262">Returns hello value of hello first non-null parameter.</span></span> <span data-ttu-id="b81b7-263">Ez a függvény minden JSON adattípusokat támogat.</span><span class="sxs-lookup"><span data-stu-id="b81b7-263">This function supports all JSON data types.</span></span>

<span data-ttu-id="b81b7-264">Tegyük fel `element1` és `element2` nincs definiálva.</span><span class="sxs-lookup"><span data-stu-id="b81b7-264">Assume `element1` and `element2` are undefined.</span></span> <span data-ttu-id="b81b7-265">hello következő példa eredménye `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-265">hello following example returns `"foobar"`:</span></span>

```json
"[coalesce(steps('foo').element1, steps('foo').element2, 'foobar')]"
```

## <a name="conversion-functions"></a><span data-ttu-id="b81b7-266">Átalakítás funkciók</span><span class="sxs-lookup"><span data-stu-id="b81b7-266">Conversion functions</span></span>
<span data-ttu-id="b81b7-267">Ezek a funkciók használt tooconvert értékek JSON-adattípusok és kódolások között lehet.</span><span class="sxs-lookup"><span data-stu-id="b81b7-267">These functions can be used tooconvert values between JSON data types and encodings.</span></span>

### <a name="int"></a><span data-ttu-id="b81b7-268">int</span><span class="sxs-lookup"><span data-stu-id="b81b7-268">int</span></span>
<span data-ttu-id="b81b7-269">Hello paraméter tooan egész számra konvertál.</span><span class="sxs-lookup"><span data-stu-id="b81b7-269">Converts hello parameter tooan integer.</span></span> <span data-ttu-id="b81b7-270">Ez a funkció támogatja a paraméterek száma és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b81b7-270">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="b81b7-271">hello következő példa eredménye `1`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-271">hello following example returns `1`:</span></span>

```json
"[int('1')]"
```

<span data-ttu-id="b81b7-272">hello következő példa eredménye `2`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-272">hello following example returns `2`:</span></span>

```json
"[int(2.9)]"
```

### <a name="float"></a><span data-ttu-id="b81b7-273">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="b81b7-273">float</span></span>
<span data-ttu-id="b81b7-274">Hello paraméter tooa lebegőpontos alakítja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-274">Converts hello parameter tooa floating-point.</span></span> <span data-ttu-id="b81b7-275">Ez a funkció támogatja a paraméterek száma és a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b81b7-275">This function supports parameters of type number and string.</span></span>

<span data-ttu-id="b81b7-276">hello következő példa eredménye `1.0`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-276">hello following example returns `1.0`:</span></span>

```json
"[float('1.0')]"
```

<span data-ttu-id="b81b7-277">hello következő példa eredménye `2.9`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-277">hello following example returns `2.9`:</span></span>

```json
"[float(2.9)]"
```

### <a name="string"></a><span data-ttu-id="b81b7-278">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b81b7-278">string</span></span>
<span data-ttu-id="b81b7-279">Számmá alakít hello paraméter tooa karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="b81b7-279">Converts hello parameter tooa string.</span></span> <span data-ttu-id="b81b7-280">Ez a függvény minden JSON adattípusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-280">This function supports parameters of all JSON data types.</span></span>

<span data-ttu-id="b81b7-281">hello következő példa eredménye `"1"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-281">hello following example returns `"1"`:</span></span>

```json
"[string(1)]"
```

<span data-ttu-id="b81b7-282">hello következő példa eredménye `"2.9"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-282">hello following example returns `"2.9"`:</span></span>

```json
"[string(2.9)]"
```

<span data-ttu-id="b81b7-283">hello következő példa eredménye `"[1,2,3]"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-283">hello following example returns `"[1,2,3]"`:</span></span>

```json
"[string([1,2,3])]"
```

<span data-ttu-id="b81b7-284">hello következő példa eredménye `"{"foo":"bar"}"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-284">hello following example returns `"{"foo":"bar"}"`:</span></span>

```json
"[string({\"foo\":\"bar\"})]"
```

### <a name="bool"></a><span data-ttu-id="b81b7-285">logikai érték</span><span class="sxs-lookup"><span data-stu-id="b81b7-285">bool</span></span>
<span data-ttu-id="b81b7-286">Hello paraméter tooa logikai alakítja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-286">Converts hello parameter tooa Boolean.</span></span> <span data-ttu-id="b81b7-287">Ez a funkció támogatja a paraméterek száma, a karakterlánc és a logikai.</span><span class="sxs-lookup"><span data-stu-id="b81b7-287">This function supports parameters of type number, string, and Boolean.</span></span> <span data-ttu-id="b81b7-288">A JavaScript, kivéve bármely érték hasonló tooBooleans `0` vagy `'false'` adja vissza `true`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-288">Similar tooBooleans in JavaScript, any value except `0` or `'false'` returns `true`.</span></span>

<span data-ttu-id="b81b7-289">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-289">hello following example returns `true`:</span></span>

```json
"[bool(1)]"
```

<span data-ttu-id="b81b7-290">hello következő példa eredménye `false`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-290">hello following example returns `false`:</span></span>

```json
"[bool(0)]"
```

<span data-ttu-id="b81b7-291">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-291">hello following example returns `true`:</span></span>

```json
"[bool(true)]"
```

<span data-ttu-id="b81b7-292">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-292">hello following example returns `true`:</span></span>

```json
"[bool('true')]"
```

### <a name="parse"></a><span data-ttu-id="b81b7-293">elemzése</span><span class="sxs-lookup"><span data-stu-id="b81b7-293">parse</span></span>
<span data-ttu-id="b81b7-294">Konvertálja hello paraméter tooa natív típusa.</span><span class="sxs-lookup"><span data-stu-id="b81b7-294">Converts hello parameter tooa native type.</span></span> <span data-ttu-id="b81b7-295">Más szóval ez a funkció akkor hello inverzét `string()`.</span><span class="sxs-lookup"><span data-stu-id="b81b7-295">In other words, this function is hello inverse of `string()`.</span></span> <span data-ttu-id="b81b7-296">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-296">This function supports parameters only of type string.</span></span>

<span data-ttu-id="b81b7-297">hello következő példa eredménye `1`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-297">hello following example returns `1`:</span></span>

```json
"[parse('1')]"
```

<span data-ttu-id="b81b7-298">hello következő példa eredménye `true`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-298">hello following example returns `true`:</span></span>

```json
"[parse('true')]"
```

<span data-ttu-id="b81b7-299">hello következő példa eredménye `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-299">hello following example returns `[1,2,3]`:</span></span>

```json
"[parse('[1,2,3]')]"
```

<span data-ttu-id="b81b7-300">hello következő példa eredménye `{"foo":"bar"}`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-300">hello following example returns `{"foo":"bar"}`:</span></span>

```json
"[parse('{\"foo\":\"bar\"}')]"
```

### <a name="encodebase64"></a><span data-ttu-id="b81b7-301">encodeBase64</span><span class="sxs-lookup"><span data-stu-id="b81b7-301">encodeBase64</span></span>
<span data-ttu-id="b81b7-302">Hello paraméter tooa base-64 kódolású karakterlánc kódolja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-302">Encodes hello parameter tooa base-64 encoded string.</span></span> <span data-ttu-id="b81b7-303">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-303">This function supports parameters only of type string.</span></span>

<span data-ttu-id="b81b7-304">hello következő példa eredménye `"Zm9vYmFy"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-304">hello following example returns `"Zm9vYmFy"`:</span></span>

```json
"[encodeBase64('foobar')]"
```

### <a name="decodebase64"></a><span data-ttu-id="b81b7-305">decodeBase64</span><span class="sxs-lookup"><span data-stu-id="b81b7-305">decodeBase64</span></span>
<span data-ttu-id="b81b7-306">Dekódol hello paraméter egy base-64 kódolású karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b81b7-306">Decodes hello parameter from a base-64 encoded string.</span></span> <span data-ttu-id="b81b7-307">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-307">This function supports parameters only of type string.</span></span>

<span data-ttu-id="b81b7-308">hello következő példa eredménye `"foobar"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-308">hello following example returns `"foobar"`:</span></span>

```json
"[decodeBase64('Zm9vYmFy')]"
```

### <a name="encodeuricomponent"></a><span data-ttu-id="b81b7-309">encodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="b81b7-309">encodeUriComponent</span></span>
<span data-ttu-id="b81b7-310">Hello paraméter tooa URL-kódolású karakterlánc kódolja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-310">Encodes hello parameter tooa URL encoded string.</span></span> <span data-ttu-id="b81b7-311">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-311">This function supports parameters only of type string.</span></span>

<span data-ttu-id="b81b7-312">hello következő példa eredménye `"https%3A%2F%2Fportal.azure.com%2F"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-312">hello following example returns `"https%3A%2F%2Fportal.azure.com%2F"`:</span></span>

```json
"[encodeUriComponent('https://portal.azure.com/')]"
```

### <a name="decodeuricomponent"></a><span data-ttu-id="b81b7-313">decodeUriComponent</span><span class="sxs-lookup"><span data-stu-id="b81b7-313">decodeUriComponent</span></span>
<span data-ttu-id="b81b7-314">Dekódol egy URL-kódolású karakterlánc hello paramétert.</span><span class="sxs-lookup"><span data-stu-id="b81b7-314">Decodes hello parameter from a URL encoded string.</span></span> <span data-ttu-id="b81b7-315">Ez a függvény csak a karakterlánc típusú paramétereket támogatja.</span><span class="sxs-lookup"><span data-stu-id="b81b7-315">This function supports parameters only of type string.</span></span>

<span data-ttu-id="b81b7-316">hello következő példa eredménye `"https://portal.azure.com/"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-316">hello following example returns `"https://portal.azure.com/"`:</span></span>

```json
"[decodeUriComponent('https%3A%2F%2Fportal.azure.com%2F')]"
```

## <a name="math-functions"></a><span data-ttu-id="b81b7-317">Matematikai függvények</span><span class="sxs-lookup"><span data-stu-id="b81b7-317">Math functions</span></span>
### <a name="add"></a><span data-ttu-id="b81b7-318">Hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b81b7-318">add</span></span>
<span data-ttu-id="b81b7-319">Két számot ad, és hello eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-319">Adds two numbers, and returns hello result.</span></span>

<span data-ttu-id="b81b7-320">hello következő példa eredménye `3`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-320">hello following example returns `3`:</span></span>

```json
"[add(1, 2)]"
```

### <a name="sub"></a><span data-ttu-id="b81b7-321">Sub</span><span class="sxs-lookup"><span data-stu-id="b81b7-321">sub</span></span>
<span data-ttu-id="b81b7-322">Csökkenti a második szám hello hello első számot, és hello eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-322">Subtracts hello second number from hello first number, and returns hello result.</span></span>

<span data-ttu-id="b81b7-323">hello következő példa eredménye `1`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-323">hello following example returns `1`:</span></span>

```json
"[sub(3, 2)]"
```

### <a name="mul"></a><span data-ttu-id="b81b7-324">MUL számú</span><span class="sxs-lookup"><span data-stu-id="b81b7-324">mul</span></span>
<span data-ttu-id="b81b7-325">Két szám szorozza meg, és hello eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-325">Multiplies two numbers, and returns hello result.</span></span>

<span data-ttu-id="b81b7-326">hello következő példa eredménye `6`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-326">hello following example returns `6`:</span></span>

```json
"[mul(2, 3)]"
```

### <a name="div"></a><span data-ttu-id="b81b7-327">DIV</span><span class="sxs-lookup"><span data-stu-id="b81b7-327">div</span></span>
<span data-ttu-id="b81b7-328">Hello első száma a hello második száma osztja, és hello eredményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-328">Divides hello first number by hello second number, and returns hello result.</span></span> <span data-ttu-id="b81b7-329">hello eredménye mindig egy egész számot.</span><span class="sxs-lookup"><span data-stu-id="b81b7-329">hello result is always an integer.</span></span>

<span data-ttu-id="b81b7-330">hello következő példa eredménye `2`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-330">hello following example returns `2`:</span></span>

```json
"[div(6, 3)]"
```

### <a name="mod"></a><span data-ttu-id="b81b7-331">MOD</span><span class="sxs-lookup"><span data-stu-id="b81b7-331">mod</span></span>
<span data-ttu-id="b81b7-332">Hello első száma a hello második száma osztja, és hello maradékot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-332">Divides hello first number by hello second number, and returns hello remainder.</span></span>

<span data-ttu-id="b81b7-333">hello következő példa eredménye `0`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-333">hello following example returns `0`:</span></span>

```json
"[mod(6, 3)]"
```

<span data-ttu-id="b81b7-334">hello következő példa eredménye `2`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-334">hello following example returns `2`:</span></span>

```json
"[mod(6, 4)]"
```

### <a name="min"></a><span data-ttu-id="b81b7-335">perc</span><span class="sxs-lookup"><span data-stu-id="b81b7-335">min</span></span>
<span data-ttu-id="b81b7-336">Beolvasása hello kis hello két szám.</span><span class="sxs-lookup"><span data-stu-id="b81b7-336">Returns hello small of hello two numbers.</span></span>

<span data-ttu-id="b81b7-337">hello következő példa eredménye `1`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-337">hello following example returns `1`:</span></span>

```json
"[min(1, 2)]"
```

### <a name="max"></a><span data-ttu-id="b81b7-338">maximális</span><span class="sxs-lookup"><span data-stu-id="b81b7-338">max</span></span>
<span data-ttu-id="b81b7-339">Hello két szám nagyobb értéket ad vissza hello.</span><span class="sxs-lookup"><span data-stu-id="b81b7-339">Returns hello larger of hello two numbers.</span></span>

<span data-ttu-id="b81b7-340">hello következő példa eredménye `2`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-340">hello following example returns `2`:</span></span>

```json
"[max(1, 2)]"
```

### <a name="range"></a><span data-ttu-id="b81b7-341">tartomány</span><span class="sxs-lookup"><span data-stu-id="b81b7-341">range</span></span>
<span data-ttu-id="b81b7-342">Állít elő egy egész sorozatát hello számokat a megadott tartomány.</span><span class="sxs-lookup"><span data-stu-id="b81b7-342">Generates a sequence of integral numbers within hello specified range.</span></span>

<span data-ttu-id="b81b7-343">hello következő példa eredménye `[1,2,3]`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-343">hello following example returns `[1,2,3]`:</span></span>

```json
"[range(1, 3)]"
```

### <a name="rand"></a><span data-ttu-id="b81b7-344">VÉL</span><span class="sxs-lookup"><span data-stu-id="b81b7-344">rand</span></span>
<span data-ttu-id="b81b7-345">Egy véletlenszerű visszaadja a megadott tartományon belül hello egész szám.</span><span class="sxs-lookup"><span data-stu-id="b81b7-345">Returns a random integral number within hello specified range.</span></span> <span data-ttu-id="b81b7-346">Ez a funkció nem hoz létre olyan titkosítással biztonságos véletlenszerű számból.</span><span class="sxs-lookup"><span data-stu-id="b81b7-346">This function does not generate cryptographically secure random numbers.</span></span>

<span data-ttu-id="b81b7-347">hello alábbi példa visszaadhatja `42`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-347">hello following example could return `42`:</span></span>

```json
"[rand(-100, 100)]"
```

### <a name="floor"></a><span data-ttu-id="b81b7-348">Emelet</span><span class="sxs-lookup"><span data-stu-id="b81b7-348">floor</span></span>
<span data-ttu-id="b81b7-349">Visszaadja a hello legnagyobb egész szám kisebb vagy egyenlő, mint toohello megadott szám.</span><span class="sxs-lookup"><span data-stu-id="b81b7-349">Returns hello largest integer less than or equal toohello specified number.</span></span>

<span data-ttu-id="b81b7-350">hello következő példa eredménye `3`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-350">hello following example returns `3`:</span></span>

```json
"[floor(3.14)]"
```

### <a name="ceil"></a><span data-ttu-id="b81b7-351">ceil</span><span class="sxs-lookup"><span data-stu-id="b81b7-351">ceil</span></span>
<span data-ttu-id="b81b7-352">Beolvasása hello legnagyobb egész szám nagyobb, mint vagy egyenlő toohello megadott szám.</span><span class="sxs-lookup"><span data-stu-id="b81b7-352">Returns hello largest integer greater than or equal toohello specified number.</span></span>

<span data-ttu-id="b81b7-353">hello következő példa eredménye `4`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-353">hello following example returns `4`:</span></span>

```json
"[ceil(3.14)]"
```

## <a name="date-functions"></a><span data-ttu-id="b81b7-354">Dátum-funkciók</span><span class="sxs-lookup"><span data-stu-id="b81b7-354">Date functions</span></span>
### <a name="utcnow"></a><span data-ttu-id="b81b7-355">utcNow</span><span class="sxs-lookup"><span data-stu-id="b81b7-355">utcNow</span></span>
<span data-ttu-id="b81b7-356">Az aktuális dátum és idő helyi számítógép hello hello ISO 8601 formátumú karakterláncot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b81b7-356">Returns a string in ISO 8601 format of hello current date and time on hello local computer.</span></span>

<span data-ttu-id="b81b7-357">hello alábbi példa visszaadhatja `"1990-12-31T23:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-357">hello following example could return `"1990-12-31T23:59:59.000Z"`:</span></span>

```json
"[utcNow()]"
```

### <a name="addseconds"></a><span data-ttu-id="b81b7-358">masodpercekHozzaadasa</span><span class="sxs-lookup"><span data-stu-id="b81b7-358">addSeconds</span></span>
<span data-ttu-id="b81b7-359">Hozzáadja a megadott másodperc toohello egész számú időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="b81b7-359">Adds an integral number of seconds toohello specified timestamp.</span></span>

<span data-ttu-id="b81b7-360">hello következő példa eredménye `"1991-01-01T00:00:00.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-360">hello following example returns `"1991-01-01T00:00:00.000Z"`:</span></span>

```json
"[addSeconds('1990-12-31T23:59:60Z', 1)]"
```

### <a name="addminutes"></a><span data-ttu-id="b81b7-361">addMinutes</span><span class="sxs-lookup"><span data-stu-id="b81b7-361">addMinutes</span></span>
<span data-ttu-id="b81b7-362">Hozzáadja a megadott perc toohello egész számú időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="b81b7-362">Adds an integral number of minutes toohello specified timestamp.</span></span>

<span data-ttu-id="b81b7-363">hello következő példa eredménye `"1991-01-01T00:00:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-363">hello following example returns `"1991-01-01T00:00:59.000Z"`:</span></span>

```json
"[addMinutes('1990-12-31T23:59:59Z', 1)]"
```

### <a name="addhours"></a><span data-ttu-id="b81b7-364">addHours</span><span class="sxs-lookup"><span data-stu-id="b81b7-364">addHours</span></span>
<span data-ttu-id="b81b7-365">Hozzáadja a megadott órák toohello egész számú időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="b81b7-365">Adds an integral number of hours toohello specified timestamp.</span></span>

<span data-ttu-id="b81b7-366">hello következő példa eredménye `"1991-01-01T00:59:59.000Z"`:</span><span class="sxs-lookup"><span data-stu-id="b81b7-366">hello following example returns `"1991-01-01T00:59:59.000Z"`:</span></span>

```json
"[addHours('1990-12-31T23:59:59Z', 1)]"
```

## <a name="next-steps"></a><span data-ttu-id="b81b7-367">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b81b7-367">Next steps</span></span>
* <span data-ttu-id="b81b7-368">Egy bevezető tooAzure erőforrás-kezelő, lásd: [Azure Resource Manager áttekintése](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b81b7-368">For an introduction tooAzure Resource Manager, see [Azure Resource Manager overview](resource-group-overview.md).</span></span>


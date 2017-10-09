---
title: "aaaResource Manager Sablonfüggvényei |} Microsoft Docs"
description: "Hello funkciók toouse ismerteti az Azure Resource Manager sablon tooretrieve értékek, karakterláncok és írhatók, és központi telepítési információk beolvasása."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: tomfitz
ms.openlocfilehash: d1b2e68a33d75058f83d6972dadb33a6390d49b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-template-functions"></a><span data-ttu-id="6bdf4-103">Az Azure Resource Manager sablonfüggvényei</span><span class="sxs-lookup"><span data-stu-id="6bdf4-103">Azure Resource Manager template functions</span></span>
<span data-ttu-id="6bdf4-104">Ez a témakör ismerteti az Azure Resource Manager-sablonokban használható összes hello függvények.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-104">This topic describes all hello functions you can use in an Azure Resource Manager template.</span></span>

<span data-ttu-id="6bdf4-105">Funkciók hozzáadása szögletes zárójelben befoglaló azokat a sablonokat: `[` és `]`, illetve.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-105">You add functions in your templates by enclosing them within brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="6bdf4-106">hello kifejezés telepítés során történik.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-106">hello expression is evaluated during deployment.</span></span> <span data-ttu-id="6bdf4-107">Megírva egy szöveges karakterlánc, miközben a hello eredménye hello kifejezés kiértékelése eltérő típusú JSON, például egy tömb, objektum vagy egész szám lehet.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-107">While written as a string literal, hello result of evaluating hello expression can be of a different JSON type, such as an array, object, or integer.</span></span> <span data-ttu-id="6bdf4-108">Csak a például a JavaScript, függvényhívások-ként formázott `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-108">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="6bdf4-109">Tulajdonságok hello pont és a [index] operátorok használatával hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-109">You reference properties by using hello dot and [index] operators.</span></span>

<span data-ttu-id="6bdf4-110">Egy sablon kifejezés nem lehet 24,576 karakternél.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-110">A template expression cannot exceed 24,576 characters.</span></span>

<span data-ttu-id="6bdf4-111">Sablon függvényeket és paramétereket nem különböztetik meg.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-111">Template functions and their parameters are case-insensitive.</span></span> <span data-ttu-id="6bdf4-112">Például az erőforrás-kezelő oldja fel **variables('var1')** és **VARIABLES('VAR1')** hello azonos módon.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-112">For example, Resource Manager resolves **variables('var1')** and **VARIABLES('VAR1')** as hello same.</span></span> <span data-ttu-id="6bdf4-113">Amikor értékeli ki, kivéve, ha hello függvény kifejezetten módosítja (például toUpper vagy toLower) eset, hello függvény megőrzi-e a kis-hello.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-113">When evaluated, unless hello function expressly modifies case (such as toUpper or toLower), hello function preserves hello case.</span></span> <span data-ttu-id="6bdf4-114">Előfordulhat, hogy az egyes erőforrástípusok eset követelmények, függetlenül attól, milyen funkciók kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-114">Certain resource types may have case requirements irrespective of how functions are evaluated.</span></span>

<a id="array" />
<a id="coalesce" />
<a id="concatarray" />
<a id="contains" />
<a id="createarray" />
<a id="empty" />
<a id="first" />
<a id="intersection" />
<a id="last" />
<a id="length" />
<a id="min" />
<a id="max" />
<a id="range" />
<a id="skip" />
<a id="take" />
<a id="union" />

## <a name="array-and-object-functions"></a><span data-ttu-id="6bdf4-115">A tömb és objektum funkciók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-115">Array and object functions</span></span>
<span data-ttu-id="6bdf4-116">Erőforrás-kezelő számos funkciókat nyújt, tömbök és objektumok.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-116">Resource Manager provides several functions for working with arrays and objects.</span></span>

* [<span data-ttu-id="6bdf4-117">a tömb</span><span class="sxs-lookup"><span data-stu-id="6bdf4-117">array</span></span>](resource-group-template-functions-array.md#array)
* [<span data-ttu-id="6bdf4-118">Egyesítés</span><span class="sxs-lookup"><span data-stu-id="6bdf4-118">coalesce</span></span>](resource-group-template-functions-array.md#coalesce)
* [<span data-ttu-id="6bdf4-119">Concat</span><span class="sxs-lookup"><span data-stu-id="6bdf4-119">concat</span></span>](resource-group-template-functions-array.md#concat)
* [<span data-ttu-id="6bdf4-120">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="6bdf4-120">contains</span></span>](resource-group-template-functions-array.md#contains)
* [<span data-ttu-id="6bdf4-121">createArray</span><span class="sxs-lookup"><span data-stu-id="6bdf4-121">createArray</span></span>](resource-group-template-functions-array.md#createarray)
* [<span data-ttu-id="6bdf4-122">üres</span><span class="sxs-lookup"><span data-stu-id="6bdf4-122">empty</span></span>](resource-group-template-functions-array.md#empty)
* [<span data-ttu-id="6bdf4-123">első</span><span class="sxs-lookup"><span data-stu-id="6bdf4-123">first</span></span>](resource-group-template-functions-array.md#first)
* [<span data-ttu-id="6bdf4-124">metszetének</span><span class="sxs-lookup"><span data-stu-id="6bdf4-124">intersection</span></span>](resource-group-template-functions-array.md#intersection)
* [<span data-ttu-id="6bdf4-125">JSON-ban</span><span class="sxs-lookup"><span data-stu-id="6bdf4-125">json</span></span>](resource-group-template-functions-array.md#json)
* [<span data-ttu-id="6bdf4-126">utolsó</span><span class="sxs-lookup"><span data-stu-id="6bdf4-126">last</span></span>](resource-group-template-functions-array.md#last)
* [<span data-ttu-id="6bdf4-127">hossza</span><span class="sxs-lookup"><span data-stu-id="6bdf4-127">length</span></span>](resource-group-template-functions-array.md#length)
* [<span data-ttu-id="6bdf4-128">perc</span><span class="sxs-lookup"><span data-stu-id="6bdf4-128">min</span></span>](resource-group-template-functions-array.md#min)
* [<span data-ttu-id="6bdf4-129">maximális</span><span class="sxs-lookup"><span data-stu-id="6bdf4-129">max</span></span>](resource-group-template-functions-array.md#max)
* [<span data-ttu-id="6bdf4-130">tartomány</span><span class="sxs-lookup"><span data-stu-id="6bdf4-130">range</span></span>](resource-group-template-functions-array.md#range)
* [<span data-ttu-id="6bdf4-131">hagyja ki</span><span class="sxs-lookup"><span data-stu-id="6bdf4-131">skip</span></span>](resource-group-template-functions-array.md#skip)
* [<span data-ttu-id="6bdf4-132">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="6bdf4-132">take</span></span>](resource-group-template-functions-array.md#take)
* [<span data-ttu-id="6bdf4-133">a UNION</span><span class="sxs-lookup"><span data-stu-id="6bdf4-133">union</span></span>](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a><span data-ttu-id="6bdf4-134">Összehasonlítás funkciók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-134">Comparison functions</span></span>
<span data-ttu-id="6bdf4-135">Erőforrás-kezelő számos funkciókat nyújt a sablonokban összehasonlításához.</span><span class="sxs-lookup"><span data-stu-id="6bdf4-135">Resource Manager provides several functions for making comparisons in your templates.</span></span>

* [<span data-ttu-id="6bdf4-136">egyenlő</span><span class="sxs-lookup"><span data-stu-id="6bdf4-136">equals</span></span>](resource-group-template-functions-comparison.md#equals)
* [<span data-ttu-id="6bdf4-137">kevesebb</span><span class="sxs-lookup"><span data-stu-id="6bdf4-137">less</span></span>](resource-group-template-functions-comparison.md#less)
* [<span data-ttu-id="6bdf4-138">lessOrEquals</span><span class="sxs-lookup"><span data-stu-id="6bdf4-138">lessOrEquals</span></span>](resource-group-template-functions-comparison.md#lessorequals)
* [<span data-ttu-id="6bdf4-139">nagyobb</span><span class="sxs-lookup"><span data-stu-id="6bdf4-139">greater</span></span>](resource-group-template-functions-comparison.md#greater)
* [<span data-ttu-id="6bdf4-140">greaterOrEquals</span><span class="sxs-lookup"><span data-stu-id="6bdf4-140">greaterOrEquals</span></span>](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a><span data-ttu-id="6bdf4-141">Központi telepítési érték funkciók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-141">Deployment value functions</span></span>
<span data-ttu-id="6bdf4-142">A Resource Manager biztosít hello alábbi az értékek lekérése hello sablon szakaszok működik, és kapcsolódó toohello telepítési értékeket:</span><span class="sxs-lookup"><span data-stu-id="6bdf4-142">Resource Manager provides hello following functions for getting values from sections of hello template and values related toohello deployment:</span></span>

* [<span data-ttu-id="6bdf4-143">központi telepítés</span><span class="sxs-lookup"><span data-stu-id="6bdf4-143">deployment</span></span>](resource-group-template-functions-deployment.md#deployment)
* [<span data-ttu-id="6bdf4-144">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="6bdf4-144">parameters</span></span>](resource-group-template-functions-deployment.md#parameters)
* [<span data-ttu-id="6bdf4-145">változók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-145">variables</span></span>](resource-group-template-functions-deployment.md#variables)

<a id="add" />
<a id="copyindex" />
<a id="div" />
<a id="float" />
<a id="int" />
<a id="minint" />
<a id="maxint" />
<a id="mod" />
<a id="mul" />
<a id="sub" />

## <a name="logical-functions"></a><span data-ttu-id="6bdf4-146">Logikai funkciók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-146">Logical functions</span></span>
<span data-ttu-id="6bdf4-147">A Resource Manager biztosít a következő funkciók logikai feltételek való munkához hello:</span><span class="sxs-lookup"><span data-stu-id="6bdf4-147">Resource Manager provides hello following functions for working with logical conditions:</span></span>

* [<span data-ttu-id="6bdf4-148">és</span><span class="sxs-lookup"><span data-stu-id="6bdf4-148">and</span></span>](resource-group-template-functions-logical.md#and)
* [<span data-ttu-id="6bdf4-149">logikai érték</span><span class="sxs-lookup"><span data-stu-id="6bdf4-149">bool</span></span>](resource-group-template-functions-logical.md#bool)
* [<span data-ttu-id="6bdf4-150">Ha</span><span class="sxs-lookup"><span data-stu-id="6bdf4-150">if</span></span>](resource-group-template-functions-logical.md#if)
* [<span data-ttu-id="6bdf4-151">nem</span><span class="sxs-lookup"><span data-stu-id="6bdf4-151">not</span></span>](resource-group-template-functions-logical.md#not)
* [<span data-ttu-id="6bdf4-152">vagy</span><span class="sxs-lookup"><span data-stu-id="6bdf4-152">or</span></span>](resource-group-template-functions-logical.md#or)

## <a name="numeric-functions"></a><span data-ttu-id="6bdf4-153">Numerikus funkciók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-153">Numeric functions</span></span>
<span data-ttu-id="6bdf4-154">A Resource Manager biztosít a következő funkciók egész számok való munkához hello:</span><span class="sxs-lookup"><span data-stu-id="6bdf4-154">Resource Manager provides hello following functions for working with integers:</span></span>

* [<span data-ttu-id="6bdf4-155">hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6bdf4-155">add</span></span>](resource-group-template-functions-numeric.md#add)
* [<span data-ttu-id="6bdf4-156">copyIndex</span><span class="sxs-lookup"><span data-stu-id="6bdf4-156">copyIndex</span></span>](resource-group-template-functions-numeric.md#copyindex)
* [<span data-ttu-id="6bdf4-157">DIV</span><span class="sxs-lookup"><span data-stu-id="6bdf4-157">div</span></span>](resource-group-template-functions-numeric.md#div)
* [<span data-ttu-id="6bdf4-158">lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="6bdf4-158">float</span></span>](resource-group-template-functions-numeric.md#float)
* [<span data-ttu-id="6bdf4-159">int</span><span class="sxs-lookup"><span data-stu-id="6bdf4-159">int</span></span>](resource-group-template-functions-numeric.md#int)
* [<span data-ttu-id="6bdf4-160">perc</span><span class="sxs-lookup"><span data-stu-id="6bdf4-160">min</span></span>](resource-group-template-functions-numeric.md#min)
* [<span data-ttu-id="6bdf4-161">maximális</span><span class="sxs-lookup"><span data-stu-id="6bdf4-161">max</span></span>](resource-group-template-functions-numeric.md#max)
* [<span data-ttu-id="6bdf4-162">MOD</span><span class="sxs-lookup"><span data-stu-id="6bdf4-162">mod</span></span>](resource-group-template-functions-numeric.md#mod)
* [<span data-ttu-id="6bdf4-163">MUL számú</span><span class="sxs-lookup"><span data-stu-id="6bdf4-163">mul</span></span>](resource-group-template-functions-numeric.md#mul)
* [<span data-ttu-id="6bdf4-164">Sub</span><span class="sxs-lookup"><span data-stu-id="6bdf4-164">sub</span></span>](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a><span data-ttu-id="6bdf4-165">Erőforrás-funkciók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-165">Resource functions</span></span>
<span data-ttu-id="6bdf4-166">A Resource Manager biztosít a következő funkciók az erőforrás-értékek első hello:</span><span class="sxs-lookup"><span data-stu-id="6bdf4-166">Resource Manager provides hello following functions for getting resource values:</span></span>

* [<span data-ttu-id="6bdf4-167">listKeys és a {Value} lista</span><span class="sxs-lookup"><span data-stu-id="6bdf4-167">listKeys and list{Value}</span></span>](resource-group-template-functions-resource.md#listkeys)
* [<span data-ttu-id="6bdf4-168">szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="6bdf4-168">providers</span></span>](resource-group-template-functions-resource.md#providers)
* [<span data-ttu-id="6bdf4-169">hivatkozás</span><span class="sxs-lookup"><span data-stu-id="6bdf4-169">reference</span></span>](resource-group-template-functions-resource.md#reference)
* [<span data-ttu-id="6bdf4-170">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="6bdf4-170">resourceGroup</span></span>](resource-group-template-functions-resource.md#resourcegroup)
* [<span data-ttu-id="6bdf4-171">resourceId</span><span class="sxs-lookup"><span data-stu-id="6bdf4-171">resourceId</span></span>](resource-group-template-functions-resource.md#resourceid)
* [<span data-ttu-id="6bdf4-172">előfizetést</span><span class="sxs-lookup"><span data-stu-id="6bdf4-172">subscription</span></span>](resource-group-template-functions-resource.md#subscription)

<a id="base64" />
<a id="base64tojson" />
<a id="base64tostring" />
<a id="concat" />
<a id="containsstring" />
<a id="datauri" />
<a id="datauritostring" />
<a id="emptystring" />
<a id="endswith" />
<a id="firststring" />
<a id="indexof" />
<a id="laststring" />
<a id="lastindexof" />
<a id="lengthstring" />
<a id="padleft" />
<a id="replace" />
<a id="skipstring" />
<a id="split" />
<a id="startswith" />
<a id="string" />
<a id="substring" />
<a id="takestring" />
<a id="tolower" />
<a id="toupper" />
<a id="trim" />
<a id="uniquestring" />
<a id="uri" />
<a id="uricomponent" />
<a id="uricomponenttostring" />

## <a name="string-functions"></a><span data-ttu-id="6bdf4-173">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="6bdf4-173">String functions</span></span>
<span data-ttu-id="6bdf4-174">A Resource Manager biztosít a következő funkciók karakterláncok való munkához hello:</span><span class="sxs-lookup"><span data-stu-id="6bdf4-174">Resource Manager provides hello following functions for working with strings:</span></span>

* [<span data-ttu-id="6bdf4-175">a Base64</span><span class="sxs-lookup"><span data-stu-id="6bdf4-175">base64</span></span>](resource-group-template-functions-string.md#base64)
* [<span data-ttu-id="6bdf4-176">base64ToJson</span><span class="sxs-lookup"><span data-stu-id="6bdf4-176">base64ToJson</span></span>](resource-group-template-functions-string.md#base64tojson)
* [<span data-ttu-id="6bdf4-177">base64ToString</span><span class="sxs-lookup"><span data-stu-id="6bdf4-177">base64ToString</span></span>](resource-group-template-functions-string.md#base64tostring)
* [<span data-ttu-id="6bdf4-178">Concat</span><span class="sxs-lookup"><span data-stu-id="6bdf4-178">concat</span></span>](resource-group-template-functions-string.md#concat)
* [<span data-ttu-id="6bdf4-179">tartalmazza</span><span class="sxs-lookup"><span data-stu-id="6bdf4-179">contains</span></span>](resource-group-template-functions-string.md#contains)
* [<span data-ttu-id="6bdf4-180">dataUri</span><span class="sxs-lookup"><span data-stu-id="6bdf4-180">dataUri</span></span>](resource-group-template-functions-string.md#datauri)
* [<span data-ttu-id="6bdf4-181">dataUriToString</span><span class="sxs-lookup"><span data-stu-id="6bdf4-181">dataUriToString</span></span>](resource-group-template-functions-string.md#datauritostring)
* [<span data-ttu-id="6bdf4-182">üres</span><span class="sxs-lookup"><span data-stu-id="6bdf4-182">empty</span></span>](resource-group-template-functions-string.md#empty)
* [<span data-ttu-id="6bdf4-183">megadott módon végződő</span><span class="sxs-lookup"><span data-stu-id="6bdf4-183">endsWith</span></span>](resource-group-template-functions-string.md#endswith)
* [<span data-ttu-id="6bdf4-184">első</span><span class="sxs-lookup"><span data-stu-id="6bdf4-184">first</span></span>](resource-group-template-functions-string.md#first)
* [<span data-ttu-id="6bdf4-185">indexOf</span><span class="sxs-lookup"><span data-stu-id="6bdf4-185">indexOf</span></span>](resource-group-template-functions-string.md#indexof)
* [<span data-ttu-id="6bdf4-186">utolsó</span><span class="sxs-lookup"><span data-stu-id="6bdf4-186">last</span></span>](resource-group-template-functions-string.md#last)
* [<span data-ttu-id="6bdf4-187">lastIndexOf</span><span class="sxs-lookup"><span data-stu-id="6bdf4-187">lastIndexOf</span></span>](resource-group-template-functions-string.md#lastindexof)
* [<span data-ttu-id="6bdf4-188">hossza</span><span class="sxs-lookup"><span data-stu-id="6bdf4-188">length</span></span>](resource-group-template-functions-string.md#length)
* [<span data-ttu-id="6bdf4-189">padLeft</span><span class="sxs-lookup"><span data-stu-id="6bdf4-189">padLeft</span></span>](resource-group-template-functions-string.md#padleft)
* [<span data-ttu-id="6bdf4-190">cserélje le</span><span class="sxs-lookup"><span data-stu-id="6bdf4-190">replace</span></span>](resource-group-template-functions-string.md#replace)
* [<span data-ttu-id="6bdf4-191">hagyja ki</span><span class="sxs-lookup"><span data-stu-id="6bdf4-191">skip</span></span>](resource-group-template-functions-string.md#skip)
* [<span data-ttu-id="6bdf4-192">felosztás</span><span class="sxs-lookup"><span data-stu-id="6bdf4-192">split</span></span>](resource-group-template-functions-string.md#split)
* [<span data-ttu-id="6bdf4-193">startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="6bdf4-193">startsWith</span></span>](resource-group-template-functions-string.md#startswith)
* [<span data-ttu-id="6bdf4-194">karakterlánc</span><span class="sxs-lookup"><span data-stu-id="6bdf4-194">string</span></span>](resource-group-template-functions-string.md#string)
* [<span data-ttu-id="6bdf4-195">Substring</span><span class="sxs-lookup"><span data-stu-id="6bdf4-195">substring</span></span>](resource-group-template-functions-string.md#substring)
* [<span data-ttu-id="6bdf4-196">hajtsa végre a megfelelő</span><span class="sxs-lookup"><span data-stu-id="6bdf4-196">take</span></span>](resource-group-template-functions-string.md#take)
* [<span data-ttu-id="6bdf4-197">toLower</span><span class="sxs-lookup"><span data-stu-id="6bdf4-197">toLower</span></span>](resource-group-template-functions-string.md#tolower)
* [<span data-ttu-id="6bdf4-198">toUpper</span><span class="sxs-lookup"><span data-stu-id="6bdf4-198">toUpper</span></span>](resource-group-template-functions-string.md#toupper)
* [<span data-ttu-id="6bdf4-199">Trim</span><span class="sxs-lookup"><span data-stu-id="6bdf4-199">trim</span></span>](resource-group-template-functions-string.md#trim)
* [<span data-ttu-id="6bdf4-200">uniqueString</span><span class="sxs-lookup"><span data-stu-id="6bdf4-200">uniqueString</span></span>](resource-group-template-functions-string.md#uniquestring)
* [<span data-ttu-id="6bdf4-201">URI</span><span class="sxs-lookup"><span data-stu-id="6bdf4-201">uri</span></span>](resource-group-template-functions-string.md#uri)
* [<span data-ttu-id="6bdf4-202">uriComponent</span><span class="sxs-lookup"><span data-stu-id="6bdf4-202">uriComponent</span></span>](resource-group-template-functions-string.md#uricomponent)
* [<span data-ttu-id="6bdf4-203">uriComponentToString</span><span class="sxs-lookup"><span data-stu-id="6bdf4-203">uriComponentToString</span></span>](resource-group-template-functions-string.md#uricomponenttostring)


## <a name="next-steps"></a><span data-ttu-id="6bdf4-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6bdf4-204">Next steps</span></span>
* <span data-ttu-id="6bdf4-205">Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="6bdf4-205">For a description of hello sections in an Azure Resource Manager template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="6bdf4-206">toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager eszközzel](resource-group-linked-templates.md)</span><span class="sxs-lookup"><span data-stu-id="6bdf4-206">toomerge multiple templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md)</span></span>
* <span data-ttu-id="6bdf4-207">megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példányát az Azure Resource Manager létrehozása](resource-group-create-multiple.md)</span><span class="sxs-lookup"><span data-stu-id="6bdf4-207">tooiterate a specified number of times when creating a type of resource, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md)</span></span>
* <span data-ttu-id="6bdf4-208">toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="6bdf4-208">toosee how toodeploy hello template you have created, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md)</span></span>


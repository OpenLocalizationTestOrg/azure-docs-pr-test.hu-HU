---
title: "Az Azure Stream Analytics JavaScript felhasználó által definiált függvények |} Microsoft Docs"
description: "Hajtsa végre a felhasználó által definiált függvények JavaScript speciális lekérdezési mechanika"
keywords: "JavaScript, felhasználó által definiált feladatokat az UDF-ben"
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: e4a9e6c7078031240c22a51378c0459426b7f626
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a><span data-ttu-id="7d6cc-104">Az Azure Stream Analytics JavaScript felhasználó által definiált függvények</span><span class="sxs-lookup"><span data-stu-id="7d6cc-104">Azure Stream Analytics JavaScript user-defined functions</span></span>
<span data-ttu-id="7d6cc-105">Az Azure Stream Analytics a felhasználó által definiált függvények JavaScript nyelven írt támogatja.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-105">Azure Stream Analytics supports user-defined functions written in JavaScript.</span></span> <span data-ttu-id="7d6cc-106">A gazdag készlete **karakterlánc**, **RegExp szolgáltatást**, **matematikai**, **tömb**, és **dátum** módszerek a JavaScript a Stream Analytics-feladatok úgy válnak egyre könnyebben létrehozása összetett adatátalakítást biztosít.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-106">With the rich set of **String**, **RegExp**, **Math**, **Array**, and **Date** methods that JavaScript provides, complex data transformations with Stream Analytics jobs become easier to create.</span></span>

## <a name="javascript-user-defined-functions"></a><span data-ttu-id="7d6cc-107">Felhasználó által definiált függvények JavaScript</span><span class="sxs-lookup"><span data-stu-id="7d6cc-107">JavaScript user-defined functions</span></span>
<span data-ttu-id="7d6cc-108">Felhasználó által definiált függvények JavaScript támogatja állapot nélküli, csak számítási skaláris függvények, amelyek nem igényelnek külső kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-108">JavaScript user-defined functions support stateless, compute-only scalar functions that do not require external connectivity.</span></span> <span data-ttu-id="7d6cc-109">Egy függvény visszatérési értéke csak a skaláris (önálló) érték lehet.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-109">The return value of a function can only be a scalar (single) value.</span></span> <span data-ttu-id="7d6cc-110">Miután hozzáadta a JavaScript felhasználó által definiált függvény egy feladatot, a funkció használata bárhol a lekérdezésben, például a beépített skaláris függvényt.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-110">After you add a JavaScript user-defined function to a job, you can use the function anywhere in the query, like a built-in scalar function.</span></span>

<span data-ttu-id="7d6cc-111">Az alábbiakban néhány forgatókönyv, ahol hasznosak lehetnek felhasználó által definiált függvények JavaScript:</span><span class="sxs-lookup"><span data-stu-id="7d6cc-111">Here are some scenarios where you might find JavaScript user-defined functions useful:</span></span>
* <span data-ttu-id="7d6cc-112">Elemzés és kezelésére, amelyek reguláris kifejezés funkciók, például karakterláncok **Regexp_Replace()** és **Regexp_Extract()**</span><span class="sxs-lookup"><span data-stu-id="7d6cc-112">Parsing and manipulating strings that have regular expression functions, for example, **Regexp_Replace()** and **Regexp_Extract()**</span></span>
* <span data-ttu-id="7d6cc-113">Dekódolás és adatok, például bináris hexadecimális átalakítás kódolás</span><span class="sxs-lookup"><span data-stu-id="7d6cc-113">Decoding and encoding data, for example, binary-to-hex conversion</span></span>
* <span data-ttu-id="7d6cc-114">JavaScript mathematic számításokat végez **matematikai** funkciók</span><span class="sxs-lookup"><span data-stu-id="7d6cc-114">Performing mathematic computations with JavaScript **Math** functions</span></span>
* <span data-ttu-id="7d6cc-115">Például a rendezési, a csatlakozást, a Keresés és a fill tömb műveletek végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7d6cc-115">Performing array operations like sort, join, find, and fill</span></span>

<span data-ttu-id="7d6cc-116">A következőkben, amely a Stream Analytics egy JavaScript felhasználó által definiált függvény nem hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="7d6cc-116">Here are some things that you cannot do with a JavaScript user-defined function in Stream Analytics:</span></span>
* <span data-ttu-id="7d6cc-117">Kimenő külső REST-végpontok, például végrehajtása hívás fordított IP keresési vagy referencia történő adatlekérést külső forrásból</span><span class="sxs-lookup"><span data-stu-id="7d6cc-117">Call out external REST endpoints, for example, performing reverse IP lookup or pulling reference data from an external source</span></span>
* <span data-ttu-id="7d6cc-118">Hajtsa végre az egyéni esemény szerializálás vagy a bemenetek/kimenetek deszerializálási</span><span class="sxs-lookup"><span data-stu-id="7d6cc-118">Perform custom event format serialization or deserialization on inputs/outputs</span></span>
* <span data-ttu-id="7d6cc-119">Egyéni összesítések létrehozása</span><span class="sxs-lookup"><span data-stu-id="7d6cc-119">Create custom aggregates</span></span>

<span data-ttu-id="7d6cc-120">Bár a Funkciók, például **Date.GetDate()** vagy **Math.random()** nem blokkolja a funkciók-definícióban az használatuk kerülendő.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-120">Although functions like **Date.GetDate()** or **Math.random()** are not blocked in the functions definition, you should avoid using them.</span></span> <span data-ttu-id="7d6cc-121">Ezek a funkciók **nem** ugyanazt az eredményt adja vissza, minden egyes keresheti őket, és az Azure Stream Analytics szolgáltatás nem őrzi meg a napló, a függvény meghívásához, és adott vissza eredményt.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-121">These functions **do not** return the same result every time you call them, and the Azure Stream Analytics service does not keep a journal of function invocations and returned results.</span></span> <span data-ttu-id="7d6cc-122">Ha a függvény különböző eredményt adja vissza, az azonos esemény, ismételhetőség nem garantált a feladat újraindításakor, akkor vagy a Stream Analytics szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-122">If a function returns different result on the same events, repeatability is not guaranteed when a job is restarted by you or by the Stream Analytics service.</span></span>

## <a name="add-a-javascript-user-defined-function-in-the-azure-portal"></a><span data-ttu-id="7d6cc-123">A JavaScript felhasználó által definiált függvény hozzáadása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7d6cc-123">Add a JavaScript user-defined function in the Azure portal</span></span>
<span data-ttu-id="7d6cc-124">Hozzon létre egy egyszerű JavaScript felhasználó által definiált függvényt a meglévő Stream Analytics-feladat, hajtsa végre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7d6cc-124">To create a simple JavaScript user-defined function under an existing Stream Analytics job, do these steps:</span></span>

1.  <span data-ttu-id="7d6cc-125">Az Azure-portálon keresse meg a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-125">In the Azure portal, find your Stream Analytics job.</span></span>
2.  <span data-ttu-id="7d6cc-126">A **feladat TOPOLÓGIA**, válassza ki a függvényt.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-126">Under **JOB TOPOLOGY**, select your function.</span></span> <span data-ttu-id="7d6cc-127">Megjelenik a funkciók listája üres.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-127">An empty list of functions appears.</span></span>
3.  <span data-ttu-id="7d6cc-128">Hozzon létre egy új felhasználó által definiált függvényt, jelölje be **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-128">To create a new user-defined function, select **Add**.</span></span>
4.  <span data-ttu-id="7d6cc-129">Az a **új függvény** panelen a **függvénytípus**, jelölje be **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-129">On the **New Function** blade, for **Function Type**, select **JavaScript**.</span></span> <span data-ttu-id="7d6cc-130">Egy alapértelmezett függvény sablon megjelenik a szerkesztőt.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-130">A default function template appears in the editor.</span></span>
5.  <span data-ttu-id="7d6cc-131">Az a **UDF alias**, adja meg **hex2Int**, és az alábbiak szerint módosítsa a függvény végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="7d6cc-131">For the **UDF alias**, enter **hex2Int**, and change the function implementation as follows:</span></span>

    ```
    // Convert Hex value to integer.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  <span data-ttu-id="7d6cc-132">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-132">Select **Save**.</span></span> <span data-ttu-id="7d6cc-133">A függvény függvények listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-133">Your function appears in the list of functions.</span></span>
7.  <span data-ttu-id="7d6cc-134">Jelölje be az új **hex2Int** működik, és ellenőrizze a függvény definícióját.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-134">Select the new **hex2Int** function, and check the function definition.</span></span> <span data-ttu-id="7d6cc-135">Minden függvényeknek a **UDF** előtagot function aliasra.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-135">All functions have a **UDF** prefix added to the function alias.</span></span> <span data-ttu-id="7d6cc-136">Kell *előtag* függvény hívható a Stream Analytics-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-136">You need to *include the prefix* when you call the function in your Stream Analytics query.</span></span> <span data-ttu-id="7d6cc-137">Ebben az esetben hívható **UDF.hex2Int**.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-137">In this case, you call **UDF.hex2Int**.</span></span>

## <a name="call-a-javascript-user-defined-function-in-a-query"></a><span data-ttu-id="7d6cc-138">A JavaScript felhasználói függvényt hívni a lekérdezésben</span><span class="sxs-lookup"><span data-stu-id="7d6cc-138">Call a JavaScript user-defined function in a query</span></span>

1. <span data-ttu-id="7d6cc-139">A lekérdezés-szerkesztő a **feladat TOPOLÓGIA**, jelölje be **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-139">In the query editor, under **JOB TOPOLOGY**, select **Query**.</span></span>
2.  <span data-ttu-id="7d6cc-140">Szerkessze a lekérdezést, és majd hívja meg a felhasználó által definiált függvényt, ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="7d6cc-140">Edit your query, and then call the user-defined function, like this:</span></span>

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  <span data-ttu-id="7d6cc-141">A minta adatfájl feltöltése, kattintson a jobb gombbal a projekt bemeneti.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-141">To upload the sample data file, right-click the job input.</span></span>
4.  <span data-ttu-id="7d6cc-142">Válassza ki a lekérdezés teszteléséhez **tesztelése**.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-142">To test your query, select **Test**.</span></span>


## <a name="supported-javascript-objects"></a><span data-ttu-id="7d6cc-143">Támogatott JavaScript-objektumok</span><span class="sxs-lookup"><span data-stu-id="7d6cc-143">Supported JavaScript objects</span></span>
<span data-ttu-id="7d6cc-144">Az Azure Stream Analytics JavaScript felhasználó által definiált függvények standard szintű, beépített JavaScript-objektumok támogatja.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-144">Azure Stream Analytics JavaScript user-defined functions support standard, built-in JavaScript objects.</span></span> <span data-ttu-id="7d6cc-145">Ezek az objektumok listáját lásd: [globális objektumok](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span><span class="sxs-lookup"><span data-stu-id="7d6cc-145">For a list of these objects, see [Global Objects](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span></span>

### <a name="stream-analytics-and-javascript-type-conversion"></a><span data-ttu-id="7d6cc-146">A Stream Analytics és a JavaScript adattípus átalakítása</span><span class="sxs-lookup"><span data-stu-id="7d6cc-146">Stream Analytics and JavaScript type conversion</span></span>

<span data-ttu-id="7d6cc-147">A típusokat, hogy a Stream Analytics lekérdezési nyelv és a JavaScript-támogatás különbségek vannak.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-147">There are differences in the types that the Stream Analytics query language and JavaScript support.</span></span> <span data-ttu-id="7d6cc-148">Ez a táblázat felsorolja az átalakítás leképezéseket a kettő között:</span><span class="sxs-lookup"><span data-stu-id="7d6cc-148">This table lists the conversion mappings between the two:</span></span>

<span data-ttu-id="7d6cc-149">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7d6cc-149">Stream Analytics</span></span> | <span data-ttu-id="7d6cc-150">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7d6cc-150">JavaScript</span></span>
--- | ---
<span data-ttu-id="7d6cc-151">bigint</span><span class="sxs-lookup"><span data-stu-id="7d6cc-151">bigint</span></span> | <span data-ttu-id="7d6cc-152">Szám (JavaScript csak jelenthet egész számok legfeljebb pontosan 2 ^ 53)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-152">Number (JavaScript can only represent integers up to precisely 2^53)</span></span>
<span data-ttu-id="7d6cc-153">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="7d6cc-153">DateTime</span></span> | <span data-ttu-id="7d6cc-154">Dátum (JavaScript csak támogatja ezredmásodperc)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-154">Date (JavaScript only supports milliseconds)</span></span>
<span data-ttu-id="7d6cc-155">Dupla</span><span class="sxs-lookup"><span data-stu-id="7d6cc-155">double</span></span> | <span data-ttu-id="7d6cc-156">Szám</span><span class="sxs-lookup"><span data-stu-id="7d6cc-156">Number</span></span>
<span data-ttu-id="7d6cc-157">típus: nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-157">nvarchar(MAX)</span></span> | <span data-ttu-id="7d6cc-158">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6cc-158">String</span></span>
<span data-ttu-id="7d6cc-159">rekord</span><span class="sxs-lookup"><span data-stu-id="7d6cc-159">Record</span></span> | <span data-ttu-id="7d6cc-160">Objektum</span><span class="sxs-lookup"><span data-stu-id="7d6cc-160">Object</span></span>
<span data-ttu-id="7d6cc-161">A tömb</span><span class="sxs-lookup"><span data-stu-id="7d6cc-161">Array</span></span> | <span data-ttu-id="7d6cc-162">A tömb</span><span class="sxs-lookup"><span data-stu-id="7d6cc-162">Array</span></span>
<span data-ttu-id="7d6cc-163">NULL ÉRTÉKŰ</span><span class="sxs-lookup"><span data-stu-id="7d6cc-163">NULL</span></span> | <span data-ttu-id="7d6cc-164">NULL értékű</span><span class="sxs-lookup"><span data-stu-id="7d6cc-164">Null</span></span>


<span data-ttu-id="7d6cc-165">Az alábbiakban a JavaScript-Stream Analytics-átalakításhoz:</span><span class="sxs-lookup"><span data-stu-id="7d6cc-165">Here are JavaScript-to-Stream Analytics conversions:</span></span>


<span data-ttu-id="7d6cc-166">JavaScript</span><span class="sxs-lookup"><span data-stu-id="7d6cc-166">JavaScript</span></span> | <span data-ttu-id="7d6cc-167">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7d6cc-167">Stream Analytics</span></span>
--- | ---
<span data-ttu-id="7d6cc-168">Szám</span><span class="sxs-lookup"><span data-stu-id="7d6cc-168">Number</span></span> | <span data-ttu-id="7d6cc-169">Bigint (Ha a szám kerek és a hosszú között. A MinValue és a hosszú. MaxValue; Ellenkező esetben dupla)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-169">Bigint (if the number is round and between long.MinValue and long.MaxValue; otherwise, it's double)</span></span>
<span data-ttu-id="7d6cc-170">Dátum</span><span class="sxs-lookup"><span data-stu-id="7d6cc-170">Date</span></span> | <span data-ttu-id="7d6cc-171">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="7d6cc-171">DateTime</span></span>
<span data-ttu-id="7d6cc-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7d6cc-172">String</span></span> | <span data-ttu-id="7d6cc-173">típus: nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-173">nvarchar(MAX)</span></span>
<span data-ttu-id="7d6cc-174">Objektum</span><span class="sxs-lookup"><span data-stu-id="7d6cc-174">Object</span></span> | <span data-ttu-id="7d6cc-175">rekord</span><span class="sxs-lookup"><span data-stu-id="7d6cc-175">Record</span></span>
<span data-ttu-id="7d6cc-176">A tömb</span><span class="sxs-lookup"><span data-stu-id="7d6cc-176">Array</span></span> | <span data-ttu-id="7d6cc-177">A tömb</span><span class="sxs-lookup"><span data-stu-id="7d6cc-177">Array</span></span>
<span data-ttu-id="7d6cc-178">NULL, nem definiált</span><span class="sxs-lookup"><span data-stu-id="7d6cc-178">Null, Undefined</span></span> | <span data-ttu-id="7d6cc-179">NULL ÉRTÉKŰ</span><span class="sxs-lookup"><span data-stu-id="7d6cc-179">NULL</span></span>
<span data-ttu-id="7d6cc-180">Bármely más típusból (például egy függvény vagy hiba)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-180">Any other type (for example, a function or error)</span></span> | <span data-ttu-id="7d6cc-181">Nem támogatott (futásidejű hibát eredményez)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-181">Not supported (results in runtime error)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7d6cc-182">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="7d6cc-182">Troubleshooting</span></span>
<span data-ttu-id="7d6cc-183">A JavaScript futásidejű hibák végzetes minősülnek, és a műveletnapló keresztül illesztett.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-183">JavaScript runtime errors are considered fatal, and are surfaced through the Activity log.</span></span> <span data-ttu-id="7d6cc-184">Beolvassa a naplót, az Azure-portálon keresse meg a feladatot, és válassza ki **tevékenységnapló**.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-184">To retrieve the log, in the Azure portal, go to your job and select **Activity log**.</span></span>


## <a name="other-javascript-user-defined-function-patterns"></a><span data-ttu-id="7d6cc-185">Más JavaScript felhasználó által definiált függvény minták</span><span class="sxs-lookup"><span data-stu-id="7d6cc-185">Other JavaScript user-defined function patterns</span></span>

### <a name="write-nested-json-to-output"></a><span data-ttu-id="7d6cc-186">Beágyazott JSON kimeneti írása</span><span class="sxs-lookup"><span data-stu-id="7d6cc-186">Write nested JSON to output</span></span>
<span data-ttu-id="7d6cc-187">Ha a követési feldolgozási lépést, amely a Stream Analytics-feladatot, kimeneti használja bemeneti adatként, és a JSON formátumban van szükség, írhat a kimeneti JSON karakterláncnak.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-187">If you have a follow-up processing step that uses a Stream Analytics job output as input, and it requires a JSON format, you can write a JSON string to output.</span></span> <span data-ttu-id="7d6cc-188">A következő példa hívások a **JSON.stringify()** függvény a bemeneti minden név/érték párok csomag, majd írja őket a kimeneti egyetlen karakterlánc értéket.</span><span class="sxs-lookup"><span data-stu-id="7d6cc-188">The next example calls the **JSON.stringify()** function to pack all name/value pairs of the input, and then write them as a single string value in output.</span></span>

<span data-ttu-id="7d6cc-189">**JavaScript felhasználó által definiált függvény definíciója:**</span><span class="sxs-lookup"><span data-stu-id="7d6cc-189">**JavaScript user-defined function definition:**</span></span>

```
function main(x) {
return JSON.stringify(x);
}
```

<span data-ttu-id="7d6cc-190">**Mintalekérdezés:**</span><span class="sxs-lookup"><span data-stu-id="7d6cc-190">**Sample query:**</span></span>
```
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a><span data-ttu-id="7d6cc-191">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="7d6cc-191">Get help</span></span>
<span data-ttu-id="7d6cc-192">Segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="7d6cc-192">For additional help, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d6cc-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7d6cc-193">Next steps</span></span>
* [<span data-ttu-id="7d6cc-194">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="7d6cc-194">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="7d6cc-195">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-195">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="7d6cc-196">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="7d6cc-196">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* [<span data-ttu-id="7d6cc-197">Az Azure Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="7d6cc-197">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="7d6cc-198">Azure Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="7d6cc-198">Azure Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

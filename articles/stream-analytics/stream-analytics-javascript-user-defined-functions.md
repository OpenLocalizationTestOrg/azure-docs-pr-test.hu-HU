---
title: "aaaAzure Stream Analytics JavaScript-a felhasználó által definiált függvények |} Microsoft Docs"
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
ms.openlocfilehash: 28eeb8f6437c23989e8887687b950361fed4414c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a><span data-ttu-id="8b3a9-104">Az Azure Stream Analytics JavaScript felhasználó által definiált függvények</span><span class="sxs-lookup"><span data-stu-id="8b3a9-104">Azure Stream Analytics JavaScript user-defined functions</span></span>
<span data-ttu-id="8b3a9-105">Az Azure Stream Analytics a felhasználó által definiált függvények JavaScript nyelven írt támogatja.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-105">Azure Stream Analytics supports user-defined functions written in JavaScript.</span></span> <span data-ttu-id="8b3a9-106">A hello széles skáláját **karakterlánc**, **RegExp szolgáltatást**, **matematikai**, **tömb**, és **dátum** módszereket, amelyek JavaScript biztosít, a Stream Analytics-feladatok összetett adatátalakítást könnyebb toocreate válik.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-106">With hello rich set of **String**, **RegExp**, **Math**, **Array**, and **Date** methods that JavaScript provides, complex data transformations with Stream Analytics jobs become easier toocreate.</span></span>

## <a name="javascript-user-defined-functions"></a><span data-ttu-id="8b3a9-107">Felhasználó által definiált függvények JavaScript</span><span class="sxs-lookup"><span data-stu-id="8b3a9-107">JavaScript user-defined functions</span></span>
<span data-ttu-id="8b3a9-108">Felhasználó által definiált függvények JavaScript támogatja állapot nélküli, csak számítási skaláris függvények, amelyek nem igényelnek külső kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-108">JavaScript user-defined functions support stateless, compute-only scalar functions that do not require external connectivity.</span></span> <span data-ttu-id="8b3a9-109">hello visszatérési érték egy függvény csak a skaláris (önálló) érték lehet.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-109">hello return value of a function can only be a scalar (single) value.</span></span> <span data-ttu-id="8b3a9-110">Miután egy felhasználó által definiált függvény JavaScript tooa feladat, hello funkció használata bárhol hello a lekérdezésben, például a beépített skaláris függvényt.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-110">After you add a JavaScript user-defined function tooa job, you can use hello function anywhere in hello query, like a built-in scalar function.</span></span>

<span data-ttu-id="8b3a9-111">Az alábbiakban néhány forgatókönyv, ahol hasznosak lehetnek felhasználó által definiált függvények JavaScript:</span><span class="sxs-lookup"><span data-stu-id="8b3a9-111">Here are some scenarios where you might find JavaScript user-defined functions useful:</span></span>
* <span data-ttu-id="8b3a9-112">Elemzés és kezelésére, amelyek reguláris kifejezés funkciók, például karakterláncok **Regexp_Replace()** és **Regexp_Extract()**</span><span class="sxs-lookup"><span data-stu-id="8b3a9-112">Parsing and manipulating strings that have regular expression functions, for example, **Regexp_Replace()** and **Regexp_Extract()**</span></span>
* <span data-ttu-id="8b3a9-113">Dekódolás és adatok, például bináris hexadecimális átalakítás kódolás</span><span class="sxs-lookup"><span data-stu-id="8b3a9-113">Decoding and encoding data, for example, binary-to-hex conversion</span></span>
* <span data-ttu-id="8b3a9-114">JavaScript mathematic számításokat végez **matematikai** funkciók</span><span class="sxs-lookup"><span data-stu-id="8b3a9-114">Performing mathematic computations with JavaScript **Math** functions</span></span>
* <span data-ttu-id="8b3a9-115">Például a rendezési, a csatlakozást, a Keresés és a fill tömb műveletek végrehajtása</span><span class="sxs-lookup"><span data-stu-id="8b3a9-115">Performing array operations like sort, join, find, and fill</span></span>

<span data-ttu-id="8b3a9-116">A következőkben, amely a Stream Analytics egy JavaScript felhasználó által definiált függvény nem hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="8b3a9-116">Here are some things that you cannot do with a JavaScript user-defined function in Stream Analytics:</span></span>
* <span data-ttu-id="8b3a9-117">Kimenő külső REST-végpontok, például végrehajtása hívás fordított IP keresési vagy referencia történő adatlekérést külső forrásból</span><span class="sxs-lookup"><span data-stu-id="8b3a9-117">Call out external REST endpoints, for example, performing reverse IP lookup or pulling reference data from an external source</span></span>
* <span data-ttu-id="8b3a9-118">Hajtsa végre az egyéni esemény szerializálás vagy a bemenetek/kimenetek deszerializálási</span><span class="sxs-lookup"><span data-stu-id="8b3a9-118">Perform custom event format serialization or deserialization on inputs/outputs</span></span>
* <span data-ttu-id="8b3a9-119">Egyéni összesítések létrehozása</span><span class="sxs-lookup"><span data-stu-id="8b3a9-119">Create custom aggregates</span></span>

<span data-ttu-id="8b3a9-120">Bár a Funkciók, például **Date.GetDate()** vagy **Math.random()** nem tiltanak hello funkciók meghatározása, az használatuk kerülendő.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-120">Although functions like **Date.GetDate()** or **Math.random()** are not blocked in hello functions definition, you should avoid using them.</span></span> <span data-ttu-id="8b3a9-121">Ezek a funkciók **nem** visszatérési hello azonos eredményeképpen minden alkalommal, amikor keresheti őket, és hello Azure Stream Analytics szolgáltatás nem őrzi meg a napló, a függvény meghívásához, és adott vissza eredményt.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-121">These functions **do not** return hello same result every time you call them, and hello Azure Stream Analytics service does not keep a journal of function invocations and returned results.</span></span> <span data-ttu-id="8b3a9-122">Ha egy olyan függvényt ad vissza különböző eredménye a hello azonos események, ismételhetőség nem garantált a feladat újraindításakor, illetve hello Stream Analytics szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-122">If a function returns different result on hello same events, repeatability is not guaranteed when a job is restarted by you or by hello Stream Analytics service.</span></span>

## <a name="add-a-javascript-user-defined-function-in-hello-azure-portal"></a><span data-ttu-id="8b3a9-123">Adja hozzá a JavaScript felhasználó által definiált függvény hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8b3a9-123">Add a JavaScript user-defined function in hello Azure portal</span></span>
<span data-ttu-id="8b3a9-124">egy egyszerű JavaScript felhasználó által definiált függvény alapján egy meglévő Stream Analytics-feladat toocreate hajtsa végre ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8b3a9-124">toocreate a simple JavaScript user-defined function under an existing Stream Analytics job, do these steps:</span></span>

1.  <span data-ttu-id="8b3a9-125">Hello Azure-portálon keresse meg a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-125">In hello Azure portal, find your Stream Analytics job.</span></span>
2.  <span data-ttu-id="8b3a9-126">A **feladat TOPOLÓGIA**, válassza ki a függvényt.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-126">Under **JOB TOPOLOGY**, select your function.</span></span> <span data-ttu-id="8b3a9-127">Megjelenik a funkciók listája üres.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-127">An empty list of functions appears.</span></span>
3.  <span data-ttu-id="8b3a9-128">Válasszon egy új felhasználó által definiált függvény toocreate **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-128">toocreate a new user-defined function, select **Add**.</span></span>
4.  <span data-ttu-id="8b3a9-129">A hello **új függvény** panelen a **függvénytípus**, jelölje be **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-129">On hello **New Function** blade, for **Function Type**, select **JavaScript**.</span></span> <span data-ttu-id="8b3a9-130">Alapértelmezett függvény sablon hello szerkesztő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-130">A default function template appears in hello editor.</span></span>
5.  <span data-ttu-id="8b3a9-131">A hello **UDF alias**, adja meg **hex2Int**, és módosítsa az alábbiak szerint hello függvény végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="8b3a9-131">For hello **UDF alias**, enter **hex2Int**, and change hello function implementation as follows:</span></span>

    ```
    // Convert Hex value toointeger.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  <span data-ttu-id="8b3a9-132">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-132">Select **Save**.</span></span> <span data-ttu-id="8b3a9-133">A függvény funkciók hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-133">Your function appears in hello list of functions.</span></span>
7.  <span data-ttu-id="8b3a9-134">Jelölje be új hello **hex2Int** működik, és ellenőrizze hello függvény definícióját.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-134">Select hello new **hex2Int** function, and check hello function definition.</span></span> <span data-ttu-id="8b3a9-135">Minden függvényeknek a **UDF** előtag hozzáadott toohello függvény aliasa.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-135">All functions have a **UDF** prefix added toohello function alias.</span></span> <span data-ttu-id="8b3a9-136">Túl kell*hello előtagot tartalmaz* hello függvény hívható a Stream Analytics-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-136">You need too*include hello prefix* when you call hello function in your Stream Analytics query.</span></span> <span data-ttu-id="8b3a9-137">Ebben az esetben hívható **UDF.hex2Int**.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-137">In this case, you call **UDF.hex2Int**.</span></span>

## <a name="call-a-javascript-user-defined-function-in-a-query"></a><span data-ttu-id="8b3a9-138">A JavaScript felhasználói függvényt hívni a lekérdezésben</span><span class="sxs-lookup"><span data-stu-id="8b3a9-138">Call a JavaScript user-defined function in a query</span></span>

1. <span data-ttu-id="8b3a9-139">Hello lekérdezésben-szerkesztőben a **feladat TOPOLÓGIA**, jelölje be **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-139">In hello query editor, under **JOB TOPOLOGY**, select **Query**.</span></span>
2.  <span data-ttu-id="8b3a9-140">Szerkessze a lekérdezést, és majd hello felhasználói függvényt hívni, ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="8b3a9-140">Edit your query, and then call hello user-defined function, like this:</span></span>

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  <span data-ttu-id="8b3a9-141">tooupload hello mintaadatfájlokat, kattintson a jobb gombbal hello feladat bemenet.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-141">tooupload hello sample data file, right-click hello job input.</span></span>
4.  <span data-ttu-id="8b3a9-142">tootest a select lekérdezés **teszt**.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-142">tootest your query, select **Test**.</span></span>


## <a name="supported-javascript-objects"></a><span data-ttu-id="8b3a9-143">Támogatott JavaScript-objektumok</span><span class="sxs-lookup"><span data-stu-id="8b3a9-143">Supported JavaScript objects</span></span>
<span data-ttu-id="8b3a9-144">Az Azure Stream Analytics JavaScript felhasználó által definiált függvények standard szintű, beépített JavaScript-objektumok támogatja.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-144">Azure Stream Analytics JavaScript user-defined functions support standard, built-in JavaScript objects.</span></span> <span data-ttu-id="8b3a9-145">Ezek az objektumok listáját lásd: [globális objektumok](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span><span class="sxs-lookup"><span data-stu-id="8b3a9-145">For a list of these objects, see [Global Objects](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span></span>

### <a name="stream-analytics-and-javascript-type-conversion"></a><span data-ttu-id="8b3a9-146">A Stream Analytics és a JavaScript adattípus átalakítása</span><span class="sxs-lookup"><span data-stu-id="8b3a9-146">Stream Analytics and JavaScript type conversion</span></span>

<span data-ttu-id="8b3a9-147">Hello típusokat támogató hello Stream Analytics lekérdezési nyelv és a JavaScript különbségek vannak.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-147">There are differences in hello types that hello Stream Analytics query language and JavaScript support.</span></span> <span data-ttu-id="8b3a9-148">Ez a táblázat közötti két hello hello átalakítás leképezéseket:</span><span class="sxs-lookup"><span data-stu-id="8b3a9-148">This table lists hello conversion mappings between hello two:</span></span>

<span data-ttu-id="8b3a9-149">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8b3a9-149">Stream Analytics</span></span> | <span data-ttu-id="8b3a9-150">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8b3a9-150">JavaScript</span></span>
--- | ---
<span data-ttu-id="8b3a9-151">bigint</span><span class="sxs-lookup"><span data-stu-id="8b3a9-151">bigint</span></span> | <span data-ttu-id="8b3a9-152">Szám (JavaScript csak jelenthet egész számok felfelé tooprecisely 2 ^ 53)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-152">Number (JavaScript can only represent integers up tooprecisely 2^53)</span></span>
<span data-ttu-id="8b3a9-153">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b3a9-153">DateTime</span></span> | <span data-ttu-id="8b3a9-154">Dátum (JavaScript csak támogatja ezredmásodperc)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-154">Date (JavaScript only supports milliseconds)</span></span>
<span data-ttu-id="8b3a9-155">Dupla</span><span class="sxs-lookup"><span data-stu-id="8b3a9-155">double</span></span> | <span data-ttu-id="8b3a9-156">Szám</span><span class="sxs-lookup"><span data-stu-id="8b3a9-156">Number</span></span>
<span data-ttu-id="8b3a9-157">típus: nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-157">nvarchar(MAX)</span></span> | <span data-ttu-id="8b3a9-158">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8b3a9-158">String</span></span>
<span data-ttu-id="8b3a9-159">rekord</span><span class="sxs-lookup"><span data-stu-id="8b3a9-159">Record</span></span> | <span data-ttu-id="8b3a9-160">Objektum</span><span class="sxs-lookup"><span data-stu-id="8b3a9-160">Object</span></span>
<span data-ttu-id="8b3a9-161">Tömb</span><span class="sxs-lookup"><span data-stu-id="8b3a9-161">Array</span></span> | <span data-ttu-id="8b3a9-162">Tömb</span><span class="sxs-lookup"><span data-stu-id="8b3a9-162">Array</span></span>
<span data-ttu-id="8b3a9-163">NULL ÉRTÉKŰ</span><span class="sxs-lookup"><span data-stu-id="8b3a9-163">NULL</span></span> | <span data-ttu-id="8b3a9-164">NULL értékű</span><span class="sxs-lookup"><span data-stu-id="8b3a9-164">Null</span></span>


<span data-ttu-id="8b3a9-165">Az alábbiakban a JavaScript-Stream Analytics-átalakításhoz:</span><span class="sxs-lookup"><span data-stu-id="8b3a9-165">Here are JavaScript-to-Stream Analytics conversions:</span></span>


<span data-ttu-id="8b3a9-166">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8b3a9-166">JavaScript</span></span> | <span data-ttu-id="8b3a9-167">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8b3a9-167">Stream Analytics</span></span>
--- | ---
<span data-ttu-id="8b3a9-168">Szám</span><span class="sxs-lookup"><span data-stu-id="8b3a9-168">Number</span></span> | <span data-ttu-id="8b3a9-169">Bigint (ha hello szám kerek és a hosszú között. A MinValue és a hosszú. MaxValue; Ellenkező esetben dupla)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-169">Bigint (if hello number is round and between long.MinValue and long.MaxValue; otherwise, it's double)</span></span>
<span data-ttu-id="8b3a9-170">Dátum</span><span class="sxs-lookup"><span data-stu-id="8b3a9-170">Date</span></span> | <span data-ttu-id="8b3a9-171">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="8b3a9-171">DateTime</span></span>
<span data-ttu-id="8b3a9-172">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="8b3a9-172">String</span></span> | <span data-ttu-id="8b3a9-173">típus: nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-173">nvarchar(MAX)</span></span>
<span data-ttu-id="8b3a9-174">Objektum</span><span class="sxs-lookup"><span data-stu-id="8b3a9-174">Object</span></span> | <span data-ttu-id="8b3a9-175">rekord</span><span class="sxs-lookup"><span data-stu-id="8b3a9-175">Record</span></span>
<span data-ttu-id="8b3a9-176">Tömb</span><span class="sxs-lookup"><span data-stu-id="8b3a9-176">Array</span></span> | <span data-ttu-id="8b3a9-177">Tömb</span><span class="sxs-lookup"><span data-stu-id="8b3a9-177">Array</span></span>
<span data-ttu-id="8b3a9-178">NULL, nem definiált</span><span class="sxs-lookup"><span data-stu-id="8b3a9-178">Null, Undefined</span></span> | <span data-ttu-id="8b3a9-179">NULL ÉRTÉKŰ</span><span class="sxs-lookup"><span data-stu-id="8b3a9-179">NULL</span></span>
<span data-ttu-id="8b3a9-180">Bármely más típusból (például egy függvény vagy hiba)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-180">Any other type (for example, a function or error)</span></span> | <span data-ttu-id="8b3a9-181">Nem támogatott (futásidejű hibát eredményez)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-181">Not supported (results in runtime error)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="8b3a9-182">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="8b3a9-182">Troubleshooting</span></span>
<span data-ttu-id="8b3a9-183">A JavaScript futásidejű hibák végzetes minősülnek, és keresztül hello tevékenységnapló illesztett.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-183">JavaScript runtime errors are considered fatal, and are surfaced through hello Activity log.</span></span> <span data-ttu-id="8b3a9-184">tooretrieve hello napló hello Azure-portálon, nyissa meg tooyour feladatot, és válasszon **tevékenységnapló**.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-184">tooretrieve hello log, in hello Azure portal, go tooyour job and select **Activity log**.</span></span>


## <a name="other-javascript-user-defined-function-patterns"></a><span data-ttu-id="8b3a9-185">Más JavaScript felhasználó által definiált függvény minták</span><span class="sxs-lookup"><span data-stu-id="8b3a9-185">Other JavaScript user-defined function patterns</span></span>

### <a name="write-nested-json-toooutput"></a><span data-ttu-id="8b3a9-186">Beágyazott JSON toooutput írása</span><span class="sxs-lookup"><span data-stu-id="8b3a9-186">Write nested JSON toooutput</span></span>
<span data-ttu-id="8b3a9-187">Ha a követési feldolgozási lépést, amely a Stream Analytics-feladatot, kimeneti használja bemeneti adatként, és a JSON formátumban van szükség, egy JSON-karakterlánc toooutput lehet írni.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-187">If you have a follow-up processing step that uses a Stream Analytics job output as input, and it requires a JSON format, you can write a JSON string toooutput.</span></span> <span data-ttu-id="8b3a9-188">hello a következő példában meghívja a hello **JSON.stringify()** toopack hello minden név-érték pár adjon meg, és ezután írja őket a kimeneti egyetlen karakterlánc értéket működik.</span><span class="sxs-lookup"><span data-stu-id="8b3a9-188">hello next example calls hello **JSON.stringify()** function toopack all name/value pairs of hello input, and then write them as a single string value in output.</span></span>

<span data-ttu-id="8b3a9-189">**JavaScript felhasználó által definiált függvény definíciója:**</span><span class="sxs-lookup"><span data-stu-id="8b3a9-189">**JavaScript user-defined function definition:**</span></span>

```
function main(x) {
return JSON.stringify(x);
}
```

<span data-ttu-id="8b3a9-190">**Mintalekérdezés:**</span><span class="sxs-lookup"><span data-stu-id="8b3a9-190">**Sample query:**</span></span>
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

## <a name="get-help"></a><span data-ttu-id="8b3a9-191">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="8b3a9-191">Get help</span></span>
<span data-ttu-id="8b3a9-192">Segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="8b3a9-192">For additional help, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b3a9-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8b3a9-193">Next steps</span></span>
* [<span data-ttu-id="8b3a9-194">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="8b3a9-194">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="8b3a9-195">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-195">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="8b3a9-196">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="8b3a9-196">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* [<span data-ttu-id="8b3a9-197">Az Azure Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="8b3a9-197">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8b3a9-198">Azure Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="8b3a9-198">Azure Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

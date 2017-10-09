---
title: "a Stream Analytics gyakori használati minták aaaQuery példák |} Microsoft Docs"
description: "Gyakori Azure Stream Analytics lekérdezési minták"
keywords: "lekérdezés példák"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jenniehubbard
editor: cgronlun
ms.assetid: 6b9a7d00-fbcc-42f6-9cbb-8bbf0bbd3d0e
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/08/2017
ms.author: jenniehubbard
ms.openlocfilehash: c8f7a8ac661eaf0281f4140b02c42141b73040fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a><span data-ttu-id="8f630-104">Példa a gyakori Stream Analytics használati minták lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8f630-104">Query examples for common Stream Analytics usage patterns</span></span>
## <a name="introduction"></a><span data-ttu-id="8f630-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="8f630-105">Introduction</span></span>
<span data-ttu-id="8f630-106">Lekérdezések Azure Stream Analytics lekérdezési SQL-szerű nyelven van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="8f630-106">Queries in Azure Stream Analytics are expressed in a SQL-like query language.</span></span> <span data-ttu-id="8f630-107">Ezeket a lekérdezéseket a rendszer részletes ismertetését lásd: hello [Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx) útmutató.</span><span class="sxs-lookup"><span data-stu-id="8f630-107">These queries are documented in hello [Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) guide.</span></span> <span data-ttu-id="8f630-108">Ez a cikk vázol fel megoldások tooseveral gyakori lekérdezési minták, alapján valós forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="8f630-108">This article outlines solutions tooseveral common query patterns, based on real-world scenarios.</span></span> <span data-ttu-id="8f630-109">Folyamatban lévő és továbbra is az új mintákat folyamatosan frissített toobe.</span><span class="sxs-lookup"><span data-stu-id="8f630-109">It is a work in progress and continues toobe updated with new patterns on an ongoing basis.</span></span>

## <a name="query-example-convert-data-types"></a><span data-ttu-id="8f630-110">Példa: adattípusok átalakítása</span><span class="sxs-lookup"><span data-stu-id="8f630-110">Query example: Convert data types</span></span>
<span data-ttu-id="8f630-111">**Leírás**: hello bemeneti adatfolyam vonatkozó hello típusú tulajdonságok meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="8f630-111">**Description**: Define hello types of properties on hello input stream.</span></span>
<span data-ttu-id="8f630-112">Például hello car karakterláncként. a bemeneti adatfolyam hello hamarosan és kell konvertálni túl toobe**INT** tooperform **SUM** azt be.</span><span class="sxs-lookup"><span data-stu-id="8f630-112">For example, hello car weight is coming on hello input stream as strings and needs toobe converted too**INT** tooperform **SUM** it up.</span></span>

<span data-ttu-id="8f630-113">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-113">**Input**:</span></span>

| <span data-ttu-id="8f630-114">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-114">Make</span></span> | <span data-ttu-id="8f630-115">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-115">Time</span></span> | <span data-ttu-id="8f630-116">Súlyozás</span><span class="sxs-lookup"><span data-stu-id="8f630-116">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-117">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-117">Honda</span></span> |<span data-ttu-id="8f630-118">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-118">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="8f630-119">"1000"</span><span class="sxs-lookup"><span data-stu-id="8f630-119">"1000"</span></span> |
| <span data-ttu-id="8f630-120">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-120">Honda</span></span> |<span data-ttu-id="8f630-121">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-121">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="8f630-122">"2000"</span><span class="sxs-lookup"><span data-stu-id="8f630-122">"2000"</span></span> |

<span data-ttu-id="8f630-123">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-123">**Output**:</span></span>

| <span data-ttu-id="8f630-124">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-124">Make</span></span> | <span data-ttu-id="8f630-125">Súlyozás</span><span class="sxs-lookup"><span data-stu-id="8f630-125">Weight</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-126">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-126">Honda</span></span> |<span data-ttu-id="8f630-127">3000</span><span class="sxs-lookup"><span data-stu-id="8f630-127">3000</span></span> |

<span data-ttu-id="8f630-128">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-128">**Solution**:</span></span>

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

<span data-ttu-id="8f630-129">**MAGYARÁZAT**: használja a **TÍPUSKONVERZIÓ** hello utasításában **súly** mezőben toospecify az adattípushoz.</span><span class="sxs-lookup"><span data-stu-id="8f630-129">**Explanation**: Use a **CAST** statement in hello **Weight** field toospecify its data type.</span></span> <span data-ttu-id="8f630-130">A támogatott adattípusokat hello tartalmazó lista [adattípusok (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span><span class="sxs-lookup"><span data-stu-id="8f630-130">See hello list of supported data types in [Data types (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span></span>

## <a name="query-example-use-likenot-like-toodo-pattern-matching"></a><span data-ttu-id="8f630-131">Példa: hasonló/nem például toodo karakterek használata</span><span class="sxs-lookup"><span data-stu-id="8f630-131">Query example: Use Like/Not like toodo pattern matching</span></span>
<span data-ttu-id="8f630-132">**Leírás**: Ellenőrizze, hogy egy mező értéke hello esemény megfelel-e egy bizonyos minta.</span><span class="sxs-lookup"><span data-stu-id="8f630-132">**Description**: Check that a field value on hello event matches a certain pattern.</span></span>
<span data-ttu-id="8f630-133">Ellenőrizze például, hogy hello eredményt adja vissza, amely az A kezdődhet és végződhet 9 licenc lemezeket.</span><span class="sxs-lookup"><span data-stu-id="8f630-133">For example, check that hello result returns license plates that start with A and end with 9.</span></span>

<span data-ttu-id="8f630-134">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-134">**Input**:</span></span>

| <span data-ttu-id="8f630-135">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-135">Make</span></span> | <span data-ttu-id="8f630-136">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-136">LicensePlate</span></span> | <span data-ttu-id="8f630-137">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-137">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-138">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-138">Honda</span></span> |<span data-ttu-id="8f630-139">ABC – 123</span><span class="sxs-lookup"><span data-stu-id="8f630-139">ABC-123</span></span> |<span data-ttu-id="8f630-140">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-140">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-141">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-141">Toyota</span></span> |<span data-ttu-id="8f630-142">AAA-999</span><span class="sxs-lookup"><span data-stu-id="8f630-142">AAA-999</span></span> |<span data-ttu-id="8f630-143">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-143">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-144">Nissan</span><span class="sxs-lookup"><span data-stu-id="8f630-144">Nissan</span></span> |<span data-ttu-id="8f630-145">ABC-369</span><span class="sxs-lookup"><span data-stu-id="8f630-145">ABC-369</span></span> |<span data-ttu-id="8f630-146">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-146">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="8f630-147">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-147">**Output**:</span></span>

| <span data-ttu-id="8f630-148">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-148">Make</span></span> | <span data-ttu-id="8f630-149">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-149">LicensePlate</span></span> | <span data-ttu-id="8f630-150">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-150">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-151">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-151">Toyota</span></span> |<span data-ttu-id="8f630-152">AAA-999</span><span class="sxs-lookup"><span data-stu-id="8f630-152">AAA-999</span></span> |<span data-ttu-id="8f630-153">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-153">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-154">Nissan</span><span class="sxs-lookup"><span data-stu-id="8f630-154">Nissan</span></span> |<span data-ttu-id="8f630-155">ABC-369</span><span class="sxs-lookup"><span data-stu-id="8f630-155">ABC-369</span></span> |<span data-ttu-id="8f630-156">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-156">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="8f630-157">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-157">**Solution**:</span></span>

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

<span data-ttu-id="8f630-158">**MAGYARÁZAT**: használata hello **PÉLDÁUL** utasítás toocheck hello **LicensePlate** mező érték.</span><span class="sxs-lookup"><span data-stu-id="8f630-158">**Explanation**: Use hello **LIKE** statement toocheck hello **LicensePlate** field value.</span></span> <span data-ttu-id="8f630-159">Emellett kell egy A, indítsa el, majd tetszőleges karakterlánc nulla vagy több rendelkezik, és majd végződhet a 9.</span><span class="sxs-lookup"><span data-stu-id="8f630-159">It should start with an A, then have any string of zero or more characters, and then end with a 9.</span></span> 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a><span data-ttu-id="8f630-160">Példa: megadása logikát a különböző esetekre és-értékek ("CASE" utasítás)</span><span class="sxs-lookup"><span data-stu-id="8f630-160">Query example: Specify logic for different cases/values (CASE statements)</span></span>
<span data-ttu-id="8f630-161">**Leírás**: Adjon meg egy másik számítási mező, egy adott feltétel alapján.</span><span class="sxs-lookup"><span data-stu-id="8f630-161">**Description**: Provide a different computation for a field, based on a particular criterion.</span></span>
<span data-ttu-id="8f630-162">Például adjon meg egy karakterlánc leírást hello megegyezik, hogy hány autók átadni, egy különleges esetben az 1.</span><span class="sxs-lookup"><span data-stu-id="8f630-162">For example, provide a string description for how many cars of hello same make passed, with a special case for 1.</span></span>

<span data-ttu-id="8f630-163">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-163">**Input**:</span></span>

| <span data-ttu-id="8f630-164">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-164">Make</span></span> | <span data-ttu-id="8f630-165">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-165">Time</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-166">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-166">Honda</span></span> |<span data-ttu-id="8f630-167">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-167">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-168">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-168">Toyota</span></span> |<span data-ttu-id="8f630-169">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-169">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-170">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-170">Toyota</span></span> |<span data-ttu-id="8f630-171">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-171">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="8f630-172">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-172">**Output**:</span></span>

| <span data-ttu-id="8f630-173">CarsPassed</span><span class="sxs-lookup"><span data-stu-id="8f630-173">CarsPassed</span></span> | <span data-ttu-id="8f630-174">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-174">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-175">1 Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-175">1 Honda</span></span> |<span data-ttu-id="8f630-176">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-176">2015-01-01T00:00:10.0000000Z</span></span> |
| <span data-ttu-id="8f630-177">2 Toyotas</span><span class="sxs-lookup"><span data-stu-id="8f630-177">2 Toyotas</span></span> |<span data-ttu-id="8f630-178">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-178">2015-01-01T00:00:10.0000000Z</span></span> |

<span data-ttu-id="8f630-179">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-179">**Solution**:</span></span>

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

<span data-ttu-id="8f630-180">**MAGYARÁZAT**: hello **eset** záradék lehetővé teszi a különböző számítási tooprovide bizonyos feltételeknek megfelelő (ebben az esetben hello hello összesített ablakban hello autók száma).</span><span class="sxs-lookup"><span data-stu-id="8f630-180">**Explanation**: hello **CASE** clause allows us tooprovide a different computation, based on some criteria (in our case, hello count of hello cars in hello aggregate window).</span></span>

## <a name="query-example-send-data-toomultiple-outputs"></a><span data-ttu-id="8f630-181">Példa: adatok toomultiple kimenete küldése</span><span class="sxs-lookup"><span data-stu-id="8f630-181">Query example: Send data toomultiple outputs</span></span>
<span data-ttu-id="8f630-182">**Leírás**: Send data toomultiple kimeneti célok egyetlen feladat.</span><span class="sxs-lookup"><span data-stu-id="8f630-182">**Description**: Send data toomultiple output targets from a single job.</span></span>
<span data-ttu-id="8f630-183">Például egy küszöbérték-alapú riasztás adatok elemzése, és minden események tooblob archivált.</span><span class="sxs-lookup"><span data-stu-id="8f630-183">For example, analyze data for a threshold-based alert and archive all events tooblob storage.</span></span>

<span data-ttu-id="8f630-184">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-184">**Input**:</span></span>

| <span data-ttu-id="8f630-185">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-185">Make</span></span> | <span data-ttu-id="8f630-186">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-186">Time</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-187">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-187">Honda</span></span> |<span data-ttu-id="8f630-188">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-188">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-189">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-189">Honda</span></span> |<span data-ttu-id="8f630-190">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-190">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-191">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-191">Toyota</span></span> |<span data-ttu-id="8f630-192">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-192">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-193">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-193">Toyota</span></span> |<span data-ttu-id="8f630-194">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-194">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-195">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-195">Toyota</span></span> |<span data-ttu-id="8f630-196">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-196">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="8f630-197">**Output1**:</span><span class="sxs-lookup"><span data-stu-id="8f630-197">**Output1**:</span></span>

| <span data-ttu-id="8f630-198">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-198">Make</span></span> | <span data-ttu-id="8f630-199">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-199">Time</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-200">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-200">Honda</span></span> |<span data-ttu-id="8f630-201">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-201">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-202">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-202">Honda</span></span> |<span data-ttu-id="8f630-203">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-203">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-204">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-204">Toyota</span></span> |<span data-ttu-id="8f630-205">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-205">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-206">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-206">Toyota</span></span> |<span data-ttu-id="8f630-207">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-207">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-208">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-208">Toyota</span></span> |<span data-ttu-id="8f630-209">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-209">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="8f630-210">**Output2**:</span><span class="sxs-lookup"><span data-stu-id="8f630-210">**Output2**:</span></span>

| <span data-ttu-id="8f630-211">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-211">Make</span></span> | <span data-ttu-id="8f630-212">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-212">Time</span></span> | <span data-ttu-id="8f630-213">Darabszám</span><span class="sxs-lookup"><span data-stu-id="8f630-213">Count</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-214">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-214">Toyota</span></span> |<span data-ttu-id="8f630-215">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-215">2015-01-01T00:00:10.0000000Z</span></span> |<span data-ttu-id="8f630-216">3</span><span class="sxs-lookup"><span data-stu-id="8f630-216">3</span></span> |

<span data-ttu-id="8f630-217">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-217">**Solution**:</span></span>

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

<span data-ttu-id="8f630-218">**MAGYARÁZAT**: hello **INTO** záradék közli a Stream Analytics, amely hello toowrite hello adatok toofrom kiírja a jelen nyilatkozat.</span><span class="sxs-lookup"><span data-stu-id="8f630-218">**Explanation**: hello **INTO** clause tells Stream Analytics which of hello outputs toowrite hello data toofrom this statement.</span></span>
<span data-ttu-id="8f630-219">hello első lekérdezés, hogy mi tooan kimeneti érkezett hello adatok csatlakoztatott **ArchiveOutput**.</span><span class="sxs-lookup"><span data-stu-id="8f630-219">hello first query is a pass-through of hello data we received tooan output that we named **ArchiveOutput**.</span></span>
<span data-ttu-id="8f630-220">hello második lekérdezés nem néhány egyszerű összesítési és szűrés, és akkor küldi hello eredmények tooa alárendelt riasztási rendszer.</span><span class="sxs-lookup"><span data-stu-id="8f630-220">hello second query does some simple aggregation and filtering, and it sends hello results tooa downstream alerting system.</span></span>

<span data-ttu-id="8f630-221">Vegye figyelembe, hogy hello eredményeit hello közös táblakifejezésekben (Táblakifejezés) is felhasználhatja (például **WITH** utasítások) több kimeneti utasításokban.</span><span class="sxs-lookup"><span data-stu-id="8f630-221">Note that you can also reuse hello results of hello common table expressions (CTEs) (such as **WITH** statements) in multiple output statements.</span></span> <span data-ttu-id="8f630-222">Ez a beállítás rendelkezik hello hozzáadott előnye, hogy a bemeneti forrás kevesebb olvasók toohello megnyitása.</span><span class="sxs-lookup"><span data-stu-id="8f630-222">This option has hello added benefit of opening fewer readers toohello input source.</span></span>
<span data-ttu-id="8f630-223">Példa:</span><span class="sxs-lookup"><span data-stu-id="8f630-223">For example:</span></span> 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a><span data-ttu-id="8f630-224">Példa: egyedi értékek száma</span><span class="sxs-lookup"><span data-stu-id="8f630-224">Query example: Count unique values</span></span>
<span data-ttu-id="8f630-225">**Leírás**: hello adatfolyamot egy olyan időkeretet belül egyedi mező értékeinek hello számát.</span><span class="sxs-lookup"><span data-stu-id="8f630-225">**Description**: Count hello number of unique field values that appear in hello stream within a time window.</span></span>
<span data-ttu-id="8f630-226">Például hogy hány egyedi lesz továbbítja a 2-második ablakban hello téren kiállítási autók?</span><span class="sxs-lookup"><span data-stu-id="8f630-226">For example, how many unique makes of cars passed through hello toll booth in a 2-second window?</span></span>

<span data-ttu-id="8f630-227">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-227">**Input**:</span></span>

| <span data-ttu-id="8f630-228">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-228">Make</span></span> | <span data-ttu-id="8f630-229">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-229">Time</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-230">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-230">Honda</span></span> |<span data-ttu-id="8f630-231">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-231">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-232">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-232">Honda</span></span> |<span data-ttu-id="8f630-233">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-233">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-234">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-234">Toyota</span></span> |<span data-ttu-id="8f630-235">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-235">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-236">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-236">Toyota</span></span> |<span data-ttu-id="8f630-237">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-237">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-238">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-238">Toyota</span></span> |<span data-ttu-id="8f630-239">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-239">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="8f630-240">**A kimenetre:**</span><span class="sxs-lookup"><span data-stu-id="8f630-240">**Output:**</span></span>

| <span data-ttu-id="8f630-241">Darabszám</span><span class="sxs-lookup"><span data-stu-id="8f630-241">Count</span></span> | <span data-ttu-id="8f630-242">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-242">Time</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-243">2</span><span class="sxs-lookup"><span data-stu-id="8f630-243">2</span></span> |<span data-ttu-id="8f630-244">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-244">2015-01-01T00:00:02.000Z</span></span> |
| <span data-ttu-id="8f630-245">1</span><span class="sxs-lookup"><span data-stu-id="8f630-245">1</span></span> |<span data-ttu-id="8f630-246">2015-01-01T00:00:04.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-246">2015-01-01T00:00:04.000Z</span></span> |

<span data-ttu-id="8f630-247">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="8f630-247">**Solution:**</span></span>

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


<span data-ttu-id="8f630-248">**Magyarázat:**
**száma (különböző ellenőrizze)** értéket ad vissza a hello különböző értékek számának hello **győződjön** belül egy olyan időkeretet oszlop.</span><span class="sxs-lookup"><span data-stu-id="8f630-248">**Explanation:**
**COUNT(DISTINCT Make)** returns hello number of distinct values in hello **Make** column within a time window.</span></span>

## <a name="query-example-determine-if-a-value-has-changed"></a><span data-ttu-id="8f630-249">Példa: határozza meg, hogy módosult-e egy értéket</span><span class="sxs-lookup"><span data-stu-id="8f630-249">Query example: Determine if a value has changed</span></span>
<span data-ttu-id="8f630-250">**Leírás**: nézze meg a korábbi érték toodetermine Ha hello aktuális értéke eltér.</span><span class="sxs-lookup"><span data-stu-id="8f630-250">**Description**: Look at a previous value toodetermine if it is different than hello current value.</span></span>
<span data-ttu-id="8f630-251">Például van hello előző car hello téren közúti hello azonos tehetjük hello aktuális autó?</span><span class="sxs-lookup"><span data-stu-id="8f630-251">For example, is hello previous car on hello toll road hello same make as hello current car?</span></span>

<span data-ttu-id="8f630-252">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-252">**Input**:</span></span>

| <span data-ttu-id="8f630-253">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-253">Make</span></span> | <span data-ttu-id="8f630-254">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-254">Time</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-255">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-255">Honda</span></span> |<span data-ttu-id="8f630-256">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-256">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-257">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-257">Toyota</span></span> |<span data-ttu-id="8f630-258">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-258">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="8f630-259">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-259">**Output**:</span></span>

| <span data-ttu-id="8f630-260">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-260">Make</span></span> | <span data-ttu-id="8f630-261">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-261">Time</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-262">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-262">Toyota</span></span> |<span data-ttu-id="8f630-263">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-263">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="8f630-264">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-264">**Solution**:</span></span>

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

<span data-ttu-id="8f630-265">**MAGYARÁZAT**: használata **LAG** toopeek hello a bemeneti adatfolyam vissza egy eseményt, és hello beolvasása **ellenőrizze** érték.</span><span class="sxs-lookup"><span data-stu-id="8f630-265">**Explanation**: Use **LAG** toopeek into hello input stream one event back and get hello **Make** value.</span></span> <span data-ttu-id="8f630-266">Hasonlítsa össze toohello **ellenőrizze** hello aktuális és a kimeneti hello esemény az értéket, ha ezek eltérnek.</span><span class="sxs-lookup"><span data-stu-id="8f630-266">Then compare it toohello **Make** value on hello current event and output hello event if they are different.</span></span>

## <a name="query-example-find-hello-first-event-in-a-window"></a><span data-ttu-id="8f630-267">Példa: keresés hello első esemény ablakban</span><span class="sxs-lookup"><span data-stu-id="8f630-267">Query example: Find hello first event in a window</span></span>
<span data-ttu-id="8f630-268">**Leírás**: keresés hello első autója minden 10 perces időszakban.</span><span class="sxs-lookup"><span data-stu-id="8f630-268">**Description**: Find hello first car in every 10-minute interval.</span></span>

<span data-ttu-id="8f630-269">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-269">**Input**:</span></span>

| <span data-ttu-id="8f630-270">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-270">LicensePlate</span></span> | <span data-ttu-id="8f630-271">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-271">Make</span></span> | <span data-ttu-id="8f630-272">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-272">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-273">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="8f630-273">DXE 5291</span></span> |<span data-ttu-id="8f630-274">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-274">Honda</span></span> |<span data-ttu-id="8f630-275">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-275">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="8f630-276">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="8f630-276">YZK 5704</span></span> |<span data-ttu-id="8f630-277">Ford</span><span class="sxs-lookup"><span data-stu-id="8f630-277">Ford</span></span> |<span data-ttu-id="8f630-278">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-278">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="8f630-279">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="8f630-279">RMV 8282</span></span> |<span data-ttu-id="8f630-280">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-280">Honda</span></span> |<span data-ttu-id="8f630-281">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-281">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-282">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="8f630-282">YHN 6970</span></span> |<span data-ttu-id="8f630-283">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-283">Toyota</span></span> |<span data-ttu-id="8f630-284">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-284">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="8f630-285">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="8f630-285">VFE 1616</span></span> |<span data-ttu-id="8f630-286">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-286">Toyota</span></span> |<span data-ttu-id="8f630-287">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-287">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="8f630-288">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="8f630-288">QYF 9358</span></span> |<span data-ttu-id="8f630-289">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-289">Honda</span></span> |<span data-ttu-id="8f630-290">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-290">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-291">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="8f630-291">MDR 6128</span></span> |<span data-ttu-id="8f630-292">BMW</span><span class="sxs-lookup"><span data-stu-id="8f630-292">BMW</span></span> |<span data-ttu-id="8f630-293">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-293">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="8f630-294">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-294">**Output**:</span></span>

| <span data-ttu-id="8f630-295">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-295">LicensePlate</span></span> | <span data-ttu-id="8f630-296">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-296">Make</span></span> | <span data-ttu-id="8f630-297">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-297">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-298">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="8f630-298">DXE 5291</span></span> |<span data-ttu-id="8f630-299">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-299">Honda</span></span> |<span data-ttu-id="8f630-300">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-300">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="8f630-301">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="8f630-301">QYF 9358</span></span> |<span data-ttu-id="8f630-302">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-302">Honda</span></span> |<span data-ttu-id="8f630-303">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-303">2015-07-27T00:12:02.0000000Z</span></span> |

<span data-ttu-id="8f630-304">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-304">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

<span data-ttu-id="8f630-305">Most már most módosítás hello probléma és a keresés hello első autója egy adott Meggyőződünk minden 10 perces időközt a.</span><span class="sxs-lookup"><span data-stu-id="8f630-305">Now let’s change hello problem and find hello first car of a particular make in every 10-minute interval.</span></span>

| <span data-ttu-id="8f630-306">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-306">LicensePlate</span></span> | <span data-ttu-id="8f630-307">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-307">Make</span></span> | <span data-ttu-id="8f630-308">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-308">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-309">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="8f630-309">DXE 5291</span></span> |<span data-ttu-id="8f630-310">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-310">Honda</span></span> |<span data-ttu-id="8f630-311">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-311">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="8f630-312">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="8f630-312">YZK 5704</span></span> |<span data-ttu-id="8f630-313">Ford</span><span class="sxs-lookup"><span data-stu-id="8f630-313">Ford</span></span> |<span data-ttu-id="8f630-314">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-314">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="8f630-315">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="8f630-315">YHN 6970</span></span> |<span data-ttu-id="8f630-316">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-316">Toyota</span></span> |<span data-ttu-id="8f630-317">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-317">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="8f630-318">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="8f630-318">QYF 9358</span></span> |<span data-ttu-id="8f630-319">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-319">Honda</span></span> |<span data-ttu-id="8f630-320">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-320">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-321">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="8f630-321">MDR 6128</span></span> |<span data-ttu-id="8f630-322">BMW</span><span class="sxs-lookup"><span data-stu-id="8f630-322">BMW</span></span> |<span data-ttu-id="8f630-323">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-323">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="8f630-324">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-324">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-hello-last-event-in-a-window"></a><span data-ttu-id="8f630-325">Példa: keresés hello utolsó esemény ablakban</span><span class="sxs-lookup"><span data-stu-id="8f630-325">Query example: Find hello last event in a window</span></span>
<span data-ttu-id="8f630-326">**Leírás**: keresés hello utolsó car minden 10 perces időszakban.</span><span class="sxs-lookup"><span data-stu-id="8f630-326">**Description**: Find hello last car in every 10-minute interval.</span></span>

<span data-ttu-id="8f630-327">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-327">**Input**:</span></span>

| <span data-ttu-id="8f630-328">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-328">LicensePlate</span></span> | <span data-ttu-id="8f630-329">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-329">Make</span></span> | <span data-ttu-id="8f630-330">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-330">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-331">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="8f630-331">DXE 5291</span></span> |<span data-ttu-id="8f630-332">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-332">Honda</span></span> |<span data-ttu-id="8f630-333">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-333">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="8f630-334">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="8f630-334">YZK 5704</span></span> |<span data-ttu-id="8f630-335">Ford</span><span class="sxs-lookup"><span data-stu-id="8f630-335">Ford</span></span> |<span data-ttu-id="8f630-336">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-336">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="8f630-337">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="8f630-337">RMV 8282</span></span> |<span data-ttu-id="8f630-338">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-338">Honda</span></span> |<span data-ttu-id="8f630-339">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-339">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-340">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="8f630-340">YHN 6970</span></span> |<span data-ttu-id="8f630-341">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-341">Toyota</span></span> |<span data-ttu-id="8f630-342">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-342">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="8f630-343">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="8f630-343">VFE 1616</span></span> |<span data-ttu-id="8f630-344">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-344">Toyota</span></span> |<span data-ttu-id="8f630-345">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-345">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="8f630-346">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="8f630-346">QYF 9358</span></span> |<span data-ttu-id="8f630-347">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-347">Honda</span></span> |<span data-ttu-id="8f630-348">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-348">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-349">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="8f630-349">MDR 6128</span></span> |<span data-ttu-id="8f630-350">BMW</span><span class="sxs-lookup"><span data-stu-id="8f630-350">BMW</span></span> |<span data-ttu-id="8f630-351">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-351">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="8f630-352">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-352">**Output**:</span></span>

| <span data-ttu-id="8f630-353">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-353">LicensePlate</span></span> | <span data-ttu-id="8f630-354">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-354">Make</span></span> | <span data-ttu-id="8f630-355">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-355">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-356">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="8f630-356">VFE 1616</span></span> |<span data-ttu-id="8f630-357">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-357">Toyota</span></span> |<span data-ttu-id="8f630-358">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-358">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="8f630-359">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="8f630-359">MDR 6128</span></span> |<span data-ttu-id="8f630-360">BMW</span><span class="sxs-lookup"><span data-stu-id="8f630-360">BMW</span></span> |<span data-ttu-id="8f630-361">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-361">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="8f630-362">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-362">**Solution**:</span></span>

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

<span data-ttu-id="8f630-363">**MAGYARÁZAT**: hello lekérdezés két lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="8f630-363">**Explanation**: There are two steps in hello query.</span></span> <span data-ttu-id="8f630-364">hello első talál meg egy hello legújabb időbélyeg a windows 10 perc.</span><span class="sxs-lookup"><span data-stu-id="8f630-364">hello first one finds hello latest time stamp in 10-minute windows.</span></span> <span data-ttu-id="8f630-365">hello második lépésben illesztések hello hello eredeti adatfolyam toofind hello események, amelyek megfelelnek a hello minden ablakban utolsó időbélyegeket hello első lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="8f630-365">hello second step joins hello results of hello first query with hello original stream toofind hello events that match hello last time stamps in each window.</span></span> 

## <a name="query-example-detect-hello-absence-of-events"></a><span data-ttu-id="8f630-366">Példa: hello hiányában esemény észlelése</span><span class="sxs-lookup"><span data-stu-id="8f630-366">Query example: Detect hello absence of events</span></span>
<span data-ttu-id="8f630-367">**Leírás**: állítson be egy adatfolyam nincs érték, amely megfelel egy adott feltételnek.</span><span class="sxs-lookup"><span data-stu-id="8f630-367">**Description**: Check that a stream has no value that matches a certain criterion.</span></span>
<span data-ttu-id="8f630-368">Például 2 egymást követő autók hello megegyezik a megadott hello téren közúti belül hello utolsó 90 másodpercet?</span><span class="sxs-lookup"><span data-stu-id="8f630-368">For example, have 2 consecutive cars from hello same make entered hello toll road within hello last 90 seconds?</span></span>

<span data-ttu-id="8f630-369">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-369">**Input**:</span></span>

| <span data-ttu-id="8f630-370">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-370">Make</span></span> | <span data-ttu-id="8f630-371">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-371">LicensePlate</span></span> | <span data-ttu-id="8f630-372">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-372">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-373">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-373">Honda</span></span> |<span data-ttu-id="8f630-374">ABC – 123</span><span class="sxs-lookup"><span data-stu-id="8f630-374">ABC-123</span></span> |<span data-ttu-id="8f630-375">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-375">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="8f630-376">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-376">Honda</span></span> |<span data-ttu-id="8f630-377">AAA-999</span><span class="sxs-lookup"><span data-stu-id="8f630-377">AAA-999</span></span> |<span data-ttu-id="8f630-378">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-378">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="8f630-379">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-379">Toyota</span></span> |<span data-ttu-id="8f630-380">DEF-987</span><span class="sxs-lookup"><span data-stu-id="8f630-380">DEF-987</span></span> |<span data-ttu-id="8f630-381">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-381">2015-01-01T00:00:03.0000000Z</span></span> |
| <span data-ttu-id="8f630-382">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-382">Honda</span></span> |<span data-ttu-id="8f630-383">GHI-345</span><span class="sxs-lookup"><span data-stu-id="8f630-383">GHI-345</span></span> |<span data-ttu-id="8f630-384">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-384">2015-01-01T00:00:04.0000000Z</span></span> |

<span data-ttu-id="8f630-385">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-385">**Output**:</span></span>

| <span data-ttu-id="8f630-386">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-386">Make</span></span> | <span data-ttu-id="8f630-387">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-387">Time</span></span> | <span data-ttu-id="8f630-388">CurrentCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-388">CurrentCarLicensePlate</span></span> | <span data-ttu-id="8f630-389">FirstCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="8f630-389">FirstCarLicensePlate</span></span> | <span data-ttu-id="8f630-390">FirstCarTime</span><span class="sxs-lookup"><span data-stu-id="8f630-390">FirstCarTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="8f630-391">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-391">Honda</span></span> |<span data-ttu-id="8f630-392">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-392">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="8f630-393">AAA-999</span><span class="sxs-lookup"><span data-stu-id="8f630-393">AAA-999</span></span> |<span data-ttu-id="8f630-394">ABC – 123</span><span class="sxs-lookup"><span data-stu-id="8f630-394">ABC-123</span></span> |<span data-ttu-id="8f630-395">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-395">2015-01-01T00:00:01.0000000Z</span></span> |

<span data-ttu-id="8f630-396">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-396">**Solution**:</span></span>

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

<span data-ttu-id="8f630-397">**MAGYARÁZAT**: használata **LAG** toopeek hello a bemeneti adatfolyam vissza egy eseményt, és hello beolvasása **ellenőrizze** érték.</span><span class="sxs-lookup"><span data-stu-id="8f630-397">**Explanation**: Use **LAG** toopeek into hello input stream one event back and get hello **Make** value.</span></span> <span data-ttu-id="8f630-398">Hasonlítsa össze azokat toohello **ELLENŐRIZZE** hello aktuális esemény, és kimeneti hello esemény, ha azok vannak hello azonos érték.</span><span class="sxs-lookup"><span data-stu-id="8f630-398">Compare it toohello **MAKE** value in hello current event, and then output hello event if they are hello same.</span></span> <span data-ttu-id="8f630-399">Is **LAG** hello előző car tooget adatait.</span><span class="sxs-lookup"><span data-stu-id="8f630-399">You can also use **LAG** tooget data about hello previous car.</span></span>

## <a name="query-example-detect-hello-duration-between-events"></a><span data-ttu-id="8f630-400">Példa: hello időtartama események között észlelése</span><span class="sxs-lookup"><span data-stu-id="8f630-400">Query example: Detect hello duration between events</span></span>
<span data-ttu-id="8f630-401">**Leírás**: hello időtartam egy adott esemény található.</span><span class="sxs-lookup"><span data-stu-id="8f630-401">**Description**: Find hello duration of a given event.</span></span> <span data-ttu-id="8f630-402">Például egy webes clickstream tekintve megállapítani, hogy hello töltött be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8f630-402">For example, given a web clickstream, determine hello time spent on a feature.</span></span>

<span data-ttu-id="8f630-403">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-403">**Input**:</span></span>  

| <span data-ttu-id="8f630-404">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="8f630-404">User</span></span> | <span data-ttu-id="8f630-405">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8f630-405">Feature</span></span> | <span data-ttu-id="8f630-406">Esemény</span><span class="sxs-lookup"><span data-stu-id="8f630-406">Event</span></span> | <span data-ttu-id="8f630-407">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-407">Time</span></span> |
| --- | --- | --- | --- |
| user@location.com |<span data-ttu-id="8f630-408">RightMenu</span><span class="sxs-lookup"><span data-stu-id="8f630-408">RightMenu</span></span> |<span data-ttu-id="8f630-409">Indítás</span><span class="sxs-lookup"><span data-stu-id="8f630-409">Start</span></span> |<span data-ttu-id="8f630-410">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-410">2015-01-01T00:00:01.0000000Z</span></span> |
| user@location.com |<span data-ttu-id="8f630-411">RightMenu</span><span class="sxs-lookup"><span data-stu-id="8f630-411">RightMenu</span></span> |<span data-ttu-id="8f630-412">Vége</span><span class="sxs-lookup"><span data-stu-id="8f630-412">End</span></span> |<span data-ttu-id="8f630-413">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-413">2015-01-01T00:00:08.0000000Z</span></span> |

<span data-ttu-id="8f630-414">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-414">**Output**:</span></span>  

| <span data-ttu-id="8f630-415">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="8f630-415">User</span></span> | <span data-ttu-id="8f630-416">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8f630-416">Feature</span></span> | <span data-ttu-id="8f630-417">Időtartam</span><span class="sxs-lookup"><span data-stu-id="8f630-417">Duration</span></span> |
| --- | --- | --- |
| user@location.com |<span data-ttu-id="8f630-418">RightMenu</span><span class="sxs-lookup"><span data-stu-id="8f630-418">RightMenu</span></span> |<span data-ttu-id="8f630-419">7</span><span class="sxs-lookup"><span data-stu-id="8f630-419">7</span></span> |

<span data-ttu-id="8f630-420">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-420">**Solution**:</span></span>

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

<span data-ttu-id="8f630-421">**MAGYARÁZAT**: használata hello **utolsó** tooretrieve hello függvény utolsó **idő** értéke, ha hello esemény típusa történt **Start**.</span><span class="sxs-lookup"><span data-stu-id="8f630-421">**Explanation**: Use hello **LAST** function tooretrieve hello last **TIME** value when hello event type was **Start**.</span></span> <span data-ttu-id="8f630-422">Hello **utolsó** függvény **PARTITION BY [user]** , amelyek eredménye hello tooindicate számított egyedi felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="8f630-422">hello **LAST** function uses **PARTITION BY [user]** tooindicate that hello result is computed per unique user.</span></span> <span data-ttu-id="8f630-423">hello lekérdezés rendelkezik hello időeltérése 1 órás maximális küszöbértéket **Start** és **leállítása** események, de igény szerint konfigurálható **(korlát DURATION(hour, 1)**.</span><span class="sxs-lookup"><span data-stu-id="8f630-423">hello query has a 1-hour maximum threshold for hello time difference between **Start** and **Stop** events, but is configurable as needed **(LIMIT DURATION(hour, 1)**.</span></span>

## <a name="query-example-detect-hello-duration-of-a-condition"></a><span data-ttu-id="8f630-424">Példa: hello időtartama feltétel észlelése</span><span class="sxs-lookup"><span data-stu-id="8f630-424">Query example: Detect hello duration of a condition</span></span>
<span data-ttu-id="8f630-425">**Leírás**: található out mennyi ideig egy állapot fordult elő.</span><span class="sxs-lookup"><span data-stu-id="8f630-425">**Description**: Find out how long a condition occurred.</span></span>
<span data-ttu-id="8f630-426">Tegyük fel például, hogy programhiba eredményezett (fent 20 000 font) egy helytelen súlyozással rendelkező összes autók.</span><span class="sxs-lookup"><span data-stu-id="8f630-426">For example, suppose that a bug resulted in all cars having an incorrect weight (above 20,000 pounds).</span></span> <span data-ttu-id="8f630-427">Azt szeretnénk, ha hello hiba toocompute hello időtartama.</span><span class="sxs-lookup"><span data-stu-id="8f630-427">We want toocompute hello duration of hello bug.</span></span>

<span data-ttu-id="8f630-428">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-428">**Input**:</span></span>

| <span data-ttu-id="8f630-429">Ellenőrizze</span><span class="sxs-lookup"><span data-stu-id="8f630-429">Make</span></span> | <span data-ttu-id="8f630-430">Time</span><span class="sxs-lookup"><span data-stu-id="8f630-430">Time</span></span> | <span data-ttu-id="8f630-431">Súlyozás</span><span class="sxs-lookup"><span data-stu-id="8f630-431">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-432">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-432">Honda</span></span> |<span data-ttu-id="8f630-433">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-433">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="8f630-434">2000</span><span class="sxs-lookup"><span data-stu-id="8f630-434">2000</span></span> |
| <span data-ttu-id="8f630-435">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-435">Toyota</span></span> |<span data-ttu-id="8f630-436">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-436">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="8f630-437">25000</span><span class="sxs-lookup"><span data-stu-id="8f630-437">25000</span></span> |
| <span data-ttu-id="8f630-438">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-438">Honda</span></span> |<span data-ttu-id="8f630-439">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-439">2015-01-01T00:00:03.0000000Z</span></span> |<span data-ttu-id="8f630-440">26000</span><span class="sxs-lookup"><span data-stu-id="8f630-440">26000</span></span> |
| <span data-ttu-id="8f630-441">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-441">Toyota</span></span> |<span data-ttu-id="8f630-442">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-442">2015-01-01T00:00:04.0000000Z</span></span> |<span data-ttu-id="8f630-443">25000</span><span class="sxs-lookup"><span data-stu-id="8f630-443">25000</span></span> |
| <span data-ttu-id="8f630-444">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-444">Honda</span></span> |<span data-ttu-id="8f630-445">2015-01-01T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-445">2015-01-01T00:00:05.0000000Z</span></span> |<span data-ttu-id="8f630-446">26000</span><span class="sxs-lookup"><span data-stu-id="8f630-446">26000</span></span> |
| <span data-ttu-id="8f630-447">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-447">Toyota</span></span> |<span data-ttu-id="8f630-448">2015-01-01T00:00:06.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-448">2015-01-01T00:00:06.0000000Z</span></span> |<span data-ttu-id="8f630-449">25000</span><span class="sxs-lookup"><span data-stu-id="8f630-449">25000</span></span> |
| <span data-ttu-id="8f630-450">Honda</span><span class="sxs-lookup"><span data-stu-id="8f630-450">Honda</span></span> |<span data-ttu-id="8f630-451">2015-01-01T00:00:07.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-451">2015-01-01T00:00:07.0000000Z</span></span> |<span data-ttu-id="8f630-452">26000</span><span class="sxs-lookup"><span data-stu-id="8f630-452">26000</span></span> |
| <span data-ttu-id="8f630-453">Toyota</span><span class="sxs-lookup"><span data-stu-id="8f630-453">Toyota</span></span> |<span data-ttu-id="8f630-454">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-454">2015-01-01T00:00:08.0000000Z</span></span> |<span data-ttu-id="8f630-455">2000</span><span class="sxs-lookup"><span data-stu-id="8f630-455">2000</span></span> |

<span data-ttu-id="8f630-456">**Kimeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-456">**Output**:</span></span>

| <span data-ttu-id="8f630-457">StartFault</span><span class="sxs-lookup"><span data-stu-id="8f630-457">StartFault</span></span> | <span data-ttu-id="8f630-458">EndFault</span><span class="sxs-lookup"><span data-stu-id="8f630-458">EndFault</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-459">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-459">2015-01-01T00:00:02.000Z</span></span> |<span data-ttu-id="8f630-460">2015-01-01T00:00:07.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-460">2015-01-01T00:00:07.000Z</span></span> |

<span data-ttu-id="8f630-461">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-461">**Solution**:</span></span>

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

<span data-ttu-id="8f630-462">**MAGYARÁZAT**: használata **LAG** tooview hello bemeneti adatfolyam 24 órán át, és keresse meg a példányok where **StartFault** és **StopFault** hello által felölelt vannak súly < 20000.</span><span class="sxs-lookup"><span data-stu-id="8f630-462">**Explanation**: Use **LAG** tooview hello input stream for 24 hours and look for instances where **StartFault** and **StopFault** are spanned by hello weight < 20000.</span></span>

## <a name="query-example-fill-missing-values"></a><span data-ttu-id="8f630-463">Példa: Töltse ki a hiányzó értékek</span><span class="sxs-lookup"><span data-stu-id="8f630-463">Query example: Fill missing values</span></span>
<span data-ttu-id="8f630-464">**Leírás**: hello az események streamjét a hiányzó, a készít rendszeres időközönként események adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="8f630-464">**Description**: For hello stream of events that have missing values, produce a stream of events with regular intervals.</span></span>
<span data-ttu-id="8f630-465">Például generál egy eseményt 5 másodpercentként utoljára látott hello adatpont jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="8f630-465">For example, generate an event every 5 seconds that reports hello most recently seen data point.</span></span>

<span data-ttu-id="8f630-466">**Bemeneti**:</span><span class="sxs-lookup"><span data-stu-id="8f630-466">**Input**:</span></span>

| <span data-ttu-id="8f630-467">T</span><span class="sxs-lookup"><span data-stu-id="8f630-467">t</span></span> | <span data-ttu-id="8f630-468">érték</span><span class="sxs-lookup"><span data-stu-id="8f630-468">value</span></span> |
| --- | --- |
| <span data-ttu-id="8f630-469">"2014-01-01T06:01:00"</span><span class="sxs-lookup"><span data-stu-id="8f630-469">"2014-01-01T06:01:00"</span></span> |<span data-ttu-id="8f630-470">1</span><span class="sxs-lookup"><span data-stu-id="8f630-470">1</span></span> |
| <span data-ttu-id="8f630-471">"2014-01-01T06:01:05"</span><span class="sxs-lookup"><span data-stu-id="8f630-471">"2014-01-01T06:01:05"</span></span> |<span data-ttu-id="8f630-472">2</span><span class="sxs-lookup"><span data-stu-id="8f630-472">2</span></span> |
| <span data-ttu-id="8f630-473">"2014-01-01T06:01:10"</span><span class="sxs-lookup"><span data-stu-id="8f630-473">"2014-01-01T06:01:10"</span></span> |<span data-ttu-id="8f630-474">3</span><span class="sxs-lookup"><span data-stu-id="8f630-474">3</span></span> |
| <span data-ttu-id="8f630-475">"2014-01-01T06:01:15"</span><span class="sxs-lookup"><span data-stu-id="8f630-475">"2014-01-01T06:01:15"</span></span> |<span data-ttu-id="8f630-476">4</span><span class="sxs-lookup"><span data-stu-id="8f630-476">4</span></span> |
| <span data-ttu-id="8f630-477">"2014-01-01T06:01:30"</span><span class="sxs-lookup"><span data-stu-id="8f630-477">"2014-01-01T06:01:30"</span></span> |<span data-ttu-id="8f630-478">5</span><span class="sxs-lookup"><span data-stu-id="8f630-478">5</span></span> |
| <span data-ttu-id="8f630-479">"2014-01-01T06:01:35"</span><span class="sxs-lookup"><span data-stu-id="8f630-479">"2014-01-01T06:01:35"</span></span> |<span data-ttu-id="8f630-480">6</span><span class="sxs-lookup"><span data-stu-id="8f630-480">6</span></span> |

<span data-ttu-id="8f630-481">**Kimeneti (első 10 sor)**:</span><span class="sxs-lookup"><span data-stu-id="8f630-481">**Output (first 10 rows)**:</span></span>

| <span data-ttu-id="8f630-482">windowend</span><span class="sxs-lookup"><span data-stu-id="8f630-482">windowend</span></span> | <span data-ttu-id="8f630-483">lastevent.t</span><span class="sxs-lookup"><span data-stu-id="8f630-483">lastevent.t</span></span> | <span data-ttu-id="8f630-484">lastevent.Value</span><span class="sxs-lookup"><span data-stu-id="8f630-484">lastevent.value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8f630-485">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-485">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="8f630-486">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-486">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="8f630-487">1</span><span class="sxs-lookup"><span data-stu-id="8f630-487">1</span></span> |
| <span data-ttu-id="8f630-488">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-488">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="8f630-489">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-489">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="8f630-490">2</span><span class="sxs-lookup"><span data-stu-id="8f630-490">2</span></span> |
| <span data-ttu-id="8f630-491">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-491">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="8f630-492">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-492">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="8f630-493">3</span><span class="sxs-lookup"><span data-stu-id="8f630-493">3</span></span> |
| <span data-ttu-id="8f630-494">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-494">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="8f630-495">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-495">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="8f630-496">4</span><span class="sxs-lookup"><span data-stu-id="8f630-496">4</span></span> |
| <span data-ttu-id="8f630-497">2014-01-01T14:01:20.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-497">2014-01-01T14:01:20.000Z</span></span> |<span data-ttu-id="8f630-498">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-498">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="8f630-499">4</span><span class="sxs-lookup"><span data-stu-id="8f630-499">4</span></span> |
| <span data-ttu-id="8f630-500">2014-01-01T14:01:25.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-500">2014-01-01T14:01:25.000Z</span></span> |<span data-ttu-id="8f630-501">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-501">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="8f630-502">4</span><span class="sxs-lookup"><span data-stu-id="8f630-502">4</span></span> |
| <span data-ttu-id="8f630-503">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-503">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="8f630-504">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-504">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="8f630-505">5</span><span class="sxs-lookup"><span data-stu-id="8f630-505">5</span></span> |
| <span data-ttu-id="8f630-506">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-506">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="8f630-507">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-507">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="8f630-508">6</span><span class="sxs-lookup"><span data-stu-id="8f630-508">6</span></span> |
| <span data-ttu-id="8f630-509">2014-01-01T14:01:40.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-509">2014-01-01T14:01:40.000Z</span></span> |<span data-ttu-id="8f630-510">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-510">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="8f630-511">6</span><span class="sxs-lookup"><span data-stu-id="8f630-511">6</span></span> |
| <span data-ttu-id="8f630-512">2014-01-01T14:01:45.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-512">2014-01-01T14:01:45.000Z</span></span> |<span data-ttu-id="8f630-513">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="8f630-513">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="8f630-514">6</span><span class="sxs-lookup"><span data-stu-id="8f630-514">6</span></span> |

<span data-ttu-id="8f630-515">**Megoldás**:</span><span class="sxs-lookup"><span data-stu-id="8f630-515">**Solution**:</span></span>

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


<span data-ttu-id="8f630-516">**MAGYARÁZAT**: Ez a lekérdezés eseményeket hoz létre minden 5 másodperc és kimenetek hello korábban fogadott utolsó esemény.</span><span class="sxs-lookup"><span data-stu-id="8f630-516">**Explanation**: This query generates events every 5 seconds and outputs hello last event that was received previously.</span></span> <span data-ttu-id="8f630-517">Hello [Hopping ablak](https://msdn.microsoft.com/library/dn835041.aspx "Hopping ablak--Azure Stream Analytics") időtartamát határozza meg, milyen távolságban hátsó hello lekérdezés keres toofind hello utolsó esemény (ebben a példában 300 másodperc).</span><span class="sxs-lookup"><span data-stu-id="8f630-517">hello [Hopping window](https://msdn.microsoft.com/library/dn835041.aspx "Hopping window--Azure Stream Analytics") duration determines how far back hello query looks toofind hello latest event (300 seconds in this example).</span></span>

## <a name="get-help"></a><span data-ttu-id="8f630-518">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="8f630-518">Get help</span></span>
<span data-ttu-id="8f630-519">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="8f630-519">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f630-520">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f630-520">Next steps</span></span>
* [<span data-ttu-id="8f630-521">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="8f630-521">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="8f630-522">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="8f630-522">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="8f630-523">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="8f630-523">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="8f630-524">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="8f630-524">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="8f630-525">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="8f630-525">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)


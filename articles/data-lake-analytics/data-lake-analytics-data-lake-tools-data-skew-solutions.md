---
title: "Adatok-döntés teljesítése Azure Data Lake Tools for Visual Studio használatával |} Microsoft Docs"
description: "Hibaelhárítás az adatok-döntés problémák lehetséges megoldások Azure Data Lake Tools for Visual Studio használatával."
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/16/2016
ms.author: yanacai
ms.openlocfilehash: 9b284ef33be4b935569fc368d81ddf040b2c2b7d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="1a298-103">Adatok-döntés teljesítése Azure Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="1a298-103">Resolve data-skew problems by using Azure Data Lake Tools for Visual Studio</span></span>

## <a name="what-is-data-skew"></a><span data-ttu-id="1a298-104">Mi az az adatok döntés?</span><span class="sxs-lookup"><span data-stu-id="1a298-104">What is data skew?</span></span>

<span data-ttu-id="1a298-105">Rövid időre is hangsúlyoztuk, adatok döntés érték túlzott kezeli őket.</span><span class="sxs-lookup"><span data-stu-id="1a298-105">Briefly stated, data skew is an over-represented value.</span></span> <span data-ttu-id="1a298-106">Tegyük fel, hogy 50 adó elméleti adó értéket ad vissza, az egyes USA állapotához egy vizsgáztatónak naplózandó rendelt-e.</span><span class="sxs-lookup"><span data-stu-id="1a298-106">Imagine that you have assigned 50 tax examiners to audit tax returns, one examiner for each US state.</span></span> <span data-ttu-id="1a298-107">A Wyoming vizsgáztatónak az nincs a feltöltési kicsi, mert rendelkezik kevés elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1a298-107">The Wyoming examiner, because the population there is small, has little to do.</span></span> <span data-ttu-id="1a298-108">Kaliforniai azonban a vizsgáztatónak tartják túlzottan megnő a szerinti nagy feltöltési miatt.</span><span class="sxs-lookup"><span data-stu-id="1a298-108">In California, however, the examiner is kept very busy because of the state's large population.</span></span>
    <span data-ttu-id="1a298-109">![Adatok-döntés probléma – példa](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span><span class="sxs-lookup"><span data-stu-id="1a298-109">![Data-skew problem example](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span></span>

<span data-ttu-id="1a298-110">A mi esetünkben az adatok nem egyenlően oszlik el minden adó elméleti, ami azt jelenti, hogy néhány elméleti több, mint a többire kell-e működni.</span><span class="sxs-lookup"><span data-stu-id="1a298-110">In our scenario, the data is unevenly distributed across all tax examiners, which means that some examiners must work more than others.</span></span> <span data-ttu-id="1a298-111">Saját feladat olyan helyzetekben, például a adó-vizsgáztatónak példa itt gyakran előfordul.</span><span class="sxs-lookup"><span data-stu-id="1a298-111">In your own job, you frequently experience situations like the tax-examiner example here.</span></span> <span data-ttu-id="1a298-112">További technikai értelemben egy-egy csúcsának lekérdezi a társaknak mint sokkal több adatot, olyan helyzet, amely lehetővé teszi a több, mint a többi és, hogy végül működik csúcspont lelassítja az egy teljes feladat.</span><span class="sxs-lookup"><span data-stu-id="1a298-112">In more technical terms, one vertex gets much more data than its peers, a situation that makes the vertex work more than the others and that eventually slows down an entire job.</span></span> <span data-ttu-id="1a298-113">Mi az rosszabb, a feladat lehet, hogy sikertelen lesz, mert csúcsban lehet, például egy 5 órás futásidejű korlátozás és a memória 6 GB-os korlátozását.</span><span class="sxs-lookup"><span data-stu-id="1a298-113">What's worse, the job might fail, because vertices might have, for example, a 5-hour runtime limitation and a 6-GB memory limitation.</span></span>

## <a name="resolving-data-skew-problems"></a><span data-ttu-id="1a298-114">Adatok-döntés problémák megoldásához</span><span class="sxs-lookup"><span data-stu-id="1a298-114">Resolving data-skew problems</span></span>

<span data-ttu-id="1a298-115">Az Azure Data Lake Tools for Visual Studio segítségével észleli, hogy a feladat rendelkezik-e az adatok-döntés probléma.</span><span class="sxs-lookup"><span data-stu-id="1a298-115">Azure Data Lake Tools for Visual Studio can help detect whether your job has a data-skew problem.</span></span> <span data-ttu-id="1a298-116">Ha a probléma fennáll, hogyan oldható meg úgy a megoldások ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="1a298-116">If a problem exists, you can resolve it by trying the solutions in this section.</span></span>

## <a name="solution-1-improve-table-partitioning"></a><span data-ttu-id="1a298-117">1. megoldás: A táblaparticionálást javítása</span><span class="sxs-lookup"><span data-stu-id="1a298-117">Solution 1: Improve table partitioning</span></span>

### <a name="option-1-filter-the-skewed-key-value-in-advance"></a><span data-ttu-id="1a298-118">1. lehetőség: A kihasználtságot kulcsérték előre szűrése</span><span class="sxs-lookup"><span data-stu-id="1a298-118">Option 1: Filter the skewed key value in advance</span></span>

<span data-ttu-id="1a298-119">Nincs hatással az üzleti logikát, a nagyobb gyakoriságot értékek előre végezhet.</span><span class="sxs-lookup"><span data-stu-id="1a298-119">If it does not affect your business logic, you can filter the higher-frequency values in advance.</span></span> <span data-ttu-id="1a298-120">Például ha nagy mennyiségű 000 000-000 oszlopban GUID, előfordulhat, hogy nem kívánt ezt az értéket összesítő.</span><span class="sxs-lookup"><span data-stu-id="1a298-120">For example, if there are a lot of 000-000-000 in column GUID, you might not want to aggregate that value.</span></span> <span data-ttu-id="1a298-121">Mielőtt összesített, írhat "WHERE GUID! ="000 000-000"" szűrése nagyon gyakori értékét.</span><span class="sxs-lookup"><span data-stu-id="1a298-121">Before you aggregate, you can write “WHERE GUID != “000-000-000”” to filter the high-frequency value.</span></span>

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a><span data-ttu-id="1a298-122">2. lehetőség: Válasszon egy másik partíció vagy terjesztési kulcs</span><span class="sxs-lookup"><span data-stu-id="1a298-122">Option 2: Pick a different partition or distribution key</span></span>

<span data-ttu-id="1a298-123">Az előző példában ha csak ellenőrizni kívánja az ország, amelynek a adó-naplózási munkaterhelés javíthatja az adatok terjesztési azonosítója számára kattintva a kulcsként.</span><span class="sxs-lookup"><span data-stu-id="1a298-123">In the preceding example, if you want only to check the tax-audit workload all over the country, you can improve the data distribution by selecting the ID number as your key.</span></span> <span data-ttu-id="1a298-124">Egy másik partíció vagy a terjesztési kulcs kiadási is néha az adatok több egyenletes elosztása, de meg kell győződnie arról, hogy ez a beállítás nincs hatással az üzleti logikát.</span><span class="sxs-lookup"><span data-stu-id="1a298-124">Picking a different partition or distribution key can sometimes distribute the data more evenly, but you need to make sure that this choice doesn’t affect your business logic.</span></span> <span data-ttu-id="1a298-125">Például az egyes adó összeg kiszámításához, érdemes kijelölni _állapot_ partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="1a298-125">For instance, to calculate the tax sum for each state, you might want to designate _State_ as the partition key.</span></span> <span data-ttu-id="1a298-126">Ha továbbra is ebbe a problémába ütközik, próbálja meg a beállítás 3.</span><span class="sxs-lookup"><span data-stu-id="1a298-126">If you continue to experience this problem, try using Option 3.</span></span>

### <a name="option-3-add-more-partition-or-distribution-keys"></a><span data-ttu-id="1a298-127">3. lehetőség: Több partíció vagy terjesztési kulcsainak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1a298-127">Option 3: Add more partition or distribution keys</span></span>

<span data-ttu-id="1a298-128">Csak helyett _állapot_ partíciókulcsként, használhat több kulcs particionálást.</span><span class="sxs-lookup"><span data-stu-id="1a298-128">Instead of using only _State_ as a partition key, you can use more than one key for partitioning.</span></span> <span data-ttu-id="1a298-129">Például, vegyen fel _irányítószám_ csökkentése adatok-partíció méretét, és az adatok több egyenletes elosztása további partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="1a298-129">For example, consider adding _ZIP Code_ as an additional partition key to reduce data-partition sizes and distribute the data more evenly.</span></span>

### <a name="option-4-use-round-robin-distribution"></a><span data-ttu-id="1a298-130">4. lehetőség: Ciklikus multiplexelés használata</span><span class="sxs-lookup"><span data-stu-id="1a298-130">Option 4: Use round-robin distribution</span></span>

<span data-ttu-id="1a298-131">Ha nem találja a megfelelő kulcs partíció és terjesztési, megpróbálhatja ciklikus multiplexelés használandó.</span><span class="sxs-lookup"><span data-stu-id="1a298-131">If you cannot find an appropriate key for partition and distribution, you can try to use round-robin distribution.</span></span> <span data-ttu-id="1a298-132">Ciklikus multiplexelés egyaránt kezeli az összes sort, és véletlenszerűen megfelelő gyűjtők elhelyezi azokat.</span><span class="sxs-lookup"><span data-stu-id="1a298-132">Round-robin distribution treats all rows equally and randomly puts them into corresponding buckets.</span></span> <span data-ttu-id="1a298-133">Az adatok beolvasása egyenletesen, de helység információ, amely az egyes műveletek esetében a feladat teljesítmény is csökkentheti visszatérítési elveszíti.</span><span class="sxs-lookup"><span data-stu-id="1a298-133">The data gets evenly distributed, but it loses locality information, a drawback that can also reduce job performance for some operations.</span></span> <span data-ttu-id="1a298-134">Emellett akkor használatos, ha a kihasználtságot kulcs összesítési ennek ellenére is, az adatok-döntés probléma áll fenn.</span><span class="sxs-lookup"><span data-stu-id="1a298-134">Additionally, if you are doing aggregation for the skewed key anyway, the data-skew problem will persist.</span></span> <span data-ttu-id="1a298-135">Ciklikus multiplexelés kapcsolatos további tudnivalókért tekintse meg a U-SQL táblázat azokat a Terjesztéseket részt [CREATE TABLE (U-SQL): a táblázatok létrehozásáról az a séma](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span><span class="sxs-lookup"><span data-stu-id="1a298-135">To learn more about round-robin distribution, see the U-SQL Table Distributions section in [CREATE TABLE (U-SQL): Creating a Table with Schema](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span></span>

## <a name="solution-2-improve-the-query-plan"></a><span data-ttu-id="1a298-136">2. megoldás: A lekérdezésterv javítása</span><span class="sxs-lookup"><span data-stu-id="1a298-136">Solution 2: Improve the query plan</span></span>

### <a name="option-1-use-the-create-statistics-statement"></a><span data-ttu-id="1a298-137">1. lehetőség: A CREATE statistics UTASÍTÁSHOZ utasítás használható.</span><span class="sxs-lookup"><span data-stu-id="1a298-137">Option 1: Use the CREATE STATISTICS statement</span></span>

<span data-ttu-id="1a298-138">U-SQL-táblák a CREATE statistics UTASÍTÁSHOZ utasítás biztosít.</span><span class="sxs-lookup"><span data-stu-id="1a298-138">U-SQL provides the CREATE STATISTICS statement on tables.</span></span> <span data-ttu-id="1a298-139">A jelen nyilatkozat ad a lekérdezésoptimalizáló a adatjellemzők, többek között a terjesztési érték, a táblában tárolt kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="1a298-139">This statement gives more information to the query optimizer about the data characteristics, such as value distribution, that are stored in a table.</span></span> <span data-ttu-id="1a298-140">A legtöbb lekérdezésnél a lekérdezésoptimalizáló már jó minőségű lekérdezéstervet szükséges statisztikai adatait állít elő.</span><span class="sxs-lookup"><span data-stu-id="1a298-140">For most queries, the query optimizer already generates the necessary statistics for a high-quality query plan.</span></span> <span data-ttu-id="1a298-141">Alkalmanként szükség lehet további statisztikák létrehozása a CREATE statistics UTASÍTÁSHOZ vagy a lekérdezés tervezési módosításával javíthatja a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="1a298-141">Occasionally, you might need to improve query performance by creating additional statistics with CREATE STATISTICS or by modifying the query design.</span></span> <span data-ttu-id="1a298-142">További információkért lásd: a [CREATE statistics UTASÍTÁSHOZ (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="1a298-142">For more information, see the [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) page.</span></span>

<span data-ttu-id="1a298-143">Példa:</span><span class="sxs-lookup"><span data-stu-id="1a298-143">Code example:</span></span>

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
><span data-ttu-id="1a298-144">Statisztikai adatok nem frissülnek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="1a298-144">Statistics information is not updated automatically.</span></span> <span data-ttu-id="1a298-145">Ha frissíti a táblákban tárolt adatokat nélkül hozza létre újra a statisztikai adatokat, a lekérdezési teljesítmény előfordulhat, hogy elutasítja.</span><span class="sxs-lookup"><span data-stu-id="1a298-145">If you update the data in a table without re-creating the statistics, the query performance might decline.</span></span>

### <a name="option-2-use-skewfactor"></a><span data-ttu-id="1a298-146">2. lehetőség: SKEWFACTOR használata</span><span class="sxs-lookup"><span data-stu-id="1a298-146">Option 2: Use SKEWFACTOR</span></span>

<span data-ttu-id="1a298-147">Ha szeretné az egyes adó sum, a GROUP BY állapot, egy módszer, amely nem az adat-döntés probléma elkerülése érdekében kell használnia.</span><span class="sxs-lookup"><span data-stu-id="1a298-147">If you want to sum the tax for each state, you must use GROUP BY state, an approach that doesn't avoid the data-skew problem.</span></span> <span data-ttu-id="1a298-148">Adatok javaslat azonban biztosíthat adatok döntés kulcsok azonosítására, hogy a optimalizáló előkészíti végrehajtási tervének meg a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="1a298-148">However, you can provide a data hint in your query to identify data skew in keys so that the optimizer can prepare an execution plan for you.</span></span>

<span data-ttu-id="1a298-149">A paraméter állítja általában 0,5-1, tehát nem kap nagy eltérésére és 1 jelentése nagy döntés 0,5.</span><span class="sxs-lookup"><span data-stu-id="1a298-149">Usually, you can set the parameter as 0.5 and 1, with 0.5 meaning not much skew and 1 meaning heavy skew.</span></span> <span data-ttu-id="1a298-150">Mivel a mutató hatással van az aktuális utasítás és az összes alárendelt utasítást végrehajtási terv optimalizálást, ügyeljen arra, hogy a mutató hozzáadása előtt a lehetséges válik egyenetlenné key-wise összesítési.</span><span class="sxs-lookup"><span data-stu-id="1a298-150">Because the hint affects execution-plan optimization for the current statement and all downstream statements, be sure to add the hint before the potential skewed key-wise aggregation.</span></span>

    SKEWFACTOR (columns) = x

    Provides a hint that the given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

<span data-ttu-id="1a298-151">Példa:</span><span class="sxs-lookup"><span data-stu-id="1a298-151">Code example:</span></span>

    //Add a SKEWFACTOR hint.
    @Impressions =
        SELECT * FROM
        searchDM.SML.PageView(@start, @end) AS PageView
        OPTION(SKEWFACTOR(Query)=0.5)
        ;

    //Query 1 for key: Query, ClientId
    @Sessions =
        SELECT
            ClientId,
            Query,
            SUM(PageClicks) AS Clicks
        FROM
            @Impressions
        GROUP BY
            Query, ClientId
        ;

    //Query 2 for Key: Query
    @Display =
        SELECT * FROM @Sessions
            INNER JOIN @Campaigns
                ON @Sessions.Query == @Campaigns.Query
        ;   

### <a name="option-3-use-rowcount"></a><span data-ttu-id="1a298-152">3. lehetőség: ROWCOUNT használata</span><span class="sxs-lookup"><span data-stu-id="1a298-152">Option 3: Use ROWCOUNT</span></span>  
<span data-ttu-id="1a298-153">Mellett SKEWFACTOR az adott válik egyenetlenné kulcs illesztési esetben, hogy a többi összekapcsolt sorkészlet kicsi, ha megadható, hogy a optimalizáló ROWCOUNT mutató hozzáadásával a ILLESZTÉS előtt U-SQL-utasításban.</span><span class="sxs-lookup"><span data-stu-id="1a298-153">In addition to SKEWFACTOR, for specific skewed-key join cases, if you know that the other joined row set is small, you can tell the optimizer by adding a ROWCOUNT hint in the U-SQL statement before JOIN.</span></span> <span data-ttu-id="1a298-154">Ezzel a módszerrel optimalizáló választható szórási illesztés stratégiáról a teljesítmény növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="1a298-154">This way, optimizer can choose a broadcast join strategy to help improve performance.</span></span> <span data-ttu-id="1a298-155">Ne feledje, hogy sorszám nem oldja meg az adatok-döntés problémát, de néhány további segítséget kínál.</span><span class="sxs-lookup"><span data-stu-id="1a298-155">Be aware that ROWCOUNT does not resolve the data-skew problem, but it can offer some additional help.</span></span>

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

<span data-ttu-id="1a298-156">Példa:</span><span class="sxs-lookup"><span data-stu-id="1a298-156">Code example:</span></span>

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information to determine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-the-user-defined-reducer-and-combiner"></a><span data-ttu-id="1a298-157">3. megoldás: A felhasználó által definiált nyomáscsökkentő és egyesítő javítása</span><span class="sxs-lookup"><span data-stu-id="1a298-157">Solution 3: Improve the user-defined reducer and combiner</span></span>

<span data-ttu-id="1a298-158">Néha írhat egy operátor felhasználó által definiált összetett folyamat logika kezelésére, és egy jól megírt nyomáscsökkentő és egyesítő adatok-döntés probléma bizonyos esetekben előfordulhat, hogy mérsékelni.</span><span class="sxs-lookup"><span data-stu-id="1a298-158">You can sometimes write a user-defined operator to deal with complicated process logic, and a well-written reducer and combiner might mitigate a data-skew problem in some cases.</span></span>

### <a name="option-1-use-a-recursive-reducer-if-possible"></a><span data-ttu-id="1a298-159">1. lehetőség: Egy rekurzív nyomáscsökkentő, ha lehetséges használata</span><span class="sxs-lookup"><span data-stu-id="1a298-159">Option 1: Use a recursive reducer, if possible</span></span>

<span data-ttu-id="1a298-160">Alapértelmezés szerint a felhasználó által definiált nyomáscsökkentő fut nem rekurzív módban, ami azt jelenti, amelyekkel csökkenthető a kulcsok munkahelyi egyetlen csúcspont van elosztva.</span><span class="sxs-lookup"><span data-stu-id="1a298-160">By default, a user-defined reducer runs in non-recursive mode, which means that reduce work for a key is distributed into a single vertex.</span></span> <span data-ttu-id="1a298-161">Azonban az adatok válik egyenetlenné, ha nagyon nagy adatkészletek egyetlen csúcspont feldolgozása előfordulhat, hogy, és hosszú ideig fussanak.</span><span class="sxs-lookup"><span data-stu-id="1a298-161">But if your data is skewed, the huge data sets might be processed in a single vertex and run for a long time.</span></span>

<span data-ttu-id="1a298-162">A teljesítmény javítása érdekében a kódot, amely meghatározza a rekurzív módban való futásra nyomáscsökkentő attribútum adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="1a298-162">To improve performance, you can add an attribute in your code to define reducer to run in recursive mode.</span></span> <span data-ttu-id="1a298-163">Ezt követően a nagyon nagy adatkészletek több csúcsban terjesztése is, és felgyorsítja a feladat párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="1a298-163">Then, the huge data sets can be distributed to multiple vertices and run in parallel, which speeds up your job.</span></span>

<span data-ttu-id="1a298-164">Módosítsa a nem rekurzív nyomáscsökkentő rekurzív, győződjön meg arról, hogy a algoritmus társuló kell.</span><span class="sxs-lookup"><span data-stu-id="1a298-164">To change a non-recursive reducer to recursive, you need to make sure that your algorithm is associative.</span></span> <span data-ttu-id="1a298-165">Például a sum társuló, pedig a középérték nem.</span><span class="sxs-lookup"><span data-stu-id="1a298-165">For example, the sum is associative, and the median is not.</span></span> <span data-ttu-id="1a298-166">Szükség győződjön meg arról, hogy a bemeneti és kimeneti nyomáscsökkentő megtartja-e az azonos sémából.</span><span class="sxs-lookup"><span data-stu-id="1a298-166">You also need to make sure that the input and output for reducer keep the same schema.</span></span>

<span data-ttu-id="1a298-167">A rekurzív nyomáscsökkentő attribútum:</span><span class="sxs-lookup"><span data-stu-id="1a298-167">Attribute of recursive reducer:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]

<span data-ttu-id="1a298-168">Példa:</span><span class="sxs-lookup"><span data-stu-id="1a298-168">Code example:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a><span data-ttu-id="1a298-169">2. lehetőség: Használja a sorszintű egyesítő módot, ha lehetséges</span><span class="sxs-lookup"><span data-stu-id="1a298-169">Option 2: Use row-level combiner mode, if possible</span></span>

<span data-ttu-id="1a298-170">A sorszám mutatót adott válik egyenetlenné kulcs illesztési adódó hasonló, egyesítő mód megpróbálja terjesztheti a több csúcsban hatalmas válik egyenetlenné kulcs-érték beállítása, hogy a munka párhuzamosan futtatható.</span><span class="sxs-lookup"><span data-stu-id="1a298-170">Similar to the ROWCOUNT hint for specific skewed-key join cases, combiner mode tries to distribute huge skewed-key value sets to multiple vertices so that the work can be executed concurrently.</span></span> <span data-ttu-id="1a298-171">Egyesítő mód nem oldható fel az adatok-döntés problémákat, de felajánlhat hatalmas válik egyenetlenné kulcs-érték beállítása további segítséget.</span><span class="sxs-lookup"><span data-stu-id="1a298-171">Combiner mode can’t resolve data-skew issues, but it can offer some additional help for huge skewed-key value sets.</span></span>

<span data-ttu-id="1a298-172">Alapértelmezés szerint a egyesítő módja teljes, ami azt jelenti, hogy a sor bal és jobb oldali sor készlet nem kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="1a298-172">By default, the combiner mode is Full, which means that the left row set and right row set cannot be separated.</span></span> <span data-ttu-id="1a298-173">A mód beállítása balra vagy jobbra vagy belső, lehetővé teszi, hogy a sorszintű illesztési.</span><span class="sxs-lookup"><span data-stu-id="1a298-173">Setting the mode as Left/Right/Inner enables row-level join.</span></span> <span data-ttu-id="1a298-174">A rendszer a megfelelő sor készletek elválasztja, és továbbítja őket a párhuzamosan futó több csúcsban.</span><span class="sxs-lookup"><span data-stu-id="1a298-174">The system separates the corresponding row sets and distributes them into multiple vertices that run in parallel.</span></span> <span data-ttu-id="1a298-175">Azonban a egyesítő mód konfigurálása előtt ügyeljen arra, hogy győződjön meg arról, hogy a megfelelő sor készletek választhatók el egymástól.</span><span class="sxs-lookup"><span data-stu-id="1a298-175">However, before you configure the combiner mode, be careful to ensure that the corresponding row sets can be separated.</span></span>

<span data-ttu-id="1a298-176">Az alábbi példában elkülönített bal sorkészlet.</span><span class="sxs-lookup"><span data-stu-id="1a298-176">The example that follows shows a separated left row set.</span></span> <span data-ttu-id="1a298-177">Minden kimeneti sor bal oldali bemeneti egyetlen sor függ, és potenciálisan attól függ, a jobb oldali azonos kulcsértékkel rendelkező összes sorát.</span><span class="sxs-lookup"><span data-stu-id="1a298-177">Each output row depends on a single input row from the left, and it potentially depends on all rows from the right with the same key value.</span></span> <span data-ttu-id="1a298-178">Ha balra egyesítő módja annak beállítása, a rendszer elválasztja a hatalmas balra soron kívüli kis megfelelően beállítani, és hozzárendeli több csúcsban.</span><span class="sxs-lookup"><span data-stu-id="1a298-178">If you set the combiner mode as left, the system separates the huge left-row set into small ones and assigns them to multiple vertices.</span></span>

![Egyesítő mód ábra](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
><span data-ttu-id="1a298-180">Ha rossz egyesítő módja annak beállítása, a kombinált kevésbé hatékony, és lehet, hogy az eredmények még.</span><span class="sxs-lookup"><span data-stu-id="1a298-180">If you set the wrong combiner mode, the combination is less efficient, and the results might be wrong.</span></span>

<span data-ttu-id="1a298-181">A egyesítő mód attribútumok:</span><span class="sxs-lookup"><span data-stu-id="1a298-181">Attributes of combiner mode:</span></span>

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all the input rows from left and right with the same key value.

- <span data-ttu-id="1a298-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Minden kimeneti sor egyetlen bemeneti sor balra (és a jobb oldali kulcs azonos értékű potenciálisan összes sorát) függ.</span><span class="sxs-lookup"><span data-stu-id="1a298-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Every output row depends on a single input row from the left (and potentially all rows from the right with the same key value).</span></span>

- <span data-ttu-id="1a298-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): minden kimeneti sor egyetlen bemeneti sor jobb (és a bal oldali kulcs azonos értékű potenciálisan összes sorát) függ.</span><span class="sxs-lookup"><span data-stu-id="1a298-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): Every output row depends on a single input row from the right (and potentially all rows from the left with the same key value).</span></span>

- <span data-ttu-id="1a298-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Minden kimeneti sor a bal és jobb azonos értékű egyetlen bemeneti sor függ.</span><span class="sxs-lookup"><span data-stu-id="1a298-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Every output row depends on a single input row from the left and the right with the same value.</span></span>

<span data-ttu-id="1a298-185">Példa:</span><span class="sxs-lookup"><span data-stu-id="1a298-185">Code example:</span></span>

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }

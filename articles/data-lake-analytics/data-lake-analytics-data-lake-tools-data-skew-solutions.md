---
title: "Azure Data Lake Tools for Visual Studio használatával aaaResolve adatok-döntés problémák |} Microsoft Docs"
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
ms.openlocfilehash: 3909fbd89eb40f061268cb7128f7fa84a3c33de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a><span data-ttu-id="29d4c-103">Adatok-döntés teljesítése Azure Data Lake Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="29d4c-103">Resolve data-skew problems by using Azure Data Lake Tools for Visual Studio</span></span>

## <a name="what-is-data-skew"></a><span data-ttu-id="29d4c-104">Mi az az adatok döntés?</span><span class="sxs-lookup"><span data-stu-id="29d4c-104">What is data skew?</span></span>

<span data-ttu-id="29d4c-105">Rövid időre is hangsúlyoztuk, adatok döntés érték túlzott kezeli őket.</span><span class="sxs-lookup"><span data-stu-id="29d4c-105">Briefly stated, data skew is an over-represented value.</span></span> <span data-ttu-id="29d4c-106">Tegyük fel, hogy rendelt 50 adó elméleti tooaudit adó értéket ad vissza, az egyes USA állapotához egy vizsgáztatónak.</span><span class="sxs-lookup"><span data-stu-id="29d4c-106">Imagine that you have assigned 50 tax examiners tooaudit tax returns, one examiner for each US state.</span></span> <span data-ttu-id="29d4c-107">hello Wyoming vizsgáztatónak, mert nincs hello feltöltési kicsi, rendelkezik-e kis toodo.</span><span class="sxs-lookup"><span data-stu-id="29d4c-107">hello Wyoming examiner, because hello population there is small, has little toodo.</span></span> <span data-ttu-id="29d4c-108">Kaliforniai azonban hello vizsgáztatónak tartják foglalt nagy feltöltési hello állapota miatt.</span><span class="sxs-lookup"><span data-stu-id="29d4c-108">In California, however, hello examiner is kept very busy because of hello state's large population.</span></span>
    <span data-ttu-id="29d4c-109">![Adatok-döntés probléma – példa](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span><span class="sxs-lookup"><span data-stu-id="29d4c-109">![Data-skew problem example](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)</span></span>

<span data-ttu-id="29d4c-110">A mi esetünkben hello adatok nem egyenlően oszlik el minden adó elméleti, ami azt jelenti, hogy néhány elméleti több, mint a többire kell-e működni.</span><span class="sxs-lookup"><span data-stu-id="29d4c-110">In our scenario, hello data is unevenly distributed across all tax examiners, which means that some examiners must work more than others.</span></span> <span data-ttu-id="29d4c-111">Saját feladat hello adó-vizsgáztatónak itt példához hasonlóan helyzetekben gyakran előfordul.</span><span class="sxs-lookup"><span data-stu-id="29d4c-111">In your own job, you frequently experience situations like hello tax-examiner example here.</span></span> <span data-ttu-id="29d4c-112">További technikai értelemben az egy-egy csúcsának sokkal több adatot, mint a társaknak olyan helyzet, amely lehetővé teszi több, mint mások, valamint arról, hogy végül lelassítja az egy teljes feladat hello működik hello csúcspont lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="29d4c-112">In more technical terms, one vertex gets much more data than its peers, a situation that makes hello vertex work more than hello others and that eventually slows down an entire job.</span></span> <span data-ttu-id="29d4c-113">Mi az rosszabb, hello feladat lehet, hogy sikertelen lesz, mert csúcsban lehet, például egy 5 órás futásidejű korlátozás és a memória 6 GB-os korlátozását.</span><span class="sxs-lookup"><span data-stu-id="29d4c-113">What's worse, hello job might fail, because vertices might have, for example, a 5-hour runtime limitation and a 6-GB memory limitation.</span></span>

## <a name="resolving-data-skew-problems"></a><span data-ttu-id="29d4c-114">Adatok-döntés problémák megoldásához</span><span class="sxs-lookup"><span data-stu-id="29d4c-114">Resolving data-skew problems</span></span>

<span data-ttu-id="29d4c-115">Az Azure Data Lake Tools for Visual Studio segítségével észleli, hogy a feladat rendelkezik-e az adatok-döntés probléma.</span><span class="sxs-lookup"><span data-stu-id="29d4c-115">Azure Data Lake Tools for Visual Studio can help detect whether your job has a data-skew problem.</span></span> <span data-ttu-id="29d4c-116">Ha a probléma fennáll, hogyan oldható meg úgy hello megoldások ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="29d4c-116">If a problem exists, you can resolve it by trying hello solutions in this section.</span></span>

## <a name="solution-1-improve-table-partitioning"></a><span data-ttu-id="29d4c-117">1. megoldás: A táblaparticionálást javítása</span><span class="sxs-lookup"><span data-stu-id="29d4c-117">Solution 1: Improve table partitioning</span></span>

### <a name="option-1-filter-hello-skewed-key-value-in-advance"></a><span data-ttu-id="29d4c-118">1. lehetőség: Szűrő hello válik egyenetlenné kulcsérték előre</span><span class="sxs-lookup"><span data-stu-id="29d4c-118">Option 1: Filter hello skewed key value in advance</span></span>

<span data-ttu-id="29d4c-119">Nincs hatással az üzleti logikát, ha előre végezhet hello nagyobb gyakoriságot értékeket.</span><span class="sxs-lookup"><span data-stu-id="29d4c-119">If it does not affect your business logic, you can filter hello higher-frequency values in advance.</span></span> <span data-ttu-id="29d4c-120">Például ha nagy mennyiségű 000 000-000 oszlopban GUID, előfordulhat, hogy nem kívánja tooaggregate ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="29d4c-120">For example, if there are a lot of 000-000-000 in column GUID, you might not want tooaggregate that value.</span></span> <span data-ttu-id="29d4c-121">Mielőtt összesített, megírhatja "WHERE GUID!"000 000-000"=" toofilter hello nagyon gyakori érték.</span><span class="sxs-lookup"><span data-stu-id="29d4c-121">Before you aggregate, you can write “WHERE GUID != “000-000-000”” toofilter hello high-frequency value.</span></span>

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a><span data-ttu-id="29d4c-122">2. lehetőség: Válasszon egy másik partíció vagy terjesztési kulcs</span><span class="sxs-lookup"><span data-stu-id="29d4c-122">Option 2: Pick a different partition or distribution key</span></span>

<span data-ttu-id="29d4c-123">Hello megelőző példa Ha azt szeretné, hogy csak toocheck hello adó-naplózási munkaterhelés összes keresztül hello ország, hello adatok terjesztési javíthatja a kulcsként hello azonosítószámát kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="29d4c-123">In hello preceding example, if you want only toocheck hello tax-audit workload all over hello country, you can improve hello data distribution by selecting hello ID number as your key.</span></span> <span data-ttu-id="29d4c-124">Egy másik partíció vagy a terjesztési kulcs kiadási is néha hello adatok több egyenletes elosztása, de meg arról, hogy ez a beállítás nincs hatással az üzleti logikát toomake van szüksége.</span><span class="sxs-lookup"><span data-stu-id="29d4c-124">Picking a different partition or distribution key can sometimes distribute hello data more evenly, but you need toomake sure that this choice doesn’t affect your business logic.</span></span> <span data-ttu-id="29d4c-125">Például toocalculate hello adó sum a egyes állapotok hasznos lehet a toodesignate _állapot_ hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="29d4c-125">For instance, toocalculate hello tax sum for each state, you might want toodesignate _State_ as hello partition key.</span></span> <span data-ttu-id="29d4c-126">Ha továbbra is tooexperience ezt a problémát, próbálja meg beállítás 3.</span><span class="sxs-lookup"><span data-stu-id="29d4c-126">If you continue tooexperience this problem, try using Option 3.</span></span>

### <a name="option-3-add-more-partition-or-distribution-keys"></a><span data-ttu-id="29d4c-127">3. lehetőség: Több partíció vagy terjesztési kulcsainak hozzáadása</span><span class="sxs-lookup"><span data-stu-id="29d4c-127">Option 3: Add more partition or distribution keys</span></span>

<span data-ttu-id="29d4c-128">Csak helyett _állapot_ partíciókulcsként, használhat több kulcs particionálást.</span><span class="sxs-lookup"><span data-stu-id="29d4c-128">Instead of using only _State_ as a partition key, you can use more than one key for partitioning.</span></span> <span data-ttu-id="29d4c-129">Például, vegyen fel _irányítószám_ további partíciók kulcs tooreduce adatok-partíció méretének és hello adatok több egyenletes elosztása.</span><span class="sxs-lookup"><span data-stu-id="29d4c-129">For example, consider adding _ZIP Code_ as an additional partition key tooreduce data-partition sizes and distribute hello data more evenly.</span></span>

### <a name="option-4-use-round-robin-distribution"></a><span data-ttu-id="29d4c-130">4. lehetőség: Ciklikus multiplexelés használata</span><span class="sxs-lookup"><span data-stu-id="29d4c-130">Option 4: Use round-robin distribution</span></span>

<span data-ttu-id="29d4c-131">Ha a megfelelő kulcs nem található a partíció és a terjesztési, megpróbálhatja toouse ciklikus multiplexelés.</span><span class="sxs-lookup"><span data-stu-id="29d4c-131">If you cannot find an appropriate key for partition and distribution, you can try toouse round-robin distribution.</span></span> <span data-ttu-id="29d4c-132">Ciklikus multiplexelés egyaránt kezeli az összes sort, és véletlenszerűen megfelelő gyűjtők elhelyezi azokat.</span><span class="sxs-lookup"><span data-stu-id="29d4c-132">Round-robin distribution treats all rows equally and randomly puts them into corresponding buckets.</span></span> <span data-ttu-id="29d4c-133">hello adatokat lekérdezi egyenletesen, de helység információ, amely az egyes műveletek esetében a feladat teljesítmény is csökkentheti visszatérítési elveszíti.</span><span class="sxs-lookup"><span data-stu-id="29d4c-133">hello data gets evenly distributed, but it loses locality information, a drawback that can also reduce job performance for some operations.</span></span> <span data-ttu-id="29d4c-134">Emellett akkor használatos, ha összesítő hello kihasználtságot kulcs mindenképpen, hello adatok-döntés probléma áll fenn.</span><span class="sxs-lookup"><span data-stu-id="29d4c-134">Additionally, if you are doing aggregation for hello skewed key anyway, hello data-skew problem will persist.</span></span> <span data-ttu-id="29d4c-135">toolearn ciklikus multiplexelés bővebben lásd: hello U-SQL táblázat azokat a Terjesztéseket szakasz [CREATE TABLE (U-SQL): a táblázatok létrehozásáról az a séma](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span><span class="sxs-lookup"><span data-stu-id="29d4c-135">toolearn more about round-robin distribution, see hello U-SQL Table Distributions section in [CREATE TABLE (U-SQL): Creating a Table with Schema](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).</span></span>

## <a name="solution-2-improve-hello-query-plan"></a><span data-ttu-id="29d4c-136">2. megoldás: Hello lekérdezésterv javítása</span><span class="sxs-lookup"><span data-stu-id="29d4c-136">Solution 2: Improve hello query plan</span></span>

### <a name="option-1-use-hello-create-statistics-statement"></a><span data-ttu-id="29d4c-137">1. lehetőség: Hello CREATE statistics UTASÍTÁSHOZ utasítás használható.</span><span class="sxs-lookup"><span data-stu-id="29d4c-137">Option 1: Use hello CREATE STATISTICS statement</span></span>

<span data-ttu-id="29d4c-138">U-SQL-táblák hello CREATE statistics UTASÍTÁSHOZ utasítás biztosít.</span><span class="sxs-lookup"><span data-stu-id="29d4c-138">U-SQL provides hello CREATE STATISTICS statement on tables.</span></span> <span data-ttu-id="29d4c-139">A jelen nyilatkozat további információkat toohello lekérdezésoptimalizáló kapcsolatos hello adatjellemzők, többek között a terjesztési érték, a táblában tárolt biztosít.</span><span class="sxs-lookup"><span data-stu-id="29d4c-139">This statement gives more information toohello query optimizer about hello data characteristics, such as value distribution, that are stored in a table.</span></span> <span data-ttu-id="29d4c-140">A legtöbb lekérdezésnél hello lekérdezésoptimalizáló már jó minőségű lekérdezéstervet hello szükséges statisztikája állít elő.</span><span class="sxs-lookup"><span data-stu-id="29d4c-140">For most queries, hello query optimizer already generates hello necessary statistics for a high-quality query plan.</span></span> <span data-ttu-id="29d4c-141">Alkalmanként szükség lehet tooimprove lekérdezési teljesítmény a CREATE statistics UTASÍTÁSHOZ további statisztikák létrehozása vagy hello Lekérdezéstervezés módosításával.</span><span class="sxs-lookup"><span data-stu-id="29d4c-141">Occasionally, you might need tooimprove query performance by creating additional statistics with CREATE STATISTICS or by modifying hello query design.</span></span> <span data-ttu-id="29d4c-142">További információkért lásd: hello [CREATE statistics UTASÍTÁSHOZ (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="29d4c-142">For more information, see hello [CREATE STATISTICS (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) page.</span></span>

<span data-ttu-id="29d4c-143">Példa:</span><span class="sxs-lookup"><span data-stu-id="29d4c-143">Code example:</span></span>

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
><span data-ttu-id="29d4c-144">Statisztikai adatok nem frissülnek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="29d4c-144">Statistics information is not updated automatically.</span></span> <span data-ttu-id="29d4c-145">A tábla adatainak hello hello statisztika újbóli létrehozása nélkül frissítésekor hello lekérdezési teljesítmény előfordulhat, hogy elutasítja.</span><span class="sxs-lookup"><span data-stu-id="29d4c-145">If you update hello data in a table without re-creating hello statistics, hello query performance might decline.</span></span>

### <a name="option-2-use-skewfactor"></a><span data-ttu-id="29d4c-146">2. lehetőség: SKEWFACTOR használata</span><span class="sxs-lookup"><span data-stu-id="29d4c-146">Option 2: Use SKEWFACTOR</span></span>

<span data-ttu-id="29d4c-147">Az egyes toosum hello adó használni szeretne, ha a GROUP BY állapota, nem hello adatok-döntés probléma elkerülése érdekében egy megközelítést kell használnia.</span><span class="sxs-lookup"><span data-stu-id="29d4c-147">If you want toosum hello tax for each state, you must use GROUP BY state, an approach that doesn't avoid hello data-skew problem.</span></span> <span data-ttu-id="29d4c-148">Adatok javaslat azonban is adja meg a lekérdezés tooidentify adatok kulcsok döntés, így hello optimalizáló előkészíti végrehajtási tervének meg.</span><span class="sxs-lookup"><span data-stu-id="29d4c-148">However, you can provide a data hint in your query tooidentify data skew in keys so that hello optimizer can prepare an execution plan for you.</span></span>

<span data-ttu-id="29d4c-149">Hello paraméter állítja általában 0,5-1, tehát nem kap nagy eltérésére és 1 jelentése nagy döntés 0,5.</span><span class="sxs-lookup"><span data-stu-id="29d4c-149">Usually, you can set hello parameter as 0.5 and 1, with 0.5 meaning not much skew and 1 meaning heavy skew.</span></span> <span data-ttu-id="29d4c-150">Mivel hello mutató hatással van a végrehajtási terv optimalizálási hello aktuális utasítás és az összes alárendelt utasítást, csak meg arról, hogy tooadd hello mutató lehetséges hello válik egyenetlenné key-wise összesítési.</span><span class="sxs-lookup"><span data-stu-id="29d4c-150">Because hello hint affects execution-plan optimization for hello current statement and all downstream statements, be sure tooadd hello hint before hello potential skewed key-wise aggregation.</span></span>

    SKEWFACTOR (columns) = x

    Provides a hint that hello given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

<span data-ttu-id="29d4c-151">Példa:</span><span class="sxs-lookup"><span data-stu-id="29d4c-151">Code example:</span></span>

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

### <a name="option-3-use-rowcount"></a><span data-ttu-id="29d4c-152">3. lehetőség: ROWCOUNT használata</span><span class="sxs-lookup"><span data-stu-id="29d4c-152">Option 3: Use ROWCOUNT</span></span>  
<span data-ttu-id="29d4c-153">Ezenkívül tooSKEWFACTOR, az adott válik egyenetlenné kulcs csatlakozás esetben, ha tudja, hogy hello más illesztett sorkészlet kicsi, hello optimalizáló hello U-SQL utasítás ILLESZTÉS előtt ROWCOUNT mutató hozzáadásával állapítható meg.</span><span class="sxs-lookup"><span data-stu-id="29d4c-153">In addition tooSKEWFACTOR, for specific skewed-key join cases, if you know that hello other joined row set is small, you can tell hello optimizer by adding a ROWCOUNT hint in hello U-SQL statement before JOIN.</span></span> <span data-ttu-id="29d4c-154">Ezzel a módszerrel a optimalizáló választhat egy szórási illesztési stratégia toohelp teljesítmény javításához.</span><span class="sxs-lookup"><span data-stu-id="29d4c-154">This way, optimizer can choose a broadcast join strategy toohelp improve performance.</span></span> <span data-ttu-id="29d4c-155">Ne feledje, hogy sorszám nem oldja meg a hello adatok-döntés probléma, de néhány további segítséget kínál.</span><span class="sxs-lookup"><span data-stu-id="29d4c-155">Be aware that ROWCOUNT does not resolve hello data-skew problem, but it can offer some additional help.</span></span>

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

<span data-ttu-id="29d4c-156">Példa:</span><span class="sxs-lookup"><span data-stu-id="29d4c-156">Code example:</span></span>

    //Unstructured (24-hour daily log impressions)
    @Huge   = EXTRACT ClientId int, ...
                FROM @"wasb://ads@wcentralus/2015/10/30/{*}.nif"
                ;

    //Small subset (that is, ForgetMe opt out)
    @Small  = SELECT * FROM @Huge
                WHERE Bing.ForgetMe(x,y,z)
                OPTION(ROWCOUNT=500)
                ;

    //Result (not enough information toodetermine simple broadcast JOIN)
    @Remove = SELECT * FROM Bing.Sessions
                INNER JOIN @Small ON Sessions.Client == @Small.Client
                ;

## <a name="solution-3-improve-hello-user-defined-reducer-and-combiner"></a><span data-ttu-id="29d4c-157">3. megoldás: Hello felhasználói nyomáscsökkentő és egyesítő javítása</span><span class="sxs-lookup"><span data-stu-id="29d4c-157">Solution 3: Improve hello user-defined reducer and combiner</span></span>

<span data-ttu-id="29d4c-158">Néha egy felhasználó által definiált operátor toodeal összetett folyamat logikával írhat, és egy jól megírt nyomáscsökkentő és egyesítő adatok-döntés probléma bizonyos esetekben előfordulhat, hogy mérsékelni.</span><span class="sxs-lookup"><span data-stu-id="29d4c-158">You can sometimes write a user-defined operator toodeal with complicated process logic, and a well-written reducer and combiner might mitigate a data-skew problem in some cases.</span></span>

### <a name="option-1-use-a-recursive-reducer-if-possible"></a><span data-ttu-id="29d4c-159">1. lehetőség: Egy rekurzív nyomáscsökkentő, ha lehetséges használata</span><span class="sxs-lookup"><span data-stu-id="29d4c-159">Option 1: Use a recursive reducer, if possible</span></span>

<span data-ttu-id="29d4c-160">Alapértelmezés szerint a felhasználó által definiált nyomáscsökkentő fut nem rekurzív módban, ami azt jelenti, amelyekkel csökkenthető a kulcsok munkahelyi egyetlen csúcspont van elosztva.</span><span class="sxs-lookup"><span data-stu-id="29d4c-160">By default, a user-defined reducer runs in non-recursive mode, which means that reduce work for a key is distributed into a single vertex.</span></span> <span data-ttu-id="29d4c-161">Azonban ha az adatok válik egyenetlenné, hello túl nagy adatkészletek egyetlen csúcspont feldolgozása előfordulhat, hogy, és hosszú ideig fussanak.</span><span class="sxs-lookup"><span data-stu-id="29d4c-161">But if your data is skewed, hello huge data sets might be processed in a single vertex and run for a long time.</span></span>

<span data-ttu-id="29d4c-162">tooimprove teljesítmény, adhat hozzá egy attribútumot a kód toodefine nyomáscsökkentő toorun rekurzív módban.</span><span class="sxs-lookup"><span data-stu-id="29d4c-162">tooimprove performance, you can add an attribute in your code toodefine reducer toorun in recursive mode.</span></span> <span data-ttu-id="29d4c-163">Ezután hello túl nagy adatkészletek elosztott toomultiple csúcsban kell és felgyorsítja a feladat párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="29d4c-163">Then, hello huge data sets can be distributed toomultiple vertices and run in parallel, which speeds up your job.</span></span>

<span data-ttu-id="29d4c-164">a nem rekurzív nyomáscsökkentő toorecursive toochange, szüksége van arra, hogy a algoritmus társuló toomake.</span><span class="sxs-lookup"><span data-stu-id="29d4c-164">toochange a non-recursive reducer toorecursive, you need toomake sure that your algorithm is associative.</span></span> <span data-ttu-id="29d4c-165">Például hello sum társuló, pedig hello medián nem.</span><span class="sxs-lookup"><span data-stu-id="29d4c-165">For example, hello sum is associative, and hello median is not.</span></span> <span data-ttu-id="29d4c-166">Is van szüksége arról, hogy hello bemeneti és kimeneti nyomáscsökkentő megtartja hello toomake azonos sémából.</span><span class="sxs-lookup"><span data-stu-id="29d4c-166">You also need toomake sure that hello input and output for reducer keep hello same schema.</span></span>

<span data-ttu-id="29d4c-167">A rekurzív nyomáscsökkentő attribútum:</span><span class="sxs-lookup"><span data-stu-id="29d4c-167">Attribute of recursive reducer:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]

<span data-ttu-id="29d4c-168">Példa:</span><span class="sxs-lookup"><span data-stu-id="29d4c-168">Code example:</span></span>

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a><span data-ttu-id="29d4c-169">2. lehetőség: Használja a sorszintű egyesítő módot, ha lehetséges</span><span class="sxs-lookup"><span data-stu-id="29d4c-169">Option 2: Use row-level combiner mode, if possible</span></span>

<span data-ttu-id="29d4c-170">Hasonló toohello ROWCOUNT mutatóban megadott válik egyenetlenné kulcs illesztési esetekben egyesítő mód megpróbál toodistribute hatalmas válik egyenetlenné kulcs-érték beállítása toomultiple csúcsban, hogy hello munkahelyi párhuzamosan futtatható.</span><span class="sxs-lookup"><span data-stu-id="29d4c-170">Similar toohello ROWCOUNT hint for specific skewed-key join cases, combiner mode tries toodistribute huge skewed-key value sets toomultiple vertices so that hello work can be executed concurrently.</span></span> <span data-ttu-id="29d4c-171">Egyesítő mód nem oldható fel az adatok-döntés problémákat, de felajánlhat hatalmas válik egyenetlenné kulcs-érték beállítása további segítséget.</span><span class="sxs-lookup"><span data-stu-id="29d4c-171">Combiner mode can’t resolve data-skew issues, but it can offer some additional help for huge skewed-key value sets.</span></span>

<span data-ttu-id="29d4c-172">Alapértelmezés szerint a hello egyesítő mód nem teljes, vagyis hello sorkészlet bal és jobb a sorkészlet nem kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="29d4c-172">By default, hello combiner mode is Full, which means that hello left row set and right row set cannot be separated.</span></span> <span data-ttu-id="29d4c-173">Mint balra vagy jobbra vagy belső hello üzemmód lehetővé teszi, hogy a sorszintű illesztési.</span><span class="sxs-lookup"><span data-stu-id="29d4c-173">Setting hello mode as Left/Right/Inner enables row-level join.</span></span> <span data-ttu-id="29d4c-174">hello rendszer hello megfelelő sor beállítása elválasztja, és továbbítja őket a párhuzamosan futó több csúcsban.</span><span class="sxs-lookup"><span data-stu-id="29d4c-174">hello system separates hello corresponding row sets and distributes them into multiple vertices that run in parallel.</span></span> <span data-ttu-id="29d4c-175">Azonban hello egyesítő mód konfigurálása előtt ügyeljen arra, hogy a megfelelő sor beállítása hello tooensure választhatók el egymástól.</span><span class="sxs-lookup"><span data-stu-id="29d4c-175">However, before you configure hello combiner mode, be careful tooensure that hello corresponding row sets can be separated.</span></span>

<span data-ttu-id="29d4c-176">Hello, amely a következő példában egy elkülönített bal sorkészlet.</span><span class="sxs-lookup"><span data-stu-id="29d4c-176">hello example that follows shows a separated left row set.</span></span> <span data-ttu-id="29d4c-177">Minden kimeneti sor hello balról egyetlen bemeneti sor függ, és potenciálisan attól függ, minden sor a jogosultsággal rendelkező hello hello azonos kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="29d4c-177">Each output row depends on a single input row from hello left, and it potentially depends on all rows from hello right with hello same key value.</span></span> <span data-ttu-id="29d4c-178">Hello egyesítő mód balra állítja be, ha a hello rendszer elválasztja hello hatalmas balra soron kívüli kis megfelelően beállítani, és toomultiple csúcsban rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="29d4c-178">If you set hello combiner mode as left, hello system separates hello huge left-row set into small ones and assigns them toomultiple vertices.</span></span>

![Egyesítő mód ábra](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
><span data-ttu-id="29d4c-180">Ha hello helytelen egyesítő mód, hello kombinációja kevésbé hatékony, és lehet, hogy hello eredmények nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="29d4c-180">If you set hello wrong combiner mode, hello combination is less efficient, and hello results might be wrong.</span></span>

<span data-ttu-id="29d4c-181">A egyesítő mód attribútumok:</span><span class="sxs-lookup"><span data-stu-id="29d4c-181">Attributes of combiner mode:</span></span>

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all hello input rows from left and right with hello same key value.

- <span data-ttu-id="29d4c-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Minden kimeneti sor függ az egyetlen bemeneti sor hello balról (és potenciálisan összes olyan sort a jogosultsággal rendelkező hello hello azonos értéket).</span><span class="sxs-lookup"><span data-stu-id="29d4c-182">SqlUserDefinedCombiner(Mode=CombinerMode.Left): Every output row depends on a single input row from hello left (and potentially all rows from hello right with hello same key value).</span></span>

- <span data-ttu-id="29d4c-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): minden kimeneti sor attól függ, a jobb oldali hello egyetlen bemeneti sor (és potenciálisan összes olyan sort a hello hello balról azonos értéket).</span><span class="sxs-lookup"><span data-stu-id="29d4c-183">qlUserDefinedCombiner(Mode=CombinerMode.Right): Every output row depends on a single input row from hello right (and potentially all rows from hello left with hello same key value).</span></span>

- <span data-ttu-id="29d4c-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Minden kimeneti sor függ hello bal és jobb a hello hello egyetlen bemeneti sor azonos érték.</span><span class="sxs-lookup"><span data-stu-id="29d4c-184">SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Every output row depends on a single input row from hello left and hello right with hello same value.</span></span>

<span data-ttu-id="29d4c-185">Példa:</span><span class="sxs-lookup"><span data-stu-id="29d4c-185">Code example:</span></span>

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }

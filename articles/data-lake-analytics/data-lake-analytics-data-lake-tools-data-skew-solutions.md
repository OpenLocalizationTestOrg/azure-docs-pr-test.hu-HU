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
# <a name="resolve-data-skew-problems-by-using-azure-data-lake-tools-for-visual-studio"></a>Adatok-döntés teljesítése Azure Data Lake Tools for Visual Studio használatával

## <a name="what-is-data-skew"></a>Mi az az adatok döntés?

Rövid időre is hangsúlyoztuk, adatok döntés érték túlzott kezeli őket. Tegyük fel, hogy rendelt 50 adó elméleti tooaudit adó értéket ad vissza, az egyes USA állapotához egy vizsgáztatónak. hello Wyoming vizsgáztatónak, mert nincs hello feltöltési kicsi, rendelkezik-e kis toodo. Kaliforniai azonban hello vizsgáztatónak tartják foglalt nagy feltöltési hello állapota miatt.
    ![Adatok-döntés probléma – példa](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/data-skew-problem.png)

A mi esetünkben hello adatok nem egyenlően oszlik el minden adó elméleti, ami azt jelenti, hogy néhány elméleti több, mint a többire kell-e működni. Saját feladat hello adó-vizsgáztatónak itt példához hasonlóan helyzetekben gyakran előfordul. További technikai értelemben az egy-egy csúcsának sokkal több adatot, mint a társaknak olyan helyzet, amely lehetővé teszi több, mint mások, valamint arról, hogy végül lelassítja az egy teljes feladat hello működik hello csúcspont lekérdezi. Mi az rosszabb, hello feladat lehet, hogy sikertelen lesz, mert csúcsban lehet, például egy 5 órás futásidejű korlátozás és a memória 6 GB-os korlátozását.

## <a name="resolving-data-skew-problems"></a>Adatok-döntés problémák megoldásához

Az Azure Data Lake Tools for Visual Studio segítségével észleli, hogy a feladat rendelkezik-e az adatok-döntés probléma. Ha a probléma fennáll, hogyan oldható meg úgy hello megoldások ebben a szakaszban.

## <a name="solution-1-improve-table-partitioning"></a>1. megoldás: A táblaparticionálást javítása

### <a name="option-1-filter-hello-skewed-key-value-in-advance"></a>1. lehetőség: Szűrő hello válik egyenetlenné kulcsérték előre

Nincs hatással az üzleti logikát, ha előre végezhet hello nagyobb gyakoriságot értékeket. Például ha nagy mennyiségű 000 000-000 oszlopban GUID, előfordulhat, hogy nem kívánja tooaggregate ezt az értéket. Mielőtt összesített, megírhatja "WHERE GUID!"000 000-000"=" toofilter hello nagyon gyakori érték.

### <a name="option-2-pick-a-different-partition-or-distribution-key"></a>2. lehetőség: Válasszon egy másik partíció vagy terjesztési kulcs

Hello megelőző példa Ha azt szeretné, hogy csak toocheck hello adó-naplózási munkaterhelés összes keresztül hello ország, hello adatok terjesztési javíthatja a kulcsként hello azonosítószámát kiválasztásával. Egy másik partíció vagy a terjesztési kulcs kiadási is néha hello adatok több egyenletes elosztása, de meg arról, hogy ez a beállítás nincs hatással az üzleti logikát toomake van szüksége. Például toocalculate hello adó sum a egyes állapotok hasznos lehet a toodesignate _állapot_ hello partíció kulcsként. Ha továbbra is tooexperience ezt a problémát, próbálja meg beállítás 3.

### <a name="option-3-add-more-partition-or-distribution-keys"></a>3. lehetőség: Több partíció vagy terjesztési kulcsainak hozzáadása

Csak helyett _állapot_ partíciókulcsként, használhat több kulcs particionálást. Például, vegyen fel _irányítószám_ további partíciók kulcs tooreduce adatok-partíció méretének és hello adatok több egyenletes elosztása.

### <a name="option-4-use-round-robin-distribution"></a>4. lehetőség: Ciklikus multiplexelés használata

Ha a megfelelő kulcs nem található a partíció és a terjesztési, megpróbálhatja toouse ciklikus multiplexelés. Ciklikus multiplexelés egyaránt kezeli az összes sort, és véletlenszerűen megfelelő gyűjtők elhelyezi azokat. hello adatokat lekérdezi egyenletesen, de helység információ, amely az egyes műveletek esetében a feladat teljesítmény is csökkentheti visszatérítési elveszíti. Emellett akkor használatos, ha összesítő hello kihasználtságot kulcs mindenképpen, hello adatok-döntés probléma áll fenn. toolearn ciklikus multiplexelés bővebben lásd: hello U-SQL táblázat azokat a Terjesztéseket szakasz [CREATE TABLE (U-SQL): a táblázatok létrehozásáról az a séma](https://msdn.microsoft.com/en-us/library/mt706196.aspx#dis_sch).

## <a name="solution-2-improve-hello-query-plan"></a>2. megoldás: Hello lekérdezésterv javítása

### <a name="option-1-use-hello-create-statistics-statement"></a>1. lehetőség: Hello CREATE statistics UTASÍTÁSHOZ utasítás használható.

U-SQL-táblák hello CREATE statistics UTASÍTÁSHOZ utasítás biztosít. A jelen nyilatkozat további információkat toohello lekérdezésoptimalizáló kapcsolatos hello adatjellemzők, többek között a terjesztési érték, a táblában tárolt biztosít. A legtöbb lekérdezésnél hello lekérdezésoptimalizáló már jó minőségű lekérdezéstervet hello szükséges statisztikája állít elő. Alkalmanként szükség lehet tooimprove lekérdezési teljesítmény a CREATE statistics UTASÍTÁSHOZ további statisztikák létrehozása vagy hello Lekérdezéstervezés módosításával. További információkért lásd: hello [CREATE statistics UTASÍTÁSHOZ (U-SQL)](https://msdn.microsoft.com/en-us/library/azure/mt771898.aspx) lap.

Példa:

    CREATE STATISTICS IF NOT EXISTS stats_SampleTable_date ON SampleDB.dbo.SampleTable(date) WITH FULLSCAN;

>[!NOTE]
>Statisztikai adatok nem frissülnek automatikusan. A tábla adatainak hello hello statisztika újbóli létrehozása nélkül frissítésekor hello lekérdezési teljesítmény előfordulhat, hogy elutasítja.

### <a name="option-2-use-skewfactor"></a>2. lehetőség: SKEWFACTOR használata

Az egyes toosum hello adó használni szeretne, ha a GROUP BY állapota, nem hello adatok-döntés probléma elkerülése érdekében egy megközelítést kell használnia. Adatok javaslat azonban is adja meg a lekérdezés tooidentify adatok kulcsok döntés, így hello optimalizáló előkészíti végrehajtási tervének meg.

Hello paraméter állítja általában 0,5-1, tehát nem kap nagy eltérésére és 1 jelentése nagy döntés 0,5. Mivel hello mutató hatással van a végrehajtási terv optimalizálási hello aktuális utasítás és az összes alárendelt utasítást, csak meg arról, hogy tooadd hello mutató lehetséges hello válik egyenetlenné key-wise összesítési.

    SKEWFACTOR (columns) = x

    Provides a hint that hello given columns have a skew factor x from 0 (no skew) through 1 (very heavy skew).

Példa:

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

### <a name="option-3-use-rowcount"></a>3. lehetőség: ROWCOUNT használata  
Ezenkívül tooSKEWFACTOR, az adott válik egyenetlenné kulcs csatlakozás esetben, ha tudja, hogy hello más illesztett sorkészlet kicsi, hello optimalizáló hello U-SQL utasítás ILLESZTÉS előtt ROWCOUNT mutató hozzáadásával állapítható meg. Ezzel a módszerrel a optimalizáló választhat egy szórási illesztési stratégia toohelp teljesítmény javításához. Ne feledje, hogy sorszám nem oldja meg a hello adatok-döntés probléma, de néhány további segítséget kínál.

    OPTION(ROWCOUNT = n)

    Identify a small row set before JOIN by providing an estimated integer row count.

Példa:

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

## <a name="solution-3-improve-hello-user-defined-reducer-and-combiner"></a>3. megoldás: Hello felhasználói nyomáscsökkentő és egyesítő javítása

Néha egy felhasználó által definiált operátor toodeal összetett folyamat logikával írhat, és egy jól megírt nyomáscsökkentő és egyesítő adatok-döntés probléma bizonyos esetekben előfordulhat, hogy mérsékelni.

### <a name="option-1-use-a-recursive-reducer-if-possible"></a>1. lehetőség: Egy rekurzív nyomáscsökkentő, ha lehetséges használata

Alapértelmezés szerint a felhasználó által definiált nyomáscsökkentő fut nem rekurzív módban, ami azt jelenti, amelyekkel csökkenthető a kulcsok munkahelyi egyetlen csúcspont van elosztva. Azonban ha az adatok válik egyenetlenné, hello túl nagy adatkészletek egyetlen csúcspont feldolgozása előfordulhat, hogy, és hosszú ideig fussanak.

tooimprove teljesítmény, adhat hozzá egy attribútumot a kód toodefine nyomáscsökkentő toorun rekurzív módban. Ezután hello túl nagy adatkészletek elosztott toomultiple csúcsban kell és felgyorsítja a feladat párhuzamosan futnak.

a nem rekurzív nyomáscsökkentő toorecursive toochange, szüksége van arra, hogy a algoritmus társuló toomake. Például hello sum társuló, pedig hello medián nem. Is van szüksége arról, hogy hello bemeneti és kimeneti nyomáscsökkentő megtartja hello toomake azonos sémából.

A rekurzív nyomáscsökkentő attribútum:

    [SqlUserDefinedReducer(IsRecursive = true)]

Példa:

    [SqlUserDefinedReducer(IsRecursive = true)]
    public class TopNReducer : IReducer
    {
        public override IEnumerable<IRow>
            Reduce(IRowset input, IUpdatableRow output)
        {
            //Your reducer code goes here.
        }
    }

### <a name="option-2-use-row-level-combiner-mode-if-possible"></a>2. lehetőség: Használja a sorszintű egyesítő módot, ha lehetséges

Hasonló toohello ROWCOUNT mutatóban megadott válik egyenetlenné kulcs illesztési esetekben egyesítő mód megpróbál toodistribute hatalmas válik egyenetlenné kulcs-érték beállítása toomultiple csúcsban, hogy hello munkahelyi párhuzamosan futtatható. Egyesítő mód nem oldható fel az adatok-döntés problémákat, de felajánlhat hatalmas válik egyenetlenné kulcs-érték beállítása további segítséget.

Alapértelmezés szerint a hello egyesítő mód nem teljes, vagyis hello sorkészlet bal és jobb a sorkészlet nem kell elválasztani. Mint balra vagy jobbra vagy belső hello üzemmód lehetővé teszi, hogy a sorszintű illesztési. hello rendszer hello megfelelő sor beállítása elválasztja, és továbbítja őket a párhuzamosan futó több csúcsban. Azonban hello egyesítő mód konfigurálása előtt ügyeljen arra, hogy a megfelelő sor beállítása hello tooensure választhatók el egymástól.

Hello, amely a következő példában egy elkülönített bal sorkészlet. Minden kimeneti sor hello balról egyetlen bemeneti sor függ, és potenciálisan attól függ, minden sor a jogosultsággal rendelkező hello hello azonos kulcs értékét. Hello egyesítő mód balra állítja be, ha a hello rendszer elválasztja hello hatalmas balra soron kívüli kis megfelelően beállítani, és toomultiple csúcsban rendeli hozzá.

![Egyesítő mód ábra](./media/data-lake-analytics-data-lake-tools-data-skew-solutions/combiner-mode-illustration.png)

>[!NOTE]
>Ha hello helytelen egyesítő mód, hello kombinációja kevésbé hatékony, és lehet, hogy hello eredmények nem megfelelő.

A egyesítő mód attribútumok:

- [SqlUserDefinedCombiner(Mode=CombinerMode.Full)]: Every output row potentially depends on all hello input rows from left and right with hello same key value.

- SqlUserDefinedCombiner(Mode=CombinerMode.Left): Minden kimeneti sor függ az egyetlen bemeneti sor hello balról (és potenciálisan összes olyan sort a jogosultsággal rendelkező hello hello azonos értéket).

- qlUserDefinedCombiner(Mode=CombinerMode.Right): minden kimeneti sor attól függ, a jobb oldali hello egyetlen bemeneti sor (és potenciálisan összes olyan sort a hello hello balról azonos értéket).

- SqlUserDefinedCombiner(Mode=CombinerMode.Inner): Minden kimeneti sor függ hello bal és jobb a hello hello egyetlen bemeneti sor azonos érték.

Példa:

    [SqlUserDefinedCombiner(Mode = CombinerMode.Right)]
    public class WatsonDedupCombiner : ICombiner
    {
        public override IEnumerable<IRow>
            Combine(IRowset left, IRowset right, IUpdatableRow output)
        {
        //Your combiner code goes here.
        }
    }

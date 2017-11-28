---
title: "Helyettesítő kulcsok létrehozása identitásával |} Microsoft Docs"
description: "Megtudhatja, hogyan IDENTITÁS használatára a táblákon helyettesítő kulcsok létrehozásához."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/13/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 3ab5d159e6eaeb830135fe134e108b0e4de4b7d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a><span data-ttu-id="1ac6d-103">Identitásával helyettesítő kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ac6d-103">Create surrogate keys by using IDENTITY</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="1ac6d-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="1ac6d-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-105">[Data types][Data Types]</span></span>
> * <span data-ttu-id="1ac6d-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="1ac6d-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-107">[Index][Index]</span></span>
> * <span data-ttu-id="1ac6d-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="1ac6d-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="1ac6d-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-110">[Temporary][Temporary]</span></span>
> * <span data-ttu-id="1ac6d-111">[Identitás][Identity]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-111">[Identity][Identity]</span></span>
> 
> 

<span data-ttu-id="1ac6d-112">Sok adatok modelers, például azok a data warehouse modellek tervezésekor helyettesítő kulcsok létrehozásához a táblákon.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-112">Many data modelers like to create surrogate keys on their tables when they design data warehouse models.</span></span> <span data-ttu-id="1ac6d-113">Az IDENTITÁS tulajdonsággal e cél eléréséhez egyszerű és hatékony terhelés teljesítmény befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-113">You can use the IDENTITY property to achieve this goal simply and effectively without affecting load performance.</span></span> 

## <a name="get-started-with-identity"></a><span data-ttu-id="1ac6d-114">Ismerkedés az IDENTITÁS</span><span class="sxs-lookup"><span data-stu-id="1ac6d-114">Get started with IDENTITY</span></span>
<span data-ttu-id="1ac6d-115">Egy táblát az azonosító tulajdonság rendelkezőként, amikor először hoz létre a tábla a következő utasítás hasonló szintaxissal definiálhatja:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-115">You can define a table as having the IDENTITY property when you first create the table by using syntax that is similar to the following statement:</span></span>

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

<span data-ttu-id="1ac6d-116">Ezután `INSERT..SELECT` a táblázat feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-116">You can then use `INSERT..SELECT` to populate the table.</span></span>

## <a name="behavior"></a><span data-ttu-id="1ac6d-117">Viselkedés</span><span class="sxs-lookup"><span data-stu-id="1ac6d-117">Behavior</span></span>
<span data-ttu-id="1ac6d-118">Az azonosító tulajdonság célja, hogy az nem befolyásolja a terhelés teljesítmény között az adatraktár összes terjesztési kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-118">The IDENTITY property is designed to scale out across all the distributions in the data warehouse without affecting load performance.</span></span> <span data-ttu-id="1ac6d-119">IDENTITÁS megvalósítása ezért objektumorientált felé ezen célok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-119">Therefore, the implementation of IDENTITY is oriented toward achieving these goals.</span></span> <span data-ttu-id="1ac6d-120">Ez a szakasz a teljes körűen jobb megértése érdekében a végrehajtási apró mutatja be.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-120">This section highlights the nuances of the implementation to help you understand them more fully.</span></span>  

### <a name="allocation-of-values"></a><span data-ttu-id="1ac6d-121">Foglalási értékek</span><span class="sxs-lookup"><span data-stu-id="1ac6d-121">Allocation of values</span></span>
<span data-ttu-id="1ac6d-122">Az azonosító tulajdonság nem biztosítja a sorrendben, amelyben a helyettesítő értékek le van foglalva, amely tükrözi a működését, az SQL Server és az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-122">The IDENTITY property doesn't guarantee the order in which the surrogate values are allocated, which reflects the behavior of SQL Server and Azure SQL Database.</span></span> <span data-ttu-id="1ac6d-123">Azonban az Azure SQL Data Warehouse garancia hiányában hangsúlyozottan.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-123">However, in Azure SQL Data Warehouse, the absence of a guarantee is more pronounced.</span></span> 

<span data-ttu-id="1ac6d-124">Az alábbi példában a következő ábrát:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-124">The following example is an illustration:</span></span>

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

<span data-ttu-id="1ac6d-125">Az előző példában két sort terjesztési 1 ki.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-125">In the preceding example, two rows landed in distribution 1.</span></span> <span data-ttu-id="1ac6d-126">Az első sort tartalmaz a helyettesítő értéke 1 oszlopban `C1`, és a második sornak 61 helyettesítő értékét.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-126">The first row has the surrogate value of 1 in column `C1`, and the second row has the surrogate value of 61.</span></span> <span data-ttu-id="1ac6d-127">Mindkét ezeket az értékeket az azonosító tulajdonság hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-127">Both of these values were generated by the IDENTITY property.</span></span> <span data-ttu-id="1ac6d-128">Azonban nincs összefüggő a foglalási érték.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-128">However, the allocation of the values is not contiguous.</span></span> <span data-ttu-id="1ac6d-129">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-129">This behavior is by design.</span></span>

### <a name="skewed-data"></a><span data-ttu-id="1ac6d-130">Kihasználtságot adatok</span><span class="sxs-lookup"><span data-stu-id="1ac6d-130">Skewed data</span></span> 
<span data-ttu-id="1ac6d-131">A típusú értékek egyenlően elosztva a terjesztési.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-131">The range of values for the data type are spread evenly across the distributions.</span></span> <span data-ttu-id="1ac6d-132">Ha egy elosztott táblában romlik a kihasználtságot adatokból, majd az adattípus számára elérhető értékek is felhasználhatók, idő előtt.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-132">If a distributed table suffers from skewed data, then the range of values available to the datatype can be exhausted prematurely.</span></span> <span data-ttu-id="1ac6d-133">Például ha az adatok egyetlen terjesztési fejeződik be, majd hatékonyan a tábla el tudja érni csak-hatvanad adattípusú érték.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-133">For example, if all the data ends up in a single distribution, then effectively the table has access to only one-sixtieth of the values of the data type.</span></span> <span data-ttu-id="1ac6d-134">Ezért az azonosító tulajdonság korlátozva `INT` és `BIGINT` adattípus csak.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-134">For this reason, the IDENTITY property is limited to `INT` and `BIGINT` data types only.</span></span>

### <a name="selectinto"></a><span data-ttu-id="1ac6d-135">VÁLASSZON... A</span><span class="sxs-lookup"><span data-stu-id="1ac6d-135">SELECT..INTO</span></span>
<span data-ttu-id="1ac6d-136">Meglévő azonosító oszlop van kijelölve, egy új táblába, az új oszlop örökli az azonosító tulajdonság, kivéve, ha a következő feltételek valamelyike teljesül:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-136">When an existing IDENTITY column is selected into a new table, the new column inherits the IDENTITY property, unless one of the following conditions is true:</span></span>
- <span data-ttu-id="1ac6d-137">A SELECT utasítás illesztést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-137">The SELECT statement contains a join.</span></span>
- <span data-ttu-id="1ac6d-138">A UNION összekapcsolhatók több KIVÁLASZTÓ utasítást.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-138">Multiple SELECT statements are joined by using UNION.</span></span>
- <span data-ttu-id="1ac6d-139">Az azonosító oszlop szerepel a kiválasztási listán egynél többször.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-139">The IDENTITY column is listed more than one time in the SELECT list.</span></span>
- <span data-ttu-id="1ac6d-140">Az azonosító oszlop egy kifejezés része.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-140">The IDENTITY column is part of an expression.</span></span>
    
<span data-ttu-id="1ac6d-141">Ha ezek a feltételek bármelyike teljesül, az oszlop nem NULL helyett az azonosító tulajdonság öröklődés jön létre.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-141">If any one of these conditions is true, the column is created NOT NULL instead of inheriting the IDENTITY property.</span></span>

### <a name="create-table-as-select"></a><span data-ttu-id="1ac6d-142">TABLE AS SELECT LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="1ac6d-142">CREATE TABLE AS SELECT</span></span>
<span data-ttu-id="1ac6d-143">A következő dokumentált válassza ki az SQL Server viselkedést létrehozása TABLE AS kiválasztása (CTAS)... .</span><span class="sxs-lookup"><span data-stu-id="1ac6d-143">CREATE TABLE AS SELECT (CTAS) follows the same SQL Server behavior that's documented for SELECT..INTO.</span></span> <span data-ttu-id="1ac6d-144">Azonban nem adhat meg egy azonosító tulajdonság oszlop definíciójában a `CREATE TABLE` része az utasítást.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-144">However, you can't specify an IDENTITY property in the column definition of the `CREATE TABLE` part of the statement.</span></span> <span data-ttu-id="1ac6d-145">Az IDENTITY függvény a is használhatja a `SELECT` a CTAS része.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-145">You also can't use the IDENTITY function in the `SELECT` part of the CTAS.</span></span> <span data-ttu-id="1ac6d-146">A táblázat feltöltéséhez, kell használnia `CREATE TABLE` megadhatók a tábla követ `INSERT..SELECT` való feltöltése.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-146">To populate a table, you need to use `CREATE TABLE` to define the table followed by `INSERT..SELECT` to populate it.</span></span>

## <a name="explicitly-insert-values-into-an-identity-column"></a><span data-ttu-id="1ac6d-147">Explicit módon értékek beszúrása azonosító oszlop</span><span class="sxs-lookup"><span data-stu-id="1ac6d-147">Explicitly insert values into an IDENTITY column</span></span> 
<span data-ttu-id="1ac6d-148">Támogatja az SQL Data Warehouse `SET IDENTITY_INSERT <your table> ON|OFF` szintaxist.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-148">SQL Data Warehouse supports `SET IDENTITY_INSERT <your table> ON|OFF` syntax.</span></span> <span data-ttu-id="1ac6d-149">Ez a szintaxis segítségével explicit módon értékek beszúrása az identitásoszlop.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-149">You can use this syntax to explicitly insert values into the IDENTITY column.</span></span>

<span data-ttu-id="1ac6d-150">Sok adatok modelers, például a dimenziók bizonyos soraihoz előre meghatározott negatív értékek használata.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-150">Many data modelers like to use predefined negative values for certain rows in their dimensions.</span></span> <span data-ttu-id="1ac6d-151">Példa: "az ismeretlen tag" sor vagy a -1.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-151">An example is the -1 or "unknown member" row.</span></span> 

<span data-ttu-id="1ac6d-152">A következő parancsfájl bemutatja, hogyan explicit módon adja hozzá a sor IDENTITY_INSERT beállítása használatával:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-152">The next script shows how to explicitly add this row by using SET IDENTITY_INSERT:</span></span>

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a><span data-ttu-id="1ac6d-153">Adatok betöltése az IDENTITÁS tartalmazó tábla</span><span class="sxs-lookup"><span data-stu-id="1ac6d-153">Load data into a table with IDENTITY</span></span>

<span data-ttu-id="1ac6d-154">Az azonosító tulajdonság jelenléte rendelkezik néhány hatással vannak az Adatbetöltési kód.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-154">The presence of the IDENTITY property has some implications to your data-loading code.</span></span> <span data-ttu-id="1ac6d-155">Ez a szakasz néhány alapvető mintázatokból az adatok táblába töltéséhez identitásával mutatja be.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-155">This section highlights some basic patterns for loading data into tables by using IDENTITY.</span></span> 

### <a name="load-data-with-polybase"></a><span data-ttu-id="1ac6d-156">Adatok betöltése PolyBase-szel</span><span class="sxs-lookup"><span data-stu-id="1ac6d-156">Load data with PolyBase</span></span>
<span data-ttu-id="1ac6d-157">Adatok betöltése a következő táblába, és hozzon létre egy helyettesítő kulcsot használva IDENTITY, hozzon létre a táblát, és ezután használja az INSERT... Válassza ki, vagy szúrja be. A betöltése ÉRTÉKEKET.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-157">To load data into a table and generate a surrogate key by using IDENTITY, create the table and then use INSERT..SELECT or INSERT..VALUES to perform the load.</span></span>

<span data-ttu-id="1ac6d-158">A következő példa az alapvető mintát információk találhatók:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-158">The following example highlights the basic pattern:</span></span>
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> <span data-ttu-id="1ac6d-159">Nincs használható `CREATE TABLE AS SELECT` jelenleg, amikor az adatok betöltését azonosító oszlopot tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-159">It's not possible to use `CREATE TABLE AS SELECT` currently when loading data into a table with an IDENTITY column.</span></span>
> 

<span data-ttu-id="1ac6d-160">Az adatok betöltéséhez a tömeges másolási funkciójával (BCP) eszközzel további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-160">For more information on loading data by using the bulk copy program (BCP) tool, see the following articles:</span></span>

- <span data-ttu-id="1ac6d-161">[A polybase-zel betöltése][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-161">[Load with PolyBase][]</span></span>
- <span data-ttu-id="1ac6d-162">[PolyBase az ajánlott eljárások][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-162">[PolyBase best practices][]</span></span>

### <a name="load-data-with-bcp"></a><span data-ttu-id="1ac6d-163">Adatok betöltése a BCP használatával</span><span class="sxs-lookup"><span data-stu-id="1ac6d-163">Load data with BCP</span></span>
<span data-ttu-id="1ac6d-164">BCP parancssori eszköz segítségével adatok betöltése az SQL Data Warehouse-t.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-164">BCP is a command-line tool that you can use to load data into SQL Data Warehouse.</span></span> <span data-ttu-id="1ac6d-165">A paraméterek egyike (-E) BCP viselkedését vezérlő az azonosító oszlopot tartalmazó tábla adatainak betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-165">One of its parameters (-E) controls the behavior of BCP when loading data into a table with an IDENTITY column.</span></span> 

<span data-ttu-id="1ac6d-166">Ha -E meg van adva, az oszlop a bemeneti fájl identitású tárolt értékek kerülnek.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-166">When -E is specified, the values held in the input file for the column with IDENTITY are retained.</span></span> <span data-ttu-id="1ac6d-167">Ha van -E *nem* adott, akkor ebben az oszlopban szereplő értékek figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-167">If -E is *not* specified, then the values in this column are ignored.</span></span> <span data-ttu-id="1ac6d-168">Ha az azonosító oszlop nincs megadva, majd az adatok betöltése normál.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-168">If the identity column is not included, then the data is loaded as normal.</span></span> <span data-ttu-id="1ac6d-169">Az értékek a tulajdonság a növekvő, mind a seed házirendnek megfelelően jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-169">The values are generated according to the increment and seed policy of the property.</span></span>

<span data-ttu-id="1ac6d-170">Adatok betöltése BCP segítségével további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-170">For more information on loading data by using BCP, see the following articles:</span></span>

- <span data-ttu-id="1ac6d-171">[A BCP-vel betöltése][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-171">[Load with BCP][]</span></span>
- <span data-ttu-id="1ac6d-172">[A BCP az MSDN-en][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-172">[BCP in MSDN][]</span></span>

## <a name="catalog-views"></a><span data-ttu-id="1ac6d-173">Katalógus-nézetek</span><span class="sxs-lookup"><span data-stu-id="1ac6d-173">Catalog views</span></span>
<span data-ttu-id="1ac6d-174">Az SQL Data Warehouse támogatja a `sys.identity_columns` katalógus megtekintése.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-174">SQL Data Warehouse supports the `sys.identity_columns` catalog view.</span></span> <span data-ttu-id="1ac6d-175">Ez a nézet segítségével azonosíthatja a azonosítótulajdonság tartalmazó oszlop.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-175">This view can be used to identify a column that has the IDENTITY property.</span></span>

<span data-ttu-id="1ac6d-176">Jobb megértése érdekében az adatbázis-séma segítségével a példa bemutatja, hogyan integrálható `sys.identity_columns` a többi rendszer katalógus nézetek:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-176">To help you better understand the database schema, this example shows how to integrate `sys.identity_columns` with other system catalog views:</span></span>

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a><span data-ttu-id="1ac6d-177">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="1ac6d-177">Limitations</span></span>
<span data-ttu-id="1ac6d-178">Az azonosító tulajdonság nem használható a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-178">The IDENTITY property can't be used in the following scenarios:</span></span>
- <span data-ttu-id="1ac6d-179">Ha az oszlop adattípusához nincs INT vagy BIGINT</span><span class="sxs-lookup"><span data-stu-id="1ac6d-179">Where the column data type is not INT or BIGINT</span></span>
- <span data-ttu-id="1ac6d-180">Ha az oszlop is a terjesztési kulcs</span><span class="sxs-lookup"><span data-stu-id="1ac6d-180">Where the column is also the distribution key</span></span>
- <span data-ttu-id="1ac6d-181">Ha a tábla nem a külső tábla</span><span class="sxs-lookup"><span data-stu-id="1ac6d-181">Where the table is an external table</span></span> 

<span data-ttu-id="1ac6d-182">A következő kapcsolódó funkciók nem támogatottak az SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-182">The following related functions are not supported in SQL Data Warehouse:</span></span>

- <span data-ttu-id="1ac6d-183">[IDENTITY()][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-183">[IDENTITY()][]</span></span>
- <span data-ttu-id="1ac6d-184">[@@IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-184">[@@IDENTITY][]</span></span>
- <span data-ttu-id="1ac6d-185">[SCOPE_IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-185">[SCOPE_IDENTITY][]</span></span>
- <span data-ttu-id="1ac6d-186">[IDENT_CURRENT][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-186">[IDENT_CURRENT][]</span></span>
- <span data-ttu-id="1ac6d-187">[IDENT_INCR][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-187">[IDENT_INCR][]</span></span>
- <span data-ttu-id="1ac6d-188">[IDENT_SEED][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-188">[IDENT_SEED][]</span></span>
- <span data-ttu-id="1ac6d-189">[DBCC CHECK_IDENT()][]</span><span class="sxs-lookup"><span data-stu-id="1ac6d-189">[DBCC CHECK_IDENT()][]</span></span>

## <a name="tasks"></a><span data-ttu-id="1ac6d-190">Feladatok</span><span class="sxs-lookup"><span data-stu-id="1ac6d-190">Tasks</span></span>

<span data-ttu-id="1ac6d-191">Ez a témakör néhány mintakód segítségével végrehajthat olyan gyakori feladatokat azonosító oszlop használata.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-191">This section provides some sample code you can use to perform common tasks when you work with IDENTITY columns.</span></span>

> [!NOTE] 
> <span data-ttu-id="1ac6d-192">Oszlop C1 az alábbi feladataival IDENTITÁSA.</span><span class="sxs-lookup"><span data-stu-id="1ac6d-192">Column C1 is the IDENTITY in all the following tasks.</span></span>
> 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a><span data-ttu-id="1ac6d-193">A legmagasabb lefoglalt érték található egy táblához</span><span class="sxs-lookup"><span data-stu-id="1ac6d-193">Find the highest allocated value for a table</span></span>
<span data-ttu-id="1ac6d-194">Használja a `MAX()` működnek, mint a legmagasabb érték egy elosztott tábla számára lefoglalt meghatározásához:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-194">Use the `MAX()` function to determine the highest value allocated for a distributed table:</span></span>

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a><span data-ttu-id="1ac6d-195">Az azonosító tulajdonság a kezdőértékek és növekményértékek keresése</span><span class="sxs-lookup"><span data-stu-id="1ac6d-195">Find the seed and increment for the IDENTITY property</span></span>
<span data-ttu-id="1ac6d-196">A katalógus nézetek segítségével mezőértékek identitás növekvő, mind a seed konfigurációs tábla a következő lekérdezés segítségével:</span><span class="sxs-lookup"><span data-stu-id="1ac6d-196">You can use the catalog views to discover the identity increment and seed configuration values for a table by using the following query:</span></span> 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a><span data-ttu-id="1ac6d-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ac6d-197">Next steps</span></span>

* <span data-ttu-id="1ac6d-198">Táblák fejlesztésével kapcsolatos további tudnivalókért lásd: [tábla áttekintése][Overview], [adattípusok tábla][Data Types], [táblaterjesztése] [ Distribute], [Index táblázat][Index], [tábla particionálásához][Partition], és [ Az ideiglenes táblák][Temporary].</span><span class="sxs-lookup"><span data-stu-id="1ac6d-198">To learn more about developing tables, see [Table overview][Overview], [Table data types][Data Types], [Distribute a table][Distribute], [Index a table][Index], [Partition a table][Partition], and [Temporary tables][Temporary].</span></span> 
* <span data-ttu-id="1ac6d-199">Ajánlott eljárásokra vonatkozó további információkért lásd: [gyakorlati tanácsok az SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="1ac6d-199">For more information about best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse Best Practices].</span></span>  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<span data-ttu-id="1ac6d-200">[A BCP-vel betöltése]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/</span><span class="sxs-lookup"><span data-stu-id="1ac6d-200">[Load with bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/</span></span>
<span data-ttu-id="1ac6d-201">[A polybase-zel betöltése]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/</span><span class="sxs-lookup"><span data-stu-id="1ac6d-201">[Load with PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/</span></span>
<span data-ttu-id="1ac6d-202">[PolyBase az ajánlott eljárások]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/</span><span class="sxs-lookup"><span data-stu-id="1ac6d-202">[PolyBase best practices]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/</span></span>


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
<span data-ttu-id="1ac6d-203">[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-203">[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx</span></span>
<span data-ttu-id="1ac6d-204">[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-204">[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx</span></span>
<span data-ttu-id="1ac6d-205">[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-205">[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx</span></span>
<span data-ttu-id="1ac6d-206">[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-206">[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx</span></span>
<span data-ttu-id="1ac6d-207">[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-207">[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx</span></span>
<span data-ttu-id="1ac6d-208">[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-208">[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx</span></span>
<span data-ttu-id="1ac6d-209">[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-209">[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx</span></span>

<span data-ttu-id="1ac6d-210">[a BCP az MSDN-en]: https://msdn.microsoft.com/library/ms162802.aspx</span><span class="sxs-lookup"><span data-stu-id="1ac6d-210">[bcp in MSDN]: https://msdn.microsoft.com/library/ms162802.aspx</span></span>
  

<!--Other Web references-->  

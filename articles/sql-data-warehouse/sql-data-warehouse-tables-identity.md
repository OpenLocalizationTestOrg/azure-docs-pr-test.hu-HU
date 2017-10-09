---
title: "aaaCreate helyettesítő kulcsok identitásával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse IDENTITÁS toocreate helyettesítő kulcsok a táblákon."
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
ms.openlocfilehash: 502cdd2b510b229b2a19c1f78b11862a7386ae3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a><span data-ttu-id="d2a73-103">Identitásával helyettesítő kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2a73-103">Create surrogate keys by using IDENTITY</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="d2a73-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="d2a73-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="d2a73-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="d2a73-105">[Data types][Data Types]</span></span>
> * <span data-ttu-id="d2a73-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="d2a73-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="d2a73-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="d2a73-107">[Index][Index]</span></span>
> * <span data-ttu-id="d2a73-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="d2a73-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="d2a73-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="d2a73-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="d2a73-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="d2a73-110">[Temporary][Temporary]</span></span>
> * <span data-ttu-id="d2a73-111">[Identitás][Identity]</span><span class="sxs-lookup"><span data-stu-id="d2a73-111">[Identity][Identity]</span></span>
> 
> 

<span data-ttu-id="d2a73-112">Sok adatok modelers, például a táblák toocreate helyettesítő kulcsok azokat a data warehouse modellek tervezésekor.</span><span class="sxs-lookup"><span data-stu-id="d2a73-112">Many data modelers like toocreate surrogate keys on their tables when they design data warehouse models.</span></span> <span data-ttu-id="d2a73-113">Hello IDENTITÁS tulajdonság tooachieve a cél egyszerű és hatékony nélkül használhatja terhelés teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d2a73-113">You can use hello IDENTITY property tooachieve this goal simply and effectively without affecting load performance.</span></span> 

## <a name="get-started-with-identity"></a><span data-ttu-id="d2a73-114">Ismerkedés az IDENTITÁS</span><span class="sxs-lookup"><span data-stu-id="d2a73-114">Get started with IDENTITY</span></span>
<span data-ttu-id="d2a73-115">Egy tábla hello azonosítótulajdonság rendelkezőként, amikor hello tábla létrehozása, amely hasonló toohello utasítás a következő szintaxis használatával adhatja meg:</span><span class="sxs-lookup"><span data-stu-id="d2a73-115">You can define a table as having hello IDENTITY property when you first create hello table by using syntax that is similar toohello following statement:</span></span>

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

<span data-ttu-id="d2a73-116">Ezután `INSERT..SELECT` toopopulate hello tábla.</span><span class="sxs-lookup"><span data-stu-id="d2a73-116">You can then use `INSERT..SELECT` toopopulate hello table.</span></span>

## <a name="behavior"></a><span data-ttu-id="d2a73-117">Viselkedés</span><span class="sxs-lookup"><span data-stu-id="d2a73-117">Behavior</span></span>
<span data-ttu-id="d2a73-118">hello azonosító tulajdonság nem tervezett tooscale ki minden hello terjesztéseket hello adatraktár terhelés teljesítmény befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d2a73-118">hello IDENTITY property is designed tooscale out across all hello distributions in hello data warehouse without affecting load performance.</span></span> <span data-ttu-id="d2a73-119">IDENTITÁS hello megvalósítása ezért objektumorientált felé ezen célok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d2a73-119">Therefore, hello implementation of IDENTITY is oriented toward achieving these goals.</span></span> <span data-ttu-id="d2a73-120">Ez a szakasz hello apró kiemeli a hello megvalósítási toohelp tisztában azokat teljes körűen.</span><span class="sxs-lookup"><span data-stu-id="d2a73-120">This section highlights hello nuances of hello implementation toohelp you understand them more fully.</span></span>  

### <a name="allocation-of-values"></a><span data-ttu-id="d2a73-121">Foglalási értékek</span><span class="sxs-lookup"><span data-stu-id="d2a73-121">Allocation of values</span></span>
<span data-ttu-id="d2a73-122">hello azonosító tulajdonság nem garantálható, hogy hello sorrendben mely hello a helyettesítő értékek foglal le, amely tükrözi az SQL Server és az Azure SQL Database hello működését.</span><span class="sxs-lookup"><span data-stu-id="d2a73-122">hello IDENTITY property doesn't guarantee hello order in which hello surrogate values are allocated, which reflects hello behavior of SQL Server and Azure SQL Database.</span></span> <span data-ttu-id="d2a73-123">Azonban az Azure SQL Data Warehouse garancia hello hiányában hangsúlyozottan.</span><span class="sxs-lookup"><span data-stu-id="d2a73-123">However, in Azure SQL Data Warehouse, hello absence of a guarantee is more pronounced.</span></span> 

<span data-ttu-id="d2a73-124">a következő példa hello a következő ábrát:</span><span class="sxs-lookup"><span data-stu-id="d2a73-124">hello following example is an illustration:</span></span>

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

<span data-ttu-id="d2a73-125">A fenti példa hello két sornyi terjesztési 1 ki.</span><span class="sxs-lookup"><span data-stu-id="d2a73-125">In hello preceding example, two rows landed in distribution 1.</span></span> <span data-ttu-id="d2a73-126">hello első sornak hello helyettesítő értéke 1 oszlopban `C1`, és a második sornak hello helyettesítő értékének 61 hello.</span><span class="sxs-lookup"><span data-stu-id="d2a73-126">hello first row has hello surrogate value of 1 in column `C1`, and hello second row has hello surrogate value of 61.</span></span> <span data-ttu-id="d2a73-127">Ezeket az értékeket is hello azonosítótulajdonság hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="d2a73-127">Both of these values were generated by hello IDENTITY property.</span></span> <span data-ttu-id="d2a73-128">Azonban nincs összefüggő hello foglalási hello értékek.</span><span class="sxs-lookup"><span data-stu-id="d2a73-128">However, hello allocation of hello values is not contiguous.</span></span> <span data-ttu-id="d2a73-129">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="d2a73-129">This behavior is by design.</span></span>

### <a name="skewed-data"></a><span data-ttu-id="d2a73-130">Kihasználtságot adatok</span><span class="sxs-lookup"><span data-stu-id="d2a73-130">Skewed data</span></span> 
<span data-ttu-id="d2a73-131">hello értéktartományt hello adattípus egyenlően elosztva hello terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="d2a73-131">hello range of values for hello data type are spread evenly across hello distributions.</span></span> <span data-ttu-id="d2a73-132">Ha egy elosztott táblában romlik a kihasználtságot adatokból, majd hello elérhető toohello adattípus idő előtt kell kimerül értékek tartományán.</span><span class="sxs-lookup"><span data-stu-id="d2a73-132">If a distributed table suffers from skewed data, then hello range of values available toohello datatype can be exhausted prematurely.</span></span> <span data-ttu-id="d2a73-133">Például minden hello adatok egyetlen terjesztési fejeződik be, ha majd hatékonyan hello táblához hozzáférési tooonly hatvanad-hello értékek hello adattípusú.</span><span class="sxs-lookup"><span data-stu-id="d2a73-133">For example, if all hello data ends up in a single distribution, then effectively hello table has access tooonly one-sixtieth of hello values of hello data type.</span></span> <span data-ttu-id="d2a73-134">Emiatt hello azonosítótulajdonság korlátozódik túl`INT` és `BIGINT` adattípus csak.</span><span class="sxs-lookup"><span data-stu-id="d2a73-134">For this reason, hello IDENTITY property is limited too`INT` and `BIGINT` data types only.</span></span>

### <a name="selectinto"></a><span data-ttu-id="d2a73-135">VÁLASSZON... A</span><span class="sxs-lookup"><span data-stu-id="d2a73-135">SELECT..INTO</span></span>
<span data-ttu-id="d2a73-136">Meglévő azonosító oszlop van kijelölve, egy új táblába, hello új oszlop örökölnek hello azonosítótulajdonság, kivéve, ha hello a következő feltételek valamelyike teljesül:</span><span class="sxs-lookup"><span data-stu-id="d2a73-136">When an existing IDENTITY column is selected into a new table, hello new column inherits hello IDENTITY property, unless one of hello following conditions is true:</span></span>
- <span data-ttu-id="d2a73-137">hello SELECT utasítás illesztést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d2a73-137">hello SELECT statement contains a join.</span></span>
- <span data-ttu-id="d2a73-138">A UNION összekapcsolhatók több KIVÁLASZTÓ utasítást.</span><span class="sxs-lookup"><span data-stu-id="d2a73-138">Multiple SELECT statements are joined by using UNION.</span></span>
- <span data-ttu-id="d2a73-139">hello azonosító oszlop szerepel a kiválasztási listán hello egynél többször.</span><span class="sxs-lookup"><span data-stu-id="d2a73-139">hello IDENTITY column is listed more than one time in hello SELECT list.</span></span>
- <span data-ttu-id="d2a73-140">hello azonosító oszlop egy kifejezés része.</span><span class="sxs-lookup"><span data-stu-id="d2a73-140">hello IDENTITY column is part of an expression.</span></span>
    
<span data-ttu-id="d2a73-141">Ha ezek a feltételek bármelyike teljesül, a hello oszlop jön létre helyett öröklődés hello IDENTITY tulajdonsága nem null értékű.</span><span class="sxs-lookup"><span data-stu-id="d2a73-141">If any one of these conditions is true, hello column is created NOT NULL instead of inheriting hello IDENTITY property.</span></span>

### <a name="create-table-as-select"></a><span data-ttu-id="d2a73-142">TABLE AS SELECT LÉTREHOZÁSA</span><span class="sxs-lookup"><span data-stu-id="d2a73-142">CREATE TABLE AS SELECT</span></span>
<span data-ttu-id="d2a73-143">Hozzon létre TABLE AS kiválasztása (CTAS) következő dokumentált válassza ki az SQL Server viselkedést hello... .</span><span class="sxs-lookup"><span data-stu-id="d2a73-143">CREATE TABLE AS SELECT (CTAS) follows hello same SQL Server behavior that's documented for SELECT..INTO.</span></span> <span data-ttu-id="d2a73-144">Azonban nem adhat meg egy azonosító tulajdonság hello oszlop definíciójában hello `CREATE TABLE` hello utasítás része.</span><span class="sxs-lookup"><span data-stu-id="d2a73-144">However, you can't specify an IDENTITY property in hello column definition of hello `CREATE TABLE` part of hello statement.</span></span> <span data-ttu-id="d2a73-145">Még nem használható hello IDENTITY függvény hello `SELECT` hello CTAS része.</span><span class="sxs-lookup"><span data-stu-id="d2a73-145">You also can't use hello IDENTITY function in hello `SELECT` part of hello CTAS.</span></span> <span data-ttu-id="d2a73-146">toopopulate egy táblát, kell toouse `CREATE TABLE` toodefine hello tábla követ `INSERT..SELECT` toopopulate azt.</span><span class="sxs-lookup"><span data-stu-id="d2a73-146">toopopulate a table, you need toouse `CREATE TABLE` toodefine hello table followed by `INSERT..SELECT` toopopulate it.</span></span>

## <a name="explicitly-insert-values-into-an-identity-column"></a><span data-ttu-id="d2a73-147">Explicit módon értékek beszúrása azonosító oszlop</span><span class="sxs-lookup"><span data-stu-id="d2a73-147">Explicitly insert values into an IDENTITY column</span></span> 
<span data-ttu-id="d2a73-148">Támogatja az SQL Data Warehouse `SET IDENTITY_INSERT <your table> ON|OFF` szintaxist.</span><span class="sxs-lookup"><span data-stu-id="d2a73-148">SQL Data Warehouse supports `SET IDENTITY_INSERT <your table> ON|OFF` syntax.</span></span> <span data-ttu-id="d2a73-149">A szintaxis tooexplicitly insert értékek hello azonosító oszlop is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d2a73-149">You can use this syntax tooexplicitly insert values into hello IDENTITY column.</span></span>

<span data-ttu-id="d2a73-150">Sok adatok modelers például toouse előre definiált negatív értékek egyes a dimenziók sorokat.</span><span class="sxs-lookup"><span data-stu-id="d2a73-150">Many data modelers like toouse predefined negative values for certain rows in their dimensions.</span></span> <span data-ttu-id="d2a73-151">Példa: hello -1 vagy az "ismeretlen tag" sor.</span><span class="sxs-lookup"><span data-stu-id="d2a73-151">An example is hello -1 or "unknown member" row.</span></span> 

<span data-ttu-id="d2a73-152">hello tovább parancsfájl bemutatja, hogyan tooexplicitly sort ad hozzá az IDENTITY_INSERT beállítása használatával:</span><span class="sxs-lookup"><span data-stu-id="d2a73-152">hello next script shows how tooexplicitly add this row by using SET IDENTITY_INSERT:</span></span>

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

## <a name="load-data-into-a-table-with-identity"></a><span data-ttu-id="d2a73-153">Adatok betöltése az IDENTITÁS tartalmazó tábla</span><span class="sxs-lookup"><span data-stu-id="d2a73-153">Load data into a table with IDENTITY</span></span>

<span data-ttu-id="d2a73-154">hello jelenléte hello azonosító tulajdonság van néhány implications tooyour Adatbetöltési kódot.</span><span class="sxs-lookup"><span data-stu-id="d2a73-154">hello presence of hello IDENTITY property has some implications tooyour data-loading code.</span></span> <span data-ttu-id="d2a73-155">Ez a szakasz néhány alapvető mintázatokból az adatok táblába töltéséhez identitásával mutatja be.</span><span class="sxs-lookup"><span data-stu-id="d2a73-155">This section highlights some basic patterns for loading data into tables by using IDENTITY.</span></span> 

### <a name="load-data-with-polybase"></a><span data-ttu-id="d2a73-156">Adatok betöltése PolyBase-szel</span><span class="sxs-lookup"><span data-stu-id="d2a73-156">Load data with PolyBase</span></span>
<span data-ttu-id="d2a73-157">egy táblába tooload adatokat, és hozzon létre egy helyettesítő kulcsot használva IDENTITY, hello tábla létrehozása, és ezután használja az INSERT... Válassza ki, vagy szúrja be. Tooperform hello betöltése ÉRTÉKEKET.</span><span class="sxs-lookup"><span data-stu-id="d2a73-157">tooload data into a table and generate a surrogate key by using IDENTITY, create hello table and then use INSERT..SELECT or INSERT..VALUES tooperform hello load.</span></span>

<span data-ttu-id="d2a73-158">a következő példa emeli ki hello alapvető mintát hello:</span><span class="sxs-lookup"><span data-stu-id="d2a73-158">hello following example highlights hello basic pattern:</span></span>
 
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

--Use INSERT..SELECT toopopulate hello table from an external table
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
> <span data-ttu-id="d2a73-159">Nincs lehetséges toouse `CREATE TABLE AS SELECT` jelenleg, amikor az adatok betöltését azonosító oszlopot tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="d2a73-159">It's not possible toouse `CREATE TABLE AS SELECT` currently when loading data into a table with an IDENTITY column.</span></span>
> 

<span data-ttu-id="d2a73-160">Az adatok betöltéséhez hello tömeges másolási funkciójával (BCP) eszköz használatával további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="d2a73-160">For more information on loading data by using hello bulk copy program (BCP) tool, see hello following articles:</span></span>

- <span data-ttu-id="d2a73-161">[A polybase-zel betöltése][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-161">[Load with PolyBase][]</span></span>
- <span data-ttu-id="d2a73-162">[PolyBase az ajánlott eljárások][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-162">[PolyBase best practices][]</span></span>

### <a name="load-data-with-bcp"></a><span data-ttu-id="d2a73-163">Adatok betöltése a BCP használatával</span><span class="sxs-lookup"><span data-stu-id="d2a73-163">Load data with BCP</span></span>
<span data-ttu-id="d2a73-164">BCP parancssori eszköz használható tooload adatokat az SQL Data Warehouse-t.</span><span class="sxs-lookup"><span data-stu-id="d2a73-164">BCP is a command-line tool that you can use tooload data into SQL Data Warehouse.</span></span> <span data-ttu-id="d2a73-165">A paraméterek egyike (-E) vezérlők hello BCP viselkedését az azonosító oszlopot tartalmazó tábla adatainak betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="d2a73-165">One of its parameters (-E) controls hello behavior of BCP when loading data into a table with an IDENTITY column.</span></span> 

<span data-ttu-id="d2a73-166">Ha -E meg van adva, identitású hello oszlop hello bemeneti fájlban tárolt hello értékek megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="d2a73-166">When -E is specified, hello values held in hello input file for hello column with IDENTITY are retained.</span></span> <span data-ttu-id="d2a73-167">Ha van -E *nem* adott, akkor az oszlop értékeinek hello figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="d2a73-167">If -E is *not* specified, then hello values in this column are ignored.</span></span> <span data-ttu-id="d2a73-168">Ha hello azonosító oszlop nincs megadva, majd hello adatok betöltése normál.</span><span class="sxs-lookup"><span data-stu-id="d2a73-168">If hello identity column is not included, then hello data is loaded as normal.</span></span> <span data-ttu-id="d2a73-169">hello értékek toohello növekvő, mind a seed házirend hello tulajdonság szerint jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="d2a73-169">hello values are generated according toohello increment and seed policy of hello property.</span></span>

<span data-ttu-id="d2a73-170">Adatok betöltése BCP segítségével további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="d2a73-170">For more information on loading data by using BCP, see hello following articles:</span></span>

- <span data-ttu-id="d2a73-171">[A BCP-vel betöltése][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-171">[Load with BCP][]</span></span>
- <span data-ttu-id="d2a73-172">[A BCP az MSDN-en][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-172">[BCP in MSDN][]</span></span>

## <a name="catalog-views"></a><span data-ttu-id="d2a73-173">Katalógus-nézetek</span><span class="sxs-lookup"><span data-stu-id="d2a73-173">Catalog views</span></span>
<span data-ttu-id="d2a73-174">Az SQL Data Warehouse támogatja hello `sys.identity_columns` katalógus megtekintése.</span><span class="sxs-lookup"><span data-stu-id="d2a73-174">SQL Data Warehouse supports hello `sys.identity_columns` catalog view.</span></span> <span data-ttu-id="d2a73-175">Ez a nézet lehet használt tooidentify hello azonosítótulajdonság tartalmazó oszlop.</span><span class="sxs-lookup"><span data-stu-id="d2a73-175">This view can be used tooidentify a column that has hello IDENTITY property.</span></span>

<span data-ttu-id="d2a73-176">jobb megértése hello adatbázisséma toohelp, ez a példa bemutatja, hogyan toointegrate `sys.identity_columns` a többi rendszer katalógus nézetek:</span><span class="sxs-lookup"><span data-stu-id="d2a73-176">toohelp you better understand hello database schema, this example shows how toointegrate `sys.identity_columns` with other system catalog views:</span></span>

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

## <a name="limitations"></a><span data-ttu-id="d2a73-177">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="d2a73-177">Limitations</span></span>
<span data-ttu-id="d2a73-178">a következő forgatókönyvek hello hello azonosító tulajdonság nem használható:</span><span class="sxs-lookup"><span data-stu-id="d2a73-178">hello IDENTITY property can't be used in hello following scenarios:</span></span>
- <span data-ttu-id="d2a73-179">Ha hello oszlop adattípusa nem egész szám vagy BIGINT</span><span class="sxs-lookup"><span data-stu-id="d2a73-179">Where hello column data type is not INT or BIGINT</span></span>
- <span data-ttu-id="d2a73-180">Ha hello oszlop is hello terjesztési kulcs</span><span class="sxs-lookup"><span data-stu-id="d2a73-180">Where hello column is also hello distribution key</span></span>
- <span data-ttu-id="d2a73-181">A külső tábla hello tábla esetén</span><span class="sxs-lookup"><span data-stu-id="d2a73-181">Where hello table is an external table</span></span> 

<span data-ttu-id="d2a73-182">a következő kapcsolódó funkciók hello nem támogatottak az SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="d2a73-182">hello following related functions are not supported in SQL Data Warehouse:</span></span>

- <span data-ttu-id="d2a73-183">[IDENTITY()][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-183">[IDENTITY()][]</span></span>
- <span data-ttu-id="d2a73-184">[@@IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-184">[@@IDENTITY][]</span></span>
- <span data-ttu-id="d2a73-185">[SCOPE_IDENTITY][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-185">[SCOPE_IDENTITY][]</span></span>
- <span data-ttu-id="d2a73-186">[IDENT_CURRENT][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-186">[IDENT_CURRENT][]</span></span>
- <span data-ttu-id="d2a73-187">[IDENT_INCR][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-187">[IDENT_INCR][]</span></span>
- <span data-ttu-id="d2a73-188">[IDENT_SEED][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-188">[IDENT_SEED][]</span></span>
- <span data-ttu-id="d2a73-189">[DBCC CHECK_IDENT()][]</span><span class="sxs-lookup"><span data-stu-id="d2a73-189">[DBCC CHECK_IDENT()][]</span></span>

## <a name="tasks"></a><span data-ttu-id="d2a73-190">Feladatok</span><span class="sxs-lookup"><span data-stu-id="d2a73-190">Tasks</span></span>

<span data-ttu-id="d2a73-191">Ez a szakasz néhány példakódot tartalmaz azonosító oszlop használata tooperform gyakori feladatok használható.</span><span class="sxs-lookup"><span data-stu-id="d2a73-191">This section provides some sample code you can use tooperform common tasks when you work with IDENTITY columns.</span></span>

> [!NOTE] 
> <span data-ttu-id="d2a73-192">Oszlop C1 hello IDENTITÁS a feladatok következő összes hello.</span><span class="sxs-lookup"><span data-stu-id="d2a73-192">Column C1 is hello IDENTITY in all hello following tasks.</span></span>
> 
 
### <a name="find-hello-highest-allocated-value-for-a-table"></a><span data-ttu-id="d2a73-193">Egy tábla hello legmagasabb lefoglalt értéket keresi</span><span class="sxs-lookup"><span data-stu-id="d2a73-193">Find hello highest allocated value for a table</span></span>
<span data-ttu-id="d2a73-194">Használjon hello `MAX()` toodetermine hello legnagyobb értékét egy elosztott tábla számára lefoglalt működik:</span><span class="sxs-lookup"><span data-stu-id="d2a73-194">Use hello `MAX()` function toodetermine hello highest value allocated for a distributed table:</span></span>

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-hello-seed-and-increment-for-hello-identity-property"></a><span data-ttu-id="d2a73-195">Hello kezdőértékek és növekményértékek hello azonosító tulajdonság keresése</span><span class="sxs-lookup"><span data-stu-id="d2a73-195">Find hello seed and increment for hello IDENTITY property</span></span>
<span data-ttu-id="d2a73-196">Hello katalógus nézetek toodiscover hello identitás növekvő, mind a seed konfigurációs értékeket használhatja a tábla a következő lekérdezés hello segítségével:</span><span class="sxs-lookup"><span data-stu-id="d2a73-196">You can use hello catalog views toodiscover hello identity increment and seed configuration values for a table by using hello following query:</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="d2a73-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2a73-197">Next steps</span></span>

* <span data-ttu-id="d2a73-198">toolearn több táblák, fejlesztésével kapcsolatban lásd: [tábla áttekintése][Overview], [adattípusok tábla][Data Types], [táblaterjesztése] [ Distribute], [Index táblázat][Index], [tábla particionálásához][Partition], és [ Az ideiglenes táblák][Temporary].</span><span class="sxs-lookup"><span data-stu-id="d2a73-198">toolearn more about developing tables, see [Table overview][Overview], [Table data types][Data Types], [Distribute a table][Distribute], [Index a table][Index], [Partition a table][Partition], and [Temporary tables][Temporary].</span></span> 
* <span data-ttu-id="d2a73-199">Ajánlott eljárásokra vonatkozó további információkért lásd: [gyakorlati tanácsok az SQL Data Warehouse][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="d2a73-199">For more information about best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse Best Practices].</span></span>  

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

[A BCP-vel betöltése]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[A polybase-zel betöltése]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[PolyBase az ajánlott eljárások]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[a BCP az MSDN-en]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  

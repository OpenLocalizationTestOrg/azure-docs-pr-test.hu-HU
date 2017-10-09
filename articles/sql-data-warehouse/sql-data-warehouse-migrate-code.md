---
title: "aaaMigrate az SQL-kódot tooSQL adatraktár |} Microsoft Docs"
description: "Tippek az SQL-kódot tooAzure SQL Data Warehouse adattárházzal történő, megoldások áttelepítéséhez."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 19c252a3-0e41-4eec-9d3e-09a68c7e7add
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/23/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 7a16d579d068e9df9aba3dc61e4a09bcaa551588
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-sql-code-toosql-data-warehouse"></a><span data-ttu-id="e5499-103">Az SQL-kódot tooSQL adatraktár áttelepítése</span><span class="sxs-lookup"><span data-stu-id="e5499-103">Migrate your SQL code tooSQL Data Warehouse</span></span>
<span data-ttu-id="e5499-104">Ez a cikk azt ismerteti, valószínűleg szüksége lesz toomake a kód egy másik adatbázis tooSQL Data warehouse-ba való áttelepítés kódmódosításokat.</span><span class="sxs-lookup"><span data-stu-id="e5499-104">This article explains code changes you will probably need toomake when migrating your code from another database tooSQL Data Warehouse.</span></span> <span data-ttu-id="e5499-105">Néhány SQL Data Warehouse funkció jelentősen fejleszti a teljesítményt elosztott módon tervezett toowork változatlanok maradnak.</span><span class="sxs-lookup"><span data-stu-id="e5499-105">Some SQL Data Warehouse features can significantly improve performance as they are designed toowork in a distributed fashion.</span></span> <span data-ttu-id="e5499-106">Azonban toomaintain teljesítmény és méretezhetőség, néhány funkció nem érhetők el is.</span><span class="sxs-lookup"><span data-stu-id="e5499-106">However, toomaintain performance and scale, some features are also not available.</span></span>

## <a name="common-t-sql-limitations"></a><span data-ttu-id="e5499-107">Közös T-SQL-korlátozások</span><span class="sxs-lookup"><span data-stu-id="e5499-107">Common T-SQL Limitations</span></span>
<span data-ttu-id="e5499-108">a következő lista hello hello leggyakoribb szolgáltatása nem támogatja az SQL Data Warehouse foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="e5499-108">hello following list summarizes hello most common features that SQL Data Warehouse does not support.</span></span> <span data-ttu-id="e5499-109">hello hivatkozásokra tooworkarounds hello nem támogatott funkciók:</span><span class="sxs-lookup"><span data-stu-id="e5499-109">hello links take you tooworkarounds for hello unsupported features:</span></span>

* <span data-ttu-id="e5499-110">[A frissítések ANSI illesztések][ANSI joins on updates]</span><span class="sxs-lookup"><span data-stu-id="e5499-110">[ANSI joins on updates][ANSI joins on updates]</span></span>
* <span data-ttu-id="e5499-111">[Törli a ANSI illesztések][ANSI joins on deletes]</span><span class="sxs-lookup"><span data-stu-id="e5499-111">[ANSI joins on deletes][ANSI joins on deletes]</span></span>
* <span data-ttu-id="e5499-112">[merge utasítás][merge statement]</span><span class="sxs-lookup"><span data-stu-id="e5499-112">[merge statement][merge statement]</span></span>
* <span data-ttu-id="e5499-113">adatbázisok közötti illesztések</span><span class="sxs-lookup"><span data-stu-id="e5499-113">cross-database joins</span></span>
* <span data-ttu-id="e5499-114">[a kurzorok][cursors]</span><span class="sxs-lookup"><span data-stu-id="e5499-114">[cursors][cursors]</span></span>
* <span data-ttu-id="e5499-115">[INSERT... EXEC][INSERT..EXEC]</span><span class="sxs-lookup"><span data-stu-id="e5499-115">[INSERT..EXEC][INSERT..EXEC]</span></span>
* <span data-ttu-id="e5499-116">Output záradékban</span><span class="sxs-lookup"><span data-stu-id="e5499-116">output clause</span></span>
* <span data-ttu-id="e5499-117">belső felhasználó által definiált függvények</span><span class="sxs-lookup"><span data-stu-id="e5499-117">inline user-defined functions</span></span>
* <span data-ttu-id="e5499-118">több utasításból álló funkciók</span><span class="sxs-lookup"><span data-stu-id="e5499-118">multi-statement functions</span></span>
* [<span data-ttu-id="e5499-119">közös táblakifejezésekben</span><span class="sxs-lookup"><span data-stu-id="e5499-119">common table expressions</span></span>](#Common-table-expressions)
* <span data-ttu-id="e5499-120">[rekurzív közös táblakifejezésekben (Táblakifejezés)] (#Recursive-common-table-expressions-(CTE)</span><span class="sxs-lookup"><span data-stu-id="e5499-120">[recursive common table expressions (CTE)](#Recursive-common-table-expressions-(CTE)</span></span>
* <span data-ttu-id="e5499-121">CLR-függvények és eljárások</span><span class="sxs-lookup"><span data-stu-id="e5499-121">CLR functions and procedures</span></span>
* <span data-ttu-id="e5499-122">$partition-függvény</span><span class="sxs-lookup"><span data-stu-id="e5499-122">$partition function</span></span>
* <span data-ttu-id="e5499-123">a Táblaváltozók</span><span class="sxs-lookup"><span data-stu-id="e5499-123">table variables</span></span>
* <span data-ttu-id="e5499-124">tábla értékkel rendelkező paraméterek</span><span class="sxs-lookup"><span data-stu-id="e5499-124">table value parameters</span></span>
* <span data-ttu-id="e5499-125">az elosztott tranzakciók</span><span class="sxs-lookup"><span data-stu-id="e5499-125">distributed transactions</span></span>
* <span data-ttu-id="e5499-126">Commit / rollback munka</span><span class="sxs-lookup"><span data-stu-id="e5499-126">commit / rollback work</span></span>
* <span data-ttu-id="e5499-127">tranzakció mentése</span><span class="sxs-lookup"><span data-stu-id="e5499-127">save transaction</span></span>
* <span data-ttu-id="e5499-128">végrehajtási környezetek (EXECUTE AS)</span><span class="sxs-lookup"><span data-stu-id="e5499-128">execution contexts (EXECUTE AS)</span></span>
* <span data-ttu-id="e5499-129">[Group by záradék összesítő / adatkocka / csoportosítási készletek beállítások][group by clause with rollup / cube / grouping sets options]</span><span class="sxs-lookup"><span data-stu-id="e5499-129">[group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options]</span></span>
* <span data-ttu-id="e5499-130">[8 felett beágyazási szinttel][nesting levels beyond 8]</span><span class="sxs-lookup"><span data-stu-id="e5499-130">[nesting levels beyond 8][nesting levels beyond 8]</span></span>
* <span data-ttu-id="e5499-131">[nézetek frissítése][updating through views]</span><span class="sxs-lookup"><span data-stu-id="e5499-131">[updating through views][updating through views]</span></span>
* <span data-ttu-id="e5499-132">[Válassza ki a változó-hozzárendelés használata][use of select for variable assignment]</span><span class="sxs-lookup"><span data-stu-id="e5499-132">[use of select for variable assignment][use of select for variable assignment]</span></span>
* <span data-ttu-id="e5499-133">[Nincs maximális adattípus dinamikus SQL-karakterlánc:][no MAX data type for dynamic SQL strings]</span><span class="sxs-lookup"><span data-stu-id="e5499-133">[no MAX data type for dynamic SQL strings][no MAX data type for dynamic SQL strings]</span></span>

<span data-ttu-id="e5499-134">Szerencsére a legtöbb ezekkel a korlátozásokkal körül dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="e5499-134">Fortunately most of these limitations can be worked around.</span></span> <span data-ttu-id="e5499-135">Ismereteket szeretnének elsajátítani a fentiekben említett hello vonatkozó fejlesztési cikkek szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="e5499-135">Explanations are provided in hello relevant development articles referenced above.</span></span>

## <a name="supported-cte-features"></a><span data-ttu-id="e5499-136">Támogatott Táblakifejezés szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="e5499-136">Supported CTE features</span></span>
<span data-ttu-id="e5499-137">Közös táblakifejezésekben (Táblakifejezés) részben támogatottak az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e5499-137">Common table expressions (CTEs) are partially supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="e5499-138">a következő Táblakifejezés szolgáltatások hello jelenleg támogatottak:</span><span class="sxs-lookup"><span data-stu-id="e5499-138">hello following CTE features are currently supported:</span></span>

* <span data-ttu-id="e5499-139">Egy Táblakifejezés egy olyan SELECT utasításon adható meg.</span><span class="sxs-lookup"><span data-stu-id="e5499-139">A CTE can be specified in a SELECT statement.</span></span>
* <span data-ttu-id="e5499-140">Egy Táblakifejezés adható meg a NÉZET létrehozása utasításban.</span><span class="sxs-lookup"><span data-stu-id="e5499-140">A CTE can be specified in a CREATE VIEW statement.</span></span>
* <span data-ttu-id="e5499-141">Egy Táblakifejezés létrehozása TABLE AS kiválasztása (CTAS) utasításban adható meg.</span><span class="sxs-lookup"><span data-stu-id="e5499-141">A CTE can be specified in a CREATE TABLE AS SELECT (CTAS) statement.</span></span>
* <span data-ttu-id="e5499-142">Egy Táblakifejezés létrehozása távoli tábla AS kiválasztása (CRTAS) utasításban adható meg.</span><span class="sxs-lookup"><span data-stu-id="e5499-142">A CTE can be specified in a CREATE REMOTE TABLE AS SELECT (CRTAS) statement.</span></span>
* <span data-ttu-id="e5499-143">Egy Táblakifejezés létrehozása külső tábla AS kiválasztása (CETAS) utasításban adható meg.</span><span class="sxs-lookup"><span data-stu-id="e5499-143">A CTE can be specified in a CREATE EXTERNAL TABLE AS SELECT (CETAS) statement.</span></span>
* <span data-ttu-id="e5499-144">Egy Táblakifejezés távoli tábla lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="e5499-144">A remote table can be referenced from a CTE.</span></span>
* <span data-ttu-id="e5499-145">A külső tábla egy Táblakifejezés lehet hivatkozni.</span><span class="sxs-lookup"><span data-stu-id="e5499-145">An external table can be referenced from a CTE.</span></span>
* <span data-ttu-id="e5499-146">Több Táblakifejezés lekérdezés definíciója egy Táblakifejezés adható meg.</span><span class="sxs-lookup"><span data-stu-id="e5499-146">Multiple CTE query definitions can be defined in a CTE.</span></span>

## <a name="cte-limitations"></a><span data-ttu-id="e5499-147">Táblakifejezés korlátozásai</span><span class="sxs-lookup"><span data-stu-id="e5499-147">CTE Limitations</span></span>
<span data-ttu-id="e5499-148">Közös táblakifejezésekben a SQL Data Warehouse, amely bizonyos korlátozásokkal rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="e5499-148">Common table expressions have some limitations in SQL Data Warehouse including:</span></span>

* <span data-ttu-id="e5499-149">Egy Táblakifejezés egyetlen SELECT utasítással kell követnie.</span><span class="sxs-lookup"><span data-stu-id="e5499-149">A CTE must be followed by a single SELECT statement.</span></span> <span data-ttu-id="e5499-150">INSERT, UPDATE, DELETE és MERGE utasítások nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="e5499-150">INSERT, UPDATE, DELETE, and MERGE statements are not supported.</span></span>
* <span data-ttu-id="e5499-151">Közös táblakifejezés, amely tartalmazza a hivatkozások tooitself (rekurzív közös táblakifejezés) nem támogatott (lásd az alábbi szakaszt).</span><span class="sxs-lookup"><span data-stu-id="e5499-151">A common table expression that includes references tooitself (a recursive common table expression) is not supported (see below section).</span></span>
* <span data-ttu-id="e5499-152">Egy Táblakifejezés ad meg egynél több ZÁRADÉKKAL nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="e5499-152">Specifying more than one WITH clause in a CTE is not allowed.</span></span> <span data-ttu-id="e5499-153">Például ha egy CTE_query_definition allekérdezést tartalmaz, a segédlekérdezés nem tartalmazhat beágyazott, amely meghatározza egy másik Táblakifejezés ZÁRADÉKKAL.</span><span class="sxs-lookup"><span data-stu-id="e5499-153">For example, if a CTE_query_definition contains a subquery, that subquery cannot contain a nested WITH clause that defines another CTE.</span></span>
* <span data-ttu-id="e5499-154">ORDER BY záradék nem használható hello CTE_query_definition, kivéve ha TOP záradék meg van adva.</span><span class="sxs-lookup"><span data-stu-id="e5499-154">An ORDER BY clause cannot be used in hello CTE_query_definition, except when a TOP clause is specified.</span></span>
* <span data-ttu-id="e5499-155">Egy Táblakifejezés olyan utasításban, amely része egy kötegelt használata esetén hello utasítás előtt pontosvesszővel kell követnie.</span><span class="sxs-lookup"><span data-stu-id="e5499-155">When a CTE is used in a statement that is part of a batch, hello statement before it must be followed by a semicolon.</span></span>
* <span data-ttu-id="e5499-156">Használatát olyan utasításokban sp_prepare készítette, amikor Táblakifejezés hello működik ugyanúgy, ahogy a PDW más kiválasztott utasítást.</span><span class="sxs-lookup"><span data-stu-id="e5499-156">When used in statements prepared by sp_prepare, CTEs will behave hello same way as other SELECT statements in PDW.</span></span> <span data-ttu-id="e5499-157">Azonban Táblakifejezés sp_prepare tanulmányát CETAS részeként használata esetén hello viselkedését is késlelteti az SQL Server és az egyéb PDW utasítások hello módon használható a kötés sp_prepare miatt.</span><span class="sxs-lookup"><span data-stu-id="e5499-157">However, if CTEs are used as part of CETAS prepared by sp_prepare, hello behavior can defer from SQL Server and other PDW statements because of hello way binding is implemented for sp_prepare.</span></span> <span data-ttu-id="e5499-158">Válassza ki, hogy hivatkozásokat Táblakifejezés használ a megfelelő oszlop, amely nem létezik a Táblakifejezés, ha hello sp_prepare nélkül hello hiba észlelése fogja továbbítani, de hello hibát fog jelezni, sp_execute során helyette.</span><span class="sxs-lookup"><span data-stu-id="e5499-158">If SELECT that references CTE is using a wrong column that does not exist in CTE, hello sp_prepare will pass without detecting hello error, but hello error will be thrown during sp_execute instead.</span></span>

## <a name="recursive-ctes"></a><span data-ttu-id="e5499-159">Rekurzív Táblakifejezés</span><span class="sxs-lookup"><span data-stu-id="e5499-159">Recursive CTEs</span></span>
<span data-ttu-id="e5499-160">Rekurzív Táblakifejezés nem támogatottak az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e5499-160">Recursive CTEs are not supported in SQL Data Warehouse.</span></span>  <span data-ttu-id="e5499-161">hello áttelepítésének Táblakifejezés rekurzív némileg összetett feladat lehet, és legjobb folyamat hello toobreak több lépést be azt.</span><span class="sxs-lookup"><span data-stu-id="e5499-161">hello migration of recursive CTE can be somewhat complex and hello best process is toobreak it into multiple steps.</span></span> <span data-ttu-id="e5499-162">Általában hurkot használja, és egy ideiglenes tábla feltöltéséhez, akkor ismétlés hello rekurzív ideiglenes lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="e5499-162">You can typically use a loop and populate a temporary table as you iterate over hello recursive interim queries.</span></span> <span data-ttu-id="e5499-163">Miután hello ideiglenes táblából a telepítéskor majd visszatérhet hello adatok egyetlen eredményhalmaz.</span><span class="sxs-lookup"><span data-stu-id="e5499-163">Once hello temporary table is populated you can then return hello data as a single result set.</span></span> <span data-ttu-id="e5499-164">A hasonló megközelítést lett használt toosolve `GROUP BY WITH CUBE` a hello [group by záradék összesítő / adatkocka / csoportosítási készletek beállítások] [ group by clause with rollup / cube / grouping sets options] cikk.</span><span class="sxs-lookup"><span data-stu-id="e5499-164">A similar approach has been used toosolve `GROUP BY WITH CUBE` in hello [group by clause with rollup / cube / grouping sets options][group by clause with rollup / cube / grouping sets options] article.</span></span>

## <a name="unsupported-system-functions"></a><span data-ttu-id="e5499-165">A rendszer nem támogatott funkciók</span><span class="sxs-lookup"><span data-stu-id="e5499-165">Unsupported system functions</span></span>
<span data-ttu-id="e5499-166">Van még néhány rendszer funkció nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="e5499-166">There are also some system functions that are not supported.</span></span> <span data-ttu-id="e5499-167">Hello fő néhányat a meglévők közül általában előfordulhat adatraktározási használatban vannak:</span><span class="sxs-lookup"><span data-stu-id="e5499-167">Some of hello main ones you might typically find used in data warehousing are:</span></span>

* <span data-ttu-id="e5499-168">NEWSEQUENTIALID()</span><span class="sxs-lookup"><span data-stu-id="e5499-168">NEWSEQUENTIALID()</span></span>
* <span data-ttu-id="e5499-169">@@NESTLEVEL()</span><span class="sxs-lookup"><span data-stu-id="e5499-169">@@NESTLEVEL()</span></span>
* <span data-ttu-id="e5499-170">@@IDENTITY()</span><span class="sxs-lookup"><span data-stu-id="e5499-170">@@IDENTITY()</span></span>
* <span data-ttu-id="e5499-171">@@ROWCOUNT()</span><span class="sxs-lookup"><span data-stu-id="e5499-171">@@ROWCOUNT()</span></span>
* <span data-ttu-id="e5499-172">ROWCOUNT_BIG</span><span class="sxs-lookup"><span data-stu-id="e5499-172">ROWCOUNT_BIG</span></span>
* <span data-ttu-id="e5499-173">ERROR_LINE()</span><span class="sxs-lookup"><span data-stu-id="e5499-173">ERROR_LINE()</span></span>

<span data-ttu-id="e5499-174">A problémák dolgozhat körül.</span><span class="sxs-lookup"><span data-stu-id="e5499-174">Some of these issues can be worked around.</span></span>

## <a name="rowcount-workaround"></a><span data-ttu-id="e5499-175">@@ROWCOUNT megoldás</span><span class="sxs-lookup"><span data-stu-id="e5499-175">@@ROWCOUNT workaround</span></span>
<span data-ttu-id="e5499-176">@ támogatása hiánya körül toowork@ROWCOUNT, hozzon létre egy tárolt eljárás hello utolsó sorok számát lekérése sys.dm_pdw_request_steps és hajthat végre `EXEC LastRowCount` egy DML-utasítás után.</span><span class="sxs-lookup"><span data-stu-id="e5499-176">toowork around lack of support for @@ROWCOUNT, create a stored procedure that will retrieve hello last row count from sys.dm_pdw_request_steps and then execute `EXEC LastRowCount` after a DML statement.</span></span>

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a><span data-ttu-id="e5499-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5499-177">Next steps</span></span>
<span data-ttu-id="e5499-178">Az összes támogatott T-SQL-utasítások teljes listáját lásd: [Transact-SQL témakörök][Transact-SQL topics].</span><span class="sxs-lookup"><span data-stu-id="e5499-178">For a complete list of all supported T-SQL statements, see [Transact-SQL topics][Transact-SQL topics].</span></span>

<!--Image references-->

<!--Article references-->
[ANSI joins on updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI joins on deletes]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[merge statement]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[INSERT..EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL topics]: ./sql-data-warehouse-reference-tsql-statements.md

[cursors]: ./sql-data-warehouse-develop-loops.md
[group by clause with rollup / cube / grouping sets options]: ./sql-data-warehouse-develop-group-by-options.md
[nesting levels beyond 8]: ./sql-data-warehouse-develop-transactions.md
[updating through views]: ./sql-data-warehouse-develop-views.md
[use of select for variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[no MAX data type for dynamic SQL strings]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->

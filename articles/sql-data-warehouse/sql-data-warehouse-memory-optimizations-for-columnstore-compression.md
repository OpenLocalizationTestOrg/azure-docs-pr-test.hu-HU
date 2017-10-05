---
title: "Az Azure SQL oszlopcentrikus index a teljesítmény javítása |} Microsoft Docs"
description: "Csökkentse a, vagy növelje a szabad memória a maximalizálása érdekében egy oszloptárindex tömöríti az egyes sorcsoport sorok száma."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 6/2/2017
ms.author: shigu;barbkess
ms.openlocfilehash: f0e0b839b4a0c216eee2eb5134d43b91d8f83289
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a><span data-ttu-id="2dc59-103">Az oszlopcentrikus maximalizálva sorcsoport minősége</span><span class="sxs-lookup"><span data-stu-id="2dc59-103">Maximizing rowgroup quality for columnstore</span></span>

<span data-ttu-id="2dc59-104">Sorcsoport minőségi egy sorcsoport sorainak száma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2dc59-104">Rowgroup quality is determined by the number of rows in a rowgroup.</span></span> <span data-ttu-id="2dc59-105">Csökkentse a, vagy növelje a szabad memória a maximalizálása érdekében egy oszloptárindex tömöríti az egyes sorcsoport sorok száma.</span><span class="sxs-lookup"><span data-stu-id="2dc59-105">Reduce memory requirements or increase the available memory to maximize the number of rows a columnstore index compresses into each rowgroup.</span></span>  <span data-ttu-id="2dc59-106">Ezek a módszerek segítségével növelheti a tömörítési sebességet, és a lekérdezési teljesítmény az oszlopcentrikus indexek.</span><span class="sxs-lookup"><span data-stu-id="2dc59-106">Use these methods to improve compression rates and query performance for columnstore indexes.</span></span>

## <a name="why-the-rowgroup-size-matters"></a><span data-ttu-id="2dc59-107">A sorcsoport mérete fontossága</span><span class="sxs-lookup"><span data-stu-id="2dc59-107">Why the rowgroup size matters</span></span>
<span data-ttu-id="2dc59-108">Mert oszlopcentrikus index táblázat egyedi rowgroups oszlop szegmenseinek beolvasásával ellenőrzi, maximalizálva minden sorcsoport sorainak száma fokozza a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="2dc59-108">Since a columnstore index scans a table by scanning column segments of individual rowgroups, maximizing the number of rows in each rowgroup enhances query performance.</span></span> <span data-ttu-id="2dc59-109">Ha rowgroups nagy mennyiségű sort, adattömörítés javítja, ami azt jelenti, hogy kevesebb adat, olvassa el a lemezről.</span><span class="sxs-lookup"><span data-stu-id="2dc59-109">When rowgroups have a high number of rows, data compression improves which means there is less data to read from disk.</span></span>

<span data-ttu-id="2dc59-110">Rowgroups kapcsolatos további információkért lásd: [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="2dc59-110">For more information about rowgroups, see [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

## <a name="target-size-for-rowgroups"></a><span data-ttu-id="2dc59-111">Rowgroups célméretet</span><span class="sxs-lookup"><span data-stu-id="2dc59-111">Target size for rowgroups</span></span>
<span data-ttu-id="2dc59-112">A legjobb lekérdezési teljesítmény a célja a kötegenkénti sorok száma az oszlopcentrikus index sorcsoport maximalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="2dc59-112">For best query performance, the goal is to maximize the number of rows per rowgroup in a columnstore index.</span></span> <span data-ttu-id="2dc59-113">Egy sorcsoport legfeljebb 1 048 576 sort is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="2dc59-113">A rowgroup can have a maximum of 1,048,576 rows.</span></span> <span data-ttu-id="2dc59-114">Már rendelkezik egy sorcsoport sorok maximális számát.</span><span class="sxs-lookup"><span data-stu-id="2dc59-114">It's okay to not have the maximum number of rows per rowgroup.</span></span> <span data-ttu-id="2dc59-115">Oszloptárindexek a megfelelő teljesítmény elérése érdekében, ha rowgroups legalább 100 000 sort.</span><span class="sxs-lookup"><span data-stu-id="2dc59-115">Columnstore indexes achieve good performance when rowgroups have at least 100,000 rows.</span></span>

## <a name="rowgroups-can-get-trimmed-during-compression"></a><span data-ttu-id="2dc59-116">Rowgroups is beolvasása levágja a tömörítés során</span><span class="sxs-lookup"><span data-stu-id="2dc59-116">Rowgroups can get trimmed during compression</span></span>

<span data-ttu-id="2dc59-117">A tömeges betöltés vagy oszlopcentrikus index rebuild, során néha nincs elég memória érhető el az egyes sorcsoport kijelölt összes sort tömörítése.</span><span class="sxs-lookup"><span data-stu-id="2dc59-117">During a bulk load or columnstore index rebuild, sometimes there isn't enough memory available to compress all the rows designated for each rowgroup.</span></span> <span data-ttu-id="2dc59-118">Memóriaprobléma esetén oszlopcentrikus indexek sorcsoport mérete vághatja a, az oszloptárindexet a tömörítés sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="2dc59-118">When there is memory pressure, columnstore indexes trim the rowgroup sizes so compression into the columnstore can succeed.</span></span> 

<span data-ttu-id="2dc59-119">Ha nincs elég memória a legalább 10 000 sorok tömörítése minden sorcsoport, az SQL Data Warehouse hibát generál.</span><span class="sxs-lookup"><span data-stu-id="2dc59-119">When there is insufficient memory to compress at least 10,000 rows into each rowgroup, SQL Data Warehouse generates an error.</span></span>

<span data-ttu-id="2dc59-120">További információ a tömeges betöltés: [fürtözött oszlopcentrikus index a tömeges betöltés](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span><span class="sxs-lookup"><span data-stu-id="2dc59-120">For more information on bulk loading, see [Bulk load into a clustered columnstore index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span></span>

## <a name="how-to-monitor-rowgroup-quality"></a><span data-ttu-id="2dc59-121">Sorcsoport minőségi figyelése</span><span class="sxs-lookup"><span data-stu-id="2dc59-121">How to monitor rowgroup quality</span></span>

<span data-ttu-id="2dc59-122">Nincs DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats), amely elérhetővé teszi a hasznos információk, például a rowgroups, és ha nem lett díszítésre levágási azért sorainak száma.</span><span class="sxs-lookup"><span data-stu-id="2dc59-122">There is a DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) that exposes useful information such as number of rows in rowgroups and the reason for trimming if there was trimming.</span></span> <span data-ttu-id="2dc59-123">A következő nézetet, hogy a lekérdezés sorcsoport levágási információt lekérni a DMV szerint hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2dc59-123">You can create the following view as a handy way to query this DMV to get information on rowgroup trimming.</span></span>

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[distribution_id]    = nt.[distribution_id]                                          
)
select *
from cte;
```

<span data-ttu-id="2dc59-124">A trim_reason_desc azt ismerteti, hogy a sorcsoport lett rövidített (trim_reason_desc = NO_TRIM azt jelenti, nem tisztítás történt, és sorcsoport optimális minőségű).</span><span class="sxs-lookup"><span data-stu-id="2dc59-124">The trim_reason_desc tells whether the rowgroup was trimmed(trim_reason_desc = NO_TRIM implies there was no trimming and row group is of optimal quality).</span></span> <span data-ttu-id="2dc59-125">A következő vágással kapcsolatos okok miatt jelzi, hogy a sorcsoport korai karakterek:</span><span class="sxs-lookup"><span data-stu-id="2dc59-125">The following trim reasons indicate premature trimming of the rowgroup:</span></span>
- <span data-ttu-id="2dc59-126">BULKLOAD: A vágás ezért használatos, ha a bejövő a betöltési-sorok kötegét kisebb, mint 1 millió sort foglalnak volt.</span><span class="sxs-lookup"><span data-stu-id="2dc59-126">BULKLOAD: This trim reason is used when the incoming batch of rows for the load had less than 1 million rows.</span></span> <span data-ttu-id="2dc59-127">A motor tömörített sorcsoportok hoz létre, ha nincsenek a nagyobb, mint 100 000 sort beszúrni (ellentétben a különbözeti tároló beszúrása), de a vágás OK BULKLOAD értékűre állítja be.</span><span class="sxs-lookup"><span data-stu-id="2dc59-127">The engine will create compressed row groups if there are greater than 100,000 rows being inserted (as opposed to inserting into the delta store) but sets the trim reason to BULKLOAD.</span></span> <span data-ttu-id="2dc59-128">Ebben a forgatókönyvben, érdemes emelni a kötegelt betöltés ablak gyűlik össze a további sorokat.</span><span class="sxs-lookup"><span data-stu-id="2dc59-128">In this scenario, consider increasing your batch load window to accumulate more rows.</span></span> <span data-ttu-id="2dc59-129">Ezenkívül újra kiértékelje a particionálási sémát, és ellenőrizze, hogy nincs túl részletes, sorcsoportok partícióhatárok nem terjedhet ki.</span><span class="sxs-lookup"><span data-stu-id="2dc59-129">Also, reevaluate your partitioning scheme to ensure it is not too granular as row groups cannot span partition boundaries.</span></span>
- <span data-ttu-id="2dc59-130">MEMORY_LIMITATION: 1 millió sort foglalnak sorcsoportok létrehozásához, egy bizonyos működő szükséges memória mérete van a motor.</span><span class="sxs-lookup"><span data-stu-id="2dc59-130">MEMORY_LIMITATION: To create row groups with 1 million rows, a certain amount of working memory is required by the engine.</span></span> <span data-ttu-id="2dc59-131">Amikor a rendelkezésre álló memória, a betöltés munkamenet nem éri el a szükséges munkát memóriával, sorcsoportok túl korán beolvasása levágja.</span><span class="sxs-lookup"><span data-stu-id="2dc59-131">When available memory of the loading session is less than the required working memory, row groups get prematurely trimmed.</span></span> <span data-ttu-id="2dc59-132">Az alábbi szakaszok ismertetik, hogyan becsléséhez memóriára van szükség, és több memóriát lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="2dc59-132">The following sections explain how to estimate memory required and allocate more memory.</span></span>
- <span data-ttu-id="2dc59-133">DICTIONARY_SIZE: Vágás ezért azt jelzi, hogy sorcsoport tisztítás történt, mert legalább egy karakterlánc-oszlopnak széles és/vagy nagy számosságot karakterláncok történt.</span><span class="sxs-lookup"><span data-stu-id="2dc59-133">DICTIONARY_SIZE: This trim reason indicates that rowgroup trimming occurred because there was at least one string column with wide and/or high cardinality strings.</span></span> <span data-ttu-id="2dc59-134">A szótár mérete legfeljebb 16 MB memória, és ez a korlát elérésekor a sorcsoport tömörített.</span><span class="sxs-lookup"><span data-stu-id="2dc59-134">The dictionary size is limited to 16 MB in memory and once this limit is reached the row group is compressed.</span></span> <span data-ttu-id="2dc59-135">Ha ez a helyzet tapasztal, fontolja meg, a problematikus oszlop azoknak egy külön táblába.</span><span class="sxs-lookup"><span data-stu-id="2dc59-135">If you do run into this situation, consider isolating the problematic column into a separate table.</span></span>

## <a name="how-to-estimate-memory-requirements"></a><span data-ttu-id="2dc59-136">Hogyan memória követelményeinek becslése</span><span class="sxs-lookup"><span data-stu-id="2dc59-136">How to estimate memory requirements</span></span>

<!--
To view an estimate of the memory requirements to compress a rowgroup of maximum size into a columnstore index, download and run the view [dbo.vCS_mon_mem_grant](). This view shows the size of the memory grant that a rowgroup requires for compression in to the columnstore.
-->

<span data-ttu-id="2dc59-137">A maximális szükséges memória egy sorcsoport tömörítendő körülbelül van</span><span class="sxs-lookup"><span data-stu-id="2dc59-137">The maximum required memory to compress one rowgroup is approximately</span></span>

- <span data-ttu-id="2dc59-138">72 MB +</span><span class="sxs-lookup"><span data-stu-id="2dc59-138">72 MB +</span></span>
- <span data-ttu-id="2dc59-139">\#sorok \* \#oszlopok \* 8 bájt +</span><span class="sxs-lookup"><span data-stu-id="2dc59-139">\#rows \* \#columns \* 8 bytes +</span></span>
- <span data-ttu-id="2dc59-140">\#sorok \* \#rövid-karakterlánc-oszlopok \* 32 bájt +</span><span class="sxs-lookup"><span data-stu-id="2dc59-140">\#rows \* \#short-string-columns \* 32 bytes +</span></span>
- <span data-ttu-id="2dc59-141">\#hosszú-karakterlánc-oszlopok \* tömörítési 16 MB</span><span class="sxs-lookup"><span data-stu-id="2dc59-141">\#long-string-columns \* 16 MB for compression dictionary</span></span>

<span data-ttu-id="2dc59-142">Ha rövid-karakterlánc-oszlopok használnak, a karakterláncos adattípusokkal < = 32 bájt és karakterláncos adattípusokkal hosszú-karakterlánc-oszlopok használata > 32 bájtban kifejezve.</span><span class="sxs-lookup"><span data-stu-id="2dc59-142">where short-string-columns use string data types of <= 32 bytes and long-string-columns use string data types of > 32 bytes.</span></span>

<span data-ttu-id="2dc59-143">Hosszú karakterláncok szöveges tömörítés készült tömörítési módszer tömörített.</span><span class="sxs-lookup"><span data-stu-id="2dc59-143">Long strings are compressed with a compression method designed for compressing text.</span></span> <span data-ttu-id="2dc59-144">Ez a tömörítés módszer egy *szótár* szövegminták tárolásához.</span><span class="sxs-lookup"><span data-stu-id="2dc59-144">This compression method uses a *dictionary* to store text patterns.</span></span> <span data-ttu-id="2dc59-145">A szótár maximális mérete 16 MB.</span><span class="sxs-lookup"><span data-stu-id="2dc59-145">The maximum size of a dictionary is 16 MB.</span></span> <span data-ttu-id="2dc59-146">Nincs a sorcsoport hosszú karakterlánc oszlopainak csak egy szótár.</span><span class="sxs-lookup"><span data-stu-id="2dc59-146">There is only one dictionary for each long string column in the rowgroup.</span></span>

<span data-ttu-id="2dc59-147">Részletes ismertető oszlopcentrikus memória követelmények, lásd: a videó [Azure SQL Data Warehouse skálázás: útmutató és konfigurációs](https://myignite.microsoft.com/videos/14822).</span><span class="sxs-lookup"><span data-stu-id="2dc59-147">For an in-depth discussion of columnstore memory requirements, see the video [Azure SQL Data Warehouse scaling: configuration and guidance](https://myignite.microsoft.com/videos/14822).</span></span>

## <a name="ways-to-reduce-memory-requirements"></a><span data-ttu-id="2dc59-148">Csökkentse a módjai</span><span class="sxs-lookup"><span data-stu-id="2dc59-148">Ways to reduce memory requirements</span></span>

<span data-ttu-id="2dc59-149">A következő eljárások használatával rowgroups tömörítés az oszlopcentrikus indexek memóriakövetelményei csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="2dc59-149">Use the following techniques to reduce the memory requirements for compressing rowgroups into columnstore indexes.</span></span>

### <a name="use-fewer-columns"></a><span data-ttu-id="2dc59-150">Oszlopok</span><span class="sxs-lookup"><span data-stu-id="2dc59-150">Use fewer columns</span></span>
<span data-ttu-id="2dc59-151">Ha lehetséges tervezze meg kevesebb oszlopot tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="2dc59-151">If possible, design the table with fewer columns.</span></span> <span data-ttu-id="2dc59-152">Amikor egy sorcsoport az oszloptárindexet a tömörített, az oszloptárindexet külön-külön tömöríti minden oszlop szegmensben.</span><span class="sxs-lookup"><span data-stu-id="2dc59-152">When a rowgroup is compressed into the columnstore, the columnstore index compresses each column segment separately.</span></span> <span data-ttu-id="2dc59-153">Ezért a egy sorcsoport tömörítését memóriakövetelményeknek oszlopok növeli a számának növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2dc59-153">Therefore the memory requirements to compress a rowgroup increase as the number of columns increases.</span></span>


### <a name="use-fewer-string-columns"></a><span data-ttu-id="2dc59-154">Oszlopok karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2dc59-154">Use fewer string columns</span></span>
<span data-ttu-id="2dc59-155">String adattípusú oszlopok numerikus-nál több memóriát és dátum adattípus van szükség.</span><span class="sxs-lookup"><span data-stu-id="2dc59-155">Columns of string data types require more memory than numeric and date data types.</span></span> <span data-ttu-id="2dc59-156">Csökkentse a, fontolja meg a karakterlánc típusú oszlopokra eltávolítása a ténytáblák és abba a kisebb dimenziótáblák.</span><span class="sxs-lookup"><span data-stu-id="2dc59-156">To reduce memory requirements, consider removing string columns from fact tables and putting them in smaller dimension tables.</span></span>

<span data-ttu-id="2dc59-157">További memóriára vonatkozó követelményeknek karakterlánc tömörítési:</span><span class="sxs-lookup"><span data-stu-id="2dc59-157">Additional memory requirements for string compression:</span></span>

- <span data-ttu-id="2dc59-158">Legfeljebb 32 karakter hosszúságú karakterlánc adattípusok 32 további bájt értéke lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="2dc59-158">String data types up to 32 characters can require 32 additional bytes per value.</span></span>
- <span data-ttu-id="2dc59-159">Karakterláncos adattípusokkal legfeljebb 32 karakter hosszú lehet a tömörített szótár módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="2dc59-159">String data types with more than 32 characters are compressed using dictionary methods.</span></span>  <span data-ttu-id="2dc59-160">A szótár létrehozásához minden egyes oszlopának a sorcsoport akár további 16 MB lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="2dc59-160">Each column in the rowgroup can require up to an additional 16 MB to build the dictionary.</span></span>

### <a name="avoid-over-partitioning"></a><span data-ttu-id="2dc59-161">Kerülje a túlzott particionálás</span><span class="sxs-lookup"><span data-stu-id="2dc59-161">Avoid over-partitioning</span></span>

<span data-ttu-id="2dc59-162">Oszlopcentrikus indexek létrehozása egy vagy több rowgroups partíciónként.</span><span class="sxs-lookup"><span data-stu-id="2dc59-162">Columnstore indexes create one or more rowgroups per partition.</span></span> <span data-ttu-id="2dc59-163">Az SQL Data Warehouse a partíciók száma nő gyorsan, mert az adatok terjeszt, és minden egyes terjesztési particionálva van.</span><span class="sxs-lookup"><span data-stu-id="2dc59-163">In SQL Data Warehouse, the number of partitions grows quickly because the data is distributed and each distribution is partitioned.</span></span> <span data-ttu-id="2dc59-164">Ha a tábla túl sok partíciókkal rendelkezik, a nem feltétlenül a rowgroups adatlehívást.</span><span class="sxs-lookup"><span data-stu-id="2dc59-164">If the table has too many partitions, there might not be enough rows to fill the rowgroups.</span></span> <span data-ttu-id="2dc59-165">Sorok hiánya nem hoz létre Memóriaterhelést tömörítés során azonban, hogy ne használjon a legjobb oszlopcentrikus lekérdezési teljesítmény rowgroups vezet.</span><span class="sxs-lookup"><span data-stu-id="2dc59-165">The lack of rows does not create memory pressure during compression, but it leads to rowgroups that do not achieve the best columnstore query performance.</span></span>

<span data-ttu-id="2dc59-166">Egy másik túlzott particionálási elkerülése érdekében szükség esetén a particionált tábla oszlopcentrikus index a sorok betöltése a memória.</span><span class="sxs-lookup"><span data-stu-id="2dc59-166">Another reason to avoid over-partitioning is there is a memory overhead for loading rows into a columnstore index on a partitioned table.</span></span> <span data-ttu-id="2dc59-167">A betöltés során sok partíciót kap a bejövő sorait, amíg mindegyik partíció rendelkezik elég sorok tömöríthetők tartják a memóriában.</span><span class="sxs-lookup"><span data-stu-id="2dc59-167">During a load, many partitions could receive the incoming rows, which are held in memory until each partition has enough rows to be compressed.</span></span> <span data-ttu-id="2dc59-168">További memória rendelkező túl sok partíciót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2dc59-168">Having too many partitions creates additional memory pressure.</span></span>

### <a name="simplify-the-load-query"></a><span data-ttu-id="2dc59-169">A betöltési lekérdezés egyszerűsítése érdekében</span><span class="sxs-lookup"><span data-stu-id="2dc59-169">Simplify the load query</span></span>

<span data-ttu-id="2dc59-170">Az adatbázis között a lekérdezésben az operátorok lekérdezés memóriabeli ideiglenes megosztja.</span><span class="sxs-lookup"><span data-stu-id="2dc59-170">The database shares the memory grant for a query among all the operators in the query.</span></span> <span data-ttu-id="2dc59-171">Ha egy terhelés-lekérdezés összetett típusú és illesztések rendelkezik, a memória a tömörítési csökken.</span><span class="sxs-lookup"><span data-stu-id="2dc59-171">When a load query has complex sorts and joins, the memory available for compression is reduced.</span></span>

<span data-ttu-id="2dc59-172">Tervezze meg a betöltési lekérdezés csak összpontosíthat betöltené a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="2dc59-172">Design the load query to focus only on loading the query.</span></span> <span data-ttu-id="2dc59-173">Az adatok átalakítások futtatásához szükséges, ha futtatásukhoz külön, a Betöltés lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="2dc59-173">If you need to run transformations on the data, run them separate from the load query.</span></span> <span data-ttu-id="2dc59-174">Például tesztelése a halommemória táblákban tárolt adatokat, az átalakítás futtatásához és az átmeneti tárolási tábla majd betölthető az oszlopcentrikus indexet.</span><span class="sxs-lookup"><span data-stu-id="2dc59-174">For example, stage the data in a heap table, run the transformations, and then load the staging table into the columnstore index.</span></span> <span data-ttu-id="2dc59-175">Először is az adatok betöltésére, és az MPP rendszer segítségével átalakíthatja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="2dc59-175">You can also load the data first and then use the MPP system to transform the data.</span></span>

### <a name="adjust-maxdop"></a><span data-ttu-id="2dc59-176">MAXDOP beállítása</span><span class="sxs-lookup"><span data-stu-id="2dc59-176">Adjust MAXDOP</span></span>

<span data-ttu-id="2dc59-177">Minden terjesztési a rowgroups történő párhuzamos oszlopcentrikus tömöríti, ha egynél több Processzormagok eloszlása érhető el.</span><span class="sxs-lookup"><span data-stu-id="2dc59-177">Each distribution compresses rowgroups into the columnstore in parallel when there is more than one CPU core available per distribution.</span></span> <span data-ttu-id="2dc59-178">A párhuzamos végrehajtás nagyobb mennyiségű memóriát használ, ami Memóriaterhelést és sorcsoport levágási igényel.</span><span class="sxs-lookup"><span data-stu-id="2dc59-178">The parallelism requires additional memory resources, which can lead to memory pressure and rowgroup trimming.</span></span>

<span data-ttu-id="2dc59-179">Memória csökkentése érdekében a MAXDOP lekérdezés mutató segítségével a load műveletet belül minden terjesztési soros módban való futásra kényszeríti.</span><span class="sxs-lookup"><span data-stu-id="2dc59-179">To reduce memory pressure, you can use the MAXDOP query hint to force the load operation to run in serial mode within each distribution.</span></span>

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a><span data-ttu-id="2dc59-180">Több memóriát módjai</span><span class="sxs-lookup"><span data-stu-id="2dc59-180">Ways to allocate more memory</span></span>

<span data-ttu-id="2dc59-181">DWU mérete és a felhasználó erőforrásosztály együtt határozza meg, mennyi memória érhető el a felhasználó lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="2dc59-181">DWU size and the user resource class together determine how much memory is available for a user query.</span></span> <span data-ttu-id="2dc59-182">A memóriabeli ideiglenes terhelés lekérdezés növeléséhez dwu-k számának növelése, vagy növelje a erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="2dc59-182">To increase the memory grant for a load query, you can either increase the number of DWUs or increase the resource class.</span></span>

- <span data-ttu-id="2dc59-183">A dwu-k növelése érdekében tekintse meg a [hogyan méretezhető teljesítmény?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span><span class="sxs-lookup"><span data-stu-id="2dc59-183">To increase the DWUs, see [How do I scale performance?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span></span>
- <span data-ttu-id="2dc59-184">A lekérdezés erőforrásosztály módosításához lásd [módosíthatja a felhasználói erőforrás osztály példa](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="2dc59-184">To change the resource class for a query, see [Change a user resource class example](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

<span data-ttu-id="2dc59-185">Például a DWU 100 smallrc erőforrásosztály felhasználójának használhatja 100 MB memóriát minden egyes terjesztési.</span><span class="sxs-lookup"><span data-stu-id="2dc59-185">For example, on DWU 100 a user in the smallrc resource class can use 100 MB of memory for each distribution.</span></span> <span data-ttu-id="2dc59-186">A részletekért lásd: [az SQL Data Warehouse párhuzamossági](sql-data-warehouse-develop-concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="2dc59-186">For the details, see [Concurrency in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span></span>

<span data-ttu-id="2dc59-187">Tegyük fel, hogy van-e 700 MB memória kiváló minőségű sorcsoport méretek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2dc59-187">Suppose you determine that you need 700 MB of memory to get high-quality rowgroup sizes.</span></span> <span data-ttu-id="2dc59-188">A példákból látható, hogyan futtathat a terhelés lekérdezés elegendő memóriával.</span><span class="sxs-lookup"><span data-stu-id="2dc59-188">These examples show how you can run the load query with enough memory.</span></span>

- <span data-ttu-id="2dc59-189">A DWU 1000 és mediumrc, a memóriabeli ideiglenes 800 MB</span><span class="sxs-lookup"><span data-stu-id="2dc59-189">Using DWU 1000 and mediumrc, your memory grant is 800 MB</span></span>
- <span data-ttu-id="2dc59-190">Használja a DWU 600 és largerc, a memóriabeli ideiglenes tárat, 800 MB.</span><span class="sxs-lookup"><span data-stu-id="2dc59-190">Using DWU 600 and largerc, your memory grant is 800 MB.</span></span>


## <a name="next-steps"></a><span data-ttu-id="2dc59-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2dc59-191">Next steps</span></span>

<span data-ttu-id="2dc59-192">Az SQL Data Warehouse teljesítmény javítása érdekében további részleteket talál a [teljesítményének áttekintését](sql-data-warehouse-overview-manage-user-queries.md).</span><span class="sxs-lookup"><span data-stu-id="2dc59-192">To find more ways to improve performance in SQL Data Warehouse, see the [Performance overview](sql-data-warehouse-overview-manage-user-queries.md).</span></span>

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->

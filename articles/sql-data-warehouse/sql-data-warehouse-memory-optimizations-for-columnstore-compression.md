---
title: "aaaImprove oszlopcentrikus index teljesítmény az Azure SQL |} Microsoft Docs"
description: "Csökkentse a, vagy az oszlopcentrikus index tömöríti az egyes sorcsoport sorok hello rendelkezésre álló memória toomaximize hello számának növeléséhez."
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
ms.openlocfilehash: 2c5a68435aa200236a2dc8538aa4638b52a59093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a><span data-ttu-id="ba544-103">Az oszlopcentrikus maximalizálva sorcsoport minősége</span><span class="sxs-lookup"><span data-stu-id="ba544-103">Maximizing rowgroup quality for columnstore</span></span>

<span data-ttu-id="ba544-104">Sorcsoport minőségi hello egy sorcsoport sorainak száma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ba544-104">Rowgroup quality is determined by hello number of rows in a rowgroup.</span></span> <span data-ttu-id="ba544-105">Csökkentse a, vagy az oszlopcentrikus index tömöríti az egyes sorcsoport sorok hello rendelkezésre álló memória toomaximize hello számának növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="ba544-105">Reduce memory requirements or increase hello available memory toomaximize hello number of rows a columnstore index compresses into each rowgroup.</span></span>  <span data-ttu-id="ba544-106">Használja az ezen módszerek tooimprove tömörítési sebességét és a lekérdezési teljesítmény az oszlopcentrikus indexek.</span><span class="sxs-lookup"><span data-stu-id="ba544-106">Use these methods tooimprove compression rates and query performance for columnstore indexes.</span></span>

## <a name="why-hello-rowgroup-size-matters"></a><span data-ttu-id="ba544-107">Hello sorcsoport mérete fontossága</span><span class="sxs-lookup"><span data-stu-id="ba544-107">Why hello rowgroup size matters</span></span>
<span data-ttu-id="ba544-108">Mert oszlopcentrikus index táblázat egyedi rowgroups oszlop szegmenseinek beolvasásával ellenőrzi, maximalizálva hello minden sorcsoport sorainak száma fokozza a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="ba544-108">Since a columnstore index scans a table by scanning column segments of individual rowgroups, maximizing hello number of rows in each rowgroup enhances query performance.</span></span> <span data-ttu-id="ba544-109">Ha rowgroups nagy mennyiségű sort, adattömörítés javítja, ami azt jelenti, hogy a kevesebb adat tooread lemezről van.</span><span class="sxs-lookup"><span data-stu-id="ba544-109">When rowgroups have a high number of rows, data compression improves which means there is less data tooread from disk.</span></span>

<span data-ttu-id="ba544-110">Rowgroups kapcsolatos további információkért lásd: [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba544-110">For more information about rowgroups, see [Columnstore Indexes Guide](https://msdn.microsoft.com/library/gg492088.aspx).</span></span>

## <a name="target-size-for-rowgroups"></a><span data-ttu-id="ba544-111">Rowgroups célméretet</span><span class="sxs-lookup"><span data-stu-id="ba544-111">Target size for rowgroups</span></span>
<span data-ttu-id="ba544-112">A legjobb lekérdezési teljesítmény elérése érdekében a hello célja toomaximize hello kötegenkénti sorok száma az oszlopcentrikus index sorcsoport.</span><span class="sxs-lookup"><span data-stu-id="ba544-112">For best query performance, hello goal is toomaximize hello number of rows per rowgroup in a columnstore index.</span></span> <span data-ttu-id="ba544-113">Egy sorcsoport legfeljebb 1 048 576 sort is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="ba544-113">A rowgroup can have a maximum of 1,048,576 rows.</span></span> <span data-ttu-id="ba544-114">Annak ellenére toonot rendelkezik hello / sorcsoport sorok maximális számát.</span><span class="sxs-lookup"><span data-stu-id="ba544-114">It's okay toonot have hello maximum number of rows per rowgroup.</span></span> <span data-ttu-id="ba544-115">Oszloptárindexek a megfelelő teljesítmény elérése érdekében, ha rowgroups legalább 100 000 sort.</span><span class="sxs-lookup"><span data-stu-id="ba544-115">Columnstore indexes achieve good performance when rowgroups have at least 100,000 rows.</span></span>

## <a name="rowgroups-can-get-trimmed-during-compression"></a><span data-ttu-id="ba544-116">Rowgroups is beolvasása levágja a tömörítés során</span><span class="sxs-lookup"><span data-stu-id="ba544-116">Rowgroups can get trimmed during compression</span></span>

<span data-ttu-id="ba544-117">A tömeges betöltés vagy oszlopcentrikus index rebuild, során néha nem áll rendelkezésre elegendő memória rendelkezésre álló toocompress minden egyes sorcsoport kijelölt sorok hello.</span><span class="sxs-lookup"><span data-stu-id="ba544-117">During a bulk load or columnstore index rebuild, sometimes there isn't enough memory available toocompress all hello rows designated for each rowgroup.</span></span> <span data-ttu-id="ba544-118">Memóriaprobléma esetén, oszlopcentrikus indexek hello sorcsoport méretek vághatja a, tömörítés hello oszlopcentrikus való sikeres lehet.</span><span class="sxs-lookup"><span data-stu-id="ba544-118">When there is memory pressure, columnstore indexes trim hello rowgroup sizes so compression into hello columnstore can succeed.</span></span> 

<span data-ttu-id="ba544-119">Ha nincs elegendő memória toocompress legalább 10 000 sorokat minden sorcsoport, az SQL Data Warehouse hibát generál.</span><span class="sxs-lookup"><span data-stu-id="ba544-119">When there is insufficient memory toocompress at least 10,000 rows into each rowgroup, SQL Data Warehouse generates an error.</span></span>

<span data-ttu-id="ba544-120">További információ a tömeges betöltés: [fürtözött oszlopcentrikus index a tömeges betöltés](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span><span class="sxs-lookup"><span data-stu-id="ba544-120">For more information on bulk loading, see [Bulk load into a clustered columnstore index](https://msdn.microsoft.com/en-us/library/dn935008.aspx#Bulk load into a clustered columnstore index).</span></span>

## <a name="how-toomonitor-rowgroup-quality"></a><span data-ttu-id="ba544-121">Hogyan toomonitor sorcsoport minősége</span><span class="sxs-lookup"><span data-stu-id="ba544-121">How toomonitor rowgroup quality</span></span>

<span data-ttu-id="ba544-122">Nincs a hasznos információk, például a tisztítás rowgroups és hello okát a sorok számát mutatja, ha nem lett díszítésre DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats).</span><span class="sxs-lookup"><span data-stu-id="ba544-122">There is a DMV (sys.dm_pdw_nodes_db_column_store_row_group_physical_stats) that exposes useful information such as number of rows in rowgroups and hello reason for trimming if there was trimming.</span></span> <span data-ttu-id="ba544-123">Létrehozhat egy hasznos tooquery nézetként következő hello a DMV tooget információk sorcsoport tisztítás.</span><span class="sxs-lookup"><span data-stu-id="ba544-123">You can create hello following view as a handy way tooquery this DMV tooget information on rowgroup trimming.</span></span>

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

<span data-ttu-id="ba544-124">hello trim_reason_desc közli, hogy hello sorcsoport lett rövidített (trim_reason_desc = NO_TRIM azt jelenti, nem tisztítás történt, és sorcsoport optimális minőségű).</span><span class="sxs-lookup"><span data-stu-id="ba544-124">hello trim_reason_desc tells whether hello rowgroup was trimmed(trim_reason_desc = NO_TRIM implies there was no trimming and row group is of optimal quality).</span></span> <span data-ttu-id="ba544-125">hello következő vágással kapcsolatos okok miatt jelzi, hogy hello sorcsoport korai karakterek:</span><span class="sxs-lookup"><span data-stu-id="ba544-125">hello following trim reasons indicate premature trimming of hello rowgroup:</span></span>
- <span data-ttu-id="ba544-126">BULKLOAD: Vágás ezért használatos hello bejövő hello terhelést-sorok kötegét kisebb, mint 1 millió sort foglalnak volna.</span><span class="sxs-lookup"><span data-stu-id="ba544-126">BULKLOAD: This trim reason is used when hello incoming batch of rows for hello load had less than 1 million rows.</span></span> <span data-ttu-id="ba544-127">hello motor tömörített sorcsoportok hoz létre, ha nagyobb, mint 100 000 sort beszúrni (a megakadályozását tooinserting hello különbözeti tárolóba), de beállítása hello vágás OK tooBULKLOAD.</span><span class="sxs-lookup"><span data-stu-id="ba544-127">hello engine will create compressed row groups if there are greater than 100,000 rows being inserted (as opposed tooinserting into hello delta store) but sets hello trim reason tooBULKLOAD.</span></span> <span data-ttu-id="ba544-128">Ebben az esetben érdemes megfontolni a kötegelt betöltés ablak tooaccumulate több sort.</span><span class="sxs-lookup"><span data-stu-id="ba544-128">In this scenario, consider increasing your batch load window tooaccumulate more rows.</span></span> <span data-ttu-id="ba544-129">Ezenkívül újra kiértékelje a particionálási sémát tooensure nincs túl részletes, sorcsoportok partícióhatárok nem terjedhet ki.</span><span class="sxs-lookup"><span data-stu-id="ba544-129">Also, reevaluate your partitioning scheme tooensure it is not too granular as row groups cannot span partition boundaries.</span></span>
- <span data-ttu-id="ba544-130">MEMORY_LIMITATION: hello motor toocreate sorcsoportok az 1 millió sort foglalnak, bizonyos mennyiségű működő memória szükséges.</span><span class="sxs-lookup"><span data-stu-id="ba544-130">MEMORY_LIMITATION: toocreate row groups with 1 million rows, a certain amount of working memory is required by hello engine.</span></span> <span data-ttu-id="ba544-131">Hello munkamenet betöltése a rendelkezésre álló memória értéke kisebb, mint a hello szükséges memória használata, ha a get túl korán rövidített sorcsoportok.</span><span class="sxs-lookup"><span data-stu-id="ba544-131">When available memory of hello loading session is less than hello required working memory, row groups get prematurely trimmed.</span></span> <span data-ttu-id="ba544-132">hello a következő szakaszok azt ismertetik, hogyan tooestimate memória szükséges, és több memóriát lefoglalni.</span><span class="sxs-lookup"><span data-stu-id="ba544-132">hello following sections explain how tooestimate memory required and allocate more memory.</span></span>
- <span data-ttu-id="ba544-133">DICTIONARY_SIZE: Vágás ezért azt jelzi, hogy sorcsoport tisztítás történt, mert legalább egy karakterlánc-oszlopnak széles és/vagy nagy számosságot karakterláncok történt.</span><span class="sxs-lookup"><span data-stu-id="ba544-133">DICTIONARY_SIZE: This trim reason indicates that rowgroup trimming occurred because there was at least one string column with wide and/or high cardinality strings.</span></span> <span data-ttu-id="ba544-134">hello szótár mérete MB memória, és ez a korlát elérésekor hello sorcsoport tömörített korlátozott too16.</span><span class="sxs-lookup"><span data-stu-id="ba544-134">hello dictionary size is limited too16 MB in memory and once this limit is reached hello row group is compressed.</span></span> <span data-ttu-id="ba544-135">Ha ez a helyzet tapasztal, fontolja meg, hello problematikus oszlop azoknak egy külön táblába.</span><span class="sxs-lookup"><span data-stu-id="ba544-135">If you do run into this situation, consider isolating hello problematic column into a separate table.</span></span>

## <a name="how-tooestimate-memory-requirements"></a><span data-ttu-id="ba544-136">Hogyan tooestimate memóriára vonatkozó követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="ba544-136">How tooestimate memory requirements</span></span>

<!--
tooview an estimate of hello memory requirements toocompress a rowgroup of maximum size into a columnstore index, download and run hello view [dbo.vCS_mon_mem_grant](). This view shows hello size of hello memory grant that a rowgroup requires for compression in toohello columnstore.
-->

<span data-ttu-id="ba544-137">hello maximális szükséges memória toocompress egy sorcsoport körülbelül van</span><span class="sxs-lookup"><span data-stu-id="ba544-137">hello maximum required memory toocompress one rowgroup is approximately</span></span>

- <span data-ttu-id="ba544-138">72 MB +</span><span class="sxs-lookup"><span data-stu-id="ba544-138">72 MB +</span></span>
- <span data-ttu-id="ba544-139">\#sorok \* \#oszlopok \* 8 bájt +</span><span class="sxs-lookup"><span data-stu-id="ba544-139">\#rows \* \#columns \* 8 bytes +</span></span>
- <span data-ttu-id="ba544-140">\#sorok \* \#rövid-karakterlánc-oszlopok \* 32 bájt +</span><span class="sxs-lookup"><span data-stu-id="ba544-140">\#rows \* \#short-string-columns \* 32 bytes +</span></span>
- <span data-ttu-id="ba544-141">\#hosszú-karakterlánc-oszlopok \* tömörítési 16 MB</span><span class="sxs-lookup"><span data-stu-id="ba544-141">\#long-string-columns \* 16 MB for compression dictionary</span></span>

<span data-ttu-id="ba544-142">Ha rövid-karakterlánc-oszlopok használnak, a karakterláncos adattípusokkal < = 32 bájt és karakterláncos adattípusokkal hosszú-karakterlánc-oszlopok használata > 32 bájtban kifejezve.</span><span class="sxs-lookup"><span data-stu-id="ba544-142">where short-string-columns use string data types of <= 32 bytes and long-string-columns use string data types of > 32 bytes.</span></span>

<span data-ttu-id="ba544-143">Hosszú karakterláncok szöveges tömörítés készült tömörítési módszer tömörített.</span><span class="sxs-lookup"><span data-stu-id="ba544-143">Long strings are compressed with a compression method designed for compressing text.</span></span> <span data-ttu-id="ba544-144">Ez a tömörítés módszer egy *szótár* toostore szövegminták.</span><span class="sxs-lookup"><span data-stu-id="ba544-144">This compression method uses a *dictionary* toostore text patterns.</span></span> <span data-ttu-id="ba544-145">hello maximális dictionary mérete 16 MB.</span><span class="sxs-lookup"><span data-stu-id="ba544-145">hello maximum size of a dictionary is 16 MB.</span></span> <span data-ttu-id="ba544-146">Csak egy szótár hello sorcsoport hosszú karakterlánc oszlopainak van.</span><span class="sxs-lookup"><span data-stu-id="ba544-146">There is only one dictionary for each long string column in hello rowgroup.</span></span>

<span data-ttu-id="ba544-147">Részletes ismertető oszlopcentrikus memória követelmények, lásd: a videó [Azure SQL Data Warehouse skálázás: útmutató és konfigurációs](https://myignite.microsoft.com/videos/14822).</span><span class="sxs-lookup"><span data-stu-id="ba544-147">For an in-depth discussion of columnstore memory requirements, see the video [Azure SQL Data Warehouse scaling: configuration and guidance](https://myignite.microsoft.com/videos/14822).</span></span>

## <a name="ways-tooreduce-memory-requirements"></a><span data-ttu-id="ba544-148">Többféleképpen tooreduce memóriára vonatkozó követelményeknek</span><span class="sxs-lookup"><span data-stu-id="ba544-148">Ways tooreduce memory requirements</span></span>

<span data-ttu-id="ba544-149">A következő módszerek tooreduce hello memóriakövetelményei rowgroups tömörítés az oszlopcentrikus indexek hello használata.</span><span class="sxs-lookup"><span data-stu-id="ba544-149">Use hello following techniques tooreduce hello memory requirements for compressing rowgroups into columnstore indexes.</span></span>

### <a name="use-fewer-columns"></a><span data-ttu-id="ba544-150">Oszlopok</span><span class="sxs-lookup"><span data-stu-id="ba544-150">Use fewer columns</span></span>
<span data-ttu-id="ba544-151">Ha lehetséges tervezze meg kevesebb oszlopot hello tábla.</span><span class="sxs-lookup"><span data-stu-id="ba544-151">If possible, design hello table with fewer columns.</span></span> <span data-ttu-id="ba544-152">Amikor egy sorcsoport hello oszlopcentrikus van tömörített, hello oszlopcentrikus index külön-külön tömöríti minden oszlop szegmensben.</span><span class="sxs-lookup"><span data-stu-id="ba544-152">When a rowgroup is compressed into hello columnstore, hello columnstore index compresses each column segment separately.</span></span> <span data-ttu-id="ba544-153">Ezért hello toocompress egy sorcsoport oszlopok növekszik hello számának növelése memóriára vonatkozó követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="ba544-153">Therefore hello memory requirements toocompress a rowgroup increase as hello number of columns increases.</span></span>


### <a name="use-fewer-string-columns"></a><span data-ttu-id="ba544-154">Oszlopok karakterlánc</span><span class="sxs-lookup"><span data-stu-id="ba544-154">Use fewer string columns</span></span>
<span data-ttu-id="ba544-155">String adattípusú oszlopok numerikus-nál több memóriát és dátum adattípus van szükség.</span><span class="sxs-lookup"><span data-stu-id="ba544-155">Columns of string data types require more memory than numeric and date data types.</span></span> <span data-ttu-id="ba544-156">tooreduce memóriára vonatkozó követelményeknek, vegye figyelembe, karakterlánc típusú oszlopokra eltávolítása a ténytáblák és abba a kisebb dimenziótáblák.</span><span class="sxs-lookup"><span data-stu-id="ba544-156">tooreduce memory requirements, consider removing string columns from fact tables and putting them in smaller dimension tables.</span></span>

<span data-ttu-id="ba544-157">További memóriára vonatkozó követelményeknek karakterlánc tömörítési:</span><span class="sxs-lookup"><span data-stu-id="ba544-157">Additional memory requirements for string compression:</span></span>

- <span data-ttu-id="ba544-158">Karakterláncos adattípusokkal mentése too32 karakterek 32 további bájt értéke lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="ba544-158">String data types up too32 characters can require 32 additional bytes per value.</span></span>
- <span data-ttu-id="ba544-159">Karakterláncos adattípusokkal legfeljebb 32 karakter hosszú lehet a tömörített szótár módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="ba544-159">String data types with more than 32 characters are compressed using dictionary methods.</span></span>  <span data-ttu-id="ba544-160">Minden egyes oszlopának hello sorcsoport tooan további 16 MB toobuild hello szótár fel lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="ba544-160">Each column in hello rowgroup can require up tooan additional 16 MB toobuild hello dictionary.</span></span>

### <a name="avoid-over-partitioning"></a><span data-ttu-id="ba544-161">Kerülje a túlzott particionálás</span><span class="sxs-lookup"><span data-stu-id="ba544-161">Avoid over-partitioning</span></span>

<span data-ttu-id="ba544-162">Oszlopcentrikus indexek létrehozása egy vagy több rowgroups partíciónként.</span><span class="sxs-lookup"><span data-stu-id="ba544-162">Columnstore indexes create one or more rowgroups per partition.</span></span> <span data-ttu-id="ba544-163">Az SQL Data Warehouse hello partíciók száma nő gyorsan mert hello adatok terjeszt, és minden egyes terjesztési particionálva van.</span><span class="sxs-lookup"><span data-stu-id="ba544-163">In SQL Data Warehouse, hello number of partitions grows quickly because hello data is distributed and each distribution is partitioned.</span></span> <span data-ttu-id="ba544-164">Ha hello tábla túl sok partíciókkal rendelkezik, a nem feltétlenül elég sorok toofill hello rowgroups.</span><span class="sxs-lookup"><span data-stu-id="ba544-164">If hello table has too many partitions, there might not be enough rows toofill hello rowgroups.</span></span> <span data-ttu-id="ba544-165">sorok hello hiánya nem hoz létre Memóriaterhelést tömörítés során, de toorowgroups, hogy ne használjon a legjobb oszlopcentrikus lekérdezési teljesítmény hello vezet.</span><span class="sxs-lookup"><span data-stu-id="ba544-165">hello lack of rows does not create memory pressure during compression, but it leads toorowgroups that do not achieve hello best columnstore query performance.</span></span>

<span data-ttu-id="ba544-166">Egy másik OK tooavoid túlzott particionálás, a memória esetén a particionált tábla oszlopcentrikus index a sorok betöltése.</span><span class="sxs-lookup"><span data-stu-id="ba544-166">Another reason tooavoid over-partitioning is there is a memory overhead for loading rows into a columnstore index on a partitioned table.</span></span> <span data-ttu-id="ba544-167">A betöltés során sok partíciót kap a hello bejövő sorok, a memóriában lévő mindaddig, amíg minden partíció elég tömörített sorok toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ba544-167">During a load, many partitions could receive hello incoming rows, which are held in memory until each partition has enough rows toobe compressed.</span></span> <span data-ttu-id="ba544-168">További memória rendelkező túl sok partíciót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ba544-168">Having too many partitions creates additional memory pressure.</span></span>

### <a name="simplify-hello-load-query"></a><span data-ttu-id="ba544-169">Hello terhelés lekérdezés egyszerűsítése</span><span class="sxs-lookup"><span data-stu-id="ba544-169">Simplify hello load query</span></span>

<span data-ttu-id="ba544-170">hello adatbázis megosztások hello memória biztosítása közötti összes hello operátorok hello lekérdezés lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="ba544-170">hello database shares hello memory grant for a query among all hello operators in hello query.</span></span> <span data-ttu-id="ba544-171">Ha egy terhelés-lekérdezés összetett típusú és illesztések rendelkezik, hello memória tömörítési csökken.</span><span class="sxs-lookup"><span data-stu-id="ba544-171">When a load query has complex sorts and joins, hello memory available for compression is reduced.</span></span>

<span data-ttu-id="ba544-172">Tervezze meg hello terhelés lekérdezés toofocus csak a hello lekérdezés betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="ba544-172">Design hello load query toofocus only on loading hello query.</span></span> <span data-ttu-id="ba544-173">Ha toorun átalakítások hello adatokra van szüksége, futtatásukhoz külön hello terhelés lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="ba544-173">If you need toorun transformations on hello data, run them separate from hello load query.</span></span> <span data-ttu-id="ba544-174">Például a szakasz hello adatok egy halommemóriában tábla hello átalakítások futtassa, és majd betölteni az előkészítési tábla hello oszlopcentrikus indexet a hello.</span><span class="sxs-lookup"><span data-stu-id="ba544-174">For example, stage hello data in a heap table, run hello transformations, and then load hello staging table into hello columnstore index.</span></span> <span data-ttu-id="ba544-175">Is először az hello adatok betöltése, és kövesse a hello MPP rendszer tootransform hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="ba544-175">You can also load hello data first and then use hello MPP system tootransform hello data.</span></span>

### <a name="adjust-maxdop"></a><span data-ttu-id="ba544-176">MAXDOP beállítása</span><span class="sxs-lookup"><span data-stu-id="ba544-176">Adjust MAXDOP</span></span>

<span data-ttu-id="ba544-177">Minden terjesztési tömöríti rowgroups hello oszlopcentrikus párhuzamosan be, ha egynél több Processzormagok érhető el) eloszlása feladatonként.</span><span class="sxs-lookup"><span data-stu-id="ba544-177">Each distribution compresses rowgroups into hello columnstore in parallel when there is more than one CPU core available per distribution.</span></span> <span data-ttu-id="ba544-178">hello párhuzamossági nagyobb mennyiségű memóriát használ, ami toomemory nyomás és sorcsoport levágási igényel.</span><span class="sxs-lookup"><span data-stu-id="ba544-178">hello parallelism requires additional memory resources, which can lead toomemory pressure and rowgroup trimming.</span></span>

<span data-ttu-id="ba544-179">tooreduce Memóriaterhelést, hello MAXDOP lekérdezés mutató tooforce hello load műveletet toorun használhatja belül minden terjesztési soros módban.</span><span class="sxs-lookup"><span data-stu-id="ba544-179">tooreduce memory pressure, you can use hello MAXDOP query hint tooforce hello load operation toorun in serial mode within each distribution.</span></span>

```
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQUota
OPTION (MAXDOP 1);
```

## <a name="ways-tooallocate-more-memory"></a><span data-ttu-id="ba544-180">Többféleképpen tooallocate további memória</span><span class="sxs-lookup"><span data-stu-id="ba544-180">Ways tooallocate more memory</span></span>

<span data-ttu-id="ba544-181">DWU méretének és hello felhasználói erőforrásosztály együtt határozzák meg, mennyi memória érhető el a felhasználó lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="ba544-181">DWU size and hello user resource class together determine how much memory is available for a user query.</span></span> <span data-ttu-id="ba544-182">tooincrease hello memória hello dwu-k számának növelése vagy hello erőforrásosztály növelje a terhelés lekérdezés megadása.</span><span class="sxs-lookup"><span data-stu-id="ba544-182">tooincrease hello memory grant for a load query, you can either increase hello number of DWUs or increase hello resource class.</span></span>

- <span data-ttu-id="ba544-183">tooincrease hello dwu-k, lásd: [hogyan méretezhető teljesítmény?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span><span class="sxs-lookup"><span data-stu-id="ba544-183">tooincrease hello DWUs, see [How do I scale performance?](sql-data-warehouse-manage-compute-overview.md#scale-compute)</span></span>
- <span data-ttu-id="ba544-184">toochange hello erőforrásosztály lekérdezéshez, lásd: [módosíthatja a felhasználói erőforrás osztály példa](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="ba544-184">toochange hello resource class for a query, see [Change a user resource class example](sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

<span data-ttu-id="ba544-185">Például a DWU 100 hello smallrc erőforrásosztály felhasználójának használhatja 100 MB memóriát minden egyes terjesztési.</span><span class="sxs-lookup"><span data-stu-id="ba544-185">For example, on DWU 100 a user in hello smallrc resource class can use 100 MB of memory for each distribution.</span></span> <span data-ttu-id="ba544-186">Hello részletekért lásd: [az SQL Data Warehouse párhuzamossági](sql-data-warehouse-develop-concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="ba544-186">For hello details, see [Concurrency in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md).</span></span>

<span data-ttu-id="ba544-187">Tegyük fel, akkor határozza meg, hogy szükséges-e 700 MB memória tooget kiváló minőségű sorcsoport méretű.</span><span class="sxs-lookup"><span data-stu-id="ba544-187">Suppose you determine that you need 700 MB of memory tooget high-quality rowgroup sizes.</span></span> <span data-ttu-id="ba544-188">Ezek a példák azt szemléltetik, hogyan futtathat hello terhelés lekérdezés elegendő memóriával.</span><span class="sxs-lookup"><span data-stu-id="ba544-188">These examples show how you can run hello load query with enough memory.</span></span>

- <span data-ttu-id="ba544-189">A DWU 1000 és mediumrc, a memóriabeli ideiglenes 800 MB</span><span class="sxs-lookup"><span data-stu-id="ba544-189">Using DWU 1000 and mediumrc, your memory grant is 800 MB</span></span>
- <span data-ttu-id="ba544-190">Használja a DWU 600 és largerc, a memóriabeli ideiglenes tárat, 800 MB.</span><span class="sxs-lookup"><span data-stu-id="ba544-190">Using DWU 600 and largerc, your memory grant is 800 MB.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ba544-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba544-191">Next steps</span></span>

<span data-ttu-id="ba544-192">toofind további módszereket tooimprove teljesítmény az SQL Data Warehouse, lásd: hello [teljesítményének áttekintését](sql-data-warehouse-overview-manage-user-queries.md).</span><span class="sxs-lookup"><span data-stu-id="ba544-192">toofind more ways tooimprove performance in SQL Data Warehouse, see hello [Performance overview](sql-data-warehouse-overview-manage-user-queries.md).</span></span>

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->

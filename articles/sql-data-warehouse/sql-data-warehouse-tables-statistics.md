---
title: "Az SQL Data Warehouse táblák statisztikák kezelése |} Microsoft Docs"
description: "Első lépések az Azure SQL Data Warehouse táblákon statisztika."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 1d5ded69e394643ddfc3de0c6d30dbd30c8e848f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a><span data-ttu-id="af4d4-103">Az SQL Data Warehouse táblák statisztikák kezelése</span><span class="sxs-lookup"><span data-stu-id="af4d4-103">Managing statistics on tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="af4d4-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="af4d4-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="af4d4-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="af4d4-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="af4d4-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="af4d4-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="af4d4-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="af4d4-107">[Index][Index]</span></span>
> * <span data-ttu-id="af4d4-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="af4d4-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="af4d4-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="af4d4-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="af4d4-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="af4d4-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="af4d4-111">Minél több ismeri az SQL Data Warehouse az adatokat, minél gyorsabban hajtsa végre a lekérdezéseket az adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="af4d4-111">The more SQL Data Warehouse knows about your data, the faster it can execute queries against your data.</span></span>  <span data-ttu-id="af4d4-112">A úgy, hogy az SQL Data Warehouse szolgál az adatokról, kérje meg, hogy statisztikája az adatok gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-112">The way that you tell SQL Data Warehouse about your data, is by collecting statistics about your data.</span></span>  <span data-ttu-id="af4d4-113">Statisztika az adatokról, akkor az a legfontosabb dolog, a lekérdezések optimalizálását is van.</span><span class="sxs-lookup"><span data-stu-id="af4d4-113">Having statistics on your data is one of the most important things you can do to optimize your queries.</span></span>  <span data-ttu-id="af4d4-114">Statisztika SQL Data Warehouse a legoptimálisabb terv a lekérdezések létrehozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="af4d4-114">Statistics help SQL Data Warehouse create the most optimal plan for your queries.</span></span>  <span data-ttu-id="af4d4-115">Ez azért, mert az SQL Data Warehouse lekérdezésoptimalizáló egy alapján optimalizáló.</span><span class="sxs-lookup"><span data-stu-id="af4d4-115">This is because the SQL Data Warehouse query optimizer is a cost based optimizer.</span></span>  <span data-ttu-id="af4d4-116">Ez azt jelenti, hogy összehasonlítja a különböző lekérdezésterveket költségét, és majd úgy dönt, hogy a tervben a legalacsonyabb költséget, és a terv, amely végrehajtja a leggyorsabb kell.</span><span class="sxs-lookup"><span data-stu-id="af4d4-116">That is, it compares the cost of various query plans and then chooses the plan with the lowest cost, which should also be the plan that will execute the fastest.</span></span>

<span data-ttu-id="af4d4-117">Statisztika is létrehozható, csak egy oszlop, több oszlop vagy táblázat index.</span><span class="sxs-lookup"><span data-stu-id="af4d4-117">Statistics can be created on a single column, multiple columns or on an index of a table.</span></span>  <span data-ttu-id="af4d4-118">Statisztika tárolódnak a hisztogram, amely a tartomány- és értékek kiválasztásánál rögzíti.</span><span class="sxs-lookup"><span data-stu-id="af4d4-118">Statistics are stored in a histogram which captures the range and selectivity of values.</span></span>  <span data-ttu-id="af4d4-119">Ez az különösen fontos, amikor a optimalizáló kell kiértékelni illesztéseket, GROUP BY, HAVING és a WHERE záradék a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="af4d4-119">This is of particular interest when the optimizer needs to evaluate JOINs, GROUP BY, HAVING and WHERE clauses in a query.</span></span>  <span data-ttu-id="af4d4-120">Például ha a optimalizáló becslése, hogy a jelenleg korlátozza a lekérdezés dátuma 1 sort ad vissza, dönthet úgy, nagyon eltérő megtervezése mint azt, hogy azok szerint megadott dátum, akkor becslése kijelölt lesz 1 millió sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="af4d4-120">For example, if the optimizer estimates that the date you are filtering in your query will return 1 row, it may choose a very different plan than if it estimates that they date you have selected will return 1 million rows.</span></span>  <span data-ttu-id="af4d4-121">Míg rendkívül fontos a statisztikák létrehozása, is ugyanilyen fontos, hogy statisztika *pontosan* táblázat aktuális állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="af4d4-121">While creating statistics is extremely important, it is equally important that statistics *accurately* reflect the current state of the table.</span></span>  <span data-ttu-id="af4d4-122">Naprakész statisztika biztosítja, hogy egy jó terv a optimalizáló van-e kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="af4d4-122">Having up-to-date statistics ensures that a good plan is selected by the optimizer.</span></span>  <span data-ttu-id="af4d4-123">Az előfizetések hozta létre a optimalizáló csak a következők megegyezik a statisztikai adatait.</span><span class="sxs-lookup"><span data-stu-id="af4d4-123">The plans created by the optimizer are only as good as the statistics on your data.</span></span>

<span data-ttu-id="af4d4-124">A folyamat létrehozásának, és frissítse a statisztikai adatokat jelenleg egy kézi művelet, de ehhez nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="af4d4-124">The process of creating and updating statistics is currently a manual process, but is very simple to do.</span></span>  <span data-ttu-id="af4d4-125">Ez nem az SQL Server, mely automatikusan létrehozza, és egyetlen oszlopok és indexek statisztikák frissíti.</span><span class="sxs-lookup"><span data-stu-id="af4d4-125">This is unlike SQL Server which automatically creates and updates statistics on single columns and indexes.</span></span>  <span data-ttu-id="af4d4-126">Az alábbi információk segítségével nagy mértékben automatizálható a statisztikák felügyeleti az adatokon.</span><span class="sxs-lookup"><span data-stu-id="af4d4-126">By using the information below, you can greatly automate the management of the statistics on your data.</span></span> 

## <a name="getting-started-with-statistics"></a><span data-ttu-id="af4d4-127">Ismerkedés a statisztikák</span><span class="sxs-lookup"><span data-stu-id="af4d4-127">Getting started with statistics</span></span>
 <span data-ttu-id="af4d4-128">Minden egyes oszlophoz a mintában szereplő statisztikák létrehozása egyszerű módja statisztika használatába.</span><span class="sxs-lookup"><span data-stu-id="af4d4-128">Creating sampled statistics on every column is an easy way to get started with statistics.</span></span>  <span data-ttu-id="af4d4-129">Egyaránt fontos, hogy naprakész állapotban tarthatja az statisztika, mert a konzervatív megközelítés lehet frissíteni a statisztikákat a naponta, vagy minden egyes betöltés után.</span><span class="sxs-lookup"><span data-stu-id="af4d4-129">Since it is equally important to keep statistics up-to-date, a conservative approach may be to update your statistics daily or after each load.</span></span> <span data-ttu-id="af4d4-130">Mindig érdemes figyelembe venni, hogyan viszonyul egymáshoz a teljesítmény és a statisztikák létrehozásának és frissítésének költségei.</span><span class="sxs-lookup"><span data-stu-id="af4d4-130">There are always trade-offs between performance and the cost to create and update statistics.</span></span>  <span data-ttu-id="af4d4-131">Ha úgy gondolja, hogy túl sokáig tart az összes statisztika karbantartása, lehet, hogy körültekintőbben kell kiválasztania, mely oszlopok rendelkezzenek statisztikákkal vagy melyek igényelnek gyakori frissítést.</span><span class="sxs-lookup"><span data-stu-id="af4d4-131">If you find it is taking too long to maintain all of your statistics, you may want to try to be more selective about which columns have statistics or which columns need frequent updating.</span></span>  <span data-ttu-id="af4d4-132">Például előfordulhat, hogy szeretné frissíteni a dátumoszlopot, naponta, új értékeket vehetők helyett minden betöltés után.</span><span class="sxs-lookup"><span data-stu-id="af4d4-132">For example, you might want to update date columns daily, as new values may be added rather than after every load.</span></span> <span data-ttu-id="af4d4-133">Ebben az esetben érheti el a legtöbb juttatás azzal, hogy a statisztika oszlopokon érintett illesztéseket, GROUP BY, HAVING és a WHERE záradék.</span><span class="sxs-lookup"><span data-stu-id="af4d4-133">Again, you will gain the most benefit by having statistics on columns involved in JOINs, GROUP BY, HAVING and WHERE clauses.</span></span>  <span data-ttu-id="af4d4-134">Ha nagy mennyiségű oszlopok a SELECT záradékban csak használt tábla, nem segíthetnek, ezen oszlopokon statisztika, és kissé költségeik további erőfeszítésekre azonosításához csak azokat az oszlopokat, ahol statisztika segít, csökkentheti az időt a statisztika karbantartása.</span><span class="sxs-lookup"><span data-stu-id="af4d4-134">If you have a table with a lot of columns which are only used in the SELECT clause, statistics on these columns may not help, and spending a little more effort to identify only the columns where statistics will help, can reduce the time to maintain your statistics.</span></span>

## <a name="multi-column-statistics"></a><span data-ttu-id="af4d4-135">Több oszlop statisztikai adatainak</span><span class="sxs-lookup"><span data-stu-id="af4d4-135">Multi-column statistics</span></span>
<span data-ttu-id="af4d4-136">Mellett statisztikák létrehozása egyetlen oszlopokon, előfordulhat, hogy a lekérdezések ki a előnyeit több oszlop statisztikai adatainak.</span><span class="sxs-lookup"><span data-stu-id="af4d4-136">In addition to creating statistics on single columns, you may find that your queries will benefit from multi-column statistics.</span></span>  <span data-ttu-id="af4d4-137">Több oszlop statisztikákat statisztikákat létrehozni az oszlopok listája.</span><span class="sxs-lookup"><span data-stu-id="af4d4-137">Multi-column statistics are statistics created on a list of columns.</span></span>  <span data-ttu-id="af4d4-138">A lista első oszlopa egy oszlop statisztikai tartoznak, valamint bizonyos kereszt-oszlop korrelációs adatokat nevű sűrűség.</span><span class="sxs-lookup"><span data-stu-id="af4d4-138">They include single column statistics on the first column in the list, plus some cross-column correlation information called densities.</span></span>  <span data-ttu-id="af4d4-139">Például ha egy táblázatot, amelyhez csatlakozik, a két oszlop között, előfordulhat, hogy az SQL Data Warehouse jobban is optimalizálhatja a terv, támogatja a két oszlop közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="af4d4-139">For example, if you have a table that joins to another on two columns, you may find that SQL Data Warehouse can better optimize the plan if it understands the relationship between two columns.</span></span>   <span data-ttu-id="af4d4-140">Több oszlop statisztikai adatainak javíthatja a lekérdezések teljesítményét az egyes műveletek, például összetett illesztések és a csoportosítás alapját.</span><span class="sxs-lookup"><span data-stu-id="af4d4-140">Multi-column statistics can improve query performance for some operations such as composite joins and group by.</span></span>

## <a name="updating-statistics"></a><span data-ttu-id="af4d4-141">Frissítse a statisztikai adatokat</span><span class="sxs-lookup"><span data-stu-id="af4d4-141">Updating statistics</span></span>
<span data-ttu-id="af4d4-142">Frissítse a statisztikai adatokat az adatbázis-felügyeleti rutin fontos részét képezi.</span><span class="sxs-lookup"><span data-stu-id="af4d4-142">Updating statistics is an important part of your database management routine.</span></span>  <span data-ttu-id="af4d4-143">Az adatbázis adatai eloszlását megváltozásakor statisztika frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="af4d4-143">When the distribution of data in the database changes, statistics need to be updated.</span></span>  <span data-ttu-id="af4d4-144">Elévült statisztikát optimális lekérdezési teljesítmény irányítja.</span><span class="sxs-lookup"><span data-stu-id="af4d4-144">Out-of-date statistics will lead to sub-optimal query performance.</span></span>

<span data-ttu-id="af4d4-145">Egy ajánlott úgy nem frissíthető statisztika dátumoszlopának dátumtulajdonságai minden nap új dátumok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="af4d4-145">One best practice is to update statistics on date columns each day as new dates are added.</span></span>  <span data-ttu-id="af4d4-146">Minden alkalommal új sorok be vannak töltve az adatraktárba, új betöltése vagy tranzakció kerülnek.</span><span class="sxs-lookup"><span data-stu-id="af4d4-146">Each time new rows are loaded into the data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="af4d4-147">Ezek az adatok terjesztési módosítsa, majd ellenőrizze a statisztika elavult.</span><span class="sxs-lookup"><span data-stu-id="af4d4-147">These change the data distribution and make the statistics out-of-date.</span></span> <span data-ttu-id="af4d4-148">Ezzel szemben egy felhasználói tábla ország oszlop statisztikai előfordulhat, hogy soha nem frissítenie kell, a terjesztési értékek általában nem változik.</span><span class="sxs-lookup"><span data-stu-id="af4d4-148">Conversely, statistics on a country column in a customer table might never need to be updated, as the distribution of values doesn’t generally change.</span></span> <span data-ttu-id="af4d4-149">Feltéve, hogy a terjesztés az ügyfelek közötti állandó, új sorok hozzáadásakor a tábla változat nem fog módosítása az adatok terjesztési.</span><span class="sxs-lookup"><span data-stu-id="af4d4-149">Assuming the distribution is constant between customers, adding new rows to the table variation isn't going to change the data distribution.</span></span> <span data-ttu-id="af4d4-150">Azonban ha az adatraktár csak tartalmaz egy országon, és az adatok egy új országból kapcsolná, az adatok tárolását, több országokból származó majd mindenképpen szeretné az ország oszlop statisztikai adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-150">However, if your data warehouse only contains one country and you bring in data from a new country, resulting in data from multiple countries being stored, then you definitely need to update statistics on the country column.</span></span>

<span data-ttu-id="af4d4-151">Az első megválaszolandó kérdések esetén végzett hibaelhárításhoz a lekérdezés egyik, "Are naprakész statisztika?"</span><span class="sxs-lookup"><span data-stu-id="af4d4-151">One of the first questions to ask when troubleshooting a query is, "Are the statistics up-to-date?"</span></span>

<span data-ttu-id="af4d4-152">Ez a kérdés nem szerepel az adatok korát válaszoló.</span><span class="sxs-lookup"><span data-stu-id="af4d4-152">This question is not one that can be answered by the age of the data.</span></span> <span data-ttu-id="af4d4-153">Lehet, hogy naprakész statisztika objektum nagyon régi, ha az alapul szolgáló adatokhoz nincs lényeges változás történt.</span><span class="sxs-lookup"><span data-stu-id="af4d4-153">An up to date statistics object could be very old if there's been no material change to the underlying data.</span></span> <span data-ttu-id="af4d4-154">Ha a sorok száma jelentősen módosult, vagy nincs az adott oszlop értékeinek eloszlását jelentős változás *majd* ideje statisztikai adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-154">When the number of rows has changed substantially or there is a material change in the distribution of values for a given column *then* it's time to update statistics.</span></span>  

<span data-ttu-id="af4d4-155">Referenciaként **SQL Server** (nem az SQL Data Warehouse) automatikusan frissíti a statisztikákat ilyen helyzetekben:</span><span class="sxs-lookup"><span data-stu-id="af4d4-155">For reference, **SQL Server** (not SQL Data Warehouse) automatically updates statistics for these situations:</span></span>

* <span data-ttu-id="af4d4-156">Ha egyetlen sor a tábla sorainak hozzáadásakor, fog kapni a statisztikák automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="af4d4-156">If you have zero rows in the table, when you add rows, you’ll get an automatic update of statistics</span></span>
* <span data-ttu-id="af4d4-157">Hozzáadásakor 500-nál több sort egy táblához legfeljebb 500 sorok kezdve (például a start rendelkezik 499, és hozzáadhatja 500 sorok 999 sorok összesen), az automatikus frissítés jelenik meg</span><span class="sxs-lookup"><span data-stu-id="af4d4-157">When you add more than 500 rows to a table starting with less than 500 rows (e.g. at start you have 499 and then add 500 rows to a total of 999 rows), you’ll get an automatic update</span></span> 
* <span data-ttu-id="af4d4-158">Ha már több mint 500 sorok meg kell adnia a 500 további sorokat + 20 %-át a tábla méretét a statisztikák a láthatja az automatikus frissítés előtt</span><span class="sxs-lookup"><span data-stu-id="af4d4-158">Once you’re over 500 rows you will have to add 500 additional rows + 20% of the size of the table before you’ll see an automatic update on the stats</span></span>

<span data-ttu-id="af4d4-159">Mivel nincs DMV annak meghatározásához, hogy a tábla adatainak módosult-e az utolsó idő statisztika frissítése, a statisztika korát tudatában adja meg a képen látható része.</span><span class="sxs-lookup"><span data-stu-id="af4d4-159">Since there is no DMV to determine if data within the table has changed since the last time statistics were updated, knowing the age of your statistics can provide you with part of the picture.</span></span>  <span data-ttu-id="af4d4-160">A következő lekérdezés segítségével határozza meg a legutóbbi a statisztika ahol minden táblában frissített.</span><span class="sxs-lookup"><span data-stu-id="af4d4-160">You can use the following query to determine the last time your statistics where updated on each table.</span></span>  

> [!NOTE]
> <span data-ttu-id="af4d4-161">Ne feledje, hogy az adott oszlop értékeinek eloszlását jelentős változás esetén frissítenie kell a statisztika függetlenül legutóbbi frissítése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="af4d4-161">Remember if there is a material change in the distribution of values for a given column, you should update statistics regardless of the last time they were updated.</span></span>  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

<span data-ttu-id="af4d4-162">Egy adatraktár dátumoszlopának dátumtulajdonságai például általában kell gyakori statisztikai adatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-162">Date columns in a data warehouse, for example, usually need frequent statistics updates.</span></span> <span data-ttu-id="af4d4-163">Minden alkalommal új sorok be vannak töltve az adatraktárba, új betöltése vagy tranzakció kerülnek.</span><span class="sxs-lookup"><span data-stu-id="af4d4-163">Each time new rows are loaded into the data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="af4d4-164">Ezek az adatok terjesztési módosítsa, majd ellenőrizze a statisztika elavult.</span><span class="sxs-lookup"><span data-stu-id="af4d4-164">These change the data distribution and make the statistics out-of-date.</span></span>  <span data-ttu-id="af4d4-165">Ezzel ellentétben egy felhasználói tábla nemét oszlop statisztikai előfordulhat, hogy soha nem frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="af4d4-165">Conversely, statistics on a gender column on a customer table might never need to be updated.</span></span> <span data-ttu-id="af4d4-166">Feltéve, hogy a terjesztés az ügyfelek közötti állandó, új sorok hozzáadásakor a tábla változat nem fog módosítása az adatok terjesztési.</span><span class="sxs-lookup"><span data-stu-id="af4d4-166">Assuming the distribution is constant between customers, adding new rows to the table variation isn't going to change the data distribution.</span></span> <span data-ttu-id="af4d4-167">Azonban ha az adatraktár csak tartalmaz egy nemét, és több genders egy új követelményt eredményezi majd mindenképpen szeretné a nemét oszlop statisztikai adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-167">However, if your data warehouse only contains one gender and a new requirement results in multiple genders then you definitely need to update statistics on the gender column.</span></span>

<span data-ttu-id="af4d4-168">További ismertetése [statisztika] [ Statistics] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="af4d4-168">For further explanation, see [Statistics][Statistics] on MSDN.</span></span>

## <a name="implementing-statistics-management"></a><span data-ttu-id="af4d4-169">Végrehajtási statisztika kezelése</span><span class="sxs-lookup"><span data-stu-id="af4d4-169">Implementing statistics management</span></span>
<span data-ttu-id="af4d4-170">Akkor érdemes gyakran az adatok betöltése a folyamat végrehajtásával biztosítható, hogy a terhelés végén frissíti statisztikáit kiterjeszteni.</span><span class="sxs-lookup"><span data-stu-id="af4d4-170">It is often a good idea to extend your data loading process to ensure that statistics are updated at the end of the load.</span></span> <span data-ttu-id="af4d4-171">Az adatok betöltését akkor, ha a táblák leggyakrabban módosítása a méretük és/vagy a terjesztési értékek.</span><span class="sxs-lookup"><span data-stu-id="af4d4-171">The data load is when tables most frequently change their size and/or their distribution of values.</span></span> <span data-ttu-id="af4d4-172">Ezért ez a logikai hely valamelyik felügyeleti folyamat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="af4d4-172">Therefore, this is a logical place to implement some management processes.</span></span>

<span data-ttu-id="af4d4-173">Néhány alapelvek a statisztikák frissítése a betöltési folyamat során az alább:</span><span class="sxs-lookup"><span data-stu-id="af4d4-173">Some guiding principles are provided below for updating your statistics during the load process:</span></span>

* <span data-ttu-id="af4d4-174">Győződjön meg arról, hogy rendelkezik-e frissíteni kell legalább egy statisztika objektum minden egyes betöltött táblákon.</span><span class="sxs-lookup"><span data-stu-id="af4d4-174">Ensure that each loaded table has at least one statistics object updated.</span></span> <span data-ttu-id="af4d4-175">Ezzel frissíti a táblák (sorok számát és oldalszám) információi a statisztikák frissítés részeként.</span><span class="sxs-lookup"><span data-stu-id="af4d4-175">This updates the tables size (row count and page count) information as part of the stats update.</span></span>
* <span data-ttu-id="af4d4-176">Összpontosítson az ILLESZTÉS, GROUP BY, ORDER BY és DISTINCT záradékban részt vevő oszlopokat</span><span class="sxs-lookup"><span data-stu-id="af4d4-176">Focus on columns participating in JOIN, GROUP BY, ORDER BY and DISTINCT clauses</span></span>
* <span data-ttu-id="af4d4-177">Fontolja meg "kulcs növekvő" oszlopok például tranzakció dátumok gyakrabban, mivel ezek az értékek nem fog szerepelni a statisztika hisztogram.</span><span class="sxs-lookup"><span data-stu-id="af4d4-177">Consider updating "ascending key" columns such as transaction dates more frequently as these values will not be included in the statistics histogram.</span></span>
* <span data-ttu-id="af4d4-178">Érdemes lehet, statikus terjesztési oszlopok gyakran frissíteni.</span><span class="sxs-lookup"><span data-stu-id="af4d4-178">Consider updating static distribution columns less frequently.</span></span>
* <span data-ttu-id="af4d4-179">Ne feledje, hogy minden statisztikai adat objektum sorozat frissül.</span><span class="sxs-lookup"><span data-stu-id="af4d4-179">Remember each statistic object is updated in series.</span></span> <span data-ttu-id="af4d4-180">Egyszerűen végrehajtási `UPDATE STATISTICS <TABLE_NAME>` nem mindig ideális megoldás – különösen a statisztika objektumok sok nagy táblák esetében.</span><span class="sxs-lookup"><span data-stu-id="af4d4-180">Simply implementing `UPDATE STATISTICS <TABLE_NAME>` may not be ideal - especially for wide tables with lots of statistics objects.</span></span>

> [!NOTE]
> <span data-ttu-id="af4d4-181">[Növekvő kulcs] vonatkozó részletes információért tekintse meg az SQL Server 2014 számossága becslés modell tanulmány.</span><span class="sxs-lookup"><span data-stu-id="af4d4-181">For more details on [ascending key] please refer to the SQL Server 2014 cardinality estimation model whitepaper.</span></span>
> 
> 

<span data-ttu-id="af4d4-182">További ismertetése [számossága becslés] [ Cardinality Estimation] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="af4d4-182">For further explanation, see  [Cardinality Estimation][Cardinality Estimation] on MSDN.</span></span>

## <a name="examples-create-statistics"></a><span data-ttu-id="af4d4-183">Példák: Statisztikák létrehozása</span><span class="sxs-lookup"><span data-stu-id="af4d4-183">Examples: Create statistics</span></span>
<span data-ttu-id="af4d4-184">Ezek a példák használatát mutatják be különböző beállítások statisztikák létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="af4d4-184">These examples show how to use various options for creating statistics.</span></span> <span data-ttu-id="af4d4-185">A beállítások, amelyekkel az egyes oszlopok az adatok és az oszlop a lekérdezésekben használt hogyan jellemzői függ.</span><span class="sxs-lookup"><span data-stu-id="af4d4-185">The options that you use for each column depend on the characteristics of your data and how the column will be used in queries.</span></span>

### <a name="a-create-single-column-statistics-with-default-options"></a><span data-ttu-id="af4d4-186">A.</span><span class="sxs-lookup"><span data-stu-id="af4d4-186">A.</span></span> <span data-ttu-id="af4d4-187">Egy oszlop statisztikák létrehozása az alapértelmezett beállításokkal</span><span class="sxs-lookup"><span data-stu-id="af4d4-187">Create single-column statistics with default options</span></span>
<span data-ttu-id="af4d4-188">Statisztikákat létrehozni egy olyan oszlop, adjon meg egy nevet a statisztika objektum és az oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="af4d4-188">To create statistics on a column, simply provide a name for the statistics object and the name of the column.</span></span>

<span data-ttu-id="af4d4-189">Ez a szintaxis összes alapértelmezett beállítást használja.</span><span class="sxs-lookup"><span data-stu-id="af4d4-189">This syntax uses all of the default options.</span></span> <span data-ttu-id="af4d4-190">Alapértelmezés szerint az SQL Data Warehouse-minták 20 százalékát a tábla statisztikai létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="af4d4-190">By default, SQL Data Warehouse samples 20 percent of the table when it creates statistics.</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

<span data-ttu-id="af4d4-191">Példa:</span><span class="sxs-lookup"><span data-stu-id="af4d4-191">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a><span data-ttu-id="af4d4-192">B.</span><span class="sxs-lookup"><span data-stu-id="af4d4-192">B.</span></span> <span data-ttu-id="af4d4-193">Minden sor megvizsgálásával egyoszlopos statisztikát létrehozása</span><span class="sxs-lookup"><span data-stu-id="af4d4-193">Create single-column statistics by examining every row</span></span>
<span data-ttu-id="af4d4-194">Az alapértelmezett mintavételi 20 százalékos aránya a legtöbb esetben elegendő.</span><span class="sxs-lookup"><span data-stu-id="af4d4-194">The default sampling rate of 20 percent is sufficient for most situations.</span></span> <span data-ttu-id="af4d4-195">A mintavételi ráta azonban módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="af4d4-195">However, you can adjust the sampling rate.</span></span>

<span data-ttu-id="af4d4-196">Példa a teljes táblázat, használja ezt a szintaxist:</span><span class="sxs-lookup"><span data-stu-id="af4d4-196">To sample the full table, use this syntax:</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

<span data-ttu-id="af4d4-197">Példa:</span><span class="sxs-lookup"><span data-stu-id="af4d4-197">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a><span data-ttu-id="af4d4-198">C.</span><span class="sxs-lookup"><span data-stu-id="af4d4-198">C.</span></span> <span data-ttu-id="af4d4-199">Egy oszlop statisztikák létrehozása a mintaméret megadásával</span><span class="sxs-lookup"><span data-stu-id="af4d4-199">Create single-column statistics by specifying the sample size</span></span>
<span data-ttu-id="af4d4-200">Azt is megteheti adhatja meg a minta mérete százalékban:</span><span class="sxs-lookup"><span data-stu-id="af4d4-200">Alternatively, you can specify the sample size as a percent:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a><span data-ttu-id="af4d4-201">D.</span><span class="sxs-lookup"><span data-stu-id="af4d4-201">D.</span></span> <span data-ttu-id="af4d4-202">Egyoszlopos statisztikát csak egy sor létrehozása</span><span class="sxs-lookup"><span data-stu-id="af4d4-202">Create single-column statistics on only some of the rows</span></span>
<span data-ttu-id="af4d4-203">Egy másik lehetőség, létrehozhat statisztika egy részét a sorokat a táblában.</span><span class="sxs-lookup"><span data-stu-id="af4d4-203">Another option, you can create statistics on a portion of the rows in your table.</span></span> <span data-ttu-id="af4d4-204">Ezt nevezik a szűrt statisztikai.</span><span class="sxs-lookup"><span data-stu-id="af4d4-204">This is called a filtered statistic.</span></span>

<span data-ttu-id="af4d4-205">Használhat például szűrt statisztikákat, ha azt tervezi, hogy egy adott partícióra egy nagy particionált tábla lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-205">For example, you could use filtered statistics when you plan to query a specific partition of a large partitioned table.</span></span> <span data-ttu-id="af4d4-206">Csak a partíció értékek statisztika létrehozásával a statisztika pontossága fog javításához, illetve ezért javíthatja a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="af4d4-206">By creating statistics on only the partition values, the accuracy of the statistics will improve, and therefore improve query performance.</span></span>

<span data-ttu-id="af4d4-207">Ez a Példa statisztika értéktartománya hoz létre.</span><span class="sxs-lookup"><span data-stu-id="af4d4-207">This example creates statistics on a range of values.</span></span> <span data-ttu-id="af4d4-208">Az értékeket egy partíció értékek megfelelő könnyen kell meghatározni.</span><span class="sxs-lookup"><span data-stu-id="af4d4-208">The values could easily be defined to match the range of values in a partition.</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> <span data-ttu-id="af4d4-209">A lekérdezésoptimalizáló figyelembe kell venni a szűrt statisztikákat, ha az elosztott lekérdezés terv úgy dönt, akkor az a lekérdezés kell férnie a statisztika objektum definíciója.</span><span class="sxs-lookup"><span data-stu-id="af4d4-209">For the query optimizer to consider using filtered statistics when it chooses the distributed query plan, the query must fit inside the definition of the statistics object.</span></span> <span data-ttu-id="af4d4-210">Az előző példában a lekérdezés ahol záradékban kell meghatároznia 2000101 és 20001231 közötti Oszlop1 értékek használatával.</span><span class="sxs-lookup"><span data-stu-id="af4d4-210">Using the previous example, the query's where clause needs to specify col1 values between 2000101 and 20001231.</span></span>
> 
> 

### <a name="e-create-single-column-statistics-with-all-the-options"></a><span data-ttu-id="af4d4-211">E.</span><span class="sxs-lookup"><span data-stu-id="af4d4-211">E.</span></span> <span data-ttu-id="af4d4-212">Egy oszlop statisztikák létrehozása a beállításokkal</span><span class="sxs-lookup"><span data-stu-id="af4d4-212">Create single-column statistics with all the options</span></span>
<span data-ttu-id="af4d4-213">Természetesen kombinálhatja a beállítások együtt.</span><span class="sxs-lookup"><span data-stu-id="af4d4-213">You can, of course, combine the options together.</span></span> <span data-ttu-id="af4d4-214">Az alábbi példában egy szűrt statisztikákat objektumot hoz létre egyéni mintájának méretét:</span><span class="sxs-lookup"><span data-stu-id="af4d4-214">The example below creates a filtered statistics object with a custom sample size:</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="af4d4-215">A teljes referenciáért lásd: [CREATE statistics UTASÍTÁSHOZ] [ CREATE STATISTICS] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="af4d4-215">For the full reference, see [CREATE STATISTICS][CREATE STATISTICS] on MSDN.</span></span>

### <a name="f-create-multi-column-statistics"></a><span data-ttu-id="af4d4-216">F.</span><span class="sxs-lookup"><span data-stu-id="af4d4-216">F.</span></span> <span data-ttu-id="af4d4-217">Több oszlop statisztikai adatainak létrehozása</span><span class="sxs-lookup"><span data-stu-id="af4d4-217">Create multi-column statistics</span></span>
<span data-ttu-id="af4d4-218">Hozzon létre egy több oszlopot tartalmazó statisztika, egyszerűen használja az előző példák, de további oszlopok megadása.</span><span class="sxs-lookup"><span data-stu-id="af4d4-218">To create a multi-column statistics, simply use the previous examples, but specify more columns.</span></span>

> [!NOTE]
> <span data-ttu-id="af4d4-219">A hisztogram, amely használható a lekérdezés eredményében sorok számának becslése, az első oszlop szerepel a statisztika objektum definíciója csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="af4d4-219">The histogram, which is used to estimate number of rows in the query result, is only available for the first column listed in the statistics object definition.</span></span>
> 
> 

<span data-ttu-id="af4d4-220">Ebben a példában a hisztogram van *termék\_kategória*.</span><span class="sxs-lookup"><span data-stu-id="af4d4-220">In this example, the histogram is on *product\_category*.</span></span> <span data-ttu-id="af4d4-221">Kereszt-oszlop statisztikai adatainak kiszámítása *termék\_kategória* és *termék\_sub_c\ategory*:</span><span class="sxs-lookup"><span data-stu-id="af4d4-221">Cross-column statistics are calculated on *product\_category* and *product\_sub_c\ategory*:</span></span>

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="af4d4-222">Mivel közötti összefüggés *termék\_kategória* és *termék\_sub\_kategória*, a többoszlopos stat akkor lehet hasznos, ha ezekben az oszlopokban elért egy időben.</span><span class="sxs-lookup"><span data-stu-id="af4d4-222">Since there is a correlation between *product\_category* and *product\_sub\_category*, a multi-column stat can be useful if these columns are accessed at the same time.</span></span>

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a><span data-ttu-id="af4d4-223">G.</span><span class="sxs-lookup"><span data-stu-id="af4d4-223">G.</span></span> <span data-ttu-id="af4d4-224">Egy táblázat összes oszlopa statisztikák létrehozása</span><span class="sxs-lookup"><span data-stu-id="af4d4-224">Create statistics on all the columns in a table</span></span>
<span data-ttu-id="af4d4-225">Statisztika létrehozásának egyik módja van problémáinak CREATE statistics UTASÍTÁSHOZ parancsok a következő tábla létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="af4d4-225">One way to create statistics is to issues CREATE STATISTICS commands after creating the table.</span></span>

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a><span data-ttu-id="af4d4-226">H.</span><span class="sxs-lookup"><span data-stu-id="af4d4-226">H.</span></span> <span data-ttu-id="af4d4-227">Statisztika létrehozásához minden oszlop egy adatbázisban tárolt eljárás használata</span><span class="sxs-lookup"><span data-stu-id="af4d4-227">Use a stored procedure to create statistics on all columns in a database</span></span>
<span data-ttu-id="af4d4-228">Az SQL Data Warehouse az SQL Server nincs egyenértékű [sp_create_stats] [-] rendszerben tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="af4d4-228">SQL Data Warehouse does not have a system stored procedure equivalent to [sp_create_stats][] in SQL Server.</span></span> <span data-ttu-id="af4d4-229">Ez a tárolt eljárás objektumot hoz létre egy oszlop statisztika minden oszlop, amely még nem rendelkezik statisztikai adatbázis.</span><span class="sxs-lookup"><span data-stu-id="af4d4-229">This stored procedure creates a single column statistics object on every column of the database that doesn't already have statistics.</span></span>

<span data-ttu-id="af4d4-230">Ez segítséget nyújt az adatbázis tervét az első lépései.</span><span class="sxs-lookup"><span data-stu-id="af4d4-230">This will help you get started with your database design.</span></span> <span data-ttu-id="af4d4-231">Nyugodtan igazodjon igényeinek.</span><span class="sxs-lookup"><span data-stu-id="af4d4-231">Feel free to adapt it to your needs.</span></span>

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

<span data-ttu-id="af4d4-232">Ezt az eljárást a táblázat összes oszlopa statisztikák létrehozása, egyszerűen hívja meg az eljárást.</span><span class="sxs-lookup"><span data-stu-id="af4d4-232">To create statistics on all columns in the table with this procedure, simply call the procedure.</span></span>

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a><span data-ttu-id="af4d4-233">Példák: statisztika frissítése</span><span class="sxs-lookup"><span data-stu-id="af4d4-233">Examples: update statistics</span></span>
<span data-ttu-id="af4d4-234">A statisztikák frissítése, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="af4d4-234">To update statistics, you can:</span></span>

1. <span data-ttu-id="af4d4-235">Egy statisztikai objektum frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-235">Update one statistics object.</span></span> <span data-ttu-id="af4d4-236">Adja meg a frissíteni kívánt statisztika objektum neve.</span><span class="sxs-lookup"><span data-stu-id="af4d4-236">Specify the name of the statistics object you wish to update.</span></span>
2. <span data-ttu-id="af4d4-237">Egy tábla összes statisztika objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-237">Update all statistics objects on a table.</span></span> <span data-ttu-id="af4d4-238">Adja meg a táblázat helyett egy adott statisztika objektum nevét.</span><span class="sxs-lookup"><span data-stu-id="af4d4-238">Specify the name of the table instead of one specific statistics object.</span></span>

### <a name="a-update-one-specific-statistics-object"></a><span data-ttu-id="af4d4-239">A.</span><span class="sxs-lookup"><span data-stu-id="af4d4-239">A.</span></span> <span data-ttu-id="af4d4-240">Egy adott statisztika objektum frissítése</span><span class="sxs-lookup"><span data-stu-id="af4d4-240">Update one specific statistics object</span></span>
<span data-ttu-id="af4d4-241">A következő szintaxissal egy adott statisztika objektum frissítése:</span><span class="sxs-lookup"><span data-stu-id="af4d4-241">Use the following syntax to update a specific statistics object:</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

<span data-ttu-id="af4d4-242">Példa:</span><span class="sxs-lookup"><span data-stu-id="af4d4-242">For example:</span></span>

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

<span data-ttu-id="af4d4-243">Adott statisztika objektumok frissítése, minimalizálhatja az idő és a statisztika kezeléséhez szükséges erőforrások.</span><span class="sxs-lookup"><span data-stu-id="af4d4-243">By updating specific statistics objects, you can minimize the time and resources required to manage statistics.</span></span> <span data-ttu-id="af4d4-244">Ehhez szükséges néhány gondolat, azonban a legjobb statisztika objektumokat frissíteni.</span><span class="sxs-lookup"><span data-stu-id="af4d4-244">This requires some thought, though, to choose the best statistics objects to update.</span></span>

### <a name="b-update-all-statistics-on-a-table"></a><span data-ttu-id="af4d4-245">B.</span><span class="sxs-lookup"><span data-stu-id="af4d4-245">B.</span></span> <span data-ttu-id="af4d4-246">Egy tábla összes statisztika frissítése</span><span class="sxs-lookup"><span data-stu-id="af4d4-246">Update all statistics on a table</span></span>
<span data-ttu-id="af4d4-247">Ez azt jelenti, egy egyszerű módszer a táblán statisztika objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-247">This shows a simple method for updating all the statistics objects on a table.</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

<span data-ttu-id="af4d4-248">Példa:</span><span class="sxs-lookup"><span data-stu-id="af4d4-248">For example:</span></span>

```sql
UPDATE STATISTICS dbo.table1;
```

<span data-ttu-id="af4d4-249">Jelen nyilatkozat is könnyen használható.</span><span class="sxs-lookup"><span data-stu-id="af4d4-249">This statement is easy to use.</span></span> <span data-ttu-id="af4d4-250">Ne feledje azonban ez a táblázat összes statisztika frissíti, és ezért előfordulhat, hogy hajtsa végre a szükségesnél több munkát.</span><span class="sxs-lookup"><span data-stu-id="af4d4-250">Just remember this updates all statistics on the table, and therefore might perform more work than is necessary.</span></span> <span data-ttu-id="af4d4-251">Ha a teljesítmény darabolása nem okoz problémát, ez az egyértelműen a legegyszerűbb és leghatékonyabb módon statisztika naprakészek biztosításához.</span><span class="sxs-lookup"><span data-stu-id="af4d4-251">If the performance is not an issue, this is definitely the easiest and most complete way to guarantee statistics are up-to-date.</span></span>

> [!NOTE]
> <span data-ttu-id="af4d4-252">Egy tábla összes statisztika frissítése, az SQL Data Warehouse minta a táblázat minden egyes statistics megvizsgálja hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="af4d4-252">When updating all statistics on a table, SQL Data Warehouse does a scan to sample the table for each statistics.</span></span> <span data-ttu-id="af4d4-253">Ha a tábla túl nagy, sok oszlopot, és sok statisztika, előfordulhat, hogy hatékonyabb, ha egyéni igények alapján statisztika frissítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-253">If the table is large, has many columns, and many statistics, it might be more efficient to update individual statistics based on need.</span></span>
> 
> 

<span data-ttu-id="af4d4-254">Egy végrehajtásához egy `UPDATE STATISTICS` eljárás tekintse meg a [az ideiglenes táblák] [ Temporary] cikk.</span><span class="sxs-lookup"><span data-stu-id="af4d4-254">For an implementation of an `UPDATE STATISTICS` procedure please see the [Temporary Tables][Temporary] article.</span></span> <span data-ttu-id="af4d4-255">A végrehajtási módszer kis mértékben eltér, a a `CREATE STATISTICS` a fenti eljárás, de a végeredménynek megegyezik.</span><span class="sxs-lookup"><span data-stu-id="af4d4-255">The implementation method is slightly different to the `CREATE STATISTICS` procedure above but the end result is the same.</span></span>

<span data-ttu-id="af4d4-256">Tekintse meg a teljes szintaxissal [Update Statistics] [ Update Statistics] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="af4d4-256">For the full syntax, see [Update Statistics][Update Statistics] on MSDN.</span></span>

## <a name="statistics-metadata"></a><span data-ttu-id="af4d4-257">Statisztika metaadatok</span><span class="sxs-lookup"><span data-stu-id="af4d4-257">Statistics metadata</span></span>
<span data-ttu-id="af4d4-258">Több rendszernézet és függvények, amelyek segítségével található információ a statisztikákat is van.</span><span class="sxs-lookup"><span data-stu-id="af4d4-258">There are several system view and functions that you can use to find information about statistics.</span></span> <span data-ttu-id="af4d4-259">Például láthatja, ha egy statisztika objektum elavult lehet a statisztikák-date függvény használatával statisztika volt utoljára létrehozásakor vagy frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="af4d4-259">For example, you can see if a statistics object might be out-of-date by using the stats-date function to see when statistics were last created or updated.</span></span>

### <a name="catalog-views-for-statistics"></a><span data-ttu-id="af4d4-260">A statisztika katalógusnézetekre</span><span class="sxs-lookup"><span data-stu-id="af4d4-260">Catalog views for statistics</span></span>
<span data-ttu-id="af4d4-261">Ezek a rendszer nézetek statisztika ismertetik:</span><span class="sxs-lookup"><span data-stu-id="af4d4-261">These system views provide information about statistics:</span></span>

| <span data-ttu-id="af4d4-262">Katalógusnézet használatával derítheti ki</span><span class="sxs-lookup"><span data-stu-id="af4d4-262">Catalog View</span></span> | <span data-ttu-id="af4d4-263">Leírás</span><span class="sxs-lookup"><span data-stu-id="af4d4-263">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="af4d4-264">[sys.Columns][sys.columns]</span><span class="sxs-lookup"><span data-stu-id="af4d4-264">[sys.columns][sys.columns]</span></span> |<span data-ttu-id="af4d4-265">Egy sor minden egyes oszlophoz.</span><span class="sxs-lookup"><span data-stu-id="af4d4-265">One row for each column.</span></span> |
| <span data-ttu-id="af4d4-266">[sys.Objects][sys.objects]</span><span class="sxs-lookup"><span data-stu-id="af4d4-266">[sys.objects][sys.objects]</span></span> |<span data-ttu-id="af4d4-267">Egy sor minden objektum az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="af4d4-267">One row for each object in the database.</span></span> |
| <span data-ttu-id="af4d4-268">[sys.schemas][sys.schemas]</span><span class="sxs-lookup"><span data-stu-id="af4d4-268">[sys.schemas][sys.schemas]</span></span> |<span data-ttu-id="af4d4-269">Egy sor minden séma az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="af4d4-269">One row for each schema in the database.</span></span> |
| <span data-ttu-id="af4d4-270">[sys.stats][sys.stats]</span><span class="sxs-lookup"><span data-stu-id="af4d4-270">[sys.stats][sys.stats]</span></span> |<span data-ttu-id="af4d4-271">Egy sor minden egyes statisztika objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="af4d4-271">One row for each statistics object.</span></span> |
| <span data-ttu-id="af4d4-272">[sys.stats_columns][sys.stats_columns]</span><span class="sxs-lookup"><span data-stu-id="af4d4-272">[sys.stats_columns][sys.stats_columns]</span></span> |<span data-ttu-id="af4d4-273">A statisztika objektum minden egyes oszlopának egy sort.</span><span class="sxs-lookup"><span data-stu-id="af4d4-273">One row for each column in the statistics object.</span></span> <span data-ttu-id="af4d4-274">Vissza a sys.columns mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="af4d4-274">Links back to sys.columns.</span></span> |
| <span data-ttu-id="af4d4-275">[sys.Tables][sys.tables]</span><span class="sxs-lookup"><span data-stu-id="af4d4-275">[sys.tables][sys.tables]</span></span> |<span data-ttu-id="af4d4-276">Egy sor minden táblához (külső táblát tartalmazza).</span><span class="sxs-lookup"><span data-stu-id="af4d4-276">One row for each table (includes external tables).</span></span> |
| <span data-ttu-id="af4d4-277">[sys.table_types][sys.table_types]</span><span class="sxs-lookup"><span data-stu-id="af4d4-277">[sys.table_types][sys.table_types]</span></span> |<span data-ttu-id="af4d4-278">Egy sor egyes adattípusok esetében.</span><span class="sxs-lookup"><span data-stu-id="af4d4-278">One row for each data type.</span></span> |

### <a name="system-functions-for-statistics"></a><span data-ttu-id="af4d4-279">A statisztika rendszer funkciók</span><span class="sxs-lookup"><span data-stu-id="af4d4-279">System functions for statistics</span></span>
<span data-ttu-id="af4d4-280">A rendszer függvények hasznosak statisztika használata:</span><span class="sxs-lookup"><span data-stu-id="af4d4-280">These system functions are useful for working with statistics:</span></span>

| <span data-ttu-id="af4d4-281">Rendszer-funkció</span><span class="sxs-lookup"><span data-stu-id="af4d4-281">System Function</span></span> | <span data-ttu-id="af4d4-282">Leírás</span><span class="sxs-lookup"><span data-stu-id="af4d4-282">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="af4d4-283">[STATS_DATE][STATS_DATE]</span><span class="sxs-lookup"><span data-stu-id="af4d4-283">[STATS_DATE][STATS_DATE]</span></span> |<span data-ttu-id="af4d4-284">A statisztika objektum utolsó módosításának dátuma.</span><span class="sxs-lookup"><span data-stu-id="af4d4-284">Date the statistics object was last updated.</span></span> |
| <span data-ttu-id="af4d4-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span><span class="sxs-lookup"><span data-stu-id="af4d4-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span></span> |<span data-ttu-id="af4d4-286">Értékek terjesztési összefoglaló szintű és részletes információkat biztosít a statisztika objektum azt értelmezni tudja módon.</span><span class="sxs-lookup"><span data-stu-id="af4d4-286">Provides summary level and detailed information about the distribution of values as understood by the statistics object.</span></span> |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a><span data-ttu-id="af4d4-287">Statisztika oszlopok és funkciók egyesítése egy nézet</span><span class="sxs-lookup"><span data-stu-id="af4d4-287">Combine statistics columns and functions into one view</span></span>
<span data-ttu-id="af4d4-288">Ez a nézet számos lehetőséget kínál a statisztika kapcsolódó oszlopok, és a [STATS_DATE()] [] függvény együtt annak az eredménye.</span><span class="sxs-lookup"><span data-stu-id="af4d4-288">This view brings columns that relate to statistics, and results from the [STATS_DATE()][]function together.</span></span>

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a><span data-ttu-id="af4d4-289">DBCC SHOW_STATISTICS() példák</span><span class="sxs-lookup"><span data-stu-id="af4d4-289">DBCC SHOW_STATISTICS() examples</span></span>
<span data-ttu-id="af4d4-290">DBCC SHOW_STATISTICS() statisztika objektumon belül tárolt adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="af4d4-290">DBCC SHOW_STATISTICS() shows the data held within a statistics object.</span></span> <span data-ttu-id="af4d4-291">Ezek az adatok származnak három részből áll.</span><span class="sxs-lookup"><span data-stu-id="af4d4-291">This data comes in three parts.</span></span>

1. <span data-ttu-id="af4d4-292">Fejléc</span><span class="sxs-lookup"><span data-stu-id="af4d4-292">Header</span></span>
2. <span data-ttu-id="af4d4-293">Sűrűség vektoros</span><span class="sxs-lookup"><span data-stu-id="af4d4-293">Density Vector</span></span>
3. <span data-ttu-id="af4d4-294">Hisztogram</span><span class="sxs-lookup"><span data-stu-id="af4d4-294">Histogram</span></span>

<span data-ttu-id="af4d4-295">A statisztika fejléc metaadatait.</span><span class="sxs-lookup"><span data-stu-id="af4d4-295">The header metadata about the statistics.</span></span> <span data-ttu-id="af4d4-296">A hisztogram statisztika objektum első kulcsoszlop értékeinek eloszlását jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="af4d4-296">The histogram displays the distribution of values in the first key column of the statistics object.</span></span> <span data-ttu-id="af4d4-297">A sűrűség vektor kereszt-oszlop korrelációs méri.</span><span class="sxs-lookup"><span data-stu-id="af4d4-297">The density vector measures cross-column correlation.</span></span> <span data-ttu-id="af4d4-298">SQLDW kiszámítja a statisztika objektumban adatokat a számosság becsléseket.</span><span class="sxs-lookup"><span data-stu-id="af4d4-298">SQLDW computes cardinality estimates with any of the data in the statistics object.</span></span>

### <a name="show-header-density-and-histogram"></a><span data-ttu-id="af4d4-299">Fejléc, sűrűség és hisztogram megjelenítése</span><span class="sxs-lookup"><span data-stu-id="af4d4-299">Show header, density, and histogram</span></span>
<span data-ttu-id="af4d4-300">Az egyszerű példában statisztika objektum összes három részből.</span><span class="sxs-lookup"><span data-stu-id="af4d4-300">This simple example shows all three parts of a statistics object.</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

<span data-ttu-id="af4d4-301">Példa:</span><span class="sxs-lookup"><span data-stu-id="af4d4-301">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a><span data-ttu-id="af4d4-302">DBCC SHOW_STATISTICS(); egy vagy több részei megjelenítése</span><span class="sxs-lookup"><span data-stu-id="af4d4-302">Show one or more parts of DBCC SHOW_STATISTICS();</span></span>
<span data-ttu-id="af4d4-303">Ha érdekli csak megtekintés részét, használja a `WITH` záradék, és adja meg a megjeleníteni kívánt részeket:</span><span class="sxs-lookup"><span data-stu-id="af4d4-303">If you are only interested in viewing specific parts, use the `WITH` clause and specify which parts you want to see:</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

<span data-ttu-id="af4d4-304">Példa:</span><span class="sxs-lookup"><span data-stu-id="af4d4-304">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a><span data-ttu-id="af4d4-305">DBCC SHOW_STATISTICS() különbségek</span><span class="sxs-lookup"><span data-stu-id="af4d4-305">DBCC SHOW_STATISTICS() differences</span></span>
<span data-ttu-id="af4d4-306">DBCC SHOW_STATISTICS() szigorúbban végrehajtani az SQL Data Warehouse az SQL Server képest.</span><span class="sxs-lookup"><span data-stu-id="af4d4-306">DBCC SHOW_STATISTICS() is more strictly implemented in SQL Data Warehouse compared to SQL Server.</span></span>

1. <span data-ttu-id="af4d4-307">Nem dokumentált funkciók nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="af4d4-307">Undocumented features are not supported</span></span>
2. <span data-ttu-id="af4d4-308">Nem használható a Stats_stream</span><span class="sxs-lookup"><span data-stu-id="af4d4-308">Cannot use Stats_stream</span></span>
3. <span data-ttu-id="af4d4-309">Nem tudja csatlakoztatni meghatározott fájlcsoportokat statisztikai adatok eredményeit pl. (STAT_HEADER ILLESZTÉSI DENSITY_VECTOR)</span><span class="sxs-lookup"><span data-stu-id="af4d4-309">Cannot join results for specific subsets of statistics data e.g. (STAT_HEADER JOIN DENSITY_VECTOR)</span></span>
4. <span data-ttu-id="af4d4-310">Üzenet tiltási NO_INFOMSGS nem állítható be</span><span class="sxs-lookup"><span data-stu-id="af4d4-310">NO_INFOMSGS cannot be set for message suppression</span></span>
5. <span data-ttu-id="af4d4-311">Statisztika neveket burkoló szögletes zárójelek között nem használható.</span><span class="sxs-lookup"><span data-stu-id="af4d4-311">Square brackets around statistics names cannot be used</span></span>
6. <span data-ttu-id="af4d4-312">Nem használható oszlopnevek statisztika objektumok azonosítása</span><span class="sxs-lookup"><span data-stu-id="af4d4-312">Cannot use column names to identify statistics objects</span></span>
7. <span data-ttu-id="af4d4-313">Egyéni hiba 2767 nem támogatott</span><span class="sxs-lookup"><span data-stu-id="af4d4-313">Custom error 2767 is not supported</span></span>

## <a name="next-steps"></a><span data-ttu-id="af4d4-314">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af4d4-314">Next steps</span></span>
<span data-ttu-id="af4d4-315">További részletekért lásd: [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="af4d4-315">For more details, see [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] on MSDN.</span></span>  <span data-ttu-id="af4d4-316">További tudnivalókért tekintse meg a cikkek [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [terjesztése egy tábla][Distribute], [tábla indexelő][Index], [tábla particionáló] [ Partition] és [az ideiglenes táblák][Temporary].</span><span class="sxs-lookup"><span data-stu-id="af4d4-316">To learn more, see the articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="af4d4-317">Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="af4d4-317">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  

---
title: "az SQL Data Warehouse táblák aaaManaging statisztikák |} Microsoft Docs"
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
ms.openlocfilehash: c9521dc47891f68d124e77a53e2e15d03275caaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a><span data-ttu-id="d5525-103">Az SQL Data Warehouse táblák statisztikák kezelése</span><span class="sxs-lookup"><span data-stu-id="d5525-103">Managing statistics on tables in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="d5525-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="d5525-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="d5525-105">[Adattípusok][Data Types]</span><span class="sxs-lookup"><span data-stu-id="d5525-105">[Data Types][Data Types]</span></span>
> * <span data-ttu-id="d5525-106">[Terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="d5525-106">[Distribute][Distribute]</span></span>
> * <span data-ttu-id="d5525-107">[Index][Index]</span><span class="sxs-lookup"><span data-stu-id="d5525-107">[Index][Index]</span></span>
> * <span data-ttu-id="d5525-108">[Partíció][Partition]</span><span class="sxs-lookup"><span data-stu-id="d5525-108">[Partition][Partition]</span></span>
> * <span data-ttu-id="d5525-109">[Statisztika][Statistics]</span><span class="sxs-lookup"><span data-stu-id="d5525-109">[Statistics][Statistics]</span></span>
> * <span data-ttu-id="d5525-110">[Ideiglenes][Temporary]</span><span class="sxs-lookup"><span data-stu-id="d5525-110">[Temporary][Temporary]</span></span>
> 
> 

<span data-ttu-id="d5525-111">hello további SQL Data Warehouse ismer az adatok hello gyorsabban azt is lekérdezheti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d5525-111">hello more SQL Data Warehouse knows about your data, hello faster it can execute queries against your data.</span></span>  <span data-ttu-id="d5525-112">Hello, amelyek biztosítják az SQL Data Warehouse szolgál az adatokról módja statisztikája az adatok gyűjtésével.</span><span class="sxs-lookup"><span data-stu-id="d5525-112">hello way that you tell SQL Data Warehouse about your data, is by collecting statistics about your data.</span></span>  <span data-ttu-id="d5525-113">Statisztika az adatokról, akkor az egyik legfontosabb dolog hello teheti toooptimize a lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="d5525-113">Having statistics on your data is one of hello most important things you can do toooptimize your queries.</span></span>  <span data-ttu-id="d5525-114">Statisztika SQL Data Warehouse hello legoptimálisabb terv a lekérdezések létrehozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d5525-114">Statistics help SQL Data Warehouse create hello most optimal plan for your queries.</span></span>  <span data-ttu-id="d5525-115">Ennek az az oka optimalizáló költségekkel hello SQL Data Warehouse lekérdezés alapú optimalizáló.</span><span class="sxs-lookup"><span data-stu-id="d5525-115">This is because hello SQL Data Warehouse query optimizer is a cost based optimizer.</span></span>  <span data-ttu-id="d5525-116">Ez azt jelenti, hogy összehasonlítja a különböző lekérdezésterveket hello költségét, és majd úgy dönt, hello terv hello legalacsonyabb költség, amely hello terv, amely végrehajtja a leggyorsabb hello kell.</span><span class="sxs-lookup"><span data-stu-id="d5525-116">That is, it compares hello cost of various query plans and then chooses hello plan with hello lowest cost, which should also be hello plan that will execute hello fastest.</span></span>

<span data-ttu-id="d5525-117">Statisztika is létrehozható, csak egy oszlop, több oszlop vagy táblázat index.</span><span class="sxs-lookup"><span data-stu-id="d5525-117">Statistics can be created on a single column, multiple columns or on an index of a table.</span></span>  <span data-ttu-id="d5525-118">Statisztika, amely rögzíti a hello tartomány- és értékek kiválasztásánál hisztogram tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="d5525-118">Statistics are stored in a histogram which captures hello range and selectivity of values.</span></span>  <span data-ttu-id="d5525-119">Ez akkor különösen fontos, ha hello optimalizáló kell tooevaluate illesztéseket, GROUP BY, HAVING és a WHERE záradék a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="d5525-119">This is of particular interest when hello optimizer needs tooevaluate JOINs, GROUP BY, HAVING and WHERE clauses in a query.</span></span>  <span data-ttu-id="d5525-120">Például ha hello optimalizáló becslése, hogy jelenleg korlátozza a lekérdezés hello dátum 1 sort ad vissza, nagyon eltérő tervezze meg, mint ha az választhatják, azok dátum becslése kijelölt lesz 1 millió sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="d5525-120">For example, if hello optimizer estimates that hello date you are filtering in your query will return 1 row, it may choose a very different plan than if it estimates that they date you have selected will return 1 million rows.</span></span>  <span data-ttu-id="d5525-121">Míg rendkívül fontos a statisztikák létrehozása, is ugyanilyen fontos, hogy statisztika *pontosan* hello tábla hello aktuális állapotát tükrözik.</span><span class="sxs-lookup"><span data-stu-id="d5525-121">While creating statistics is extremely important, it is equally important that statistics *accurately* reflect hello current state of hello table.</span></span>  <span data-ttu-id="d5525-122">Naprakész statisztika biztosítja, hogy egy jó terv hello optimalizáló van-e kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="d5525-122">Having up-to-date statistics ensures that a good plan is selected by hello optimizer.</span></span>  <span data-ttu-id="d5525-123">hello optimalizáló hello tervekhez csak olyan megegyezik az hello statisztikai adatait.</span><span class="sxs-lookup"><span data-stu-id="d5525-123">hello plans created by hello optimizer are only as good as hello statistics on your data.</span></span>

<span data-ttu-id="d5525-124">hello folyamat létrehozása, és frissítse a statisztikai adatokat egy kézi művelet jelenleg azonban nagyon egyszerű toodo.</span><span class="sxs-lookup"><span data-stu-id="d5525-124">hello process of creating and updating statistics is currently a manual process, but is very simple toodo.</span></span>  <span data-ttu-id="d5525-125">Ez nem az SQL Server, mely automatikusan létrehozza, és egyetlen oszlopok és indexek statisztikák frissíti.</span><span class="sxs-lookup"><span data-stu-id="d5525-125">This is unlike SQL Server which automatically creates and updates statistics on single columns and indexes.</span></span>  <span data-ttu-id="d5525-126">Az alábbi részleteket hello segítségével nagy mértékben automatizálható hello felügyeleti hello statisztikák az adatokon.</span><span class="sxs-lookup"><span data-stu-id="d5525-126">By using hello information below, you can greatly automate hello management of hello statistics on your data.</span></span> 

## <a name="getting-started-with-statistics"></a><span data-ttu-id="d5525-127">Ismerkedés a statisztikák</span><span class="sxs-lookup"><span data-stu-id="d5525-127">Getting started with statistics</span></span>
 <span data-ttu-id="d5525-128">Mintavételi statisztikák létrehozása minden oszlop egy egyszerűen tooget parancsot statisztika.</span><span class="sxs-lookup"><span data-stu-id="d5525-128">Creating sampled statistics on every column is an easy way tooget started with statistics.</span></span>  <span data-ttu-id="d5525-129">Mivel ez egyaránt fontos tookeep statisztika naprakész, előfordulhat, hogy a konzervatív megközelítést kell tooupdate a statisztika naponta vagy minden egyes betöltés után.</span><span class="sxs-lookup"><span data-stu-id="d5525-129">Since it is equally important tookeep statistics up-to-date, a conservative approach may be tooupdate your statistics daily or after each load.</span></span> <span data-ttu-id="d5525-130">Mindig akadnak teljesítmény- és hello költség toocreate és frissítési statisztikái közötti kompromisszumot.</span><span class="sxs-lookup"><span data-stu-id="d5525-130">There are always trade-offs between performance and hello cost toocreate and update statistics.</span></span>  <span data-ttu-id="d5525-131">Ha tart túl hosszú toomaintain összes a statisztika, érdemes lehet több szelektív oszlopok statisztika rendelkezik és mely oszlopok tootry toobe kell gyakori frissítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-131">If you find it is taking too long toomaintain all of your statistics, you may want tootry toobe more selective about which columns have statistics or which columns need frequent updating.</span></span>  <span data-ttu-id="d5525-132">Érdemes például tooupdate dátumoszlopának dátumtulajdonságai, naponta, új értékeket vehetők helyett minden betöltés után.</span><span class="sxs-lookup"><span data-stu-id="d5525-132">For example, you might want tooupdate date columns daily, as new values may be added rather than after every load.</span></span> <span data-ttu-id="d5525-133">Ebben az esetben érheti el hello legtöbb juttatás azzal, hogy a statisztika oszlopokon érintett illesztéseket, GROUP BY, HAVING és a WHERE záradék.</span><span class="sxs-lookup"><span data-stu-id="d5525-133">Again, you will gain hello most benefit by having statistics on columns involved in JOINs, GROUP BY, HAVING and WHERE clauses.</span></span>  <span data-ttu-id="d5525-134">Ha sok tábla hello csak használt oszlopok záradék válassza ki, nem segíthetnek, ezen oszlopokon statisztika, és egy kis további elérhető tooidentify költségeik csak az adott statisztika segít, hello oszlopok csökkentheti hello idő toomaintain a statisztika .</span><span class="sxs-lookup"><span data-stu-id="d5525-134">If you have a table with a lot of columns which are only used in hello SELECT clause, statistics on these columns may not help, and spending a little more effort tooidentify only hello columns where statistics will help, can reduce hello time toomaintain your statistics.</span></span>

## <a name="multi-column-statistics"></a><span data-ttu-id="d5525-135">Több oszlop statisztikai adatainak</span><span class="sxs-lookup"><span data-stu-id="d5525-135">Multi-column statistics</span></span>
<span data-ttu-id="d5525-136">Ezenkívül toocreating statisztika egyetlen oszlopokon, előfordulhat, hogy a lekérdezések ki a előnyeit több oszlop statisztikai adatainak.</span><span class="sxs-lookup"><span data-stu-id="d5525-136">In addition toocreating statistics on single columns, you may find that your queries will benefit from multi-column statistics.</span></span>  <span data-ttu-id="d5525-137">Több oszlop statisztikákat statisztikákat létrehozni az oszlopok listája.</span><span class="sxs-lookup"><span data-stu-id="d5525-137">Multi-column statistics are statistics created on a list of columns.</span></span>  <span data-ttu-id="d5525-138">Egyetlen oszlop statisztikai adatainak hello első oszlop hello listában tartalmaznak, valamint bizonyos kereszt-oszlop korrelációs adatokat nevű sűrűség.</span><span class="sxs-lookup"><span data-stu-id="d5525-138">They include single column statistics on hello first column in hello list, plus some cross-column correlation information called densities.</span></span>  <span data-ttu-id="d5525-139">Például ha egy táblázatot, amelyhez csatlakozik tooanother két oszlopokon, előfordulhat, hogy az SQL Data Warehouse jobban is optimalizálhatja hello terv, támogatja a hello kapcsolat két oszlop között.</span><span class="sxs-lookup"><span data-stu-id="d5525-139">For example, if you have a table that joins tooanother on two columns, you may find that SQL Data Warehouse can better optimize hello plan if it understands hello relationship between two columns.</span></span>   <span data-ttu-id="d5525-140">Több oszlop statisztikai adatainak javíthatja a lekérdezések teljesítményét az egyes műveletek, például összetett illesztések és a csoportosítás alapját.</span><span class="sxs-lookup"><span data-stu-id="d5525-140">Multi-column statistics can improve query performance for some operations such as composite joins and group by.</span></span>

## <a name="updating-statistics"></a><span data-ttu-id="d5525-141">Frissítse a statisztikai adatokat</span><span class="sxs-lookup"><span data-stu-id="d5525-141">Updating statistics</span></span>
<span data-ttu-id="d5525-142">Frissítse a statisztikai adatokat az adatbázis-felügyeleti rutin fontos részét képezi.</span><span class="sxs-lookup"><span data-stu-id="d5525-142">Updating statistics is an important part of your database management routine.</span></span>  <span data-ttu-id="d5525-143">Hello terjesztési hello adatbázis adatok megváltozásakor statisztika kell toobe frissítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-143">When hello distribution of data in hello database changes, statistics need toobe updated.</span></span>  <span data-ttu-id="d5525-144">Elévült statisztikát toosub optimális lekérdezési teljesítmény irányítja.</span><span class="sxs-lookup"><span data-stu-id="d5525-144">Out-of-date statistics will lead toosub-optimal query performance.</span></span>

<span data-ttu-id="d5525-145">Egy ajánlott dátumoszlopának dátumtulajdonságai tooupdate statisztikák minden nap új dátumok hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="d5525-145">One best practice is tooupdate statistics on date columns each day as new dates are added.</span></span>  <span data-ttu-id="d5525-146">Minden alkalommal új sorok töltődnek be a hello data warehouse-ba, új betöltése vagy tranzakció kerülnek.</span><span class="sxs-lookup"><span data-stu-id="d5525-146">Each time new rows are loaded into hello data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="d5525-147">Ezek hello adatok terjesztési módosítsa, majd ellenőrizze hello statisztika elavult.</span><span class="sxs-lookup"><span data-stu-id="d5525-147">These change hello data distribution and make hello statistics out-of-date.</span></span> <span data-ttu-id="d5525-148">Ezzel szemben ország oszlop egy felhasználói tábla statisztikai előfordulhat, hogy soha nem kell toobe frissítve, hello terjesztési értékek általában nem változik.</span><span class="sxs-lookup"><span data-stu-id="d5525-148">Conversely, statistics on a country column in a customer table might never need toobe updated, as hello distribution of values doesn’t generally change.</span></span> <span data-ttu-id="d5525-149">Feltéve, hogy hello terjesztési állandó, az ügyfelek között, hozzáadása új sorok toohello tábla változat nem fog toochange hello adatok terjesztési.</span><span class="sxs-lookup"><span data-stu-id="d5525-149">Assuming hello distribution is constant between customers, adding new rows toohello table variation isn't going toochange hello data distribution.</span></span> <span data-ttu-id="d5525-150">Azonban ha az adatraktár csak tartalmaz egy országon, és az adatok egy új országból kapcsolná, az adatok tárolását, több országokból származó akkor mindenképpen szüksége tooupdate statisztikák hello ország oszlop.</span><span class="sxs-lookup"><span data-stu-id="d5525-150">However, if your data warehouse only contains one country and you bring in data from a new country, resulting in data from multiple countries being stored, then you definitely need tooupdate statistics on hello country column.</span></span>

<span data-ttu-id="d5525-151">Hello első kérdések tooask lekérdezés hibaelhárítás esetén egyik "naprakészek hello statisztika?"</span><span class="sxs-lookup"><span data-stu-id="d5525-151">One of hello first questions tooask when troubleshooting a query is, "Are hello statistics up-to-date?"</span></span>

<span data-ttu-id="d5525-152">Ezt a kérdést egyike nem tud válaszolni hello kor hello adatok.</span><span class="sxs-lookup"><span data-stu-id="d5525-152">This question is not one that can be answered by hello age of hello data.</span></span> <span data-ttu-id="d5525-153">Lehet, hogy nagyon régi, ha nincs jelentős változás toohello alapjául szolgáló adatok naprakészek toodate statisztika objektum.</span><span class="sxs-lookup"><span data-stu-id="d5525-153">An up toodate statistics object could be very old if there's been no material change toohello underlying data.</span></span> <span data-ttu-id="d5525-154">Ha a sorok számát hello jelentősen módosult, vagy nincs az adott oszlop értékeinek hello elosztása jelentős változás *majd* tooupdate statisztikája.</span><span class="sxs-lookup"><span data-stu-id="d5525-154">When hello number of rows has changed substantially or there is a material change in hello distribution of values for a given column *then* it's time tooupdate statistics.</span></span>  

<span data-ttu-id="d5525-155">Referenciaként **SQL Server** (nem az SQL Data Warehouse) automatikusan frissíti a statisztikákat ilyen helyzetekben:</span><span class="sxs-lookup"><span data-stu-id="d5525-155">For reference, **SQL Server** (not SQL Data Warehouse) automatically updates statistics for these situations:</span></span>

* <span data-ttu-id="d5525-156">Ha egyetlen sor hello táblázatban szereplő sorok hozzáadásakor, fog kapni a statisztikák automatikus frissítés</span><span class="sxs-lookup"><span data-stu-id="d5525-156">If you have zero rows in hello table, when you add rows, you’ll get an automatic update of statistics</span></span>
* <span data-ttu-id="d5525-157">Sorok tooa 500-nál több tábla kezdődő, és kevesebb, mint 500 sorok hozzáadásakor (például a start rendelkezik 499, és adja meg az 500 sorok tooa összesen 999 sorok), az automatikus frissítés jelenik meg</span><span class="sxs-lookup"><span data-stu-id="d5525-157">When you add more than 500 rows tooa table starting with less than 500 rows (e.g. at start you have 499 and then add 500 rows tooa total of 999 rows), you’ll get an automatic update</span></span> 
* <span data-ttu-id="d5525-158">500 sorok közben fog tooadd 500 további sorokat + 20 %-át hello tábla hello méretét a hello statisztikák láthatja az automatikus frissítés előtt</span><span class="sxs-lookup"><span data-stu-id="d5525-158">Once you’re over 500 rows you will have tooadd 500 additional rows + 20% of hello size of hello table before you’ll see an automatic update on hello stats</span></span>

<span data-ttu-id="d5525-159">Mivel nincs nincs DMV toodetermine Ha hello táblázatban levő adatok hello utolsó idő statisztika frissítése óta megváltozott, a statisztika hello korát ismerete adja meg a hello kép része.</span><span class="sxs-lookup"><span data-stu-id="d5525-159">Since there is no DMV toodetermine if data within hello table has changed since hello last time statistics were updated, knowing hello age of your statistics can provide you with part of hello picture.</span></span>  <span data-ttu-id="d5525-160">A következő lekérdezés toodetermine hello legutóbbi a statisztika hello is használhatja, ahol minden táblában frissített.</span><span class="sxs-lookup"><span data-stu-id="d5525-160">You can use hello following query toodetermine hello last time your statistics where updated on each table.</span></span>  

> [!NOTE]
> <span data-ttu-id="d5525-161">Ne feledje, hogy az adott oszlop értékeinek eloszlását hello jelentős változás esetén frissítse függetlenül hello statisztika legutóbbi frissítése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="d5525-161">Remember if there is a material change in hello distribution of values for a given column, you should update statistics regardless of hello last time they were updated.</span></span>  
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

<span data-ttu-id="d5525-162">Egy adatraktár dátumoszlopának dátumtulajdonságai például általában kell gyakori statisztikai adatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-162">Date columns in a data warehouse, for example, usually need frequent statistics updates.</span></span> <span data-ttu-id="d5525-163">Minden alkalommal új sorok töltődnek be a hello data warehouse-ba, új betöltése vagy tranzakció kerülnek.</span><span class="sxs-lookup"><span data-stu-id="d5525-163">Each time new rows are loaded into hello data warehouse, new load dates or transaction dates are added.</span></span> <span data-ttu-id="d5525-164">Ezek hello adatok terjesztési módosítsa, majd ellenőrizze hello statisztika elavult.</span><span class="sxs-lookup"><span data-stu-id="d5525-164">These change hello data distribution and make hello statistics out-of-date.</span></span>  <span data-ttu-id="d5525-165">Ezzel szemben az ügyfél táblán nemét oszlop statisztikai előfordulhat, hogy soha nem kell toobe frissítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-165">Conversely, statistics on a gender column on a customer table might never need toobe updated.</span></span> <span data-ttu-id="d5525-166">Feltéve, hogy hello terjesztési állandó, az ügyfelek között, hozzáadása új sorok toohello tábla változat nem fog toochange hello adatok terjesztési.</span><span class="sxs-lookup"><span data-stu-id="d5525-166">Assuming hello distribution is constant between customers, adding new rows toohello table variation isn't going toochange hello data distribution.</span></span> <span data-ttu-id="d5525-167">Azonban ha az adatraktár csak tartalmaz egy nemét, és több genders egy új követelményt eredményezi majd mindenképpen kell tooupdate statisztikák hello nemét oszlop.</span><span class="sxs-lookup"><span data-stu-id="d5525-167">However, if your data warehouse only contains one gender and a new requirement results in multiple genders then you definitely need tooupdate statistics on hello gender column.</span></span>

<span data-ttu-id="d5525-168">További ismertetése [statisztika] [ Statistics] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="d5525-168">For further explanation, see [Statistics][Statistics] on MSDN.</span></span>

## <a name="implementing-statistics-management"></a><span data-ttu-id="d5525-169">Végrehajtási statisztika kezelése</span><span class="sxs-lookup"><span data-stu-id="d5525-169">Implementing statistics management</span></span>
<span data-ttu-id="d5525-170">Gyakran egy jó ötlet tooextend az adatok betöltése a frissített statisztika folyamat tooensure hello hello terhelés végét.</span><span class="sxs-lookup"><span data-stu-id="d5525-170">It is often a good idea tooextend your data loading process tooensure that statistics are updated at hello end of hello load.</span></span> <span data-ttu-id="d5525-171">hello adatbetöltés akkor, ha a táblák leggyakrabban módosítása a méretük és/vagy a terjesztési értékek.</span><span class="sxs-lookup"><span data-stu-id="d5525-171">hello data load is when tables most frequently change their size and/or their distribution of values.</span></span> <span data-ttu-id="d5525-172">Ezért ez az a logikai hely tooimplement egyes felügyeleti folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="d5525-172">Therefore, this is a logical place tooimplement some management processes.</span></span>

<span data-ttu-id="d5525-173">Néhány alapelvek alább a statisztika frissítési hello betöltése során:</span><span class="sxs-lookup"><span data-stu-id="d5525-173">Some guiding principles are provided below for updating your statistics during hello load process:</span></span>

* <span data-ttu-id="d5525-174">Győződjön meg arról, hogy rendelkezik-e frissíteni kell legalább egy statisztika objektum minden egyes betöltött táblákon.</span><span class="sxs-lookup"><span data-stu-id="d5525-174">Ensure that each loaded table has at least one statistics object updated.</span></span> <span data-ttu-id="d5525-175">A frissítések hello táblák (sorok számát és oldalszám) információi hello statisztikák frissítés részeként.</span><span class="sxs-lookup"><span data-stu-id="d5525-175">This updates hello tables size (row count and page count) information as part of hello stats update.</span></span>
* <span data-ttu-id="d5525-176">Összpontosítson az ILLESZTÉS, GROUP BY, ORDER BY és DISTINCT záradékban részt vevő oszlopokat</span><span class="sxs-lookup"><span data-stu-id="d5525-176">Focus on columns participating in JOIN, GROUP BY, ORDER BY and DISTINCT clauses</span></span>
* <span data-ttu-id="d5525-177">Fontolja meg "kulcs növekvő" oszlopok például tranzakció dátumok gyakrabban, mivel ezek az értékek nem fog szerepelni hello statisztika hisztogram.</span><span class="sxs-lookup"><span data-stu-id="d5525-177">Consider updating "ascending key" columns such as transaction dates more frequently as these values will not be included in hello statistics histogram.</span></span>
* <span data-ttu-id="d5525-178">Érdemes lehet, statikus terjesztési oszlopok gyakran frissíteni.</span><span class="sxs-lookup"><span data-stu-id="d5525-178">Consider updating static distribution columns less frequently.</span></span>
* <span data-ttu-id="d5525-179">Ne feledje, hogy minden statisztikai adat objektum sorozat frissül.</span><span class="sxs-lookup"><span data-stu-id="d5525-179">Remember each statistic object is updated in series.</span></span> <span data-ttu-id="d5525-180">Egyszerűen végrehajtási `UPDATE STATISTICS <TABLE_NAME>` nem mindig ideális megoldás – különösen a statisztika objektumok sok nagy táblák esetében.</span><span class="sxs-lookup"><span data-stu-id="d5525-180">Simply implementing `UPDATE STATISTICS <TABLE_NAME>` may not be ideal - especially for wide tables with lots of statistics objects.</span></span>

> [!NOTE]
> <span data-ttu-id="d5525-181">[Növekvő kulcs] vonatkozó részletes információért tekintse meg az SQL Server 2014 toohello számossága becslés modell tanulmány.</span><span class="sxs-lookup"><span data-stu-id="d5525-181">For more details on [ascending key] please refer toohello SQL Server 2014 cardinality estimation model whitepaper.</span></span>
> 
> 

<span data-ttu-id="d5525-182">További ismertetése [számossága becslés] [ Cardinality Estimation] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="d5525-182">For further explanation, see  [Cardinality Estimation][Cardinality Estimation] on MSDN.</span></span>

## <a name="examples-create-statistics"></a><span data-ttu-id="d5525-183">Példák: Statisztikák létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5525-183">Examples: Create statistics</span></span>
<span data-ttu-id="d5525-184">Ezek a példák azt szemléltetik, hogyan toouse statisztika létrehozásának különböző beállításait.</span><span class="sxs-lookup"><span data-stu-id="d5525-184">These examples show how toouse various options for creating statistics.</span></span> <span data-ttu-id="d5525-185">használhatja az egyes oszlopok hello-beállítások az adatok jellemzői hello és hello oszlop a lekérdezésekben használt hogyan függ.</span><span class="sxs-lookup"><span data-stu-id="d5525-185">hello options that you use for each column depend on hello characteristics of your data and how hello column will be used in queries.</span></span>

### <a name="a-create-single-column-statistics-with-default-options"></a><span data-ttu-id="d5525-186">A.</span><span class="sxs-lookup"><span data-stu-id="d5525-186">A.</span></span> <span data-ttu-id="d5525-187">Egy oszlop statisztikák létrehozása az alapértelmezett beállításokkal</span><span class="sxs-lookup"><span data-stu-id="d5525-187">Create single-column statistics with default options</span></span>
<span data-ttu-id="d5525-188">egy oszlop toocreate statisztikák egyszerűen adjon hello statisztika objektum nevét hello hello oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="d5525-188">toocreate statistics on a column, simply provide a name for hello statistics object and hello name of hello column.</span></span>

<span data-ttu-id="d5525-189">Ez a szintaxis hello alapértelmezett beállításokat használja.</span><span class="sxs-lookup"><span data-stu-id="d5525-189">This syntax uses all of hello default options.</span></span> <span data-ttu-id="d5525-190">Alapértelmezés szerint az SQL Data Warehouse hello tábla 20 százalékát minták, amikor létrehozza statisztika.</span><span class="sxs-lookup"><span data-stu-id="d5525-190">By default, SQL Data Warehouse samples 20 percent of hello table when it creates statistics.</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

<span data-ttu-id="d5525-191">Példa:</span><span class="sxs-lookup"><span data-stu-id="d5525-191">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a><span data-ttu-id="d5525-192">B.</span><span class="sxs-lookup"><span data-stu-id="d5525-192">B.</span></span> <span data-ttu-id="d5525-193">Minden sor megvizsgálásával egyoszlopos statisztikát létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5525-193">Create single-column statistics by examining every row</span></span>
<span data-ttu-id="d5525-194">hello alapértelmezett mintavételi 20 százalékos aránya a legtöbb esetben elegendő.</span><span class="sxs-lookup"><span data-stu-id="d5525-194">hello default sampling rate of 20 percent is sufficient for most situations.</span></span> <span data-ttu-id="d5525-195">Hello mintavételi ráta azonban módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d5525-195">However, you can adjust hello sampling rate.</span></span>

<span data-ttu-id="d5525-196">teljes toosample hello table, a következő szintaxist használja:</span><span class="sxs-lookup"><span data-stu-id="d5525-196">toosample hello full table, use this syntax:</span></span>

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

<span data-ttu-id="d5525-197">Példa:</span><span class="sxs-lookup"><span data-stu-id="d5525-197">For example:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-hello-sample-size"></a><span data-ttu-id="d5525-198">C.</span><span class="sxs-lookup"><span data-stu-id="d5525-198">C.</span></span> <span data-ttu-id="d5525-199">Hozza létre a egyoszlopos statisztikát hello mintaméret megadásával</span><span class="sxs-lookup"><span data-stu-id="d5525-199">Create single-column statistics by specifying hello sample size</span></span>
<span data-ttu-id="d5525-200">Másik lehetőségként százalékban hello mintaméret adhat meg:</span><span class="sxs-lookup"><span data-stu-id="d5525-200">Alternatively, you can specify hello sample size as a percent:</span></span>

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-hello-rows"></a><span data-ttu-id="d5525-201">D.</span><span class="sxs-lookup"><span data-stu-id="d5525-201">D.</span></span> <span data-ttu-id="d5525-202">Egyoszlopos statisztikát csak egyes hello sorok létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5525-202">Create single-column statistics on only some of hello rows</span></span>
<span data-ttu-id="d5525-203">Egy másik lehetőség, létrehozhat statisztika hello sorok része a táblában.</span><span class="sxs-lookup"><span data-stu-id="d5525-203">Another option, you can create statistics on a portion of hello rows in your table.</span></span> <span data-ttu-id="d5525-204">Ezt nevezik a szűrt statisztikai.</span><span class="sxs-lookup"><span data-stu-id="d5525-204">This is called a filtered statistic.</span></span>

<span data-ttu-id="d5525-205">Használhat például szűrt statisztikákat egy adott partícióra egy nagy particionált tábla tooquery tervezése során.</span><span class="sxs-lookup"><span data-stu-id="d5525-205">For example, you could use filtered statistics when you plan tooquery a specific partition of a large partitioned table.</span></span> <span data-ttu-id="d5525-206">Létrehozásával statisztikák csak hello partíció értékek, hello statisztika hello pontossága fog javítása, és ezért javíthatja a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d5525-206">By creating statistics on only hello partition values, hello accuracy of hello statistics will improve, and therefore improve query performance.</span></span>

<span data-ttu-id="d5525-207">Ez a Példa statisztika értéktartománya hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d5525-207">This example creates statistics on a range of values.</span></span> <span data-ttu-id="d5525-208">hello értékek könnyen lehet meghatározott értékek tartományán toomatch hello partíció.</span><span class="sxs-lookup"><span data-stu-id="d5525-208">hello values could easily be defined toomatch hello range of values in a partition.</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> <span data-ttu-id="d5525-209">Hello lekérdezés optimalizáló tooconsider szűrt statisztikákat használja, akkor azt úgy dönt, hogy hello elosztott lekérdezésterv, a hello lekérdezés hello hello statisztika objektum meghatározását kell férnie.</span><span class="sxs-lookup"><span data-stu-id="d5525-209">For hello query optimizer tooconsider using filtered statistics when it chooses hello distributed query plan, hello query must fit inside hello definition of hello statistics object.</span></span> <span data-ttu-id="d5525-210">Hello előző példában hello lekérdezés amikor záradékban kell toospecify Oszlop1 értékek között 2000101 és 20001231 használatával.</span><span class="sxs-lookup"><span data-stu-id="d5525-210">Using hello previous example, hello query's where clause needs toospecify col1 values between 2000101 and 20001231.</span></span>
> 
> 

### <a name="e-create-single-column-statistics-with-all-hello-options"></a><span data-ttu-id="d5525-211">E.</span><span class="sxs-lookup"><span data-stu-id="d5525-211">E.</span></span> <span data-ttu-id="d5525-212">Hozzon létre egyoszlopos statisztikát összes hello-beállítások</span><span class="sxs-lookup"><span data-stu-id="d5525-212">Create single-column statistics with all hello options</span></span>
<span data-ttu-id="d5525-213">Természetesen kombinálhatja hello beállítások együtt.</span><span class="sxs-lookup"><span data-stu-id="d5525-213">You can, of course, combine hello options together.</span></span> <span data-ttu-id="d5525-214">az alábbi példa hello egy szűrt statisztikákat objektum egyéni mintaméret hoz létre:</span><span class="sxs-lookup"><span data-stu-id="d5525-214">hello example below creates a filtered statistics object with a custom sample size:</span></span>

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="d5525-215">Hello teljes referenciáért lásd: [CREATE statistics UTASÍTÁSHOZ] [ CREATE STATISTICS] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="d5525-215">For hello full reference, see [CREATE STATISTICS][CREATE STATISTICS] on MSDN.</span></span>

### <a name="f-create-multi-column-statistics"></a><span data-ttu-id="d5525-216">F.</span><span class="sxs-lookup"><span data-stu-id="d5525-216">F.</span></span> <span data-ttu-id="d5525-217">Több oszlop statisztikai adatainak létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5525-217">Create multi-column statistics</span></span>
<span data-ttu-id="d5525-218">toocreate többoszlopos statisztika, egyszerűen hello előző példák használja, de további oszlopok megadása.</span><span class="sxs-lookup"><span data-stu-id="d5525-218">toocreate a multi-column statistics, simply use hello previous examples, but specify more columns.</span></span>

> [!NOTE]
> <span data-ttu-id="d5525-219">hello hisztogram, amely csak akkor hello lekérdezési eredményhez, sorainak száma tooestimate hello hello statisztika Objektumdefiníció szereplő első oszlop.</span><span class="sxs-lookup"><span data-stu-id="d5525-219">hello histogram, which is used tooestimate number of rows in hello query result, is only available for hello first column listed in hello statistics object definition.</span></span>
> 
> 

<span data-ttu-id="d5525-220">Ebben a példában hello hisztogram van *termék\_kategória*.</span><span class="sxs-lookup"><span data-stu-id="d5525-220">In this example, hello histogram is on *product\_category*.</span></span> <span data-ttu-id="d5525-221">Kereszt-oszlop statisztikai adatainak kiszámítása *termék\_kategória* és *termék\_sub_c\ategory*:</span><span class="sxs-lookup"><span data-stu-id="d5525-221">Cross-column statistics are calculated on *product\_category* and *product\_sub_c\ategory*:</span></span>

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

<span data-ttu-id="d5525-222">Mivel közötti összefüggés *termék\_kategória* és *termék\_sub\_kategória*, a többoszlopos stat akkor lehet hasznos, ha ezekben az oszlopokban érhetők el a hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="d5525-222">Since there is a correlation between *product\_category* and *product\_sub\_category*, a multi-column stat can be useful if these columns are accessed at hello same time.</span></span>

### <a name="g-create-statistics-on-all-hello-columns-in-a-table"></a><span data-ttu-id="d5525-223">G.</span><span class="sxs-lookup"><span data-stu-id="d5525-223">G.</span></span> <span data-ttu-id="d5525-224">Egy táblázat összes hello oszlopa statisztikák létrehozása</span><span class="sxs-lookup"><span data-stu-id="d5525-224">Create statistics on all hello columns in a table</span></span>
<span data-ttu-id="d5525-225">Egyirányú toocreate statisztika tooissues CREATE statistics UTASÍTÁSHOZ parancsok hello tábla létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="d5525-225">One way toocreate statistics is tooissues CREATE STATISTICS commands after creating hello table.</span></span>

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

### <a name="h-use-a-stored-procedure-toocreate-statistics-on-all-columns-in-a-database"></a><span data-ttu-id="d5525-226">H.</span><span class="sxs-lookup"><span data-stu-id="d5525-226">H.</span></span> <span data-ttu-id="d5525-227">A tárolt eljárás toocreate statisztika használatát egy adatbázis összes oszlopa</span><span class="sxs-lookup"><span data-stu-id="d5525-227">Use a stored procedure toocreate statistics on all columns in a database</span></span>
<span data-ttu-id="d5525-228">Az SQL Data Warehouse nem rendelkezik túl a rendszer tárolt eljárás megfelelőjét az SQL Server [sp_create_stats] [-].</span><span class="sxs-lookup"><span data-stu-id="d5525-228">SQL Data Warehouse does not have a system stored procedure equivalent too[sp_create_stats][] in SQL Server.</span></span> <span data-ttu-id="d5525-229">Ez a tárolt eljárás objektumot hoz létre egy oszlop statisztika hello adatbázis, amely még nem rendelkezik statisztikai minden oszlop alapján.</span><span class="sxs-lookup"><span data-stu-id="d5525-229">This stored procedure creates a single column statistics object on every column of hello database that doesn't already have statistics.</span></span>

<span data-ttu-id="d5525-230">Ez segítséget nyújt az adatbázis tervét az első lépései.</span><span class="sxs-lookup"><span data-stu-id="d5525-230">This will help you get started with your database design.</span></span> <span data-ttu-id="d5525-231">Szabad tooadapt látja azt tooyour kell.</span><span class="sxs-lookup"><span data-stu-id="d5525-231">Feel free tooadapt it tooyour needs.</span></span>

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

<span data-ttu-id="d5525-232">Ezzel az eljárással hello táblázat összes oszlopa toocreate statisztikák egyszerűen hello eljárás hívása.</span><span class="sxs-lookup"><span data-stu-id="d5525-232">toocreate statistics on all columns in hello table with this procedure, simply call hello procedure.</span></span>

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a><span data-ttu-id="d5525-233">Példák: statisztika frissítése</span><span class="sxs-lookup"><span data-stu-id="d5525-233">Examples: update statistics</span></span>
<span data-ttu-id="d5525-234">tooupdate statisztika, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="d5525-234">tooupdate statistics, you can:</span></span>

1. <span data-ttu-id="d5525-235">Egy statisztikai objektum frissítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-235">Update one statistics object.</span></span> <span data-ttu-id="d5525-236">Adja meg a statisztika objektum tooupdate kívánja hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d5525-236">Specify hello name of hello statistics object you wish tooupdate.</span></span>
2. <span data-ttu-id="d5525-237">Egy tábla összes statisztika objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-237">Update all statistics objects on a table.</span></span> <span data-ttu-id="d5525-238">Adja meg egy adott statisztika objektum helyett hello tábla hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d5525-238">Specify hello name of hello table instead of one specific statistics object.</span></span>

### <a name="a-update-one-specific-statistics-object"></a><span data-ttu-id="d5525-239">A.</span><span class="sxs-lookup"><span data-stu-id="d5525-239">A.</span></span> <span data-ttu-id="d5525-240">Egy adott statisztika objektum frissítése</span><span class="sxs-lookup"><span data-stu-id="d5525-240">Update one specific statistics object</span></span>
<span data-ttu-id="d5525-241">A következő szintaxist tooupdate egy adott statisztika objektum hello használata:</span><span class="sxs-lookup"><span data-stu-id="d5525-241">Use hello following syntax tooupdate a specific statistics object:</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

<span data-ttu-id="d5525-242">Példa:</span><span class="sxs-lookup"><span data-stu-id="d5525-242">For example:</span></span>

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

<span data-ttu-id="d5525-243">Adott statisztika objektumok frissítésével hello időt és erőforrásokat szükséges toomanage statisztika minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="d5525-243">By updating specific statistics objects, you can minimize hello time and resources required toomanage statistics.</span></span> <span data-ttu-id="d5525-244">Ehhez az egyes-re, azonban toochoose hello legjobb statisztika objektumok tooupdate.</span><span class="sxs-lookup"><span data-stu-id="d5525-244">This requires some thought, though, toochoose hello best statistics objects tooupdate.</span></span>

### <a name="b-update-all-statistics-on-a-table"></a><span data-ttu-id="d5525-245">B.</span><span class="sxs-lookup"><span data-stu-id="d5525-245">B.</span></span> <span data-ttu-id="d5525-246">Egy tábla összes statisztika frissítése</span><span class="sxs-lookup"><span data-stu-id="d5525-246">Update all statistics on a table</span></span>
<span data-ttu-id="d5525-247">Ez azt jelenti, egy egyszerű módszer egy tábla összes hello statisztika objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-247">This shows a simple method for updating all hello statistics objects on a table.</span></span>

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

<span data-ttu-id="d5525-248">Példa:</span><span class="sxs-lookup"><span data-stu-id="d5525-248">For example:</span></span>

```sql
UPDATE STATISTICS dbo.table1;
```

<span data-ttu-id="d5525-249">A jelen nyilatkozat könnyen toouse.</span><span class="sxs-lookup"><span data-stu-id="d5525-249">This statement is easy toouse.</span></span> <span data-ttu-id="d5525-250">Ne feledje azonban ez frissíti az összes statisztika hello táblán, és ezért előfordulhat, hogy hajtsa végre a szükségesnél több munkát.</span><span class="sxs-lookup"><span data-stu-id="d5525-250">Just remember this updates all statistics on hello table, and therefore might perform more work than is necessary.</span></span> <span data-ttu-id="d5525-251">Ha hello teljesítmény darabolása nem okoz problémát, ez az egyértelműen hello legegyszerűbb és leghatékonyabb módon tooguarantee statisztika naprakészek legyenek.</span><span class="sxs-lookup"><span data-stu-id="d5525-251">If hello performance is not an issue, this is definitely hello easiest and most complete way tooguarantee statistics are up-to-date.</span></span>

> [!NOTE]
> <span data-ttu-id="d5525-252">Egy tábla összes statisztika frissítése, az SQL Data Warehouse egy vizsgálat toosample hello tábla minden egyes statistics hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="d5525-252">When updating all statistics on a table, SQL Data Warehouse does a scan toosample hello table for each statistics.</span></span> <span data-ttu-id="d5525-253">Hello tábla túl nagy, ha sok oszlopot, és sok statisztika, előfordulhat, hogy hatékonyabb tooupdate egyedi statisztika igények alapján.</span><span class="sxs-lookup"><span data-stu-id="d5525-253">If hello table is large, has many columns, and many statistics, it might be more efficient tooupdate individual statistics based on need.</span></span>
> 
> 

<span data-ttu-id="d5525-254">Egy végrehajtásához egy `UPDATE STATISTICS` eljárást lásd: hello [az ideiglenes táblák] [ Temporary] cikk.</span><span class="sxs-lookup"><span data-stu-id="d5525-254">For an implementation of an `UPDATE STATISTICS` procedure please see hello [Temporary Tables][Temporary] article.</span></span> <span data-ttu-id="d5525-255">hello megvalósítási módja kissé eltérő toohello `CREATE STATISTICS` fent leírt lépéseket, de hello végeredménynek van hello azonos.</span><span class="sxs-lookup"><span data-stu-id="d5525-255">hello implementation method is slightly different toohello `CREATE STATISTICS` procedure above but hello end result is hello same.</span></span>

<span data-ttu-id="d5525-256">Hello teljes szintaxisát, lásd: [Update Statistics] [ Update Statistics] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="d5525-256">For hello full syntax, see [Update Statistics][Update Statistics] on MSDN.</span></span>

## <a name="statistics-metadata"></a><span data-ttu-id="d5525-257">Statisztika metaadatok</span><span class="sxs-lookup"><span data-stu-id="d5525-257">Statistics metadata</span></span>
<span data-ttu-id="d5525-258">Több rendszernézet és, hogy használható-e toofind információkat statisztikai függvények is van.</span><span class="sxs-lookup"><span data-stu-id="d5525-258">There are several system view and functions that you can use toofind information about statistics.</span></span> <span data-ttu-id="d5525-259">Például láthatja, ha egy statisztika objektum elavult lehet hello statisztikák-date függvény toosee használatával statisztika volt utoljára létrehozásakor vagy frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="d5525-259">For example, you can see if a statistics object might be out-of-date by using hello stats-date function toosee when statistics were last created or updated.</span></span>

### <a name="catalog-views-for-statistics"></a><span data-ttu-id="d5525-260">A statisztika katalógusnézetekre</span><span class="sxs-lookup"><span data-stu-id="d5525-260">Catalog views for statistics</span></span>
<span data-ttu-id="d5525-261">Ezek a rendszer nézetek statisztika ismertetik:</span><span class="sxs-lookup"><span data-stu-id="d5525-261">These system views provide information about statistics:</span></span>

| <span data-ttu-id="d5525-262">Katalógusnézet használatával derítheti ki</span><span class="sxs-lookup"><span data-stu-id="d5525-262">Catalog View</span></span> | <span data-ttu-id="d5525-263">Leírás</span><span class="sxs-lookup"><span data-stu-id="d5525-263">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d5525-264">[sys.Columns][sys.columns]</span><span class="sxs-lookup"><span data-stu-id="d5525-264">[sys.columns][sys.columns]</span></span> |<span data-ttu-id="d5525-265">Egy sor minden egyes oszlophoz.</span><span class="sxs-lookup"><span data-stu-id="d5525-265">One row for each column.</span></span> |
| <span data-ttu-id="d5525-266">[sys.Objects][sys.objects]</span><span class="sxs-lookup"><span data-stu-id="d5525-266">[sys.objects][sys.objects]</span></span> |<span data-ttu-id="d5525-267">Egy sor minden objektum hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="d5525-267">One row for each object in hello database.</span></span> |
| <span data-ttu-id="d5525-268">[sys.schemas][sys.schemas]</span><span class="sxs-lookup"><span data-stu-id="d5525-268">[sys.schemas][sys.schemas]</span></span> |<span data-ttu-id="d5525-269">Egy sor minden hello adatbázis-séma.</span><span class="sxs-lookup"><span data-stu-id="d5525-269">One row for each schema in hello database.</span></span> |
| <span data-ttu-id="d5525-270">[sys.stats][sys.stats]</span><span class="sxs-lookup"><span data-stu-id="d5525-270">[sys.stats][sys.stats]</span></span> |<span data-ttu-id="d5525-271">Egy sor minden egyes statisztika objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="d5525-271">One row for each statistics object.</span></span> |
| <span data-ttu-id="d5525-272">[sys.stats_columns][sys.stats_columns]</span><span class="sxs-lookup"><span data-stu-id="d5525-272">[sys.stats_columns][sys.stats_columns]</span></span> |<span data-ttu-id="d5525-273">Egy sor hello statisztika objektum minden egyes oszlophoz.</span><span class="sxs-lookup"><span data-stu-id="d5525-273">One row for each column in hello statistics object.</span></span> <span data-ttu-id="d5525-274">Hivatkozások biztonsági toosys.columns.</span><span class="sxs-lookup"><span data-stu-id="d5525-274">Links back toosys.columns.</span></span> |
| <span data-ttu-id="d5525-275">[sys.Tables][sys.tables]</span><span class="sxs-lookup"><span data-stu-id="d5525-275">[sys.tables][sys.tables]</span></span> |<span data-ttu-id="d5525-276">Egy sor minden táblához (külső táblát tartalmazza).</span><span class="sxs-lookup"><span data-stu-id="d5525-276">One row for each table (includes external tables).</span></span> |
| <span data-ttu-id="d5525-277">[sys.table_types][sys.table_types]</span><span class="sxs-lookup"><span data-stu-id="d5525-277">[sys.table_types][sys.table_types]</span></span> |<span data-ttu-id="d5525-278">Egy sor egyes adattípusok esetében.</span><span class="sxs-lookup"><span data-stu-id="d5525-278">One row for each data type.</span></span> |

### <a name="system-functions-for-statistics"></a><span data-ttu-id="d5525-279">A statisztika rendszer funkciók</span><span class="sxs-lookup"><span data-stu-id="d5525-279">System functions for statistics</span></span>
<span data-ttu-id="d5525-280">A rendszer függvények hasznosak statisztika használata:</span><span class="sxs-lookup"><span data-stu-id="d5525-280">These system functions are useful for working with statistics:</span></span>

| <span data-ttu-id="d5525-281">Rendszer-funkció</span><span class="sxs-lookup"><span data-stu-id="d5525-281">System Function</span></span> | <span data-ttu-id="d5525-282">Leírás</span><span class="sxs-lookup"><span data-stu-id="d5525-282">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="d5525-283">[STATS_DATE][STATS_DATE]</span><span class="sxs-lookup"><span data-stu-id="d5525-283">[STATS_DATE][STATS_DATE]</span></span> |<span data-ttu-id="d5525-284">Utolsó frissítés dátuma hello statisztika objektum.</span><span class="sxs-lookup"><span data-stu-id="d5525-284">Date hello statistics object was last updated.</span></span> |
| <span data-ttu-id="d5525-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span><span class="sxs-lookup"><span data-stu-id="d5525-285">[DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS]</span></span> |<span data-ttu-id="d5525-286">Hello terjesztési értékek összefoglaló szintű és részletes információt nyújt hello statisztika objektum tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="d5525-286">Provides summary level and detailed information about hello distribution of values as understood by hello statistics object.</span></span> |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a><span data-ttu-id="d5525-287">Statisztika oszlopok és funkciók egyesítése egy nézet</span><span class="sxs-lookup"><span data-stu-id="d5525-287">Combine statistics columns and functions into one view</span></span>
<span data-ttu-id="d5525-288">Ez a nézet során az oszlopok ki az egymáshoz kapcsolódó toostatistics, és az eredmények hello [STATS_DATE()] [] függvényből együtt.</span><span class="sxs-lookup"><span data-stu-id="d5525-288">This view brings columns that relate toostatistics, and results from hello [STATS_DATE()][]function together.</span></span>

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

## <a name="dbcc-showstatistics-examples"></a><span data-ttu-id="d5525-289">DBCC SHOW_STATISTICS() példák</span><span class="sxs-lookup"><span data-stu-id="d5525-289">DBCC SHOW_STATISTICS() examples</span></span>
<span data-ttu-id="d5525-290">DBCC SHOW_STATISTICS() statisztika objektumon belül tárolt hello adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="d5525-290">DBCC SHOW_STATISTICS() shows hello data held within a statistics object.</span></span> <span data-ttu-id="d5525-291">Ezek az adatok származnak három részből áll.</span><span class="sxs-lookup"><span data-stu-id="d5525-291">This data comes in three parts.</span></span>

1. <span data-ttu-id="d5525-292">Fejléc</span><span class="sxs-lookup"><span data-stu-id="d5525-292">Header</span></span>
2. <span data-ttu-id="d5525-293">Sűrűség vektoros</span><span class="sxs-lookup"><span data-stu-id="d5525-293">Density Vector</span></span>
3. <span data-ttu-id="d5525-294">Hisztogram</span><span class="sxs-lookup"><span data-stu-id="d5525-294">Histogram</span></span>

<span data-ttu-id="d5525-295">hello fejléc metaadatok hello statisztika.</span><span class="sxs-lookup"><span data-stu-id="d5525-295">hello header metadata about hello statistics.</span></span> <span data-ttu-id="d5525-296">hello hisztogram hello terjesztési értékek hello első hello statisztika objektum kulcsoszlop jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d5525-296">hello histogram displays hello distribution of values in hello first key column of hello statistics object.</span></span> <span data-ttu-id="d5525-297">hello sűrűség vektoros kereszt-oszlop korrelációs méri.</span><span class="sxs-lookup"><span data-stu-id="d5525-297">hello density vector measures cross-column correlation.</span></span> <span data-ttu-id="d5525-298">SQLDW kiszámítja hello adatok hello statisztika objektum egyik számossága becslése.</span><span class="sxs-lookup"><span data-stu-id="d5525-298">SQLDW computes cardinality estimates with any of hello data in hello statistics object.</span></span>

### <a name="show-header-density-and-histogram"></a><span data-ttu-id="d5525-299">Fejléc, sűrűség és hisztogram megjelenítése</span><span class="sxs-lookup"><span data-stu-id="d5525-299">Show header, density, and histogram</span></span>
<span data-ttu-id="d5525-300">Az egyszerű példában statisztika objektum összes három részből.</span><span class="sxs-lookup"><span data-stu-id="d5525-300">This simple example shows all three parts of a statistics object.</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

<span data-ttu-id="d5525-301">Példa:</span><span class="sxs-lookup"><span data-stu-id="d5525-301">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a><span data-ttu-id="d5525-302">DBCC SHOW_STATISTICS(); egy vagy több részei megjelenítése</span><span class="sxs-lookup"><span data-stu-id="d5525-302">Show one or more parts of DBCC SHOW_STATISTICS();</span></span>
<span data-ttu-id="d5525-303">Ha érdekli csak megtekintés részét, használja a hello `WITH` záradék, és adja meg, mely részeit toosee szeretné:</span><span class="sxs-lookup"><span data-stu-id="d5525-303">If you are only interested in viewing specific parts, use hello `WITH` clause and specify which parts you want toosee:</span></span>

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

<span data-ttu-id="d5525-304">Példa:</span><span class="sxs-lookup"><span data-stu-id="d5525-304">For example:</span></span>

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a><span data-ttu-id="d5525-305">DBCC SHOW_STATISTICS() különbségek</span><span class="sxs-lookup"><span data-stu-id="d5525-305">DBCC SHOW_STATISTICS() differences</span></span>
<span data-ttu-id="d5525-306">DBCC SHOW_STATISTICS() szigorúbban vezettek be az SQL Data Warehouse képest tooSQL kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="d5525-306">DBCC SHOW_STATISTICS() is more strictly implemented in SQL Data Warehouse compared tooSQL Server.</span></span>

1. <span data-ttu-id="d5525-307">Nem dokumentált funkciók nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="d5525-307">Undocumented features are not supported</span></span>
2. <span data-ttu-id="d5525-308">Nem használható a Stats_stream</span><span class="sxs-lookup"><span data-stu-id="d5525-308">Cannot use Stats_stream</span></span>
3. <span data-ttu-id="d5525-309">Nem tudja csatlakoztatni meghatározott fájlcsoportokat statisztikai adatok eredményeit pl. (STAT_HEADER ILLESZTÉSI DENSITY_VECTOR)</span><span class="sxs-lookup"><span data-stu-id="d5525-309">Cannot join results for specific subsets of statistics data e.g. (STAT_HEADER JOIN DENSITY_VECTOR)</span></span>
4. <span data-ttu-id="d5525-310">Üzenet tiltási NO_INFOMSGS nem állítható be</span><span class="sxs-lookup"><span data-stu-id="d5525-310">NO_INFOMSGS cannot be set for message suppression</span></span>
5. <span data-ttu-id="d5525-311">Statisztika neveket burkoló szögletes zárójelek között nem használható.</span><span class="sxs-lookup"><span data-stu-id="d5525-311">Square brackets around statistics names cannot be used</span></span>
6. <span data-ttu-id="d5525-312">Oszlop nevek tooidentify statisztika objektumok nem használható.</span><span class="sxs-lookup"><span data-stu-id="d5525-312">Cannot use column names tooidentify statistics objects</span></span>
7. <span data-ttu-id="d5525-313">Egyéni hiba 2767 nem támogatott</span><span class="sxs-lookup"><span data-stu-id="d5525-313">Custom error 2767 is not supported</span></span>

## <a name="next-steps"></a><span data-ttu-id="d5525-314">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5525-314">Next steps</span></span>
<span data-ttu-id="d5525-315">További részletekért lásd: [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="d5525-315">For more details, see [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] on MSDN.</span></span>  <span data-ttu-id="d5525-316">toolearn több, lásd: hello cikkeket a [tábla áttekintése][Overview], [tábla adattípusok][Data Types], [táblaterjesztése] [ Distribute], [Tábla indexelő][Index], [tábla particionáló] [ Partition] és [ Az ideiglenes táblák][Temporary].</span><span class="sxs-lookup"><span data-stu-id="d5525-316">toolearn more, see hello articles on [Table Overview][Overview], [Table Data Types][Data Types], [Distributing a Table][Distribute], [Indexing a Table][Index],  [Partitioning a Table][Partition] and [Temporary Tables][Temporary].</span></span>  <span data-ttu-id="d5525-317">Gyakorlati tanácsok kapcsolatban bővebben lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].</span><span class="sxs-lookup"><span data-stu-id="d5525-317">For more about best practices, see [SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices].</span></span>  

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

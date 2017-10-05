---
title: "Az SQL Data Warehouse tárolt eljárások |} Microsoft Docs"
description: "Tippek a tárolt eljárások végrehajtása az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: e42d80f0ca35f3fbb67389c66d072bc40d8a8d2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a><span data-ttu-id="72f25-103">Az SQL Data Warehouse tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="72f25-103">Stored procedures in SQL Data Warehouse</span></span>
<span data-ttu-id="72f25-104">Az SQL Data Warehouse számos található az SQL Server Transact-SQL funkció támogatja.</span><span class="sxs-lookup"><span data-stu-id="72f25-104">SQL Data Warehouse supports many of the Transact-SQL features found in SQL Server.</span></span> <span data-ttu-id="72f25-105">Nincsenek ennél is fontosabb, hogy a megoldás bővítette ki lesz szeretnénk funkciók kibővítési.</span><span class="sxs-lookup"><span data-stu-id="72f25-105">More importantly there are scale out specific features that we will want to leverage to maximize the performance of your solution.</span></span>

<span data-ttu-id="72f25-106">Azonban fenntartásához a méretezés és teljesítmény az SQL Data Warehouse hiba történik is egyes szolgáltatások és funkciók, amelyek viselkedési különbségek és mások által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="72f25-106">However, to maintain the scale and performance of SQL Data Warehouse there are also some features and functionality that have behavioral differences and others that are not supported.</span></span>

<span data-ttu-id="72f25-107">Ez a cikk azt ismerteti, hogyan SQL Data warehouse tárolt eljárások végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="72f25-107">This article explains how to implement stored procedures within SQL Data Warehouse.</span></span>

## <a name="introducing-stored-procedures"></a><span data-ttu-id="72f25-108">Tárolt eljárások bemutatása</span><span class="sxs-lookup"><span data-stu-id="72f25-108">Introducing stored procedures</span></span>
<span data-ttu-id="72f25-109">Tárolt eljárások nagyszerű módját a és az SQL-kódot; tárolja az adatokat az adatraktárban közelében.</span><span class="sxs-lookup"><span data-stu-id="72f25-109">Stored procedures are a great way for encapsulating your SQL code; storing it close to your data in the data warehouse.</span></span> <span data-ttu-id="72f25-110">A kód és kezelhető egységekbe által a tárolt eljárások segítségével a fejlesztők modularize megoldásuk; a kód nagyobb re-usability megkönnyítése.</span><span class="sxs-lookup"><span data-stu-id="72f25-110">By encapsulating the code into manageable units stored procedures help developers modularize their solutions; facilitating greater re-usability of code.</span></span> <span data-ttu-id="72f25-111">Minden tárolt eljárás is fogadhat rugalmasabb még így ezek a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="72f25-111">Each stored procedure can also accept parameters to make them even more flexible.</span></span>

<span data-ttu-id="72f25-112">Az SQL Data Warehouse egy egyszerűsített és zökkenőmentes tárolt eljárás végrehajtása biztosítja.</span><span class="sxs-lookup"><span data-stu-id="72f25-112">SQL Data Warehouse provides a simplified and streamlined stored procedure implementation.</span></span> <span data-ttu-id="72f25-113">A legfontosabb különbség az SQL Server képest, győződjön meg arról, hogy a tárolt eljárás nem előre lefordított kódot.</span><span class="sxs-lookup"><span data-stu-id="72f25-113">The biggest difference compared to SQL Server is that the stored procedure is not pre-compiled code.</span></span> <span data-ttu-id="72f25-114">Az adatraktárak dolgozunk általában kevésbé fontos szempont a fordítási idő.</span><span class="sxs-lookup"><span data-stu-id="72f25-114">In data warehouses we are generally less concerned with the compilation time.</span></span> <span data-ttu-id="72f25-115">Több fontos, hogy a tárolt eljárás kódot megfelelően optimalizálni, ha nagy adatmennyiség elleni működik.</span><span class="sxs-lookup"><span data-stu-id="72f25-115">It is more important that the stored procedure code is correctly optimised when operating against large data volumes.</span></span> <span data-ttu-id="72f25-116">A cél, óra, perc és másodperc, ezredmásodperc nem menti.</span><span class="sxs-lookup"><span data-stu-id="72f25-116">The goal is to save hours, minutes and seconds not milliseconds.</span></span> <span data-ttu-id="72f25-117">Éppen ezért jobban használható SQL logika tárolójaként tárolt eljárások gondol.</span><span class="sxs-lookup"><span data-stu-id="72f25-117">It is therefore more helpful to think of stored procedures as containers for SQL logic.</span></span>     

<span data-ttu-id="72f25-118">A tárolt eljárás végrehajtásakor a SQL Data Warehouse az SQL-utasítások elemzésének, lefordított és optimalizált futási időben.</span><span class="sxs-lookup"><span data-stu-id="72f25-118">When SQL Data Warehouse executes your stored procedure the SQL statements are parsed, translated and optimized at run time.</span></span> <span data-ttu-id="72f25-119">A folyamat során minden utasításhoz konvertálni az elosztott lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="72f25-119">During this process each statement is converted into distributed queries.</span></span> <span data-ttu-id="72f25-120">Az SQL-kódot, amely az adatok alapján végrehajtása nem egyezik az elküldött lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="72f25-120">The SQL code that is actually executed against the data is different to the query submitted.</span></span>

## <a name="nesting-stored-procedures"></a><span data-ttu-id="72f25-121">A beágyazási tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="72f25-121">Nesting stored procedures</span></span>
<span data-ttu-id="72f25-122">Ha a tárolt eljárások hívás az más tárolt eljárások, vagy dinamikus SQL-utasítás végrehajtása, majd a belső tárolt eljárás vagy a kód hívása, különállónak lehet egymásba ágyazni.</span><span class="sxs-lookup"><span data-stu-id="72f25-122">When stored procedures call other stored procedures or execute dynamic sql then the inner stored procedure or code invocation is said to be nested.</span></span>

<span data-ttu-id="72f25-123">Az SQL Data Warehouse 8 beágyazási szinttel legfeljebb támogat.</span><span class="sxs-lookup"><span data-stu-id="72f25-123">SQL Data Warehouse support a maximum of 8 nesting levels.</span></span> <span data-ttu-id="72f25-124">Ez kis mértékben eltér az SQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="72f25-124">This is slightly different to SQL Server.</span></span> <span data-ttu-id="72f25-125">SQL Server szintje kivételblokkokra 32.</span><span class="sxs-lookup"><span data-stu-id="72f25-125">The nest level in SQL Server is 32.</span></span>

<span data-ttu-id="72f25-126">1. szintű beágyazásához megfelel a legfelső szintű tárolt eljárás hívása</span><span class="sxs-lookup"><span data-stu-id="72f25-126">The top level stored procedure call equates to nest level 1</span></span>

```sql
EXEC prc_nesting
```
<span data-ttu-id="72f25-127">Ha a tárolt eljárás is küld egy másik hívás EXEC majd ez megnöveli a nest szint 2</span><span class="sxs-lookup"><span data-stu-id="72f25-127">If the stored procedure also makes another EXEC call then this will increase the nest level to 2</span></span>

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
<span data-ttu-id="72f25-128">Ha a második eljárás végrehajtása során majd néhány dinamikus sql majd ez megnöveli a nest szint 3</span><span class="sxs-lookup"><span data-stu-id="72f25-128">If the second procedure then executes some dynamic sql then this will increase the nest level to 3</span></span>

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

<span data-ttu-id="72f25-129">Megjegyzés: az SQL Data Warehouse jelenleg nem támogatja a@NESTLEVEL.</span><span class="sxs-lookup"><span data-stu-id="72f25-129">Note SQL Data Warehouse does not currently support @@NESTLEVEL.</span></span> <span data-ttu-id="72f25-130">Nyomon egy a nest szint saját magának kell.</span><span class="sxs-lookup"><span data-stu-id="72f25-130">You will need to keep a track of your nest level yourself.</span></span> <span data-ttu-id="72f25-131">Nem valószínű, a 8 nest szintű korlátot elért, de ellenkező esetben szüksége lesz a kódot újra működik, és "egybesimítására" azt, hogy elférjen ezt a határt.</span><span class="sxs-lookup"><span data-stu-id="72f25-131">It is unlikely you will hit the 8 nest level limit but if you do you will need to re-work your code and "flatten" it so that it fits within this limit.</span></span>

## <a name="insertexecute"></a><span data-ttu-id="72f25-132">INSERT... VÉGREHAJTÁS</span><span class="sxs-lookup"><span data-stu-id="72f25-132">INSERT..EXECUTE</span></span>
<span data-ttu-id="72f25-133">Az SQL Data Warehouse nem teszi lehetővé az eredményhalmaz egy INSERT utasítás a tárolt eljárás felhasználását.</span><span class="sxs-lookup"><span data-stu-id="72f25-133">SQL Data Warehouse does not permit you to consume the result set of a stored procedure with an INSERT statement.</span></span> <span data-ttu-id="72f25-134">Van azonban egy másik módszert is használhat.</span><span class="sxs-lookup"><span data-stu-id="72f25-134">However, there is an alternative approach you can use.</span></span>

<span data-ttu-id="72f25-135">Tekintse meg az alábbi cikket a [az ideiglenes táblák] ennek példát.</span><span class="sxs-lookup"><span data-stu-id="72f25-135">Please refer to the following article on [temporary tables] for an example on how to do this.</span></span>

## <a name="limitations"></a><span data-ttu-id="72f25-136">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="72f25-136">Limitations</span></span>
<span data-ttu-id="72f25-137">Nincsenek a Transact-SQL tárolt eljárások, amelyeket a rendszer nem az SQL Data Warehouse egyes funkcióit.</span><span class="sxs-lookup"><span data-stu-id="72f25-137">There are some aspects of Transact-SQL stored procedures that are not implemented in SQL Data Warehouse.</span></span>

<span data-ttu-id="72f25-138">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="72f25-138">They are:</span></span>

* <span data-ttu-id="72f25-139">az ideiglenes tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="72f25-139">temporary stored procedures</span></span>
* <span data-ttu-id="72f25-140">a számozott tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="72f25-140">numbered stored procedures</span></span>
* <span data-ttu-id="72f25-141">Bővített tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="72f25-141">extended stored procedures</span></span>
* <span data-ttu-id="72f25-142">A közös nyelvi futtató környezet tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="72f25-142">CLR stored procedures</span></span>
* <span data-ttu-id="72f25-143">a titkosítási beállítás</span><span class="sxs-lookup"><span data-stu-id="72f25-143">encryption option</span></span>
* <span data-ttu-id="72f25-144">replikációs beállítás</span><span class="sxs-lookup"><span data-stu-id="72f25-144">replication option</span></span>
* <span data-ttu-id="72f25-145">tábla értékű paraméter</span><span class="sxs-lookup"><span data-stu-id="72f25-145">table-valued parameters</span></span>
* <span data-ttu-id="72f25-146">csak olvasható paraméterek</span><span class="sxs-lookup"><span data-stu-id="72f25-146">read-only parameters</span></span>
* <span data-ttu-id="72f25-147">alapértelmezett paraméterek</span><span class="sxs-lookup"><span data-stu-id="72f25-147">default parameters</span></span>
* <span data-ttu-id="72f25-148">végrehajtási környezet</span><span class="sxs-lookup"><span data-stu-id="72f25-148">execution contexts</span></span>
* <span data-ttu-id="72f25-149">térjen vissza a utasítás</span><span class="sxs-lookup"><span data-stu-id="72f25-149">return statement</span></span>

## <a name="next-steps"></a><span data-ttu-id="72f25-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="72f25-150">Next steps</span></span>
<span data-ttu-id="72f25-151">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="72f25-151">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
<span data-ttu-id="72f25-152">[az ideiglenes táblák]: ./sql-data-warehouse-tables-temporary.md#modularizing-code</span><span class="sxs-lookup"><span data-stu-id="72f25-152">[temporary tables]: ./sql-data-warehouse-tables-temporary.md#modularizing-code</span></span>
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->

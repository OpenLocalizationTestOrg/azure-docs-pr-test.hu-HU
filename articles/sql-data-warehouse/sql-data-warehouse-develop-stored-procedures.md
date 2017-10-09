---
title: "az SQL Data Warehouse aaaStored eljárások |} Microsoft Docs"
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
ms.openlocfilehash: 416252dd3dea95c66aa5e886860b933b22578002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a><span data-ttu-id="153c9-103">Az SQL Data Warehouse tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="153c9-103">Stored procedures in SQL Data Warehouse</span></span>
<span data-ttu-id="153c9-104">Az SQL Data Warehouse számos hello Transact-SQL szolgáltatást az SQL Serverben támogatja.</span><span class="sxs-lookup"><span data-stu-id="153c9-104">SQL Data Warehouse supports many of hello Transact-SQL features found in SQL Server.</span></span> <span data-ttu-id="153c9-105">Nincsenek ennél is fontosabb, hogy a megoldás tooleverage toomaximize hello teljesítményének fog szeretnénk funkciók kibővítési.</span><span class="sxs-lookup"><span data-stu-id="153c9-105">More importantly there are scale out specific features that we will want tooleverage toomaximize hello performance of your solution.</span></span>

<span data-ttu-id="153c9-106">Azonban toomaintain hello méretezés és teljesítmény az SQL Data Warehouse hiba történik is egyes szolgáltatások és funkciók, amelyek viselkedési különbségek és mások által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="153c9-106">However, toomaintain hello scale and performance of SQL Data Warehouse there are also some features and functionality that have behavioral differences and others that are not supported.</span></span>

<span data-ttu-id="153c9-107">Ez a cikk azt ismerteti, hogyan tooimplement tárolt eljárások az SQL Data Warehouse belül.</span><span class="sxs-lookup"><span data-stu-id="153c9-107">This article explains how tooimplement stored procedures within SQL Data Warehouse.</span></span>

## <a name="introducing-stored-procedures"></a><span data-ttu-id="153c9-108">Tárolt eljárások bemutatása</span><span class="sxs-lookup"><span data-stu-id="153c9-108">Introducing stored procedures</span></span>
<span data-ttu-id="153c9-109">Tárolt eljárások nagyszerű módját a és az SQL-kódot; hello adatraktár Bezárás tooyour adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="153c9-109">Stored procedures are a great way for encapsulating your SQL code; storing it close tooyour data in hello data warehouse.</span></span> <span data-ttu-id="153c9-110">Által kezelhető egységekbe hello kód és a tárolt eljárások segítségével a fejlesztők modularize megoldásuk; a kód nagyobb re-usability megkönnyítése.</span><span class="sxs-lookup"><span data-stu-id="153c9-110">By encapsulating hello code into manageable units stored procedures help developers modularize their solutions; facilitating greater re-usability of code.</span></span> <span data-ttu-id="153c9-111">Minden tárolt eljárás is fogadhat paraméterek toomake rugalmasabb is azokat.</span><span class="sxs-lookup"><span data-stu-id="153c9-111">Each stored procedure can also accept parameters toomake them even more flexible.</span></span>

<span data-ttu-id="153c9-112">Az SQL Data Warehouse egy egyszerűsített és zökkenőmentes tárolt eljárás végrehajtása biztosítja.</span><span class="sxs-lookup"><span data-stu-id="153c9-112">SQL Data Warehouse provides a simplified and streamlined stored procedure implementation.</span></span> <span data-ttu-id="153c9-113">hello legnagyobb különbség képest tooSQL kiszolgáló, amely a tárolt eljárás hello nincs előre lefordított kódot.</span><span class="sxs-lookup"><span data-stu-id="153c9-113">hello biggest difference compared tooSQL Server is that hello stored procedure is not pre-compiled code.</span></span> <span data-ttu-id="153c9-114">Az adatraktárak dolgozunk kevesebb általában érintett hello a fordítás során.</span><span class="sxs-lookup"><span data-stu-id="153c9-114">In data warehouses we are generally less concerned with hello compilation time.</span></span> <span data-ttu-id="153c9-115">Több fontos, hogy a hello tárolt eljárás kódot megfelelően optimalizálni, ha nagy adatmennyiség elleni működik.</span><span class="sxs-lookup"><span data-stu-id="153c9-115">It is more important that hello stored procedure code is correctly optimised when operating against large data volumes.</span></span> <span data-ttu-id="153c9-116">hello célja toosave óra, perc és másodperc nem ezredmásodperc.</span><span class="sxs-lookup"><span data-stu-id="153c9-116">hello goal is toosave hours, minutes and seconds not milliseconds.</span></span> <span data-ttu-id="153c9-117">Éppen ezért a tárolt eljárások további hasznos toothink SQL logika tárolójaként.</span><span class="sxs-lookup"><span data-stu-id="153c9-117">It is therefore more helpful toothink of stored procedures as containers for SQL logic.</span></span>     

<span data-ttu-id="153c9-118">Az SQL Data Warehouse végrehajtásakor a a tárolt eljárás hello SQL-utasítások elemezni, a lefordított és optimalizált futási időben.</span><span class="sxs-lookup"><span data-stu-id="153c9-118">When SQL Data Warehouse executes your stored procedure hello SQL statements are parsed, translated and optimized at run time.</span></span> <span data-ttu-id="153c9-119">A folyamat során minden utasításhoz konvertálni az elosztott lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="153c9-119">During this process each statement is converted into distributed queries.</span></span> <span data-ttu-id="153c9-120">hello SQL futtatott kód is ténylegesen hello adatok alapján az különböző toohello lekérdezés elküldése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="153c9-120">hello SQL code that is actually executed against hello data is different toohello query submitted.</span></span>

## <a name="nesting-stored-procedures"></a><span data-ttu-id="153c9-121">A beágyazási tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="153c9-121">Nesting stored procedures</span></span>
<span data-ttu-id="153c9-122">Amikor tárolt eljárások hívás az más tárolt eljárások, vagy dinamikus SQL-utasítás végrehajtása, majd hello belső tárolt eljárás vagy kód hívása, különállónak toobe beágyazott.</span><span class="sxs-lookup"><span data-stu-id="153c9-122">When stored procedures call other stored procedures or execute dynamic sql then hello inner stored procedure or code invocation is said toobe nested.</span></span>

<span data-ttu-id="153c9-123">Az SQL Data Warehouse 8 beágyazási szinttel legfeljebb támogat.</span><span class="sxs-lookup"><span data-stu-id="153c9-123">SQL Data Warehouse support a maximum of 8 nesting levels.</span></span> <span data-ttu-id="153c9-124">Ez a kiszolgáló némileg eltérő tooSQL.</span><span class="sxs-lookup"><span data-stu-id="153c9-124">This is slightly different tooSQL Server.</span></span> <span data-ttu-id="153c9-125">hello nest az SQL Server szintje 32.</span><span class="sxs-lookup"><span data-stu-id="153c9-125">hello nest level in SQL Server is 32.</span></span>

<span data-ttu-id="153c9-126">hello felső szintű tárolt eljárás hívása csatlakozás toonest szintjét 1</span><span class="sxs-lookup"><span data-stu-id="153c9-126">hello top level stored procedure call equates toonest level 1</span></span>

```sql
EXEC prc_nesting
```
<span data-ttu-id="153c9-127">Ha hello tárolt eljárás is lehetővé teszi egy másik EXEC hívható meg, akkor ez megnöveli a hello nest szintű too2</span><span class="sxs-lookup"><span data-stu-id="153c9-127">If hello stored procedure also makes another EXEC call then this will increase hello nest level too2</span></span>

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
<span data-ttu-id="153c9-128">Ha hello második eljárás végrehajtása során majd néhány dinamikus sql majd ez megnöveli a hello nest szintű too3</span><span class="sxs-lookup"><span data-stu-id="153c9-128">If hello second procedure then executes some dynamic sql then this will increase hello nest level too3</span></span>

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

<span data-ttu-id="153c9-129">Megjegyzés: az SQL Data Warehouse jelenleg nem támogatja a@NESTLEVEL.</span><span class="sxs-lookup"><span data-stu-id="153c9-129">Note SQL Data Warehouse does not currently support @@NESTLEVEL.</span></span> <span data-ttu-id="153c9-130">Szüksége lesz a nest szintű nyomon tookeep saját maga.</span><span class="sxs-lookup"><span data-stu-id="153c9-130">You will need tookeep a track of your nest level yourself.</span></span> <span data-ttu-id="153c9-131">Nem valószínű, akkor elért hello 8 nest szintű korlátot, de ha így tesz, toore-munkahelyi kell a kódot, és "egybesimítására" azt, hogy elférjen ezt a határt.</span><span class="sxs-lookup"><span data-stu-id="153c9-131">It is unlikely you will hit hello 8 nest level limit but if you do you will need toore-work your code and "flatten" it so that it fits within this limit.</span></span>

## <a name="insertexecute"></a><span data-ttu-id="153c9-132">INSERT... VÉGREHAJTÁS</span><span class="sxs-lookup"><span data-stu-id="153c9-132">INSERT..EXECUTE</span></span>
<span data-ttu-id="153c9-133">Az SQL Data Warehouse nem teszi lehetővé az INSERT utasítás tárolt eljárásnak tooconsume hello eredménykészletből.</span><span class="sxs-lookup"><span data-stu-id="153c9-133">SQL Data Warehouse does not permit you tooconsume hello result set of a stored procedure with an INSERT statement.</span></span> <span data-ttu-id="153c9-134">Van azonban egy másik módszert is használhat.</span><span class="sxs-lookup"><span data-stu-id="153c9-134">However, there is an alternative approach you can use.</span></span>

<span data-ttu-id="153c9-135">Tekintse meg következő cikket toohello [az ideiglenes táblák] hogyan például toodo ez.</span><span class="sxs-lookup"><span data-stu-id="153c9-135">Please refer toohello following article on [temporary tables] for an example on how toodo this.</span></span>

## <a name="limitations"></a><span data-ttu-id="153c9-136">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="153c9-136">Limitations</span></span>
<span data-ttu-id="153c9-137">Nincsenek a Transact-SQL tárolt eljárások, amelyeket a rendszer nem az SQL Data Warehouse egyes funkcióit.</span><span class="sxs-lookup"><span data-stu-id="153c9-137">There are some aspects of Transact-SQL stored procedures that are not implemented in SQL Data Warehouse.</span></span>

<span data-ttu-id="153c9-138">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="153c9-138">They are:</span></span>

* <span data-ttu-id="153c9-139">az ideiglenes tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="153c9-139">temporary stored procedures</span></span>
* <span data-ttu-id="153c9-140">a számozott tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="153c9-140">numbered stored procedures</span></span>
* <span data-ttu-id="153c9-141">Bővített tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="153c9-141">extended stored procedures</span></span>
* <span data-ttu-id="153c9-142">A közös nyelvi futtató környezet tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="153c9-142">CLR stored procedures</span></span>
* <span data-ttu-id="153c9-143">a titkosítási beállítás</span><span class="sxs-lookup"><span data-stu-id="153c9-143">encryption option</span></span>
* <span data-ttu-id="153c9-144">replikációs beállítás</span><span class="sxs-lookup"><span data-stu-id="153c9-144">replication option</span></span>
* <span data-ttu-id="153c9-145">tábla értékű paraméter</span><span class="sxs-lookup"><span data-stu-id="153c9-145">table-valued parameters</span></span>
* <span data-ttu-id="153c9-146">csak olvasható paraméterek</span><span class="sxs-lookup"><span data-stu-id="153c9-146">read-only parameters</span></span>
* <span data-ttu-id="153c9-147">alapértelmezett paraméterek</span><span class="sxs-lookup"><span data-stu-id="153c9-147">default parameters</span></span>
* <span data-ttu-id="153c9-148">végrehajtási környezet</span><span class="sxs-lookup"><span data-stu-id="153c9-148">execution contexts</span></span>
* <span data-ttu-id="153c9-149">térjen vissza a utasítás</span><span class="sxs-lookup"><span data-stu-id="153c9-149">return statement</span></span>

## <a name="next-steps"></a><span data-ttu-id="153c9-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="153c9-150">Next steps</span></span>
<span data-ttu-id="153c9-151">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="153c9-151">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[az ideiglenes táblák]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->

---
title: "A memórián belüli online Tranzakciófeldolgozási javítja az SQL túlhasználat telj |} Microsoft Docs"
description: "Fogadja el a memórián belüli online Tranzakciófeldolgozási egy meglévő SQL-adatbázis tranzakciós teljesítmény javítása érdekében."
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: MightyPen
ms.assetid: c2bf14a0-905b-47b4-afb6-efe9a61147d5
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2016
ms.author: jodebrui
ms.openlocfilehash: 50eed9aed417778bd497f55e20c8e732fdae9cf9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-in-memory-oltp-to-improve-your-application-performance-in-sql-database"></a><span data-ttu-id="fed59-103">Használja a memórián belüli online Tranzakciófeldolgozási, az alkalmazás SQL-adatbázis teljesítményének növelése</span><span class="sxs-lookup"><span data-stu-id="fed59-103">Use In-Memory OLTP to improve your application performance in SQL Database</span></span>
<span data-ttu-id="fed59-104">[A memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md) is használható a tranzakció-feldolgozást, adatfeldolgozást és átmeneti adatáttelepítések esetében teljesítményének javítása [prémium](sql-database-service-tiers.md) Azure SQL-adatbázisok növelése az árképzési szint nélkül.</span><span class="sxs-lookup"><span data-stu-id="fed59-104">[In-Memory OLTP](sql-database-in-memory.md) can be used to improve the performance of transaction processing, data ingestion, and transient data scenarios, in  [Premium](sql-database-service-tiers.md) Azure SQL Databases without increasing the pricing tier.</span></span> 

> [!NOTE] 
> <span data-ttu-id="fed59-105">Megtudhatja, hogyan [kvórum kulcsának adatbázis munkaterhelés megduplázódik, miközben csökkenti a DTU az SQL Database 70 %-kal](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span><span class="sxs-lookup"><span data-stu-id="fed59-105">Learn how [Quorum doubles key database’s workload while lowering DTU by 70% with SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span></span>


<span data-ttu-id="fed59-106">Kövesse az alábbi lépéseket a meglévő adatbázis memórián belüli online Tranzakciófeldolgozási elfogadására.</span><span class="sxs-lookup"><span data-stu-id="fed59-106">Follow these steps to adopt In-Memory OLTP in your existing database.</span></span>

## <a name="step-1-ensure-you-are-using-a-premium-database"></a><span data-ttu-id="fed59-107">1. lépés: Győződjön meg arról, a prémium szintű adatbázist használ</span><span class="sxs-lookup"><span data-stu-id="fed59-107">Step 1: Ensure you are using a Premium database</span></span>
<span data-ttu-id="fed59-108">A memórián belüli online Tranzakciófeldolgozási csak Premium adatbázisokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="fed59-108">In-Memory OLTP is supported only in Premium databases.</span></span> <span data-ttu-id="fed59-109">A memóriában támogatott, ha a visszaadott eredmény 1 (0):</span><span class="sxs-lookup"><span data-stu-id="fed59-109">In-Memory is supported if the returned result is 1 (not 0):</span></span>

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

<span data-ttu-id="fed59-110">*XTP* rövidítése *szélsőséges tranzakció feldolgozása*</span><span class="sxs-lookup"><span data-stu-id="fed59-110">*XTP* stands for *Extreme Transaction Processing*</span></span>



## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a><span data-ttu-id="fed59-111">2. lépés: A memórián belüli online Tranzakciófeldolgozási át objektumokat azonosító</span><span class="sxs-lookup"><span data-stu-id="fed59-111">Step 2: Identify objects to migrate to In-Memory OLTP</span></span>
<span data-ttu-id="fed59-112">SSMS tartalmaz egy **tranzakció teljesítményét elemző áttekintése** jelentést, amely egy aktív munkaterhelés adatbázis is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="fed59-112">SSMS includes a **Transaction Performance Analysis Overview** report that you can run against a database with an active workload.</span></span> <span data-ttu-id="fed59-113">A jelentés azonosítja a táblák és tárolt eljárások, amelyek erre a memórián belüli online Tranzakciófeldolgozási való áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="fed59-113">The report identifies tables and stored procedures that are candidates for migration to In-Memory OLTP.</span></span>

<span data-ttu-id="fed59-114">Az SSMS jelentést:</span><span class="sxs-lookup"><span data-stu-id="fed59-114">In SSMS, to generate the report:</span></span>

* <span data-ttu-id="fed59-115">Az a **Object Explorer**, kattintson a jobb gombbal az adatbázis-csomópont.</span><span class="sxs-lookup"><span data-stu-id="fed59-115">In the **Object Explorer**, right-click your database node.</span></span>
* <span data-ttu-id="fed59-116">Kattintson a **jelentések** > **szabványos jelentések** > **tranzakció teljesítményét elemző áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="fed59-116">Click **Reports** > **Standard Reports** > **Transaction Performance Analysis Overview**.</span></span>

<span data-ttu-id="fed59-117">További információkért lásd: [meghatározása, ha tábla vagy tárolt eljárás kell használatát. a memórián belüli online Tranzakciófeldolgozási](http://msdn.microsoft.com/library/dn205133.aspx).</span><span class="sxs-lookup"><span data-stu-id="fed59-117">For more information, see [Determining if a Table or Stored Procedure Should Be Ported to In-Memory OLTP](http://msdn.microsoft.com/library/dn205133.aspx).</span></span>

## <a name="step-3-create-a-comparable-test-database"></a><span data-ttu-id="fed59-118">3. lépés: Összehasonlítható teszt-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="fed59-118">Step 3: Create a comparable test database</span></span>
<span data-ttu-id="fed59-119">Tegyük fel, hogy a jelentés azt jelzi, az adatbázis előnyös alakít át egy memóriaoptimalizált tábla tábla.</span><span class="sxs-lookup"><span data-stu-id="fed59-119">Suppose the report indicates your database has a table that would benefit from being converted to a memory-optimized table.</span></span> <span data-ttu-id="fed59-120">Azt javasoljuk, hogy Ön először ellenőrizze, hogy erősítse meg az arra utal, hogy a tesztelés.</span><span class="sxs-lookup"><span data-stu-id="fed59-120">We recommend that you first test to confirm the indication by testing.</span></span>

<span data-ttu-id="fed59-121">Az éles adatbázis test másolatának van szüksége.</span><span class="sxs-lookup"><span data-stu-id="fed59-121">You need a test copy of your production database.</span></span> <span data-ttu-id="fed59-122">A teszt adatbázis ugyanazon a szinten szolgáltatási réteg az éles adatbázis kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fed59-122">The test database should be at the same service tier level as your production database.</span></span>

<span data-ttu-id="fed59-123">Megkönnyítése érdekében a teszteléshez, végeznünk a tesztadatbázis az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fed59-123">To ease testing, tweak your test database as follows:</span></span>

1. <span data-ttu-id="fed59-124">A teszt adatbázis csatlakozni a szolgáltatáshoz az SSMS használatával.</span><span class="sxs-lookup"><span data-stu-id="fed59-124">Connect to the test database by using SSMS.</span></span>
2. <span data-ttu-id="fed59-125">A WITH (SNAPSHOT) lehetőséget a lekérdezésekben kellene elkerüléséhez állítsa az adatbázis-beállítás, ahogy az a következő T-SQL utasítást:</span><span class="sxs-lookup"><span data-stu-id="fed59-125">To avoid needing the WITH (SNAPSHOT) option in queries, set the database option as shown in the following T-SQL statement:</span></span>
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a><span data-ttu-id="fed59-126">4. lépés: A tábla áttelepítése</span><span class="sxs-lookup"><span data-stu-id="fed59-126">Step 4: Migrate tables</span></span>
<span data-ttu-id="fed59-127">Kell létrehozni, és feltölti a vizsgálni kívánt tábla memóriaoptimalizált másolatát.</span><span class="sxs-lookup"><span data-stu-id="fed59-127">You must create and populate a memory-optimized copy of the table you want to test.</span></span> <span data-ttu-id="fed59-128">Létrehozhat használatával:</span><span class="sxs-lookup"><span data-stu-id="fed59-128">You can create it by using either:</span></span>

* <span data-ttu-id="fed59-129">A hasznos memória optimalizálási varázsló szolgáltatáshoz az ssms.</span><span class="sxs-lookup"><span data-stu-id="fed59-129">The handy Memory Optimization Wizard in SSMS.</span></span>
* <span data-ttu-id="fed59-130">Kézi T-SQL.</span><span class="sxs-lookup"><span data-stu-id="fed59-130">Manual T-SQL.</span></span>

#### <a name="memory-optimization-wizard-in-ssms"></a><span data-ttu-id="fed59-131">Memória optimalizálási varázsló szolgáltatáshoz az ssms</span><span class="sxs-lookup"><span data-stu-id="fed59-131">Memory Optimization Wizard in SSMS</span></span>
<span data-ttu-id="fed59-132">A áttelepítési beállítás használata:</span><span class="sxs-lookup"><span data-stu-id="fed59-132">To use this migration option:</span></span>

1. <span data-ttu-id="fed59-133">Csatlakozás a teszt adatbázishoz ssms alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="fed59-133">Connect to the test database with SSMS.</span></span>
2. <span data-ttu-id="fed59-134">Az a **Object Explorer**, kattintson a jobb gombbal a táblában, és kattintson a **memória optimalizálási Advisor**.</span><span class="sxs-lookup"><span data-stu-id="fed59-134">In the **Object Explorer**, right-click on the table, and then click **Memory Optimization Advisor**.</span></span>
   
   * <span data-ttu-id="fed59-135">A **tábla memória optimalizáló Advisor** varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fed59-135">The **Table Memory Optimizer Advisor** wizard is displayed.</span></span>
3. <span data-ttu-id="fed59-136">A varázslóban kattintson **áttelepítési érvényesítési** (vagy a **következő** gomb) a tábla tartalmaz-e a nem támogatott funkciókat, a memóriaoptimalizált táblákban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fed59-136">In the wizard, click **Migration validation** (or the **Next** button) to see if the table has any unsupported features that are unsupported in memory-optimized tables.</span></span> <span data-ttu-id="fed59-137">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="fed59-137">For more information, see:</span></span>
   
   * <span data-ttu-id="fed59-138">A *memória optimalizálási ellenőrzőlista* a [memória optimalizálási Advisor](http://msdn.microsoft.com/library/dn284308.aspx).</span><span class="sxs-lookup"><span data-stu-id="fed59-138">The *memory optimization checklist* in [Memory Optimization Advisor](http://msdn.microsoft.com/library/dn284308.aspx).</span></span>
   * <span data-ttu-id="fed59-139">[Nem támogatja a memórián belüli online Tranzakciófeldolgozási Transact-SQL szerkezetek](http://msdn.microsoft.com/library/dn246937.aspx).</span><span class="sxs-lookup"><span data-stu-id="fed59-139">[Transact-SQL Constructs Not Supported by In-Memory OLTP](http://msdn.microsoft.com/library/dn246937.aspx).</span></span>
   * <span data-ttu-id="fed59-140">[A memórián belüli online Tranzakciófeldolgozási történő](http://msdn.microsoft.com/library/dn247639.aspx).</span><span class="sxs-lookup"><span data-stu-id="fed59-140">[Migrating to In-Memory OLTP](http://msdn.microsoft.com/library/dn247639.aspx).</span></span>
4. <span data-ttu-id="fed59-141">Ha a tábla nem nem támogatott szolgáltatások, az advisor hajthat végre a tényleges séma- és adatáttelepítés meg.</span><span class="sxs-lookup"><span data-stu-id="fed59-141">If the table has no unsupported features, the advisor can perform the actual schema and data migration for you.</span></span>

#### <a name="manual-t-sql"></a><span data-ttu-id="fed59-142">Kézi T-SQL</span><span class="sxs-lookup"><span data-stu-id="fed59-142">Manual T-SQL</span></span>
<span data-ttu-id="fed59-143">A áttelepítési beállítás használata:</span><span class="sxs-lookup"><span data-stu-id="fed59-143">To use this migration option:</span></span>

1. <span data-ttu-id="fed59-144">A teszt adatbázis csatlakozni a szolgáltatáshoz az SSMS (vagy egy hasonló segédprogram) segítségével.</span><span class="sxs-lookup"><span data-stu-id="fed59-144">Connect to your test database by using SSMS (or a similar utility).</span></span>
2. <span data-ttu-id="fed59-145">Szerezze be a teljes T-SQL-parancsfájlt a táblázat és az indexeket.</span><span class="sxs-lookup"><span data-stu-id="fed59-145">Obtain the complete T-SQL script for your table and its indexes.</span></span>
   
   * <span data-ttu-id="fed59-146">Az SSMS kattintson jobb gombbal a tábla csomópontra.</span><span class="sxs-lookup"><span data-stu-id="fed59-146">In SSMS, right-click your table node.</span></span>
   * <span data-ttu-id="fed59-147">Kattintson a **a táblázatban parancsfájl** > **hozhat létre** > **új lekérdezési ablakba**.</span><span class="sxs-lookup"><span data-stu-id="fed59-147">Click **Script Table As** > **CREATE To** > **New Query Window**.</span></span>
3. <span data-ttu-id="fed59-148">A parancsfájl ablakban adja hozzá a WITH (MEMORY_OPTIMIZED = ON) a CREATE TABLE utasításban.</span><span class="sxs-lookup"><span data-stu-id="fed59-148">In the script window, add WITH (MEMORY_OPTIMIZED = ON) to the CREATE TABLE statement.</span></span>
4. <span data-ttu-id="fed59-149">Ha egy FÜRTÖZÖTT index, módosítsa a NONCLUSTERED.</span><span class="sxs-lookup"><span data-stu-id="fed59-149">If there is a CLUSTERED index, change it to NONCLUSTERED.</span></span>
5. <span data-ttu-id="fed59-150">Nevezze át a meglévő táblázat SP_RENAME.</span><span class="sxs-lookup"><span data-stu-id="fed59-150">Rename the existing table by using SP_RENAME.</span></span>
6. <span data-ttu-id="fed59-151">A szerkesztett CREATE TABLE parancsfájl futtatásával hozzon létre a tábla új memóriaoptimalizált példányát.</span><span class="sxs-lookup"><span data-stu-id="fed59-151">Create the new memory-optimized copy of the table by running your edited CREATE TABLE script.</span></span>
7. <span data-ttu-id="fed59-152">Másolja az adatokat a memóriaoptimalizált táblához INSERT használatával... VÁLASSZA KI * BE:</span><span class="sxs-lookup"><span data-stu-id="fed59-152">Copy the data to your memory-optimized table by using INSERT...SELECT * INTO:</span></span>

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a><span data-ttu-id="fed59-153">(Választható) 5. lépés: telepítse át a tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="fed59-153">Step 5 (optional): Migrate stored procedures</span></span>
<span data-ttu-id="fed59-154">A memórián belüli szolgáltatás módosíthatja is javítja a teljesítményt a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="fed59-154">The In-Memory feature can also modify a stored procedure for improved performance.</span></span>

### <a name="considerations-with-natively-compiled-stored-procedures"></a><span data-ttu-id="fed59-155">A natív módon lefordított tárolt eljárásokkal kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="fed59-155">Considerations with natively compiled stored procedures</span></span>
<span data-ttu-id="fed59-156">Egy natív módon lefordított tárolt eljárásból kell rendelkeznie a T-SQL WITH záradék a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="fed59-156">A natively compiled stored procedure must have the following options on its T-SQL WITH clause:</span></span>

* <span data-ttu-id="fed59-157">WITH NATIVE_COMPILATION BEÁLLÍTÁST</span><span class="sxs-lookup"><span data-stu-id="fed59-157">NATIVE_COMPILATION</span></span>
* <span data-ttu-id="fed59-158">Sémához kötési: tábla, amelyen a tárolt eljárás nem módosul, hogy az hatással lenne a tárolt eljárás csak akkor dobható el, a tárolt eljárás oszlop definícióikat jelentését.</span><span class="sxs-lookup"><span data-stu-id="fed59-158">SCHEMABINDING: meaning tables that the stored procedure cannot have their column definitions changed in any way that would affect the stored procedure, unless you drop the stored procedure.</span></span>

<span data-ttu-id="fed59-159">Egy natív modult kell használni a nagy [ATOMI blokkokban](http://msdn.microsoft.com/library/dn452281.aspx) tranzakció-kezelésre.</span><span class="sxs-lookup"><span data-stu-id="fed59-159">A native module must use one big [ATOMIC blocks](http://msdn.microsoft.com/library/dn452281.aspx) for transaction management.</span></span> <span data-ttu-id="fed59-160">Az explicit BEGIN TRANSACTION, vagy a ROLLBACK TRANSACTION Role szerepkör nincs.</span><span class="sxs-lookup"><span data-stu-id="fed59-160">There is no role for an explicit BEGIN TRANSACTION, or for ROLLBACK TRANSACTION.</span></span> <span data-ttu-id="fed59-161">A kód egy üzleti szabály megsértését észleli, ha azt is leáll az atomic blokk egy [THROW](http://msdn.microsoft.com/library/ee677615.aspx) utasítást.</span><span class="sxs-lookup"><span data-stu-id="fed59-161">If your code detects a violation of a business rule, it can terminate the atomic block with a [THROW](http://msdn.microsoft.com/library/ee677615.aspx) statement.</span></span>

### <a name="typical-create-procedure-for-natively-compiled"></a><span data-ttu-id="fed59-162">A tipikus CREATE PROCEDURE natív módon lefordított.</span><span class="sxs-lookup"><span data-stu-id="fed59-162">Typical CREATE PROCEDURE for natively compiled</span></span>
<span data-ttu-id="fed59-163">Általában a T-SQL natív módon lefordított tárolt eljárás létrehozása a következő sablon hasonlít:</span><span class="sxs-lookup"><span data-stu-id="fed59-163">Typically the T-SQL to create a natively compiled stored procedure is similar to the following template:</span></span>

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

* <span data-ttu-id="fed59-164">A TRANSACTION_ISOLATION_LEVEL a PILLANATKÉP érték a leggyakrabban használt a natív módon lefordított tárolt eljárásból.</span><span class="sxs-lookup"><span data-stu-id="fed59-164">For the TRANSACTION_ISOLATION_LEVEL, SNAPSHOT is the most common value for the natively compiled stored procedure.</span></span> <span data-ttu-id="fed59-165">Azonban más értékek egy részhalmazát is támogatottak:</span><span class="sxs-lookup"><span data-stu-id="fed59-165">However,  a subset of the other values are also supported:</span></span>
  
  * <span data-ttu-id="fed59-166">ISMÉTELHETŐ OLVASÁS</span><span class="sxs-lookup"><span data-stu-id="fed59-166">REPEATABLE READ</span></span>
  * <span data-ttu-id="fed59-167">A SZERIALIZÁLHATÓ</span><span class="sxs-lookup"><span data-stu-id="fed59-167">SERIALIZABLE</span></span>
* <span data-ttu-id="fed59-168">A NYELVI érték szerepelnie kell a sys.languages nézetben.</span><span class="sxs-lookup"><span data-stu-id="fed59-168">The LANGUAGE value must be present in the sys.languages view.</span></span>

### <a name="how-to-migrate-a-stored-procedure"></a><span data-ttu-id="fed59-169">Hogyan telepíthet át egy tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="fed59-169">How to migrate a stored procedure</span></span>
<span data-ttu-id="fed59-170">Az áttelepítés lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="fed59-170">The migration steps are:</span></span>

1. <span data-ttu-id="fed59-171">Szerezze be a CREATE PROCEDURE parancsfájlt, amellyel a rendszeres értelmezett tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="fed59-171">Obtain the CREATE PROCEDURE script to the regular interpreted stored procedure.</span></span>
2. <span data-ttu-id="fed59-172">A fejlécben megfelelően az előző sablon újraírási.</span><span class="sxs-lookup"><span data-stu-id="fed59-172">Rewrite its header to match the previous template.</span></span>
3. <span data-ttu-id="fed59-173">Annak megállapítása, hogy a tárolt eljárás T-SQL-kódot használja-e ki a szolgáltatásokat, amelyeket nem támogat natív módon lefordított tárolt eljárásokhoz.</span><span class="sxs-lookup"><span data-stu-id="fed59-173">Ascertain whether the stored procedure T-SQL code uses any features that are not supported for natively compiled stored procedures.</span></span> <span data-ttu-id="fed59-174">Lehetséges megoldások megvalósításához, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="fed59-174">Implement workarounds if necessary.</span></span>
   
   * <span data-ttu-id="fed59-175">További információ: [áttelepítésekor fellépő hibák tárolt eljárások natív módon lefordított](http://msdn.microsoft.com/library/dn296678.aspx).</span><span class="sxs-lookup"><span data-stu-id="fed59-175">For details see [Migration Issues for Natively Compiled Stored Procedures](http://msdn.microsoft.com/library/dn296678.aspx).</span></span>
4. <span data-ttu-id="fed59-176">Nevezze át a régi tárolt eljárás SP_RENAME.</span><span class="sxs-lookup"><span data-stu-id="fed59-176">Rename the old stored procedure by using SP_RENAME.</span></span> <span data-ttu-id="fed59-177">Vagy egyszerűen ELDOBNI.</span><span class="sxs-lookup"><span data-stu-id="fed59-177">Or simply DROP it.</span></span>
5. <span data-ttu-id="fed59-178">A szerkesztett létrehozása eljárás T-SQL-parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="fed59-178">Run your edited CREATE PROCEDURE T-SQL script.</span></span>

## <a name="step-6-run-your-workload-in-test"></a><span data-ttu-id="fed59-179">6. lépés: A számítási feladatok futtatása teszt</span><span class="sxs-lookup"><span data-stu-id="fed59-179">Step 6: Run your workload in test</span></span>
<span data-ttu-id="fed59-180">A munkaterhelés futtassa a test adatbázisban hasonló az alkalmazások, az éles adatbázis futó.</span><span class="sxs-lookup"><span data-stu-id="fed59-180">Run a workload in your test database that is similar to the workload that runs in your production database.</span></span> <span data-ttu-id="fed59-181">Ez a memórián belüli funkciójának használatát a táblák és tárolt eljárások megvalósítani jobb teljesítménye kódproblémájáról van.</span><span class="sxs-lookup"><span data-stu-id="fed59-181">This should reveal the performance gain achieved by your use of the In-Memory feature for tables and stored procedures.</span></span>

<span data-ttu-id="fed59-182">Jelentős részét a munkaterhelésből attribútumok:</span><span class="sxs-lookup"><span data-stu-id="fed59-182">Major attributes of the workload are:</span></span>

* <span data-ttu-id="fed59-183">Egyidejű kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="fed59-183">Number of concurrent connections.</span></span>
* <span data-ttu-id="fed59-184">Olvasási/írási arányt.</span><span class="sxs-lookup"><span data-stu-id="fed59-184">Read/write ratio.</span></span>

<span data-ttu-id="fed59-185">Testre szabni, és futtassa a test-munkaterhelés, fontolja meg a hasznos ostress.exe eszközt, amely a [Itt](sql-database-in-memory.md).</span><span class="sxs-lookup"><span data-stu-id="fed59-185">To tailor and run the test workload, consider using the handy ostress.exe tool, which illustrated in [here](sql-database-in-memory.md).</span></span>

<span data-ttu-id="fed59-186">Hálózati késés csökkentése érdekében érdemes, a teszt futtatása a azonos Azure földrajzi régióban, ahol az adatbázis létezik.</span><span class="sxs-lookup"><span data-stu-id="fed59-186">To minimize network latency, run your test in the same Azure geographic region where the database exists.</span></span>

## <a name="step-7-post-implementation-monitoring"></a><span data-ttu-id="fed59-187">7. lépés: A megvalósítás utáni figyelése</span><span class="sxs-lookup"><span data-stu-id="fed59-187">Step 7: Post-implementation monitoring</span></span>
<span data-ttu-id="fed59-188">Vegye figyelembe, hogy az éles környezetben a memórián belüli megvalósítások teljesítmény hatásainak figyelése:</span><span class="sxs-lookup"><span data-stu-id="fed59-188">Consider monitoring the performance effects of your In-Memory implementations in production:</span></span>

* <span data-ttu-id="fed59-189">[Figyelje a memórián belüli tároló](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="fed59-189">[Monitor In-Memory Storage](sql-database-in-memory-oltp-monitoring.md).</span></span>
* [<span data-ttu-id="fed59-190">Az Azure SQL Database felügyelete dinamikus felügyeleti nézetek használatával</span><span class="sxs-lookup"><span data-stu-id="fed59-190">Monitoring Azure SQL Database using dynamic management views</span></span>](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a><span data-ttu-id="fed59-191">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="fed59-191">Related links</span></span>
* [<span data-ttu-id="fed59-192">Memórián belüli online Tranzakciófeldolgozási (a memóriában optimalizálása)</span><span class="sxs-lookup"><span data-stu-id="fed59-192">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
* [<span data-ttu-id="fed59-193">A natív módon lefordított tárolt eljárások bemutatása</span><span class="sxs-lookup"><span data-stu-id="fed59-193">Introduction to Natively Compiled Stored Procedures</span></span>](http://msdn.microsoft.com/library/dn133184.aspx)
* [<span data-ttu-id="fed59-194">Memória optimalizálási Advisor</span><span class="sxs-lookup"><span data-stu-id="fed59-194">Memory Optimization Advisor</span></span>](http://msdn.microsoft.com/library/dn284308.aspx)


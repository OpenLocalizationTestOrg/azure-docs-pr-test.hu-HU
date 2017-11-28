---
title: "aaaIn memórián belüli online Tranzakciófeldolgozási javítja az SQL túlhasználat telj |} Microsoft Docs"
description: "Fogadja el a memórián belüli online Tranzakciófeldolgozási tooimprove tranzakciós teljesítmény meglévő SQL-adatbázisban."
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
ms.openlocfilehash: 1bed796069565eb6741953837cf0477c2ddd8519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-in-memory-oltp-tooimprove-your-application-performance-in-sql-database"></a><span data-ttu-id="7369d-103">Használja a memórián belüli online Tranzakciófeldolgozási tooimprove az alkalmazások teljesítményéről, az SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="7369d-103">Use In-Memory OLTP tooimprove your application performance in SQL Database</span></span>
<span data-ttu-id="7369d-104">[A memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md) lehet a tranzakció-feldolgozást, adatfeldolgozást és átmeneti adatáttelepítések esetében használt tooimprove hello teljesítményének [prémium](sql-database-service-tiers.md) Azure SQL-adatbázisok nélkül növelése hello árképzési szint.</span><span class="sxs-lookup"><span data-stu-id="7369d-104">[In-Memory OLTP](sql-database-in-memory.md) can be used tooimprove hello performance of transaction processing, data ingestion, and transient data scenarios, in  [Premium](sql-database-service-tiers.md) Azure SQL Databases without increasing hello pricing tier.</span></span> 

> [!NOTE] 
> <span data-ttu-id="7369d-105">Megtudhatja, hogyan [kvórum kulcsának adatbázis munkaterhelés megduplázódik, miközben csökkenti a DTU az SQL Database 70 %-kal](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span><span class="sxs-lookup"><span data-stu-id="7369d-105">Learn how [Quorum doubles key database’s workload while lowering DTU by 70% with SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)</span></span>


<span data-ttu-id="7369d-106">Kövesse a lépéseket tooadopt memórián belüli online Tranzakciófeldolgozási a meglévő adatbázis.</span><span class="sxs-lookup"><span data-stu-id="7369d-106">Follow these steps tooadopt In-Memory OLTP in your existing database.</span></span>

## <a name="step-1-ensure-you-are-using-a-premium-database"></a><span data-ttu-id="7369d-107">1. lépés: Győződjön meg arról, a prémium szintű adatbázist használ</span><span class="sxs-lookup"><span data-stu-id="7369d-107">Step 1: Ensure you are using a Premium database</span></span>
<span data-ttu-id="7369d-108">A memórián belüli online Tranzakciófeldolgozási csak Premium adatbázisokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="7369d-108">In-Memory OLTP is supported only in Premium databases.</span></span> <span data-ttu-id="7369d-109">A memóriában támogatott, ha hello visszaadott eredmény 1 (0):</span><span class="sxs-lookup"><span data-stu-id="7369d-109">In-Memory is supported if hello returned result is 1 (not 0):</span></span>

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

<span data-ttu-id="7369d-110">*XTP* rövidítése *szélsőséges tranzakció feldolgozása*</span><span class="sxs-lookup"><span data-stu-id="7369d-110">*XTP* stands for *Extreme Transaction Processing*</span></span>



## <a name="step-2-identify-objects-toomigrate-tooin-memory-oltp"></a><span data-ttu-id="7369d-111">2. lépés: Azonosítsa objektumok toomigrate tooIn memórián belüli online Tranzakciófeldolgozási</span><span class="sxs-lookup"><span data-stu-id="7369d-111">Step 2: Identify objects toomigrate tooIn-Memory OLTP</span></span>
<span data-ttu-id="7369d-112">SSMS tartalmaz egy **tranzakció teljesítményét elemző áttekintése** jelentést, amely egy aktív munkaterhelés adatbázis is futtathatók.</span><span class="sxs-lookup"><span data-stu-id="7369d-112">SSMS includes a **Transaction Performance Analysis Overview** report that you can run against a database with an active workload.</span></span> <span data-ttu-id="7369d-113">hello a jelentés azonosítja a táblák és tárolt eljárások a deduplikációra kijelölt áttelepítési tooIn memórián belüli online Tranzakciófeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="7369d-113">hello report identifies tables and stored procedures that are candidates for migration tooIn-Memory OLTP.</span></span>

<span data-ttu-id="7369d-114">Az SSMS, toogenerate hello jelentés:</span><span class="sxs-lookup"><span data-stu-id="7369d-114">In SSMS, toogenerate hello report:</span></span>

* <span data-ttu-id="7369d-115">A hello **Object Explorer**, kattintson a jobb gombbal az adatbázis-csomópont.</span><span class="sxs-lookup"><span data-stu-id="7369d-115">In hello **Object Explorer**, right-click your database node.</span></span>
* <span data-ttu-id="7369d-116">Kattintson a **jelentések** > **szabványos jelentések** > **tranzakció teljesítményét elemző áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="7369d-116">Click **Reports** > **Standard Reports** > **Transaction Performance Analysis Overview**.</span></span>

<span data-ttu-id="7369d-117">További információkért lásd: [meghatározása, ha tábla vagy tárolt eljárás kell legelterjedtebb tooIn memórián belüli online Tranzakciófeldolgozási](http://msdn.microsoft.com/library/dn205133.aspx).</span><span class="sxs-lookup"><span data-stu-id="7369d-117">For more information, see [Determining if a Table or Stored Procedure Should Be Ported tooIn-Memory OLTP](http://msdn.microsoft.com/library/dn205133.aspx).</span></span>

## <a name="step-3-create-a-comparable-test-database"></a><span data-ttu-id="7369d-118">3. lépés: Összehasonlítható teszt-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="7369d-118">Step 3: Create a comparable test database</span></span>
<span data-ttu-id="7369d-119">Tegyük fel, hogy hello jelentés azt jelzi, az adatbázis egy táblát, amely éppen előnyös konvertálta tooa memóriaoptimalizált tábla.</span><span class="sxs-lookup"><span data-stu-id="7369d-119">Suppose hello report indicates your database has a table that would benefit from being converted tooa memory-optimized table.</span></span> <span data-ttu-id="7369d-120">Azt javasoljuk, hogy először tesztelje a tooconfirm hello arra utal, hogy végzett teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7369d-120">We recommend that you first test tooconfirm hello indication by testing.</span></span>

<span data-ttu-id="7369d-121">Az éles adatbázis test másolatának van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7369d-121">You need a test copy of your production database.</span></span> <span data-ttu-id="7369d-122">hello tesztadatbázis kell lennie: hello azonos szolgáltatási réteg szintjét az éles adatbázis.</span><span class="sxs-lookup"><span data-stu-id="7369d-122">hello test database should be at hello same service tier level as your production database.</span></span>

<span data-ttu-id="7369d-123">tooease tesztelése, tudjon végezni az adatbázis tesztelése az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="7369d-123">tooease testing, tweak your test database as follows:</span></span>

1. <span data-ttu-id="7369d-124">Kapcsolódás toohello tesztadatbázis az SSMS használatával.</span><span class="sxs-lookup"><span data-stu-id="7369d-124">Connect toohello test database by using SSMS.</span></span>
2. <span data-ttu-id="7369d-125">hello lekérdezésekben WITH (SNAPSHOT) lehetőséget, és állítsa hello adatbázis-beállítás, ahogy az következő T-SQL-utasítást hello kellene tooavoid:</span><span class="sxs-lookup"><span data-stu-id="7369d-125">tooavoid needing hello WITH (SNAPSHOT) option in queries, set hello database option as shown in hello following T-SQL statement:</span></span>
   
   ```
   ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
   ```

## <a name="step-4-migrate-tables"></a><span data-ttu-id="7369d-126">4. lépés: A tábla áttelepítése</span><span class="sxs-lookup"><span data-stu-id="7369d-126">Step 4: Migrate tables</span></span>
<span data-ttu-id="7369d-127">Létre kell hoznia és memóriaoptimalizált másolatot feltöltése hello tábla tootest szeretné.</span><span class="sxs-lookup"><span data-stu-id="7369d-127">You must create and populate a memory-optimized copy of hello table you want tootest.</span></span> <span data-ttu-id="7369d-128">Létrehozhat használatával:</span><span class="sxs-lookup"><span data-stu-id="7369d-128">You can create it by using either:</span></span>

* <span data-ttu-id="7369d-129">hello szolgáltatáshoz az ssms hasznos memória optimalizálási varázsló.</span><span class="sxs-lookup"><span data-stu-id="7369d-129">hello handy Memory Optimization Wizard in SSMS.</span></span>
* <span data-ttu-id="7369d-130">Kézi T-SQL.</span><span class="sxs-lookup"><span data-stu-id="7369d-130">Manual T-SQL.</span></span>

#### <a name="memory-optimization-wizard-in-ssms"></a><span data-ttu-id="7369d-131">Memória optimalizálási varázsló szolgáltatáshoz az ssms</span><span class="sxs-lookup"><span data-stu-id="7369d-131">Memory Optimization Wizard in SSMS</span></span>
<span data-ttu-id="7369d-132">toouse az áttelepítési lehetőség:</span><span class="sxs-lookup"><span data-stu-id="7369d-132">toouse this migration option:</span></span>

1. <span data-ttu-id="7369d-133">Csatlakozás toohello tesztadatbázis ssms alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="7369d-133">Connect toohello test database with SSMS.</span></span>
2. <span data-ttu-id="7369d-134">A hello **Object Explorer**, hello táblán kattintson a jobb gombbal, és kattintson a **memória optimalizálási Advisor**.</span><span class="sxs-lookup"><span data-stu-id="7369d-134">In hello **Object Explorer**, right-click on hello table, and then click **Memory Optimization Advisor**.</span></span>
   
   * <span data-ttu-id="7369d-135">Hello **tábla memória optimalizáló Advisor** varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7369d-135">hello **Table Memory Optimizer Advisor** wizard is displayed.</span></span>
3. <span data-ttu-id="7369d-136">Hello varázslóban kattintson **áttelepítési érvényesítési** (vagy hello **következő** gomb) toosee Ha hello tábla tartozik-e a memóriaoptimalizált táblákban nem támogatott funkciók nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7369d-136">In hello wizard, click **Migration validation** (or hello **Next** button) toosee if hello table has any unsupported features that are unsupported in memory-optimized tables.</span></span> <span data-ttu-id="7369d-137">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="7369d-137">For more information, see:</span></span>
   
   * <span data-ttu-id="7369d-138">Hello *memória optimalizálási ellenőrzőlista* a [memória optimalizálási Advisor](http://msdn.microsoft.com/library/dn284308.aspx).</span><span class="sxs-lookup"><span data-stu-id="7369d-138">hello *memory optimization checklist* in [Memory Optimization Advisor](http://msdn.microsoft.com/library/dn284308.aspx).</span></span>
   * <span data-ttu-id="7369d-139">[Nem támogatja a memórián belüli online Tranzakciófeldolgozási Transact-SQL szerkezetek](http://msdn.microsoft.com/library/dn246937.aspx).</span><span class="sxs-lookup"><span data-stu-id="7369d-139">[Transact-SQL Constructs Not Supported by In-Memory OLTP](http://msdn.microsoft.com/library/dn246937.aspx).</span></span>
   * <span data-ttu-id="7369d-140">[Áttelepítése tooIn memórián belüli online Tranzakciófeldolgozási](http://msdn.microsoft.com/library/dn247639.aspx).</span><span class="sxs-lookup"><span data-stu-id="7369d-140">[Migrating tooIn-Memory OLTP](http://msdn.microsoft.com/library/dn247639.aspx).</span></span>
4. <span data-ttu-id="7369d-141">Ha hello táblának nincs nem támogatott funkciókat, hello advisor végezhet hello tényleges séma- és adatáttelepítés meg.</span><span class="sxs-lookup"><span data-stu-id="7369d-141">If hello table has no unsupported features, hello advisor can perform hello actual schema and data migration for you.</span></span>

#### <a name="manual-t-sql"></a><span data-ttu-id="7369d-142">Kézi T-SQL</span><span class="sxs-lookup"><span data-stu-id="7369d-142">Manual T-SQL</span></span>
<span data-ttu-id="7369d-143">toouse az áttelepítési lehetőség:</span><span class="sxs-lookup"><span data-stu-id="7369d-143">toouse this migration option:</span></span>

1. <span data-ttu-id="7369d-144">Csatlakozás tooyour tesztadatbázis SSMS (vagy egy hasonló segédprogramot) használatával.</span><span class="sxs-lookup"><span data-stu-id="7369d-144">Connect tooyour test database by using SSMS (or a similar utility).</span></span>
2. <span data-ttu-id="7369d-145">Szerezze be a hello teljes T-SQL-parancsfájlt a táblázat és az indexeket.</span><span class="sxs-lookup"><span data-stu-id="7369d-145">Obtain hello complete T-SQL script for your table and its indexes.</span></span>
   
   * <span data-ttu-id="7369d-146">Az SSMS kattintson jobb gombbal a tábla csomópontra.</span><span class="sxs-lookup"><span data-stu-id="7369d-146">In SSMS, right-click your table node.</span></span>
   * <span data-ttu-id="7369d-147">Kattintson a **a táblázatban parancsfájl** > **hozhat létre** > **új lekérdezési ablakba**.</span><span class="sxs-lookup"><span data-stu-id="7369d-147">Click **Script Table As** > **CREATE To** > **New Query Window**.</span></span>
3. <span data-ttu-id="7369d-148">A hello parancsfájl ablakban, adja hozzá a WITH (MEMORY_OPTIMIZED = ON) toohello CREATE TABLE utasítás.</span><span class="sxs-lookup"><span data-stu-id="7369d-148">In hello script window, add WITH (MEMORY_OPTIMIZED = ON) toohello CREATE TABLE statement.</span></span>
4. <span data-ttu-id="7369d-149">Ha egy FÜRTÖZÖTT index, módosíthatja tooNONCLUSTERED.</span><span class="sxs-lookup"><span data-stu-id="7369d-149">If there is a CLUSTERED index, change it tooNONCLUSTERED.</span></span>
5. <span data-ttu-id="7369d-150">Nevezze át a meglévő tábla hello SP_RENAME használatával.</span><span class="sxs-lookup"><span data-stu-id="7369d-150">Rename hello existing table by using SP_RENAME.</span></span>
6. <span data-ttu-id="7369d-151">A szerkesztett CREATE TABLE parancsfájl futtatásával hozzon létre hello új memóriaoptimalizált hello tábla példányát.</span><span class="sxs-lookup"><span data-stu-id="7369d-151">Create hello new memory-optimized copy of hello table by running your edited CREATE TABLE script.</span></span>
7. <span data-ttu-id="7369d-152">Másolja a hello adatok tooyour memóriaoptimalizált tábla INSERT használatával... VÁLASSZA KI * BE:</span><span class="sxs-lookup"><span data-stu-id="7369d-152">Copy hello data tooyour memory-optimized table by using INSERT...SELECT * INTO:</span></span>

```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a><span data-ttu-id="7369d-153">(Választható) 5. lépés: telepítse át a tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="7369d-153">Step 5 (optional): Migrate stored procedures</span></span>
<span data-ttu-id="7369d-154">hello memórián belüli szolgáltatás módosíthatja is javítja a teljesítményt a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="7369d-154">hello In-Memory feature can also modify a stored procedure for improved performance.</span></span>

### <a name="considerations-with-natively-compiled-stored-procedures"></a><span data-ttu-id="7369d-155">A natív módon lefordított tárolt eljárásokkal kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="7369d-155">Considerations with natively compiled stored procedures</span></span>
<span data-ttu-id="7369d-156">Egy natív módon lefordított tárolt eljárás a következő beállításokat a T-SQL WITH záradék hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="7369d-156">A natively compiled stored procedure must have hello following options on its T-SQL WITH clause:</span></span>

* <span data-ttu-id="7369d-157">WITH NATIVE_COMPILATION BEÁLLÍTÁST</span><span class="sxs-lookup"><span data-stu-id="7369d-157">NATIVE_COMPILATION</span></span>
* <span data-ttu-id="7369d-158">Sémához kötési: hello tárolt eljárás jelentése táblákhoz az oszlopdefiníciók megváltozott olyan módon, ami hatással lenne a hello tárolt eljárás csak akkor dobható el, hogy hello tárolt eljárás nem lehet.</span><span class="sxs-lookup"><span data-stu-id="7369d-158">SCHEMABINDING: meaning tables that hello stored procedure cannot have their column definitions changed in any way that would affect hello stored procedure, unless you drop hello stored procedure.</span></span>

<span data-ttu-id="7369d-159">Egy natív modult kell használni a nagy [ATOMI blokkokban](http://msdn.microsoft.com/library/dn452281.aspx) tranzakció-kezelésre.</span><span class="sxs-lookup"><span data-stu-id="7369d-159">A native module must use one big [ATOMIC blocks](http://msdn.microsoft.com/library/dn452281.aspx) for transaction management.</span></span> <span data-ttu-id="7369d-160">Az explicit BEGIN TRANSACTION, vagy a ROLLBACK TRANSACTION Role szerepkör nincs.</span><span class="sxs-lookup"><span data-stu-id="7369d-160">There is no role for an explicit BEGIN TRANSACTION, or for ROLLBACK TRANSACTION.</span></span> <span data-ttu-id="7369d-161">A kód egy üzleti szabály megsértését észleli, ha azt is leáll hello atomi blokkot, amelynek a [THROW](http://msdn.microsoft.com/library/ee677615.aspx) utasítást.</span><span class="sxs-lookup"><span data-stu-id="7369d-161">If your code detects a violation of a business rule, it can terminate hello atomic block with a [THROW](http://msdn.microsoft.com/library/ee677615.aspx) statement.</span></span>

### <a name="typical-create-procedure-for-natively-compiled"></a><span data-ttu-id="7369d-162">A tipikus CREATE PROCEDURE natív módon lefordított.</span><span class="sxs-lookup"><span data-stu-id="7369d-162">Typical CREATE PROCEDURE for natively compiled</span></span>
<span data-ttu-id="7369d-163">T-SQL hello toocreate natív módon lefordított tárolt eljárás általában a következő sablon hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="7369d-163">Typically hello T-SQL toocreate a natively compiled stored procedure is similar toohello following template:</span></span>

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

* <span data-ttu-id="7369d-164">Hello TRANSACTION_ISOLATION_LEVEL PILLANATKÉP esetén hello leggyakoribb értéke hello natív módon lefordított tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="7369d-164">For hello TRANSACTION_ISOLATION_LEVEL, SNAPSHOT is hello most common value for hello natively compiled stored procedure.</span></span> <span data-ttu-id="7369d-165">Azonban egy részhalmazát hello egyéb értékek is támogatottak:</span><span class="sxs-lookup"><span data-stu-id="7369d-165">However,  a subset of hello other values are also supported:</span></span>
  
  * <span data-ttu-id="7369d-166">ISMÉTELHETŐ OLVASÁS</span><span class="sxs-lookup"><span data-stu-id="7369d-166">REPEATABLE READ</span></span>
  * <span data-ttu-id="7369d-167">A SZERIALIZÁLHATÓ</span><span class="sxs-lookup"><span data-stu-id="7369d-167">SERIALIZABLE</span></span>
* <span data-ttu-id="7369d-168">hello NYELVI érték hello sys.languages nézetben jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7369d-168">hello LANGUAGE value must be present in hello sys.languages view.</span></span>

### <a name="how-toomigrate-a-stored-procedure"></a><span data-ttu-id="7369d-169">Hogyan toomigrate tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="7369d-169">How toomigrate a stored procedure</span></span>
<span data-ttu-id="7369d-170">hello áttelepítés lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="7369d-170">hello migration steps are:</span></span>

1. <span data-ttu-id="7369d-171">Szerezze be a hello CREATE PROCEDURE parancsfájl toohello rendszeres értelmezett tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="7369d-171">Obtain hello CREATE PROCEDURE script toohello regular interpreted stored procedure.</span></span>
2. <span data-ttu-id="7369d-172">A fejléc toomatch hello előző sablon újraírási.</span><span class="sxs-lookup"><span data-stu-id="7369d-172">Rewrite its header toomatch hello previous template.</span></span>
3. <span data-ttu-id="7369d-173">Annak megállapítása, hogy használja-e ki a szolgáltatásokat, amelyeket nem támogat natív módon lefordított tárolt eljárások hello tárolt eljárás T-SQL-kódot.</span><span class="sxs-lookup"><span data-stu-id="7369d-173">Ascertain whether hello stored procedure T-SQL code uses any features that are not supported for natively compiled stored procedures.</span></span> <span data-ttu-id="7369d-174">Lehetséges megoldások megvalósításához, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="7369d-174">Implement workarounds if necessary.</span></span>
   
   * <span data-ttu-id="7369d-175">További információ: [áttelepítésekor fellépő hibák tárolt eljárások natív módon lefordított](http://msdn.microsoft.com/library/dn296678.aspx).</span><span class="sxs-lookup"><span data-stu-id="7369d-175">For details see [Migration Issues for Natively Compiled Stored Procedures](http://msdn.microsoft.com/library/dn296678.aspx).</span></span>
4. <span data-ttu-id="7369d-176">Nevezze át a régi tárolt eljárás hello SP_RENAME használatával.</span><span class="sxs-lookup"><span data-stu-id="7369d-176">Rename hello old stored procedure by using SP_RENAME.</span></span> <span data-ttu-id="7369d-177">Vagy egyszerűen ELDOBNI.</span><span class="sxs-lookup"><span data-stu-id="7369d-177">Or simply DROP it.</span></span>
5. <span data-ttu-id="7369d-178">A szerkesztett létrehozása eljárás T-SQL-parancsfájl futtatása.</span><span class="sxs-lookup"><span data-stu-id="7369d-178">Run your edited CREATE PROCEDURE T-SQL script.</span></span>

## <a name="step-6-run-your-workload-in-test"></a><span data-ttu-id="7369d-179">6. lépés: A számítási feladatok futtatása teszt</span><span class="sxs-lookup"><span data-stu-id="7369d-179">Step 6: Run your workload in test</span></span>
<span data-ttu-id="7369d-180">Futtassa a munkaterhelés a hasonló toohello munkaterhelés az éles adatbázis futó adatbázis tesztelése.</span><span class="sxs-lookup"><span data-stu-id="7369d-180">Run a workload in your test database that is similar toohello workload that runs in your production database.</span></span> <span data-ttu-id="7369d-181">Ez kódproblémájáról hello jobb teljesítménye a hello memórián belüli funkciójának használatát a táblák és tárolt eljárások megvalósítani.</span><span class="sxs-lookup"><span data-stu-id="7369d-181">This should reveal hello performance gain achieved by your use of hello In-Memory feature for tables and stored procedures.</span></span>

<span data-ttu-id="7369d-182">Hello munkaterhelés fő attribútumok pedig a következők:</span><span class="sxs-lookup"><span data-stu-id="7369d-182">Major attributes of hello workload are:</span></span>

* <span data-ttu-id="7369d-183">Egyidejű kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="7369d-183">Number of concurrent connections.</span></span>
* <span data-ttu-id="7369d-184">Olvasási/írási arányt.</span><span class="sxs-lookup"><span data-stu-id="7369d-184">Read/write ratio.</span></span>

<span data-ttu-id="7369d-185">tootailor és futtatási hello teszt munkaterhelés, érdemes lehet hello hasznos ostress.exe eszközt, amely a [Itt](sql-database-in-memory.md).</span><span class="sxs-lookup"><span data-stu-id="7369d-185">tootailor and run hello test workload, consider using hello handy ostress.exe tool, which illustrated in [here](sql-database-in-memory.md).</span></span>

<span data-ttu-id="7369d-186">toominimize hálózati késés, a teszt futtatása a hello azonos Azure földrajzi régióban hello adatbázis esetén.</span><span class="sxs-lookup"><span data-stu-id="7369d-186">toominimize network latency, run your test in hello same Azure geographic region where hello database exists.</span></span>

## <a name="step-7-post-implementation-monitoring"></a><span data-ttu-id="7369d-187">7. lépés: A megvalósítás utáni figyelése</span><span class="sxs-lookup"><span data-stu-id="7369d-187">Step 7: Post-implementation monitoring</span></span>
<span data-ttu-id="7369d-188">Vegye figyelembe, hogy éles környezetben a memórián belüli megvalósítások hatásait hello teljesítmény figyelése:</span><span class="sxs-lookup"><span data-stu-id="7369d-188">Consider monitoring hello performance effects of your In-Memory implementations in production:</span></span>

* <span data-ttu-id="7369d-189">[Figyelje a memórián belüli tároló](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="7369d-189">[Monitor In-Memory Storage](sql-database-in-memory-oltp-monitoring.md).</span></span>
* [<span data-ttu-id="7369d-190">Az Azure SQL Database felügyelete dinamikus felügyeleti nézetek használatával</span><span class="sxs-lookup"><span data-stu-id="7369d-190">Monitoring Azure SQL Database using dynamic management views</span></span>](sql-database-monitoring-with-dmvs.md)

## <a name="related-links"></a><span data-ttu-id="7369d-191">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="7369d-191">Related links</span></span>
* [<span data-ttu-id="7369d-192">Memórián belüli online Tranzakciófeldolgozási (a memóriában optimalizálása)</span><span class="sxs-lookup"><span data-stu-id="7369d-192">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
* [<span data-ttu-id="7369d-193">Bevezetés tooNatively tárolt eljárások fordítása</span><span class="sxs-lookup"><span data-stu-id="7369d-193">Introduction tooNatively Compiled Stored Procedures</span></span>](http://msdn.microsoft.com/library/dn133184.aspx)
* [<span data-ttu-id="7369d-194">Memória optimalizálási Advisor</span><span class="sxs-lookup"><span data-stu-id="7369d-194">Memory Optimization Advisor</span></span>](http://msdn.microsoft.com/library/dn284308.aspx)


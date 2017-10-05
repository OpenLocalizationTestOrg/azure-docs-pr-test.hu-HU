---
title: "Lekérdezési teljesítménybe az Azure SQL Database |} Microsoft Docs"
description: "A legtöbb CPU-felhasználása lekérdezések lekérdezési teljesítmény figyeléséhez azonosítja az Azure SQL-adatbázis."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 1925d4ff8f5b16a0df56de987f8653cfd8441c52
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sql-database-query-performance-insight"></a><span data-ttu-id="47740-103">Az Azure SQL adatbázis-lekérdezési Terheléselemző</span><span class="sxs-lookup"><span data-stu-id="47740-103">Azure SQL Database Query Performance Insight</span></span>
<span data-ttu-id="47740-104">Kezelése és a relációs adatbázisok teljesítményének hangolása jelentős szakértelmét és az idő befektetési igénylő nehéz feladat.</span><span class="sxs-lookup"><span data-stu-id="47740-104">Managing and tuning the performance of relational databases is a challenging task that requires significant expertise and time investment.</span></span> <span data-ttu-id="47740-105">Lekérdezési Terheléselemző kevesebb időt azáltal, hogy a következő adatbázis teljesítményének hibaelhárítási teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="47740-105">Query Performance Insight allows you to spend less time troubleshooting database performance by providing the following:</span></span>

* <span data-ttu-id="47740-106">Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést.</span><span class="sxs-lookup"><span data-stu-id="47740-106">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="47740-107">A leggyakoribb lekérdezések szerinti CPU/időtartama/végrehajtás, amely potenciálisan a jobb teljesítmény kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="47740-107">The top queries by CPU/Duration/Execution count, which can potentially be tuned for improved performance.</span></span>
* <span data-ttu-id="47740-108">Részletekbe menően tárhatják fel a részletek a lekérdezés olyan szöveg- és erőforrás-használat előzményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="47740-108">The ability to drill down into the details of a query, view its text and history of resource utilization.</span></span> 
* <span data-ttu-id="47740-109">Teljesítményhangolás által végrehajtott műveleteket megjelenítő jegyzetek [SQL Azure Database Advisor](sql-database-advisor.md)</span><span class="sxs-lookup"><span data-stu-id="47740-109">Performance tuning annotations that show actions performed by [SQL Azure Database Advisor](sql-database-advisor.md)</span></span>  



## <a name="prerequisites"></a><span data-ttu-id="47740-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="47740-110">Prerequisites</span></span>
* <span data-ttu-id="47740-111">A lekérdezési Terheléselemző megköveteli, hogy [Lekérdezéstár](https://msdn.microsoft.com/library/dn817826.aspx) aktív az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="47740-111">Query Performance Insight requires that [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) is active on your database.</span></span> <span data-ttu-id="47740-112">Ha a Lekérdezéstár nem fut, a portál kéri kapcsolja be.</span><span class="sxs-lookup"><span data-stu-id="47740-112">If Query Store is not running, the portal prompts you to turn it on.</span></span>

## <a name="permissions"></a><span data-ttu-id="47740-113">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="47740-113">Permissions</span></span>
<span data-ttu-id="47740-114">A következő [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) lekérdezési Terheléselemző használandó engedélyek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="47740-114">The following [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions are required to use Query Performance Insight:</span></span> 

* <span data-ttu-id="47740-115">**Olvasó**, **tulajdonos**, **közreműködő**, **SQL DB Contributor**, vagy **SQL Server közreműködői** engedélyekre szükség, a lekérdezések és diagramokat fel felső erőforrás megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="47740-115">**Reader**, **Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required to view the top resource consuming queries and charts.</span></span> 
* <span data-ttu-id="47740-116">**Tulajdonos**, **közreműködő**, **SQL DB Contributor**, vagy **SQL Server közreműködői** engedélyekre van szükség a lekérdezés szövegének megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="47740-116">**Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required to view query text.</span></span>

## <a name="using-query-performance-insight"></a><span data-ttu-id="47740-117">Lekérdezési Terheléselemző használatával</span><span class="sxs-lookup"><span data-stu-id="47740-117">Using Query Performance Insight</span></span>
<span data-ttu-id="47740-118">Lekérdezési Terheléselemző könnyen használható:</span><span class="sxs-lookup"><span data-stu-id="47740-118">Query Performance Insight is easy to use:</span></span>

* <span data-ttu-id="47740-119">Nyissa meg [Azure-portálon](https://portal.azure.com/) és a keresési adatbázis, amelyet meg szeretne vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="47740-119">Open [Azure portal](https://portal.azure.com/) and find database that you want to examine.</span></span> 
  * <span data-ttu-id="47740-120">A bal oldali menüben, a támogatási és hibaelhárítási válassza a "Lekérdezési Terheléselemző".</span><span class="sxs-lookup"><span data-stu-id="47740-120">From left-hand side menu, under support and troubleshooting, select “Query Performance Insight”.</span></span>
* <span data-ttu-id="47740-121">Az első lapon tekintse át a legfelső szintű erőforrás-igényes lekérdezések listáját.</span><span class="sxs-lookup"><span data-stu-id="47740-121">On the first tab, review the list of top resource-consuming queries.</span></span>
* <span data-ttu-id="47740-122">Válassza ki az egyes lekérdezések a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="47740-122">Select an individual query to view its details.</span></span>
* <span data-ttu-id="47740-123">Nyissa meg [SQL Azure Database Advisor](sql-database-advisor.md) és ellenőrizze, hogy e javaslatokkal érhető el.</span><span class="sxs-lookup"><span data-stu-id="47740-123">Open [SQL Azure Database Advisor](sql-database-advisor.md) and check if any recommendations are available.</span></span>
* <span data-ttu-id="47740-124">Csúszkákkal vagy nagyítás ikonok megfigyelt időköz módosításához.</span><span class="sxs-lookup"><span data-stu-id="47740-124">Use sliders or zoom icons to change observed interval.</span></span>
  
    ![teljesítmény irányítópult](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> <span data-ttu-id="47740-126">Néhány órányi adatot kell az SQL Database, a lekérdezési teljesítmény áttekintést adnak a Lekérdezéstár lekérdezésével rögzíthetők.</span><span class="sxs-lookup"><span data-stu-id="47740-126">A couple hours of data needs to be captured by Query Store for SQL Database to provide query performance insights.</span></span> <span data-ttu-id="47740-127">Ha az adatbázis nincs tevékenység vagy a Lekérdezéstár nem volt aktív egy bizonyos időn belül, a diagramok üres lesz az adott időszak megjelenítésekor.</span><span class="sxs-lookup"><span data-stu-id="47740-127">If the database has no activity or Query Store was not active during a certain time period, the charts will be empty when displaying that time period.</span></span> <span data-ttu-id="47740-128">A Lekérdezéstár engedélyezheti bármikor, ha az nem futna.</span><span class="sxs-lookup"><span data-stu-id="47740-128">You may enable Query Store at any time if it is not running.</span></span>   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a><span data-ttu-id="47740-129">Tekintse át a erőforrásigényes lekérdezések felső Processzor</span><span class="sxs-lookup"><span data-stu-id="47740-129">Review top CPU consuming queries</span></span>
<span data-ttu-id="47740-130">Az a [portal](http://portal.azure.com) tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="47740-130">In the [portal](http://portal.azure.com) do the following:</span></span>

1. <span data-ttu-id="47740-131">Keresse meg az SQL-adatbázis, és kattintson a **összes beállítás** > **támogatási + hibaelhárítás** > **lekérdezési terheléselemzőhöz**.</span><span class="sxs-lookup"><span data-stu-id="47740-131">Browse to a SQL database and click **All settings** > **Support + Troubleshooting** > **Query performance insight**.</span></span> 
   
    ![Lekérdezési terheléselemző][1]
   
    <span data-ttu-id="47740-133">A leggyakoribb lekérdezések nézet megnyílik, és a leggyakoribb CPU fogyasztó lekérdezések vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="47740-133">The top queries view opens and the top CPU consuming queries are listed.</span></span>
2. <span data-ttu-id="47740-134">Kattintson a részletek a diagram körül.</span><span class="sxs-lookup"><span data-stu-id="47740-134">Click around the chart for details.</span></span><br><span data-ttu-id="47740-135">Az első sor az adatbázis teljes DTU % jeleníti meg, amíg a sáv megjelenítése a kijelölt időszak során a kijelölt lekérdezés által használt CPU % (például ha **elmúlt hét** kijelölt minden sáv jelöli egy nap).</span><span class="sxs-lookup"><span data-stu-id="47740-135">The top line shows overall DTU% for the database, while the bars show CPU% consumed by the selected queries during the selected interval (for example, if **Past week** is selected each bar represents one day).</span></span>
   
    ![Leggyakoribb lekérdezések][2]
   
    <span data-ttu-id="47740-137">Az alsó rács látható lekérdezések összesített adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="47740-137">The bottom grid represents aggregated information for the visible queries.</span></span>
   
   * <span data-ttu-id="47740-138">Lekérdezésazonosítóval - lekérdezés adatbázis belül egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="47740-138">Query ID - unique identifier of query inside database.</span></span>
   * <span data-ttu-id="47740-139">CPU-t (aggregátumfüggvény függ) lekérdezés megfigyelhető időköze alatt kerülne sor.</span><span class="sxs-lookup"><span data-stu-id="47740-139">CPU per query during observable interval (depends on aggregation function).</span></span>
   * <span data-ttu-id="47740-140">Lekérdezésenként időtartama (aggregátumfüggvény függ).</span><span class="sxs-lookup"><span data-stu-id="47740-140">Duration per query (depends on aggregation function).</span></span>
   * <span data-ttu-id="47740-141">Egy adott lekérdezés végrehajtások teljes száma.</span><span class="sxs-lookup"><span data-stu-id="47740-141">Total number of executions for a particular query.</span></span>
     
     <span data-ttu-id="47740-142">Válassza ki, vagy törölje a belefoglalása / kizárása azokat a diagram jelölőnégyzetek segítségével egyéni lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="47740-142">Select or clear individual queries to include or exclude them from the chart using checkboxes.</span></span>
3. <span data-ttu-id="47740-143">Ha az adatok elavulttá válik, kattintson a **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="47740-143">If your data becomes stale, click the **Refresh** button.</span></span>
4. <span data-ttu-id="47740-144">Nagyítás gomb megfigyelési időköz módosításához, és vizsgálja meg a teljesítményt és használhatja a csúszkák: ![beállítások](./media/sql-database-query-performance/zoom.png)</span><span class="sxs-lookup"><span data-stu-id="47740-144">You can use sliders and zoom buttons to change observation interval and investigate spikes:  ![settings](./media/sql-database-query-performance/zoom.png)</span></span>
5. <span data-ttu-id="47740-145">Ha szükséges, ha azt szeretné, hogy egy másik nézetet, válassza **egyéni** lapra, és állítsa be:</span><span class="sxs-lookup"><span data-stu-id="47740-145">Optionally, if you want a different view, you can select **Custom** tab and set:</span></span>
   
   * <span data-ttu-id="47740-146">A metrika (CPU, időtartama, végrehajtási száma)</span><span class="sxs-lookup"><span data-stu-id="47740-146">Metric (CPU, duration, execution count)</span></span>
   * <span data-ttu-id="47740-147">Időtartam (utolsó 24 óra, elmúlt hét elmúlt hónap).</span><span class="sxs-lookup"><span data-stu-id="47740-147">Time interval (Last 24 hours, Past week, Past month).</span></span> 
   * <span data-ttu-id="47740-148">Lekérdezések száma.</span><span class="sxs-lookup"><span data-stu-id="47740-148">Number of queries.</span></span>
   * <span data-ttu-id="47740-149">Aggregátumfüggvény.</span><span class="sxs-lookup"><span data-stu-id="47740-149">Aggregation function.</span></span>
     
     ![beállítások](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a><span data-ttu-id="47740-151">Egyes lekérdezések részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="47740-151">Viewing individual query details</span></span>
<span data-ttu-id="47740-152">Lekérdezés a részletek megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="47740-152">To view query details:</span></span>

1. <span data-ttu-id="47740-153">Kattintson a lista leggyakoribb lekérdezések egyetlen lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="47740-153">Click any query in the list of top queries.</span></span>
   
    ![Részletek](./media/sql-database-query-performance/details.png)
2. <span data-ttu-id="47740-155">A részleteket megjelenítő nézetet megnyílik, és a lekérdezések fogyasztás/időtartama/végrehajtás processzorszám időbeli bontásban.</span><span class="sxs-lookup"><span data-stu-id="47740-155">The details view opens and the queries CPU consumption/Duration/Execution count is broken down over time.</span></span>
3. <span data-ttu-id="47740-156">Kattintson a részletek a diagram körül.</span><span class="sxs-lookup"><span data-stu-id="47740-156">Click around the chart for details.</span></span>
   
   * <span data-ttu-id="47740-157">Sor általános adatbázis DTU % a felső diagram ábrázolja, és a görgetősávokat a kijelölt lekérdezés által használt CPU %.</span><span class="sxs-lookup"><span data-stu-id="47740-157">Top chart shows line with overall database DTU%, and the bars are CPU% consumed by the selected query.</span></span>
   * <span data-ttu-id="47740-158">Második diagram teljes időtartam látható a kijelölt lekérdezés által.</span><span class="sxs-lookup"><span data-stu-id="47740-158">Second chart shows total duration by the selected query.</span></span>
   * <span data-ttu-id="47740-159">Alsó diagram végrehajtások teljes száma a kijelölt lekérdezés jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="47740-159">Bottom chart shows total number of executions by the selected query.</span></span>
     
     ![Lekérdezés részletei][3]
4. <span data-ttu-id="47740-161">Másik lehetőségként csúszkákkal, Nagyítás gomb, vagy kattintson a **beállítások** testreszabásához lekérdezési adatok megjelenítésére, vagy válasszon másik időtartamot.</span><span class="sxs-lookup"><span data-stu-id="47740-161">Optionally, use sliders, zoom buttons or click **Settings** to customize how query data is displayed, or to pick a different time period.</span></span>

## <a name="review-top-queries-per-duration"></a><span data-ttu-id="47740-162">Tekintse át a felső lekérdezések száma időtartama</span><span class="sxs-lookup"><span data-stu-id="47740-162">Review top queries per duration</span></span>
<span data-ttu-id="47740-163">A lekérdezési Terheléselemző legutóbbi frissítését, hogy vezette be két új mérőszámok, amelyik segíthet a potenciális szűk keresztmetszetek azonosítása: duration és végrehajtási számát.</span><span class="sxs-lookup"><span data-stu-id="47740-163">In the recent update of Query Performance Insight, we introduced two new metrics that can help you identify potential bottlenecks: duration and execution count.</span></span><br>

<span data-ttu-id="47740-164">Hosszan futó lekérdezések lennie a legnagyobb hosszabb erőforrások zárolása, más felhasználók számára és méretezhetőség korlátozza.</span><span class="sxs-lookup"><span data-stu-id="47740-164">Long-running queries have the greatest potential for locking resources longer, blocking other users, and limiting scalability.</span></span> <span data-ttu-id="47740-165">Azok a-zel is a legjobb optimalizálás.</span><span class="sxs-lookup"><span data-stu-id="47740-165">They are also the best candidates for optimization.</span></span><br>

<span data-ttu-id="47740-166">Hosszú ideig futó lekérdezések megadása:</span><span class="sxs-lookup"><span data-stu-id="47740-166">To identify long running queries:</span></span>

1. <span data-ttu-id="47740-167">Nyissa meg **egyéni** lapon lekérdezési Terheléselemző a kiválasztott adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="47740-167">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="47740-168">Módosítsa a mérni kívánt kell **időtartama**</span><span class="sxs-lookup"><span data-stu-id="47740-168">Change metrics to be **duration**</span></span>
3. <span data-ttu-id="47740-169">Válassza ki a lekérdezések és megfigyelési időközben</span><span class="sxs-lookup"><span data-stu-id="47740-169">Select number of queries and observation interval</span></span>
4. <span data-ttu-id="47740-170">Összesítő függvény kiválasztása</span><span class="sxs-lookup"><span data-stu-id="47740-170">Select aggregation function</span></span>
   
   * <span data-ttu-id="47740-171">**Sum** hozzáadja az összes lekérdezés végrehajtási idő teljes megfigyelési időközben során.</span><span class="sxs-lookup"><span data-stu-id="47740-171">**Sum** adds up all query execution time during whole observation interval.</span></span>
   * <span data-ttu-id="47740-172">**Maximális** megkeresi a lekérdezések teljes megfigyelési időközben maximális korábban mely végrehajtási idő.</span><span class="sxs-lookup"><span data-stu-id="47740-172">**Max** finds queries which execution time was maximum at whole observation interval.</span></span>
   * <span data-ttu-id="47740-173">**Átlagos** talál átlagos végrehajtási idő az összes lekérdezés végrehajtások, és ezek átlagok kívül felső is láthat.</span><span class="sxs-lookup"><span data-stu-id="47740-173">**Avg** finds average execution time of all query executions and show you the top out of these averages.</span></span> 
     
     ![lekérdezés időtartama][4]

## <a name="review-top-queries-per-execution-count"></a><span data-ttu-id="47740-175">Tekintse át a felső lekérdezések száma végrehajtási száma</span><span class="sxs-lookup"><span data-stu-id="47740-175">Review top queries per execution count</span></span>
<span data-ttu-id="47740-176">Végrehajtások nagy száma lehet, hogy nem kell érintő maga adatbázis és erőforrás-használat alacsony lehet, de alkalmazás általános juthat, lassú.</span><span class="sxs-lookup"><span data-stu-id="47740-176">High number of executions might not be affecting database itself and resources usage can be low, but overall application might get slow.</span></span>

<span data-ttu-id="47740-177">Bizonyos esetekben nagyon magas végrehajtási száma is előfordulhat, hogy növelje a hálózati kiszolgálókkal való adatváltások számát.</span><span class="sxs-lookup"><span data-stu-id="47740-177">In some cases, very high execution count may lead to increase of network round trips.</span></span> <span data-ttu-id="47740-178">Adatváltások jelentős mértékben befolyásolhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="47740-178">Round trips significantly affect performance.</span></span> <span data-ttu-id="47740-179">Azok a hálózati késés és alsóbb rétegbeli kiszolgáló késleltetésű.</span><span class="sxs-lookup"><span data-stu-id="47740-179">They are subject to network latency and to downstream server latency.</span></span> 

<span data-ttu-id="47740-180">Például számos adatvezérelt webhely fokozottan érik el az adatbázist minden felhasználói kérelem esetén.</span><span class="sxs-lookup"><span data-stu-id="47740-180">For example, many data-driven Web sites heavily access the database for every user request.</span></span> <span data-ttu-id="47740-181">Amíg a kapcsolat készletezését segítségével, a megnövekedett hálózati forgalmat, és az adatbázis-kiszolgáló terhelése feldolgozása hátrányosan befolyásolhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="47740-181">While connection pooling helps, the increased network traffic and processing load on the database server can adversely affect performance.</span></span>  <span data-ttu-id="47740-182">Általános útmutatásként adatváltások tartsa a lehető legkisebb értéke.</span><span class="sxs-lookup"><span data-stu-id="47740-182">General advice is to keep round trips to an absolute minimum.</span></span>

<span data-ttu-id="47740-183">Gyakran azonosításához végrehajtott lekérdezések ("chatty") lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="47740-183">To identify frequently executed queries (“chatty”) queries:</span></span>

1. <span data-ttu-id="47740-184">Nyissa meg **egyéni** lapon lekérdezési Terheléselemző a kiválasztott adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="47740-184">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="47740-185">Módosítsa a mérni kívánt kell **végrehajtási száma**</span><span class="sxs-lookup"><span data-stu-id="47740-185">Change metrics to be **execution count**</span></span>
3. <span data-ttu-id="47740-186">Válassza ki a lekérdezések és megfigyelési időközben</span><span class="sxs-lookup"><span data-stu-id="47740-186">Select number of queries and observation interval</span></span>
   
    ![lekérdezés-végrehajtási száma][5]

## <a name="understanding-performance-tuning-annotations"></a><span data-ttu-id="47740-188">Teljesítmény hangolási jegyzetek ismertetése</span><span class="sxs-lookup"><span data-stu-id="47740-188">Understanding performance tuning annotations</span></span>
<span data-ttu-id="47740-189">Tervezi a terhelést a lekérdezési teljesítmény elemzését, miközben bizonyára észrevette, hogy fölött a diagram függőleges vonallal ikonok.</span><span class="sxs-lookup"><span data-stu-id="47740-189">While exploring your workload in Query Performance Insight, you might notice icons with vertical line on top of the chart.</span></span><br>

<span data-ttu-id="47740-190">Ezekkel az ikonokkal jegyzetek; érintő által végrehajtott műveletek teljesítményének képviselnek [SQL Azure Database Advisor](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="47740-190">These icons are annotations; they represent performance affecting actions performed by [SQL Azure Database Advisor](sql-database-advisor.md).</span></span> <span data-ttu-id="47740-191">Rámutató jegyzet által alapvető tudnivalók az beszerzése:</span><span class="sxs-lookup"><span data-stu-id="47740-191">By hovering annotation, you get basic information about the action:</span></span>

![lekérdezés Megjegyzés][6]

<span data-ttu-id="47740-193">Ha további vagy advisor javaslat alkalmazni kívánja, kattintson az ikonra.</span><span class="sxs-lookup"><span data-stu-id="47740-193">If you want to know more or apply advisor recommendation, click the icon.</span></span> <span data-ttu-id="47740-194">Az a művelet részleteit nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="47740-194">It will open details of action.</span></span> <span data-ttu-id="47740-195">Ha egy aktív adott alkalmazhatja azonnal paranccsal.</span><span class="sxs-lookup"><span data-stu-id="47740-195">If it’s an active recommendation you can apply it straight away using command.</span></span>

![lekérdezés jegyzet részletei][7]

### <a name="multiple-annotations"></a><span data-ttu-id="47740-197">Több megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="47740-197">Multiple annotations.</span></span>
<span data-ttu-id="47740-198">Azonban lehetséges, hogy miatt nagyítási szintjét, egymás közelében lévő fogja lekérni összecsukott valamelyikébe.</span><span class="sxs-lookup"><span data-stu-id="47740-198">It’s possible, that because of zoom level, annotations that are close to each other will get collapsed into one.</span></span> <span data-ttu-id="47740-199">Ez különleges ikon fog megjelenni, kattintson rá fog megnyitása új panel, ahol csoportosított listája a jegyzetek megjelenik.</span><span class="sxs-lookup"><span data-stu-id="47740-199">This will be represented by special icon, clicking it will open new blade where list of grouped annotations will be shown.</span></span>
<span data-ttu-id="47740-200">Adatok, lekérdezések és teljesítményének hangolása műveletek jobb megértése érdekében az alkalmazások és szolgáltatások segítségével.</span><span class="sxs-lookup"><span data-stu-id="47740-200">Correlating queries and performance tuning actions can help to better understand your workload.</span></span> 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a><span data-ttu-id="47740-201">A Lekérdezéstár konfigurációs betekintés a lekérdezési teljesítmény optimalizálása</span><span class="sxs-lookup"><span data-stu-id="47740-201">Optimizing the Query Store configuration for Query Performance Insight</span></span>
<span data-ttu-id="47740-202">A lekérdezési Terheléselemző felhasználása során merülhetnek fel az alábbi Lekérdezéstár üzenetek:</span><span class="sxs-lookup"><span data-stu-id="47740-202">During your use of Query Performance Insight, you might encounter the following Query Store messages:</span></span>

* <span data-ttu-id="47740-203">"A Lekérdezéstár nincs megfelelően konfigurálva ezen az adatbázison.</span><span class="sxs-lookup"><span data-stu-id="47740-203">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="47740-204">Kattintson ide további."</span><span class="sxs-lookup"><span data-stu-id="47740-204">Click here to learn more."</span></span>
* <span data-ttu-id="47740-205">"A Lekérdezéstár nincs megfelelően konfigurálva ezen az adatbázison.</span><span class="sxs-lookup"><span data-stu-id="47740-205">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="47740-206">Kattintson ide a beállítások módosításához."</span><span class="sxs-lookup"><span data-stu-id="47740-206">Click here to change settings."</span></span> 

<span data-ttu-id="47740-207">Ezek az üzenetek általában jelennek meg, amikor a Lekérdezéstár nem képes új adatok gyűjtéséért felelős ügyfélfeladatot.</span><span class="sxs-lookup"><span data-stu-id="47740-207">These messages usually appear when Query Store is not able to collect new data.</span></span> 

<span data-ttu-id="47740-208">Első esetben történik, ha a Lekérdezéstár csak olvasható állapotban van, és paraméterei optimálisan vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="47740-208">First case happens when Query Store is in Read-Only state and parameters are set optimally.</span></span> <span data-ttu-id="47740-209">A hibát a Lekérdezéstár méretének növelését, vagy a jelölés törlésével a Lekérdezéstár.</span><span class="sxs-lookup"><span data-stu-id="47740-209">You can fix this by increasing size of Query Store or clearing Query Store.</span></span>

![qds gomb][8]

<span data-ttu-id="47740-211">Második esetben történik, ha a Lekérdezéstár le van tiltva, vagy a paraméterek nincsenek beállítva optimális.</span><span class="sxs-lookup"><span data-stu-id="47740-211">Second case happens when Query Store is Off or parameters aren’t set optimally.</span></span> <br><span data-ttu-id="47740-212">Módosítsa a rögzítése és megőrzési házirendet, és engedélyezze a Lekérdezéstárat, a következő az alábbi parancsok futtatásával, vagy közvetlenül a portálon:</span><span class="sxs-lookup"><span data-stu-id="47740-212">You can change the Retention and Capture policy and enable Query Store by executing commands below or directly from portal:</span></span>

![qds gomb][9]

### <a name="recommended-retention-and-capture-policy"></a><span data-ttu-id="47740-214">Ajánlott rögzítése és az adatmegőrzési házirend</span><span class="sxs-lookup"><span data-stu-id="47740-214">Recommended retention and capture policy</span></span>
<span data-ttu-id="47740-215">Az adatmegőrzési két típusa van:</span><span class="sxs-lookup"><span data-stu-id="47740-215">There are two types of retention policies:</span></span>

* <span data-ttu-id="47740-216">Méret - alapú Ha automatikus értékre van beállítva, megtisztítja az adatok automatikusan elérésekor közelében maximális méretét.</span><span class="sxs-lookup"><span data-stu-id="47740-216">Size based - if set to AUTO it will clean data automatically when near max size is reached.</span></span>
* <span data-ttu-id="47740-217">Idő alapján - alapértelmezett helyezünk 30 nap során, ami azt jelenti, a Lekérdezéstár nincs elég lemezterület fog futni, ha törli az információ lekérdezése 30 napnál régebbi</span><span class="sxs-lookup"><span data-stu-id="47740-217">Time based - by default we will set it to 30 days, which means, if Query Store will run out of space, it will delete query information older than 30 days</span></span>

<span data-ttu-id="47740-218">Rögzítése házirend beállítható:</span><span class="sxs-lookup"><span data-stu-id="47740-218">Capture policy could be set to:</span></span>

* <span data-ttu-id="47740-219">**Minden** – összes lekérdezés rögzíti.</span><span class="sxs-lookup"><span data-stu-id="47740-219">**All** - Captures all queries.</span></span>
* <span data-ttu-id="47740-220">**Automatikus** -alkalomszerű lekérdezések és lekérdezések jelentéktelen fordítási és végrehajtási időtartamú figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="47740-220">**Auto** - Infrequent queries and queries with insignificant compile and execution duration are ignored.</span></span> <span data-ttu-id="47740-221">Végrehajtási szám, a fordítás és a futási ideje küszöbértékek belső határozza meg.</span><span class="sxs-lookup"><span data-stu-id="47740-221">Thresholds for execution count, compile and runtime duration are internally determined.</span></span> <span data-ttu-id="47740-222">Ez a beállítás az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="47740-222">This is the default option.</span></span>
* <span data-ttu-id="47740-223">**Nincs** -Lekérdezéstár leállítja az új lekérdezések rögzítését, azonban már rögzített lekérdezések futásidejű statisztikák még gyűjtik.</span><span class="sxs-lookup"><span data-stu-id="47740-223">**None** - Query Store stops capturing new queries, however runtime stats for already captured queries are still collected.</span></span>

<span data-ttu-id="47740-224">Azt javasoljuk, hogy minden szabályzatok beállítása automatikus és 30 nap tiszta házirend:</span><span class="sxs-lookup"><span data-stu-id="47740-224">We recommend setting all policies to AUTO and clean policy to 30 days:</span></span>

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

<span data-ttu-id="47740-225">A Lekérdezéstár méretének növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="47740-225">Increase size of Query Store.</span></span> <span data-ttu-id="47740-226">Ez végre tudja hajtani-adatbázishoz szeretne csatlakozni, és a következő lekérdezés kiállító:</span><span class="sxs-lookup"><span data-stu-id="47740-226">This could be performed by connecting to a database and issuing following query:</span></span>

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

<span data-ttu-id="47740-227">Ezek a beállítások alkalmazásának végül ellenőrizze a Lekérdezéstár új lekérdezések gyűjtése, azonban ha nem akarja megvárni a Lekérdezéstár törlése.</span><span class="sxs-lookup"><span data-stu-id="47740-227">Applying these settings will eventually make Query Store collecting new queries, however if you don’t want to wait you can clear Query Store.</span></span> 

> [!NOTE]
> <span data-ttu-id="47740-228">A következő lekérdezés végrehajtásakor a lekérdezéstárban az összes aktuális adatokat törli.</span><span class="sxs-lookup"><span data-stu-id="47740-228">Executing following query will delete all current information in the Query Store.</span></span> 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a><span data-ttu-id="47740-229">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="47740-229">Summary</span></span>
<span data-ttu-id="47740-230">Lekérdezési Terheléselemző segítségével megismerheti, hogy a lekérdezés-munkaterhelési a hatását, és hogyan vonatkozik adatbázis hálózatierőforrás-fogyasztás.</span><span class="sxs-lookup"><span data-stu-id="47740-230">Query Performance Insight helps you understand the impact of your query workload and how it relates to database resource consumption.</span></span> <span data-ttu-id="47740-231">Ezzel a szolgáltatással akkor fog információ a leginkább erőforrásigényes lekérdezések, és könnyen azonosíthatja a meglévők közül, így elkerülhetők a probléma megoldásához.</span><span class="sxs-lookup"><span data-stu-id="47740-231">With this feature, you will learn about the top consuming queries, and easily identify the ones to fix before they become a problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="47740-232">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47740-232">Next steps</span></span>
<span data-ttu-id="47740-233">Az SQL-adatbázis teljesítményének javítása kapcsolatos további javaslatok kattintson [javaslatok](sql-database-advisor.md) a a **lekérdezési Terheléselemző** panelen.</span><span class="sxs-lookup"><span data-stu-id="47740-233">For additional recommendations about improving the performance of your SQL database, click [Recommendations](sql-database-advisor.md) on the **Query Performance Insight** blade.</span></span>

![Teljesítmény Advisor](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

